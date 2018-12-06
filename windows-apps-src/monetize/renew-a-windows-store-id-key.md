---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: Utilisez cette méthode pour renouveler une clé du MicrosoftStore.
title: Renouveler une clé d’ID du MicrosoftStore
ms.date: 03/16/2018
ms.topic: article
keywords: windows10, uwp, API de collection du MicrosoftStore, API d’achat du MicrosoftStore, clé d’ID du MicrosoftStore, renouveler
ms.localizationpriority: medium
ms.openlocfilehash: 0f1c6248b2d87a68b77cad6f1bdc7cce0fae587e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8746733"
---
# <a name="renew-a-microsoft-store-id-key"></a>Renouveler une clé d’ID du MicrosoftStore


Utilisez cette méthode pour renouveler une clé du MicrosoftStore. Lorsque vous [générez une clé d’ID du MicrosoftStore](view-and-grant-products-from-a-service.md#step-4), la clé est valide pendant 90jours. Après l’expiration de la clé, vous pouvez utiliser la clé arrivée à expiration pour en renégocier une nouvelle à l’aide de cette méthode.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez disposer des éléments suivants:

* Un jeton d’accès AzureAD créé avec la valeur d’URI d’audience `https://onestore.microsoft.com`.
* une clé d’ID du MicrosoftStore expirée qui a été [générée à partir du code côté client de votre app](view-and-grant-products-from-a-service.md#step-4).

Pour plus d’informations, voir [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Type de clé    | Méthode | URI de la requête                                              |
|-------------|--------|----------------------------------------------------------|
| Collections | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Achat    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | chaîne | Doit être défini sur la valeur **collections.mp.microsoft.com** ou **purchase.mp.microsoft.com**.           |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |


### <a name="request-body"></a>Corps de la requête

| Paramètre     | Type   | Description                       | Obligatoire |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | chaîne | Jeton d’accès Azure AD.        | Oui      |
| key           | chaîne | Clé d’ID du MicrosoftStore arrivée à expiration. | Oui       |


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

| Paramètre | Type   | Description                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| key       | chaîne | Clé du MicrosoftStore actualisée qui peut être utilisée dans les futurs appels de l’API de collection ou de l’API d’achat du MicrosoftStore. |


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


| Code | Erreur        | Code d’erreur interne           | Description   |
|------|--------------|----------------------------|---------------|
| 401  | Non autorisé | AuthenticationTokenInvalid | Le jeton d’accès Azure AD n’est pas valide. Dans certains cas, les détails de l’erreur ServiceError contiennent plus d’informations, par exemple lorsque le jeton est arrivé à expiration ou que la revendication *appid* est manquante. |
| 401  | Non autorisé | InconsistentClientId       | La revendication *clientId* dans la clé d’ID du MicrosoftStore et la revendication *appid* dans le jeton d’accès Azure AD ne correspondent pas.                                                                     |


## <a name="related-topics"></a>Rubriques connexes


* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
