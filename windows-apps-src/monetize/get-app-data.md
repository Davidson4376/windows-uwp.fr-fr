---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "Utilisez ces méthodes de l’API de soumission du Windows Store pour récupérer des données pour les applications inscrites dans votre compte du Centre de développement Windows."
title: "Obtenir des données d’application à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99d609decc8c38952961deac5bb8ec6926d91c88

---

# Obtenir des données d’application à l’aide de l’API de soumission du Windows Store

Utilisez les méthodes suivantes dans l’API de soumission du Windows Store pour obtenir des données pour les applications inscrites dans votre compte du Centre de développement Windows. Pour obtenir une présentation de l’API de soumission du Windows Store, voir [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**  Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation. Ces méthodes peuvent uniquement être utilisées pour obtenir des données pour les applications. Pour créer ou gérer des soumissions pour des applications, voir les méthodes dans [Gérer les soumissions d’applications](manage-app-submissions.md).

| Méthode        | URI    | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` | Récupère les données de toutes les applications inscrites dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Obtenir toutes les applications](get-all-apps.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` | Récupère les données relatives à une application spécifique inscrite dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Obtenir une application](get-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` | Répertorie les extensions (également connues sous le nom PIA ou produits in-app) pour une application inscrite dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Obtenir des extensions pour une application](get-add-ons-for-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` | Répertorie les versions d’évaluation du package pour une application inscrite dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md). |

<span/>

## Conditions préalables

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store avant d’essayer d’utiliser l’une de ces méthodes.

## Ressources

Ces méthodes utilisent les ressources suivantes pour formater les données.

<span id="application_object" />
### Application

Cette ressource représente une application inscrite dans votre compte. L’exemple suivant illustre le format de cette ressource.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  }
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | chaîne  | ID Windows Store de l’application. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | chaîne  | Nom principal de l’application.                                                                                                                                                   |
| packageFamilyName | chaîne  | Nom de la famille de packages de l’application.                                                                                                                                                                                                         |
| packageIdentityName          | chaîne  | Nom de l’identité du package de l’application.                                                                                                                                                              |
| publisherName       | chaîne  | ID de l’éditeur Windows associé à l’application. Celui-ci correspond à la valeur **Package/Identité/Éditeur** qui apparaît dans la page [Identité de l’application](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) de l’application dans le tableau de bord du Centre de développement Windows.                                                                                             |
| firstPublishedDate      | chaîne  | Date de la première publication de l’application, au format ISO 8601.                                                                                         |
| lastPublishedApplicationSubmission       | objet | Objet qui fournit des informations sur la dernière soumission publiée de l’application. Pour plus d’informations, voir la section [Soumission](#submission_object) ci-dessous.                                                                                                                                                          |
| pendingApplicationSubmission        | objet  |  Objet qui fournit des informations sur la soumission actuellement en attente pour l’application. Pour plus d’informations, voir la section [Soumission](#submission_object) ci-dessous.  |   |


<span id="add-on-object" />
### Extension

Cette ressource fournit des informations sur une extension. L’exemple suivant illustre le format de cette ressource.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inAppProductId            | chaîne  | ID Windows Store de l’extension. Cette valeur est fournie par le Windows Store. Exemple d’ID Windows Store : 9NBLGGH4TNMP.   |


<span id="flight-object" />
### Version d’évaluation

Cette ressource fournit des informations sur une version d’évaluation du package pour une application. L’exemple suivant illustre le format de cette ressource.

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | chaîne  | ID de la version d’évaluation du package. Cette valeur est fournie par le Centre de développement.  |
| friendlyName           | chaîne  | Nom de la version d’évaluation du package, tel que spécifié par le développeur.   |
| lastPublishedFlightSubmission       | objet | Objet qui fournit des informations sur la dernière soumission publiée de la version d’évaluation du package. Pour plus d’informations, voir la section [Soumission](#submission_object) ci-dessous.  |
| pendingFlightSubmission        | objet  |  Objet qui fournit des informations sur la soumission actuellement en attente pour la version d’évaluation du package. Pour plus d’informations, voir la section [Soumission](#submission_object) ci-dessous.  |    
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation du package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation du package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | chaîne  | Nom convivial de la version d’évaluation du package classée juste en dessous de la version d’évaluation du package actuelle. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation du package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />
### Soumission

Cette ressource fournit des informations sur une soumission. L’exemple suivant illustre le format de cette ressource.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | chaîne  | ID de la soumission.    |
| resourceLocation   | chaîne  | Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de la soumission.                                                                                                                                               |
 
<span/>

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions d’applications à l’aide de l’API de soumission du Windows Store](manage-app-submissions.md)
* [Obtenir toutes les applications](get-all-apps.md)
* [Obtenir une application](get-an-app.md)
* [Obtenir des modules complémentaires pour une application](get-add-ons-for-an-app.md)
* [Obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->


