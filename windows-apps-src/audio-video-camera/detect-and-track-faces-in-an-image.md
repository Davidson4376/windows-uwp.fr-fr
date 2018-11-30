---
ms.assetid: 84729E44-10E9-4D7D-8575-6A9D97467ECD
description: Cette rubrique montre comment utiliser FaceDetector pour détecter des visages dans une images. FaceTracker est optimisé pour suivre les visages au fil du temps dans une séquence d’images vidéo.
title: Détecter les visages dans des images ou des vidéos
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 970fbcef984f28e779ea7c133de95f7f7f99be8d
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8330067"
---
# <a name="detect-faces-in-images-or-videos"></a>Détecter les visages dans des images ou des vidéos



\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

Cette rubrique montre comment utiliser [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) pour détecter des visages dans une image. [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) est optimisé pour suivre les visages au fil du temps dans une séquence d’images vidéo.

Pour une autre méthode de suivi des visages à l’aide de [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/dn948776), voir [Analyse de scène pour la capture multimédia](scene-analysis-for-media-capture.md).

Le code contenu dans cet article a été adapté à partir des exemples [Détection des visages de base](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409) et [Suivi des visages de base](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409). Vous pouvez télécharger ces exemples pour voir le code utilisé en contexte ou pour vous en servir comme point de départ pour votre propre application.

## <a name="detect-faces-in-a-single-image"></a>Détecter des visages dans une seule image

La classe [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) vous permet de détecter un ou plusieurs visages dans une image fixe.

Cet exemple utilise des API à partir des espaces de noms suivants.

