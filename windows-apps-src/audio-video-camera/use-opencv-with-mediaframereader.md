---
author: drewbatgit
ms.assetid: ''
description: Cet article vous montre comment utiliser la bibliothèque Open Source Computer Vision Library (OpenCV) avec la classe MediaFrameReader.
title: Utiliser OpenCV avec MediaFrameReader
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, openCV
ms.localizationpriority: medium
ms.openlocfilehash: 43545f2a8e1965124560479d399df79d247c5f05
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4351636"
---
# <a name="use-the-open-source-computer-vision-library-opencv-with-mediaframereader"></a>Utiliser la bibliothèque Open Source Computer Vision Library (OpenCV) avec MediaFrameReader

Cet article vous montre comment utiliser l’Open Source Computer Vision Library (OpenCV), une bibliothèque de code natif qui fournit un large éventail d'algorithmes de traitement d’image, avec la classe [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader), qui peut lire des images multimédias depuis plusieurs sources simultanées. L’exemple de code de cet article vous guide dans la création d’une application simple qui obtient des images à partir d’un capteur de couleur, rend chaque image floue à l’aide de la bibliothèque OpenCV et affiche ensuite l’image traitée dans un contrôle **Image** XAML. 

>[!NOTE]
>Les packages OpenCV.Win.Core et OpenCV.Win.ImgProc ne sont pas régulièrement mis à jour, mais sont toujours recommandés pour créer un OpenCVHelper, comme décrit dans cette page.

Cet article repose sur le contenu de deux autres articles:

* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md): cet article fournit des informations détaillées sur l’utilisation de **MediaFrameReader** pour obtenir des images à partir d’une ou de plusieurs images multimédias sources et décrit en détail l'essentiel de l'exemple de code de cet article. En particulier, **Traiter des images multimédias avec MediaFrameReader** fournit la description du code pour une classe d’assistance, **FrameRenderer**, qui gère la présentation des images multimédias dans un élément **Image** XAML. L’exemple de code dans cet article utilise également cette classe d’assistance.

* [Traiter des images bitmap logicielles avec OpenCV](process-software-bitmaps-with-opencv.md): cet article vous guide dans la création d’un composant Windows Runtime en code natif, **OpenCVBridge**, qui permet d’effectuer une conversion entre l'objet **SoftwareBitmap**, utilisé par le **MediaFrameReader**, et le type **Mat**, utilisé par la bibliothèque OpenCV. L’exemple de code de cet article suppose que vous avez suivi les étapes permettant d'ajouter le composant **OpenCVBridge** à votre solution d’application UWP.

