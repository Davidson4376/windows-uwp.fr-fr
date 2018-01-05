---
author: drewbatgit
ms.assetid: 
description: "Cet article vous montre comment capturer une vidéo à partir de plusieurs sources simultanées dans un fichier unique avec plusieurs pistes vidéos incorporées."
title: "Capture à partir de plusieurs sources à l’aide de MediaFrameSourceGroup"
ms.author: drewbat
ms.date: 09/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, capture vidéo"
ms.localizationpriority: medium
ms.openlocfilehash: 56996cf9edb5aef317421e5ca8eef05153328bf9
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Capture à partir de plusieurs sources à l’aide de MediaFrameSourceGroup

Cet article vous montre comment capturer une vidéo à partir de plusieurs sources simultanées dans un fichier unique avec plusieurs pistes vidéos incorporées. À partir de RS3, vous pouvez spécifier plusieurs objets **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** pour un seul **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**. Cela vous permet d’encoder plusieurs flux simultanément dans un seul fichier. Les flux vidéo encodés dans cette opération doivent être inclus dans un seul **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** qui spécifie un ensemble des caméras de l’appareil actuel qui peuvent être utilisées en même temps. 

Pour plus d’informations sur l’utilisation de **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** avec la classe **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** pour mettre en œuvre des scénarios de vision par ordinateur en temps réel utilisant plusieurs caméras, voir [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md).

La suite de cet article vous explique la procédure d'enregistrement de vidéos à partir de deux caméras couleur dans un fichier unique avec plusieurs pistes vidéos.

## <a name="find-available-sensor-groups"></a>Rechercher les groupes de capteur disponibles
Un **MediaFrameSourceGroup** représente une collection de sources d’images, généralement des caméras, accessibles simultanément. L’ensemble des groupes de sources d’images disponibles est différent pour chaque appareil. La première étape de cet exemple consiste donc à obtenir la liste des groupes de sources d’images disponibles et de rechercher celui qui contient les caméras nécessaires pour le scénario, lequel nécessite dans ce cas deux caméras couleur.

La méthode **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup#Windows_Media_Capture_Frames_MediaFrameSourceGroup_FindAllAsync)** retourne tous les groupes de sources disponibles sur l'appareil actuel. Chaque **MediaFrameSourceGroup** retourné dispose d’une liste d’objets **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** qui décrit chaque source d’images du groupe. Une requête Linq est utilisée pour trouver un groupe de sources contenant deux caméras couleur, une sur le panneau avant et une à l’arrière. Un objet anonyme est retourné contenant le **MediaFrameSourceGroup** sélectionné et la liste **MediaFrameSourceInfo** pour chaque caméra couleur. Au lieu d’utiliser la syntaxe Linq, vous pouvez également parcourir chaque groupe, puis chaque **MediaFrameSourceInfo** pour rechercher un groupe correspondant à vos besoins.

Notez que tous les appareils ne disposent pas d'un groupe de sources contenant deux caméras couleur. Vous devez donc vous assurer qu’un groupe de sources a été trouvé avant d’essayer de capturer des vidéos.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialiser l’objet MediaCapture
La classe **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** est la principale classe utilisée pour la plupart des opérations de capture audio, vidéo et photo dans les applications UWP. Initialisez l’objet en appelant **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_InitializeAsync)**, en transmettant un objet **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** qui contient les paramètres d’initialisation. Dans cet exemple, le seul paramètre spécifié est la propriété **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings#Windows_Media_Capture_MediaCaptureInitializationSettings_SourceGroup)**, définie sur le **MediaFrameSourceGroup** qui a été récupéré dans l’exemple de code précédent.

Pour plus d’informations sur les autres opérations possibles avec **MediaCapture** et d'autres fonctionnalités des applications UWP permettant de capturer du contenu multimédia, voir [Caméra](camera.md).

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>Créer un MediaEncodingProfile
La classe **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** indique au pipeline de capture multimédia comment encoder le contenu audio et vidéo capturé pour l'écriture dans un fichier. Pour les scénarios de capture et de transcodage standard, cette classe fournit un ensemble de méthodes statiques pour la création de profils communs, tels que **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAvi_Windows_Media_MediaProperties_VideoEncodingQuality_)** et **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp3_Windows_Media_MediaProperties_AudioEncodingQuality_)**. Pour cet exemple, un profil d’encodage est créé manuellement en utilisant un conteneur Mpeg4 et l'encodage vidéo H264. Les paramètres d’encodage vidéo sont spécifiés à l'aide d'un objet **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)**. Pour chaque caméra couleur utilisée dans ce scénario, un objet **VideoStreamDescriptor** est configuré. Le descripteur est construit avec l'objet **VideoEncodingProperties** spécifiant l’encodage. La propriété **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor#Windows_Media_Core_VideoStreamDescriptor_Label)** du **VideoStreamDescriptor** doit être définie sur l’ID de la source d’images multimédias qui sera capturée dans le flux. Voici comment le pipeline de capture connaît le descripteur de flux et les propriétés d’encodage à utiliser pour chaque caméra. L’ID de la source d’images est exposé par les objets **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** qui ont été trouvés dans la section précédente, lorsqu’un **MediaFrameSourceGroup** a été sélectionné.


À partir de RS3, vous pouvez définir plusieurs propriétés d’encodage sur un **MediaEncodingProfile** en appelant **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_SetVideoTracks_Windows_Foundation_Collections_IIterable_Windows_Media_Core_VideoStreamDescriptor__)**. Vous pouvez récupérer la liste des descripteurs de flux vidéo en appelant **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_GetVideoTracks)**. Notez que si vous définissez la propriété **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_Video)**, qui stocke un seul descripteur de flux, la liste de descripteurs que vous définissez en appelant **SetVideoTracks** sera remplacée par une liste qui contient le descripteur unique que vous avez spécifié.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Enregistrement à l’aide du MediaEncodingProfile multi-flux
L’étape finale de cet exemple consiste à lancer la capture vidéo en appelant **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_)**, en transmettant le **StorageFile** dans lequel écrire le contenu multimédia capturé et le **MediaEncodingProfile** créé dans l’exemple de code précédent. Après quelques secondes, l’enregistrement est arrêté avec un appel à **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StopRecordAsync)**.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

Une fois l’opération terminée, un fichier vidéo a été créé contenant la vidéo capturée à partir de chaque caméra encodée en tant que flux distinct dans le fichier. Pour plus d’informations sur la lecture de fichiers multimédias contenant plusieurs pistes vidéo, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Rubriques associées

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md)


 

 




