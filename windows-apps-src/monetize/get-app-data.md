---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Utilisez ces méthodes dans l’API de soumission au Microsoft Store pour récupérer des données pour les applications qui sont enregistrées sur votre compte espace partenaires.
title: Obtenir des données d’application
ms.date: 02/28/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, données d'app
ms.localizationpriority: medium
ms.openlocfilehash: 312729c25d5d9f34471c7154a84273bcbf844da4
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7720225"
---
# <a name="get-app-data"></a>Obtenir des données d’application

Utilisez les méthodes suivantes dans l’API de soumission au Microsoft Store pour obtenir des données pour les applications existantes dans votre compte espace partenaires. Pour obtenir une présentation de l’API de soumission au MicrosoftStore, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md).

Avant de pouvoir utiliser ces méthodes, l’application doit déjà exister dans votre compte espace partenaires. Pour créer ou gérer des soumissions pour des applications, consultez les méthodes indiquées dans l’article [Gérer les soumissions d’applications](manage-app-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Méthode</th>
<th align="left">URI</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications</td>
<td align="left"><a href="get-all-apps.md">Obtenir des données pour toutes vos applications</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}</td>
<td align="left"><a href="get-an-app.md">Obtenir des données pour une application spécifique</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts</td>
<td align="left"><a href="get-add-ons-for-an-app.md">Obtenir des modules complémentaires pour une application</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights</td>
<td align="left"><a href="get-flights-for-an-app.md">Obtenir des versions d’évaluation du package pour une application</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="prerequisites"></a>Conditions préalables

Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore avant d’essayer d’utiliser l’une de ces méthodes.

## <a name="data-resources"></a>Ressources de données

Les méthodes de l’API de soumission au MicrosoftStore pour obtenir des données d’app utilisent les ressources de données JSON suivantes.

<span id="application_object" />

### <a name="application-resource"></a>Ressource d’application

Cette ressource représente une application inscrite dans votre compte.

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
  },
  "hasAdvancedListingPermission": true
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|---------------------|
| id            | chaîne  | ID Windows Store de l’application. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | chaîne  | Nom principal de l’application.      |
| packageFamilyName | chaîne  | Nom de la famille de packages de l’application.      |
| packageIdentityName          | chaîne  | Nom de l’identité du package de l’application.                       |
| publisherName       | chaîne  | ID de l’éditeur Windows associé à l’application. Cela correspond à la valeur de **Package/identité/éditeur** qui s’affiche sur la page [identité de l’application](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) pour l’application dans l’espace partenaires.       |
| firstPublishedDate      | chaîne  | Date de la première publication de l’application, au format ISO8601.   |
| lastPublishedApplicationSubmission       | objet | [Ressource de soumission](#submission_object) qui fournit des informations sur la dernière soumission publiée de l’application.    |
| pendingApplicationSubmission        | objet  |  [Ressource de soumission](#submission_object) qui fournit des informations sur la soumission actuellement en attente pour l’application.   |   
| hasAdvancedListingPermission        | valeur booléenne  |  Indique si vous pouvez configurer les valeurs [gamingOptions](manage-app-submissions.md#gaming-options-object) ou [trailers](manage-app-submissions.md#trailer-object) pour les soumissions de l’application. Cette valeur est vraie pour les soumissions créées après mai2017. |  |


<span id="add-on-object" />

### <a name="add-on-resouce"></a>Ressource d’extension

Cette ressource fournit des informations sur une extension.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description         |
|-----------------|---------|----------------------|
| inAppProductId            | chaîne  | ID WindowsStore de l’extension. Cette valeur est fournie par le Windows Store. Exemple d’ID Windows Store: 9NBLGGH4TNMP.   |


<span id="flight-object" />

### <a name="flight-resource"></a>Ressource de version d’évaluation du package

Cette ressource fournit des informations sur une version d’évaluation du package pour une application.

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

| Valeur           | Type    | Description           |
|-----------------|---------|------------------------|
| flightId            | chaîne  | ID de la version d’évaluation du package. Cette valeur est fournie par l’espace partenaires.  |
| friendlyName           | chaîne  | Nom de la version d’évaluation du package, tel que spécifié par le développeur.   |
| lastPublishedFlightSubmission       | objet | [Ressource de soumission](#submission_object) qui fournit des informations sur la dernière soumission publiée de la version d’évaluation du package.   |
| pendingFlightSubmission        | objet  |  [Ressource de soumission](#submission_object) qui fournit des informations sur la soumission actuellement en attente pour la version d’évaluation du package.  |    
| groupIds           | tableau  | Tableau de chaînes qui contiennent les ID des groupes de versions d’évaluation associés à la version d’évaluation du package. Pour plus d’informations sur les groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | chaîne  | Nom convivial de la version d’évaluation du package classée juste en dessous de la version d’évaluation du package actuelle. Pour plus d’informations sur le classement des groupes de versions d’évaluation, voir [Versions d’évaluation de package](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />

### <a name="submission-resource"></a>Ressource de soumission

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

| Valeur           | Type    | Description                 |
|-----------------|---------|------------------------------|
| id            | chaîne  | ID de la soumission.    |
| resourceLocation   | chaîne  | Chemin relatif à ajouter à l’URI de requête ```https://manage.devcenter.microsoft.com/v1.0/my/``` de base pour récupérer les données complètes de la soumission.            |
 
<span/>

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions d’apps à l’aide de l’API de soumission au MicrosoftStore](manage-app-submissions.md)
* [Obtenir toutes les applications](get-all-apps.md)
* [Obtenir une application](get-an-app.md)
* [Obtenir des extensions pour une application](get-add-ons-for-an-app.md)
* [Obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md)
