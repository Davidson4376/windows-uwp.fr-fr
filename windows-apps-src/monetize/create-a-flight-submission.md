---
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Utilisez cette méthode dans l’API de soumission au Microsoft Store pour créer une soumission de version d’évaluation du package pour une application inscrite dans votre compte espace partenaires.
title: Créer une soumission de version d’évaluation de package
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au MicrosoftStore, créer une soumission de version d’évaluation
ms.localizationpriority: medium
ms.openlocfilehash: 1e303027aaf3b10260090c500df573f1bf484e20
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7976316"
---
# <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation de package

Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour créer une soumission de version d’évaluation de package pour une app. Après avoir créé une soumission à l’aide de cette méthode, [mettez à jour cette soumission](update-a-flight-submission.md) pour apporter les modifications nécessaires aux données de soumission, puis [validez la soumission](commit-a-flight-submission.md) pour permettre son intégration et sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission au MicrosoftStore, voir [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md).

> [!NOTE]
> Cette méthode permet de créer une soumission pour une version d’évaluation de package existante. Pour créer une version d’évaluation de package, utilisez la méthode [Créer une version d’évaluation de package](create-a-flight.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer une version d’évaluation de package pour une application. Vous pouvez le faire dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode de [créer une version d’évaluation de package](create-a-flight.md) .

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez créer une soumission de version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package pour laquelle vous voulez ajouter la soumission. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md).  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une soumission de version d’évaluation de package pour une application ayant l’ID Windows Store 9WZDNCRD91MD.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la nouvelle soumission. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource Soumission de version d’évaluation de package](manage-flight-submissions.md#flight-submission-object).

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
| 409  | La soumission de version d’évaluation du package n’a pas pu être créée en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas pris en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
* [Obtenir une soumission de version d’évaluation de package](get-a-flight-submission.md)
* [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md)
* [Mettre à jour une soumission de version d’évaluation de package](update-a-flight-submission.md)
* [Supprimer une soumission de version d’évaluation du package](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation du package](get-status-for-a-flight-submission.md)
