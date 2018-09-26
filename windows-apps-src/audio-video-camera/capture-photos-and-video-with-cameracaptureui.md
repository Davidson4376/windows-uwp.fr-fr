---
author: drewbatgit
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Cet article décrit comment utiliser la classe CameraCaptureUI pour capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows.
title: Capturer des photos et des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ba33a1e79a2447c5dac546ce0f1caeaf16929a3
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/25/2018
ms.locfileid: "4178874"
---
# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>Capturer des photos et des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows



Cet article décrit comment utiliser la classe CameraCaptureUI pour capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows. Cette fonctionnalité est facile à utiliser et permet à votre application d’obtenir une photo ou une vidéo capturée par l’utilisateur avec seulement quelques lignes de code.

Si vous voulez fournir votre propre interface utilisateur d’appareil photo ou si votre scénario nécessite un contrôle de bas niveau plus robuste de l’opération de capture, utilisez l’objet [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) et implémentez votre propre expérience de capture. Pour plus d’informations, voir [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Vous ne devez pas spécifier les fonctionnalités **webcam** ou **microphone** dans le fichier manifeste de votre application si votre application utilise uniquement CameraCaptureUI. Sinon, votre application sera affichée dans les paramètres de confidentialité de l’appareil photo, mais même si l’utilisateur n’autorise pas l’appareil photo à accéder à votre application, CameraCaptureUI pourra capturer du contenu multimédia. Cela s’explique par le fait que l’application d’appareil photo intégrée de Windows est une application interne approuvée qui nécessite que l’utilisateur démarre la capture photo, vidéo ou audio en appuyant sur un bouton. Votre application risque de ne pas Kit de Certification des applications Windows lorsque soumises au Store si vous spécifiez les fonctionnalités webcam ou microphone lorsque vous utilisez CameraCaptureUI comme votre seul mécanisme de capture photo.
> Spécifiez les fonctionnalités webcam ou microphone dans le fichier manifeste de votre application si vous utilisez MediaCapture pour effectuer des captures audio, photo ou vidéo par programmation.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturer une photo avec CameraCaptureUI

Pour utiliser l’interface utilisateur de capture d’appareil photo, incluez l’espace de noms [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) dans votre projet. Pour les opérations de fichier avec le fichier image renvoyé, incluez [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Pour capturer une photo, créez un objet [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). À l’aide de la propriété [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) de l’objet, vous pouvez spécifier les propriétés de la photo renvoyée, comme le format d’image. Par défaut, l’interface utilisateur de capture de l’appareil photo permet à l’utilisateur de rogner la photo avant de la renvoyer. Cette fonctionnalité peut être désactivée avec la propriété [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042). Cet exemple définit [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) pour demander que l’image renvoyée soit au format 200x200pixels.

> [!NOTE]
> Le rognage d’images dans **CameraCaptureUI** n’est pas pris en charge pour les appareils de la famille d’appareils mobiles. La valeur de la propriété [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) est ignorée quand votre application est exécutée sur ces appareils.

Appelez [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) et spécifiez [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) pour indiquer qu’une photo doit être capturée. La méthode renvoie une instance [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) contenant l’image si la capture est réussie. Si l’utilisateur annule la capture, l’objet renvoyé est null.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

L’élément **StorageFile** qui contient la photo capturée reçoit un nom généré de manière dynamique et est enregistré dans le dossier local de votre application. Pour mieux organiser vos photos capturées, vous pouvez déplacer le fichier dans un autre dossier.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Pour utiliser votre photo dans votre application, vous pouvez créer un objet [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) qui peut être utilisé avec plusieurs fonctionnalités différentes des applications Windows universelles.

Vous devez d’abord inclure l’espace de noms [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) dans votre projet.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Appelez [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) pour obtenir un flux à partir du fichier image. Appelez [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) pour obtenir un décodeur d’image bitmap à partir du flux. Appelez ensuite [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) pour obtenir une représentation **SoftwareBitmap** de l’image.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Pour afficher l’image dans votre interface utilisateur, déclarez un contrôle [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) dans votre page XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Pour utiliser l’image bitmap logicielle dans votre page XAML, incluez l’espace de noms [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) dans votre projet.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

Le contrôle **Image** nécessite que la source d’image soit au format BGRA8 avec alpha prémultiplié ou sans alpha. Appelez la méthode statique [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) pour créer une image bitmap logicielle au format souhaité. Ensuite, créez un objet [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) et appelez [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) pour affecter l’image bitmap logicielle à la source. Enfin, définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) du contrôle **Image** pour afficher la photo capturée dans l’interface utilisateur.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Capturer une vidéo avec CameraCaptureUI

Pour capturer une vidéo, créez un objet [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). À l’aide de la propriété [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) de l’objet, vous pouvez spécifier les propriétés de la vidéo renvoyée, comme le format.

Appelez [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) et spécifiez [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) pour indiquer qu’une vidéo doit être capturée. La méthode renvoie une instance [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) contenant la vidéo si la capture est réussie. Si l’utilisateur annule la capture, l’objet renvoyé est null.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

Ce que vous faites du fichier vidéo capturé dépend du scénario pour votre application. Le reste de cet article décrit comment créer rapidement une composition multimédia à partir d’une ou plusieurs vidéos capturées et l’afficher dans votre interface utilisateur.

Tout d’abord, ajoutez un contrôle [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) dans lequel la composition vidéo s’affichera dans votre page XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Une fois le fichier vidéo renvoyé depuis l’interface utilisateur de capture de l’appareil photo, créez un objet [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) en appelant **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)**. Appelez la méthode **[Play](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** de la valeur par défaut **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** associée au **MediaPlayerElement** pour lire la vidéo.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Rubriquesassociées

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 
 

 




