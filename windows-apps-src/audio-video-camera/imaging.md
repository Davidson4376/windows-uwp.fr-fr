---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: Cet article explique comment charger et enregistrer des fichiers image à l’aide de BitmapDecoder et de BitmapEncoder, et comment utiliser l’objet SoftwareBitmap pour représenter des images bitmap.
title: Créer, modifier et enregistrer des images bitmap
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c25fc09d606c0f143f357dd7f89026fa94b80922
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318358"
---
# <a name="create-edit-and-save-bitmap-images"></a>Créer, modifier et enregistrer des images bitmap



Cet article explique comment charger et enregistrer des fichiers image à l’aide de [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) et de [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder), et comment utiliser l’objet [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) pour représenter des images bitmap.

La classe **SoftwareBitmap** est une API polyvalente qui peut être créée à partir de plusieurs sources, y compris les fichiers image, les objets [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap), les surfaces Direct3D et le code. **SoftwareBitmap** permet de convertir facilement entre les différents formats de pixels et les modes alpha, et offre l’accès de bas niveau aux données de pixel. En outre, **SoftwareBitmap** est une interface commune utilisée par plusieurs fonctionnalités de Windows, notamment :

-   [**CapturedFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrame) vous permet d’obtenir les frames capturés par la caméra en tant qu’un **SoftwareBitmap**.

-   [**VideoFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) vous permet d’obtenir un **SoftwareBitmap** représentation sous forme d’un **VideoFrame**.

-   [**FaceDetector** ](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) vous permet de détecter les visages dans une **SoftwareBitmap**.

L’exemple de code dans cet article utilise les API des espaces de noms suivants.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Créer un élément SoftwareBitmap à partir d’un fichier image avec BitmapDecoder

Pour créer un élément [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) à partir d’un fichier, obtenez une instance de [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) qui contient les données d’image. Cet exemple utilise un élément [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour permettre à l’utilisateur de sélectionner un fichier image.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

Appelez la méthode [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) de l’objet **StorageFile** pour obtenir un flux d’accès aléatoire contenant les données d’image. Appelez la méthode statique [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) pour obtenir une instance de la classe [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) pour le flux spécifié. Appelez [**GetSoftwareBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) pour obtenir un objet [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) contenant l’image.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>Enregistrer un élément SoftwareBitmap dans un fichier avec BitmapEncoder

Pour enregistrer un élément **SoftwareBitmap** dans un fichier, obtenez une instance de **StorageFile** sur laquelle l’image sera enregistrée. Cet exemple utilise un élément [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) pour permettre à l’utilisateur de sélectionner un fichier de sortie.

[!code-cs[PickOutputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOutputFile)]

Appelez la méthode [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) de l’objet **StorageFile** pour obtenir un flux d’accès aléatoire sur lequel écrire l’image. Appelez la méthode statique [**BitmapEncoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) pour obtenir une instance de la classe [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) pour le flux spécifié. Le premier paramètre de **CreateAsync** est un GUID représentant le codec qui doit être utilisé pour encoder l’image. La classe **BitmapEncoder** expose une propriété qui contient l’ID de chaque codec pris en charge par l’encodeur, telle que [**JpegEncoderId**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid).

Utilisez la méthode [**SetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap) pour définir l’image qui sera encodée. Vous pouvez définir les valeurs de la propriété [**BitmapTransform**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTransform) pour appliquer des transformations de base à l’image pendant qu’elle est encodée. La propriété [**IsThumbnailGenerated**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated) détermine si une miniature est générée par l’encodeur. Veuillez noter que tous les formats de fichier ne prennent pas en charge les miniatures. Si vous utilisez cette fonctionnalité, vous devez intercepter l’erreur d’opération non prise en charge qui sera levée si les miniatures ne sont pas prises en charge.

Appelez [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) pour forcer l’encodeur à écrire les données d’image dans le fichier spécifié.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

Vous pouvez spécifier des options d’encodage supplémentaires lorsque vous créez l’élément **BitmapEncoder** en créant un objet [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) et en le remplissant avec un ou plusieurs objets [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) représentant les paramètres d’encodeur. Pour obtenir la liste des options d’encodeur prises en charge, voir [Référence des options BitmapEncoder](bitmapencoder-options-reference.md).

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>Utiliser SoftwareBitmap avec un contrôle XAML Image

Pour afficher une image dans une page XAML à l’aide du contrôle [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), commencez par définir un contrôle **Image** dans votre page XAML.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

Actuellement, le contrôle **Image** prend en charge uniquement les images qui utilisent le codage BGRA8 et le canal prémultiplié ou non alpha. Avant d’essayer d’afficher une image, testez-la pour vérifier qu’elle a le format correct, sinon, utilisez la méthode statique [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert) de **SoftwareBitmap** pour convertir l’image au format pris en charge.

Créez un objet [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource). Définissez le contenu de l’objet source en appelant [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync), en transmettant un élément **SoftwareBitmap**. Vous pouvez ensuite définir la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) du contrôle **Image** sur l’élément **SoftwareBitmapSource** créé.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

