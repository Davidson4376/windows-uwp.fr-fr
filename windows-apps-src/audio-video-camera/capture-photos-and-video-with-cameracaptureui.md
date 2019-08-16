---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Cet article explique comment utiliser la classe CameraCaptureUI pour capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégrée à Windows.
title: Capturez des photos et des vidéos avec l’interface utilisateur de l’appareil photo intégré Windows
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d582d4815b4fb2168b187a1efff3795cc98aca02
ms.sourcegitcommit: 99595e4938213aafdb49635d684d8ba8eb3f697a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487804"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Capturez des photos et des vidéos avec l’interface utilisateur de l’appareil photo intégré Windows



Cet article explique comment utiliser la classe CameraCaptureUI pour capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégrée à Windows. Cette fonctionnalité est facile à utiliser. Il permet à votre application d’obtenir une photo ou une vidéo capturée par l’utilisateur avec seulement quelques lignes de code.

Si vous voulez fournir votre propre interface utilisateur d’appareil photo ou si votre scénario nécessite un contrôle de bas niveau plus robuste de l’opération de capture, utilisez l’objet [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) et implémentez votre propre expérience de capture. Pour plus d’informations, voir [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Vous ne devez pas spécifier les fonctionnalités de la **webcam** ou du **microphone** dans le fichier manifeste de votre application si votre application utilise uniquement CameraCaptureUI. Dans ce cas, votre application s’affiche dans les paramètres de confidentialité de l’appareil, mais même si l’utilisateur refuse l’accès de l’appareil photo à votre application, cela n’empêche pas le CameraCaptureUI de capturer le média. <p>Cela s’explique par le fait que l’application d’appareil photo intégrée de Windows est une application interne approuvée qui nécessite que l’utilisateur démarre la capture photo, vidéo ou audio en appuyant sur un bouton. Votre application peut échouer à la certification du kit de certification des applications Windows lorsque celle-ci est soumise à Microsoft Store si vous spécifiez les fonctionnalités de la webcam ou du microphone lorsque vous utilisez CameraCaptureUI comme seul mécanisme de capture de photos.<p>
Vous devez spécifier les fonctionnalités de la webcam ou du microphone dans le fichier manifeste de votre application si vous utilisez MediaCapture pour capturer des données audio, des photos ou des vidéos par programmation.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturer une photo avec CameraCaptureUI

Pour utiliser l’interface utilisateur de capture d’appareil photo, incluez l’espace de noms [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) dans votre projet. Pour les opérations de fichier avec le fichier image renvoyé, incluez [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Pour capturer une photo, créez un objet [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI). À l’aide de la propriété [**photosettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings) de l’objet, vous pouvez spécifier des propriétés pour la photo retournée, telles que le format d’image de la photo. Par défaut, l’interface utilisateur de capture de l’appareil photo prend en charge le rognage de la photo avant son retour. Cela peut être désactivé à l’aide de la propriété [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) . Cet exemple définit [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) pour demander que l’image renvoyée soit au format 200 x 200 pixels.

> [!NOTE]
> Le rognage d’image dans le **CameraCaptureUI** n’est pas pris en charge pour les appareils de la famille d’appareils mobiles. La valeur de la propriété [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) est ignorée quand votre application est exécutée sur ces appareils.

Appelez [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) et spécifiez [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) pour indiquer qu’une photo doit être capturée. La méthode renvoie une instance [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) contenant l’image si la capture est réussie. Si l’utilisateur annule la capture, l’objet renvoyé est null.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

L’élément **StorageFile** qui contient la photo capturée reçoit un nom généré de manière dynamique et est enregistré dans le dossier local de votre application. Pour mieux organiser vos photos capturées, vous pouvez déplacer le fichier vers un autre dossier.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Pour utiliser votre photo dans votre application, vous pouvez créer un objet [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) qui peut être utilisé avec plusieurs fonctionnalités différentes des applications Windows universelles.

Tout d’abord, incluez l’espace de noms [**Windows. Graphics. Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) dans votre projet.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Appelez [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) pour obtenir un flux à partir du fichier image. Appelez [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) pour obtenir un décodeur d’image bitmap à partir du flux. Ensuite, appelez [**GetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) pour obtenir une représentation **SoftwareBitmap** de l’image.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Pour afficher l’image dans votre interface utilisateur, déclarez un contrôle [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) dans votre page XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Pour utiliser l’image bitmap logicielle dans votre page XAML, incluez l’espace de noms using [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) dans votre projet.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

Le contrôle **image** nécessite que la source de l’image soit au format BGRA8 avec des caractères alpha prémultipliés ou sans alpha. Appelez la méthode statique [**SoftwareBitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) pour créer une nouvelle image bitmap de logiciel avec le format souhaité. Ensuite, créez un nouvel objet [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) et appelez-le [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) pour affecter la bitmap logicielle à la source. Enfin, définissez le contrôle **Image** et sa propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) pour afficher la photo capturée dans l’interface utilisateur.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Capturer une vidéo avec CameraCaptureUI

Pour capturer une vidéo, créez un objet [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI). À l’aide de la propriété [**VideoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) de l’objet, vous pouvez spécifier des propriétés pour la vidéo retournée, telles que le format de la vidéo.

Appelez [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) et spécifiez [**Video**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) pour capturer une vidéo. La méthode renvoie une instance [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) contenant la vidéo si la capture est réussie. Si vous annulez la capture, l’objet retourné a la valeur null.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Ce que vous faites du fichier vidéo capturé dépend du scénario pour votre application. Le reste de cet article décrit comment créer rapidement une composition multimédia à partir d’une ou plusieurs vidéos capturées et l’afficher dans votre interface utilisateur.

Tout d’abord, ajoutez un contrôle [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) dans lequel la composition vidéo s’affichera sur votre page XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Lorsque le fichier vidéo est retourné par l’interface utilisateur de capture de l’appareil photo, créez un nouvel [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) en appelant **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** . Appelez la méthode **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** de la valeur par défaut **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** associée au **MediaPlayerElement** pour lire la vidéo.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture de photos, vidéo et audio de base avec MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




