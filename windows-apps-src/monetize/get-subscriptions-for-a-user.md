---
author: Xansky
ms.assetid: 94B5B2E9-BAEE-4B7F-BAF1-DA4D491427D7
description: Utilisez cette méthode dans l’API d’achat du MicrosoftStore pour obtenir les abonnements qu’un utilisateur donné est autorisé à utiliser.
title: Recueillir les abonnements d’un utilisateur
ms.author: mhopkins
ms.date: 07/10/2018
ms.topic: article
keywords: windows10, uwp, API d’achat du MicrosoftStore, abonnements
ms.localizationpriority: medium
ms.openlocfilehash: b8fe6262ca6ef52ca94ade2c56b18f5e71951f07
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7303419"
---
# <a name="get-subscriptions-for-a-user"></a>Recueillir les abonnements d’un utilisateur

Utilisez cette méthode dans l’API d’achat du MicrosoftStore pour obtenir les extensions d’abonnement qu’un utilisateur donné est autorisé à utiliser.

> [!NOTE]
> Cette méthode peut uniquement être utilisée par les comptes de développeur qui ont été configurés par Microsoft pour être en mesure de créer des extensions d’abonnement pour les applications Universal Windows Platform (UWP). Les extensions d’abonnement ne sont actuellement pas disponibles pour la plupart des comptes de développeur.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez disposer des éléments suivants:

* Un jeton d’accès AzureAD créé avec la valeur d’URI d’audience `https://onestore.microsoft.com`.
* Une clé d’ID du MicrosoftStore qui représente l’identité de l’utilisateur duquel vous souhaitez obtenir les abonnements.

