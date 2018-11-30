---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: Cet article vous explique comment utiliser un objet MediaFrameReader avec MediaCapture pour obtenir des AudioFrames contenant des données audio à partir d’une source de capture.
title: Traiter des trames audio avec MediaFrameReader
ms.date: 04/18/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f85570d5c66db1641ec6352526d4db6213e199b4
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8330047"
---
# <a name="process-audio-frames-with-mediaframereader"></a>Traiter des trames audio avec MediaFrameReader

Cet article vous explique comment utiliser un objet [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) avec [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) pour obtenir des données audio à partir d’une source d’images multimédias. Pour plus d’informations sur l’utilisation d’un objet **MediaFrameReader** pour obtenir des données d’image, notamment à partir d’un appareil photo couleur, infrarouge ou de profondeur, consultez [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md). Cet article donne une vue d’ensemble du modèle d’utilisation du lecteur d’images et décrit certaines fonctionnalités supplémentaires de la classe **MediaFrameReader**, notamment l’utilisation de **MediaFrameSourceGroup** pour récupérer simultanément des images de plusieurs sources. 

> [!NOTE] 
> Les fonctionnalités décrites dans cet article sont disponibles uniquement à partir de Windows10, version1803.

> [!NOTE] 
> Il existe un exemple d’application Windows universelle qui illustre l’utilisation de **MediaFrameReader** pour afficher des images de différentes sources, notamment d’appareils photos couleur, de profondeur et infrarouges. Pour plus d’informations voir [Profils d’appareil photo](http://go.microsoft.com/fwlink/?LinkId=823230).

## <a name="setting-up-your-project"></a>Configuration de votre projet
Le processus d’acquisition de trames audio est en grande partie identique à l’acquisition d’autres types d’images multimédias. Comme avec toute application utilisant **MediaCapture**, vous devez déclarer que votre application utilise la fonctionnalité *webcam* avant de tenter d’accéder à un appareil photo. Si votre application capture à partir d’un périphérique audio, vous devez également déclarer la fonctionnalité *microphone*. 

**Ajouter des fonctionnalités au manifeste de l’application**

1.  Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2.  Sélectionnez l’onglet **Fonctionnalités**.
3.  Activez les cases à cocher **Webcam** et **Microphone**.
4.  Pour accéder à la bibliothèque d’images et à la vidéothèque, cochez les cases correspondant à **Bibliothèque d’images** et **Vidéothèque**.



## <a name="select-frame-sources-and-frame-source-groups"></a>Sélectionner des sources d’images et des groupes de sources d’images

La première étape de la capture de trames audio consiste à initialiser un objet [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource) représentant la source des données audio, par exemple un microphone ou tout autre appareil de capture audio. Pour ce faire, vous devez créer une instance de l’objet [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). Pour cet exemple, le seul paramètre d’initialisation pour l’objet **MediaCapture** est de définir l’objet [**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) pour indiquer que la diffusion audio doit être effectuée à partir de l’appareil de capture. 

Après avoir appelé la méthode [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync), vous pouvez obtenir la liste des sources d’images multimédias accessibles avec la propriété [**FrameSources**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.framesources). Cet exemple utilise une requête Linq pour sélectionner toutes les sources d’images où l’objet [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo) décrivant la source d’images a un[**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) défini sur **Audio**, pour indiquer que la source produit des données audio.

Si la requête retourne une ou plusieurs sources d’images, vous pouvez examiner la propriété [**CurrentFormat**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) pour déterminer si la source prend en charge le format audio souhaité, en l’occurrence, les données audio de type float. Examinez la propriété [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) pour vérifier que l’encodage audio souhaité est pris en charge par la source.

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>Créer et démarrer l’objet MediaFrameReader

Obtenez une nouvelle instance de **MediaFrameReader** en appelant [**MediaCapture.CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_), qui passe l’objet **MediaFrameSource** que vous avez sélectionné dans l’étape précédente. Par défaut, les trames audio sont obtenues en mode d’acquisition en mémoire tampon, il est donc peu probable que les images soient perdues, bien que cela puisse se produire si vous ne traitez pas les trames audio assez rapidement et saturez la mémoire tampon allouée du système.

Enregistrez un gestionnaire pour l’événement [**MediaFrameReader.FrameArrived**](*https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived), qui est déclenché par le système quand une nouvelle trame de données audio est disponible. Appelez [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) pour commencer l’acquisition des trames audio. Si le lecteur d’images ne parvient pas à démarrer, la valeur d’état retournée par l’appel aura une valeur autre que [**Success**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus).

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

Dans le gestionnaire d’événements **FrameArrived**, appelez [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) sur l’objet **MediaFrameReader** transmis comme expéditeur au gestionnaire pour tenter de récupérer une référence à la dernière image multimédia. Notez que cet objet peut être null, vous devez donc toujours vérifier avant d’utiliser l’objet. Le type d’image multimédia encapsulée dans l’objet **MediaFrameReference** renvoyé par **TryAcquireLatestFrame** dépend du type de source(s) d’images pour lesquelles vous avez configuré le lecteur d’images pour l’acquisition. Comme le lecteur d’images dans cet exemple a été configuré pour l’acquisition de trames audio, il obtient la trame sous-jacente à l’aide de la propriété [**AudioMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe). 

La méthode d’assistance **ProcessAudioFrame** dans l’exemple ci-dessous montre comment obtenir un objet [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) qui fournit des informations telles que l’horodatage de l’image et indique si elle est discontinue de l’objet **AudioMediaFrame**. Pour lire ou traiter les exemples de données audio, vous devez obtenir l’objet [**AudioBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer) à partir de l’objet **AudioMediaFrame**, créer un objet [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference) et appeler la méthode COM **IMemoryBufferByteAccess::GetBuffer** pour récupérer les données. Pour plus d’informations sur l’accès aux mémoires tampons natives, consultez la note sous le listing du code.

Le format des données dépend de la source d’images. Dans cet exemple, lorsque vous sélectionnez une source d’images multimédias, nous nous sommes explicitement assurés que la source d’images sélectionnée utilise un canal unique de données de type float. Le reste de l’exemple de code montre comment déterminer la durée et le nombre d’échantillons pour les données audio de la trame.  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> Pour effectuer des opérations sur les données audio, vous devez accéder à une mémoire tampon native. Pour ce faire, vous devez utiliser l’interface COM **IMemoryBufferByteAccess** en incluant le listing du code ci-dessous. Les opérations sur la mémoire tampon native doivent être exécutées dans une méthode qui utilise le mot clé **unsafe**. Vous devez également cocher la case pour autoriser le code unsafe sous l'onglet **Build** de la boîte de dialogue **Projet -> Propriétés**.

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>Informations supplémentaires sur l’utilisation de MediaFrameReader avec des données audio

Vous pouvez récupérer l’objet [**AudioDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.AudioDeviceController) associé à la source de trames audio en accédant à la propriété [**MediaFrameSource.Controller**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.controller). Cet objet peut être utilisé pour obtenir ou définir les propriétés de flux de l’appareil de capture ou pour contrôler le niveau de capture. L’exemple suivant désactive le périphérique audio afin que le lecteur d’images continue à acquérir des images, mais tous les échantillons ont la valeur 0.

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

Vous pouvez utiliser un objet [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) pour transmettre les données audio capturées par une source d’images multimédias dans un objet [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph). Transmettez la trame dans la méthode [**AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) d’un objet [**AudioFrameInputNode**](https://docs.microsoft.com/en-us/uwp/api/windows.media.audio.audioframeinputnode). Pour plus d’informations sur l’utilisation de graphiques audio pour capturer, traiter et combiner des signaux audio, voir [Graphiques audio](audio-graphs.md).

## <a name="related-topics"></a>Rubriquesassociées

* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Caméra](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Profils d’appareil photo](http://go.microsoft.com/fwlink/?LinkId=823230)
* [Graphiques audio](audio-graphs.md)
 






