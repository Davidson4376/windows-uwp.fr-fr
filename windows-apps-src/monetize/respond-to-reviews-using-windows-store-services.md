---
author: mcleanbyron
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: "Utilisez l’API d’avis du Windows Store pour répondre par programmation aux avis concernant votre application dans le Windows Store."
title: "Répondre aux avis à l’aide des services du Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API d’avis du Windows Store, répondre aux avis"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 657149304048a88bf85f0dd6f205e7db0497e591
ms.lasthandoff: 02/08/2017

---

# <a name="respond-to-reviews-using-windows-store-services"></a>Répondre aux avis à l’aide des services du Windows Store

Utilisez cette méthode dans l’*API d’avis du Windows Store* pour répondre par programmation aux avis concernant votre application dans le Windows Store. Cette API est particulièrement utile pour les développeurs qui souhaitent répondre en bloc à de nombreux avis sans utiliser le tableau de bord du Centre de développement Windows. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

Les étapes suivantes décrivent le processus de bout en bout :

1.  Vérifiez que vous avez rempli toutes les [conditions préalables](#prerequisites).
2.  Avant d’appeler une méthode dans l’API d’avis du Windows Store, [procurez-vous un jeton d’accès Azure AD](#obtain-an-azure-ad-access-token). Une fois le jeton obtenu, vous avez 60 minutes pour l’utiliser dans les appels à l’API d’avis du Windows Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
3.  [Appelez l’API d’avis du Windows Store](#call-the-windows-store-reviews-api).

>**Remarque**&nbsp;&nbsp;En plus de l’API d’avis du Windows Store, vous pouvez également [utiliser le tableau de bord du Centre de développement Windows](../publish/respond-to-customer-reviews.md) pour répondre par programmation aux avis.

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-reviews-api"></a>Étape 1 : Remplir les conditions préalables à l’utilisation de l’API d’avis du Windows Store

Avant d’écrire le code d’appel de l’API d’avis du Windows Store, vérifiez que vous remplissez bien les conditions préalables suivantes.

* Vous (ou votre organisation) devez disposer d’un annuaire Azure AD et d’une autorisation [Administrateur global](http://go.microsoft.com/fwlink/?LinkId=746654) pour l’annuaire. Si vous utilisez déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’un annuaire Azure AD. Dans le cas contraire, vous pouvez [créer un annuaire Azure AD dans le Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sans frais supplémentaires.

* Vous devez associer une application Azure AD à votre compte du Centre de développement, récupérer l’ID de locataire et l’ID client pour l’application et générer une clé. L’application Azure AD est l’application ou le service à partir duquel vous allez appeler l’API d’avis du Windows Store. Vous avez besoin de l’ID de locataire, de l’ID client et de la clé pour obtenir le jeton d’accès Azure AD à transmettre à l’API.

  >**Remarque**&nbsp;&nbsp;Vous n’avez besoin d’effectuer cette tâche qu’une seule fois. Une fois que vous avez l’ID de locataire, l’ID client et la clé à disposition, vous pouvez les réutiliser chaque fois que vous avez besoin de créer un nouveau jeton d’accès Azure AD.

Pour associer une application Azure AD à votre compte du Centre de développement Windows et récupérer les valeurs nécessaires :

1.  Dans le Centre de développement, accédez à vos **Paramètres du compte**, cliquez sur **Gérer les utilisateurs**, puis associez le compte du Centre de développement de votre organisation à son annuaire Azure AD. Pour obtenir des instructions détaillées, voir [Gérer les utilisateurs de comptes](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  Dans la page **Gérer les utilisateurs**, cliquez sur **Ajouter des applications Azure AD**, ajoutez l’application Azure AD qui représente l’application ou le service que vous allez utiliser pour gérer les campagnes de promotion de votre compte du Centre de développement, puis affectez-lui le rôle **Gestionnaire**. Si cette application existe déjà dans votre annuaire Azure AD, vous pouvez la sélectionner dans la page **Ajouter des applications Azure AD** pour l’ajouter à votre compte du Centre de développement. Sinon, vous pouvez créer une application Azure AD dans la page **Ajouter des applications Azure AD**. Pour plus d’informations, voir [Ajouter et gérer des applications Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Revenez à la page **Gérer les utilisateurs**, cliquez sur le nom de votre application Azure AD pour accéder aux paramètres de l’application, puis notez les valeurs des champs **ID de locataire** et **ID client**.

4. Cliquez sur **Ajouter une clé**. Dans l’écran suivant, notez la valeur du champ **Clé**. Vous ne pourrez plus accéder à ces informations une fois que vous aurez quitté cette page. Pour en savoir plus, consultez les informations sur la gestion des clés dans l’article [Ajouter et gérer des applications Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Étape 2 : Obtenir un jeton d’accès Azure AD

Avant d’appeler l’une des méthodes dans l’API d’avis du Windows Store, vous devez d’abord obtenir un jeton d’accès Azure AD pour le passer à l’en-tête **Authorization** de chaque méthode de l’API. Après avoir obtenu un jeton d’accès, vous disposez de 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez l’actualiser pour pouvoir continuer à l’utiliser dans d’autres appels à l’API.

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

<span id="call-the-windows-store-reviews-api" />
## <a name="step-3-call-the-windows-store-reviews-api"></a>Étape 3 : Appeler l’API d’avis du Windows Store

Une fois que vous disposez d’un jeton d’accès Azure AD, vous pouvez appeler l’API d’avis du Windows Store. Vous devez transmettre le jeton d’accès à l’en-tête **Authorization** de chaque méthode.

L’API d’avis du Windows Store contient plusieurs méthodes que vous pouvez utiliser pour déterminer si vous êtes autorisé à répondre à un avis spécifique et à envoyer des réponses à un ou plusieurs avis. Suivez cette procédure pour utiliser cette API :

1. Obtenez les ID des avis auxquels vous souhaitez répondre. Les ID d’avis sont disponibles dans les données de réponse de la méthode [Obtenir les avis sur les applications](get-app-reviews.md) dans l’API d’analyse du Windows Store et dans le [téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).
2. Appelez la méthode [Obtenir des informations de réponse pour les avis sur les applications](get-response-info-for-app-reviews.md) pour déterminer si vous êtes autorisé à répondre aux avis. Lorsqu’un client envoie un avis, il peut choisir ne pas de recevoir les réponses concernant ses avis. Vous ne pouvez pas répondre aux avis soumis par les clients qui ont choisi de ne pas recevoir de réponses.
3. Appelez la méthode [Envoyer des réponses aux avis concernant l’application](submit-responses-to-app-reviews.md) pour répondre par programmation aux avis.


## <a name="related-topics"></a>Rubriques connexes

* [Obtenir les avis sur les applications](get-app-reviews.md)
* [Obtenir des informations de réponse pour les avis sur les applications](get-response-info-for-app-reviews.md)
* [Envoyer des réponses aux avis concernant l’application](submit-responses-to-app-reviews.md)

 

