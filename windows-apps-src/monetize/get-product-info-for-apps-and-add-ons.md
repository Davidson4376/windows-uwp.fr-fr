---
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations du Windows Store concernant l’application active ou l’un de ses modules complémentaires.
title: Obtenir les informations produit des applications et des extensions
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp, achats dans l’application, extensions, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 9b923764c6374e403d2652db715f65a80c48bacf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623094"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>Obtenir les informations produit des applications et des extensions

Cet article explique comment utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour obtenir des informations liées au Store sur l’application actuelle ou l'une de ses extensions.

Pour obtenir un exemple d’application complète, consultez [Exemple Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

> [!NOTE]
> L'espace de noms **Windows.Services.Store** a été introduit dans Windows 10, version 1607 et peut être utilisé uniquement dans les projets qui ciblent **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure dans Visual Studio. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [cet article](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

Les conditions préalables de ces exemples sont les suivantes :
* Un projet Visual Studio pour une application de plateforme Windows universelle (UWP) qui cible **Windows 10 Anniversary Edition (version 10.0 ; build 14393)** ou une version ultérieure.
* Vous avez [créé une soumission de l’application](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) dans Centre partenaires et de cette application est publiée dans le Store. Vous pouvez éventuellement configurer l’application pour qu'elle ne soit pas détectable dans le Store pendant que vous la testez. Pour plus d’informations, consultez nos [conseils de test](in-app-purchases-and-trials.md#testing).
* Si vous souhaitez obtenir les informations de produit pour un module complémentaire pour l’application, vous devez également [créer le module complémentaire dans partenaires](../publish/add-on-submissions.md).

Le code de ces exemples respecte les présupposés suivants :
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Si vous disposez d’une application de bureau qui utilise [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans ces exemples pour configurer l’objet  [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obtenir les informations de l’application en cours

Pour obtenir des informations du Windows Store concernant l’application actuelle, utilisez la méthode [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync). Cette méthode asynchrone renvoie un objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que vous pouvez utiliser pour obtenir des informations telles que le prix.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>Obtenir les informations des modules complémentaires sur les produits ayant des ID Store connus associés à l'app actuelle

Pour obtenir les informations du Microsoft Store sur les extensions associées à l'app actuelle et dont vous connaissez déjà les [ID Store](in-app-purchases-and-trials.md#store_ids), utilisez la méthode [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent chaque module complémentaire. Outre les ID Windows Store, vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind).

> [!NOTE]
> La méthode **GetStoreProductsAsync** renvoie des informations de produit pour les extensions spécifiées qui sont associées à l’app, que ces extensions soient disponibles ou non à l'achat. Pour récupérer les informations de toutes les extensions de l’app actuelle qui peut actuellement être achetée, utilisez à la place la méthode **GetAssociatedStoreProductsAsync** comme décrit dans la [section suivante](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app).

L’exemple suivant récupère les informations concernant les extensions durables dont les ID Store spécifiés sont associés à l'app actuelle.

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

## <a name="related-topics"></a>Rubriques connexes

* [Achats dans l’application et essais](in-app-purchases-and-trials.md)
* [Obtenir les informations de licence pour les applications et modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats dans l’application des applications et des modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple de Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
