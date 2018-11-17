---
author: drewbatgit
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: Cet article explique comment les applications UWP peuvent détecter les changements de niveaux de flux audio initiés par le système et y répondre
title: Détecter les changements d’état audio et y répondre
ms.author: drewbat
ms.date: 04/03/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f7b4addf2a7bdc2d93cbcf64f13a640a4ef5b12a
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7104241"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Détecter les changements d’état audio et y répondre
À partir de Windows10, version1803, votre application peut détecter quand le système baisse ou désactive le niveau audio d’un flux audio utilisé par votre application. Vous pouvez recevoir des notifications pour les flux de capture et de rendu, pour un périphérique audio et une catégorie audio spécifiques, ou pour un objet [**MediaPlayer**](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) que votre application utilise pour la lecture multimédia. Par exemple, le système peut baisser, ou «atténuer», le niveau de lecture audio lorsqu’une alarme sonne. Le système désactive votre application lorsqu’elle passe à l’arrière-plan si la fonctionnalité *backgroundMediaPlayback* n’a pas été déclarée dans le manifeste de l’application. 

Le modèle de gestion des changements d’état audio est identique pour tous les flux audio pris en charge. Tout d’abord, créez une instance de la classe [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor). Dans l’exemple suivant, l’application utilise la classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) pour capturer le contenu audio des conversations de jeu. Une méthode de fabrique est appelée pour obtenir une analyse de l’état audio associée au flux de capture audio des conversations de jeu de l’appareil de communication par défaut.  Ensuite, un gestionnaire est enregistré pour l’événement [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) qui est déclenché lorsque le niveau audio du flux associé est modifié par le système.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

Dans le Gestionnaire d’événements **SoundLevelChanged** , vérifiez la propriété [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) de l’expéditeur **AudioStateMonitor** transmis au gestionnaire pour déterminer quel est le nouveau niveau audio du flux. Dans cet exemple, l’application arrête la capture audio lorsque le niveau sonore est désactivé et reprend la capture lorsque le niveau sonore est restauré sur le volume complet.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

Pour plus d’informations sur la capture audio avec **MediaCapture**, voir [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Chaque instance de la classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) a un objet **AudioStateMonitor** associé que vous pouvez utiliser pour détecter quand le système change le niveau de volume du contenu en cours de lecture. Vous pouvez décider de gérer les changements d’état audio différemment selon le type de contenu en cours de lecture. Par exemple, vous pouvez décider de suspendre la lecture d’un podcast lorsque le volume est baissé, mais de continuer la lecture si le contenu est de la musique. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

Pour plus d’informations sur l’utilisation de **MediaPlayer**, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Rubriquesassociées