---
author: Xansky
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Utilisez l’API de promotions du MicrosoftStore pour gérer par programmation les campagnes publicitaires promotionnelles pour les apps qui sont enregistrées sur votre compte personnel ou compte d’organisation du Centre de développement Windows.
title: Gérer des campagnes publicitaires à l’aide des services du Windows Store
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, API de promotions du MicrosoftStore, campagnes de publicité
ms.localizationpriority: medium
ms.openlocfilehash: 6a1f08dbd70904284179c90fa00d2abd9acf4b97
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5133698"
---
# <a name="run-ad-campaigns-using-store-services"></a>Gérer des campagnes publicitaires à l’aide des services du Windows Store

Utilisez l’*API de promotions du MicrosoftStore* pour gérer par programmation les campagnes publicitaires promotionnelles pour les apps qui sont enregistrées sur votre compte personnel ou compte d’organisation du Centre de développement Windows. Cette API permet de créer, de mettre à jour et de surveiller vos campagnes et d'autres ressources connexes, telles que des options créatives et de ciblage. Cette API est particulièrement utile pour les développeurs qui créent des volumes importants de campagnes publicitaires, et qui souhaitent le faire sans utiliser le tableau de bord du Centre de développement Windows. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout:

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API de promotions du MicrosoftStore, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API des promotions du MicrosoftStore. Passé ce délai, le jeton n’est plus valable. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API de promotions du MicrosoftStore](#call-the-windows-store-promotions-api).

Vous pouvez également créer et gérer les campagnes publicitaires à l’aide du tableau de bord du Centre de développement Windows et toutes les campagnes publicitaires que vous créez programmatiquement via l’API de promotions du MicrosoftStore sont également accessibles à partir du tableau de bord. Pour plus d'informations sur la gestion des campagnes publicitaires dans le tableau de bord, consultez la section [Création d’une campagne de publicité pour votre application](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Tous les développeurs disposant d’un compte Centre de développement Windows peuvent utiliser l’API de promotions du MicrosoftStore pour gérer les campagnes publicitaires pour leurs apps. Les agences média peuvent également demander l’accès à cette API pour diffuser des campagnes publicitaires pour le compte de leurs annonceurs. Si vous êtes une agence média et que vous souhaitez en savoir plus sur cette API ou en demander l'accès, faites parvenir votre demande à l'adresse storepromotionsapi@microsoft.com.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Étape 1: Remplir les conditions préalables pour utiliser l’API de promotions du MicrosoftStore

Avant d’écrire le code d’appel de l’API de promotions du MicrosoftStore, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Avant de pouvoir créer et démarrer une campagne publicitaire à l’aide de cette API, vous devez d’abord [créer une campagne publicité payée à l’aide de la page **promouvoir votre application** du tableau de bord du centre de développement](../publish/create-an-ad-campaign-for-your-app.md), et vous devez ajouter au moins un instrument de paiement sur cette page. Après cela, vous serez en mesure de créer des lignes de livraison facturables pour les campagnes publicitaires à l’aide de cette API. Les lignes de livraison des campagnes publicitaires que vous créez à l’aide de cette API factureront automatiquement l’instrument de paiement sélectionné par défaut sur la page **Promouvoir votre application** du tableau de bord.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un annuaire Azure AD dans le Centre de développement](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account) sans frais supplémentaires.

* Vous devez associer une application Azure AD à votre compte du Centre de développement, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD est l’app ou le service à partir duquel vous allez appeler l’API de promotions du MicrosoftStore. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Cette tâche ne doit être effectuée qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte du Centre de développement Windows et récupérer les valeurs nécessaires:

