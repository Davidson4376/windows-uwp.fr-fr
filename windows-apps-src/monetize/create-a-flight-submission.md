---
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour créer une nouvelle soumission de vol de package pour une application qui est inscrit pour votre compte espace partenaires.
title: Créer une soumission de version d’évaluation du package
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, créer une soumission de version d’évaluation
ms.localizationpriority: medium
ms.openlocfilehash: b6474c566795043a435e70b2b41d4f5b59280c15
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371985"
---
# <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

Utilisez cette méthode dans l’API de soumission au Microsoft Store pour créer une soumission de version d’évaluation de package pour une app. Après avoir créé une soumission à l’aide de cette méthode, [mettez à jour cette soumission](update-a-flight-submission.md) pour apporter les modifications nécessaires aux données de soumission, puis [validez la soumission](commit-a-flight-submission.md) pour permettre son intégration et sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission au Microsoft Store, voir [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md).

> [!NOTE]
> Cette méthode permet de créer une soumission pour une version d’évaluation de package existante. Pour créer une version d’évaluation de package, utilisez la méthode [Créer une version d’évaluation de package](create-a-flight.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer un vol de package pour une application. Vous pouvez le faire dans le centre de partenaires, ou vous pouvez le faire à l’aide de la [créer un package de vol](create-a-flight.md) (méthode).

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| PUBLIER    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez créer une soumission de version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation de package pour laquelle vous voulez ajouter la soumission. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md).  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une soumission de version d’évaluation de package pour une application ayant l’ID Windows Store 9WZDNCRD91MD.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la nouvelle soumission. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission de version d’évaluation du package](manage-flight-submissions.md#flight-submission-object).

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
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Impossible de créer la soumission de version d’évaluation de package, car la requête n’est pas valide. |
| 409  | L’envoi de vol de package n’a pas pu être créé en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de vol de package](manage-flight-submissions.md)
* [Obtenir une soumission de vol de package](get-a-flight-submission.md)
* [Valider une soumission de vol de package](commit-a-flight-submission.md)
* [Mettre à jour une soumission de vol de package](update-a-flight-submission.md)
* [Supprimer l’envoi de vol d’un package](delete-a-flight-submission.md)
* [Obtenir l’état de l’envoi de vol d’un package](get-status-for-a-flight-submission.md)
