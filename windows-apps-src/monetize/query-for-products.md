---
author: Xansky
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: Utilisez cette méthode dans l’API de collection du MicrosoftStore pour obtenir tous les produits possédés par un client pour les apps associées à votre ID client AzureAD. Vous pouvez limiter votre requête à un produit spécifique ou utiliser d’autres filtres.
title: Demander des produits
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
keywords: windows10, uwp, API de collection du MicrosoftStore, afficher produits
ms.localizationpriority: medium
ms.openlocfilehash: 3e5d6376420a1ee4ac166330371cf6e058d3476e
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7155926"
---
# <a name="query-for-products"></a>Demander des produits


Utilisez cette méthode dans l’API de collection du MicrosoftStore pour obtenir tous les produits possédés par un client pour les apps associées à votre ID client AzureAD. Vous pouvez limiter votre requête à un produit spécifique ou utiliser d’autres filtres.

Cette méthode est conçue pour être appelée par votre service en réponse à un message de votre application. Votre service ne doit pas interroger régulièrement tous les utilisateurs en fonction d’une planification.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez disposer des éléments suivants:

* Un jeton d’accès AzureAD créé avec la valeur d’URI d’audience `https://onestore.microsoft.com`.
* une clé d’ID du MicrosoftStore qui représente l’identité de l’utilisateur dont vous souhaitez obtenir les produits.

