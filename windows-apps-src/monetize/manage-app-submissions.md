---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "Utilisez ces méthodes dans l’API de soumission du Windows Store pour gérer les soumissions des applications qui sont inscrites dans votre compte du Centre de développement Windows."
title: "Gérer les soumissions d’applications à l’aide de l’API de soumission du Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 178b70db1583790c174d65e060c8bce6e4f69243
ms.openlocfilehash: 448eafbdadb21476da43e7408bb8bad354ba486d

---

# Gérer les soumissions d’applications à l’aide de l’API de soumission du Windows Store




Utilisez les méthodes suivantes dans l’API de soumission du Windows Store pour gérer les soumissions des applications qui sont inscrites dans votre compte du Centre de développement Windows. Pour obtenir une présentation de l’API de soumission du Windows Store, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Remarque**&nbsp;&nbsp;Ces méthodes ne peuvent être utilisées que pour les comptes du Centre de développement Windows qui ont reçu l’autorisation d’utiliser l’API de soumission du Windows Store. Tous les comptes ne bénéficient pas de cette autorisation.


| Méthode        | URI    | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Obtient les données d’une soumission d’application existante. Pour plus d’informations, voir [Obtenir une soumission d’application](get-an-app-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status``` | Obtient l’état d’une soumission d’application existante. Pour plus d’informations, voir [Obtenir l’état d’une soumission d’application](get-status-for-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions``` | Crée une soumission pour une application inscrite dans votre compte du Centre de développement Windows. Pour plus d’informations, voir [Créer une soumission d’application](create-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` | Valide une soumission d’application nouvelle ou mise à jour dans le Centre de développement Windows. Pour plus d’informations, voir [Valider une soumission d’application](commit-an-app-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Met à jour une soumission d’application existante. Pour plus d’informations, voir [Mettre à jour une soumission d’application](update-an-app-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Supprime une soumission d’application. Pour plus d’informations, voir [Supprimer une soumission d’application](delete-an-app-submission.md). |

<span id="create-an-app-submission">
## Créer une soumission d’application

Pour créer une soumission pour une application, suivez ce processus.

1. Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission du Windows Store.

  >**Remarque**&nbsp;&nbsp;Vérifiez que l’application a déjà fait l’objet d’au moins une soumission complète avec les informations de [classification par âge](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) spécifiées.

3. [Obtenir un jeton d’accès AzureAD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission du Windows Store. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

4. Exécutez la méthode suivante de l’API de soumission du Windows Store. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée. Pour plus d’informations, voir [Créer une soumission d’application](create-an-app-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  Le corps de la réponse contient trois éléments: l’ID de la nouvelle soumission, ses données (notamment toutes les listes et informations tarifaires), ainsi que l’URI de signature d’accès partagé (SAS) pour le chargement de tous les packages d’application et toutes les images de listing de la soumission. Pour plus d’informations sur SAS, voir [Signatures d’accès partagé, partie 1: présentation du modèle SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

3. Si vous ajoutez de nouveaux packages ou de nouvelles images pour la soumission, [préparez les packages d’application](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) et [préparez les images et captures d’écran de l’application](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Ajoutez tous ces fichiers à une archive ZIP.

4. Mettez à jour les données de la soumission avec toutes les modifications requises pour la nouvelle et lancez la méthode suivante pour mettre à jour la soumission. Pour plus d’informations, voir [Mettre à jour une soumission d’application](update-an-app-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  >**Remarque**&nbsp;&nbsp;Si vous ajoutez de nouveaux packages ou de nouvelles images pour la soumission, veillez à mettre à jour les données de la soumission pour faire référence au nom et au chemin relatif de ces fichiers dans l’archiveZIP.

4. Si vous ajoutez de nouveaux packages ou de nouvelles images pour la soumission, chargez l’archiveZIP sur l’URI SAS fourni dans le corps de la réponse de la méthode POST appelée à l’étape2. Pour plus d’informations, voir [Signatures d’accès partagé, partie 2: créer et utiliser une SAS avec le stockage d’objets blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  L’extrait de code suivant montre comment charger l’archive à l’aide de la classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) incluse dans la bibliothèque cliente de stockage Azure pour .NET.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Validez la soumission en exécutant la méthode suivante. Le Centre de développement est ainsi informé que vous avez terminé votre soumission et que vos mises à jour doivent être appliqués à votre compte. Pour plus d’informations, voir [Valider une soumission d’application](commit-an-app-submission.md).

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. Vérifiez l’état de validation en exécutant la méthode suivante. Pour plus d’informations, voir [Obtenir l’état d’une soumission d’application](get-status-for-an-app-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la requête aboutit ou à **CommitFailed** si elle contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de la soumission à l’aide de la méthode précédente ou en visitant le tableau de bord du Centre de développement.

## Ressources

Ces méthodes utilisent les ressources suivantes pour formater les données.

<span id="app-submission-object" />
### Soumission d’application

Cette ressource représente une soumission pour une application. L’exemple suivant illustre le format de cette ressource.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2"
}
```

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | chaîne  | ID de la soumission.  |
| applicationCategory           | chaîne  |   Chaîne qui spécifie la [catégorie et/ou sous-catégorie](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) pour votre application. Les catégories et sous-catégories sont combinées en une seule chaîne à l’aide du caractère trait de soulignement«_», par exemple **BooksAndReference_EReader**.      |  
| pricing           |  objet  | Objet qui contient les informations de tarification pour l’application. Pour plus d’informations, voir la section relative à la [ressource de tarification](#pricing-object) ci-après.       |   
| visibility           |  chaîne  |  Visibilité de l’application. Les valeurs possibles sont les suivantes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |  
| listings           |   objet  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays et chaque valeur est un objet de [ressource de listing](#listing-object) qui contient des informations de listing pour l’application.       |   
| hardwarePreferences           |  tableau  |   Tableau de chaînes qui définissent les [préférences matérielles](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) pour votre application. Les valeurs possibles sont les suivantes: <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>NFC</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  valeur booléenne  |   Indique si Windows peut inclure les données de votre application dans les sauvegardes automatiques sur OneDrive. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  valeur booléenne  |   Indique si les clients peuvent installer votre application sur un stockage amovible. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  valeur booléenne |   Indique si les jeux DVR sont activés pour l’application.    |   
| hasExternalInAppProducts           |     valeur booléenne          |   Indique si votre application permet aux utilisateurs d’effectuer des achats hors du système de commerce du Windows Store. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    valeur booléenne           |  Indique si votre application a fait l’objet de tests pour voir si elle est conforme aux recommandations d’accessibilité. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  chaîne  |   Contient des [notes de certification](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) pour votre application.    |    
| status           |   chaîne  |  État de la soumission. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   objet  |  Contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs. Pour plus d’informations, voir la section [Détails d’état](#status-details-object) ci-après.       |    
| fileUploadUrl           |   chaîne  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages ou de nouvelles images à la soumission, chargez l’archive ZIP qui les contient vers cet URI. Pour plus d’informations, voir [Créer une soumission d’application](#create-an-app-submission).       |    
| applicationPackages           |   tableau  | Contient des objets qui fournissent des détails sur chaque package de la soumission. Pour plus d’informations, voir la section [Package d’application](#application-package-object) ci-après. |    
| enterpriseLicensing           |  chaîne  |  Une des [valeurs de gestion des licences d’entreprise](#enterprise-licensing) qui indiquent le comportement de gestion des licences d’entreprise adopté pour l’application.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  valeur booléenne   |  Indique si Microsoft est autorisé à [rendre l’application disponible pour les futures familles d’appareils Windows10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | objet   |  Dictionnaire de paires clé/valeur, où chaque clé est une [famille d’appareils Windows10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) et chaque valeur est une valeur booléenne qui indique si votre application est autorisée à cibler la famille d’appareils spécifiée.     |    
| friendlyName           |   chaîne  |  Nom convivial de l’application, utilisé à des fins d’affichage.       |  


<span id="listing-object" />
### Listing

Cette ressource contient des informations de listing pour une application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  baseListing               |   objet      |  Informations de [listing de base](#base-listing-object) pour l’application, qui définissent les informations de listing par défaut pour toutes les plateformes.   |     
|  platformOverrides               | objet |   Dictionnaire de paires clé/valeur, où chaque clé est une chaîne qui identifie une plateforme pour laquelle remplacer les informations de listing et chaque valeur est un objet [listing de base](#base-listing-object) (contenant uniquement les valeurs de la description au titre) qui spécifie les informations de listing à remplacer pour la plateforme spécifiée. Les clés peuvent avoir les valeurs suivantes: <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### Listing de base

Cette ressource contient des informations de listing de base pour une application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   chaîne      |  [Informations de copyright et/ou de marque](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info) facultatives.  |
|  keywords                |  tableau       |  Tableau de [mots clés](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords) facilitant l’apparition de l’application dans les résultats de recherche.    |
|  licenseTerms                |    chaîne     | [Termes du contrat de licence](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms) facultatifs de votre application.     |
|  privacyPolicy                |   chaîne      |   URL de la [politique de confidentialité](https://msdn.microsoft.com/windows/uwp/publish/privacy-policy) de votre application.    |
|  supportContact                |   chaîne      |  URL ou adresse e-mail de l’[assistance technique](https://msdn.microsoft.com/windows/uwp/publish/support-contact-info) de votre application.     |
|  websiteUrl                |   chaîne      |  URL de la [page web](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) de votre application.    |    
|  description               |    chaîne     |   [Description](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) du listing de l’application.   |     
|  fonctionnalités               |    tableau     |  Tableau contenant 20chaînes au maximum qui répertorient les [fonctionnalités](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) de votre application.     |
|  releaseNotes               |  chaîne       |  [Notes de publication](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) de votre application.    |
|  images               |   tableau      |  Tableau des données d’[image et d’icône](#image-object) du listing de l’application.  |
|  recommendedHardware               |   tableau      |  Tableau contenant jusqu’à 11chaînes qui répertorient les [configurations matérielles recommandées](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware) pour votre application.     |
|  title               |     chaîne    |   Titre du listing de l’application.   |  


<span id="image-object" />
### Image

Cette ressource contient les données d’image et d’icône d’un listing d’application. Pour plus d’informations sur les images et icônes pour un listing, voir [Images et captures d’écran de l’application](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  fileName               |    chaîne     |   Nom du fichier image dans l’archive ZIP que vous avez chargé pour la soumission.    |     
|  fileStatus               |   chaîne      |  État du fichier image. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  chaîne  | ID de l’image, comme indiqué par le Centre de développement.  |
|  description  |  chaîne  | Description de l’image.  |
|  imageType  |  chaîne  | Une des chaînes suivantes qui indique le type de l’image: <ul><li>Unknown</li><li>Screenshot</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>Icon</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### Tarification

Cette ressource contient des informations de tarification pour l’application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  trialPeriod               |    chaîne     |  Chaîne qui spécifie la période d’évaluation de l’application. Les valeurs possibles sont les suivantes: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    objet     |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre application sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *priceId* du marché spécifié.      |     
|  sales               |   tableau      |  Tableau d’objets qui contiennent des informations commerciales pour l’application. Pour plus d’informations, consultez la section [Vente](#sale-object) ci-dessous.    |     
|  priceId               |   chaîne      |  [Niveau de prix](#price-tier) qui spécifie le [prix de base](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) de l’application.   |


<span id="sale-object" />
### Vente

Cette ressources contient des informations commerciales pour une application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    chaîne     |   Nom de la vente.    |     
|  basePriceId               |   chaîne      |  [Niveau de prix](#price-tiers) à utiliser pour le prix de base de la vente.    |     
|  startDate               |   chaîne      |   Date de début de la vente au format ISO 8601.  |     
|  endDate               |   chaîne      |  Date de fin de la vente au format ISO 8601.      |     
|  marketSpecificPricings               |   objet      |   Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre application sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *basePriceId* du marché spécifié.    |


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


<span id="application-package-object" />
### Package d’application

Cette ressource contient des détails sur un package d’application pour la soumission. L’exemple suivant illustre le format de cette ressource.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

Cette ressource a les valeurs suivantes.  

>**Remarque**&nbsp;&nbsp;Quand vous appelez la méthode de [mise à jour d’une soumission d’application](update-an-app-submission.md), seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de cet objet sont obligatoires dans le corps de la requête. Les autres valeurs sont renseignées par le Centre de développement.

| Valeur           | Type    | Description                   |
|-----------------|---------|------|
| fileName   |   chaîne      |  Nom du package.    |  
| fileStatus    | chaîne    |  État du package. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  chaîne   |  ID qui identifie de manière unique le package. Cette valeur est utilisée par le Centre de développement.   |     
| version    |  chaîne   |  Version du package d’application. Pour plus d’informations, voir [Numérotation des versions de packages](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  chaîne   |  Architecture du package (par exemple, ARM).   |     
| languages    | tableau    |  Tableau des codes des langues prises en charge par l’application. Pour plus d’informations, voir [Langues prises en charge](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  tableau   |  Tableau des fonctionnalités exigées par le package. Pour plus d’informations sur les fonctionnalités, voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  chaîne   |  Version DirectX minimale prise en charge par le package d’application. Cette valeur peut être définie uniquement pour les applications qui ciblent Windows8.x. Elle est ignorée pour les applications qui ciblent d’autres versions. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | chaîne    |  Mémoire RAM minimale exigée par le package d’application. Cette valeur peut être définie uniquement pour les applications qui ciblent Windows8.x. Elle est ignorée pour les applications qui ciblent d’autres versions. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | tableau    |  Tableau de chaînes qui représentent les familles d’appareils que le package cible. Cette valeur est utilisée uniquement pour les packages qui ciblent Windows10. Pour les packages qui ciblent des versions antérieures, cette valeur est **None**. Les chaînes de familles d’appareils suivantes sont actuellement prises en charge pour les packages Windows10, où *{0}* est une chaîne de version de Windows10 comme 10.0.10240.0, 10.0.10586.0 ou 10.0.14393.0: <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

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

Les valeurs suivantes représentent les niveaux de prix disponibles pour une soumission d’application.

| Valeur           | Description                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   Le niveau de prix n’est pas défini; utilisez le prix de base de l’application.      |     
|  NotAvailable              |   L’application n’est pas disponible dans la région spécifiée.    |     
|  Free              |   L’application est gratuite.    |    
|  Tier2 à Tier194               |   Tier2 représente le niveau de prix 0,99USD. Chaque niveau supplémentaire représente des incréments supplémentaires (1,29USD, 1,49USD, 1,99USD, etc.).    |


<span id="enterprise-licensing" />
### Valeurs de gestion des licences d’entreprise

Les valeurs suivantes représentent le comportement de gestion des licences d’entreprise adopté pour l’application. Pour plus d’informations sur ces options, voir [Options de gestion des licences organisationnelles](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

| Valeur           |  Description      |
|-----------------|---------------|
| None            |     Ne mets pas votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne).         |     
| Online        |     Mets votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne).  |
| OnlineAndOffline | Mets votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne) et à la disposition des entreprises par le biais de l’achat de licences en mode hors connexion. |


<span id="submission-status-code" />
### Code d’état de soumission

Les valeurs suivantes représentent le code d’état d’une soumission.

| Valeur           |  Description      |
|-----------------|---------------|
| None            |     Aucun code n’a été spécifié.         |     
| InvalidArchive        |     L’archive ZIP contenant le package n’est pas valide ou a un format d’archive non reconnu.  |
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
* [Obtenir des données d’application à l’aide de l’API de soumission du Windows Store](get-app-data.md)
* [Soumissions d’applications dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)



<!--HONumber=Aug16_HO5-->


