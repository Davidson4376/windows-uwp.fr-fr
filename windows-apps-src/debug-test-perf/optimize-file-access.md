---
ms.assetid: 40122343-1FE3-4160-BABE-6A2DD9AF1E8E
title: Optimiser l’accès aux fichiers
description: Créez des applications de plateforme Windows universelle (UWP) qui accèdent au système de fichiers efficacement, en évitant les problèmes de performances dus à la latence de disque et aux cycles de la mémoire et du processeur.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cacd915530bb599936730ec404a6e524fef0105d
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8747657"
---
# <a name="optimize-file-access"></a>Optimiser l’accès aux fichiers


Créez des applications de plateforme Windows universelle (UWP) qui accèdent au système de fichiers efficacement, en évitant les problèmes de performances dus à la latence de disque et aux cycles de la mémoire et du processeur.

Lorsque vous souhaitez accéder à une grande collection de fichiers et aux valeurs de propriétés autres que les propriétés courantes Name, FileType et Path, vous pouvez y accéder en créant [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) et en appelant [**SetPropertyPrefetch**](https://msdn.microsoft.com/library/windows/apps/hh973319). La méthode **SetPropertyPrefetch** peut considérablement améliorer les performances des applications qui affichent une collection d’éléments obtenus à partir du système de fichiers, comme une collection d’images. Les exemples qui suivent montrent différentes manières d’accéder à des fichiers multiples.

Le premier exemple utilise [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273) pour récupérer les informations de nom d’un ensemble de fichiers. Cette approche offre de bonnes performances, car l’exemple accède uniquement à la propriété Name.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> 
> for (int i = 0; i < files.Count; i++)
> {
>     // do something with the name of each file
>     string fileName = files[i].Name;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) =
>     Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> 
> For i As Integer = 0 To files.Count - 1
>     ' do something with the name of each file
>     Dim fileName As String = files(i).Name
> Next i
> ```

Le deuxième exemple utilise [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273), puis récupère les propriétés de l’image pour chaque fichier. Cette approche offre de mauvaises performances.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> for (int i = 0; i < files.Count; i++)
> {
>     ImageProperties imgProps = await files[i].Properties.GetImagePropertiesAsync();
> 
>     // do something with the date the image was taken
>     DateTimeOffset date = imgProps.DateTaken;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) = Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> For i As Integer = 0 To files.Count - 1
>     Dim imgProps As ImageProperties =
>         Await files(i).Properties.GetImagePropertiesAsync()
> 
>     ' do something with the date the image was taken
>     Dim dateTaken As DateTimeOffset = imgProps.DateTaken
> Next i
> ```

Le troisième exemple utilise [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) pour obtenir des informations sur un ensemble de fichiers. Cette approche offre de bien meilleures performances que l’exemple précédent.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> // Set QueryOptions to prefetch our specific properties
> var queryOptions = new Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, null);
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView, 100,
>         ThumbnailOptions.ReturnOnlyIfCached);
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties, 
>        new string[] {"System.Size"});
> 
> StorageFileQueryResult queryResults = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions);
> IReadOnlyList<StorageFile> files = await queryResults.GetFilesAsync();
> 
> foreach (var file in files)
> {
>     ImageProperties imageProperties = await file.Properties.GetImagePropertiesAsync();
> 
>     // Do something with the date the image was taken.
>     DateTimeOffset dateTaken = imageProperties.DateTaken;
> 
>     // Performance gains increase with the number of properties that are accessed.
>     IDictionary<String, object> propertyResults =
>         await file.Properties.RetrievePropertiesAsync(
>               new string[] {"System.Size" });
> 
>     // Get/Set extra properties here
>     var systemSize = propertyResults["System.Size"];
> }
> ```
> ```vb
> ' Set QueryOptions to prefetch our specific properties
> Dim queryOptions = New Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, Nothing)
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView,
>             100, Windows.Storage.FileProperties.ThumbnailOptions.ReturnOnlyIfCached)
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties,
>                                  New String() {"System.Size"})
> 
> Dim queryResults As StorageFileQueryResult = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions)
> Dim files As IReadOnlyList(Of StorageFile) = Await queryResults.GetFilesAsync()
> 
> 
> For Each file In files
>     Dim imageProperties As ImageProperties = Await file.Properties.GetImagePropertiesAsync()
> 
>     ' Do something with the date the image was taken.
>     Dim dateTaken As DateTimeOffset = imageProperties.DateTaken
> 
>     ' Performance gains increase with the number of properties that are accessed.
>     Dim propertyResults As IDictionary(Of String, Object) =
>         Await file.Properties.RetrievePropertiesAsync(New String() {"System.Size"})
> 
>     ' Get/Set extra properties here
>     Dim systemSize = propertyResults("System.Size")
> 
> Next file
> ```
Si vous réalisez plusieurs opérations sur des objets Windows.Storage tels que `Windows.Storage.ApplicationData.Current.LocalFolder`, créez une variable locale pour référencer cette source de stockage, de manière à ne pas recréer des objets intermédiaires chaque fois que vous y accédez.

## <a name="stream-performance-in-c-and-visual-basic"></a>Performances des flux en C# et en Visual Basic

### <a name="buffering-between-uwp-and-net-streams"></a>Mise en mémoire tampon entre des flux UWP et .NET

Il existe de nombreuses situations où vous pourriez vouloir convertir un flux UWP (tel qu’un [**Windows.Storage.Streams.IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) ou [**IOutputStream**](https://msdn.microsoft.com/library/windows/apps/BR241728)) en un flux .NET ([**System.IO.Stream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.aspx)). Ceci est utile par exemple lorsque vous écrivez une application UWP et que vous souhaitez utiliser du code .NET existant qui opère sur des flux avec le système de fichiers UWP. Pour ce faire, les API .NET pour les applications UWP fournit des méthodes d’extension qui vous permettent d’effectuer des conversions entre les types de flux UWP et .NET. Pour plus d’informations, voir [**WindowsRuntimeStreamExtensions**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.aspx).

Quand vous convertissez un flux UWP en flux .NET, vous créez en fait un adaptateur pour le flux UWP sous-jacent. Dans certaines circonstances, un coût d’exécution est associé à l’appel de méthodes sur des flux UWP. Ceci peut affecter la rapidité de votre application, notamment lorsque vous effectuez de nombreuses et fréquentes opérations de lecture ou écriture de faible taille.

Pour accélérer les applications, les adaptateurs de flux UWP contiennent une mémoire tampon de données. L’exemple de code suivant illustre plusieurs petites lectures consécutives utilisant un adaptateur de flux UWP avec une taille de mémoire tampon par défaut.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFile file = await Windows.Storage.ApplicationData.Current
>     .LocalFolder.GetFileAsync("example.txt");
> Windows.Storage.Streams.IInputStream windowsRuntimeStream = 
>     await file.OpenReadAsync();
> 
> byte[] destinationArray = new byte[8];
> 
> // Create an adapter with the default buffer size.
> using (var managedStream = windowsRuntimeStream.AsStreamForRead())
> {
> 
>     // Read 8 bytes into destinationArray.
>     // A larger block is actually read from the underlying 
>     // windowsRuntimeStream and buffered within the adapter.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> 
>     // Read 8 more bytes into destinationArray.
>     // This call may complete much faster than the first call
>     // because the data is buffered and no call to the 
>     // underlying windowsRuntimeStream needs to be made.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> }
> ```
> ```vb
> Dim file As StorageFile = Await Windows.Storage.ApplicationData.Current -
> .LocalFolder.GetFileAsync("example.txt")
> Dim windowsRuntimeStream As Windows.Storage.Streams.IInputStream =
>     Await file.OpenReadAsync()
> 
> Dim destinationArray() As Byte = New Byte(8) {}
> 
> ' Create an adapter with the default buffer size.
> Dim managedStream As Stream = windowsRuntimeStream.AsStreamForRead()
> Using (managedStream)
> 
>     ' Read 8 bytes into destinationArray.
>     ' A larger block is actually read from the underlying 
>     ' windowsRuntimeStream and buffered within the adapter.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
>     ' Read 8 more bytes into destinationArray.
>     ' This call may complete much faster than the first call
>     ' because the data is buffered and no call to the 
>     ' underlying windowsRuntimeStream needs to be made.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
> End Using
> ```

