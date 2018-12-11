---
ms.assetid: ''
description: Cet article vous montre comment capturer une vidéo à partir de plusieurs sources simultanées dans un fichier unique avec plusieurs pistes vidéos incorporées.
title: Capture à partir de plusieurs sources à l’aide de MediaFrameSourceGroup
ms.date: 09/12/2017
ms.topic: article
keywords: Windows10, uwp, capture vidéo
ms.localizationpriority: medium
ms.openlocfilehash: a654739490043b9f821e7906fa8cf9e3e7259fed
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883068"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Capture à partir de plusieurs sources à l’aide de MediaFrameSourceGroup

Cet article vous montre comment capturer une vidéo à partir de plusieurs sources simultanées dans un fichier unique avec plusieurs pistes vidéos incorporées. À partir de RS3, vous pouvez spécifier plusieurs objets **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** pour un seul **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**. Cela vous permet d’encoder plusieurs flux simultanément dans un seul fichier. Les flux vidéo encodés dans cette opération doivent être inclus dans un seul **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** qui spécifie un ensemble des caméras de l’appareil actuel qui peuvent être utilisées en même temps. 

Pour plus d’informations sur l’utilisation de **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** avec la classe **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** pour mettre en œuvre des scénarios de vision par ordinateur en temps réel utilisant plusieurs caméras, voir [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md).

La suite de cet article vous explique la procédure d'enregistrement de vidéos à partir de deux caméras couleur dans un fichier unique avec plusieurs pistes vidéos.

## <a name="find-available-sensor-groups"></a>Rechercher les groupes de capteur disponibles
Un **MediaFrameSourceGroup** représente une collection de sources d’images, généralement des caméras, accessibles simultanément. L’ensemble des groupes de sources d’images disponibles est différent pour chaque appareil. La première étape de cet exemple consiste donc à obtenir la liste des groupes de sources d’images disponibles et de rechercher celui qui contient les caméras nécessaires pour le scénario, lequel nécessite dans ce cas deux caméras couleur.

La méthode **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** retourne tous les groupes de sources disponibles sur l'appareil actuel. Chaque **MediaFrameSourceGroup** retourné dispose d’une liste d’objets **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** qui décrit chaque source d’images du groupe. Une requête Linq est utilisée pour trouver un groupe de sources contenant deux caméras couleur, une sur le panneau avant et une à l’arrière. Un objet anonyme est retourné contenant le **MediaFrameSourceGroup** sélectionné et la liste **MediaFrameSourceInfo** pour chaque caméra couleur. Au lieu d’utiliser la syntaxe Linq, vous pouvez également parcourir chaque groupe, puis chaque **MediaFrameSourceInfo** pour rechercher un groupe correspondant à vos besoins.

Notez que tous les appareils ne disposent pas d'un groupe de sources contenant deux caméras couleur. Vous devez donc vous assurer qu’un groupe de sources a été trouvé avant d’essayer de capturer des vidéos.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>Initialiser l’objet MediaCapture
La classe **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** est la principale classe utilisée pour la plupart des opérations de capture audio, vidéo et photo dans les applications UWP. Initialisez l’objet en appelant **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.InitializeAsync)**, en transmettant un objet **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** qui contient les paramètres d’initialisation. Dans cet exemple, le seul paramètre spécifié est la propriété **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)**, définie sur le **MediaFrameSourceGroup** qui a été récupéré dans l’exemple de code précédent.

Pour plus d’informations sur les autres opérations possibles avec **MediaCapture** et d'autres fonctionnalités des applications UWP permettant de capturer du contenu multimédia, voir [Caméra](camera.md).

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>Créer un MediaEncodingProfile
La classe **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** indique au pipeline de capture multimédia comment encoder le contenu audio et vidéo capturé pour l'écriture dans un fichier. Pour les scénarios de capture et de transcodage standard, cette classe fournit un ensemble de méthodes statiques pour la création de profils communs, tels que **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** et **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**. Pour cet exemple, un profil d’encodage est créé manuellement en utilisant un conteneur Mpeg4 et l'encodage vidéo H264. Les paramètres d’encodage vidéo sont spécifiés à l'aide d'un objet **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)**. Pour chaque caméra couleur utilisée dans ce scénario, un objet **VideoStreamDescriptor** est configuré. Le descripteur est construit avec l'objet **VideoEncodingProperties** spécifiant l’encodage. La propriété **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor.Label)** du **VideoStreamDescriptor** doit être définie sur l’ID de la source d’images multimédias qui sera capturée dans le flux. Voici comment le pipeline de capture connaît le descripteur de flux et les propriétés d’encodage à utiliser pour chaque caméra. L’ID de la source d’images est exposé par les objets **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** qui ont été trouvés dans la section précédente, lorsqu’un **MediaFrameSourceGroup** a été sélectionné.