1.  Dans le Centre de développement, [associez le compte du Centre de développement de votre organisation à l'annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-dev-center.md).

2.  Ensuite, dans la page **Utilisateurs** dans la section **Paramètres de compte** du Centre de développement, [ajoutez l’application AzureAD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-dev-center-account) qui représente l’application ou le service que vous allez utiliser pour gérer les campagnes de promotion pour votre compte du Centre de développement. Assurez-vous d'attribuer à cette application le rôle **Manager**. Si l’application n’existe pas encore dans votre annuaire Azure AD, vous pouvez [créer une application Azure AD dans le Centre de développement](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account). 

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape2: Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API de promotions du MicrosoftStore, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Autorisation** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

Pour obtenir le jeton d’accès, suivez les instructions présentées dans l’article [Appels de service à service à l’aide des informations d’identification du client](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) pour envoyer une requête HTTP POST au point de terminaison ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Voici un exemple de requête.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Pour la valeur *tenant\_id* dans l’URI POST et les paramètres *client\_id* et *client\_secret*, spécifiez l’ID de locataire, l’ID client et la clé pour votre application que vous avez récupérés à partir du Centre de développement à l’étape précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Étape3: Appeler l’API de promotions du MicrosoftStore

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API de promotions du MicrosoftStore. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

Dans le contexte de l’API de promotions du MicrosoftStore, une campagne publicitaire se compose d’un objet *campaign* qui contient des informations générales sur la campagne et d’autres objets qui représentent les *chaînes de distribution*, les *profils de ciblage* et les *contenus créatifs* de la campagne publicitaire. L’API inclut différents ensembles de méthodes qui sont regroupés par ces types d’objets. Pour créer une campagne, vous appelez généralement une autre méthode POST pour chacun de ces objets. L’API fournit également les méthodes GET que vous pouvez utiliser pour récupérer un objet et les méthodes PUT que vous pouvez utiliser pour modifier une campagne, une chaîne et distribution et les objets des profils de ciblage.

Pour plus d’informations sur ces objets et leurs méthodes associées, voir le tableau suivant.


| Objet       | Description   |
|---------------|-----------------|
| Campagnes |  Cet objet représente la campagne publicitaire, et il se situe en haut de la hiérarchie du modèle objet pour les campagnes publicitaires. Cet objet identifie le type de campagne que vous diffusez (payante, maison ou annonce communautaire), l’objectif de campagne, les chaînes de distribution de la campagne et d’autres détails. Chaque campagne ne peut être associée qu’à une seule application.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, voir [Gérer les campagnes publicitaires](manage-ad-campaigns.md).<br/><br/>**Remarque**&nbsp;&nbsp;Après avoir créé une campagne publicitaire, vous pouvez récupérer des données de performance de la campagne à l’aide de la méthode [Obtenir les données relatives aux performances de la campagne publicitaire](get-ad-campaign-performance-data.md) dans l'[l’API d’analyse du MicrosoftStore](access-analytics-data-using-windows-store-services.md).  |
| Chaînes de distribution | Chaque campagne comporte une ou plusieurs chaînes de distribution qui sont utilisées pour acquérir du stock et diffuser vos annonces. Pour chaque chaîne de distribution, vous pouvez définir le ciblage, définir votre prix de soumission et définir le montant que vous souhaitez dépenser en spécifiant le budget et en lui associant les contenus que vous souhaitez utiliser.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, voir [Gérer les chaînes de distribution des campagnes publicitaires](manage-delivery-lines-for-ad-campaigns.md). |
| Profils de ciblage | Chaque chaîne de distribution possède un profil de ciblage qui spécifie les utilisateurs, les zones géographiques et les types de stock que vous voulez cibler. Les profils de ciblage peuvent être créés en tant que modèle et être partagés entre les chaînes de distribution.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, voir [Gérer les profils de ciblage pour les campagnes publicitaires](manage-targeting-profiles-for-ad-campaigns.md). |
| Contenus | Chaque chaîne de distribution contient un ou plusieurs contenus qui représentent les annonces diffusées aux clients dans le cadre de la campagne. Un contenu peut être associé à plusieurs chaînes de distribution, dans plusieurs campagnes, sous réserve qu'il représente toujours la même application.<br/><br/>Pour plus d’informations sur les méthodes associées à cet objet, voir [Gérer les contenus des campagnes publicitaires](manage-creatives-for-ad-campaigns.md). |


Le diagramme suivant illustre la relation entre les campagnes, les chaînes de distribution, les profils de ciblage et les contenus.

![Hiérarchie de la campagne publicitaire](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment obtenir un jeton d’accès AzureAD et appeler l’API de promotions du MicrosoftStore à partir d’une app de console C#. Pour utiliser cet exemple de code, affectez les variables *tenantId*, *clientId*, *clientSecret*, et *appID* aux valeurs appropriées pour votre scénario. Cet exemple requiert le [package Json.NET](http://www.newtonsoft.com/json) de Newtonsoft afin de désérialiser les données JSON renvoyées par l’API de promotions du MicrosoftStore.

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Rubriquesassociées

* [Gérer les campagnes publicitaires](manage-ad-campaigns.md)
* [Gérer les chaînes de distribution des campagnes publicitaires](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes publicitaires](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les contenus des campagnes publicitaires](manage-creatives-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances des campagnes publicitaires](get-ad-campaign-performance-data.md)


 
