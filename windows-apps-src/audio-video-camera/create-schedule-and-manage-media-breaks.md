---
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: Cet article vous explique comment créer, planifier et gérer des coupures de médias dans votre application de lecture de contenu multimédia.
title: Créer, planifier et gérer des coupures de médias
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 68260cadd4322d51c528bf6c6af7f5c8472f2199
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361861"
---
# <a name="create-schedule-and-manage-media-breaks"></a>Créer, planifier et gérer des coupures de médias

Cet article vous explique comment créer, planifier et gérer des coupures de médias dans votre application de lecture de contenu multimédia. Les coupures de médias sont généralement utilisées pour insérer des publicités audio ou vidéo dans du contenu multimédia. À partir de Windows 10, version 1607, vous pouvez utiliser la classe [**MediaBreakManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakManager) pour ajouter rapidement et facilement des coupures de médias dans un objet [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) lu dans [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer).


Une fois que vous avez planifié une ou plusieurs coupures de médias, le système lit automatiquement votre contenu multimédia à l’intervalle spécifié durant la lecture. L’objet **MediaBreakManager** génère des événements de manière à ce que votre application puisse réagir au démarrage et à l’arrêt des coupures de médias, ou lorsqu’elles sont ignorées par l’utilisateur. Vous pouvez également accéder à un objet [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) associé à vos coupures de médias afin de surveiller les événements tels que les mises à jour de l’avancement des téléchargements et de la mise en mémoire tampon.

## <a name="schedule-media-breaks"></a>Planifier des coupures de médias
Chaque objet **MediaPlaybackItem** présente son propre [**MediaBreakSchedule**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakSchedule), que vous utilisez pour configurer les interruptions de média activées lors de la lecture de l’élément. La première phase de l’utilisation des interruptions de média dans votre application consiste à créer un [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) pour votre contenu de lecture principal. 

[!code-cs[MoviePlaybackItem](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMoviePlaybackItem)]

Pour plus d’informations sur l’utilisation de **MediaPlaybackItem**, [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList) et d’autres API de lecture de contenu multimédia, consultez la section [Lecture de contenu multimédia avec MediaSource](media-playback-with-mediasource.md).

L’exemple suivant illustre l’ajout d’une coupure de preroll à **MediaPlaybackItem**, ce qui signifie que le système lira la coupure de médias avant de lire l’élément de lecture auquel elle appartient. Dans un premier temps, un nouvel objet [**MediaBreak**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreak) est instancié. Dans cet exemple, le constructeur est appelé avec [**MediaBreakInsertionMethod.Interrupt**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod), ce qui signifie que le contenu principal sera interrompu pendant la lecture de la coupure de médias. 

Ensuite, un nouvel élément **MediaPlaybackItem** est créé pour le contenu à lire pendant la coupure, tel qu’une publicité. La propriété [**CanSkip**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.canskip) de cet élément de lecture est définie sur false. Ici, l’utilisateur ne pourra pas ignorer l’élément à l’aide des contrôles multimédias intégrés. Votre application peut toujours ignorer automatiquement la coupure, en appelant [**SkipCurrentBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak). 

La propriété [**PlaybackList**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreak.playbacklist) de la coupure de média est un élément [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList), qui vous permet de lire plusieurs éléments multimédias en tant que playlist. Ajoutez un ou plusieurs objets **MediaPlaybackItem** de la collection **Items** de la liste afin de les inclure dans la playlist de la coupure de média.

Enfin, planifiez la coupure de média à l’aide de la propriété  [**BreakSchedule**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.breakschedule) de l’élément de lecture du contenu principal. Définissez la coupure en tant qu’objet de preroll en l’affectant à la propriété [**PrerollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) de l’objet de planification.

[!code-cs[PreRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPreRollBreak)]