Ce comportement de mise en mémoire tampon par défaut est souhaitable dans la plupart des situations où vous convertissez un flux UWP en flux .NET. Cependant, dans certains cas, il peut être préférable d’ajuster le comportement de mise en mémoire tampon afin d’accroître les performances.

### <a name="working-with-large-data-sets"></a>Utilisation de grands jeux de données

Lors de la lecture ou de l’écriture de grands jeux de données, vous pourrez peut-être augmenter le débit en lecture ou en écriture en fournissant une taille de mémoire tampon élevée aux méthodes d’extension [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx)et [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx). Cela procure à l’adaptateur de flux une taille de mémoire tampon interne plus élevée. Par exemple, lors du passage d’un flux qui provient d’un gros fichier à un analyseur XML, celui-ci peut effectuer de nombreuses petites lectures séquentielles à partir du flux. Une mémoire tampon de grande taille peut réduire le nombre d’appels au flux UWP sous-jacent et accroître les performances.

> **Remarque**  vous devez être prudent lors de la définition d’une taille de mémoire tampon qui est supérieure à environ 80 Ko, car cela peut provoquer la fragmentation sur le tas garbage collector (voir [l’amélioration des performances de garbage collection](improve-garbage-collection-performance.md)). L’exemple de code suivant crée un adaptateur de flux managé avec une mémoire tampon de 81920octets.

