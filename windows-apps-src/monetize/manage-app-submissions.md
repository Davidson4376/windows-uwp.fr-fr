---
author: Xansky
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Utilisez ces méthodes dans l’API de soumission au Microsoft Store pour gérer les soumissions pour les applications qui sont enregistrées sur votre compte espace partenaires.
title: Gérer les soumissions d’applications
ms.author: mhopkins
ms.date: 04/30/2018
ms.topic: article
keywords: windows10, uwp, API de soumission au MicrosoftStore, soumissions d’app
ms.localizationpriority: medium
ms.openlocfilehash: 76bc7932665e3f9893c6f0aa9644b9edc07a6dcf
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6186350"
---
# <a name="manage-app-submissions"></a>Gérer les soumissions d’applications

L’API de soumission au MicrosoftStore fournit des méthodes qui permettent de gérer les soumissions de vos apps, notamment les lancements de packages progressifs. Pour obtenir une présentation de l’API de soumission au MicrosoftStore, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si vous utilisez l’API de soumission au Microsoft Store pour créer une soumission pour une application, veillez à apporter d’autres modifications à la soumission uniquement à l’aide de l’API, plutôt que de l’espace partenaires. Si vous utilisez l’espace partenaires pour modifier une soumission que vous avez créé à l’origine à l’aide de l’API, vous ne serez n’est plus en mesure de modifier ou valider cette soumission à l’aide de l’API. Dans certains cas, la soumission non validée peut rester définie sur l'état d'erreur. Si cela se produit, vous devez supprimer la soumission et en créer une nouvelle.

> [!IMPORTANT]
> Vous ne pouvez pas utiliser cette API pour publier des soumissions pour [les achats en volume par le biais du MicrosoftStore pour Entreprises et du MicrosoftStore pour Éducation](../publish/organizational-licensing.md) ou pour publier des soumissions pour les [apps cœur de métier](../publish/distribute-lob-apps-to-enterprises.md) directement aux entreprises. Pour ces deux scénarios, vous devez utiliser l’espace partenaires de publier la soumission.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>Méthodes de gestion des soumissions d’apps

Pour obtenir, créer, mettre à jour, valider ou supprimer une soumission d’applications, procédez comme suit. Avant de pouvoir utiliser ces méthodes, l’application doit déjà exister dans votre compte espace partenaires et vous devez d’abord créer une soumission pour l’application dans l’espace partenaires. Pour plus d’informations, consultez les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">Obtient une soumission d’applications existante</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">Obtient l’état d’une soumission d’applications existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">Crée une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">Met à jour une soumission d’applications existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">Valide une soumission d’applications nouvelle ou mise à jour</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">Supprime une soumission d’applications</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>Crée une soumission d’applications

Pour créer une soumission pour une application, suivez ce processus.

1. Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au MicrosoftStore.
    > [!NOTE]
    > Vérifiez que l’app a déjà fait l’objet d’au moins une soumission complète avec les informations de [classification par âge](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) spécifiées.

