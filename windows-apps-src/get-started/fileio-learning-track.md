---
title: Utiliser des fichiers
description: Découvrez comment utiliser des fichiers dans la plateforme Windows universelle.
ms.date: 05/01/2018
ms.topic: article
keywords: prise en main, uwp, windows 10, piste d’apprentissage, fichiers, e/s de fichier, lire un fichier, écrire un fichier, créer un fichier, écrire du texte, lire du texte
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 29cfeef852f240548f1cd961f73766346da7afa4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321132"
---
# <a name="work-with-files"></a>Utiliser des fichiers

Cette rubrique décrit ce que vous devez savoir pour commencer la lecture et l’écriture dans des fichiers dans une application de plateforme Windows universelle (UWP). Les API et les types principaux sont présentés et des liens sont fournis pour vous aider à en savoir plus.

Il ne s’agit pas d’un didacticiel. Si vous souhaitez un didacticiel, consultez [créer, écrire et lire un fichier](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) qui, en plus de montrer comment créer, lire et écrire un fichier, explique comment utiliser des flux et des mémoires tampons. Vous pouvez également être intéressé par l’[exemple d’accès aux fichiers](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) qui montre comment créer, lire, écrire, copier et supprimer un fichier, mais aussi comment récupérer les propriétés de fichier et se souvenir d’un fichier ou d’un dossier afin que votre application puisse y accéder facilement à nouveau.

Nous allons étudier le code qui permet d’écrire et de lire du texte dans un fichier et comment accéder aux dossiers d’application locaux, itinérants et temporaires.

## <a name="what-do-you-need-to-know"></a>Ce que vous devez savoir

Voici les principaux types que vous devez connaître pour lire ou écrire du texte dans un fichier :

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) représente un fichier. Cette classe a des propriétés qui fournissent des informations sur le fichier et les méthodes de création, d’ouverture, de copie, de suppression et de modification du nom des fichiers.
Vous pouvez être habitué à gérer des chemins d’accès en chaîne. Certaines API UWP utilisent un chemin d’accès en chaîne, mais le plus souvent, vous utiliserez un **StorageFile** pour représenter un fichier, car certains fichiers que vous utilisez dans UWP ne disposent pas d’un chemin d’accès ou ont un chemin d’accès parfois difficile à gérer. Utilisez [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) pour convertir un chemin d’accès en chaîne en **StorageFile**. 

- La classe [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) offre un moyen simple pour lire et écrire du texte. Cette classe peut également lire/écrire un tableau d’octets ou le contenu d’une mémoire tampon. Cette classe est très similaire à la classe [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio). La principale différence est qu’au lieu d’utiliser un chemin d’accès en chaîne, comme le fait **PathIO**, elle utilise un **StorageFile**.
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) représente un dossier (répertoire). Cette classe dispose de méthodes pour la création de fichiers, l’interrogation du contenu d’un dossier, la création, la modification du nom et la suppression des dossiers, ainsi que de propriétés qui fournissent des informations sur un dossier. 

Les méthodes courantes pour obtenir un **StorageFolder** sont notamment les suivantes :

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) qui permet à l’utilisateur de naviguer dans le dossier à utiliser.
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) qui fournit le **StorageFolder** spécifique de l’un des dossiers locaux à l’application comme le dossier local, itinérant et temporaire.
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) qui fournit le **StorageFolder** pour les bibliothèques connues telles que les bibliothèques de musique ou d’images.

## <a name="write-text-to-a-file"></a>Écrire du texte dans un fichier

 Pour cette introduction, nous allons nous concentrer sur un scénario simple : lecture et écriture de texte. Commençons par examiner un code qui utilise la classe **FileIO** pour écrire du texte dans un fichier.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