[!code-cs[FaceDetectionUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

Déclarez une variable de membre de classe pour l’objet [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) et la liste d’objets [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) qui seront détectés dans l’image.

[!code-cs[ClassVariables1](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables1)]

La détection des visages fonctionne sur un objet [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) qui peut être créé de diverses manières. Dans cet exemple un élément [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) est utilisé pour permettre à l’utilisateur de choisir un fichier image dans lequel des visages sont détectés. Pour plus d’informations sur l’utilisation d’images bitmap de logiciel, voir [Acquisition d’images](imaging.md).

[!code-cs[Picker](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetPicker)]

Utilisez la classe [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) pour décoder le fichier image dans un **SoftwareBitmap**. Le processus de détection des visages est plus rapide avec une image plus petite, c’est pourquoi vous voudrez peut-être réduire la taille de l’image source. Cette opération peut être réalisée lors du décodage en créant un objet [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254), en définissant les propriétés [**ScaledWidth**](https://msdn.microsoft.com/library/windows/apps/br226261) et [**ScaledHeight**](https://msdn.microsoft.com/library/windows/apps/br226260) et en les transmettant à l’appel à [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332), qui renvoie l’élément **SoftwareBitmap** décodé et mis à l’échelle.

[!code-cs[Decode](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDecode)]

Dans la version actuelle, la classe **FaceDetector** prend uniquement en charge les images dans Gray8 ou Nv12. La classe **SoftwareBitmap** fournit la méthode [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362), qui convertit une image bitmap d’un format vers un autre. Cet exemple convertit l’image source au format de pixel Gray8 si elle n’est pas déjà dans ce format. Si vous le souhaitez, vous pouvez utiliser les méthodes [**GetSupportedBitmapPixelFormats**](https://msdn.microsoft.com/library/windows/apps/dn974140) et [**IsBitmapPixelFormatSupported**](https://msdn.microsoft.com/library/windows/apps/dn974142) pour déterminer lors de l’exécution si un format de pixel est pris en charge, au cas où l’ensemble des formats pris en charge est étendu dans les prochaines versions.

[!code-cs[Format](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFormat)]

Instanciez l’objet **FaceDetector** en appelant [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974132), puis appelez [**DetectFacesAsync**](https://msdn.microsoft.com/library/windows/apps/dn974134), en transmettant l’image bitmap mise à l’échelle à une taille raisonnable et convertie en un format de pixel pris en charge. Cette méthode renvoie une liste d’objets [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123). **ShowDetectedFaces** est une méthode d’assistance, indiquée ci-dessous, qui dessine des cadres autour des visages de l’image.

[!code-cs[Detect](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDetect)]

Veillez à supprimer les objets qui ont été créés au cours du processus de détection des visages.

[!code-cs[Dispose](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetDispose)]

Pour afficher l’image et dessiner des cadres autour des visages détectés, ajoutez un élément [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) à votre page XAML.

[!code-xml[Canvas](./code/FaceDetection_Win10/cs/MainPage.xaml#SnippetCanvas)]

Définissez des variables de membre pour styliser les cadres dessinés.

[!code-cs[ClassVariables2](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables2)]

Dans la méthode d’assistance **ShowDetectedFaces**, un nouvel élément [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) est créé et la source est définie sur un élément [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) créé à partir de **SoftwareBitmap** représentant l’image source. L’arrière-plan du contrôle **Canvas** XAML est défini sur le pinceau image.

Si la liste des visages transmise à la méthode d’assistance n’est pas vide, passez en boucle sur chaque visage de la liste et utilisez la propriété [**FaceBox**](https://msdn.microsoft.com/library/windows/apps/dn974126) de la classe [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) pour déterminer la position et la taille du rectangle au sein de l’image qui contient le visage. Dans la mesure où la taille du contrôle **Canvas** est très probablement différente de celle de l’image source, vous devez multiplier les coordonnées X et Y, ainsi que la largeur et la hauteur de **FaceBox** par une valeur de mise à l’échelle qui est le rapport entre la taille de l’image source et la taille réelle du contrôle **Canvas**.

[!code-cs[ShowDetectedFaces](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetShowDetectedFaces)]

## <a name="track-faces-in-a-sequence-of-frames"></a>Suivre les visages dans une séquence d’images

Si vous voulez détecter les visages d’une vidéo, il est plus efficace d’utiliser la classe [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) que la classe [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129), bien que les étapes d’implémentation soient très similaires. **FaceTracker** utilise les informations d’images traitées précédemment pour optimiser le processus de détection.

[!code-cs[FaceTrackingUsing](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetFaceTrackingUsing)]

Déclarez une variable de classe pour l’objet **FaceTracker**. Cet exemple utilise un élément [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) pour lancer un suivi des visages sur un intervalle défini. [SemaphoreSlim](https://msdn.microsoft.com/library/system.threading.semaphoreslim.aspx) est utilisé pour s’assurer qu’une seule opération de détection des visages s’exécute à la fois.

[!code-cs[ClassVariables3](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetClassVariables3)]

Pour initialiser l’opération de suivi des visages, créez un nouvel objet **FaceTracker** en appelant [**CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn974151). Initialisez l’intervalle de minuterie souhaité, puis créez le minuteur. La méthode d’assistance **ProcessCurrentVideoFrame** est appelée dès que l’intervalle spécifié est écoulé.

[!code-cs[TrackingInit](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetTrackingInit)]

L’application d’assistance **ProcessCurrentVideoFrame** est appelée de manière asynchrone par le minuteur, si bien qu’elle appelle d’abord la méthode **Wait** du sémaphore pour voir si une opération de suivi est en cours, et si tel est le cas, la méthode revient sans tenter de détecter les visages. À la fin de cette méthode, la méthode **Release** du sémaphore est appelée, ce qui permet la poursuite de l’appel consécutif à **ProcessCurrentVideoFrame**.

La classe [**FaceTracker**](https://msdn.microsoft.com/library/windows/apps/dn974150) fonctionne sur les objets [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917). Vous pouvez obtenir un élément **VideoFrame** en capturant une image d’aperçu à partir d’un objet [MediaCapture](capture-photos-and-video-with-mediacapture.md) en cours d’exécution ou en implémentant la méthode [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) de [**IBasicVideoEffect**](https://msdn.microsoft.com/library/windows/apps/dn764788). Cet exemple utilise une méthode d’assistance non définie qui renvoie une image vidéo, **GetLatestFrame**, sous la forme d’un espace réservé pour cette opération. Pour en savoir plus sur l’obtention d’images vidéo à partir du flux d’aperçu d’un appareil de capture multimédia en cours d’exécution, voir [Obtenir une image d’aperçu](get-a-preview-frame.md).

Comme avec **FaceDetector**, **FaceTracker** prend en charge un ensemble limité de formats de pixels. Cet exemple abandonne la détection des visages si l’image fournie n’est pas au format Nv12.

Appelez [**ProcessNextFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn974157) pour récupérer une liste d’objets [**DetectedFace**](https://msdn.microsoft.com/library/windows/apps/dn974123) représentant les visages dans l’image. Une fois que vous disposez de la liste des visages, vous pouvez les afficher de la même manière que celle décrite ci-dessus pour la détection des visages. Notez que dans la mesure où la méthode d’assistance de suivi des visages n’est pas appelée sur le thread d’interface utilisateur, vous devez effectuer les mises à jour de l’interface utilisateur dans une méthode [**CoredDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) d’appel.

[!code-cs[ProcessCurrentVideoFrame](./code/FaceDetection_Win10/cs/MainPage.xaml.cs#SnippetProcessCurrentVideoFrame)]

## <a name="related-topics"></a>Rubriquesconnexes

* [Analyse de scène de capture multimédia](scene-analysis-for-media-capture.md)
* [Exemple de détection des visages de base](http://go.microsoft.com/fwlink/p/?LinkId=620512&clcid=0x409)
* [Exemple de suivi des visages de base](http://go.microsoft.com/fwlink/p/?LinkId=620513&clcid=0x409)
* [Camera](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Lecture de contenu multimédia](media-playback.md)
