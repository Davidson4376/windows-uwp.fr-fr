---
author: mcleanbyron
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: "Utilisez ces méthodes dans l’API de soumission du Windows Store pour gérer les soumissions d’extensions des applications qui sont inscrites dans votre compte du Centre de développement Windows."
title: "Gérer les soumissions d’extensions à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 52e589c90a8d78905a9617dc2802d76a2f0f0360

---

# Gérer les soumissions d’extensions à l’aide de l’API de soumission du Windows Store



Utilisez les méthodes suivantes dans l’API de soumission du Windows Store pour gérer les soumissions d’extensions (également connue sous le nom produit in-app ou PIA) pour les applications inscrites dans votre compte du Centre de développement Windows. Pour obtenir une présentation de l’API de soumission du Windows Store, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**&nbsp;&nbsp;Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation. Pour pouvoir utiliser ces méthodes pour créer ou gérer des soumissions pour une extension, l’extension doit déjà exister dans votre compte du Centre de développement. Vous pouvez créer une extension [à l’aide du tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) ou en utilisant les méthodes de l’API de soumission du Windows Store décrites dans [Gérer les extensions](manage-add-ons.md).

| Méthode        | URI    | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Obtient les données d’une soumission d’extension existante. Pour plus d’informations, voir [Obtenir une soumission d’extension](get-an-add-on-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status``` | Obtient l’état d’une soumission d’extension existante. Pour plus d’informations, voir [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions``` | Crée une soumission d’extension pour une application inscrite dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Créer une soumission d’extension](create-an-add-on-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit``` | Valide une soumission d’extension nouvelle ou mise à jour dans le Centre de développement Windows. Pour plus d’informations, voir [Valider une soumission d’extension](commit-an-add-on-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Met à jour une soumission d’extension existante. Pour plus d’informations, voir [Mettre à jour une soumission d’extension](update-an-add-on-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | Supprime une soumission d’extension. Pour plus d’informations, voir [Supprimer une soumission d’extension](delete-an-add-on-submission.md). |

<span id="create-an-add-on-submission">
## Créer une soumission d’extension

Pour créer une soumission pour une extension, suivez ce processus.

1. Si vous ne l’avez pas encore fait, remplissez les conditions préalables décrites dans [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md), notamment l’association d’une application Azure AD à votre compte du Centre de développement Windows et l’obtention de votre ID client et de votre clé. Vous n’aurez à le faire qu’une seule fois. L’ID client et la clé une fois à votre disposition sont réutilisables chaque fois que vous avez besoin de créer un jeton d’accès Azure AD.  

2. [Obtenir un jeton d’accès AzureAD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission du Windows Store. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

3. Exécutez la méthode suivante de l’API de soumission du Windows Store. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée. Pour plus d’informations, voir [Créer une soumission d’extension](create-an-add-on-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
  ```

  Le corps de la réponse contient trois éléments: l’ID de la nouvelle soumission, ses données (notamment toutes les listes et informations tarifaires), ainsi que l’URI de signature d’accès partagé (SAS) pour le chargement de toutes les icônes d’extension de la soumission. Pour plus d’informations sur SAS, voir [Signatures d’accès partagé, partie 1: présentation du modèle SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

4. Si vous ajoutez de nouvelles icônes pour la soumission, [préparez-les](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon) et ajoutez-les à une archive ZIP.

5. Mettez à jour les données de la soumission avec toutes les modifications requises pour la nouvelle et lancez la méthode suivante pour mettre à jour la soumission. Pour plus d’informations, voir [Mettre à jour une soumission d’extension](update-an-add-on-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
  ```

  >**Remarque**&nbsp;&nbsp;Si vous ajoutez de nouvelles icônes pour la soumission, assurez-vous de mettre à jour les données de la soumission pour faire référence au nom et au chemin relatif de ces fichiers dans l’archive ZIP.

4. Si vous ajoutez de nouvelles icônes pour la soumission, chargez l’archive ZIP sur l’URI SAS fourni dans le corps de la réponse de la méthode POST appelée à l’étape2. Pour plus d’informations, voir [Signatures d’accès partagé, partie 2: créer et utiliser une SAS avec le stockage d’objets blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  L’extrait de code suivant montre comment charger l’archive à l’aide de la classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) incluse dans la bibliothèque cliente de stockage Azure pour .NET.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Validez la soumission en exécutant la méthode suivante. Le Centre de développement est ainsi informé que vous avez terminé votre soumission et que vos mises à jour doivent être appliqués à votre compte. Pour plus d’informations, voir [Valider une soumission d’extension](commit-an-add-on-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
  ```

6. Vérifiez l’état de validation en exécutant la méthode suivante. Pour plus d’informations, voir [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md).

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
  ```

  Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la requête aboutit ou à **CommitFailed** si elle contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de la soumission à l’aide de la méthode précédente ou en visitant le tableau de bord du Centre de développement.

## Ressources

Ces méthodes utilisent les ressources suivantes pour formater les données.

<span id="add-on-submission-object" />
### Soumission d’extension

Cette ressource représente une soumission pour une extension. L’exemple suivant illustre le format de cette ressource.

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

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | chaîne  | ID de la soumission.  |
| contentType           | chaîne  |  [Type de contenu](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties#content-type) fourni dans l’extension. Les valeurs possibles sont les suivantes: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | tableau  | Tableau de chaînes qui contiennent jusqu’à 10[motsclés](../publish/enter-iap-properties.md#keywords) pour l’extension. Votre application peut rechercher des extensions à l’aide de ces motsclés.   |
| lifetime           | chaîne  |  Durée de vie de l’extension. Les valeurs possibles sont les suivantes: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | objet  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un objet de [ressource de listing](#listing-object) qui contient les informations de listing pour l’application.  |
| pricing           | objet  | Objet qui contient des informations de tarification pour l’extension. Pour plus d’informations, voir la section relative à la [ressource de tarification](#pricing-object) ci-après.  |
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| tag           | chaîne  |  [Balise](../publish/enter-iap-properties.md#tag) associée à l’extension.   |
| visibility  | chaîne  |  Visibilité de l’extension. Les valeurs possibles sont les suivantes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | chaîne  |  État de la soumission. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objet  |  Contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs. Pour plus d’informations, voir la section [Détails d’état](#status-details-object) ci-après. |
| fileUploadUrl           | chaîne  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages à la soumission, chargez l’archive ZIP contenant les packages vers cet URI. Pour plus d’informations, voir [Créer une soumission d’extension](#create-an-add-on-submission).  |
| friendlyName  | chaîne  |  Nom convivial de l’extension, utilisé à des fins d’affichage.  |

<span id="listing-object" />
### Listing

Cette ressource contient des informations de listing pour une extension. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  description               |    chaîne     |   Description du listing d’extensions.   |     
|  icon               |   objet      |  Contient les données de l’icône du listing d’extensions. Pour plus d’informations, voir la section [Icône](#icon-object) ci-dessous.   |
|  title               |     chaîne    |   Titre du listing d’extensions.   |  

<span id="icon-object" />
### Icône

Cette ressource contient les données d’icône du listing d’extensions. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  fileName               |    chaîne     |   Nom du fichier d’icône dans l’archive ZIP que vous avez chargé pour la soumission.    |     
|  fileStatus               |   chaîne      |  État du fichier d’icône. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />
### Tarification

Cette ressource contient des informations de tarification pour l’extension. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  marketSpecificPricings               |    objet     |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre extension sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *priceId* du marché spécifié.     |     
|  sales               |   tableau      |  Tableau d’objets qui contiennent des informations commerciales pour l’extension. Pour plus d’informations, consultez la section [Vente](#sale-object) ci-dessous.    |     
|  priceId               |   chaîne      |  [Niveau de prix](#price-tier) qui spécifie le [prix de base](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price) de l’extension.    |


<span id="sale-object" />
### Vente

Cette ressources contient des informations commerciales pour une extension. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    chaîne     |   Nom de la vente.    |     
|  basePriceId               |   chaîne      |  [Niveau de prix](#price-tiers) à utiliser pour le prix de base de la vente.    |     
|  startDate               |   chaîne      |   Date de début de la vente au format ISO 8601.  |     
|  endDate               |   chaîne      |  Date de fin de la vente au format ISO 8601.      |     
|  marketSpecificPricings               |   objet      |   Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre extension sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *basePriceId* du marché spécifié.    |



<span id="status-details-object" />
### Détails d’état

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  erreurs               |    objet     |   Tableau d’objets qui contiennent des détails d’erreur pour la soumission. Pour plus d’informations, voir la section [Détail d’état](#status-detail-object) ci-après.   |     
|  warnings               |   objet      | Tableau d’objets qui contiennent des détails d’avertissement pour la soumission. Pour plus d’informations, voir la section [Détail d’état](#status-detail-object) ci-après.     |
|  certificationReports               |     objet    |   Tableau d’objets qui donnent accès aux données du rapport de certification pour la soumission. Vous pouvez examiner ces rapports pour obtenir plus d’informations en cas d’échec de la certification. Pour plus d’informations, voir la section [Rapport de certification](#certification-report-object) ci-dessous.   |  


<span id="status-detail-object" />
### Détail d’état

Cette ressource contient des informations supplémentaires sur les éventuels erreurs ou avertissements pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    chaîne     |   Chaîne qui décrit le type d’erreur ou d’avertissement. Pour plus d’informations, voir la section [Code d’état de soumission](#submission-status-code) ci-dessous.   |     
|  details               |     chaîne    |  Message contenant des détails sur le problème.     |


<span id="certification-report-object" />
### Rapport de certification

Cette ressource donne accès aux données du rapport de certification pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    chaîne     |  Date et heure de génération du rapport, au format ISO8601.    |
|     reportUrl            |    chaîne     |  URL vous permettant d’accéder au rapport.    |



## Énumérations

Ces méthodes utilisent les énumérations suivantes.


<span id="price-tiers" />
### Niveaux de prix

Les valeurs suivantes représentent les niveaux de prix disponibles pour une soumission d’extension.

| Valeur           | Description                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   Le niveau de prix n’est pas défini; utilisez le prix de base de l’extension.      |     
|  NotAvailable              |   L’extension n’est pas disponible dans la région spécifiée.    |     
|  Free              |   L’extension est gratuite.    |    
|  Tier2 à Tier194               |   Tier2 représente le niveau de prix 0,99USD. Chaque niveau supplémentaire représente des incréments supplémentaires (1,29USD, 1,49USD, 1,99USD, etc.).    |


<span id="submission-status-code" />
### Code d’état de soumission

Les valeurs suivantes représentent le code d’état d’une soumission.

| Valeur           |  Description      |
|-----------------|---------------|
|  None            |     Aucun code n’a été spécifié.         |     
|      InvalidArchive        |     L’archive ZIP contenant le package n’est pas valide ou a un format d’archive non reconnu.  |
| MissingFiles | L’archive ZIP ne dispose pas de tous les fichiers qui ont été répertoriés dans les données de votre soumission, ou ils se trouvent dans un emplacement incorrect dans l’archive. |
| PackageValidationFailed | La validation d’un ou de plusieurs packages de votre soumission a échoué. |
| InvalidParameterValue | Un des paramètres dans le corps de la requête n’est pas valide. |
| InvalidOperation | L’opération que vous avez tentée n’est pas valide. |
| InvalidState | L’opération que vous avez tentée n’est pas valide pour l’état actuel de la version d’évaluation du package. |
| ResourceNotFound | La version d’évaluation du package spécifiée est introuvable. |
| ServiceError | Une erreur de service interne a empêché la requête de réussir. Relancez la requête. |
| ListingOptOutWarning | Le développeur supprimé un listing d’une soumission précédente ou il n’a pas inclus d’informations de listing prises en charge par le package. |
| ListingOptInWarning  | Le développeur a ajouté un listing. |
| UpdateOnlyWarning | Le développeur essaie d’insérer quelque chose qui prend uniquement en charge la mise à jour. |
| Other  | La soumission est dans un état non reconnu ou non affecté à une catégorie. |
| PackageValidationWarning | Le processus de validation du package a généré un avertissement. |

<span/>

## Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les extensions à l’aide de l’API de soumission du Windows Store](manage-add-ons.md)
* [Soumissions d’extensions dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)



<!--HONumber=Aug16_HO5-->


