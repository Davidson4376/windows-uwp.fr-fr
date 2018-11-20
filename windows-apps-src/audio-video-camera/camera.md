---
author: drewbatgit
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: Cet article répertorie les fonctionnalités d’appareil photo disponibles pour les applications UWP et renvoie vers les articles de procédures décrivant leur utilisation.
title: Caméra
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d330a762844e4889d0da9ae5457acc8d9f0c2b91
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7292007"
---
# <a name="camera"></a>Caméra

Cette section fournit des recommandations relatives à la création d’application de la plateforme Windows universelle qui utilise l’appareil photo ou le microphone pour capturer des photos, des vidéos et du contenu audio.

## <a name="use-the-windows-built-in-camera-ui"></a>Utiliser l’interface utilisateur de l’appareil photo intégré à Windows

| Rubrique | Description |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturer des photos et des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows](capture-photos-and-video-with-cameracaptureui.md) | Cet article décrit comment utiliser la classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI) afin de capturer des photos ou des vidéos à l’aide de l’interface utilisateur de l’appareil photo intégré à Windows. Si vous souhaitez simplement permettre à l’utilisateur de capturer une photo ou une vidéo et de renvoyer les résultats à votre application, il s’agit du moyen le plus simple et le plus rapide de procéder.  |

## <a name="basic-mediacapture-tasks"></a>Tâches de base MediaCapture

| Rubrique | Description |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Afficher l’aperçu de l’appareil photo](simple-camera-preview-access.md) | Cet article décrit comment afficher rapidement le flux d’aperçu d’appareil photo au sein de la page XAML d’une application UWP. |
| [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Cet article vous présente le moyen le plus simple de capturer des photos et des vidéos à l’aide de la classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). La classe **MediaCapture** expose un jeu robuste d’API qui fournit un contrôle de niveau inférieur sur le pipeline de capture et prend en charge des scénarios de capture avancés, mais cet article est destiné à vous aider à ajouter rapidement et facilement la capture multimédia à votre application. |
| [Fonctionnalités d’interface utilisateur d’appareil photo pour les appareils mobiles](camera-ui-features-for-mobile-devices.md) | Cet article vous explique comment valoriser les fonctionnalités spécifiques d’interface utilisateur présentes de manière exclusive sur les appareils mobiles.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Tâches avancées MediaCapture   
                                                                                                               
| Rubrique                                                                                             | Description                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gérer l’orientation de l’appareil et de l’écran à l’aide de MediaCapture](handle-device-orientation-with-mediacapture.md) | Cet article vous explique comment gérer l’orientation de l’appareil quand vous capturez des photos et des vidéos à l’aide d’une classe d’assistance. | 
| [Détecter et sélectionner des fonctions de l’appareil photo à l’aide de profils d’appareil photo](camera-profiles.md) | Cet article explique comment utiliser les profils d’appareil photo pour découvrir et gérer les fonctionnalités des différents appareils de capture vidéo. Cela inclut les tâches telles que la sélection des profils qui prennent en charge des résolutions ou de fréquences d’images spécifiques, des profils qui prennent en charge un accès simultané à plusieurs appareils photos et des profils qui prennent en charge la capture HDR. |
| [Définir le format, la résolution et fréquence d’images pour MediaCapture](set-media-encoding-properties.md) | Cet article vous montre comment utiliser l’interface [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) pour définir la résolution et la fréquence d’images du flux d’aperçu d’appareil photo et des photos et vidéos capturées. Il montre également comment s’assurer que les proportions du flux d’aperçu correspondent à celle du média capturé. |
| [Capture photo HDR et en basse lumière](high-dynamic-range-hdr-photo-capture.md) | Cet article vous explique comment utiliser la classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) pour capturer des photos avec plage dynamique élevée(HDR)et en basse lumière. |
| [Contrôles d’appareil photo manuel pour la capture photo et vidéo](capture-device-controls-for-photo-and-video-capture.md) | Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture photo et vidéo, y compris la stabilisation d’image optique et le zoom fluide. |
| [Contrôles d’appareil photo manuel pour la capture vidéo](capture-device-controls-for-video-capture.md) | Cet article vous montre comment utiliser les contrôles des appareils manuels pour activer les scénarios de capture vidéo, y compris la vidéo HDR et la priorité de l’exposition.  |
| [Effet de stabilisation vidéo pour la capture vidéo](effects-for-video-capture.md) | Cet article vous montre comment utiliser l’effet de stabilisation vidéo.  |
| [Analyse de scène pour MediaCapture](scene-analysis-for-media-capture.md) | Cet article décrit comment utiliser les classes [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.SceneAnalysisEffect) et [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.FaceDetectionEffect) pour analyser le contenu du flux d’aperçu de capture multimédia.  |
| [Capturer une séquence de photos avec VariablePhotoSequence](variable-photo-sequence.md) | Cet article indique comment capturer une séquence de photos variables pour capturer plusieurs trames d’images en une succession rapide et configurer des paramètres de mise au point, de flash, de sensibilité ISO, d’exposition et de compensation de l’exposition différents pour chaque trame.  |
| [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md) | Cet article vous explique comment utiliser une instance [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) avec [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) pour récupérer des images à partir d’une ou de plusieurs sources disponibles, notamment d’appareils photos couleur, de profondeur et infrarouges, de périphériques audio ou même de sources d’images personnalisés telles que celles qui produisent des images de suivi des squelettes. Cette fonctionnalité est conçue pour être utilisée par les applications qui effectuent le traitement en temps réel des images multimédias, telles que les applications de caméra prenant en charge la profondeur et de réalité augmentée.  |
| [Obtenir une image d’aperçu](get-a-preview-frame.md) | Cet article vous montre comment obtenir une image d’aperçu à partir du flux d’aperçu de capture multimédia.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>Exemple d’application UWP pour l’appareil photo

* [Exemple de détection des visages de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=619486&clcid=0x409)
* [Exemple d’image d’aperçu de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=620516&clcid=0x409)
* [Exemple HDR de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=620517&clcid=0x409)
* [Exemple des contrôles manuels de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=627611&clcid=0x409)
* [Exemple de profil de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=620518&clcid=0x409)
* [Exemple de résolution de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=624252&clcid=0x409)
* [Kit de démarrage de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=619479&clcid=0x409)
* [Exemple de stabilisation vidéo de l’appareil photo](http://go.microsoft.com/fwlink/p/?LinkID=620519&clcid=0x409)

## <a name="related-topics"></a>Rubriques connexes

* [Audio, vidéo et appareil photo](index.md)
 

 




