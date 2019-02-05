---
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Utilisez l’API des offres ciblées du MicrosoftStore pour récupérer les offres ciblées disponibles pour l'utilisateur actuel de votre app.
title: Gérer les offres ciblées à l’aide des services du WindowsStore
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, services du MicrosoftStore, API des offres ciblées du MicrosoftStore, offres ciblées
ms.localizationpriority: medium
ms.openlocfilehash: bcf270bd56d17936ef404adbc3663034b58e7a2c
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2019
ms.locfileid: "9045022"
---
# <a name="manage-targeted-offers-using-store-services"></a>Gérer les offres ciblées à l’aide des services du WindowsStore

Si vous créez un *ciblées offre* dans la page **> impliquer des offres ciblées** pour votre application dans l’espace partenaires, utilisez l' *API des offres ciblées du Microsoft Store* dans le code de votre application pour récupérer les informations qui vous permet d’implémenter l’expérience dans l’application pour le offre ciblée. Pour plus d’informations concernant les offres ciblées et leur création dans le tableau de bord, consultez [Utiliser les offres ciblées pour optimiser l’engagement et les conversions](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

L’API des offres ciblées est une API REST simple que vous pouvez utiliser afin d’obtenir les offres ciblées disponibles pour l’utilisateur actuel, basée sur le fait que l’utilisateur fasse partie ou non du segment de clientèle pour l’offre ciblée. Pour utiliser cette API dans le code de votre app, procédez ainsi:

1.  [Obtenez un jeton de compte Microsoft](#obtain-a-microsoft-account-token) pour l’utilisateur de votre application actuellement connecté.
2.  [Obtenez les offres ciblées pour l’utilisateur actuellement connecté](#get-targeted-offers).
3.  Implémenter l’expérience d’achat in-app pour l’extension associée à l'une des offres ciblés. Pour plus d’informations sur l’implémentation des achats in-app, consultez [cet article](enable-in-app-purchases-of-apps-and-add-ons.md).

Pour obtenir un exemple de code complet qui montre toutes ces étapes, voir l'[exemple de code](#code-example) à la fin de cet article. Les sections suivantes fournissent plus d’informations sur chaque étape.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtenez un jeton de compte Microsoft pour l’utilisateur actuellement connecté

Dans le code de votre app, obtenez un jeton de compte Microsoft (MSA) pour l’utilisateur actuellement connecté. Vous devez transmettre ce jeton dans l’en-tête de requête ```Authorization``` de l’API des offres ciblées du MicrosoftStore. Ce jeton est utilisé par le Store pour récupérer les offres ciblées disponibles pour l’utilisateur actuellement connecté.

Pour obtenir le jeton du MAS, utilisez la classe [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) pour demander un jeton selon la règle ```devcenter_implicit.basic,wl.basic```. L’exemple suivant montre comment procéder. Cet exemple est un extrait de l'[exemple complet](#code-example) et nécessite des instructions **using** fournies dans l’exemple complet.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Pour plus d’informations sur l’obtention des jetons MSA, consultez la section [Gestionnaire de comptes web](../security/web-account-manager.md).

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtenez les offres ciblées pour l’utilisateur actuellement connecté

Dès que vous disposez d’un jeton de compte Microsoft pour l’utilisateur actuellement connecté, appelez la méthode GET de l’URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` pour obtenir les offres ciblées disponibles pour cet utilisateur. Pour plus d’informations sur cette méthode REST, voir [Obtenir des offres ciblées](get-targeted-offers.md).

Cette méthode renvoie les ID de produit des modules complémentaires, qui sont associés aux offres ciblées disponibles pour l’utilisateur actuel. Ces informations vous permettent de proposer à l’utilisateur une ou plusieurs des offres ciblées sous forme d’achat in-app.

L’exemple suivant montre comment obtenir les offres ciblées pour l’utilisateur actuel. Cet exemple est un extrait de l'[exemple complet](#code-example). Il requiert la bibliothèque [Json.NET](https://www.newtonsoft.com/json) de Newtonsoft, ainsi que des classes supplémentaires et des instructions **using**fournies dans l’exemple complet.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>Exemple de code complet

L’exemple de code ci-dessous montre les tâches suivantes:

* Obtenir un jeton de compte MSA pour l’utilisateur actuel.
* Obtenir toutes les offres ciblées pour l’utilisateur actuel grâce à la méthode [Obtenir des offres ciblées](get-targeted-offers.md).
* Achetez l'extension associée à une offre ciblée.

Cet exemple nécessite la bibliothèque [Json.NET](https://www.newtonsoft.com/json) de Newtonsoft. L’exemple utilise cette bibliothèque pour sérialiser et désérialiser les données au format JSON.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Rubriquesassociées

* [Utiliser des offres ciblées pour optimiser l’engagement et les conversions](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Obtenir des offres ciblées](get-targeted-offers.md)
