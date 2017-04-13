---
author: laurenhughes
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: "Créer, écrire et lire un fichier"
description: "Lisez et écrivez un fichier à l’aide d’un objet StorageFile."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 6b69a2e69948e1d774abe78ba0958aa48ba4d318
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-write-and-read-a-file"></a>Créer, écrire et lire un fichier


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Classe StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**Classe StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**Classe FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440)

Lisez et écrivez un fichier à l’aide d’un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

> **Remarque** Consultez également l’[exemple d’accès aux fichiers](http://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Prérequis

-   **Comprendre la programmation asynchrone pour les applications pour la plateforme Windows universelle (UWP)**

    Pour apprendre à écrire des applications asynchrones en C# ou Visual Basic, voir [Appeler des API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Pour apprendre à écrire des applications asynchrones en C++, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Savoir vous procurer le fichier que vous voulez lire et dans lequel vous voulez écrire, ou les deux**

    Pour apprendre à vous procurer un fichier à l’aide d’un sélecteur de fichiers, voir [Ouvrir des fichiers et dossiers à l’aide d’un sélecteur](quickstart-using-file-and-folder-pickers.md).

## <a name="creating-a-file"></a>Création d’un fichier

Voici comment créer un fichier dans le dossier local de l'application. S'il existe déjà, nous le remplaçons.
> [!div class="tabbedCodeSnippets"]
```cs
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```
```vb
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## <a name="writing-to-a-file"></a>Écriture dans un fichier


Voici comment écrire dans un fichier accessible en écriture sur le disque à l’aide de la classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). La première étape pour chacune des méthodes d'écriture dans un fichier (sauf si vous écrivez dans le fichier immédiatement après sa création) consiste à obtenir le fichier à l’aide de la méthode [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272).
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Écriture de texte dans un fichier**

Écrivez du texte dans votre fichier en appelant la méthode [**WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505) de la classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```
```vb
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**Écriture d’octets dans un fichier par le biais d’une mémoire tampon (2 étapes)**

1.  Tout d'abord, appelez la méthode [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385) pour obtenir une mémoire tampon des octets (basés sur une chaîne arbitraire) que vous voulez écrire dans votre fichier.
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```
```vb
Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
                    "What fools these mortals be",
                    Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
```

2.  Ensuite, écrivez les octets de votre mémoire tampon dans votre fichier en appelant la méthode [**WriteBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701490) de la classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
```
```vb
Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
```

**Écriture de texte dans un fichier par le biais d’un flux (4 étapes)**

1.  Premièrement, ouvrez le fichier en appelant la méthode [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851). Cette dernière renvoie un flux du contenu du fichier lorsque l’opération d’ouverture se termine.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  Ensuite, obtenez un flux de sortie en appelant la méthode [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738) à partir du `stream`. Placez ce code dans une instruction **using** pour gérer la durée de vie du flux en sortie.
> [!div class="tabbedCodeSnippets"]
```cs
using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
```
```vb
Using outputStream = stream.GetOutputStreamAt(0)
  ' We'll add more code here in the next step.
End Using
```

3.  À présent, ajoutez ce code dans l’instruction **using** existante pour écrire dans le flux de sortie en créant un objet [**DataWriter**](https://msdn.microsoft.com/library/windows/apps/br208154) et en appelant la méthode [**DataWriter.WriteString**](https://msdn.microsoft.com/library/windows/apps/br241642).
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
```
```vb
    Dim dataWriter As New DataWriter(outputStream)
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
```

4.  Pour finir, ajoutez le code suivant (dans l’instruction **using**) pour enregistrer le texte dans votre fichier avec [**StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) et fermer le flux avec [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br241729).
> [!div class="tabbedCodeSnippets"]
```cs
    await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
```
```vb
    Await dataWriter.StoreAsync()
        Await outputStream.FlushAsync()
```

## <a name="reading-from-a-file"></a>Lecture d’un fichier


Voici comment lire un fichier sur disque en utilisant la classe [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). La première étape de lecture d'un fichier consiste à obtenir le fichier à l’aide de la méthode [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272).
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Lecture de texte dans un fichier**

Lisez du texte de votre fichier en appelant la méthode [**ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) de la classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```
```vb
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**Lecture de texte dans un fichier via une mémoire tampon (2étapes)**

1.  Commencez par appeler la méthode [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701468) de la classe [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
```
```vb
Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
```

2.  Utilisez ensuite un objet [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) pour lire la longueur de la mémoire tampon, puis son contenu.
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
```
```vb
Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
```

**Lecture de texte dans un fichier via un flux (4 étapes)**

1.  Ouvrez un flux pour votre fichier en appelant la méthode [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851). Celle-ci renvoie un flux du contenu du fichier pendant l’exécution de l’opération.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  Obtenez la taille du flux à utiliser ultérieurement.
> [!div class="tabbedCodeSnippets"]
```cs
ulong size = stream.Size;
```
```vb
Dim size = stream.Size
```

3.  Obtenez un flux d’entrée en appelant la méthode [**GetInputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241737). Placez ce code dans une instruction **using** pour gérer la durée de vie du flux. Spécifiez 0 quand vous appelez **GetInputStreamAt** pour définir la position sur le début du flux.
> [!div class="tabbedCodeSnippets"]
```cs
using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
```
```vb
Using inputStream = stream.GetInputStreamAt(0)
    ' We'll add more code here in the next step.
End Using
```

4.  Enfin, ajoutez ce code dans l’instruction **using** existante pour obtenir un objet [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) sur le flux, puis lisez le texte en appelant les méthodes [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) et [**DataReader.ReadString**](https://msdn.microsoft.com/library/windows/apps/br208147).
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
```
```vb
Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
```

 

 
