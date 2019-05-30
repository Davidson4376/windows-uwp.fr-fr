---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: Cette rubrique montre comment utiliser FaceDetector pour détecter des visages dans une images. FaceTracker est optimisé pour suivre les visages au fil du temps dans une séquence d’images vidéo.
title: Détecter les visages dans des images ou des vidéos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1be694be412bbf0a4e076e8ac5753eefda74c55
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360908"
---
# <a name="detect-faces-in-images-or-videos"></a>Détecter les visages dans des images ou des vidéos



\[Certaines informations sont relatives à la version préliminaire du produit qui peut être substantiellement modifié avant sa commercialisation. Microsoft exclut toute garantie, expresse ou implicite, concernant les informations fournies ici.\]

Cette rubrique montre comment utiliser [**FaceDetector**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) pour détecter des visages dans une image. [  **FaceTracker**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) est optimisé pour suivre les visages au fil du temps dans une séquence d’images vidéo.

Pour une autre méthode de suivi des visages à l’aide de [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect), voir [Analyse de scène pour la capture multimédia](scene-analysis-for-media-capture.md).

Le code contenu dans cet article a été adapté à partir des exemples [Détection des visages de base](https://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409) et [Suivi des visages de base](https://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409). Vous pouvez télécharger ces exemples pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

## <a name="detect-faces-in-a-single-image"></a>Détecter des visages dans une seule image

La classe [**FaceDetector**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) vous permet de détecter un ou plusieurs visages dans une image fixe.

Cet exemple utilise des API à partir des espaces de noms suivants.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

Déclarez une variable de membre de classe pour l’objet [**FaceDetector**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) et la liste d’objets [**DetectedFace**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) qui seront détectés dans l’image.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

La détection des visages fonctionne sur un objet [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) qui peut être créé de diverses manières. Dans cet exemple un élément [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) est utilisé pour permettre à l’utilisateur de choisir un fichier image dans lequel des visages sont détectés. Pour plus d’informations sur l’utilisation d’images bitmap de logiciel, voir [Acquisition d’images](imaging.md).

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

Utilisez la classe [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) pour décoder le fichier image dans un **SoftwareBitmap**. Le processus de détection des visages est plus rapide avec une image plus petite, c’est pourquoi vous voudrez peut-être réduire la taille de l’image source. Cette opération peut être réalisée lors du décodage en créant un objet [**BitmapTransform**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTransform), en définissant les propriétés [**ScaledWidth**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmaptransform.scaledwidth) et [**ScaledHeight**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmaptransform.scaledheight) et en les transmettant à l’appel à [**GetSoftwareBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync), qui renvoie l’élément **SoftwareBitmap** décodé et mis à l’échelle.

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

Dans la version actuelle, la classe **FaceDetector** prend uniquement en charge les images dans Gray8 ou Nv12. La classe **SoftwareBitmap** fournit la méthode [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows), qui convertit une image bitmap d’un format vers un autre. Cet exemple convertit l’image source au format de pixel Gray8 si elle n’est pas déjà dans ce format. Si vous le souhaitez, vous pouvez utiliser les méthodes [**GetSupportedBitmapPixelFormats**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.facedetector.getsupportedbitmappixelformats) et [**IsBitmapPixelFormatSupported**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.facedetector.isbitmappixelformatsupported) pour déterminer lors de l’exécution si un format de pixel est pris en charge, au cas où l’ensemble des formats pris en charge est étendu dans les prochaines versions.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

Instanciez l’objet **FaceDetector** en appelant [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.facedetector.createasync), puis appelez [**DetectFacesAsync**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.facedetector.detectfacesasync), en transmettant l’image bitmap mise à l’échelle à une taille raisonnable et convertie en un format de pixel pris en charge. Cette méthode renvoie une liste d’objets [**DetectedFace**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.DetectedFace). **ShowDetectedFaces** est une méthode d’assistance, indiquée ci-dessous, qui dessine des cadres autour des visages de l’image.

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

Veillez à supprimer les objets qui ont été créés au cours du processus de détection des visages.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

Pour afficher l’image et dessiner des cadres autour des visages détectés, ajoutez un élément [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) à votre page XAML.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

Définissez des variables de membre pour styliser les cadres dessinés.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

Dans la méthode d’assistance **ShowDetectedFaces**, un nouvel élément [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) est créé et la source est définie sur un élément [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) créé à partir de **SoftwareBitmap** représentant l’image source. L’arrière-plan du contrôle **Canvas** XAML est défini sur le pinceau image.

Si la liste des visages transmise à la méthode d’assistance n’est pas vide, passez en boucle sur chaque visage de la liste et utilisez la propriété [**FaceBox**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.detectedface.facebox) de la classe [**DetectedFace**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) pour déterminer la position et la taille du rectangle au sein de l’image qui contient le visage. Dans la mesure où la taille du contrôle **Canvas** est très probablement différente de celle de l’image source, vous devez multiplier les coordonnées X et Y, ainsi que la largeur et la hauteur de **FaceBox** par une valeur de mise à l’échelle qui est le rapport entre la taille de l’image source et la taille réelle du contrôle **Canvas**.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>Suivre les visages dans une séquence d’images

Si vous voulez détecter les visages d’une vidéo, il est plus efficace d’utiliser la classe [**FaceTracker**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) que la classe [**FaceDetector**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector), bien que les étapes d’implémentation soient très similaires. **FaceTracker** utilise les informations d’images traitées précédemment pour optimiser le processus de détection.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

Déclarez une variable de classe pour l’objet **FaceTracker**. Cet exemple utilise un élément [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) pour lancer un suivi des visages sur un intervalle défini. [SemaphoreSlim](https://docs.microsoft.com/dotnet/api/system.threading.semaphoreslim?redirectedfrom=MSDN) est utilisé pour s’assurer qu’une seule opération de détection des visages s’exécute à la fois.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

Pour initialiser l’opération de suivi des visages, créez un nouvel objet **FaceTracker** en appelant [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.facetracker.createasync). Initialisez l’intervalle de minuterie souhaité, puis créez le minuteur. La méthode d’assistance **ProcessCurrentVideoFrame** est appelée dès que l’intervalle spécifié est écoulé.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

L’application d’assistance **ProcessCurrentVideoFrame** est appelée de manière asynchrone par le minuteur, si bien qu’elle appelle d’abord la méthode **Wait** du sémaphore pour voir si une opération de suivi est en cours, et si tel est le cas, la méthode revient sans tenter de détecter les visages. À la fin de cette méthode, la méthode **Release** du sémaphore est appelée, ce qui permet la poursuite de l’appel consécutif à **ProcessCurrentVideoFrame**.

La classe [**FaceTracker**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceTracker) fonctionne sur les objets [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame). Vous pouvez obtenir un élément **VideoFrame** en capturant une image d’aperçu à partir d’un objet [MediaCapture](capture-photos-and-video-with-mediacapture.md) en cours d’exécution ou en implémentant la méthode [**ProcessFrame**](https://docs.microsoft.com/uwp/api/windows.media.effects.ibasicaudioeffect.processframe) de [**IBasicVideoEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IBasicVideoEffect). Cet exemple utilise une méthode d’assistance non définie qui renvoie une image vidéo, **GetLatestFrame**, sous la forme d’un espace réservé pour cette opération. Pour en savoir plus sur l’obtention d’images vidéo à partir du flux d’aperçu d’un appareil de capture multimédia en cours d’exécution, voir [Obtenir une image d’aperçu](get-a-preview-frame.md).

Comme avec **FaceDetector**, **FaceTracker** prend en charge un ensemble limité de formats de pixels. Cet exemple abandonne la détection des visages si l’image fournie n’est pas au format Nv12.

Appelez [**ProcessNextFrameAsync**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.facetracker.processnextframeasync) pour récupérer une liste d’objets [**DetectedFace**](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.DetectedFace) représentant les visages dans l’image. Une fois que vous disposez de la liste des visages, vous pouvez les afficher de la même manière que celle décrite ci-dessus pour la détection des visages. Notez que dans la mesure où la méthode d’assistance de suivi des visages n’est pas appelée sur le thread d’interface utilisateur, vous devez effectuer les mises à jour de l’interface utilisateur dans une méthode [**CoredDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) d’appel.

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>Rubriques connexes

* [Analyse de la scène pour la capture de média](scene-analysis-for-media-capture.md)
* [Exemple de détection de visage de base](https://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [Exemple de suivi de visage de base](https://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Lecture de contenu multimédia](media-playback.md)
