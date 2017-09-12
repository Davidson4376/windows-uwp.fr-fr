---
author: mcleanbyron
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: "Utilisez l’API des offres ciblées du WindowsStore pour récupérer les offres ciblées disponibles pour une application."
title: "Gérer les offres ciblées à l’aide des services du WindowsStore"
ms.author: mcleans
ms.date: 05/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, services de WindowsStore, API des offres ciblées du WindowsStore, offres ciblées"
ms.openlocfilehash: 684c37d4439f415ad607b7f3e6a166966cc9f835
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2017
---
# <a name="manage-targeted-offers-using-store-services"></a>Gérer les offres ciblées à l’aide des services du WindowsStore

Si vous créez une *offre ciblée* dans la page **Engager > Offre ciblée** de votre application dans le tableau de bord du Centre de développement Windows, utilisez l’*API des offres ciblées du WindowsStore* dans le code de votre application pour implanter l’expérience relative à l’offre ciblée au sein de l’application. Pour plus d’informations concernant les offres ciblées et leur création dans le tableau de bord, consultez [Utiliser les offres ciblées pour optimiser l’engagement et les conversions](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

L’API des offres ciblées est une API REST permettant d’effectuer ces tâches:

* Obtenir les offres ciblées disponibles pour l’utilisateur actuel, selon que l’utilisateur fait ou non partie du segment de clientèle ciblé dans le cadre de l’offre.
* Si l’utilisateur achète l’offre ciblée, vous pouvez soumettre une réclamation dans le WindowsStore afin de pouvoir bénéficier de la prime associée à l’offre ciblée. Ceci est uniquement nécessaire si l’offre ciblée est associée à une promotion parrainée par Microsoft rapportant un bonus aux développeurs pour chaque conversion réussie.

Pour utiliser cette API dans le code de votre application, procédez ainsi:

1.  [Obtenez un jeton de compte Microsoft](#obtain-a-microsoft-account-token) pour l’utilisateur de votre application actuellement connecté.
2.  [Obtenez les offres ciblées pour l’utilisateur actuellement connecté](#get-targeted-offers).
3.  [Achetez l'extension associée à une offre ciblée](#purchase-add-on).
3.  [Réclamez l'offre ciblée](#claim-targeted-offer) (si elle est associée à une promotion parrainée par Microsoft rapportant un bonus aux développeurs pour chaque conversion réussie).

> [!NOTE]
> Actuellement, la possibilité d’associer une offre ciblée et une promotion parrainée par Microsoft puis de réclamer l’offre n’est pas disponible pour tous les comptes de développeur.

Pour obtenir un exemple de code complet qui montre toutes ces étapes, voir l'[exemple de code](#code-example) à la fin de cet article. Les sections suivantes fournissent plus d’informations sur chaque étape.

<span id="obtain-a-microsoft-account-token" />
## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtenez un jeton de compte Microsoft pour l’utilisateur actuellement connecté

Dans le code de votre application, obtenez un jeton de compte Microsoft (MSA) pour l’utilisateur actuellement connecté. Vous devez transmettre ce jeton dans l’en-tête de requête ```Authorization``` de chacune des méthodes de l’API des offres ciblées du WindowsStore. Ce jeton est utilisé par le WindowsStore pour récupérer les offres ciblées disponibles pour l’utilisateur actuellement connecté.

Pour obtenir le jeton du MAS, utilisez la classe [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) pour demander un jeton selon la règle ```devcenter_implicit.basic,wl.basic```. L’exemple suivant montre comment procéder. Cet exemple est un extrait de l'[exemple complet](#code-example) et nécessite des instructions **using** fournies dans l’exemple complet.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Pour plus d’informations sur l’obtention des jetons MSA, consultez la section [Gestionnaire de comptes web](../security/web-account-manager.md).

<span id="get-targeted-offers" />
## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtenez les offres ciblées pour l’utilisateur actuellement connecté

Dès que vous disposez d’un jeton de compte Microsoft pour l’utilisateur actuellement connecté, appelez la méthode GET de l’URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` pour obtenir les offres ciblées disponibles pour cet utilisateur. Pour plus d’informations sur cette méthode REST, voir [Obtenir des offres ciblées](get-targeted-offers.md).

Cette méthode renvoie les ID de produit des modules complémentaires, qui sont associés aux offres ciblées disponibles pour l’utilisateur actuel. Ces informations vous permettent de proposer à l’utilisateur une ou plusieurs des offres ciblées sous forme d’achat in-app. Cette méthode renvoie également un ID de suivi que vous pouvez utiliser pour [soumettre une réclamation](#claim-targeted-offer) au WindowsStore afin de recevoir les éventuels bonus associés à l’une des offres ciblées.

L’exemple suivant montre comment obtenir les offres ciblées pour l’utilisateur actuel. Cet exemple est un extrait de l'[exemple complet](#code-example). Il requiert la bibliothèque [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft, ainsi que des classes supplémentaires et des instructions **using**fournies dans l’exemple complet.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="purchase-add-on" />
## <a name="purchase-the-add-on-that-is-associated-with-a-targeted-offer"></a>Achetez l'extension associée à une offre ciblée

Ensuite, proposez à l’utilisateur d’acheter l’une des offres ciblées. Si l’utilisateur accepte d’acheter l’offre ciblée, utilisez l’une des méthodes suivantes pour programmer l’achat du module complémentaire associé à l’offre ciblée. Utilisez les valeurs d’ID de produit récupérées lors de l’étape précédente pour identifier le module complémentaire que vous souhaitez acheter.

* Si votre application cible une version1607 ou ultérieure de Windows10, nous vous recommandons d’utiliser l’une des méthodes **RequestPurchaseAsync** de l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store). Pour plus d’informations sur l’utilisation de ces méthodes, voir la section [Activer les achats in-app d’applications et de modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md).

* Si votre application cible une version antérieure de Windows10, utilisez la méthode [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) de l’espace de noms [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) pour acheter le module complémentaire. Pour plus d’informations sur l’utilisation de cette méthode, voir [Activer les achats de produits in-app](enable-in-app-product-purchases.md).

Pour obtenir des exemples de code qui montrent chaque option, voir l'[exemple de code](#code-example) à la fin de cet article.

> [!NOTE]
> Il existe deux espaces de noms différents pour l’implémentation des achats in-app dans une application UWP: [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) (introduit dans la version1607 de Windows10) et [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) (disponible dans toutes les versions de Windows10). Pour plus d’informations sur les différences entre ces espaces de noms, voir [Achats dans l’application et versions d’évaluation](in-app-purchases-and-trials.md).

<span id="claim-targeted-offer" />
## <a name="submit-a-claim-for-a-targeted-offer"></a>Soumettre une réclamation pour une offre ciblée

Si l’utilisateur achète une offre ciblée associée à une promotion parrainée par Microsoft et proposant un bonus aux développeurs pour chaque conversion réussie, vous devez soumettre une réclamation dans le WindowsStore afin de recevoir votre prime. Quand vous envoyez votre réclamation, vous devez fournir une valeur qui représente la confirmation d’achat. Vous obtenez cette confirmation d’achat différemment selon que votre application utilise des API dans l'espace de noms **Windows.ApplicationModel.Store** ou **Windows.Services.Store** pour acheter l'extension.

> [!NOTE]
> Actuellement, la possibilité d’associer une offre ciblée et une promotion parrainée par Microsoft puis de réclamer l’offre n’est pas disponible pour tous les comptes de développeur.

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Applications qui utilisent l’espace de noms Windows.ApplicationModel.Store

1. Après avoir acheté l'extension à l'aide de la méthode [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) dans l’espace de noms **Windows.ApplicationModel.Store**, n’oubliez pas de récupérer un [reçu pour votre achat](use-receipts-to-verify-product-purchases.md). Ce reçu est disponible dans l’objet [PurchaseResults.ReceiptXml](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults#Windows_ApplicationModel_Store_PurchaseResults_ReceiptXml_) retourné par la méthode **RequestProductPurchaseAsync**. Ce reçu représente la confirmation d’achat que vous allez utiliser dans l’étape suivante.

2. Envoyez un message POST à l’URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` pour soumettre une réclamation pour la conversion de l’offre ciblée. Pour plus d’informations sur cette méthode REST, voir [Réclamer une offre ciblée](claim-a-targeted-offer.md). Dans le corps de la requête, transmettez les données suivantes:

  * Champ *storeOffer*: transmettez l’un des objets JSON reçus dans le corps de la requête de la méthode [Obtenir des offres ciblées](get-targeted-offers.md) (cet objet doit inclure les champs *offers* et *trackingId*).

  * Champ *receipt*: transmettez la chaîne de réception que vous avez récupérée à l’étape précédente (cette option est disponible dans l'objet **PurchaseResults.ReceiptXml** qui est renvoyé par la méthode **RequestProductPurchaseAsync**).

L’exemple suivant montre comment acheter et réclamer une offre ciblée à l’aide de l’API de l'espace de noms **Windows.ApplicationModel.Store**. Cet exemple est un extrait de l'[exemple complet](#code-example). Il requiert la bibliothèque [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft, ainsi que des classes supplémentaires et des instructions **using**fournies dans l’exemple complet.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnAnyVersionWindows10)]

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Applications qui utilisent l’espace de noms Windows.Services.Store

1. Après avoir acheté l'extension à l'aide de l'une des méthodes **RequestPurchaseAsync** dans l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), récupérez l’*ID de commande* de l’achat en procédant comme suit. Cette valeur représente la confirmation d’achat.

  1. Appelez la méthode [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetUserCollectionAsync_Windows_Foundation_Collections_IIterable_System_String__) pour obtenir la collection d'objets [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) qui représentent les extensions acquises par l’utilisateur.

  2. Dans cette collection, récupérez l'objet **StoreProduct** qui représente l'extension achetée pour l’offre ciblée.

  3. Récupérez la valeur de la propriété [ExtendedJsonData](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) de cet objet **StoreProduct**. Cette propriété renvoie une chaîne au format JSON qui contient les données complètes du Windows Store pour l'extension.

  4. Analysez cette chaîne JSON et récupérez la valeur du champ *orderId* dans la chaîne.

2. Envoyez un message POST à l’URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` pour soumettre une réclamation pour la conversion de l’offre ciblée. Pour plus d’informations sur cette méthode REST, voir [Réclamer une offre ciblée](claim-a-targeted-offer.md). Dans le corps de la requête, transmettez les objets suivants:

  * Champ *storeOffer*: transmettez l’un des objets JSON reçus dans le corps de la requête de la méthode [Obtenir des offres ciblées](get-targeted-offers.md) (cet objet doit inclure les champs *offers* et *trackingId*).

  * Champ *receipt*: transmettez la valeur *orderId* que vous avez récupérée lors de l’étape précédente.

L’exemple suivant montre comment acheter et réclamer une offre ciblée à l’aide des API de l'espace de noms **Windows.Services.Store**. Cet exemple est un extrait de l'[exemple complet](#code-example). Il requiert la bibliothèque [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft, ainsi que des classes supplémentaires et des instructions **using**fournies dans l’exemple complet.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnWindows1607AndLater)]

<span id="code-example" />
## <a name="complete-code-example"></a>Exemple de code complet

L’exemple de code ci-dessous montre les tâches suivantes:

* Obtenir un jeton de compte MSA pour l’utilisateur actuel.
* Obtenir toutes les offres ciblées pour l’utilisateur actuel grâce à la méthode [Obtenir des offres ciblées](get-targeted-offers.md).
* Achetez l'extension associée à une offre ciblée.
* Réclamez l’offre ciblée grâce à la méthode [Réclamer une offre ciblée](claim-a-targeted-offer.md).

Cet exemple nécessite la bibliothèque [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft. L’exemple utilise cette bibliothèque pour sérialiser et désérialiser les données au format JSON.

> [!NOTE]
> Il existe deux espaces de noms différents pour l’implémentation des achats in-app dans une application UWP: [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) (introduit dans la version1607 de Windows10) et [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) (disponible dans toutes les versions de Windows10). Cet exemple utilise du [code adaptatif de version](../debug-test-perf/version-adaptive-code.md) pour utiliser ces deux espaces de noms dans la même application pour acheter une extension et réclamer l’offre ciblée. Pour utiliser ce code, la version du système d’exploitation cible de votre projet doit être **Windows10 Édition anniversaire (version10.0, build14394)** ou une version ultérieure, bien que la version minimale du système d’exploitation puisse être une version antérieure si vous voulez que votre application s'exécute sur des versions antérieures de Windows10. Pour plus d’informations sur les différences entre ces espaces de noms, voir [Achats dans l’application et versions d’évaluation](in-app-purchases-and-trials.md).

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Rubriques associées

* [Utiliser des offres ciblées pour optimiser l’engagement et les conversions](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Obtenir des offres ciblées](get-targeted-offers.md)
* [Réclamer une offre ciblée](claim-a-targeted-offer.md)
