---
author: Xansky
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: Utilisez cette méthode dans l'API d'avis sur le MicrosoftStore pour soumettre des réponses aux avis sur votre app.
title: Soumettre des réponses aux avis
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, services du MicrosoftStore, API d'avis sur le MicrosoftStore, acquisitions d’extensions
ms.localizationpriority: medium
ms.openlocfilehash: 0fdfe811a90eae1e67ef7f626815be1ef78a4c61
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861521"
---
# <a name="submit-responses-to-reviews"></a>Soumettre des réponses aux avis


Utilisez cette méthode dans l'API d'avis sur le MicrosoftStore pour programmer des réponses aux avis sur votre app. Lorsque vous utilisez cette méthode, vous devez spécifier les ID des avis auxquels vous souhaitez répondre. Les ID d'avis sont disponibles dans les données de réponse de la méthode [obtenir les avis sur les apps](get-app-reviews.md) de l'API d'analyse du MicrosoftStore et dans le [téléchargement hors ligne](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).

Lorsqu’un client envoie un avis, il peut choisir ne pas de recevoir les réponses concernant ses avis. Si vous essayez de répondre à un avis pour lequel le client a choisi de ne pas recevoir de réponses, le corps de réponse de cette méthode indique que la tentative de réponse a échoué. Avant d’appeler cette méthode, vous pouvez également déterminer si vous êtes autorisé à répondre à un avis donné à l’aide de la méthode [obtenir des informations de réponse pour les avis sur les applications](get-response-info-for-app-reviews.md) méthode.

> [!NOTE]
> Vous pouvez répondre aux avis par programme à l'aide de cette méthode, ou bien via le [tableau de bord du Centre de développement Windows](../publish/respond-to-customer-reviews.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](respond-to-reviews-using-windows-store-services.md#prerequisites) relatives à l’API d'avis sur le MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez les ID des avis auxquels vous souhaitez répondre. Les ID d’avis sont disponibles dans les données de réponse de la méthode [Obtenir les avis sur les apps](get-app-reviews.md) dans l’API d’analyse du MicrosoftStore et dans le [téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

Cette méthode n’a aucun paramètre de requête.


### <a name="request-body"></a>Corps de la requête

Le corps de la requête contient les valeurs suivantes:

| Valeur        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------|
| Réponses | tableau | Un tableau d’objets qui contient les données de réponse que vous souhaitez soumettre. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant. |


Chaque objet figurant dans le tableau *Réponses* contient les valeurs suivantes.

| Valeur        | Type   | Description           |  Obligatoire  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | chaîne |  L’ID Windows Store de l’application qui contient l'avis auquel vous souhaitez répondre. L’ID WindowsStore est disponible sur la [page Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID WindowsStore: 9WZDNCRFJ3Q8.   |  Oui  |
| ReviewId | chaîne |  L’ID de l’avis auquel vous souhaitez répondre (il s’agit d’un GUID). Les ID d’avis sont disponibles dans les données de réponse de la méthode [Obtenir les avis sur les applications](get-app-reviews.md) dans l’API d’analyse du MicrosoftStore et dans le [téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).   |  Oui  |
| ResponseText | chaîne | La réponse que vous souhaitez soumettre. Votre réponse doit suivre [les recommandations suivantes](../publish/respond-to-customer-reviews.md#guidelines-for-responses).   |  Oui  |
| SupportEmail | chaîne | L'adresse e-mail de support de votre application que votre client pourra utiliser par la suite pour vous contacter directement. Vous devez spécifier une adresse e-mail valide.     |  Oui  |
| IsPublic | Booléen |  Si vous spécifiez **true**, votre réponse s’affichera dans Description de votre application, juste en dessous de l’avis du client et elle sera visible pour tous les clients. Si vous spécifiez **false** et l’utilisateur n’a pas encore choisi de ne pas recevoir de réponses de courrier électronique, votre réponse sera envoyée au client par courrier électronique, et il ne sera pas visible pour les autres clients dans la description de votre application. Si vous spécifiez **false** et que l’utilisateur a choisi de ne pas recevoir de réponses des messages, une erreur sera renvoyée.   |  Oui  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant illustre comment utiliser cette méthode pour envoyer des réponses à plusieurs avis.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "true"
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "false"
    }
  ]
}
```

## <a name="response"></a>Réponse

### <a name="response-body"></a>Corps de la réponse

| Valeur        | Type   | Description            |
|---------------|--------|---------------------|
| Résultat | tableau | Un tableau d’objets comportant des données sur chaque réponse que vous avez soumise. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.  |


Chaque objet figurant dans le tableau *Résultat* contient les valeurs suivantes.

| Valeur        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | chaîne |  L’ID Windows Store de l’application qui contient l'avis auquel vous avez répondu. Exemple d’ID WindowsStore: 9WZDNCRFJ3Q8.   |
| ReviewId | chaîne |  L’ID de l’avis auquel vous avez répondu. Il s’agit d’un GUID.   |
| Réussite | chaîne | La valeur **true** indique que votre réponse a été envoyée avec succès. La valeur **false** indique que votre réponse a échoué.    |
| FailureReason | chaîne | Si la valeur de **Réussite** est égale à **false**, cette valeur contient le motif de l’échec. Si la valeur **Réussite** est égale à **true**, cette valeur est vide.      |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Répondre aux avis des clients à l’aide du tableau de bord du Centre de développement](../publish/respond-to-customer-reviews.md)
* [Répondre aux avis à l'aide des services MicrosoftStore](respond-to-reviews-using-windows-store-services.md)
* [Obtenir des informations de réponse pour les avis sur les applications](get-response-info-for-app-reviews.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)