À partir de Windows10, version1709, vous pouvez définir plusieurs propriétés d’encodage sur un objet **MediaEncodingProfile** en appelant **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)**. Vous pouvez récupérer la liste des descripteurs de flux vidéo en appelant **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)**. Notez que si vous définissez la propriété **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)**, qui stocke un seul descripteur de flux, la liste de descripteurs que vous définissez en appelant **SetVideoTracks** sera remplacée par une liste qui contient le descripteur unique que vous avez spécifié.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

### <a name="encode-timed-metadata-in-media-files"></a>Encoder des métadonnées synchronisées dans des fichiers multimédias

À compter de Windows10, version1803, en plus des données audio et vidéo, vous pouvez encoder des métadonnées synchronisées dans un fichier multimédia pour lequel le format des données est pris en charge. Par exemple, les métadonnées GoPro (gpmd) peuvent être stockées dans des fichiers MP4 pour transmettre l’emplacement géographique en corrélation avec un flux vidéo. 

L’encodage des métadonnées utilise un modèle qui est parallèle à l’encodage audio ou vidéo. La classe [**TimedMetadataEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) décrit le type, le sous-type et les propriétés d’encodage des métadonnées, tout comme la classe **VideoEncodingProperties** pour la vidéo. L’objet [**TimedMetadataStreamDescriptor**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) identifie un flux de métadonnées, tout comme l’objet **VideoStreamDescriptor** pour les flux vidéo.  

L’exemple suivant montre comment initialiser un objet **TimedMetadataStreamDescriptor**. Tout d’abord, un objet **TimedMetadataEncodingProperties** est créé et le **Subtype** est défini sur un GUID qui identifie le type de métadonnées qui seront inclus dans le flux. Cet exemple utilise le GUID des métadonnées GoPro (gpmd). La méthode [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) est appelée pour définir des données spécifiques au format. Pour les fichiers MP4, les données spécifiques au format sont stockées dans la zone SampleDescription (stsd). Ensuite, un nouvel objet **TimedMetadataStreamDescriptor** est créé à partir des propriétés d’encodage. Les propriétés **Label** et **Name** sont définies pour identifier le flux à encoder. 

[!code-cs[GetStreamDescriptor](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetStreamDescriptor)]

Appelez [MediaEncodingProfile.SetTimedMetadataTracks](**https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks**) pour ajouter le descripteur de flux de métadonnées au profil d’encodage. L’exemple suivant illustre une méthode d’assistance qui accepte deux descripteurs de flux vidéo, un descripteur de flux audio et un descripteur de flux de métadonnées synchronisées, et renvoie un objet **MediaEncodingProfile** qui peut être utilisé pour encoder les flux.

[!code-cs[GetMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Enregistrement à l’aide du MediaEncodingProfile multi-flux
L’étape finale de cet exemple consiste à lancer la capture vidéo en appelant **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)**, en transmettant le **StorageFile** dans lequel écrire le contenu multimédia capturé et le **MediaEncodingProfile** créé dans l’exemple de code précédent. Après quelques secondes, l’enregistrement est arrêté avec un appel à **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)**.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

Une fois l’opération terminée, un fichier vidéo a été créé contenant la vidéo capturée à partir de chaque caméra encodée en tant que flux distinct dans le fichier. Pour plus d’informations sur la lecture de fichiers multimédias contenant plusieurs pistes vidéo, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Rubriques associées

* [Caméra](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Traiter des images multimédias avec MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md)


 

 




