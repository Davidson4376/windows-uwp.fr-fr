---
author: Xansky
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Utilisez cette méthode dans l’API de collection du MicrosoftStore pour indiquer le traitement de la commande d’un produit consommable pour un client donné. Pour qu’un utilisateur puisse racheter un produit consommable, votre application ou votre service doit indiquer que la commande de ce produit a été traitée pour cet utilisateur.
title: Signaler le traitement de la commande d’un produit consommable
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
keywords: windows10, uwp, API de collection du MicrosoftStore, traiter, produit consommable
ms.localizationpriority: medium
ms.openlocfilehash: 69795a40ae5e46f95330cd6a6d2738adc9a74686
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7424342"
---
# <a name="report-consumable-products-as-fulfilled"></a>Signaler le traitement de la commande d’un produit consommable

Utilisez cette méthode dans l’API de collection du MicrosoftStore pour indiquer le traitement de la commande d’un produit consommable pour un client donné. Pour qu’un utilisateur puisse racheter un produit consommable, votre application ou votre service doit indiquer que la commande de ce produit a été traitée pour cet utilisateur.

Vous pouvez utiliser cette méthode pour indiquer que la commande d’un produit consommable a été traitée de deux façons:

* Indiquez l’ID d’article du produit consommable (tel qu’il est retourné dans le paramètre **itemId** d’une [demande de produits](query-for-products.md)) et un ID de suivi unique que vous fournissez. Si le même ID de suivi est utilisé pour plusieurs tentatives, le même résultat est retourné, même si l’article est déjà consommé. Si vous ne savez pas si une demande de consommation a abouti, votre service doit de nouveau la soumettre avec le même ID de suivi. L’ID de suivi sera toujours lié à cette demande de consommation et peut être soumis indéfiniment.
* Indiquez l’ID produit (tel qu’il est retourné dans le paramètre **productId** d’une [demande de produits](query-for-products.md)) et un ID de transaction qui est obtenu à partir de l’une des sources indiquées dans la description du paramètre **transactionId** dans la section Corps de la requête ci-dessous.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez disposer des éléments suivants:

* Un jeton d’accès AzureAD créé avec la valeur d’URI d’audience `https://onestore.microsoft.com`.
* Une clé d’ID du MicrosoftStore, qui représente l’identité de l’utilisateur pour lequel vous souhaitez indiquer le traitement de la commande d’un produit consommable.

Pour plus d’informations, voir [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;.                           |
| Host           | chaîne | Doit être défini sur la valeur **collections.mp.microsoft.com**.                                            |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |


### <a name="request-body"></a>Corps de la requête

| Paramètre     | Type         | Description         | Obligatoire |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | L’utilisateur pour lequel cet élément est utilisé. Pour plus d’informations, voir le tableau suivant.        | Oui      |
| itemId        | chaîne       | La valeur *itemId* renvoyée par une [demande de produits](query-for-products.md). Utilisez ce paramètre avec *trackingId*      | Non       |
| trackingId    | guid         | ID de suivi unique fourni par le développeur. Utilisez ce paramètre avec *itemId*.         | Non       |
| productId     | chaîne       | la valeur *productId* renvoyée par une [demande de produits](query-for-products.md). Utilisez ce paramètre avec *transactionId*   | Non       |
| transactionId | guid         | Valeur d’ID de transaction qui est obtenue à partir de l’une des sources suivantes. Utilisez ce paramètre avec *productId*.<ul><li>Propriété [TransactionID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) de la classe [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392).</li><li>Accusé de réception de l’application ou du produit retourné par [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) ou [GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync).</li><li>Le paramètre *transactionId* renvoyé par une [demande de produits](query-for-products.md).</li></ul>   | Non       |


L’objet UserIdentity contient les paramètres ci-dessous.

| Paramètre            | Type   | Description       | Obligatoire |
|----------------------|--------|-------------------|----------|
| identityType         | chaîne | Spécifiez la valeur chaîne **b2b**.    | Oui      |
| identityValue        | chaîne | La [clé d’ID du MicrosoftStore](view-and-grant-products-from-a-service.md#step-4) qui représente l’identité de l’utilisateur pour lequel vous souhaitez indiquer le traitement de la commande d’un produit consommable.      | Oui      |
| localTicketReference | chaîne | Identificateur demandé pour la réponse retournée. Nous vous recommandons d’utiliser la même valeur que la [revendication](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key) *userId* dans la clé d’ID du MicrosoftStore. | Oui      |


### <a name="request-examples"></a>Exemples de demande

L’exemple suivant utilise les paramètres *itemId* et *trackingId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

L’exemple suivant utilise les paramètres *productId* et *transactionId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>Réponse

Aucun contenu n’est retourné si l’utilisation a été exécutée correctement.

### <a name="response-example"></a>Exemple de réponse

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>Codes d’erreur


| Code | Erreur        | Code d’erreur interne           | Description           |
|------|--------------|----------------------------|-----------------------|
| 401  | Non autorisé | AuthenticationTokenInvalid | Le jeton d’accès Azure AD n’est pas valide. Dans certains cas, les détails de l’erreur ServiceError contiennent plus d’informations, par exemple lorsque le jeton est arrivé à expiration ou que la revendication *appid* est manquante. |
| 401  | Non autorisé | PartnerAadTicketRequired   | Un jeton d’accès Azure AD n’a pas été transmis au service dans l’en-tête d’autorisation.                                                                                                   |
| 401  | Non autorisé | InconsistentClientId       | La revendication *clientId* dans la clé d’ID du MicrosoftStore du corps de la requête et la revendication *applicationId* du jeton d’accès AzureAD de l’en-tête d’autorisation ne correspondent pas.                     |

<span/> 

## <a name="related-topics"></a>Rubriques connexes

* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Demander des produits](query-for-products.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Renouveler une clé d’ID du MicrosoftStore](renew-a-windows-store-id-key.md)
