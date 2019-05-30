---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: Cet article décrit comment utiliser les classes SceneAnalysisEffect et FaceDetectionEffect pour analyser le contenu du flux d’aperçu de capture multimédia.
title: Effets d’analyse de cadres caméra
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2dec80e4b79a93fc5b6f1e2990ad89570d62c5f1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361435"
---
# <a name="effects-for-analyzing-camera-frames"></a>Effets d’analyse de cadres caméra



Cet article décrit comment utiliser les classes [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) et [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) pour analyser le contenu du flux d’aperçu de capture multimédia.

## <a name="scene-analysis-effect"></a>Effet d’analyse de scène

La classe [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) analyse les trames vidéo au sein du flux d’aperçu de capture multimédia et recommande le traitement des options pour améliorer le résultat de la capture vidéo. Actuellement, l’effet prend en charge la fonctionnalité visant à détecter si la capture serait améliorée par l’utilisation du traitement HDR (High Dynamic Range).

Si l’effet recommande l’utilisation du traitement HDR, vous pouvez procéder comme suit :

-   Utilisez la classe [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) pour capturer des photos à l’aide de l’algorithme de traitement HDR intégré de Windows. Pour plus d’informations, voir [Capture photo avec plage dynamique élevée (HDR)](high-dynamic-range-hdr-photo-capture.md).

-   Utilisez la classe [**HdrVideoControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.HdrVideoControl) pour capturer une vidéo à l’aide de l’algorithme de traitement HDR intégré de Windows. Pour plus d’informations, voir [Contrôles de l’appareil de capture pour la vidéo](capture-device-controls-for-video-capture.md).

-   Utilisez la classe [**VariablePhotoSequenceControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) pour capturer une séquence de trames que vous pouvez ensuite composer à l’aide d’une implémentation HDR personnalisée. Pour plus d’informations, voir [Séquence de photos variables](variable-photo-sequence.md).

### <a name="scene-analysis-namespaces"></a>Espaces de noms d’analyse de scène

Pour utiliser l’analyse de scène, votre application doit inclure les espaces de noms suivants en plus de ceux nécessaires à la capture multimédia de base.

[!code-cs[SceneAnalysisUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalysisUsing)]

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>Initialiser l’effet d’analyse de scène et l’ajouter au flux d’aperçu

Les effets vidéo sont implémentés à l’aide de deux API, une dédiée à la définition d’effets, qui fournit les paramètres dont l’appareil de capture a besoin pour initialiser l’effet, et l’autre dédiée à l’instance d’effet, qui peut être utilisée pour contrôler l’effet. Pour accéder à l’instance d’effet à partir de plusieurs emplacements au sein de votre code, vous devez généralement déclarer une variable de membre pour stocker l’objet.

[!code-cs[DeclareSceneAnalysisEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSceneAnalysisEffect)]

Dans votre application, une fois l’objet **MediaCapture** initialisé, créez une instance de [**SceneAnalysisEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition).

Enregistrez l’effet avec l’appareil de capture en appelant [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) sur votre objet **MediaCapture**, en fournissant l’élément **SceneAnalysisEffectDefinition** et en spécifiant [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) pour indiquer que l’effet doit être appliqué au flux d’aperçu vidéo et non au flux de capture. **AddVideoEffectAsync** renvoie une instance de l’effet ajouté. Étant donné que cette méthode peut être utilisée avec plusieurs types d’effets, vous devez diffuser l’instance renvoyée sur un objet [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect).

