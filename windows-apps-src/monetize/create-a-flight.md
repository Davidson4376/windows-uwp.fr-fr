---
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour créer un vol de package pour une application qui est inscrit pour votre compte espace partenaires.
title: Crée une version d’évaluation du package
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, créer une version d’évaluation
ms.localizationpriority: medium
ms.openlocfilehash: af5ffe0dd72f0c3aae21a2dc522b469358626bab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603454"
---
# <a name="create-a-package-flight"></a>Crée une version d’évaluation du package

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour créer un vol de package pour une application qui est inscrit pour votre compte espace partenaires.

> [!NOTE]
> Cette méthode permet de créer une version d’évaluation de package sans soumission. Pour créer une soumission pour une version d’évaluation de package, voir les méthodes présentées dans l’article [Gérer les soumissions de version d’évaluation de package](manage-flight-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez créer une version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |


### <a name="request-body"></a>Corps de la requête

Le corps de la requête contient les paramètres suivants.

|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  friendlyName  |  chaîne  |  Nom de la version d’évaluation du package, tel que spécifié par le développeur.  |  Non  |
|  groupIds  |  tableau  |  Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation du package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation du package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |  Non  |
|  rankHigherThan  |  chaîne  |  Nom convivial de la version d’évaluation du package classée juste en dessous de la version d’évaluation du package actuelle. Si vous ne définissez pas ce paramètre, la nouvelle version d’évaluation de package est classée au-dessus de toutes les autres. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).    |  Non  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une nouvelle version d’évaluation de package pour une application ayant l’ID Windows Store 9WZDNCRD911W.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir les sections suivantes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | chaîne  | ID de la version d’évaluation du package. Cette valeur est fournie par les partenaires.  |
| friendlyName           | chaîne  | Nom de la version d’évaluation du package, tel que spécifié dans la requête.   |  
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation de package, comme spécifié dans la requête. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation du package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | chaîne  | Nom convivial de la version d’évaluation de package qui est classée juste en dessous de la version d’évaluation de package actuelle, comme spécifié dans la requête. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | La requête n’est pas valide. |
| 409  | Le vol de package n’a pas pu être créé en raison de son état actuel, ou l’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir un vol de package](get-a-flight.md)
* [Supprimer un vol de package](delete-a-flight.md)
