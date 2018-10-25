---
author: Xansky
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour créer une soumission d’extension d’app inscrite dans le compte du Centre de développement Windows.
title: Crée une soumission d’extension
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de soumission au MicrosoftStore, créer une soumission d’extension, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 39af744d8c8e4b9d26cd9d82a9489396857ffa5b
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5514766"
---
# <a name="create-an-add-on-submission"></a>Crée une soumission d’extension


Utilisez cette méthode dans l’API de soumission au MicrosoftStore pour créer une soumission d’extension (également connue sous le nom PIA, produit in-app) pour une app inscrite dans votre compte du Centre de développement Windows. Après avoir créé une soumission à l’aide de cette méthode, [mettez à jour cette soumission](update-an-add-on-submission.md) pour apporter les modifications nécessaires aux données de soumission, puis [validez la soumission](commit-an-add-on-submission.md) pour permettre son intégration et sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission d’extension à l’aide de l’API de soumission au MicrosoftStore, consultez [Gérer les soumissions d’extensions](manage-add-on-submissions.md).

> [!NOTE]
> Cette méthode permet de créer une soumission pour une extension existante. Pour créer une extension, utilisez la méthode [Créer une extension](create-an-add-on.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une extension pour une application dans votre compte du Centre de développement. Pour cela, vous pouvez utiliser le tableau de bord du Centre de développement ou la méthode [Créer une extension](create-an-add-on.md).

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | chaîne | Obligatoire. ID Windows Store de l’extension pour laquelle vous voulez créer une soumission. L’ID Windows Store est disponible dans le tableau de bord du Centre de développement. Il figure également dans les données de réponse des requêtes pour [créer une extension](create-an-add-on.md) ou [obtenir les détails sur les extensions](get-all-add-ons.md).  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une soumission pour une extension.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la nouvelle soumission. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource Soumission d’extension](manage-add-on-submissions.md#add-on-submission-object).

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
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free",
    "isAdvancedPricingModel": true
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
| 400  | Impossible de créer la soumission, car la requête n’est pas valide. |
| 409  | La soumission n’a pas pu être créée en raison de l’état actuel de l’app, ou celle-ci utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de modules complémentaires](manage-add-on-submissions.md)
* [Obtenir une soumission d’extension](get-an-add-on-submission.md)
* [Valider une soumission d’extension](commit-an-add-on-submission.md)
* [Mettre à jour une soumission d’extension](update-an-add-on-submission.md)
* [Supprimer une soumission d’extension](delete-an-add-on-submission.md)
* [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md)
