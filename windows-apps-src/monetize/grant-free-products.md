---
author: mcleanbyron
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: "Utilisez cette méthode dans l’API d’achat du Windows Store pour octroyer une application ou extension gratuite à un utilisateur donné."
title: Octroyer des produits gratuits
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API d’achat du Windows Store, octroyer des produits"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f7509be016f32282478d6ebf0e373b2540537d3d
ms.lasthandoff: 02/07/2017

---

# <a name="grant-free-products"></a>Octroyer des produits gratuits

Utilisez cette méthode dans l’API d’achat du Windows Store pour octroyer une application ou extension gratuite à un utilisateur donné.

Actuellement, vous ne pouvez octroyer que des produits gratuits. Si votre service tente d’utiliser cette méthode pour octroyer un produit qui n’est pas gratuit, cette méthode retourne une erreur.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez disposer des éléments suivants :

* un jeton d’accès Azure AD créé avec l’URI d’audience `https://onestore.microsoft.com` ;
* une clé d’ID du Windows Store représentant l’identité de l’utilisateur auquel vous souhaitez octroyer un produit gratuit.

Pour plus d’informations, consultez l’article [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v6.0/purchases/grant``` |

<span/> 

### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;.                           |
| Host           | chaîne | Doit être défini sur la valeur **purchase.mp.microsoft.com**.                                            |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |

<span/>

### <a name="request-body"></a>Corps de la requête

| Paramètre      | Type   | Description        | Obligatoire |
|----------------|--------|---------------------|----------|
| availabilityId | chaîne | ID de disponibilité du produit à octroyer dans le catalogue du Windows Store.         | Oui      |
| b2bKey         | chaîne | [Clé d’ID du Windows Store](view-and-grant-products-from-a-service.md#step-4) représentant l’identité de l’utilisateur auquel vous souhaitez octroyer un produit.    | Oui      |
| devOfferId     | chaîne | ID d’offre spécifié par le développeur qui s’affiche dans l’élément de collection après l’achat.        |
| language       | chaîne | Langue de l’utilisateur.  | Oui      |
| market         | chaîne | Marché de l’utilisateur.       | Oui      |
| orderId        | GUID   | GUID généré pour la commande. Cette valeur doit être propre à l’utilisateur, mais il n’est pas impératif qu’elle soit unique dans toutes les commandes.    | Oui      |
| productId      | chaîne | [ID Windows Store](in-app-purchases-and-trials.md#store-ids) pour le [produit](in-app-purchases-and-trials.md#products-skus-and-availabilities) dans le catalogue du Windows Store. Exemple d’ID Windows Store pour un produit : 9NBLGGH42CFD. | Oui      |
| quantity       | int    | Quantité à acheter. Actuellement, la seule valeur prise en charge est 1. Si aucune valeur n’est spécifiée, la valeur par défaut est 1.   | Non       |
| skuId          | chaîne | [ID Windows Store](in-app-purchases-and-trials.md#store-ids) pour la [référence (SKU)](in-app-purchases-and-trials.md#products-skus-and-availabilities) du produit dans le catalogue du Windows Store. Exemple d’ID Windows Store pour une référence (SKU) : 0010.     | Oui      |

<span/>

### <a name="request-example"></a>Exemple de requête

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Paramètre                 | Type                        | Description             | Obligatoire |
|---------------------------|-----------------------------|-----------------------|----------|
| clientContext             | ClientContextV6             | Informations contextuelles client de cette commande. Ce paramètre est affecté à la valeur *clientID* du jeton Azure AD.    | Oui      |
| createdtime               | datetimeoffset              | Heure de création de la commande.         | Oui      |
| currencyCode              | chaîne                      | Code devise pour *totalAmount* et *totalTaxAmount*. Non applicable pour les articles gratuits.     | Oui      |
| friendlyName              | chaîne                      | Nom convivial de la commande. Non applicable pour les commandes passées à l’aide de l’API d’achat du Windows Store. | Oui      |
| isPIRequired              | booléen                     | Indique si un instrument de paiement (PI) est nécessaire dans le cadre de la commande d’achat.  | Oui      |
| language                  | chaîne                      | ID de langue de la commande (par exemple, « fr »).       | Oui      |
| market                    | chaîne                      | ID de marché de la commande (par exemple, « FR »).  | Oui      |
| orderId                   | chaîne                      | ID qui identifie la commande d’un utilisateur particulier.                | Oui      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | Liste des articles de la commande. En règle générale, il y a un article par commande.       | Oui      |
| orderState                | chaîne                      | État de la commande. Les états valides sont **Editing**, **CheckingOut**, **Pending**, **Purchased**, **Refunded**, **ChargedBack** et **Cancelled**. | Oui      |
| orderValidityEndTime      | chaîne                      | Dernière fois que la tarification de la commande a été valide avant sa soumission. Non applicable pour les applications gratuites.      | Oui      |
| orderValidityStartTime    | chaîne                      | Première fois que la tarification de la commande a été valide avant sa soumission. Non applicable pour les applications gratuites.          | Oui      |
| purchaser                 | IdentityV6                  | Objet qui décrit l’identité de l’acheteur.       | Oui      |
| totalAmount               | décimal                     | Montant total d’achat TTC de tous les articles de la commande.       | Oui      |
| totalAmountBeforeTax      | décimal                     | Montant total d’achat HT de tous les articles de la commande.      | Oui      |
| totalChargedToCsvTopOffPI | décimal                     | Si vous utilisez un instrument de paiement (PI) et une valeur de stockage (CSV) distincts, le montant est facturé au format CSV.            | Oui      |
| totalTaxAmount            | décimal                     | Montant total des taxes de tous les articles.    | Oui      |

<span/>

L’objet ClientContext contient les paramètres ci-dessous.

| Paramètre | Type   | Description                           | Obligatoire |
|-----------|--------|---------------------------------------|----------|
| client    | chaîne | ID client qui a créé la commande. | Non       |

<span/>

L’objet OrderLineItemV6 contient les paramètres ci-dessous.

| Paramètre               | Type           | Description                                                                                                  | Obligatoire |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| agent                   | IdentityV6     | Dernier agent à avoir modifié l’article. Pour plus d’informations sur cet objet, voir le tableau ci-dessous.       | Non       |
| availabilityId          | chaîne         | ID de disponibilité du produit à acheter dans le catalogue du Windows Store.                           | Oui      |
| beneficiary             | IdentityV6     | Identité du bénéficiaire de la commande.                                                                | Non       |
| billingState            | chaîne         | État de facturation de la commande. Défini sur **Charged** lorsque la commande est terminée.                                   | Non       |
| campaignId              | chaîne         | ID campagne de cette commande.                                                                              | Non       |
| currencyCode            | chaîne         | Code de devise utilisé pour les détails de prix.                                                                    | Oui      |
| description             | chaîne         | Description localisée de l’article.                                                                    | Oui      |
| devofferId              | chaîne         | ID d’offre de la commande particulière, le cas échéant.                                                           | Non       |
| fulfillmentDate         | datetimeoffset | Date du traitement de la commande.                                                                           | Non       |
| fulfillmentState        | chaîne         | État du traitement de la commande de cet article. Défini sur **Fulfilled** lorsque le traitement est effectué.                      | Non       |
| isPIRequired            | booléen        | Indique si un instrument de paiement est requis pour cet article.                                       | Oui      |
| isTaxIncluded           | booléen        | Indique si les taxes sont comprises dans les détails de la tarification de l’article.                                        | Oui      |
| legacyBillingOrderId    | chaîne         | ID de facturation hérité.                                                                                       | Non       |
| lineItemId              | chaîne         | ID de l’article de cette commande.                                                                 | Oui      |
| listPrice               | décimal        | Prix catalogue de l’article de cette commande.                                                                    | Oui      |
| productId               | chaîne         | [ID Windows Store](in-app-purchases-and-trials.md#store-ids) pour le [produit](in-app-purchases-and-trials.md#products-skus-and-availabilities) qui représente l’article dans le catalogue du Windows Store. Exemple d’ID Windows Store pour un produit : 9NBLGGH42CFD.   | Oui      |
| productType             | chaîne         | Type du produit. Les valeurs prises en charge sont **Durable**, **Application** et **UnmanagedConsumable**. | Oui      |
| quantity                | entier            | Quantité de l’article commandé.                                                                            | Oui      |
| retailPrice             | décimal        | Prix de vente au détail de l’article commandé.                                                                        | Oui      |
| revenueRecognitionState | chaîne         | État de prise en compte de revenu.                                                                               | Oui      |
| skuId                   | chaîne         | [ID Windows Store](in-app-purchases-and-trials.md#store-ids) pour la [référence (SKU)](in-app-purchases-and-trials.md#products-skus-and-availabilities) de l’article dans le catalogue du Windows Store. Exemple d’ID Windows Store pour une référence (SKU) : 0010.                                                                   | Oui      |
| taxAmount               | décimal        | Montant des taxes de l’article.                                                                            | Oui      |
| taxType                 | chaîne         | Type de taxe pour les taxes applicables.                                                                       | Oui      |
| Title                   | chaîne         | Titre localisé de l’article.                                                                        | Oui      |
| totalAmount             | décimal        | Montant total TTC d’achat de l’article.                                                    | Oui      |

<span/>

L’objet IdentityV6 contient les paramètres ci-dessous.

| Paramètre     | Type   | Description                                                                        | Obligatoire |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | chaîne | Contient la valeur **"pub"**.                                                      | Oui      |
| identityValue | chaîne | Valeur chaîne du paramètre *publisherUserId* dans la clé d’ID du Windows Store. | Oui      |

<span/> 

### <a name="response-example"></a>Exemple de réponse

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## <a name="error-codes"></a>Codes d’erreur


| Code | Erreur        | Code d’erreur interne           | Description   |
|------|--------------|----------------------------|----------------|
| 401  | Non autorisé | AuthenticationTokenInvalid | Le jeton d’accès Azure AD n’est pas valide. Dans certains cas, les détails de l’erreur ServiceError contiennent plus d’informations, par exemple lorsque le jeton est arrivé à expiration ou que la revendication *appid* est manquante. |
| 401  | Non autorisé | PartnerAadTicketRequired   | Un jeton d’accès Azure AD n’a pas été transmis au service dans l’en-tête d’autorisation.   |
| 401  | Non autorisé | InconsistentClientId       | La revendication *clientId* dans la clé d’ID du Windows Store du corps de la demande et la revendication *appid* du jeton d’accès Azure AD de l’en-tête d’autorisation ne correspondent pas.       |
| 400  | BadRequest   | InvalidParameter           | Les détails contiennent des informations relatives au corps de la requête et aux champs comprenant une valeur non valide.           |

<span/> 

## <a name="related-topics"></a>Articles connexes


* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Renouveler une clé d’ID du Windows Store](renew-a-windows-store-id-key.md)

