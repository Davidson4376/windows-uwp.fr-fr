---
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: Cet article vous explique comment interagir avec les contrôles de transport de média système.
title: Intégration avec les contrôles de transport de média système
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d2c8e05d2b01b110085ed82c19cecd251c9c6971
ms.sourcegitcommit: c95915f8a13736705eab74951a12b2cf528ea612
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876244"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>Intégration avec les contrôles de transport de média système

Cet article vous explique comment interagir avec les contrôles de transport de média système. Il s’agit d’un ensemble de contrôles communs à l’ensemble des appareils Windows 10 et qui octroient aux utilisateurs un moyen simple de contrôler la lecture des éléments multimédias pour les applications exécutées qui recourent à [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) pour la lecture.

Pour obtenir un exemple complet illustrant l’intégration avec les contrôles de transport de média système, consultez l’[exemple de contrôle de transport de média système sur github](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls).
                    
## <a name="automatic-integration-with-smtc"></a>Intégration automatique avec les contrôles de transport de média système
À partir de Windows 10, version 1607, les applications UWP qui utilisent la classe [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) pour lire le contenu multimédia sont automatiquement intégrées par défaut avec les contrôles de transport de média système. Instanciez simplement une nouvelle instance de**MediaPlayer** et affectez un objet [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) ou [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList) sur la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) du lecteur. Dès lors, l’utilisateur verra le nom de votre application dans les contrôles de transport de média système et sera en mesure de lire et de suspendre le contenu lu, et de se déplacer dans la liste de lecture à l’aide des contrôles de transport de média système. 

Votre application peut créer et utiliser plusieurs objets **MediaPlayer** simultanément. Pour chaque instance **MediaPlayer** active dans votre application, un onglet séparé est créé dans les contrôles de transport de média système. Ici, l’utilisateur peut basculer entre vos différents lecteurs multimédias actifs et ceux d’autres applications en cours d’exécution. Les contrôles de transport de média système affectent le lecteur multimédia actuellement sélectionné.