Vous pouvez également utiliser **SoftwareBitmapSource** pour définir un élément **SoftwareBitmap** comme [**ImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) pour [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush).

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>Créer un élément SoftwareBitmap à partir d’un élément WriteableBitmap

Vous pouvez créer un élément **SoftwareBitmap** à partir d’un élément **WriteableBitmap** existant en appelant [**SoftwareBitmap.CreateCopyFromBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer) et en fournissant la propriété **PixelBuffer** de la classe **WriteableBitmap** pour définir les données de pixel. Le deuxième argument permet de demander un format de pixel pour l’élément **WriteableBitmap** créé. Vous pouvez utiliser les propriétés [**PixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth) et [**PixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight) de **WriteableBitmap** pour spécifier les dimensions de la nouvelle image.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>Créer ou modifier un élément SoftwareBitmap par programme

Jusqu’à présent, cette rubrique faisait référence aux fichiers image. Vous pouvez également créer un élément **SoftwareBitmap** par programme dans le code et utiliser la même technique pour accéder aux données de pixel de **SoftwareBitmap** et les modifier.

**SoftwareBitmap** utilise COM Interop pour exposer la mémoire tampon brute contenant les données de pixel.

Pour utiliser COM Interop, vous devez inclure une référence à l’espace de noms **System.Runtime.InteropServices** dans votre projet.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

Initialisez l’interface COM [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions/mt297505(v=vs.85)) en ajoutant le code suivant dans votre espace de noms.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

Créez un élément **SoftwareBitmap** avec le format de pixel et la taille souhaités. Vous pouvez aussi utiliser un élément **SoftwareBitmap** existant pour lequel vous voulez modifier les données de pixel. Appelez [**SoftwareBitmap.LockBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer) pour obtenir une instance de la classe [**BitmapBuffer**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) représentant la mémoire tampon des données de pixel. Diffusez l’élément **BitmapBuffer** sur l’interface COM **IMemoryBufferByteAccess**, puis appelez [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) pour remplir un tableau d’octets avec des données. Utilisez la méthode [**BitmapBuffer.GetPlaneDescription**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) pour obtenir un objet [**BitmapPlaneDescription**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription) qui vous aidera à calculer le décalage dans la mémoire tampon pour chaque pixel.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

Étant donné que cette méthode accède à la mémoire tampon brute sous-jacente des types Windows Runtime, elle doit être déclarée à l’aide du mot clé **unsafe**. Vous devez également configurer votre projet dans Microsoft Visual Studio pour permettre la compilation du code non sécurisé en ouvrant la page **Propriétés** du projet, en cliquant sur la page de propriétés **Créer**, puis en sélectionnant la case à cocher **Autoriser du code unsafe**.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Créer un élément SoftwareBitmap à partir d’une surface Direct3D

Pour créer un objet **SoftwareBitmap** à partir d’une surface Direct3D, vous devez inclure l’espace de noms [**Windows.Graphics.DirectX.Direct3D11**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11) dans votre projet.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

Appelez [**CreateCopyFromSurfaceAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync) pour créer un élément **SoftwareBitmap** à partir de la surface. Comme son nom l’indique, le nouvel élément **SoftwareBitmap** contient une copie distincte des données d’image. Les modifications apportées à **SoftwareBitmap** n’auront pas d’effet sur la surface Direct3D.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>Convertir un élément SoftwareBitmap dans un format de pixel différent

La classe **SoftwareBitmap** fournit la méthode statique [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert) permettant de créer facilement un nouvel élément **SoftwareBitmap**, qui utilise le format pixel et le mode alpha spécifiés à partir d’un élément **SoftwareBitmap** existant. Veuillez noter que l’image bitmap créée contient une copie distincte des données d’image. Les modifications apportées à la nouvelle image bitmap n’affectent pas l’image bitmap source.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>Transcoder un fichier image

Vous pouvez transcoder un fichier image directement à partir de [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) vers [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Créez un [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) à partir du fichier à transcoder. Créez un **BitmapDecoder** à partir du flux d’entrée. Créez un [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) sur lequel l’encodeur pourra écrire et appelez [**BitmapEncoder.CreateForTranscodingAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) en transmettant le flux en mémoire et l’objet de décodeur. Les options d'encodage ne sont pas prises en charge lors du transcodage ; vous devez plutôt utiliser [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync). Toutes les propriétés dans le fichier image d’entrée que vous ne définissez pas spécifiquement sur l’encodeur seront écrites dans le fichier de sortie inchangé. Appelez [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) pour forcer l’encodeur à encoder dans le flux en mémoire. Enfin, recherchez le début du flux de fichier et du flux en mémoire et appelez [**CopyAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.randomaccessstream.copyasync) pour écrire le flux en mémoire dans le flux de fichier.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>Rubriques connexes

* [Référence des options BitmapEncoder](bitmapencoder-options-reference.md)
* [Métadonnées d’image](image-metadata.md)
 

 




