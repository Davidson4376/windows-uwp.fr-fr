---
author: Xansky
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Utilisez ces méthodes dans l’API de soumission au MicrosoftStore pour gérer les soumissions d’extensions des apps qui sont inscrites dans votre compte du Centre de développement Windows.
title: Gérer les soumissions de modules complémentaires
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, API de soumission au MicrosoftStore, soumissions d'extension, produit dans l'app, FAI
ms.localizationpriority: medium
ms.openlocfilehash: 78c4f1603b921320009305e9646a7227ff47e8e3
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5518585"
---
# <a name="manage-add-on-submissions"></a>Gérer les soumissions de modules complémentaires

L’API de soumission au MicrosoftStore fournit des méthodes qui permettent de gérer les soumissions d’extensions (également connues sous le nom PIA ou produits in-app) pour vos apps. Pour obtenir une présentation de l’API de soumission au MicrosoftStore, notamment les conditions préalables à l’utilisation de l’API, voir [Créer et gérer des soumissions à l’aide des services au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si vous utilisez l’API de soumission au MicrosoftStore pour créer une soumission pour un module complémentaire, vous devrez employer exclusivement l’API pour apporter d’autres modifications à la soumission, sans passer par le tableau de bord du Centre de développement. Si vous passez par le tableau de bord pour modifier une soumission initialement créée via l'API, vous ne pourrez plus modifier ou valider cette soumission à l'aide de l'API. Dans certains cas, la soumission non validée peut rester définie sur l'état d'erreur. Si cela se produit, vous devez supprimer la soumission et en créer une nouvelle.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>Méthodes de gestion des soumissions d’extensions

Pour obtenir, créer, mettre à jour, valider ou supprimer une soumission d’extension, utilisez les méthodes ci-dessous. Pour pouvoir utiliser ces méthodes, vous devez disposer de l’extension dans votre compte du Centre de développement. Vous pouvez créer une extension dans le tableau de bord, en [définissant son type et son ID de produit](../publish/set-your-add-on-product-id.md) ou en utilisant les méthodes de l’API de soumission au MicrosoftStore, tel que décrit sur la page [Gérer les extensions](manage-add-ons.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">Obtient une soumission d’extension existante</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">Obtient l’état d’une soumission d’extension existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">Crée une soumission d’extension</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">Met à jour une soumission d’extension existante</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">Valide une soumission d’extension nouvelle ou mise à jour</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">Supprime une soumission d’extension</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>Crée une soumission d’extension

Pour créer une soumission pour une extension, suivez ce processus.

1. Si vous ne l’avez pas encore fait, remplissez les conditions préalables décrites dans [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md), notamment l’association d’une application AzureAD à votre compte du Centre de développement Windows et l’obtention de votre ID client et de votre clé. Vous n’aurez à le faire qu’une seule fois. Une fois à votre disposition, l’ID client et la clé sont réutilisables chaque fois que vous avez besoin de créer un jeton d’accès AzureAD.  

2. [Obtenir un jeton d’accès AzureAD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Vous devez transmettre ce jeton d’accès aux méthodes de l’API de soumission au MicrosoftStore. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

3. Exécutez la méthode suivante de l’API de soumission au MicrosoftStore. Cette méthode crée une soumission en cours, qui est une copie de votre dernière soumission publiée. Pour plus d’informations, voir [Créer une soumission d’extension](create-an-add-on-submission.md).

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    Le corps de la réponse contient une ressource de [soumission d'extension](#add-on-submission-object) qui inclut l'ID de la nouvelle soumission, l’URI de signature d’accès partagé (SAS) pour le chargement de toutes les icônes d’extension de la soumission vers le Stockage Blob Azure, ainsi que toutes les données de la nouvelle soumission (notamment les descriptions et les informations tarifaires).

    > [!NOTE]
    > Un URI SAS permet d’accéder à une ressource sécurisée dans le stockage Azure sans besoin de clés de compte. Pour obtenir des informations générales sur les URI SAS et leur utilisation avec le stockage d’objets blob Azure, consultez [Signatures d’accès partagé, partie1: présentation du modèle SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) et [Signatures d’accès partagé, partie2: créer et utiliser une SAS avec le stockage d’objets blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si vous ajoutez de nouvelles icônes pour la soumission, [préparez-les](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon) et ajoutez-les à une archive ZIP.

5. Mettez à jour les données de la [soumission d'extension](#add-on-submission-object) avec toutes les modifications requises pour la nouvelle soumission et lancez la méthode suivante pour mettre à jour la soumission. Pour plus d’informations, voir [Mettre à jour une soumission d’extension](update-an-add-on-submission.md).

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si vous ajoutez de nouvelles icônes pour la soumission, assurez-vous de mettre à jour les données de la soumission pour faire référence au nom et au chemin relatif de ces fichiers dans l’archive ZIP.

4. Si vous ajoutez de nouvelles icônes pour la soumission, chargez l’archiveZIP dans le [stockage d’objets blob Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) à l’aide de l’URI SAS fourni dans le corps de la réponse de la méthode POST appelée précédemment. Vous pouvez utiliser différentes bibliothèques Azure pour effectuer cette opération sur de nombreuses plateformes, notamment:

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

5. Validez la soumission en exécutant la méthode suivante. Le Centre de développement est ainsi informé que vous avez terminé votre soumission et que vos mises à jour doivent être appliqués à votre compte. Pour plus d’informations, voir [Valider une soumission d’extension](commit-an-add-on-submission.md).

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. Vérifiez l’état de validation en exécutant la méthode suivante. Pour plus d’informations, voir [Obtenir l’état d’une soumission d’extension](get-status-for-an-add-on-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    Pour vérifier l’état de la soumission, examinez la valeur *status* dans le corps de la réponse. Cette valeur doit passer de **CommitStarted** à **PreProcessing** si la requête aboutit ou à **CommitFailed** si elle contient des erreurs. S’il existe des erreurs, le champ *statusDetails* contient d’autres détails s’y rapportant.

7. Une fois la validation correctement terminée, la soumission est envoyée au Windows Store en vue de son intégration. Vous pouvez continuer à surveiller la progression de la soumission à l’aide de la méthode précédente ou en consultant le tableau de bord du Centre de développement.

<span/>

## <a name="code-examples"></a>Exemples de code

Les articles suivants fournissent des exemples de code détaillés qui montrent comment créer une soumission d’extension dans différents langages de programmation:

* [Exemples de code C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemples de code Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>module StoreBroker PowerShell

Au lieu d'appeler l'API de soumission au MicrosoftStore directement, nous fournissons également un module PowerShell OpenSource qui implémente une interface de ligne de commande sur l’API. Ce module est appelé [StoreBroker](https://aka.ms/storebroker). Vous pouvez utiliser ce module pour gérer les soumissions de votre app, de votre version et de vos modules complémentaires à partir de la ligne de commande, en lieu et place de l'appel direct de l'API de soumission au MicrosoftStore. Sinon, vous pouvez simplement parcourir la source pour consulter des exemples supplémentaires d'appel de cette API. Le module StoreBroker est activement utilisé au sein de Microsoft en tant que vecteur principal de soumission de nombreuses applications internes dans le WindowsStore.

Pour plus d’informations, consultez notre [page StoreBroker sur GitHub](https://aka.ms/storebroker).

<span/>

## <a name="data-resources"></a>Ressources de données

Les méthodes de l’API de soumission au MicrosoftStore pour gérer les soumissions d’extensions utilisent les ressources de données JSON suivantes.

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>Ressource de soumission d’extension

Cette ressource décrit une soumission d’extension.

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

Cette ressource a les valeurs suivantes.

| Valeur      | Type   | Description        |
|------------|--------|----------------------|
| id            | chaîne  | ID de la soumission. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’extension](create-an-add-on-submission.md), [obtenir toutes les extensions](get-all-add-ons.md) et [obtenir une extension](get-an-add-on.md). Concernant une soumission qui a été créée dans le tableau de bord du centre de développement, cet ID est également disponible dans l’URL de la page de la soumission dans le tableau de bord.  |
| contentType           | chaîne  |  [Type de contenu](../publish/enter-add-on-properties.md#content-type) qui est fourni dans l’extension. Les valeurs possibles sont les suivantes: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | tableau  | Tableau de chaînes qui contiennent jusqu’à 10[motsclés](../publish/enter-add-on-properties.md#keywords) pour l’extension. Votre application peut rechercher des extensions à l’aide de ces motsclés.   |
| lifetime           | chaîne  |  Durée de vie de l’extension. Les valeurs possibles sont les suivantes: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | objet  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deuxlettres ISO3166-1 alpha-2, et chaque valeur est un objet de [ressource de référencement](#listing-object) qui contient les informations de référencement de l’extension.  |
| pricing           | objet  | [Ressource de tarification](#pricing-object) qui contient les informations de tarification de l’extension.   |
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| tag           | chaîne  |  [Données développeur personnalisées](../publish/enter-add-on-properties.md#custom-developer-data) de l’extension (ces informations étaient précédemment appelées *tag*).   |
| visibility  | chaîne  |  Visibilité de l’extension. Les valeurs possibles sont les suivantes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | chaîne  |  État de la soumission. Les valeurs possibles sont les suivantes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objet  |  [Ressource des détails d’état](#status-details-object) qui contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs. |
| fileUploadUrl           | chaîne  | URI de la signature d’accès partagé (SAS) pour le chargement des packages de la soumission. Si vous ajoutez de nouveaux packages à la soumission, chargez l’archive ZIP contenant les packages vers cet URI. Pour plus d’informations, voir [Créer une soumission d’extension](#create-an-add-on-submission).  |
| friendlyName  | chaîne  |  Le nom convivial de la soumission, tel que représenté dans le tableau de bord du Centre de développement. La valeur est générée pour vous lorsque vous créez la soumission.  |

<span id="listing-object" />

### <a name="listing-resource"></a>Ressource de référencement

Cette ressource contient des  [informations de référencement pour une extension](../publish/create-add-on-store-listings.md). Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  description               |    chaîne     |   Description du listing d’extensions.   |     
|  icon               |   objet      |[Ressource d’icône](#icon-object) qui contient les données de l’icône du listing d’extensions.    |
|  title               |     chaîne    |   Titre du listing d’extensions.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>Ressource d’icône

Cette ressource contient les données d’icône du listing d’extensions. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description     |
|-----------------|---------|------|
|  fileName               |    chaîne     |   Nom du fichier d’icône dans l’archive ZIP que vous avez chargé pour la soumission. L'icône doit prendre la forme d’un fichier PNG de 300x300pixels exactement.   |     
|  fileStatus               |   chaîne      |  État du fichier d’icône. Les valeurs possibles sont les suivantes: <ul><li>Aucune</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>Ressource de tarification

Cette ressource contient des informations de tarification pour l’extension. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description    |
|-----------------|---------|------|
|  marketSpecificPricings               |    objet     |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre extension sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *priceId* du marché spécifié.     |     
|  sales               |   tableau      |  **Deprecated**. Tableau des [ressources de ventes](#sale-object) qui contiennent des informations commerciales pour l’extension.     |     
|  priceId               |   chaîne      |  [Niveau de prix](#price-tiers) spécifiant le [prix de base](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price) de l’extension.    |    
|  isAdvancedPricingModel               |   valeur booléenne      |  Si la valeur **true** est définie, votre compte de développeur dispose d’un accès à la plage étendue de tarification, de 0,99à 1999,99dollars. Si la valeur **false** est définie, votre compte de développeur dispose d’un accès à la plage initiale de tarification, de 0,99à 999,99dollars. Pour plus d’informations sur les différents niveaux, voir [Niveaux de prix](#price-tiers).<br/><br/>**Remarque**&nbsp;&nbsp;Ce champ est en lecture seule.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Ressource Sale

Cette ressource contient des informations commerciales sur une extension.

> [!IMPORTANT]
> La ressource **Sale** n’est plus prise en charge, et vous ne pouvez ni obtenir ni modifier les données commerciales concernant la soumission d’une extension à l’aide de l’API de soumission au MicrosoftStore. À l’avenir, nous allons mettre à jour l’API de soumission au MicrosoftStore pour proposer une nouvelle façon d’accéder par programmation aux informations commerciales concernant la soumission de modules complémentaires.
>    * Après l’appel de la [méthode GET pour soumettre un module complémentaire](get-an-add-on-submission.md), la ressource *Sales* est vide. Vous pouvez toujours utiliser le tableau de bord du Centre de développement pour obtenir les données commerciales concernant la soumission de votre module complémentaire.
>    * Lors de l’appel de la [méthode PUT pour mettre à jour la soumission d’un module complémentaire](update-an-add-on-submission.md), les informations de la valeur *Sales* sont ignorées. Vous pouvez toujours utiliser le tableau de bord du Centre de développement pour changer les données commerciales concernant la soumission de modules complémentaires.

Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description           |
|-----------------|---------|------|
|  name               |    chaîne     |   Nom de la vente.    |     
|  basePriceId               |   chaîne      |  [Niveau de prix](#price-tiers) à utiliser pour le prix de base de la vente.    |     
|  startDate               |   chaîne      |   Date de début de la vente au format ISO 8601.  |     
|  endDate               |   chaîne      |  Date de fin de la vente au format ISO 8601.      |     
|  marketSpecificPricings               |   objet      |   Dictionnaire de paires clé/valeur, où chaque clé est un code de pays à deux lettres ISO 3166-1 alpha-2 et chaque valeur est un [niveau de prix](#price-tiers). Ces éléments représentent les [prix personnalisés de votre extension sur des marchés spécifiques](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess). Tous les éléments de ce dictionnaire remplacent le prix de base spécifié par la valeur *basePriceId* du marché spécifié.    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Ressource des détails d’état

Cette ressource contient des détails supplémentaires sur l’état d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description       |
|-----------------|---------|------|
|  errors               |    objet     |   Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’erreur de la soumission.   |     
|  warnings               |   objet      | Tableau des [ressources des détails d’état](#status-detail-object) qui contiennent les détails d’avertissement de la soumission.     |
|  certificationReports               |     objet    |   Tableau des [ressources de rapport de certification](#certification-report-object) qui donnent accès aux données du rapport de certification de la soumission. Vous pouvez examiner ces rapports pour obtenir plus d’informations en cas d’échec de la certification.    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Ressource des détails d’état

Cette ressource contient des informations supplémentaires sur les éventuels erreurs ou avertissements pour une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description    |
|-----------------|---------|------|
|  code               |    chaîne     |   [Code d’état de soumission](#submission-status-code) qui décrit le type d’erreur ou d’avertissement.   |     
|  details               |     chaîne    |  Message contenant des détails sur le problème.     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Ressource de rapport de certification

Cette ressource donne accès aux données du rapport de certification d’une soumission. Cette ressource a les valeurs suivantes.

| Valeur           | Type    | Description               |
|-----------------|---------|------|
|     date            |    chaîne     |  Date et heure de génération du rapport, au format ISO8601.    |
|     reportUrl            |    chaîne     |  URL vous permettant d’accéder au rapport.    |

## <a name="enums"></a>Énumérations

Ces méthodes utilisent les énumérations suivantes.

<span id="price-tiers" />

### <a name="price-tiers"></a>Niveaux de prix

Les valeurs suivantes représentent les niveaux de prix disponibles dans la [ressource de tarification](#pricing-object) d’une soumission d’extension.

| Valeur           | Description       |
|-----------------|------|
|  Base               |   Le niveau de prix n’est pas défini; utilisez le prix de base de l’extension.      |     
|  NotAvailable              |   L’extension n’est pas disponible dans la région spécifiée.    |     
|  Free              |   L’extension est gratuite.    |    
|  Tier*xxxx*               |   Une chaîne spécifiant le niveau de prix d’une extension, au format **Tier<em>xxxx</em>**. Actuellement, les plages suivantes de tarification sont prises en charge:<br/><br/><ul><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **true**, les valeurs de tarification disponibles pour votre compte sont **Tier1012** - **Tier1424**.</li><li>Si la valeur *isAdvancedPricingModel* de la [ressource de tarification](#pricing-object) est **false**, les valeurs de tarification disponibles pour votre compte sont **Tier2** - **Tier96**.</li></ul>Pour consulter le tableau complet des niveaux de prix disponibles pour votre compte de développeur, notamment les prix du marché associés à chaque niveau, accédez à la page **Tarification et disponibilité** des soumissions d’applications dans le tableau de bord du Centre de développement, puis cliquez sur le lien d’**affichage de table** de la section **Marchés et prix personnalisés** (pour certains comptes développeur, ce lien se trouve dans la section **Tarification**).     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Code d’état de soumission

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

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les extensions à l’aide de l’API de soumission au MicrosoftStore](manage-add-ons.md)
* [Soumissions d’extensions dans le tableau de bord du Centre de développement](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)
