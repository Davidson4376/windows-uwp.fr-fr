---
author: mcleanbyron
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: "Utilisez cette méthode dans l’API de soumission du Windows Store pour mettre à jour une soumission de version d’évaluation du package existante."
title: "Mettre à jour une soumission de version d’évaluation de package à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 9dfad9c0cc6b6e03a2196946ac578ca3170fccc5

---

# Mettre à jour une soumission de version d’évaluation de package à l’aide de l’API de soumission du Windows Store


Utilisez cette méthode dans l’API de soumission du Windows Store pour mettre à jour une soumission de version d’évaluation du package existante. Après avoir mis à jour une soumission à l’aide de cette méthode, vous devez [valider la soumission](commit-a-flight-submission.md) en vue de son intégration et de sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission du Windows Store, voir [Gérer les soumissions d’extensions](manage-flight-submissions.md).

## Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission de version d’évaluation du package pour une application dans votre compte du Centre de développement. Pour cela, vous pouvez utiliser le tableau de bord du Centre de développement ou la méthode [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md).

>**Remarque**&nbsp;&nbsp;Cette méthode ne peut être utilisée que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.

## Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}``` |

<span/>
 

### En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

<span/>

### Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez mettre à jour une soumission de version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation de package pour laquelle vous voulez mettre à jour une soumission. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une version d’évaluation du package](create-a-flight.md) et à [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md).  |
| submissionId | chaîne | Obligatoire. ID de la soumission à mettre à jour. Cet ID est disponible dans le tableau de bord du Centre de développement et figure également dans les données de réponse aux requêtes visant à [créer une soumission de version d’évaluation du package](create-a-flight-submission.md).  |

<span/>

### Corps de la requête

Le corps de la requête contient les paramètres suivants.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | tableau  | Contient des objets qui fournissent des détails sur chaque package de la soumission. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de package de version d’évaluation](manage-flight-submissions.md#flight-package-object). Quand vous appelez cette méthode pour mettre à jour une soumission d’application, seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de ces objets sont nécessaires dans le corps de la requête. Les autres valeurs sont renseignées par le Centre de développement. |
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| notesForCertification           | chaîne  |  Fournit des informations supplémentaires aux testeurs de certification, telles que les informations d’identification du compte de test et les étapes permettant d’accéder aux fonctionnalités et de les vérifier. Pour plus d’informations, voir [Notes de certification](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span/>

### Exemple de requête

L’exemple suivant montre comment mettre à jour une soumission de version d’évaluation de package pour une application.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la soumission mise à jour. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission de version d’évaluation du package](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Impossible de mettre à jour la soumission de version d’évaluation de package, car la requête n’est pas valide. |
| 409  | La soumission de version d’évaluation de package n’a pas pu être mise à jour en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission du Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md)
* [Valider une soumission de version d’évaluation du package](commit-a-flight-submission.md)
* [Supprimer une soumission de version d’évaluation du package](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation du package](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


