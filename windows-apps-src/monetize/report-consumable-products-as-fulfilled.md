---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Utilisez cette méthode dans l’API de collection du Windows Store pour indiquer le traitement de la commande d’un produit consommable pour un client donné. Pour qu’un utilisateur puisse racheter un produit consommable, votre application ou votre service doit indiquer que la commande de ce produit a été traitée pour cet utilisateur.
title: Signaler le traitement de la commande d’un produit consommable
---

# Signaler le traitement de la commande d’un produit consommable


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Utilisez cette méthode dans l’API de collection du Windows Store pour indiquer le traitement de la commande d’un produit consommable pour un client donné. Pour qu’un utilisateur puisse racheter un produit consommable, votre application ou votre service doit indiquer que la commande de ce produit a été traitée pour cet utilisateur.

Vous pouvez utiliser cette méthode pour indiquer que la commande d’un produit consommable a été traitée de deux façons :

-   Indiquez l’ID d’article du produit consommable (tel qu’il est retourné dans le paramètre **itemId** d’une [demande de produits](query-for-products.md)) et un ID de suivi unique que vous fournissez. Si le même ID de suivi est utilisé pour plusieurs tentatives, le même résultat est retourné, même si l’article est déjà consommé. Si vous ne savez pas si une demande de consommation a abouti, votre service doit de nouveau la soumettre avec le même ID de suivi. L’ID de suivi sera toujours lié à cette demande de consommation et peut être soumis indéfiniment.
-   Indiquez l’ID produit (tel qu’il est retourné dans le paramètre **productId** d’une [demande de produits](query-for-products.md)) et un ID de transaction qui est obtenu à partir de l’une des sources indiquées dans la description du paramètre **transactionId** dans la section Corps de la requête ci-dessous.

## Prérequis


Pour utiliser cette méthode, vous devez disposer des éléments suivants :

-   un jeton d’accès Azure AD créé avec l’URI d’audience **https://onestore.microsoft.com** ;
-   une clé d’ID du Windows Store générée en appelant la méthode [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) à partir du code côté client de votre application.

Pour plus d’informations, voir [Afficher et octroyer des produits à partir d’un service](view-and-grant-products-from-a-service.md).

## Requête


### Syntaxe de la requête

| Méthode | URI de la requête                                                   |
|--------|---------------------------------------------------------------|
| POST   | https://collections.mp.microsoft.com/v6.0/collections/consume |

 

### En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*token*&gt;.                           |
| Host           | chaîne | Doit être défini sur la valeur **collections.mp.microsoft.com**.                                            |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |

 

### Corps de la requête

| Paramètre     | Type         | Description         | Obligatoire |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | L’utilisateur pour lequel cet élément est utilisé.                                                                                                                                                                                                                                                                 | Oui      |
| itemId        | Chaîne       | Valeur itemId retournée par une [demande de produits](query-for-products.md). Utilisez ce paramètre avec trackingId.                                                                                                                                                                                                  | Non       |
| trackingId    | GUID         | ID de suivi unique fourni par le développeur. Utilisez ce paramètre avec itemId.                                                                                                                                                                                                                                     | Non       |
| productId     | Chaîne       | Valeur productId retournée par une [demande de produits](query-for-products.md). Utilisez ce paramètre avec transactionId.                                                                                                                                                                                            | Non       |
| transactionId | GUID         | Valeur d’ID de transaction qui est obtenue à partir de l’une des sources suivantes :                                                                                                                                                                                                                                      | Non       | 
|               |              | * Propriété [TransactionID](https://msdn.microsoft.com/library/windows/apps/dn263396) de la classe [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392).   |        | 
|               |              | * Accusé de réception de l’application ou du produit retourné par [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381), [RequestAppPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/hh967813) ou [GetAppReceiptAsync](https://msdn.microsoft.com/library/windows/apps/hh967811).   |        |
|               |              | * Paramètre transactionId retourné par une [demande de produits](query-for-products.md).   |        |        
|               |              | Utilisez ce paramètre avec productId.   |        |
 

L’objet UserIdentity contient les paramètres ci-dessous.

| Paramètre            | Type   | Description                                                                                                                                 | Obligatoire |
|----------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | chaîne | Spécifiez la valeur chaîne **b2b**.                                                                                                           | Oui      |
| identityValue        | chaîne | Valeur chaîne de la clé d’ID du Windows Store.                                                                                                   | Oui      |
| localTicketReference | chaîne | Identificateur demandé pour la réponse retournée. Nous vous recommandons d’utiliser la même valeur que la revendication *userId* dans la clé d’ID du Windows Store. | Oui      |

 

### Exemples de demande

L’exemple suivant utilise les paramètres *itemId* et *trackingId*.

```
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

```
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

## Réponse


Aucun contenu n’est retourné si l’utilisation a été exécutée correctement.

### Exemple de réponse

```
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## Codes d’erreur


| Code | Erreur        | Code d’erreur interne           | Description                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Non autorisé | AuthenticationTokenInvalid | Le jeton d’accès Azure AD n’est pas valide. Dans certains cas, les détails de l’erreur ServiceError contiennent plus d’informations, par exemple lorsque le jeton est arrivé à expiration ou que la revendication *appid* est manquante. |
| 401  | Non autorisé | PartnerAadTicketRequired   | Un jeton d’accès Azure AD n’a pas été transmis au service dans l’en-tête d’autorisation.                                                                                                   |
| 401  | Non autorisé | InconsistentClientId       | La revendication *clientId* dans la clé d’ID du Windows Store du corps de la requête et la revendication *appid* du jeton d’accès Azure AD de l’en-tête d’autorisation ne correspondent pas.                     |

 

## Rubriques connexes

* [Afficher et octroyer des produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Demander des produits](query-for-products.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Renouveler une clé d’ID du Windows Store](renew-a-windows-store-id-key.md)
 

 





<!--HONumber=Mar16_HO1-->