Maintenant que vous pouvez lire l’élément multimédia principal, et la coupure de média créée sera lue avant le contenu principal. Créez un nouvel objet [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) et définissez éventuellement la propriété [**AutoPlay**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.autoplay) sur true afin de démarrer automatiquement la lecture. Définissez la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) de **MediaPlayer** sur l’élément de lecture de votre contenu principal. Cela n’est pas obligatoire, mais vous pouvez affecter l’objet **MediaPlayer** à un [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) afin d’afficher le contenu multimédia sur une page XAML. Pour plus d’informations sur l’utilisation de **MediaPlayer**, consultez la section [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[Play](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

Ajoutez une coupure de postroll à lire après la fin de la lecture de l’objet **MediaPlaybackItem** contenant votre contenu principal, en appliquant une technique identique à celle employée avec une coupure de preroll. Ici seulement, vous affectez votre objet **MediaBreak** à la propriété [**PostrollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.postrollbreak).

[!code-cs[PostRollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetPostRollBreak)]

Vous pouvez également planifier une ou plusieurs coupures de midroll à lire à des intervalles spécifiques durant la lecture du contenu principal. Dans l’exemple suivant, l’objet [**MediaBreak**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreak) est créé avec la surcharge du constructeur qui accepte un objet **TimeSpan**, qui spécifie l’intervalle d’activation de la coupure durant la lecture de l’élément multimédia principal. Là encore, [**MediaBreakInsertionMethod.Interrupt**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) est définie pour indiquer que la lecture du contenu principal est interrompue pendant l’activation de la coupure. La coupure de midroll est ajoutée à la planification, via l’appel à [**InsertMidrollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.insertmidrollbreak). Vous pouvez récupérer une liste en lecture seule des coupures de midroll actives dans la planification, en accédant à la propriété [**MidrollBreaks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.midrollbreaks).

[!code-cs[MidrollBreak](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak)]

Le prochain exemple représenté de coupure de midroll utilise la méthode d’insertion [**MediaBreakInsertionMethod.Replace**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod), ce qui signifie que le système continuera à traiter le contenu principal pendant la lecture de la coupure. Cette option est généralement utilisée par les applications de diffusion de contenu multimédia en continu au sein desquelles le contenu n’est pas interrompu et déplacé derrière le flux en direct pendant la lecture de la publicité. 

Cet exemple utilise également une surcharge du constructeur [**MediaPlaybackItem**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) qui accepte deux paramètres [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan). Le premier paramètre spécifie le point de départ de la lecture au sein de l’élément de coupure de média. Le second paramètre spécifie la durée pendant laquelle l’élément de coupure de média sera lu. Ainsi, dans l’exemple suivant, l’objet **MediaBreak** démarrera 20 minutes après le démarrage du contenu principal. La lecture de l’élément multimédia démarre 30 secondes après le début de l’élément de coupure de média et s’arrête 15 secondes avant la reprise de la lecture du contenu multimédia principal.

[!code-cs[MidrollBreak2](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetMidrollBreak2)]

## <a name="skip-media-breaks"></a>Ignorer les coupures de médias
Comme indiqué précédemment dans cet article, la propriété [**CanSkip**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.canskip) d’un élément **MediaPlaybackItem** peut être définie pour empêcher l’utilisateur d’ignorer le contenu avec les contrôles intégrés. Toutefois, vous pouvez appeler à tout moment [**SkipCurrentBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) dans votre code afin d’ignorer la coupure active.

[!code-cs[SkipButtonClick](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetSkipButtonClick)]

## <a name="handle-mediabreak-events"></a>Gérer les événements MediaBreak

Plusieurs des événements associés aux coupures de médias peuvent être enregistrés, de manière à ce que vous puissiez agir en fonction de l’évolution de l’état des coupures de média.

[!code-cs[RegisterMediaBreakEvents](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterMediaBreakEvents)]

L’événement [**BreakStarted**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.breakstarted) est déclenché lors du démarrage d’une coupure de média. Vous voudrez peut-être mettre à jour votre interface utilisateur afin d’indiquer à l’utilisateur que la coupure de média est en cours de lecture. Cet exemple utilise l’objet [**MediaBreakStartedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakStartedEventArgs) transmis au gestionnaire pour récupérer une référence à la coupure de média démarrée. Ensuite, la propriété [**CurrentItemIndex**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.currentitemindex) est utilisée pour identifier l’élément multimédia en cours de lecture de la playlist de la coupure de média. Ensuite, l’interface utilisateur est mise à jour afin d’indiquer à l’utilisateur l’index des publicités et le nombre de publicités restant dans la coupure. N’oubliez pas que les mises à jour de l’interface utilisateur doivent être effectuées sur le thread d’interface utilisateur. Dès lors, l’appel doit être effectué dans un appel à [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows). 

[!code-cs[BreakStarted](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakStarted)]

[**BreakEnded** ](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.breakended) est déclenché lorsque tous les éléments multimédias dans le saut sont terminées ou ont été ignorés. Vous pouvez utiliser le gestionnaire de cet événement pour mettre à jour l’interface utilisateur afin d’indiquer que le contenu de la coupure de média n’est plus en cours de lecture.

[!code-cs[BreakEnded](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakEnded)]

L’événement **BreakSkipped** est déclenché lorsque l’utilisateur appuie sur le bouton *Suivant* dans l’interface utilisateur durant la lecture d’un élément pour lequel l’objet [**CanSkip**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.canskip) est défini sur true,ou lorsque vous ignorez une coupure dans votre code en appelant [**SkipCurrentBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak).

L’exemple suivant utilise la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) de l’objet **MediaPlayer** afin d’obtenir une référence à l’élément multimédia pour le contenu principal. La coupure de média ignorée appartient à la planification des coupures de cet élément. Ensuite, le code vérifie si la coupure de média ignorée correspond à celle définie dans la propriété [**PrerollBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) de la planification. Si tel est le cas, cela signifie que la coupure de preroll est bien celle qui a été ignorée. Le cas échéant, une nouvelle coupure de midroll est créée, et sa lecture est planifiée 10 minutes après le démarrage du contenu principal.

[!code-cs[BreakSkipped](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSkipped)]

[**BreaksSeekedOver** ](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.breaksseekedover) est déclenché lorsque la position de lecture de l’élément multimédia principal passe au-dessus de l’heure planifiée pour un ou plusieurs sauts de média. L’exemple suivant détecte si une ou plusieurs coupures de média ont été recherchées, si la position de lecture a été avancée et si elle a été avancée de moins de 10 minutes. Le cas échéant, la première coupure recherchée, récupérée de la collection [**SeekedOverBreaks**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakseekedovereventargs.seekedoverbreaks) exposée par les arguments d’événement, est lue immédiatement avec un appel à la méthode [**PlayBreak**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediabreakmanager.playbreak) de **MediaPlayer.BreakManager**.

[!code-cs[BreakSeekedOver](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBreakSeekedOver)]


## <a name="access-the-current-playback-session"></a>Accéder à la session de lecture active
L’objet [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) utilise la classe **MediaPlayer** pour fournir des données et des événements associés au contenu multimédia en cours de lecture. L’objet [**MediaBreakManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaBreakManager) présente également un élément **MediaPlaybackSession** auquel vous accédez pour récupérer les données et les événements spécifiquement associés au contenu de la coupure de média en cours de lecture. Parmi les informations pouvant être obtenues de la session de lecture, vous trouverez l’état de lecture actif (en lecture ou en pause), ainsi que la position de lecture au sein du contenu. Vous pouvez utiliser les propriétés [**NaturalVideoWidth**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideowidth) et [**NaturalVideoHeight**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight), ainsi que [**NaturalVideoSizeChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideosizechanged) pour ajuster votre interface utilisateur vidéo si le contenu de la coupure de média présente des proportions différentes de celles de votre contenu principal. Vous pouvez également recevoir des événements tels que [**BufferingStarted**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.bufferingstarted), [**BufferingEnded**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.bufferingended) et [**DownloadProgressChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.downloadprogresschanged), qui peuvent fournir des données précieuses de télémétrie relatives aux performances de votre application.

Dans l’exemple suivant, un gestionnaire est enregistré pour l’**événement BufferingProgressChanged** ; dans le gestionnaire d’événement, il met à jour l’interface utilisateur en fonction de l’avancement de la mise en mémoire tampon.

[!code-cs[RegisterBufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingProgressChanged)]

[!code-cs[BufferingProgressChanged](./code/MediaBreaks_RS1/cs/MainPage.xaml.cs#SnippetBufferingProgressChanged)]

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Lecture audio et vidéo MediaPlayer.](play-audio-and-video-with-mediaplayer.md)
* [Contrôle manuel, le système de Transport de contrôles de média](system-media-transport-controls.md)

 

 