Pour plus d’informations, consultez l’article [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête

### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                 |
|--------|-------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/query``` |


### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;.                           |
| Host           | chaîne | Doit être défini sur la valeur **collections.mp.microsoft.com**.                                            |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |


### <a name="request-body"></a>Corps de la requête

| Paramètre         | Type         | Description         | Obligatoire |
|-------------------|--------------|---------------------|----------|
| beneficiaries     | UserIdentity | Objet UserIdentity qui représente l’utilisateur interrogé pour les produits. Pour plus d’informations, voir le tableau ci-dessous.    | Oui      |
| continuationToken | chaîne       | S’il existe plusieurs ensembles de produits, le corps de la réponse retourne un jeton de continuation lorsque la limite de la page est atteinte. Indiquez ce jeton de continuation ici dans les appels ultérieurs pour récupérer les produits restants.       | Non       |
| maxPageSize       | nombre       | Nombre maximal de produits à retourner dans une réponse. La valeur par défaut et maximale est de 100.                 | Non       |
| modifiedAfter     | DateHeure     | Si ce paramètre est spécifié, le service retourne uniquement les produits qui ont été modifiés après cette date.        | Non       |
| parentProductId   | chaîne       | Si ce paramètre est spécifié, le service retourne uniquement les extensions correspondant à l’application spécifiée.      | Non       |
| productSkuIds     | list&lt;ProductSkuId&gt; | Si ce paramètre est spécifié, le service retourne uniquement les produits applicables aux paires produit/référence fournies. Pour plus d’informations, voir le tableau ci-dessous.      | Non       |
| productTypes      | chaîne       | Si ce paramètre est spécifié, le service retourne uniquement les produits qui correspondent aux types de produit spécifiés. Types de produit pris en charge: **Application**, **Durable** et **UnmanagedConsumable**.     | Non       |
| validityType      | chaîne       | Si ce paramètre est défini sur **All**, tous les produits d’un utilisateur sont retournés, y compris les articles arrivés à expiration. S’il est défini sur **Valid**, seuls les produits qui sont valides à ce stade sont retournés (autrement dit, ils ont un état actif, une date de début &lt; maintenant et une date de fin &gt; maintenant). | Non       |


L’objet UserIdentity contient les paramètres ci-dessous.

| Paramètre            | Type   |  Description      | Obligatoire |
|----------------------|--------|----------------|----------|
| identityType         | chaîne | Spécifiez la valeur chaîne **b2b**.    | Oui      |
| identityValue        | chaîne | [Clé d’ID du MicrosoftStore](view-and-grant-products-from-a-service.md#step-4) représentant l’identité de l’utilisateur auquel vous souhaitez demander des produits.  | Oui      |
| localTicketReference | chaîne | Identificateur demandé pour les produits retournés. Les articles retournés dans le corps de la réponse ont un paramètre *localTicketReference* correspondant. Nous vous recommandons d’utiliser la même valeur que la revendication *userId* dans la clé d’ID du MicrosoftStore. | Oui      |


L’objet ProductSkuId contient les paramètres ci-dessous.

| Paramètre | Type   | Description          | Obligatoire |
|-----------|--------|----------------------|----------|
| productId | chaîne | L'[ID Store](in-app-purchases-and-trials.md#store-ids) pour un [produit](in-app-purchases-and-trials.md#products-skus-and-availabilities) dans le catalogue du MicrosoftStore. Exemple d’ID WindowsStore pour un produit: 9NBLGGH42CFD. | Oui      |
| skuID     | chaîne | [ID Store](in-app-purchases-and-trials.md#store-ids) pour la [référence](in-app-purchases-and-trials.md#products-skus-and-availabilities) d’un produit dans le catalogue du MicrosoftStore. Exemple d’ID WindowsStore pour une référence (SKU): 0010.       | Oui      |


### <a name="request-example"></a>Exemple de requête

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
  "maxPageSize": 100,
  "beneficiaries": [
    {
      "localTicketReference": "1055521810674918",
      "identityValue": "eyJ0eXAiOiJ……",
      "identityType": "b2b"
    }
  ],
  "modifiedAfter": "\/Date(-62135568000000)\/",
  "productSkuIds": [
    {
      "productId": "9NBLGGH5WVP6",
      "skuId": "0010"
    }
  ],
  "productTypes": [
    "UnmanagedConsumable"
  ],
  "validityType": "All"
}
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Paramètre         | Type                     | Description          | Obligatoire |
|-------------------|--------------------------|-----------------------|----------|
| continuationToken | chaîne                   | S’il existe plusieurs ensembles de produits, ce jeton est retourné lorsque la limite de la page est atteinte. Vous pouvez spécifier ce jeton de continuation dans les appels ultérieurs pour récupérer les produits restants. | Non       |
| éléments             | CollectionItemContractV6 | Tableau de produits de l’utilisateur spécifié. Pour plus d’informations, voir le tableau ci-dessous.        | Non       |


L’objet CollectionItemContractV6 contient les paramètres ci-dessous.

| Paramètre            | Type               | Description            | Obligatoire |
|----------------------|--------------------|-------------------------|----------|
| acquiredDate         | DateHeure           | Date à laquelle l’utilisateur a acquis l’article.                  | Oui      |
| campaignId           | chaîne             | ID campagne fourni au moment de l’achat de cet article.                  | Non       |
| devOfferId           | chaîne             | ID d’offre d’un achat dans l’application.              | Non       |
| endDate              | DateHeure           | Date de fin de l’article.              | Oui      |
| fulfillmentData      | chaîne             | Non applicable         | Non       |
| inAppOfferToken      | chaîne             | La chaîne d’ID de produit spécifié par le développeur qui est attribuée à l’élément dans l’espace partenaires. Un exemple d’ID produit est *product123*. | Non       |
| itemId               | chaîne             | ID qui identifie cet élément de collection à partir des autres articles dont l’utilisateur est propriétaire. Cet ID est unique par produit.   | Oui      |
| localTicketReference | chaîne             | L’ID du paramètre *localTicketReference* précédemment fourni dans le corps de la requête.                  | Oui      |
| modifiedDate         | DateHeure           | Date de la dernière modification de cet article.              | Oui      |
| orderId              | chaîne             | Le cas échéant, référence de la commande par le biais de laquelle cet article a été obtenu.              | Non       |
| orderLineItemId      | chaîne             | Le cas échéant, ligne d’article de la commande spécifique dans laquelle cet article a été obtenu.              | Non       |
| ownershipType        | chaîne             | La chaîne *OwnedByBeneficiary*.   | Oui      |
| productId            | chaîne             | [ID Store](in-app-purchases-and-trials.md#store-ids) pour le [produit](in-app-purchases-and-trials.md#products-skus-and-availabilities) dans le catalogue du MicrosoftStore. Exemple d’ID Store pour un produit: 9NBLGGH42CFD.          | Oui      |
| productType          | chaîne             | L’un des types de produit suivants: **Application**, **Durable** et **UnmanagedConsumable**.        | Oui      |
| purchasedCountry     | chaîne             | Non applicable   | Non       |
| purchaser            | IdentityContractV6 | Le cas échéant, représente l’identité de l’acheteur de l’article. Voir les détails de cet objet ci-dessous.        | Non       |
| quantity             | nombre             | Quantité de l’article. Actuellement, il s’agit toujours de la valeur 1.      | Non       |
| skuId                | chaîne             | [ID Store](in-app-purchases-and-trials.md#store-ids) pour la [référence (SKU)](in-app-purchases-and-trials.md#products-skus-and-availabilities) du produit dans le catalogue du MicrosoftStore. Exemple d’ID Store pour une référence: 0010.     | Oui      |
| skuType              | chaîne             | Type de référence. Valeurs possibles: **Trial**, **Full** et **Rental**.        | Oui      |
| startDate            | DateHeure           | Date de début de validité de l’article.       | Oui      |
| status               | chaîne             | État de l’article. Valeurs possibles: **Active**, **Expired**, **Revoked** et **Banned**.    | Oui      |
| tags                 | chaîne             | Non applicable    | Oui      |
| transactionId        | GUID               | ID de la transaction résultant de l’achat de cet article. Peut être utilisé pour signaler le traitement de la commande d’un article.      | Oui      |


L’objet IdentityContractV6 contient les paramètres ci-dessous.

| Paramètre     | Type   | Description                                                                        | Obligatoire |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | chaîne | Contient la valeur *pub*.                                                      | Oui      |
| identityValue | chaîne | Valeur chaîne du paramètre *publisherUserId* dans la clé d’ID du MicrosoftStore. | Oui      |


### <a name="response-example"></a>Exemple de réponse

```syntax
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
  "items" : [
    {
      "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
      "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
      "endDate" : "9999-12-31T23:59:59.9999999+00:00",
      "fulfillmentData" : [],
      "inAppOfferToken" : "consumable2",
      "itemId" : "4b8fbb13127a41f299270ea668681c1d",
      "localTicketReference" : "1055521810674918",
      "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
      "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
      "ownershipType" : "OwnedByBeneficiary",
      "productId" : "9NBLGGH5WVP6",
      "productType" : "UnmanagedConsumable",
      "purchaser" : {
        "identityType" : "pub",
        "identityValue" : "user123"
      },
      "skuId" : "0010",
      "skuType" : "Full",
      "startDate" : "2015-09-22T19:22:51.2068724+00:00",
      "status" : "Active",
      "tags" : [],
      "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
    }
  ]
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Octroyer des produits gratuits](grant-free-products.md)
* [Renouveler une clé d’ID du MicrosoftStore](renew-a-windows-store-id-key.md)