> [!div class="tabbedCodeSnippets"]
```csharp
// Create a stream adapter with an 80 KB buffer.
Stream managedStream = nativeStream.AsStreamForRead(bufferSize: 81920);
```
```vb
' Create a stream adapter with an 80 KB buffer.
Dim managedStream As Stream = nativeStream.AsStreamForRead(bufferSize:=81920)
```

Les méthodes [**Stream.CopyTo**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copyto.aspx) et [**CopyToAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copytoasync.aspx) allouent également une mémoire tampon locale pour la copie entre les flux. Comme avec la méthode d’extension [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx), vous obtiendrez peut-être de meilleures performances pour les copies de flux de taille conséquente en remplaçant la taille par défaut de la mémoire tampon. L’exemple de code suivant illustre la modification de la taille de mémoire tampon par défaut d’un appel à **CopyToAsync**.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> MemoryStream destination = new MemoryStream();
> // copies the buffer into memory using the default copy buffer
> await managedStream.CopyToAsync(destination);
> 
> // Copy the buffer into memory using a 1 MB copy buffer.
> await managedStream.CopyToAsync(destination, bufferSize: 1024 * 1024);
> ```
> ```vb
> Dim destination As MemoryStream = New MemoryStream()
> ' copies the buffer into memory using the default copy buffer
> Await managedStream.CopyToAsync(destination)
> 
> ' Copy the buffer into memory using a 1 MB copy buffer.
> Await managedStream.CopyToAsync(destination, bufferSize:=1024 * 1024)
> ```

Cet exemple utilise une taille de mémoire tampon de 1Mo, ce qui est supérieur aux 80Ko recommandés plus haut. L’utilisation d’une mémoire tampon aussi grande peut améliorer le débit de l’opération de copie pour les très grands jeux de données (plusieurs centaines de mégaoctets). Toutefois, cette mémoire tampon est allouée sur la grande pile d’objets et pourrait potentiellement entraîner une dégradation du nettoyage de la mémoire. Il convient de configurer des tailles de mémoire tampon élevées uniquement si cela améliorera sensiblement les performances de votre application.

Lorsque vous travaillez avec un grand nombre de flux simultanément, vous souhaiterez peut-être réduire ou éliminer la surcharge sur la mémoire tampon. Vous pouvez spécifier une plus petite mémoire tampon, ou affecter la valeur 0 au paramètre *bufferSize* pour désactiver entièrement la mise en mémoire tampon pour cet adaptateur de flux. Vous pouvez tout de même obtenir un débit satisfaisant sans mise en mémoire tampon si vous effectuez de grandes opérations de lecture et d’écriture vers le flux managé.

### <a name="performing-latency-sensitive-operations"></a>Exécution d’opérations sensibles à la latence

Vous souhaiterez peut-être aussi éviter la mise en mémoire tampon si vous voulez bénéficier de lectures et d’écriture à faible latence sans avoir à lire de gros blocs du flux UWP sous-jacent. Par exemple, des lectures et écritures à faible latence peuvent être souhaitables si vous utilisez le flux pour des communications réseau.

Dans une application de conversation, vous pourriez utiliser un flux sur une interface réseau pour envoyer et recevoir des messages. Dans ce cas, vous souhaiterez envoyer les messages dès qu’ils sont prêts, sans attendre que la mémoire tampon soit pleine. Si vous affectez la valeur 0 à la taille de mémoire tampon lors de l’appel des méthodes d’extension [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) et [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx), l’adaptateur résultant n’allouera pas de mémoire tampon, et tous les appels manipuleront directement le flux UWP sous-jacent.


