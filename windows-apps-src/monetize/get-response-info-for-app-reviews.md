---
author: mcleanbyron
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: "Utilisez cette méthode dans l’API d’avis du Windows Store pour déterminer si vous pouvez ou non répondre à un avis spécifique ou à n’importe quel avis concernant une application donnée."
title: "Obtenir des informations de réponse pour les avis sur les applications"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, services du Windows Store, API d’avis du Windows Store, informations de réponse"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 88e6158bfc5df23e5c2056624e353b38b39c7331
ms.lasthandoff: 02/08/2017

---

# <a name="get-response-info-for-app-reviews"></a>Obtenir des informations de réponse pour les avis sur les applications

Si vous souhaitez répondre par programmation à un avis de client concernant votre application, vous pouvez utiliser cette méthode dans l’API d’avis du Windows Store pour commencer par déterminer si vous êtes autorisé à répondre à cet avis. Vous ne pouvez pas répondre aux avis soumis par les clients qui ont choisi de ne pas recevoir de réponses. Après avoir vérifié que vous pouvez répondre à l’avis, vous pouvez utiliser la méthode [Envoyer des réponses aux avis concernant l’application](submit-responses-to-app-reviews.md) pour y répondre par programmation.


## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez commencer par effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](respond-to-reviews-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Windows Store.
* [Obtenez un jeton d’accès Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID de l’avis concernant lequel vous souhaitez déterminer si vous disposez d’une possibilité de réponse. Les ID d’avis sont disponibles dans les données de réponse de la méthode [Obtenir les avis sur les applications](get-app-reviews.md) dans l’API d’analyse du Windows Store et dans le [téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md).

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |

<span/> 

### <a name="request-header"></a>En-tête de la requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/> 

### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   | Description                                     |  Obligatoire  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | chaîne | ID Windows Store de l’application qui contient l’avis concernant lequel vous souhaitez déterminer si vous disposez d’une possibilité de réponse. L’ID Windows Store est disponible dans la page [Identité de l’application](../publish/view-app-identity-details.md) du tableau de bord du Centre de développement. Exemple d’ID Windows Store : 9WZDNCRFJ3Q8. |  Oui  |
| reviewId | chaîne | ID de l’avis auquel vous souhaitez répondre (il s’agit d’un GUID). Les ID d’avis sont disponibles dans les données de réponse de la méthode [Obtenir les avis sur les applications](get-app-reviews.md) dans l’API d’analyse du Windows Store et dans le [téléchargement hors connexion](../publish/download-analytic-reports.md) du [rapport Avis](../publish/reviews-report.md). <br/>Si vous omettez ce paramètre, le corps de la réponse de cette méthode indique si vous êtes autorisé à répondre à n’importe quel avis concernant l’application spécifiée. |  Non  |

<span/>

### <a name="request-example"></a>Exemple de requête

Les exemples ci-après indiquent comment utiliser cette méthode pour déterminer si vous pouvez répondre à un avis spécifique.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description    |  
|------------|--------|-----------------------|
| CanRespond      | Booléen  | La valeur **true** indique que vous pouvez répondre à l’avis concerné, ou que vous êtes autorisé à répondre à n’importe quel avis concernant l’application spécifiée. Dans le cas contraire, ce champ est défini sur la valeur **false**.       |
| DefaultSupportEmail  | chaîne |  [Adresse e-mail de support](../publish/create-app-store-listings.md#support-contact-info) spécifiée dans la description de votre application dans le Windows Store. Si vous n’indiquez aucune adresse e-mail de support, ce champ est vide.    |

<span/>
 
### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>Articles connexes

* [Envoyer des réponses aux avis à l’aide de l’API d’analyse du Windows Store](submit-responses-to-app-reviews.md)
* [Répondre aux avis des clients à l’aide du tableau de bord du Centre de développement](../publish/respond-to-customer-reviews.md)
* [Répondre aux avis à l’aide des services du Windows Store](respond-to-reviews-using-windows-store-services.md)
* [Obtenir les avis sur les applications](get-app-reviews.md)

