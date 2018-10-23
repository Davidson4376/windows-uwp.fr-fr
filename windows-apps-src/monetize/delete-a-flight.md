---
author: Xansky
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour supprimer une version d’évaluation du package pour une app inscrite dans votre compte du Centre de développement Windows.
title: Supprime une version d’évaluation du package
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de soumission au MicrosoftStore, supprimer une version d’évaluation
ms.localizationpriority: medium
ms.openlocfilehash: 436a28cc1be0c106928784086731fe078d789527
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5443701"
---
# <a name="delete-a-package-flight"></a>Supprime une version d’évaluation du package

Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour supprimer une version d’évaluation du package pour une app inscrite dans votre compte du Centre de développement Windows.


## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la version d’évaluation du package à supprimer. L’ID Windows Store de l’application est disponible dans le tableau de bord du Centre de développement.  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package à supprimer. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md). Concernant une version d’évaluation créée dans le tableau de bord du Centre de développement, cet ID est également disponible dans l’URL de la page de la version d’évaluation, dans le tableau de bord.  |


### <a name="request-body"></a>Corps de demande

Ne fournissez pas de corps de requête pour cette méthode.


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment supprimer une version d’évaluation du package.

```
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
| 409  | La version d’évaluation du package spécifiée a été trouvée, mais elle n’a pas pu être supprimée en raison de son état actuel, ou l’app utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Crée une version d’évaluation du package](create-a-flight.md)
* [Obtenir une version d’évaluation du package](get-a-flight.md)
