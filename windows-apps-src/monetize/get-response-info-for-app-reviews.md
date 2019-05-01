---
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: Utilisez cette méthode dans l’API des avis du Microsoft Store pour déterminer si vous pouvez répondre à un avis spécifique, ou si vous pouvez répondre à n’importe quel avis sur une app donnée.
title: Obtenez des informations sur les réponses concernant les avis
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, services du Microsoft Store, API d'avis du Microsoft Store, informations de réponse
ms.localizationpriority: medium
ms.openlocfilehash: 095afd1eab9b7bd0acdac7c38d9e8e99dd59f38c
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63785756"
---
# <a name="get-response-info-for-reviews"></a>Obtenez des informations sur les réponses concernant les avis

Si vous souhaitez répondre de manière programmée à l'avis d'un client sur votre app, vous pouvez utiliser cette méthode dans l'API d'avis du Microsoft Store pour déterminer en premier lieu si vous êtes autorisé à répondre à cet avis. Vous ne pouvez pas répondre aux avis soumis par les clients qui ont choisi de ne pas recevoir de réponses. Après avoir vérifié que vous pouvez répondre à l’avis, vous pouvez utiliser la méthode [Envoyer des réponses aux avis concernant l’application](submit-responses-to-app-reviews.md) pour y répondre par programmation.


## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](respond-to-reviews-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID de l’avis concernant lequel vous souhaitez déterminer si vous disposez d’une possibilité de réponse. Les ID d'avis sont disponibles dans les données de réponse de la méthode [obtenir les avis sur les apps](get-app-reviews.md) de l'API d'analyse du Microsoft Store et dans le [téléchargement hors ligne](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |


### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   | Description                                     |  Obligatoire  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | chaîne | ID Windows Store de l’application qui contient l’avis concernant lequel vous souhaitez déterminer si vous disposez d’une possibilité de réponse. L’ID de Store est disponible sur le [page identité des applications](../publish/view-app-identity-details.md) dans Partner Center. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| reviewId | chaîne | ID de l’avis auquel vous souhaitez répondre (il s’agit d’un GUID). Les ID d'avis sont disponibles dans les données de réponse de la méthode [obtenir les avis sur les apps](get-app-reviews.md) de l'API d'analyse du Microsoft Store et dans le [téléchargement hors ligne](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md). <br/>Si vous omettez ce paramètre, le corps de la réponse de cette méthode indique si vous êtes autorisé à répondre à n’importe quel avis concernant l’application spécifiée. |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples ci-après indiquent comment utiliser cette méthode pour déterminer si vous pouvez répondre à un avis spécifique.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Value      | Type   | Description    |  
|------------|--------|-----------------------|
| CanRespond      | Booléen  | La valeur **true** indique que vous pouvez répondre à l’avis concerné, ou que vous êtes autorisé à répondre à n’importe quel avis concernant l’application spécifiée. Dans le cas contraire, ce champ est défini sur la valeur **false**.       |
| DefaultSupportEmail  | chaîne |  [Adresse e-mail de support](../publish/enter-app-properties.md#support-contact-info) spécifiée dans la description de votre application dans le Windows Store. Si vous n’indiquez aucune adresse e-mail de support, ce champ est vide.    |

 
### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Envoyer des réponses aux évaluations à l’aide de l’API d’analytique Microsoft Store](submit-responses-to-app-reviews.md)
* [Répondre aux révisions de client à l’aide de partenaires](../publish/respond-to-customer-reviews.md)
* [Répondre aux révisions à l’aide des services de Microsoft Store](respond-to-reviews-using-windows-store-services.md)
* [Obtenir les révisions d’application](get-app-reviews.md)
