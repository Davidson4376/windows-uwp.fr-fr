---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: "Utilisez cette méthode pour renouveler une clé du Windows Store."
title: "Renouveler une clé d’ID du Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de collection du Windows Store, API d’achat du Windows Store, clé d’ID du Windows Store, renouveler"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b740cf431607f1748a8513a02746a70560d09da2
ms.lasthandoff: 02/07/2017

---

# <a name="renew-a-windows-store-id-key"></a>Renouveler une clé d’ID du Windows Store


Utilisez cette méthode pour renouveler une clé du Windows Store. Lorsque vous [générez une clé d’ID du Windows Store](view-and-grant-products-from-a-service.md#step-4), la clé est valide pendant 90 jours. Après l’expiration de la clé, vous pouvez utiliser la clé arrivée à expiration pour en renégocier une nouvelle à l’aide de cette méthode.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez disposer des éléments suivants :

* un jeton d’accès Azure AD créé avec l’URI d’audience `https://onestore.microsoft.com` ;
* une clé d’ID du Windows Store expirée qui a été [générée à partir du code côté client de votre application](view-and-grant-products-from-a-service.md#step-4).

Pour plus d’informations, voir [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Type de clé    | Méthode | URI de la requête                                              |
|-------------|--------|----------------------------------------------------------|
| Collections | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Achat    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |

<span/>

### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | chaîne | Doit être défini sur la valeur **collections.mp.microsoft.com** ou **purchase.mp.microsoft.com**.           |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |

<span/>

### <a name="request-body"></a>Corps de la requête

| Paramètre     | Type   | Description                       | Obligatoire |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | chaîne | Jeton d’accès Azure AD.        | Oui      |
| key           | chaîne | Clé d’ID du Windows Store arrivée à expiration. | Non       |

<span/> 

### <a name="request-example"></a>Exemple de requête

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Paramètre | Type   | Description                                                                                                            | Obligatoire |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| key       | chaîne | Clé du Windows Store actualisée qui peut être utilisée dans les futurs appels de l’API de collection ou de l’API d’achat du Windows Store. | Non       |

<span/>

### <a name="response-example"></a>Exemple de réponse

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>Codes d’erreur


| Code | Erreur        | Code d’erreur interne           | Description                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Non autorisé | AuthenticationTokenInvalid | Le jeton d’accès Azure AD n’est pas valide. Dans certains cas, les détails de l’erreur ServiceError contiennent plus d’informations, par exemple lorsque le jeton est arrivé à expiration ou que la revendication *appid* est manquante. |
| 401  | Non autorisé | InconsistentClientId       | La revendication *clientId* dans la clé d’ID du Windows Store et la revendication *appid* dans le jeton d’accès Azure AD ne correspondent pas.                                                                     |

<span/>

## <a name="related-topics"></a>Rubriques connexes


* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)

