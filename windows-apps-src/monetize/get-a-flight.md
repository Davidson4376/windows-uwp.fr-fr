---
author: Xansky
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour obtenir des données pour une version d’évaluation du package pour une app inscrite dans votre compte du Centre de développement Windows.
title: Obtenir une version d’évaluation du package
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, version d’évaluation, version d'évaluation du package
ms.localizationpriority: medium
ms.openlocfilehash: 772dd59ad25cd5439df8b88cd8818f98334c2969
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5940792"
---
# <a name="get-a-package-flight"></a>Obtenir une version d’évaluation du package

Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour obtenir des données pour une version d’évaluation du package pour une app inscrite dans votre compte du Centre de développement Windows.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la version d’évaluation du package à obtenir. L’ID Windows Store de l’application est disponible dans le tableau de bord du Centre de développement.  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package à obtenir. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md). Concernant une version d’évaluation créée dans le tableau de bord du Centre de développement, cet ID est également disponible dans l’URL de la page de la version d’évaluation, dans le tableau de bord.  |


### <a name="request-body"></a>Corps de demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment récupérer les informations sur un version d’évaluation du dont l’ID est 43e448df-97c9-4a43-a0bc-2a445e736bcd pour une application dont la valeur de l’IS Windows Store est 9WZDNCRD91MD.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | chaîne  | ID de la version d’évaluation du package. Cette valeur est fournie par le Centre de développement.  |
| friendlyName           | chaîne  | Nom de la version d’évaluation du package, tel que spécifié par le développeur.   |  
| lastPublishedFlightSubmission       | objet | Objet qui fournit des informations sur la dernière soumission publiée de la version d’évaluation du package. Pour plus d’informations, voir la section [Objet de la soumission](#submission_object) ci-dessous.  |
| pendingFlightSubmission        | objet  |  Objet qui fournit des informations sur la soumission actuellement en attente pour la version d’évaluation du package. Pour plus d’informations, voir la section [Objet de la soumission](#submission_object) ci-dessous.  |   
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation du package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | chaîne  | Nom convivial de la version d’évaluation du package classée juste en dessous de la version d’évaluation du package actuelle. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation du package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />

### <a name="submission-object"></a>Objet de la soumission

Les valeurs *lastPublishedFlightSubmission* et *pendingFlightSubmission* dans le corps de la réponse contiennent des objets qui fournissent des informations sur les ressources d’une soumission de version d’évaluation du package. Ces objets ont les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | chaîne  | ID de la soumission.    |
| resourceLocation   | chaîne  | Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de la soumission.               |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description     |
|--------|---------------------  |
| 400  | La requête n’est pas valide. |
| 404  | La version d’évaluation du package spécifiée est introuvable.   |   
| 409  | L’app utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md#not_supported). |                                                                                                 


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Crée une version d’évaluation du package](create-a-flight.md)
* [Supprimer une version d’évaluation du package](delete-a-flight.md)