Pour recevoir les résultats de l’analyse de scène, vous devez enregistrer un gestionnaire pour l’événement [**SceneAnalyzed**](https://docs.microsoft.com/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed).

Actuellement, l’effet d’analyse de scène inclut uniquement l’analyseur HDR. Activez l’analyse HDR en définissant la propriété [**HighDynamicRangeControl.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) de l’effet sur true.

[!code-cs[CreateSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateSceneAnalysisEffectAsync)]

### <a name="implement-the-sceneanalyzed-event-handler"></a>Implémenter le gestionnaire d’événements SceneAnalyzed

Les résultats de l’analyse de scène sont renvoyés dans le gestionnaire d’événements **SceneAnalyzed**. L’objet [**SceneAnalyzedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs) transmis au gestionnaire comporte un objet [**SceneAnalysisEffectFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame) contenant lui-même un objet [**HighDynamicRangeOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.HighDynamicRangeOutput). La propriété [**Certainty**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangeoutput.certainty) de la sortie HDR fournit une valeur comprise entre 0 et 1,0, où 0 indique que le traitement HDR ne permettrait pas d’améliorer le résultat de la capture et 1,0 que le traitement HDR pourrait l’améliorer. Vous pouvez choisir le seuil au niveau duquel vous voulez utiliser HDR ou montrer les résultats à l’utilisateur pour le laisser choisir.

[!code-cs[SceneAnalyzed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalyzed)]

L’objet [**HighDynamicRangeOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) transmis au gestionnaire dispose également d’une propriété [**FrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers) qui contient des contrôleurs de trame suggérés pour la capture d’une séquence de photos variables pour le traitement HDR. Pour plus d’informations, voir [Séquence de photos variables](variable-photo-sequence.md).

### <a name="clean-up-the-scene-analysis-effect"></a>Nettoyer l’effet d’analyse de scène

Une fois que votre application a terminé la capture et avant de supprimer l’objet **MediaCapture**, vous devez désactiver l’effet d’analyse de scène en définissant la propriété [**HighDynamicRangeAnalyzer.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) de l’effet sur false et annuler l’enregistrement de votre gestionnaire d’événements [**SceneAnalyzed**](https://docs.microsoft.com/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed). Appelez [**MediaCapture.ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) en indiquant le flux d’aperçu vidéo étant donné qu’il s’agissait du flux auquel l’effet a été ajouté. Enfin, définissez la variable de membre sur null.

[!code-cs[CleanUpSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpSceneAnalysisEffectAsync)]

## <a name="face-detection-effect"></a>Effet de détection des visages

La classe [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) identifie la position des visages au sein du flux d’aperçu de capture multimédia. L’effet vous permet de recevoir une notification chaque fois qu’un visage est détecté dans le flux d’aperçu et fournit le cadre englobant pour chaque visage détecté dans la trame d’aperçu. Sur les appareils pris en charge, l’effet de détection des visages propose également une amélioration de l’exposition et une mise au point sur le visage le plus important de la scène.

### <a name="face-detection-namespaces"></a>Espaces de noms de détection des visages

Pour utiliser la détection des visages, votre application doit inclure les espaces de noms suivants en plus de ceux nécessaires à la capture multimédia de base.

[!code-cs[FaceDetectionUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>Initialiser l’effet de détection des visages et l’ajouter au flux d’aperçu

Les effets vidéo sont implémentés à l’aide de deux API, une dédiée à la définition d’effets, qui fournit les paramètres dont l’appareil de capture a besoin pour initialiser l’effet, et l’autre dédiée à l’instance d’effet, qui peut être utilisée pour contrôler l’effet. Pour accéder à l’instance d’effet à partir de plusieurs emplacements au sein de votre code, vous devez généralement déclarer une variable de membre pour stocker l’objet.

[!code-cs[DeclareFaceDetectionEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareFaceDetectionEffect)]

Dans votre application, une fois l’objet **MediaCapture** initialisé, créez une instance de [**FaceDetectionEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition). Définissez la propriété [**DetectionMode**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode) de manière à privilégier une détection des visages plus rapide ou plus précise. Définissez [**SynchronousDetectionEnabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled) pour indiquer que les trames entrantes n’attendent pas que la détection des visages soit terminée afin de ne pas obtenir un aperçu « haché ».

Enregistrez l’effet avec l’appareil de capture en appelant [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) sur votre objet **MediaCapture**, en fournissant l’élément **FaceDetectionEffectDefinition** et en spécifiant [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) pour indiquer que l’effet doit être appliqué au flux d’aperçu vidéo et non au flux de capture. **AddVideoEffectAsync** renvoie une instance de l’effet ajouté. Étant donné que cette méthode peut être utilisée avec plusieurs types d’effets, vous devez diffuser l’instance renvoyée sur un objet [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect).

Activez ou désactivez l’effet en définissant la propriété [**FaceDetectionEffect.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.enabled). Ajustez la fréquence à laquelle l’effet analyse les trames en définissant la propriété [**FaceDetectionEffect.DesiredDetectionInterval**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval). Ces deux propriétés peuvent être ajustées lors de la capture multimédia.

[!code-cs[CreateFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFaceDetectionEffectAsync)]

### <a name="receive-notifications-when-faces-are-detected"></a>Recevoir des notifications une fois les visages détectés

Si vous voulez effectuer des actions une fois la détection effectuée, comme dessiner un cadre autour des visages détectés dans l’aperçu vidéo, vous pouvez vous enregistrer pour l’événement [**FaceDetected**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.facedetected).

[!code-cs[RegisterFaceDetectionHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterFaceDetectionHandler)]

Dans le gestionnaire de l’événement, vous pouvez obtenir une liste de tous les visages détectés dans une trame en accédant à la propriété [**FaceDetectionEffectFrame.DetectedFaces**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces) de la classe [**FaceDetectedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectedEventArgs). La propriété [**FaceBox**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.detectedface.facebox) est une structure [**BitmapBounds**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBounds) qui décrit le rectangle contenant le visage détecté sous forme d’unités par rapport aux dimensions du flux d’aperçu. Pour afficher l’exemple de code qui transforme les coordonnés du flux d’aperçu en coordonnées d’écran, voir l’[exemple de détection des visages UWP](https://go.microsoft.com/fwlink/?LinkId=619486).

[!code-cs[FaceDetected](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetected)]

### <a name="clean-up-the-face-detection-effect"></a>Nettoyer l’effet de détection des visages

Une fois que votre application a terminé la capture et avant de supprimer l’objet **MediaCapture**, vous devez désactiver l’effet de détection des visages à l’aide de l’élément [**FaceDetectionEffect.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.enabled) et annuler l’enregistrement de votre gestionnaire d’événements [**FaceDetected**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.facedetected), si vous en aviez enregistré un. Appelez [**MediaCapture.ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) en indiquant le flux d’aperçu vidéo étant donné qu’il s’agissait du flux auquel l’effet a été ajouté. Enfin, définissez la variable de membre sur null.

[!code-cs[CleanUpFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpFaceDetectionEffectAsync)]

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>Vérifier la prise en charge de la mise au point et de l’exposition pour la détection des visages

Tous les appareils ne disposent pas d’un appareil de capture permettant d’ajuster la mise au point et l’exposition en se basant sur les visages détectés. Dans la mesure où la détection des visages consomme les ressources de l’appareil, vous pouvez l’activer uniquement sur les appareils capables d’utiliser la fonctionnalité pour améliorer la capture. Pour savoir si l’optimisation de la capture basée sur les visages est disponible, obtenez la classe [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController) pour votre [MediaCapture](capture-photos-and-video-with-mediacapture.md) initialisé, puis procurez-vous la classe [**RegionsOfInterestControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) du contrôleur d’appareil vidéo. Vérifiez que la propriété [**MaxRegions**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) prend en charge au moins une région. Puis, assurez-vous que [**AutoExposureSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported) ou [**AutoFocusSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) sont définies sur true. Si ces conditions sont réunies, l’appareil peut tirer parti de la détection des visages pour améliorer la capture.

[!code-cs[AreFaceFocusAndExposureSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAreFaceFocusAndExposureSupported)]

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Photo de base, vidéo, audio et de capture à MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