Pour plus d’informations sur l’utilisation de **MediaPlayer** dans votre application, notamment sur sa liaison à un objet [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) dans votre page XAML, consultez la section [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

Pour plus d’informations sur l’utilisation de **MediaSource**, **MediaPlaybackItem** et **MediaPlaybackList**, consultez la section [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>Ajouter des métadonnées à afficher par les contrôles de transport de média système
Si vous souhaitez ajouter ou modifier des métadonnées affichées pour vos éléments multimédias dans les contrôles de transport de média système, comme une vidéo ou un morceau de musique, vous devez mettre à jour les propriétés d’affichage de l’objet **MediaPlaybackItem** représentant votre élément multimédia. Tout d’abord, récupérez une référence à l’objet [**MediaItemDisplayProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaItemDisplayProperties) en appelant [**GetDisplayProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties). Ensuite, définissez le type de média, musique ou vidéo, pour l’élément avec la propriété [**Type**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.type). Vous pouvez ensuite remplir les champs [**MusicProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) ou [**VideoProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties), en fonction du type de média spécifié. Enfin, mettez à jour les métadonnées associées à l’élément multimédia en appelant [**ApplyDisplayProperties**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties).

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]


> [!Note]
> Les applications doivent définir une valeur pour la propriété [**type**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaitemdisplayproperties.type) , même si elles ne fournissent pas d’autres métadonnées de média devant être affichées par les contrôles de transport de média système. Cette valeur aide le système à gérer correctement votre contenu multimédia, y compris empêcher l’activation de l’économiseur d’écran pendant la lecture.


## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>Utilisez CommandManager pour modifier ou remplacer les commandes par défaut des contrôles de transport de média système.
Votre application peut modifier ou remplacer complètement le comportement des contrôles de transport de média système avec la classe [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager). Une instance du gestionnaire de commandes peut être récupérée pour chaque instance de la classe **MediaPlayer**, en accédant à la propriété [**CommandManager**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.commandmanager).

Pour chaque commande, comme la commande *Next*, qui par défaut passe à l’élément suivant d’une classe **MediaPlaybackList**, le gestionnaire de commandes expose un événement reçu, comme [**NextReceived**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextreceived), et un objet qui gère le comportement de la commande, comme [**NextBehavior**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.nextbehavior). 

L’exemple suivant enregistre un gestionnaire pour l’événement **NextReceived** et pour l’événement [**IsEnabledChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) de **NextBehavior**.

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

L’exemple suivant illustre un scénario au cours duquel une application tente de désactiver la commande *Next* après que l’utilisateur a cliqué sur cinq éléments de la playlist, en nécessitant peut-être un certain niveau d’interaction de l’utilisateur avant de poursuivre la lecture du contenu. Chaque fois qu’un événement **NextReceived** est déclenché, un compteur est incrémenté. Une fois que le compteur atteint le nombre cible, l’objet [**EnablingRule**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.enablingrule) de la commande *Next* est défini sur [**Never**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaCommandEnablingRule), auquel cas la commande est désactivée. 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

Vous pouvez également définir la commande sur **Always**, ce qui signifieu qu’elle sera toujours activée même si, comme c’est le cas avec l’exemple de la commande *Next*, la playlist ne comporte plus d’éléments. Sinon, vous pouvez définir la commande sur **Auto**, auquel cas le système détermine si la commande doit être activée en fonction du contenu actuellement lu.

Pour le scénario décrit ci-dessus, à un moment donné l’application tentera de réactiver la commande *Next* en définissant **EnablingRule** sur **Auto**.

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

Étant donné que votre application peut disposer de sa propre interface utilisateur dédiée au contrôle de la lecture au premier plan, vous pouvez utiliser les événements [**IsEnabledChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabledchanged) pour mettre à jour votre propre interface utilisateur en fonction des contrôles de transport de média système à mesure de l’activation et de la désactivation des commandes, en accédant à l’objet [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagercommandbehavior.isenabled) de la propriété [**MediaPlaybackCommandManagerCommandBehavior**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) transmise au gestionnaire.

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

Dans certains cas, vous voudrez remplacer complètement le comportement de la commande des contrôles de transport de média système. L’exemple suivant illustre un scénario dans lequel une application utilise les commandes *Next* et *Previous* pour basculer entre les différentes stations de radio sur Internet, au lieu de transiter entre les pistes de la playlist actuelle. Comme dans l’exemple précédent, un gestionnaire est enregistré dans le cadre d’une réception de commande ; ici, il s’agit de l’événement [**PreviousReceived**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.previousreceived).

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

Dans le gestionnaire **PreviousReceived**, dans un premier temps un objet [**Deferral**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Deferral) est récupéré en appelant l’instance [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.getdeferral) de [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) transmise au gestionnaire. Cela signale au système d’attendre la fin du report pour exécuter la commande. Cette procédure est extrêmement importante si vous envisagez de passer des appels asynchrones dans le gestionnaire. À ce stade, l’exemple appelle une méthode personnalisée qui renvoie une classe **MediaPlaybackItem** représentant la station de radio précédente.

Ensuite, la propriété [**Handled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanagerpreviousreceivedeventargs.handled) est examinée afin de garantir que l’événement n’a pas été précédemment traité par un autre gestionnaire. Si ce n’est pas le cas, la propriété **Handled** est définie sur true. Ainsi, les contrôles de transport de média système et tous les autres gestionnaires inscrits savent que cette commande déjà traitée ne doit pas être exécutée. Ensuite, le code définit la nouvelle source pour le lecteur multimédia, qu’il démarre.

Enfin, la commande [**Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete) est appelée sur l’objet de report, afin d’indiquer au système que le traitement de la commande est terminé.

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                
## <a name="manual-control-of-the-smtc"></a>Contrôle manuel des contrôles de transport de média système
Comme mentionné précédemment dans cet article, les contrôles de transport de média système détectent et affichent automatiquement les informations pour chaque instance de **MediaPlayer** créée par votre application. SI vous souhaitez utiliser plusieurs instances de **MediaPlayer** mais voulez que les contrôles de transport de média système fournissent une seule entrée pour votre application, vous devez définir manuellement le comportement des contrôles de transport de média système, au lieu de vous appuyer sur l’intégration automatique. Par ailleurs, si vous utilisez [**MediaTimelineController**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaTimelineController) pour contrôler un ou plusieurs lecteurs multimédias, vous devez recourir à l’intégration manuelle des contrôles de transport de média système. En outre, si votre application utilise une API différente de **MediaPlayer**, comme la classe [**AudioGraph**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph) pour lire du contenu multimédia, vous devez implémenter l’intégration manuelle des contrôles de transport de média système pour que l’utilisateur les utilise pour contrôler votre application. Pour plus d’informations sur le contrôle manuel des contrôles de transport de média système, consultez la section [Contrôle manuel des contrôles de transport de média système](system-media-transport-controls.md).



## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Lire des fichiers audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Contrôle manuel des contrôles de transport des médias système](system-media-transport-controls.md)
* [Exemple de contrôles System Media tranport sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 




