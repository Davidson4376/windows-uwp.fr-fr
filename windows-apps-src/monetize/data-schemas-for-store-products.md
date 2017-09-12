---
author: mcleanbyron
description: "Décrit le schéma de données JSON étendu des produits du Windows Store dans l’espace de noms Windows.Services.Store."
title: "Schémas de données des produits du Windows Store"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 
ms.openlocfilehash: 2a294dc490a0b7bed5e426ff26dfed948f337dfa
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/21/2017
---
# <a name="data-schemas-for-store-products"></a>Schémas de données des produits du Windows Store

Lorsque vous soumettez un produit (par exemple, une application ou une extension) dans le Windows Store, celui-ci gère un ensemble complet de données pour le produit et ses licences. Dans le code de votre application, vous pouvez accéder par programme à certaines de ces données à l’aide des propriétés de l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Par exemple, vous pouvez récupérer la description et le prix de l’application actuelle ou de son extension à l’aide des propriétés [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Description) et [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Price).

Toutefois, la plupart des données des produits du Windows Store ne sont pas exposées par des propriétés prédéfinies dans l'espace de noms [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Pour accéder à toutes les données d’un produit dans votre code, vous pouvez utiliser à la place les propriétés générales suivantes:

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_ExtendedJsonData_)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAvailability_ExtendedJsonData_)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_ExtendedJsonData_)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense#Windows_Services_Store_StoreLicense_ExtendedJsonData_)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties#Windows_Services_Store_StorePurchaseProperties_ExtendedJsonData_)

Ces propriétés renvoient les données complètes de l’objet correspondant sous forme de chaîne au format JSON. Cet article fournit le schéma complet des données renvoyées par ces propriétés.

> [!NOTE]
> Les produits du Windows Store sont organisés selon une hiérarchie d'objets [produit](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [référence (SKU)](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) et [disponibilité](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability). Pour plus d’informations, voir [Produits, références et disponibilités](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Schéma pour StoreProduct, StoreSku, StoreAvailability et StoreCollectionData

Le schéma suivant décrit la chaîne au format JSON renvoyée par [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_). Les propriétés [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_ExtendedJsonData_), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAvailability_ExtendedJsonData_) et [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_ExtendedJsonData_) renvoient uniquement les parties du schéma qui sont définies sous les chemins d’accès ```DisplaySkuAvailabilities\Sku```, ```DisplaySkuAvailabilities\Availabilities``` et ```DisplaySkuAvailabilities\Sku\CollectionData```, respectivement.

Pour obtenir un exemple de chaîne au format JSON renvoyée par [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_), voir [cette section](#product-example).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L603)]

<span id="product-example" />
### <a name="example"></a>Exemple

L’exemple suivant montre une chaîne au format JSON renvoyée par la propriété [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) d'application.

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Schéma pour StoreAppLicense et StoreLicense

Le schéma suivant décrit la chaîne au format JSON renvoyée par [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_). La propriété [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreLicense_ExtendedJsonData_) renvoie uniquement les parties du schéma qui sont définies sous le chemin d’accès ```productAddOns```.

Pour obtenir un exemple de chaîne au format JSON renvoyée par [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_), voir [cette section](#license-example).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />
### <a name="example"></a>Exemple

L’exemple suivant montre une chaîne au format JSON renvoyée par la propriété [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability#Windows_Services_Store_StoreAppLicense_ExtendedJsonData_) d'application.

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Schéma pour StorePurchaseProperties

Le schéma suivant décrit la chaîne au format JSON renvoyée par [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties#Windows_Services_Store_StorePurchaseProperties_ExtendedJsonData_).

[!code[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>Rubriques associées

* [Versions d’évaluation et achats in-app](in-app-purchases-and-trials.md)
* [Obtenir les informations produit des applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence des applications et des modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats de modules complémentaires consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
