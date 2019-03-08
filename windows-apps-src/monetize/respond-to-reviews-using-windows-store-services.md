---
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Utilisez l’API d’avis du Microsoft Store pour soumettre par programmation les réponses aux avis sur votre app dans le Microsoft Store.
title: Répondre aux avis à l’aide des services du Windows Store
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, API d'avis du Microsoft Store, répondre aux avis
ms.localizationpriority: medium
ms.openlocfilehash: 677108e692bbc702778cad3c42a45b4f5408b8cd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653164"
---
# <a name="respond-to-reviews-using-store-services"></a>Répondre aux avis à l’aide des services du Windows Store

Utilisez l'*API d'avis du Microsoft Store* pour répondre par programmation aux avis sur votre app dans le Microsoft Store. Cette API est particulièrement utile pour les développeurs qui souhaitent en bloc répondent aux nombreux révisions sans utiliser de partenaires. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API d’avis du Microsoft Store, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60 minutes pour l’utiliser dans les appels à l’API d’avis du Microsoft Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API d’avis du Microsoft Store](#call-the-windows-store-reviews-api).

> [!NOTE]
> Outre l’utilisation du Microsoft Store passe en revue les API répondre par programmation aux révisions, vous pouvez également répondre aux révisions [à l’aide de partenaires](../publish/respond-to-customer-reviews.md).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Étape 1 : Conditions préalables pour l’utilisation du Microsoft Store passe en revue les API

Avant d’écrire le code d’appel de l’API d’avis du Microsoft Store, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](https://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Sinon, vous pouvez [créer un nouvelle application Azure AD dans partenaires](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sans aucun frais supplémentaire.

* Vous devez associer une application Azure AD à votre compte espace partenaires, récupérer l’ID client et ID client pour l’application et générer une clé. L’application Azure AD est l’app ou le service à partir duquel vous allez appeler l’API d’avis du Microsoft Store. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.
    > [!NOTE]
    > Cette tâche ne doit être effectuée qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte espace partenaires et récupérer les valeurs requises :

1.  Dans le centre de partenaires, [associer un compte espace partenaires de votre organisation avec un annuaire Azure AD de votre organisation](../publish/associate-azure-ad-with-partner-center.md).

2.  Ensuite, à partir de la **utilisateurs** page dans le **paramètres du compte** section de partenaires, [ajouter l’application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) qui représente l’application ou le service que vous utiliserez pour répondre aux révisions. Assurez-vous d'attribuer à cette application le rôle **Manager**. Si l’application n’existe pas encore dans votre annuaire Azure AD, vous pouvez [créer une nouvelle application Azure AD dans le centre partenaires](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Revenez à la page **Utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour plus d’informations, voir [Gérer les clés pour une application Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API d’avis du Microsoft Store, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Authorization** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

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

Pour le *locataire\_id* valeur dans l’URI de la publication et la *client\_id* et *client\_secret* paramètres, spécifiez le locataire ID, ID de client et la clé de votre application que vous avez récupérées à partir du centre de partenaires dans la section précédente. Pour le paramètre *resource*, vous devez spécifier ```https://manage.devcenter.microsoft.com```.

Une fois votre jeton d’accès arrivé à expiration, vous pouvez l’actualiser en suivant les instructions fournies [ici](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Étape 3 : Appeler les API de révisions Microsoft Store

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API d’avis du Microsoft Store. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

L’API d’avis du Microsoft Store contient plusieurs méthodes que vous pouvez utiliser pour déterminer si vous êtes autorisé à répondre à un avis spécifique et à envoyer des réponses à un ou plusieurs avis. Suivez cette procédure pour utiliser cette API :

1. Obtenez les ID des avis auxquels vous souhaitez répondre. Les ID d'avis sont disponibles dans les données de réponse de la méthode [obtenir les avis sur les apps](get-app-reviews.md) de l'API d'analyse du Microsoft Store et dans le [téléchargement hors ligne](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).
2. Appelez la méthode [Obtenir des informations de réponse pour les avis sur les applications](get-response-info-for-app-reviews.md) pour déterminer si vous êtes autorisé à répondre aux avis. Lorsqu’un client envoie un avis, il peut choisir ne pas de recevoir les réponses concernant ses avis. Vous ne pouvez pas répondre aux avis soumis par les clients qui ont choisi de ne pas recevoir de réponses.
3. Appelez la méthode [Envoyer des réponses aux avis concernant l’application](submit-responses-to-app-reviews.md) pour répondre par programmation aux avis.


## <a name="related-topics"></a>Rubriques connexes

* [Obtenir les révisions d’application](get-app-reviews.md)
* [Obtenir les informations de réponse pour les révisions d’application](get-response-info-for-app-reviews.md)
* [Envoie les réponses à des révisions d’application](submit-responses-to-app-reviews.md)

 