Tout d’abord, nous identifions où le fichier doit être situé. `Windows.Storage.ApplicationData.Current.LocalFolder` fournit l’accès au dossier de données local, qui est créé pour votre application lors de son installation. Voir [Accéder au système de fichiers](#access-the-file-system) pour plus d’informations sur les dossiers auxquels votre application peut accéder.

Ensuite, nous utilisons **StorageFolder** pour créer le fichier (ou l’ouvrir s’il existe déjà).

La classe **FileIO** fournit un moyen pratique d’écrire du texte dans le fichier. `FileIO.WriteTextAsync()` remplace l’ensemble du contenu du fichier par le texte fourni. `FileIO.AppendLinesAsync()` ajoute une collection de chaînes au fichier, en écrivant une seule chaîne par ligne.

## <a name="read-text-from-a-file"></a>Lire du texte dans un fichier

Pour la lecture d’un fichier comme pour l’écriture, il faut d’abord spécifier où se trouve le fichier. Nous allons utiliser le même emplacement, comme dans l’exemple ci-dessus. Ensuite, nous allons utiliser la classe **FileIO** pour lire son contenu.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Vous pouvez également lire chaque ligne du fichier dans des chaînes individuelles dans une collection avec `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);`

## <a name="access-the-file-system"></a>Accéder au système de fichiers

Dans la plateforme UWP, l’accès au dossier est limité pour garantir l’intégrité et la confidentialité des données de l’utilisateur. 

### <a name="app-folders"></a>Dossiers d’application

Lorsqu’une application UWP est installée, plusieurs dossiers sont créés sous c:\users\\<nom de l’utilisateur>\AppData\Local\Packages\<Identificateur du package de l’application>\ pour stocker, entre autres choses, les fichiers d’application locaux, itinérants et temporaires. L’application n’a pas besoin de déclarer de fonctionnalités pour accéder à ces dossiers, et ces dossiers ne sont pas accessibles par d’autres applications. Ces dossiers sont également supprimés lorsque l’application est désinstallée.

Voici quelques-uns des dossiers d’application que vous utilisez généralement :

- **LocalState** : pour les données locales sur l’appareil actuel. Lorsque l’appareil est sauvegardé, les données de ce répertoire sont enregistrées dans une image de sauvegarde dans OneDrive. Si l’utilisateur réinitialise ou remplace l’appareil, les données seront restaurées. Accédez à ce dossier avec `Windows.Storage.ApplicationData.Current.LocalFolder.` Enregistrez les données locales que vous ne voulez pas sauvegarder sur OneDrive dans le **LocalCacheFolder** auquel vous pouvez accéder avec `Windows.Storage.ApplicationData.Current.LocalCacheFolder`.

- **RoamingState** : pour les données qui doivent être répliquées sur tous les appareils sur lesquels l’application est installée. Windows limite la quantité de données itinérantes, donc n’enregistrez que les paramètres utilisateur et de petits fichiers ici. Accédez au dossier itinérant avec `Windows.Storage.ApplicationData.Current.RoamingFolder`.

- **TempState** : pour les données qui peuvent être supprimées lorsque l’application n’est pas en cours d’exécution. Accédez à ce dossier avec `Windows.Storage.ApplicationData.Current.TemporaryFolder`.

### <a name="access-the-rest-of-the-file-system"></a>Accédez au reste du système de fichiers

Une application UWP doit déclarer son intention d’accéder à une bibliothèque d’utilisateur spécifique en ajoutant la fonctionnalité correspondante dans son manifeste. L’utilisateur est alors invité lors de l’installation de l’application à vérifier qu’il autorise l’accès à la bibliothèque spécifiée. Si ce n’est pas le cas, l’application n’est pas installée. Il existe des fonctionnalités pour accéder aux bibliothèques de musique, de vidéos et d’images. Voir [Déclaration des fonctionnalités d’application](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) pour obtenir la liste complète. Pour obtenir un **StorageFolder** pour ces bibliothèques, utilisez la classe [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders).

#### <a name="documents-library"></a>Bibliothèque de documents

Bien qu’il existe une fonctionnalité pour accéder à la bibliothèque de documents de l’utilisateur, cette fonctionnalité est limitée, ce qui signifie qu’une application qui la déclare est rejetée par le Microsoft Store, sauf si vous suivez un processus pour obtenir une approbation spéciale. Elle n’est pas destinée à être utilisée de manière générale. Au lieu de cela, utilisez les sélecteurs de fichiers ou de dossiers (voir [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) et [Enregistrer un fichier avec un sélecteur](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker)) qui permettent à l’utilisateur de naviguer vers le fichier ou le dossier. Lorsque l’utilisateur navigue vers un fichier ou un dossier, il a donné implicitement l’autorisation à l’application d’y accéder et le système autorise l’accès.

#### <a name="general-access"></a>Accès général

Sinon, votre application peut déclarer la fonctionnalité restreinte [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) dans son manifeste, ce qui requiert également l’approbation du Microsoft Store. L’application peut alors accéder à n’importe quel fichier auquel l’utilisateur a accès sans nécessiter l’intervention d’un sélecteur de fichier ou de dossier.

Pour obtenir la liste complète des emplacements auxquels les applications peuvent accéder, voir [Autorisations d’accès aux fichiers](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

## <a name="useful-apis-and-docs"></a>API et documents utiles

Voici un résumé rapide des API et d’autres documents utiles pour vous aider à commencer à utiliser des fichiers et des dossiers.

### <a name="useful-apis"></a>API utiles

| API | Description |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | Fournit des informations sur le fichier et les méthodes de création, d’ouverture, de copie, de suppression et de modification du nom des fichiers. |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | Fournit des informations sur le dossier, des méthodes de création de fichiers et des méthodes de création, de modification du nom et de suppression des dossiers. |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  Offre un moyen simple pour lire et écrire du texte. Cette classe peut également lire/écrire un tableau d’octets ou le contenu d’une mémoire tampon. |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | Fournit un moyen simple de lire/écrire du texte dans un fichier en donnant un chemin d’accès en chaîne au fichier. Cette classe peut également lire/écrire un tableau d’octets ou le contenu d’une mémoire tampon. |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  Lire et écrire des mémoires tampons, des octets, des entiers, des GUID, des TimeSpans, etc., dans un flux. |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | Fournit l’accès aux dossiers créés pour l’application, tels que le dossier local, le dossier itinérant et le dossier des fichiers temporaires. |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  Permet à l’utilisateur de choisir un dossier et retourne un **StorageFolder** pour celui-ci. Voici comment vous pouvez accéder à des emplacements auxquels l’application ne peut pas accéder par défaut. |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | Permet à l’utilisateur de choisir un fichier à ouvrir et retourne un **StorageFile** pour celui-ci. Voici comment vous pouvez accéder à un fichier auquel l’application ne peut pas accéder par défaut. |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | Permet à l’utilisateur de choisir le nom de fichier, l’extension et l’emplacement de stockage d’un fichier. Renvoie un **StorageFile**. Voici comment enregistrer un fichier dans un emplacement auquel l’application ne peut pas accéder par défaut. |
|  [Espace de noms Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Explique comment lire et écrire des flux. En particulier, examinez les classes [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) et [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) qui lisent et écrivent des mémoires tampons, des octets, des entiers, des GUID, des TimeSpans, etc. |

### <a name="useful-docs"></a>Documents utiles

| Rubrique | Description |
|-------|----------------|
| [Espace de noms Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage) | Documents de référence sur les API. |
| [Fichiers, dossiers et bibliothèques](https://docs.microsoft.com/windows/uwp/files/) | Documents conceptuels. |
| [Créer, écrire et lire un fichier](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | Aborde la création, la lecture et l’écriture de texte, de données binaires et de flux. |
| [Prise en main du stockage de données d’application en local](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | En plus de traiter des meilleures pratiques d’enregistrement des données locales, explique le rôle des dossiers LocalSettings et LocalCache. |
| [Prise en main des données d’application itinérantes](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | Une série de deux parties sur l’utilisation des données d’application itinérantes. |
| [Recommandations en matière de données d’application itinérantes](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Suivez ces recommandations en matière d’itinérance des données lorsque vous concevez votre application. |
| [Stocker et récupérer des paramètres et autres données d’application](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Fournit une vue d’ensemble des différents magasins de données d’application, tels que les dossiers locaux, itinérants et temporaires. Consultez la section [Données itinérantes](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data) pour des instructions et des informations supplémentaires sur l’écriture de données qui se déplacent entre les appareils. |
| [Autorisations d’accès aux fichiers](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | Informations sur les emplacements du système de fichiers auxquels votre application peut accéder. |
| [Ouvrir des fichiers et des dossiers à l’aide d’un sélecteur](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | Montre comment accéder aux fichiers et aux dossiers en permettant à l’utilisateur de choisir à l’aide d’une interface utilisateur de sélecteur. |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Types utilisés pour lire et écrire des flux. |
| [Fichiers et dossiers des bibliothèques Musique, Images et Vidéos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | Explique comment supprimer des dossiers de bibliothèques, obtenir la liste des dossiers d’une bibliothèque et découvrir des photos, de la musique et des vidéos. |

## <a name="useful-code-samples"></a>Exemples de code utiles

| Exemple de code | Description |
|-----------------|---------------|
| [Exemple de données d’application](https://code.msdn.microsoft.com/windowsapps/ApplicationData-sample-fb043eb2) | Montre comment stocker et récupérer des données spécifiques à chaque utilisateur à l’aide des API de données d’application. |
| [Exemple d’accès aux fichiers](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | Montre comment créer, lire, écrire, copier et supprimer un fichier. |
| [Exemple de sélecteur de fichiers](https://code.msdn.microsoft.com/windowsapps/File-picker-sample-9f294cba) | Montre comment accéder aux fichiers et aux dossiers en permettant à l’utilisateur de les choisir à l’aide d’une interface utilisateur et comment enregistrer un fichier de sorte que l’utilisateur puisse spécifier le nom, le type de fichier et l’emplacement du fichier à enregistrer. |
| [Exemple JSON](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | Indique comment encoder et décoder des objets, des tableaux, des chaînes, des chiffres et des valeurs booléennes JSON (JavaScript Object Notation) à l’aide de l’[espace de noms Windows.Data.Json](https://docs.microsoft.com/uwp/api/Windows.Data.Json). |
| [Exemples de code supplémentaires](https://developer.microsoft.com/windows/samples) | Choisissez **Fichiers, dossiers et bibliothèques** dans la liste déroulante des catégories. |