Outre ces articles, pour afficher et télécharger un exemple de travail complet, de bout en bout, du scénario décrit dans cet article, voir la [Profils d’appareil photo + OpenCV](https://go.microsoft.com/fwlink/?linkid=854003) dans le référentiel Windows Universal Samples GitHub.

Pour commencer à développer rapidement, vous pouvez inclure la bibliothèque OpenCV dans un projet d’application UWP à l’aide de packages NuGet, mais ces packages peut ne pas passent le processus d’application certficication lorsque vous soumettez votre application au Windows Store, afin qu’il est recommandé de télécharger le OpenCV bibliothèque de code source et créer les fichiers binaires avant de soumettre votre application. Vous trouverez des informations sur le développement avec OpenCV à l’adresse [http://opencv.org](http://opencv.org)


## <a name="implement-the-opencvhelper-native-windows-runtime-component"></a>Implémenter le composant Windows Runtime natif OpenCVHelper
Suivez les étapes de [Traiter des images bitmap logicielles avec OpenCV](process-software-bitmaps-with-opencv.md) pour créer le composant Windows Runtime d'application d’assistance OpenCV et ajoutez une référence au projet de composant à votre solution d’application UWP.

## <a name="find-available-frame-source-groups"></a>Rechercher des groupes de sources d’images disponibles
Tout d’abord, vous devez trouver un groupe de sources d’images multimédias à partir duquel des images multimédias seront obtenues. Récupérez la liste des groupes de sources disponibles sur l’appareil actuel en appelant **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)**. Sélectionnez ensuite les groupes de sources qui fournissent les types de capteur requis pour votre scénario d’application. Pour cet exemple, nous avons simplement besoin d'un groupe de sources qui fournit des images à partir d’une caméra RVB.

[!code-cs[OpenCVFrameSourceGroups](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameSourceGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialiser l’objet MediaCapture
Vous devez ensuite initialiser l'objet **MediaCapture** pour utiliser le groupe de sources d’images sélectionné à l’étape précédente, en définissant la propriété **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** des **MediaCaptureInitializationSettings**.

> [!NOTE] 
> La technique utilisée par le composant OpenCVHelper, décrite en détail dans [Traiter des images bitmap logicielles avec OpenCV](process-software-bitmaps-with-opencv.md), requiert que les données d’image à traiter résident dans la mémoire du processeur, pas dans celle du GPU. Vous devez donc spécifier **MemoryPreference.CPU** dans le champ **[MemoryPreference](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.MemoryPreference)** des **MediaCaptureInitializationSettings**.

Une fois l'objet **MediaCapture** initialisé avec succès, obtenez une référence à la source d’images RVB en accédant à la propriété **[MediaCapture.FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)**.

[!code-cs[OpenCVInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVInitMediaCapture)]

## <a name="initialize-the-mediaframereader"></a>Initialiser le MediaFrameReader
Ensuite, créez un [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) pour la source d’images RVB récupérée à l’étape précédente. Afin de conserver une fréquence d’images correcte, vous voudrez peut-être traiter les images dont la résolution est inférieure à celle du capteur. Cet exemple fournit l’argument optionnel **[BitmapSize](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapsize)** à la méthode **[MediaCapture.CreateFrameReaderAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync)** pour demander que les images fournies par le lecteur d’images soient redimensionnées à 640x480pixels.

Après avoir créé le lecteur d’images, enregistrez un gestionnaire pour l'événement **[FrameArrived](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.FrameArrived)**. Créez ensuite un nouvel objet **[SoftwareBitmapSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource)** que la classe d’assistance **FrameRenderer** utilisera pour présenter l’image traitée. Appelez ensuite le constructeur du **FrameRenderer**. Initialiser l’instance de la classe **OpenCVHelper** définie dans le composant Windows Runtime OpenCVBridge. Cette classe d’assistance est utilisée dans le gestionnaire **FrameArrived** pour traiter chaque image. Enfin, démarrez le lecteur d'images en appelant **[StartAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.StartAsync)**.

[!code-cs[OpenCVFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameReader)]


## <a name="handle-the-framearrived-event"></a>Gérer l’événement FrameArrived
L’événement **FrameArrived** est déclenché quand une nouvelle image est disponible depuis le lecteur d'images. Appelez **[TryAcquireLatestFrame](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.TryAcquireLatestFrame)** pour obtenir l’image, si elle existe. Obtenez le **SoftwareBitmap** à partir de la **[MediaFrameReference](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference)**. Notez que la classe **CVHelper** utilisée dans cet exemple oblige les images à utiliser le format de pixels BRGA8 avec alpha prémultiplié. Si le bloc transmis dans l’événement est d'un format différent, convertissez le **SoftwareBitmap** au format approprié. Créez ensuite un **SoftwareBitmap** pour servir de cible à l’opération de flou. Les propriétés de l’image source sont utilisées comme arguments au constructeur pour créer une image bitmap avec format correspondant. Appelez la classe d’assistance de la méthode **Blur** pour traiter l'image. Enfin, passez l’image résultant de l’opération de flou dans **PresentSoftwareBitmap**, la méthode de la classe d’assistance **FrameRenderer** qui affiche l’image dans le contrôle **Image** XAML avec lequel elle a été initialisée.

[!code-cs[OpenCVFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.OpenCV.xaml.cs#SnippetOpenCVFrameArrived)]

## <a name="related-topics"></a>Rubriquesassociées

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Traiter les images bitmap logicielles avec OpenCV](process-software-bitmaps-with-opencv.md)
* [Profils d’appareil photo](http://go.microsoft.com/fwlink/?LinkId=823230)
* [Profils d’appareil photo + OpenCV](https://go.microsoft.com/fwlink/?linkid=854003)
 

 




