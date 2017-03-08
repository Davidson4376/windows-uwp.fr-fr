---
author: mcleanbyron
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: "Découvrez comment utiliser l’espace de noms Windows.Services.Store pour obtenir les informations du Windows Store concernant l’application active ou l’une de ses extensions."
title: Obtenir les informations produit des applications et des extensions
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, achats dans l’application, extensions, Windows.Services.Store"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7e486c451174cd24429dc35cda07d22fe2b28745
ms.lasthandoff: 02/07/2017

---

# <a name="get-product-info-for-apps-and-add-ons"></a>Obtenir les informations produit des applications et des extensions

Les applications qui ciblent Windows 10 version 1607 ou une version ultérieure peuvent utiliser les méthodes de la classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) dans l’espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) pour obtenir les informations du Windows Store concernant l’application actuelle ou l’une de ses extensions. Les exemples suivants dans cet article montrent comment effectuer cette opération dans différents cas de figure. Pour un exemple complète, consultez la section [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Remarque**&nbsp;&nbsp;Cet article concerne les applications ciblant Windows 10 version 1607 ou ultérieure. Si votre application cible une version antérieure de Windows 10, vous devez utiliser l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) à la place de l’espace de noms **Windows.Services.Store**. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app utilisant l’espace de noms Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

## <a name="prerequisites"></a>Conditions préalables

Les conditions préalables de ces exemples sont les suivantes :
* Un projet Visual Studio d’application de plateforme Windows universelle (UWP) qui cible Windows 10 version 1607 ou ultérieure.
* Vous avez créé une application dans le tableau de bord du Centre de développement Windows, et celle-ci est publiée et disponible dans le Windows Store. Il peut s’agir d’une application que vous souhaitez vendre à des clients, ou d’une application de base qui présente la configuration minimale requise par le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) et que vous utilisez uniquement à des fins de test. Pour plus d’informations, consultez les [conseils de test](in-app-purchases-and-trials.md#testing).

Le code de ces exemples respecte les présupposés suivants :
* Le code s’exécute dans le contexte d’une [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) qui contient un [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) nommé ```workingProgressRing``` et un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) nommé ```textBlock```. Ces objets sont utilisés pour respectivement indiquer qu’une opération asynchrone est en cours et afficher les messages de sortie.
* Le fichier de code contient une instruction **using** pour l’espace de noms **Windows.Services.Store**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md#api_intro).

Pour obtenir un exemple d’application complète, voir l’[exemple du Windows Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

>**Remarque**&nbsp;&nbsp;Si vous disposez d’une application de bureau qui utilise [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop), vous devrez peut-être ajouter du code supplémentaire non affiché dans ces exemples pour configurer l’objet [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Pour plus d’informations, voir [Utilisation de la classe StoreContext dans une application de bureau qui utilise Desktop Bridge](in-app-purchases-and-trials.md#desktop).

## <a name="get-info-for-the-current-app"></a>Obtenir les informations de l’application en cours

Pour obtenir des informations du Windows Store concernant l’application actuelle, utilisez la méthode [GetStoreProductForCurrentAppAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getstoreproductforcurrentappasync.aspx). Cette méthode asynchrone renvoie un objet [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que vous pouvez utiliser pour obtenir des informations telles que le prix.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-products-with-known-store-ids"></a>Obtenir des informations sur les produits ayant des ID Windows Store connus

Pour obtenir les informations du Windows Store sur des applications ou des modules complémentaires dont vous connaissez déjà l’[ID Windows Store](in-app-purchases-and-trials.md#store_ids), utilisez la méthode [GetStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706579.aspx). Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent chaque application ou extension. Outre les ID Windows Store, vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

L’exemple suivant récupère les informations concernant les modules complémentaires durables qui ont les ID Windows Store spécifiés.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-the-current-app"></a>Obtenir les informations des modules complémentaires qui sont disponibles pour l’application en cours

Pour obtenir les informations du Windows Store correspondant aux modules complémentaires disponibles pour l’application en cours, utilisez la méthode [GetAssociatedStoreProductsAsync](https://msdn.microsoft.com/library/windows/apps/mt706571.aspx). Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent chaque extension disponible. Vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires que vous souhaitez récupérer. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

>**Remarque**&nbsp;&nbsp;Si l’application compte de nombreux modules complémentaires, vous pouvez également employer la méthode [GetAssociatedStoreProductsWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706572.aspx) pour paginer les résultats.

L’exemple suivant récupère les informations de tous les modules complémentaires durables, des modules complémentaires consommables gérés par le Windows Store et des modules complémentaires consommables gérés par un développeur.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-current-user-is-entitled-to-use"></a>Obtenir des informations sur les modules complémentaires de l’application active, que l’utilisateur actuel est autorisé à utiliser

Pour obtenir les informations du Windows Store correspondant aux modules complémentaires que l’utilisateur actuel est autorisé à utiliser, utilisez la méthode [GetUserCollectionAsync](https://msdn.microsoft.com/library/windows/apps/mt706580.aspx). Cette méthode asynchrone renvoie un ensemble d’objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent chaque extension. Vous devez transmettre à cette méthode une liste de chaînes qui identifient les types de modules complémentaires que vous souhaitez récupérer. Pour obtenir la liste des valeurs de chaîne prises en charge, consultez la propriété [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx).

>**Remarque**&nbsp;&nbsp;Si l’application compte de nombreux modules complémentaires, vous pouvez également employer la méthode [GetUserCollectionWithPagingAsync](https://msdn.microsoft.com/library/windows/apps/mt706581.aspx) pour paginer les résultats.

L’exemple suivant récupère les informations concernant les modules complémentaires durables qui ont les ID Windows Store spécifiés.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>Rubriques connexes

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
* [Exemple Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)

