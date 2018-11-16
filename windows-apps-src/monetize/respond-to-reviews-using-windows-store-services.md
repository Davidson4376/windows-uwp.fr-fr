---
author: Xansky
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Utilisez l’API d’avis du MicrosoftStore pour soumettre par programmation les réponses aux avis sur votre app dans le MicrosoftStore.
title: Répondre aux avis à l’aide des services du Windows Store
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows10, uwp, API d'avis du MicrosoftStore, répondre aux avis
ms.localizationpriority: medium
ms.openlocfilehash: 063c228a9a2fcfde9350af4872aabba44f9bb8a5
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843083"
---
# <a name="respond-to-reviews-using-store-services"></a>Répondre aux avis à l’aide des services du Windows Store

Utilisez l'*API d'avis du MicrosoftStore* pour répondre par programmation aux avis sur votre app dans le MicrosoftStore. Cette API est particulièrement utile pour les développeurs qui souhaitent en bloc répondent à de nombreux avis sans utiliser l’espace partenaires. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout:

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API d’avis du MicrosoftStore, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60minutes pour l’utiliser dans les appels à l’API d’avis du MicrosoftStore avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API d’avis du MicrosoftStore](#call-the-windows-store-reviews-api).

> [!NOTE]
> Outre l’utilisation du Microsoft Store passe en revue les API pour répondre par programmation aux avis, vous pouvez répondre aux avis [à l’aide de l’espace partenaires](../publish/respond-to-customer-reviews.md).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Étape1: Remplir les conditions préalables à l’utilisation de l’API d’avis du MicrosoftStore

Avant d’écrire le code d’appel de l’API d’avis du MicrosoftStore, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un nouvel Azure AD dans l’espace partenaires](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) pour sans frais supplémentaires.

* Vous devez associer une application Azure AD avec votre compte espace partenaires, récupérer l’ID de locataire et ID de client pour l’application et générer une clé. L’application Azure AD est l’app ou le service à partir duquel vous allez appeler l’API d’avis du MicrosoftStore. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Cette tâche ne doit être effectuée qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

Pour associer une application Azure AD avec votre compte espace partenaires et récupérer les valeurs nécessaires:

1.  Dans l’espace partenaires, [Associez le compte de l’espace partenaires de votre organisation avec un annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-dev-center.md).

2.  Ensuite, dans la page **utilisateurs** dans la section **paramètres du compte** de l’espace partenaires, [Ajoutez l’application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous allez utiliser pour répondre aux avis. Assurez-vous d'attribuer à cette application le rôle **Manager**. Si l’application n’existe pas encore dans votre répertoire Azure Active directory, vous pouvez [créer une nouvelle application Azure AD dans l’espace partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape2: Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API d’avis du MicrosoftStore, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Authorization** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous disposez de 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

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

Pour la valeur *tenant\_id* dans l’URI POST et les paramètres *client\_id* et *client\_secret* , spécifiez l’ID de locataire, ID client et la clé pour votre application que vous avez récupérées à partir de l’espace partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Étape3: Appeler l’API d’avis du MicrosoftStore

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API d’avis du MicrosoftStore. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

L’API d’avis du MicrosoftStore contient plusieurs méthodes que vous pouvez utiliser pour déterminer si vous êtes autorisé à répondre à un avis spécifique et à envoyer des réponses à un ou plusieurs avis. Suivez cette procédure pour utiliser cette API:

1. Obtenez les ID des avis auxquels vous souhaitez répondre. Les ID d’avis sont disponibles dans les données de réponse de la méthode [Obtenir les avis sur les apps](get-app-reviews.md) dans l’API d’analyse du MicrosoftStore et dans le [téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).
2. Appelez la méthode [Obtenir des informations de réponse pour les avis sur les applications](get-response-info-for-app-reviews.md) pour déterminer si vous êtes autorisé à répondre aux avis. Lorsqu’un client envoie un avis, il peut choisir ne pas de recevoir les réponses concernant ses avis. Vous ne pouvez pas répondre aux avis soumis par les clients qui ont choisi de ne pas recevoir de réponses.
3. Appelez la méthode [Envoyer des réponses aux avis concernant l’application](submit-responses-to-app-reviews.md) pour répondre par programmation aux avis.


## <a name="related-topics"></a>Rubriques connexes

* [Obtenir les avis sur les applications](get-app-reviews.md)
* [Obtenir des informations de réponse pour les avis sur les applications](get-response-info-for-app-reviews.md)
* [Envoyer des réponses aux avis concernant l’application](submit-responses-to-app-reviews.md)

 