2. [Obtenir un jeton d’accès AzureAD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission au MicrosoftStore. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

3. [Créez une soumission d’app](create-an-app-submission.md) en exécutant la méthode suivante dans l’API de soumission au MicrosoftStore. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    Le corps de la réponse contient une ressource de [soumission d'application](#app-submission-object) qui inclut l'ID de la nouvelle soumission, l’URI de signature d’accès partagé (SAS) pour le chargement de tous les fichiers associés à la soumission vers le Stockage Blob Azure (notamment les packages d'application, les images de description et les fichiers de bande-annonce), ainsi que toutes les données de la nouvelle soumission (notamment les descriptions et les informations tarifaires).

    > [!NOTE]
    > Un URI SAS permet d’accéder à une ressource sécurisée dans le stockage Azure sans besoin de clés de compte. Pour obtenir des informations générales sur les URI SAS et leur utilisation avec le stockage d’objets blob Azure, consultez [Signatures d’accès partagé, partie1: présentation du modèle SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) et [Signatures d’accès partagé, partie2: créer et utiliser une SAS avec le stockage d’objets blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si vous ajoutez de nouveaux packages, images de description ou fichiers de bande-annonce pour la soumission, [préparez les packages d’application](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) et [préparez les captures d’écran, les images et les bandes-annonces de l’application](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Ajoutez tous ces fichiers à une archive ZIP.

5. Révisez les données de la [soumission de l'app](#app-submission-object) avec toutes les modifications requises pour la nouvelle et lancez la méthode suivante pour [mettre à jour la soumission d’app](update-an-app-submission.md).

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si vous ajoutez de nouveaux fichiers pour la soumission, assurez-vous de mettre à jour les données de la soumission pour faire référence au nom et au chemin relatif de ces fichiers dans l’archive ZIP.

4. Si vous ajoutez de nouveaux packages, images de description ou fichiers de bande-annonce pour la soumission, chargez l’archiveZIP dans le [Stockage Blob Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) à l’aide de l’URI SAS fourni dans le corps de la réponse de la méthode POST appelée précédemment. Vous pouvez utiliser différentes bibliothèques Azure pour effectuer cette opération sur de nombreuses plateformes, notamment:

    * [Bibliothèque cliente de stockage Azure pour .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Kit de développement logiciel (SDK) Stockage Azure pour Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Kit de développement logiciel (SDK) Stockage Azure pour Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    L’exemple de code suivant en C# montre comment charger une archive ZIP vers le stockage d’objets blob Azure à l’aide de la classe [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) incluse dans la bibliothèque cliente de stockage Azure pour .NET. Cet exemple repose sur le principe que l’archive ZIP a déjà été écrite dans un objet de flux.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. [Validez la soumission d’applications](commit-an-app-submission.md) en exécutant la méthode suivante. Cela vous avertit l’espace partenaires que vous avez terminé avec votre soumission et que vos mises à jour doivent maintenant être appliqués à votre compte.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. Vérifiez l’état de validation en exécutant la méthode suivante pour [obtenir l’état de la soumission d’application](get-status-for-an-app-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la requête aboutit ou à **CommitFailed** si elle contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de la soumission à l’aide de la méthode précédente ou en consultant l’espace partenaires.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>Méthodes de gestion d’un lancement de packages progressif

Vous pouvez publier progressivement les packages mis à jour d’une soumission d’applications pour un pourcentage des clients de votre application sur Windows10. Cela vous permet de surveiller les commentaires et les données d’analyse des packages spécifiques et de vérifier l’adéquation de votre mise à jour avant de la déployer plus largement. Vous pouvez modifier le pourcentage de lancement (ou arrêter la mise à jour) d’une soumission publiée sans avoir à créer une nouvelle soumission. Pour plus d’informations, y compris les instructions pour savoir comment activer et gérer un lancement progressif du package dans l’espace partenaires, voir [cet article](../publish/gradual-package-rollout.md).

Pour activer par programmation un lancement de packages progressif pour une soumission d’apps, suivez cette procédure à l’aide des méthodes de l’API de soumission au MicrosoftStore:

  1. [Créez une soumission d’apps](create-an-app-submission.md) ou [obtenez une soumission d’apps existante](get-an-app-submission.md).
  2. Dans les données de réponse, localisez la ressource [packageRollout](#package-rollout-object), définissez le champ *isPackageRollout* sur **true**, puis définissez le champ *packageRolloutPercentage* sur le pourcentage des clients de votre application qui doivent obtenir les packages mis à jour.
  3. Transmettez les données de soumission d’applications mises à jour à la méthode [Mettre à jour une soumission d’applications](update-an-app-submission.md).

Après avoir activé un lancement de packages progressif pour une soumission d’applications, vous pouvez utiliser les méthodes suivantes pour obtenir, mettre à jour, arrêter ou finaliser le lancement progressif par programmation.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">Obtient des informations sur le lancement progressif d’une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">Met à jour le pourcentage de lancement progressif d’une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">Arrête le lancement progressif d’une soumission d’applications</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">Finalise le lancement progressif d’une soumission d’apps</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>Exemples de code pour gérer des soumissions d’apps

Les articles suivants fournissent des exemples de code détaillés qui montrent comment créer une soumission d’applications dans différents langages de programmation:

* [Exemple de code C#: soumissions d'applications, d'extensions et de versions d’évaluation](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code C#: soumission d’application avec options de jeu et bandes-annonces](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Java: soumissions d'applications, d'extensions et de versions d’évaluation](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemple de code Java: soumission d’applications avec options de jeu et bandes-annonces](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemple de code Python: soumissions d'applications, d'extensions et de versions d’évaluation](python-code-examples-for-the-windows-store-submission-api.md)
* [Exemple Python: soumission d’apps avec des options de jeu et bandes-annonces](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Module StoreBroker PowerShell

Au lieu d'appeler l'API de soumission au MicrosoftStore directement, nous fournissons également un module PowerShell OpenSource qui implémente une interface de ligne de commande sur l’API. Ce module est appelé [StoreBroker](https://aka.ms/storebroker). Vous pouvez utiliser ce module pour gérer les soumissions de votre app, de votre version et de vos modules complémentaires à partir de la ligne de commande, en lieu et place de l'appel direct de l'API de soumission au MicrosoftStore. Sinon, vous pouvez simplement parcourir la source pour consulter des exemples supplémentaires d'appel de cette API. Le module StoreBroker est activement utilisé au sein de Microsoft en tant que vecteur principal de soumission de nombreuses applications internes dans le WindowsStore.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://aka.ms/storebroker).

<span/>

## <a name="data-resources"></a>Ressources de données
Les méthodes de l’API de soumission au MicrosoftStore pour gérer les soumissions d’apps utilisent les ressources de données JSON suivantes.

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>Ressource de soumission d’applications

Cette ressource décrit une soumission d’applications.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
          "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "description": "Main page",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description      |
|------------|--------|-------------------|
| id            | chaîne  | ID de la soumission. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’apps](create-an-app-submission.md), [obtenir toutes les apps](get-all-apps.md) et [obtenir une app](get-an-app.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de la soumission dans l’espace partenaires.  |
| applicationCategory           | chaîne  |   Chaîne qui spécifie la [catégorie et/ou sous-catégorie](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) pour votre application. Les catégories et sous-catégories sont combinées en une seule chaîne à l’aide du caractère trait de soulignement«_», par exemple **BooksAndReference_EReader**.      |  
| pricing           |  objet  | [Ressource de tarification](#pricing-object) qui contient les informations de tarification de l’application.        |   
| visibility           |  chaîne  |  Visibilité de l’application. Les valeurs possibles sont les suivantes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |  
| listings           |   objet  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays et chaque valeur est une [ressource de référencement](#listing-object) qui contient des informations de référencement de l’application.       |   
| hardwarePreferences           |  tableau  |   Tableau de chaînes qui définissent les [préférences matérielles](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) pour votre application. Les valeurs possibles sont les suivantes: <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>NFC</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  booléen  |   Indique si Windows peut inclure les données de votre application dans les sauvegardes automatiques sur OneDrive. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booléen  |   Indique si les clients peuvent installer votre application sur un stockage amovible. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booléen |   Indique si les jeux DVR sont activés pour l’application.    |   
| gamingOptions           |  array |   Tableau contenant une [ressource d’options de jeu](#gaming-options-object) qui définit les paramètres relatifs au jeu pour l’application.     |   
| hasExternalInAppProducts           |     booléen          |   Indique si votre app permet aux utilisateurs d’effectuer des achats hors du système de commerce du Microsoft Store. Pour plus d’informations, voir [Déclarations d’app](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booléen           |  Indique si votre application a fait l’objet de tests pour voir si elle est conforme aux recommandations d’accessibilité. Pour plus d’informations, voir [Déclarations d’application](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  chaîne  |   Contient des [notes de certification](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) pour votre application.    |    
| status           |   chaîne  |  État de la soumission. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   objet  | [Ressource des détails d’état](#status-details-object) qui contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs.       |    
| fileUploadUrl           |   chaîne  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages, images de description ou fichiers de bande-annonce à la soumission, chargez l’archive ZIP qui les contient vers cet URI. Pour plus d’informations, voir [Créer une soumission d’applications](#create-an-app-submission).       |    
| applicationPackages           |   tableau  | Tableau des [ressources de package d’application](#application-package-object) qui fournissent des détails sur chaque package de la soumission. |    
| packageDeliveryOptions    | objet  | [Ressource des options de remise du package](#package-delivery-options-object) qui contient les paramètres de lancement de packages progressif et de mise à jour obligatoire de la soumission.  |
| enterpriseLicensing           |  chaîne  |  Une des [valeur de gestion des licences d’entreprise](#enterprise-licensing) qui indiquent le comportement de la gestion des licences d’entreprise pour l’app.  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  booléen   |  Indique si Microsoft est autorisé à [rendre l’application disponible pour les futures familles d’appareils Windows10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | objet   |  Dictionnaire de paires clé/valeur, où chaque clé est une [famille d’appareils Windows10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) et chaque valeur est une valeur booléenne qui indique si votre application est autorisée à cibler la famille d’appareils spécifiée.     |    
| friendlyName           |   chaîne  |  Le nom convivial de la soumission, comme illustré dans l’espace partenaires. La valeur est générée pour vous lorsque vous créez la soumission.       |  
| trailers           |  tableau |   Tableau contenant jusqu'à 15 [ressources de bande-annonce](#trailer-object) qui représentent les bandes-annonces vidéos de la description de l’app.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>Ressource de tarification

Cette ressource contient des informations de tarification pour l’application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  trialPeriod               |    chaîne     |  Chaîne qui spécifie la période d’évaluation de l’application. Les valeurs possibles sont les suivantes: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    objet     |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre application sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *priceId* du marché spécifié.      |     
|  sales               |   tableau      |  **Deprecated**. Tableau des [ressources de ventes](#sale-object) qui contiennent des informations commerciales pour l’application.   |     
|  priceId               |   chaîne      |  [Niveau de prix](#price-tiers), spécifiant le [prix de base](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) de l’application.   |     
|  isAdvancedPricingModel               |   valeur booléenne      |  Si la valeur **true** est définie, votre compte de développeur dispose d’un accès à la plage étendue de tarification, de 0,99à 1999,99dollars. Si la valeur **false** est définie, votre compte de développeur dispose d’un accès à la plage initiale de tarification, de 0,99à 999,99dollars. Pour plus d’informations sur les différents niveaux, voir [Niveaux de prix](#price-tiers).<br/><br/>**Remarque**&nbsp;&nbsp;Ce champ est en lecture seule.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Ressource Sale

Cette ressource contient des informations commerciales pour une application.

> [!IMPORTANT]
> La ressource **Sale** n’est plus prise en charge, et vous ne pouvez ni obtenir ni modifier les données commerciales concernant la soumission d’une app à l’aide de l’API de soumission au MicrosoftStore. Bientôt, nous allons mettre à jour l’API de soumission au MicrosoftStore pour proposer une nouvelle façon d’accéder par programmation aux informations commerciales concernant les soumissions d’app.
>    * Après avoir appelé la [méthode GET pour soumettre une application](get-an-app-submission.md), la ressource *Sales* est vide. Vous pouvez continuer à utiliser l’espace partenaires pour obtenir les données de vente à prix réduit pour la soumission de votre application.
>    * Lors de l’appel de la [méthode PUT pour mettre à jour la soumission d’une application](update-an-app-submission.md), les informations de la valeur *Sales* sont ignorées. Vous pouvez continuer à utiliser l’espace partenaires pour modifier les données commerciales concernant la soumission de votre application.

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description    |
|-----------------|---------|------|
|  name               |    chaîne     |   Nom de la vente.    |     
|  basePriceId               |   chaîne      |  [Niveau de prix](#price-tiers) à utiliser pour le prix de base de la vente.    |     
|  startDate               |   chaîne      |   Date de début de la vente au format ISO 8601.  |     
|  endDate               |   chaîne      |  Date de fin de la vente au format ISO 8601.      |     
|  marketSpecificPricings               |   objet      |   Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre application sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *basePriceId* du marché spécifié.    |


<span id="listing-object" />

### <a name="listing-resource"></a>Ressource de référencement

Cette ressource contient des informations de référencement d’une application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description                  |
|-----------------|---------|------|
|  baseListing               |   objet      |  Informations de [listing de base](#base-listing-object) pour l’application, qui définissent les informations de listing par défaut pour toutes les plateformes.   |     
|  platformOverrides               | objet |   Dictionnaire de paires clé/valeur, où chaque clé est une chaîne qui identifie une plateforme pour laquelle remplacer les informations de référencement et chaque valeur est une ressource de [référencement de base](#base-listing-object) (contenant uniquement les valeurs de la description au titre) qui spécifie les informations de référencement à remplacer pour la plateforme spécifiée. Les clés peuvent avoir les valeurs suivantes: <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>Ressource de référencement de base

Cette ressource contient des informations de référencement de base pour une application. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   chaîne      |  [Informations de copyright et/ou de marque](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info) facultatives.  |
|  keywords                |  tableau       |  Tableau de [mots clés](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords) facilitant l’apparition de l’application dans les résultats de recherche.    |
|  licenseTerms                |    chaîne     | [Termes du contrat de licence](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms) facultatifs de votre application.     |
|  privacyPolicy                |   chaîne      |   Cette valeur est obsolète. Pour définir ou modifier l’URL de politique de confidentialité de votre application, vous devez le faire dans la page de [Propriétés](../publish/enter-app-properties.md#privacy-policy-url) dans l’espace partenaires. Vous pouvez omettre cette valeur dans vos appels à l’API de soumission. Si vous définissez cette valeur, elle sera ignorée.       |
|  supportContact                |   chaîne      |  Cette valeur est obsolète. Pour définir ou modifier la prise en charge de contact URL ou adresse e-mail de votre application, vous devez le faire dans la page de [Propriétés](../publish/enter-app-properties.md#support-contact-info) dans l’espace partenaires. Vous pouvez omettre cette valeur dans vos appels à l’API de soumission. Si vous définissez cette valeur, elle sera ignorée.        |
|  websiteUrl                |   chaîne      |  Cette valeur est obsolète. Pour définir ou modifier l’URL de la page web de votre application, vous devez le faire dans la page de [Propriétés](../publish/enter-app-properties.md#website) dans l’espace partenaires. Vous pouvez omettre cette valeur dans vos appels à l’API de soumission. Si vous définissez cette valeur, elle sera ignorée.      |    
|  description               |    chaîne     |   [Description](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) du listing de l’application.   |     
|  fonctionnalités               |    tableau     |  Tableau contenant 20chaînes au maximum qui répertorient les [fonctionnalités](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) de votre application.     |
|  releaseNotes               |  chaîne       |  [Notes de publication](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) de votre application.    |
|  images               |   tableau      |  Tableau des ressources d’[image et d’icône](#image-object) de la description de l’application.  |
|  recommendedHardware               |   tableau      |  Tableau contenant jusqu’à 11chaînes qui répertorient les [configurations matérielles recommandées](../publish/create-app-store-listings.md#additional-information) pour votre application.     |
|  minimumHardware               |     chaîne    |  Tableau contenant jusqu’à 11chaînes qui répertorient les [configurations matérielles minimales](../publish/create-app-store-listings.md#additional-information) pour votre application.    |  
|  title               |     chaîne    |   Titre de la description de l’application.   |  
|  shortDescription               |     chaîne    |  Utilisée uniquement pour les jeux. Cette description s’affiche dans la section  **Information** du Hub de jeux sur XboxOne et permet aux clients d’en savoir plus sur votre jeu.   |  
|  shortTitle               |     chaîne    |  Version raccourcie du nom de votre produit. S’il est fourni, ce nom plus court peut apparaître à divers emplacements sur XboxOne (pendant l’installation, dans les succès, etc.) à la place du titre complet de votre produit.    |  
|  sortTitle               |     chaîne    |   Si votre produit peut être trié par ordre alphabétique de différentes manières, vous pouvez entrer une autre version ici. Cela peut aider les clients à trouver plus rapidement le produit qu'ils recherchent.    |  
|  voiceTitle               |     chaîne    |   Autre nom pour votre produit qui, s’il est fourni, peut être utilisé dans l’expérience audio sur XboxOne lors de l’utilisation de Kinect ou d’un casque.    |  
|  devStudio               |     chaîne    |   Spécifiez cette valeur si vous souhaitez inclure un champ **Développé par** dans la description. (Le champ **Publié par** répertorie le nom d’affichage de l'éditeur associé à votre compte, que vous fournissiez ou non une valeur *devStudio*.)    |  

<span id="image-object" />

### <a name="image-resource"></a>Ressource d’image

Cette ressource contient les données d’image et d’icône d’une description d’app. Pour plus d’informations sur les images et icônes pour un listing d'app, voir [Images et captures d’écran de l’app](../publish/app-screenshots-and-images.md). Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description           |
|-----------------|---------|------|
|  fileName               |    chaîne     |   Nom du fichier image dans l’archive ZIP que vous avez chargé pour la soumission.    |     
|  fileStatus               |   chaîne      |  État du fichier image. Les valeurs possibles sont les suivantes: <ul><li>Aucune</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  chaîne  | ID de l'image. Cette valeur est fournie par l’espace partenaires.  |
|  description  |  chaîne  | Description de l’image.  |
|  imageType  |  chaîne  | Indique le type de l'image. Les chaînes suivantes sont actuellement prises en charge. <p/>[Images de capture d’écran](../publish/app-screenshots-and-images.md#screenshots): <ul><li>Capture d’écran (Utilisez cette valeur pour la capture d’écran de bureau)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[Logos Windows Store](../publish/app-screenshots-and-images.md#store-logos):<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>Icône (utilisez cette valeur pour le logo 1:1 300 x 300pixels)</li></ul><p/>[Images promotionnelles](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Images Xbox](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[Images promotionnelles facultatives](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>Ressource d'options de jeu

Cette ressource contient des paramètres relatifs au jeu pour l’application. Les valeurs de cette ressource correspondent aux [paramètres de jeu](../publish/enter-app-properties.md#game-settings) pour les soumissions dans l’espace partenaires.

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  genres               |    tableau     |  Un tableau d’un ou plusieurs des chaînes suivantes qui décrivent les genres du jeu: <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    booléen     |  Indique si le jeu prend en charge le mode multijoueur local.      |     
|  isLocalCooperative               |   booléen      |  Indique si le jeu prend en charge le mode coopération local.    |     
|  isOnlineMultiplayer               |   booléen      |  Indique si le jeu prend en charge le mode multijoueur en ligne.    |     
|  isOnlineCooperative               |   booléen      |  Indique si le jeu prend en charge le mode coopération en ligne.    |     
|  localMultiplayerMinPlayers               |   entier      |   Spécifie le nombre minimal de joueurs que le jeu prend en charge pour le mode multijoueur local.   |     
|  localMultiplayerMaxPlayers               |   entier      |   Spécifie le nombre maximal de joueurs que le jeu prend en charge pour le mode multijoueur local.  |     
|  localCooperativeMinPlayers               |   entier      |   Spécifie le nombre minimal de joueurs que le jeu prend en charge pour le mode coopération local.  |     
|  localCooperativeMaxPlayers               |   entier      |   Spécifie le nombre maximal de joueurs que le jeu prend en charge pour le mode coopération local.  |     
|  isBroadcastingPrivilegeGranted               |   booléen      |  Indique si le jeu prend en charge la diffusion.   |     
|  isCrossPlayEnabled               |   booléen      |   Indique si le jeu prend en charge des sessions multijoueurs entre joueurs sur PC Windows10 et Xbox.  |     
|  kinectDataForExternal               |   chaîne      |  Une des valeurs de chaîne suivantes qui indique si le jeu peut collecter des données de Kinect et les envoyer à des services externes: <ul><li>NotSet</li><li>Inconnu</li><li>Activé</li><li>Désactivé</li></ul>   |

> [!NOTE]
> La ressource *gamingOptions* a été ajoutée en mai2017, après que l'API de soumission au MicrosoftStore a été diffusée pour la première fois auprès des développeurs. Si vous avez créé une soumission pour une app via l’API de soumission avant l'introduction de cette ressource et que cette soumission est toujours en cours, cette ressource sera nulle pour la soumission de l'app tant que vous n'aurez pas validé correctement ou supprimé la soumission. Si la ressource *gamingOptions* n'est pas disponible pour les soumissions d'une app, le champ *hasAdvancedListingPermission* de la [ressource de l'application](get-app-data.md#application_object) retourné par la méthode [obtenir une app](get-an-app.md) sera false.

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource des détails d’état

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description         |
|-----------------|---------|------|
|  errors               |    objet     |   Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’erreur de la soumission.    |     
|  warnings               |   objet      | Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’avertissement de la soumission.      |
|  certificationReports               |     objet    |   Tableau des [ressources de rapport de certification](#certification-report-object) qui donnent accès aux données du rapport de certification de la soumission. Vous pouvez examiner ces rapports pour obtenir plus d’informations en cas d’échec de la certification.   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource des détails d’état

Cette ressource contient des informations supplémentaires sur les éventuels erreurs ou avertissements pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  code               |    chaîne     |   [Code d’état de soumission](#submission-status-code) qui décrit le type d’erreur ou d’avertissement.   |     
|  details               |     chaîne    |  Message contenant des détails sur le problème.     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>Ressource de package d’application

Cette ressource contient des détails sur un package d’application pour la soumission.

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

> [!NOTE]
> Quand vous appelez la méthode de [mise à jour d’une soumission d’applications](update-an-app-submission.md), seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de cet objet sont obligatoires dans le corps de la requête. Les autres valeurs sont renseignées par l’espace partenaires.

| Valeur           | Type    | Description                   |
|-----------------|---------|------|
| fileName   |   chaîne      |  Nom du package.    |  
| fileStatus    | chaîne    |  État du package. Les valeurs possibles sont les suivantes: <ul><li>Aucune</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  chaîne   |  ID qui identifie de manière unique le package. Cette valeur est fournie par l’espace partenaires.   |     
| version    |  chaîne   |  Version du package d’application. Pour plus d’informations, voir [Numérotation des versions de packages](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  chaîne   |  Architecture du package (par exemple, ARM).   |     
| languages    | tableau    |  Tableau des codes des langues prises en charge par l’application. Pour plus d’informations, consultez la page [Langues prises en charge](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  tableau   |  Tableau des fonctionnalités exigées par le package. Pour plus d’informations sur les fonctionnalités, voir [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  chaîne   |  Version DirectX minimale prise en charge par le package d’app. Cela peut être défini uniquement pour les apps qui ciblent Windows8. x. Concernant les apps qui ciblent d’autres versions du système d’exploitation, cette valeur doit être présente lors de l’appel de la méthode [mettre à jour une soumission d’apps](update-an-app-submission.md), mais la valeur que vous spécifiez sera ignorée. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | chaîne    |  Mémoire RAM minimale exigée par le package d’app. Cela peut être défini uniquement pour les apps qui ciblent Windows8. x. Concernant les apps qui ciblent d’autres versions du système d’exploitation, cette valeur doit être présente lors de l’appel de la méthode [mettre à jour une soumission d’apps](update-an-app-submission.md), mais la valeur que vous spécifiez sera ignorée. Les valeurs possibles sont les suivantes: <ul><li>Aucune</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | tableau    |  Tableau de chaînes qui représentent les familles d’appareils que le package cible. Cette valeur est utilisée uniquement pour les packages qui ciblent Windows10. Pour les packages qui ciblent des versions antérieures, cette valeur est **None**. Les chaînes de familles d’appareils suivantes sont actuellement prises en charge pour les packages Windows10, où *{0}* est une chaîne de version de Windows10 comme 10.0.10240.0, 10.0.10586.0 ou 10.0.14393.0: <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource de rapport de certification

Cette ressource donne accès aux données du rapport de certification d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description             |
|-----------------|---------|------|
|     date            |    chaîne     |  Date et heure de que génération du rapport, au format ISO 8601.    |
|     reportUrl            |    chaîne     |  URL vous permettant d’accéder au rapport.    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Ressource des options de remise du package

Cette ressource contient les paramètres de lancement de packages progressif et de mise à jour obligatoire de la soumission.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| packageRollout   |   objet      |  [Ressource de lancement de packages](#package-rollout-object) qui contient les paramètres de lancement de packages progressif de la soumission.   |  
| isMandatoryUpdate    | booléen    |  Indique si vous souhaitez traiter les packages de cette soumission comme obligatoires pour l’installation automatique des mises à jour de l’application. Pour plus d’informations sur les packages obligatoires pour l’installation automatique des mises à jour de l’application, consultez [Télécharger et installer les mises à jour de package pour votre application](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  Date et heure auxquelles les packages de cette soumission deviennent obligatoires, au format ISO8601 dans le fuseau horaire UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Ressource de lancement de packages

Cette ressource contient les [paramètres de lancement de packages](#manage-gradual-package-rollout) progressif de la soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| isPackageRollout   |   booléen      |  Indique si le déploiement de package progressif est activé pour la soumission.    |  
| packageRolloutPercentage    | flottant    |  Pourcentage d’utilisateurs qui recevront les packages de déploiement progressif.    |  
| packageRolloutStatus    |  chaîne   |  Une des chaînes suivantes qui indique l’état de déploiement de package progressif: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  chaîne   |  ID de la soumission qui sera reçue par les clients qui ne récupèrent pas les packages de lancement progressif.   |          

> [!NOTE]
> Les valeurs *packageRolloutStatus* et *fallbackSubmissionId* sont attribuées par l’espace partenaires et ne sont pas censées être définies par le développeur. Si vous incluez ces valeurs dans un corps de requête, celles-ci seront ignorées.

<span id="trailer-object" />

### <a name="trailers-resource"></a>Ressource de bande-annonce

Cette ressource représente une vidéo de bande-annonce pour la description de l’app. Les valeurs de cette ressource correspondent aux options [bandes-annonces](../publish/app-screenshots-and-images.md#trailers) des soumissions dans l’espace partenaires.

Vous pouvez ajouter jusqu'à 15ressources de bande-annonce pour le tableau *trailers* dans une [ressource de soumission d’applications](#app-submission-object). Pour télécharger des fichiers vidéo de bande-annonce et des images miniatures pour une soumission, ajoutez ces fichiers à l'archive ZIP qui contient les packages et les images de description pour la soumission, puis téléchargez cette archive ZIP dans l'URI de la signature d’accès partagé (SAS) de la soumission. Pour plus d’informations sur le téléchargement de l’archive ZIP sur l’URI SAS, voir [créer une soumission d’apps](#create-an-app-submission).

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
|  id               |    chaîne     |   ID de la bande-annonce. Cette valeur est fournie par l’espace partenaires.   |
|  videoFileName               |    chaîne     |    Nom du fichier vidéo de bande-annonce dans l’archive ZIP qui contient les fichiers pour la soumission.    |     
|  videoFileId               |   chaîne      |  ID du fichier vidéo de bande-annonce. Cette valeur est fournie par l’espace partenaires.   |     
|  trailerAssets               |   objet      |  Dictionnaire de paires clé/valeur, où chaque clé est un code de langue et chaque valeur est une [ressource de références de bande-annonce](#trailer-assets-object) qui contient des références supplémentaires spécifiques aux paramètres régionaux pour la bande-annonce. Pour plus d’informations sur les codes de langue pris en charge, voir [Langues prises en charge](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     

> [!NOTE]
> La ressource *trailers* a été ajoutée en mai2017, après que l'API de soumission au MicrosoftStore a été diffusée pour la première fois auprès des développeurs. Si vous avez créé une soumission pour une app via l’API de soumission avant l'introduction de cette ressource et que cette soumission est toujours en cours, cette ressource sera nulle pour la soumission de l'app tant que vous n'aurez pas validé correctement ou supprimé la soumission. Si la ressource *trailers* n'est pas disponible pour les soumissions d'une app, le champ *hasAdvancedListingPermission* de la [ressource de l'application](get-app-data.md#application_object) retourné par la méthode [obtenir une app](get-an-app.md) sera false.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>Ressource de références de bande-annonce

Cette ressource contient des références supplémentaires spécifiques aux paramètres régionaux pour une bande-annonce définie dans une [ressource de bande-annonce](#trailer-object). Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description        |
|-----------------|---------|------|
| title   |   chaîne      |  Titre localisé de la bande-annonce. Le titre s'affiche lorsque l’utilisateur lit la bande-annonce en mode plein écran.     |  
| imageList    | tableau    |   Tableau contenant une ressource [image](#image-for-trailer-object) qui fournit l’image miniature pour la bande-annonce. Vous ne pouvez inclure qu'une seule ressource [image](#image-for-trailer-object) dans ce tableau.  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>Ressources image (pour une bande-annonce)

Cette ressource décrit l’image miniature d'une bande-annonce. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description           |
|-----------------|---------|------|
|  fileName               |    chaîne     |   Nom du fichier d'image miniature dans l’archive ZIP que vous avez chargée pour la soumission.    |     
|  id  |  chaîne  | ID de l'image miniature. Cette valeur est fournie par l’espace partenaires.  |
|  description  |  chaîne  | Description de l’image miniature. Cette valeur est en métadonnées uniquement et n'apparaît pas pour les utilisateurs.   |

<span/>

## <a name="enums"></a>Énumérations

Ces méthodes utilisent les énumérations suivantes.

<span id="price-tiers" />

### <a name="price-tiers"></a>Niveaux de prix

Les valeurs suivantes représentent les niveaux de prix disponibles dans la [ressource de tarification](#pricing-object) pour une soumission d’application.

| Valeur           | Description        |
|-----------------|------|
|  Base               |   Le niveau de prix n’est pas défini; utilisez le prix de base de l’application.      |     
|  NotAvailable              |   L’application n’est pas disponible dans la région spécifiée.    |     
|  Free              |   L’application est gratuite.    |    
|  Tier*xxxx*               |   Une chaîne spécifiant le niveau de prix d’une application, au format **Tier<em>xxxx</em>**. Actuellement, les plages suivantes de tarification sont prises en charge:<br/><br/><ul><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **true**, les valeurs de tarification disponibles pour votre compte sont **Tier1012** - **Tier1424**.</li><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **false**, les valeurs de tarification disponibles pour votre compte sont **Tier2** - **Tier96**.</li></ul>Pour voir le tableau complet des prix niveaux qui sont disponibles pour votre compte de développeur, y compris les prix spécifiques au marché qui sont associés à chaque niveau, accédez à la page **tarification et disponibilité** pour un de vos soumissions d’applications dans l’espace partenaires et Cliquez sur le lien **Afficher la table** dans la section **marchés et prix personnalisés** (pour certains comptes développeur, ce lien est dans la section **tarification** ).    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>Valeurs de gestion des licences d’entreprise

Les valeurs suivantes représentent le comportement de gestion des licences organisationnelles adopté pour l’app. Pour plus d’informations sur ces options, voir [Options de gestion des licences organisationnelles](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

> [!NOTE]
> Vous pouvez configurer les options de gestion des licences organisationnelles pour une soumission d’apps via l’API de soumission, toutefois vous ne pouvez pas utiliser cette API pour publier des soumissions pour [les achats en volume par le biais du MicrosoftStore pour Entreprises et MicrosoftStore pour Éducation](../publish/organizational-licensing.md). Pour publier des soumissions au Microsoft Store pour entreprises et Microsoft Store pour éducation, vous devez utiliser l’espace partenaires.


| Valeur           |  Description      |
|-----------------|---------------|
| None            |     Ne mets pas votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne).         |     
| Online        |     Mets votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne).  |
| OnlineAndOffline | Mets votre application à la disposition des entreprises dont les licences en volume sont gérées par le Windows Store (en ligne) et à la disposition des entreprises par le biais de l’achat de licences en mode hors connexion. |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>Code d’état de soumission

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

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir des données d’app à l’aide de l’API de soumission au MicrosoftStore](get-app-data.md)
* [Soumissions d’applications dans l’espace partenaires](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)
