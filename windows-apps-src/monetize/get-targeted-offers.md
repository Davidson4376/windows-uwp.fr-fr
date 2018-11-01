---
author: Xansky
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: Utilisez cette méthode dans l’API d’offres ciblées du MicrosoftStore afin d’obtenir les offres ciblées, disponibles pour l’utilisateur actuel dans le cadre de l’app actuelle.
title: Obtenir des offres ciblées
ms.author: mhopkins
ms.date: 10/10/2017
ms.topic: article
keywords: windows10, uwp, services du MicrosoftStore, API des offres ciblées du Store, obtenir des offres ciblées
ms.localizationpriority: medium
ms.openlocfilehash: e6a0e9237c7c803a64ec20df0c501773f690f5e9
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5873085"
---
# <a name="get-targeted-offers"></a>Obtenir des offres ciblées

Suivez cette méthode afin d’obtenir les offres ciblées disponibles pour l’utilisateur actuel, basée sur le fait que l’utilisateur fasse partie ou non du segment de clientèle pour l’offre ciblée. Pour plus d’informations, consultez [Gérer les offres ciblées à l’aide des services du Windows Store](manage-targeted-offers-using-windows-store-services.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez tout d’abord [obtenir un jeton de compte Microsoft](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token) pour l’utilisateur actuel connecté de votre application. Vous devez transmettre ce jeton dans l’en-tête de requête ```Authorization``` de cette méthode. Ce jeton est utilisé par le MicrosoftStore pour obtenir des offres ciblées pour l’utilisateur actuel.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description  |
|---------------|--------|--------------|
| Authorization | chaîne | Requis. Le jeton de compte Microsoft pour l’actuel utilisateur connecté de votre application sous la forme d’un **jeton**&lt;*de support*&gt;. |


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
| offers      | array  | Un tableau des ID produit des extensions qui sont associées aux offres ciblées, disponibles pour l’utilisateur actuel. Ces ID de produit sont spécifiés sur la page **Offres ciblées** de votre app, dans le tableau de bord du Centre de développement Windows.            |
| trackingId  | chaîne | Un GUID que vous pouvez éventuellement utiliser pour assurer le suivi de l’offre ciblée dans votre propre code ou vos services. |


### <a name="example"></a>Exemple

L’exemple suivant représente un exemple de corps de réponse JSON pour cette requête.

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

## <a name="related-topics"></a>Rubriquesassociées

* [Gérer les offres ciblées à l’aide des services du Store](manage-targeted-offers-using-windows-store-services.md)

 

 
