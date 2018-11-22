---
author: Xansky
ms.assetid: 8C63D33B-557D-436E-9DDA-11F7A5BFA2D7
description: Utilisez cette méthode dans l’API de soumission au Microsoft Store pour mettre à jour une soumission d’extension existante.
title: Mettre à jour une soumission d’extension
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, soumission d’extension, mise à jour, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 2b363132924af5fca976fda814b185155292385e
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7566434"
---
# <a name="update-an-add-on-submission"></a>Mettre à jour une soumission d’extension


Utilisez cette méthode dans l’API de soumission au Microsoft Store pour mettre à jour une extension existante (également dénommée PIA ou produit in-app). Après avoir mis à jour une soumission à l’aide de cette méthode, vous devez [valider la soumission](commit-an-add-on-submission.md) en vue de son intégration et de sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission d’extension à l’aide de l’API de soumission au Microsoft Store, consultez [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer une soumission d’extension pour l’une de vos applications. Vous pouvez le faire dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode de [créer une soumission d’extension](create-an-add-on-submission.md) .

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension pour laquelle vous voulez mettre à jour une soumission. L’ID Windows Store est disponible dans l’espace partenaires, et il est inclus dans les données de réponse des requêtes pour [créer une extension](create-an-add-on.md) ou [d’obtenir des détails sur les extensions](get-all-add-ons.md).  |
| submissionId | chaîne | Obligatoire. ID de la soumission à mettre à jour. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’extension](create-an-add-on-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de la soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Le corps de la requête contient les paramètres suivants.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| contentType           | chaîne  |  [Type de contenu](../publish/enter-add-on-properties.md#content-type) qui est fourni dans l’extension. Les valeurs possibles sont les suivantes: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | tableau  | Tableau de chaînes qui contiennent jusqu’à 10[motsclés](../publish/enter-add-on-properties.md#keywords) pour l’extension. Votre application peut rechercher des extensions à l’aide de ces motsclés.   |
| lifetime           | chaîne  |  Durée de vie de l’extension. Les valeurs possibles sont les suivantes: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | objet  | Objet qui contient des informations de référencement pour l’extension. Pour plus d’informations, voir la [ressource de référencement](manage-add-on-submissions.md#listing-object).  |
| pricing           | objet  | Objet qui contient des informations de tarification pour l’extension. Pour plus d’informations, voir la [ressource de tarification](manage-add-on-submissions.md#pricing-object).  |
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| tag           | chaîne  |  [Données développeur personnalisées](../publish/enter-add-on-properties.md#custom-developer-data) de l’extension (ces informations étaient précédemment appelées *tag*).   |
| visibility  | chaîne  |  Visibilité de l’extension. Les valeurs possibles sont les suivantes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment mettre à jour une soumission d’extension.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
}
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la soumission mise à jour. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission d’extension](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Impossible de mettre à jour la soumission, car la requête n’est pas valide. |
| 409  | La soumission ne peut pas être mis à jour en raison de l’état actuel de l’extension, ou l’extension utilise une fonctionnalité de l’espace partenaires qui n’est [actuellement pas pris en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de modules complémentaires](manage-add-on-submissions.md)
* [Obtenir une soumission de module complémentaire](get-an-add-on-submission.md)
* [Créer une soumission d’extension](create-an-add-on-submission.md)
* [Valider une soumission d’extension](commit-an-add-on-submission.md)
* [Supprimer une soumission d’extension](delete-an-add-on-submission.md)
* [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md)
