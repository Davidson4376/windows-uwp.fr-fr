---
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour supprimer un vol de package pour une application qui est inscrit pour votre compte espace partenaires.
title: Supprimer une version d’évaluation de package
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, supprimer une version d’évaluation
ms.localizationpriority: medium
ms.openlocfilehash: a3455973d86b0c9e7c779cca429e36fa32266ed6
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335107"
---
# <a name="delete-a-package-flight"></a>Supprimer une version d’évaluation de package

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour supprimer un vol de package pour une application qui est inscrit pour votre compte espace partenaires.


## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| Suppression    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}` |


### <a name="request-header"></a>En-tête de requête

| Header        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la version d’évaluation du package à supprimer. L’ID de Store pour l’application est disponible dans le centre de partenaires.  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package à supprimer. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md). Pour un vol a été créé dans le centre de partenaires, cet ID est également disponible dans l’URL de la page de vol de partenaires.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment supprimer une version d’évaluation du package.

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

En cas de succès, cette méthode retourne un corps de réponse vide.

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description                                                                                                                                                                           |
|--------|------------------|
| 400  | Les paramètres de la requête ne sont pas valides. |
| 404  | La version d’évaluation du package spécifiée est introuvable.  |
| 409  | Le vol de package spécifié a été trouvé, mais il ne peut pas être supprimé dans son état actuel, ou l’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Créer un package de vol](create-a-flight.md)
* [Obtenir un vol de package](get-a-flight.md)
