---
ms.assetid: CAC6A7C7-3348-4EC4-8327-D47EB6E0C238
title: Accéder à la carte SD
description: Vous pouvez stocker des données non essentielles et y accéder sur une carte microSD en option, plus particulièrement sur les appareils mobiles à faible coût dont le stockage interne est limité.
ms.date: 03/08/2017
ms.topic: article
keywords: windows10, uwp, cartesd, stockage
ms.localizationpriority: medium
ms.openlocfilehash: 9ef97ed489f2dc35aece83821633a583dfba77e2
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8742264"
---
# <a name="access-the-sd-card"></a>Accéder à la carte SD



Vous pouvez stocker des données non essentielles et y accéder sur une carte microSD en option, plus particulièrement sur les appareils mobiles à faible coût dont le stockage interne est limité et qui disposent d’un emplacement pour les cartes de ce type.

Dans la plupart des cas, vous devez spécifier la fonctionnalité **removableStorage** dans le fichier manifeste de l’application pour que votre application puisse stocker des fichiers sur la carte SD et y accéder. En général, vous devez également inscrire les types de fichier stockés et accessibles que votre application peut gérer.

Utilisez les moyens suivants pour stocker des fichiers sur la carteSD en option et y accéder:
- Sélecteurs de fichiers.
- API [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

## <a name="what-you-can-and-cant-access-on-the-sd-card"></a>Éléments accessibles et non accessibles sur la carte SD

### <a name="what-you-can-access"></a>Ce à quoi vous pouvez accéder

- Votre application peut uniquement lire et écrire des fichiers dont le type a été inscrit à des fins de gestion dans le fichier manifeste de l’application.
- Votre application peut également créer et gérer des dossiers.

### <a name="what-you-cant-access"></a>Ce à quoi vous ne pouvez pas accéder

- Votre application ne peut pas voir les dossiers système et les fichiers qu’ils contiennent, ni y accéder.
- Votre application ne peut pas voir les fichiers marqués de l’attribut Hidden (masqué). L’attribut Hidden sert généralement à réduire le risque de suppression accidentelle de données.
- Votre application ne peut pas voir la bibliothèque de documents ni y accéder à l’aide de la propriété [**KnownFolders.DocumentsLibrary**](https://msdn.microsoft.com/library/windows/apps/br227152). En revanche, vous pouvez accéder à la bibliothèque de documents sur la carte SD en parcourant le système de fichiers.

## <a name="security-and-privacy-considerations"></a>Considérations liées à la sécurité et à la confidentialité

Quand une application enregistre des fichiers à un emplacement global sur la carte SD, ces fichiers n’étant pas chiffrés, ils sont en général accessibles à d’autres applications.

- Tant que la carte SD se trouve dans l’appareil, vos fichiers sont accessibles à d’autres applications dès lors que celles-ci se sont inscrites pour gérer les types de fichiers correspondants.
- Quand vous retirez la carte SD de l’appareil et que vous l’ouvrez à partir d’un ordinateur, vos fichiers sont visibles dans l’Explorateur de fichiers et accessibles aux autres applications.

Lorsqu’une application installée sur la carte SD enregistre des fichiers dans son élément [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621), ces fichiers sont chiffrés et ne sont pas accessibles aux autres applications.

## <a name="requirements-for-accessing-files-on-the-sd-card"></a>Exigences relatives à l’accès aux fichiers sur la carte SD

Pour accéder aux fichiers sur la carte SD, vous devez généralement spécifier les éléments suivants.

1.  Vous devez spécifier la capacité **removableStorage** dans le fichier manifeste de l’application.
2.  À des fins de gestion, vous devez aussi inscrire les extensions de fichier associées au type de média auquel vous voulez accéder.

Vous pouvez également utiliser la méthode ci-dessus pour accéder aux fichiers multimédias figurant sur la carte SD sans référencer de dossier connu comme **KnownFolders.MusicLibrary**, ou pour accéder aux fichiers multimédias stockés hors des dossiers de bibliothèque multimédia.

Pour accéder aux fichiers multimédias stockés dans les bibliothèques multimédias (Musique, Photos ou Vidéos) en utilisant les dossiers connus, il suffit de spécifier la fonctionnalité associée dans le fichier manifeste de l’application : **musicLibrary**, **picturesLibrary**, ou **videoLibrary**. Vous n’avez pas besoin de spécifier la fonctionnalité **removableStorage**. Pour plus d’informations, voir [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

## <a name="accessing-files-on-the-sd-card"></a>Accès aux fichiers sur la carte SD

### <a name="getting-a-reference-to-the-sd-card"></a>Obtention d’une référence à la carte SD

Le dossier [**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158) correspond à la classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) de la racine logique de l’ensemble des appareils amovibles actuellement connectés à l’appareil. Si une carte SD est présente, la première (et unique) classe **StorageFolder** sous le dossier **KnownFolders.RemovableDevices** représente la carte SD.

Utilisez du code semblable au suivant pour déterminer si une carte SD est présente et obtenir une référence à celle-ci en tant que classe [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230).

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    // An SD card is present and the sdCard variable now contains a reference to it.
}
else
{
    // No SD card is present.
}
```

> [!NOTE]
> Si votre lecteur de carte SD est intégré (emplacement dans l’ordinateur portable ou le PC), il peut ne pas être accessible via KnownFolders.RemovableDevices.

### <a name="querying-the-contents-of-the-sd-card"></a>Interrogation du contenu de la carte SD

La carte SD peut contenir de nombreux dossiers et fichiers qui ne sont pas identifiés en tant que dossiers connus et qui ne peuvent pas être interrogés à l’aide d’un emplacement issu de l’élément [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151). Pour rechercher des fichiers, votre application doit énumérer le contenu de la carte en balayant le système de fichiers de manière récursive. Utilisez les éléments [**GetFilesAsync (CommonFileQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227274) et [**GetFoldersAsync (CommonFolderQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227281) pour obtenir efficacement le contenu de la carte SD.

Nous vous recommandons d’utiliser un thread d’arrière-plan pour balayer la carte SD. Une carte SD peut contenir un grand nombre de gigaoctets de données.

Votre application peut également inviter l’utilisateur à choisir des dossiers spécifiques à l’aide du sélecteur de dossiers.

Quand vous accédez au système de fichiers de la carte SD par un chemin d’accès dérivé de l’élément [**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158), les méthodes se comportent de la manière suivante.

-   La méthode [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227273) renvoie les extensions de fichier que vous avez inscrites à des fins de gestion et les extensions de fichier associées aux fonctionnalités de bibliothèque multimédia que vous avez spécifiées.
-   La méthode [**GetFileFromPathAsync**](https://msdn.microsoft.com/library/windows/apps/br227206) échoue si vous n’avez pas inscrit l’extension du fichier auquel vous essayez d’accéder.

## <a name="identifying-the-individual-sd-card"></a>Identification de la carte SD individuelle

Lors du montage initial de la carte SD, le système d’exploitation génère un identificateur unique (ID) pour elle. Il stocke cet ID dans un fichier du dossier WPSystem à la racine de la carte. Une application peut utiliser cet ID pour déterminer si elle reconnaît la carte. Si une application reconnaît la carte, elle peut être capable de différer certaines opérations qui ont été accomplies auparavant. Cependant, le contenu de la carte peut avoir changé depuis le dernier accès à la carte par l’application.

Prenons l’exemple d’une application qui indexe des livres électroniques. Si l’application a analysé toute la carte SD au préalable pour rechercher des fichiers de livres électroniques, puis créé un index de ces livres, elle peut en afficher immédiatement la liste si la carte est réinsérée et que l’application la reconnaît. Par ailleurs, elle peut démarrer un thread d’arrière-plan de faible priorité pour rechercher les nouveaux livres électroniques. Elle peut également gérer l’échec d’une recherche de livre électronique précédemment existant quand l’utilisateur essaie d’accéder à ce livre supprimé.

Le nom de la propriété qui contient cet ID est **WindowsPhone.ExternalStorageId**.

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    var allProperties = sdCard.Properties;
    IEnumerable<string> propertiesToRetrieve = new List<string> { "WindowsPhone.ExternalStorageId" };

    var storageIdProperties = await allProperties.RetrievePropertiesAsync(propertiesToRetrieve);

    string cardId = (string)storageIdProperties["WindowsPhone.ExternalStorageId"];

    if (...) // If cardID matches the cached ID of a recognized card.
    {
        // Card is recognized. Index contents opportunistically.
    }
    else
    {
        // Card is not recognized. Index contents immediately.
    }
}
```

 

 