Pour plus d’informations, consultez [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query``` |


### <a name="request-header"></a>En-tête de requête

| En-tête         | Type   | Description      |
|----------------|--------|-------------------|
| Authorization  | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;.                           |
| Host           | chaîne | Doit être défini sur la valeur **purchase.mp.microsoft.com**.                                            |
| Content-Length | nombre | Longueur du corps de la requête.                                                                       |
| Content-Type   | chaîne | Spécifie le type de requête et de réponse. Actuellement, la seule valeur prise en charge est **application/json**. |


### <a name="request-body"></a>Corps de la requête

| Paramètre      | Type   | Description     | Requis |
|----------------|--------|-----------------|----------|
| b2bKey         | chaîne | La [clé d’ID du MicrosoftStore](view-and-grant-products-from-a-service.md#step-4) qui représente l’identité de l’utilisateur duquel vous souhaitez obtenir les abonnements.  | Oui      |
| continuationToken |  chaîne     |  Si l’utilisateur dispose des droits à plusieurs abonnements, le corps de réponse renvoie un jeton de continuation lorsque la limite de la page est atteinte. Indiquez ce jeton de continuation ici dans les appels ultérieurs pour récupérer les produits restants.    | Non      |
| pageSize       | chaîne | Nombre maximal d’abonnements à renvoyer dans une réponse. La valeur par défaut est 25.     |  Non      |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment utiliser cette méthode pour obtenir les extensions d’abonnement qu’un utilisateur donné a le droit d’utiliser. Remplacez la valeur *b2bKey* par la [clé d’ID du MicrosoftStore](view-and-grant-products-from-a-service.md#step-4) qui représente l’identité de l’utilisateur duquel vous souhaitez modifier l’abonnement.

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/query HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ..."
}
```


## <a name="response"></a>Réponse

Cette méthode renvoie un corps de réponse JSON qui contient une collection d’objets de données décrivant les extensions d’abonnement qui peuvent être utilisées par l’utilisateur. L’exemple suivant présente le corps de réponse pour un utilisateur qui possède des droits pour un abonnement.

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-11T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-08T21:07:51.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>Corps de réponse

Le corps de réponse contient les données suivantes.

| Valeur        | Type   | Description            |
|---------------|--------|---------------------|
| éléments | tableau | Un tableau d’objets qui contiennent des données sur chaque extension d’abonnement, pouvant être utilisé par un utilisateur spécifié. Pour plus d’informations sur les données incluses dans chaque objet, voir le tableau suivant.  |


Chaque objet figurant dans le tableau *éléments* contient les valeurs suivantes.

| Valeur        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | Valeur booléenne |  Indique si l’abonnement est configuré pour se renouveler automatiquement à la fin de la période d’abonnement en cours.   |
| bénéficiaire | chaîne |  L’ID du bénéficiaire du droit associé à cet abonnement.   |
| expirationTime | chaîne | La date et l’heure auxquelles l’abonnement expire, au format ISO8601. Ce champ est uniquement disponible lorsque l’abonnement est dans certains états. Généralement, le délai d’expiration indique le moment où l’état actuel arrive à expiration. Par exemple, pour un abonnement actif, la date d’expiration indique quand le renouvellement automatique suivant a lieu.    |
| expirationTimeWithGrace | chaîne | Date et heure de qu'expiration de l’abonnement, y compris la période de grâce au format ISO 8601. Cette valeur indique quand l’utilisateur perd l’accès à l’abonnement une fois que l’abonnement n’a pas pu renouveler automatiquement.    |
| id | chaîne |  L’ID de l’abonnement. Utilisez cette valeur afin d’indiquer l’abonnement que vous souhaitez modifier lorsque vous appelez la méthode [modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md).    |
| isTrial | Valeur booléenne |  Indique si l’abonnement est une version d’évaluation.     |
| lastModified | chaîne |  La date et l’heure auxquelles l’abonnement a été modifié pour la dernière fois, au format ISO8601.      |
| marché | chaîne | Le code pays (code de deux lettres au format ISO3166-1 alpha-2) dans lequel l’utilisateur a obtenu l’abonnement.      |
| productId | chaîne |  [L’ID Store](in-app-purchases-and-trials.md#store-ids) pour le [produit](in-app-purchases-and-trials.md#products-skus-and-availabilities) qui représente l’extension d’abonnement dans le catalogue MicrosoftStore. Exemple d’ID Store pour un produit: 9NBLGGH42CFD.     |
| skuId | chaîne |  [L’ID Store](in-app-purchases-and-trials.md#store-ids) pour la [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) qui représente l’extension d’abonnement dans le catalogue MicrosoftStore. Exemple d’ID Store pour une référence: 0010.    |
| startTime | chaîne |  L’heure et la date de début de l’abonnement, au format ISO8601.     |
| recurrenceState | chaîne  |  Une des valeurs suivantes:<ul><li>**Aucun**:&nbsp;&nbsp;cela indique un abonnement à durée indéterminée.</li><li>**Actif**:&nbsp;&nbsp;l’abonnement est actif et l’utilisateur est autorisé à utiliser les services.</li><li>**Inactif**:&nbsp;&nbsp;l’abonnement est expiré et l’utilisateur a désactivé l’option de renouvellement automatique de l’abonnement.</li><li>**Annulé**:&nbsp;&nbsp;l’abonnement a été délibérément arrêté avant la date d’expiration, avec ou sans remboursement.</li><li>**InDunning**:&nbsp;&nbsp;l’abonnement est en *relance* (autrement dit, l’abonnement est arrivé à expiration et Microsoft essaie d’acquérir des fonds pour renouveler automatiquement l’abonnement).</li><li>**Échec**:&nbsp;&nbsp;la période de relance est terminée et malgré plusieurs essais, l’abonnement n’a pu être renouvelé.</li></ul><p>**Remarque:**</p><ul><li>**Inactif**/**Annulé**/**Échec** sont des états terminaux. Lorsqu’un abonnement acquiert l’un de ces états, l’utilisateur doit obligatoirement procéder au rachat de l’abonnement pour le réactiver. L’utilisateur n’est pas autorisé à utiliser les services dans ces états.</li><li>Lorsqu’un abonnement est **Annulé**, la valeur expirationTime sera mise à jour avec la date et l’heure de l’annulation.</li><li>L’ID de l’abonnement reste le même pendant toute sa durée de vie. Il ne changera pas si l’option renouvellement automatique est activée ou désactivée. Si un utilisateur rachète un abonnement après avoir atteint un état de terminal, un nouvel ID d’abonnement sera généré.</li><li>L’ID d’un abonnement doit être utilisé pour exécuter n’importe quelle opération sur un abonnement individuel.</li><li>Dans le cas où un utilisateur rachète un abonnement après une annulation ou une interruption, si vous interrogez les résultats de l’utilisateur, vous allez obtenir deux entrées: une entrée avec l’ancien ID d’abonnement en état terminal et une entrée avec le nouvel ID d’abonnement en état actif.</li><li>Il est toujours bon de vérifier à la fois les valeurs recurrenceState et expirationTime, dans la mesure où les mises à jour vers la valeur recurrenceState peuvent être potentiellement retardées de quelques minutes (parfois même quelques heures).       |
| cancellationDate | chaîne   |  La date et l’heure auxquelles l’abonnement de l’utilisateur a été annulé, au format ISO8601.     |


## <a name="related-topics"></a>Rubriques associées

* [Gérer les droits sur les produits à partir d’un service](view-and-grant-products-from-a-service.md)
* [Modifier l’état de facturation de l’abonnement d’un utilisateur](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Demander des produits](query-for-products.md)
* [Signaler le traitement de la commande d’un produit consommable](report-consumable-products-as-fulfilled.md)
* [Renouveler une clé d’ID du MicrosoftStore](renew-a-windows-store-id-key.md)
