---
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: Utilisez cette méthode dans l’API d’offres ciblées du Microsoft Store afin d’obtenir les offres ciblées, disponibles pour l’utilisateur actuel dans le cadre de l’app actuelle.
title: Obtenir des offres ciblées
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, services du Microsoft Store, API des offres ciblées du Store, obtenir des offres ciblées
ms.localizationpriority: medium
ms.openlocfilehash: 71cd6ce3b9736b812f8ccdf4d21d35357928c63c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622764"
---
# <a name="get-targeted-offers"></a>Obtenir des offres ciblées

Suivez cette méthode afin d’obtenir les offres ciblées disponibles pour l’utilisateur actuel, basée sur le fait que l’utilisateur fasse partie ou non du segment de clientèle pour l’offre ciblée. Pour plus d’informations, consultez [Gérer les offres ciblées à l’aide des services du Windows Store](manage-targeted-offers-using-windows-store-services.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez tout d’abord [obtenir un jeton de compte Microsoft](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token) pour l’utilisateur actuel connecté de votre application. Vous devez transmettre ce jeton dans l’en-tête de requête ```Authorization``` de cette méthode. Ce jeton est utilisé par le Microsoft Store pour obtenir des offres ciblées pour l’utilisateur actuel.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description  |
|---------------|--------|--------------|
| Authorization | chaîne | Obligatoire. Le jeton Account Microsoft pour l’utilisateur connecté actuel de votre application sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

Cette méthode ne possède aucun paramètre de requête ou d’URI.

### <a name="request-example"></a>Exemple de requête

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>Réponse

Cette méthode retourne un corps de réponse au format JSON qui contient un tableau d’objets avec les champs suivants. Chaque objet dans le tableau représente les offres ciblées qui sont disponibles pour l’utilisateur spécifié, dans le cadre d’un segment de clients particulier.

| Champ      | Type   | Description         |
|------------|--------|------------------|
| offers      | tableau  | Un tableau des ID produit des extensions qui sont associées aux offres ciblées, disponibles pour l’utilisateur actuel. Ces ID de produit sont spécifiés dans le **ciblés offres** page de votre application dans le centre de partenaires.            |
| trackingId  | chaîne | Un GUID que vous pouvez éventuellement utiliser pour assurer le suivi de l’offre ciblée dans votre propre code ou vos services. |


### <a name="example"></a>Exemple

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
[
  {
    "offers": [
      "10x gold coins",
      "100x gold coins"
    ],
    "trackingId": "5de5dd29-6dce-4e68-b45e-d8ee6c2cd203"
  }
]
```

## <a name="related-topics"></a>Rubriques connexes

* [Gestion des offres ciblées à l’aide des services de Store](manage-targeted-offers-using-windows-store-services.md)

 

 
