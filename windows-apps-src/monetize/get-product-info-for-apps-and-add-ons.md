---
author: Xansky
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations du WindowsStore concernant l’application active ou l’un de ses modules complémentaires.
title: Obtenir les informations produit des applications et des extensions
ms.author: mhopkins
ms.date: 02/08/2018
ms.topic: article
keywords: windows10, uwp, achats dans l’application, extensions, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: f1544ee3404e77ec7565c626a6ca96e439832c90
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6181330"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Obtenir les informations produit des applications et des extensions

Cet article explique comment utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour obtenir des informations liées au Store sur l’application actuelle ou l'une de ses extensions.

Pour obtenir un exemple d’application complète, consultez [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows10, version1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

Les conditions préalables de ces exemples sont les suivantes:
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows10 Anniversary Edition (version10.0; build14393)** ou une version ultérieure.
* Vous avez [créé une soumission d’application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans l’espace partenaires et que cette application est publiée dans le Windows Store. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Windows Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez obtenir les informations de produit pour une extension de l’application, vous devez également [créer l’extension dans l’espace partenaires](../publish/add-on-submissions.md).

Le code de ces exemples respecte les présupposés suivants:
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats dans l'application](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans ces exemples pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obtenir les informations de l’application en cours

Pour obtenir des informations du WindowsStore concernant l’application actuelle, utilisez la méthode [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync). Cette méthode asynchrone renvoie un objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que vous pouvez utiliser pour obtenir des informations telles que le prix.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Obtenir les informations des modules complémentaires sur les produits ayant des ID Store connus associés à l'app actuelle

Pour obtenir les informations du MicrosoftStore sur les extensions associées à l'app actuelle et dont vous connaissez déjà les [IDStore](in-app-purchases-and-trials.md#store_ids), utilisez la méthode [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent chaque module complémentaire. Outre les ID Store, vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> La méthode **GetStoreProductsAsync** renvoie des informations de produit pour les extensions spécifiées qui sont associées à l’app, que ces extensions soient disponibles ou non à l'achat. Pour récupérer les informations de toutes les extensions de l’app actuelle qui peut actuellement être achetée, utilisez à la place la méthode **GetAssociatedStoreProductsAsync** comme décrit dans la [section suivante](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app).

L’exemple suivant récupère les informations concernant les extensions durables dont les IDStore spécifiés sont associés à l'app actuelle.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>Obtenir les informations des extensions qui sont disponibles à l'achat pour l’app en cours

Pour obtenir les informations du Store correspondant aux extensions actuellement disponibles à l'achat pour l’app en cours, utilisez la méthode [GetAssociatedStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync). Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent chaque module complémentaire disponible. Vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires que vous souhaitez récupérer. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> Si l’app compte de nombreuses extensions disponibles à l'achat, vous pouvez également employer la méthode  [GetAssociatedStoreProductsWithPagingAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) pour paginer les résultats.

L’exemple suivant récupère les informations de tous les modules complémentaires durables, des modules complémentaires consommables gérés par le Store et des modules complémentaires consommables gérés par un développeur qui sont disponibles à l'achat pour l'app en cours.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>Obtenir les informations sur les extensions associées à l'app en cours que l'utilisateur a achetées

Pour obtenir les informations du Store correspondant aux extensions que l’utilisateur a achetées, utilisez la méthode [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionasync). Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent chaque extension. Vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires que vous souhaitez récupérer. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

> [!NOTE]
> Si l’application compte de nombreux modules complémentaires, vous pouvez également employer la méthode [GetUserCollectionWithPagingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) pour paginer les résultats.

L’exemple suivant récupère les informations concernant les modules complémentaires durables qui ont les [ID Store](in-app-purchases-and-trials.md#store_ids) spécifiés.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Rubriquesassociées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats d’extensions consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
