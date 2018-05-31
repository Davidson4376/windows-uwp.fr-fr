---
author: drewbatgit
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: Cet article vous explique comment lire du contenu multimédia dans votre application Windows universelle avec MediaPlayer.
title: Lire du contenu audio et vidéo avec MediaPlayer
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 24b54e202835bb3dba9098591ae08527e12565bf
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832583"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Lire du contenu audio et vidéo avec MediaPlayer

Cet article vous explique comment lire du contenu multimédia dans votre application Windows universelle avec la classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Avec la version 1607 de Windows10, des améliorations considérables ont été apportées aux API de lecture de contenus multimédias. Profitez notamment d’une conception simplifiée à un seul processus pour l’audio d’arrière-plan, de l’intégration automatique avec les contrôles de transport de média système, de la capacité de synchronisation de plusieurs lecteurs multimédias, de la capacité de connexion à une surface Windows.UI.Composition et d’une interface simple pour la création et la planification de coupures de médias dans votre contenu. Pour tirer parti de ces améliorations, la meilleure pratique recommandée pour la lecture de contenus multimédias consiste à utiliser la classe **MediaPlayer** en lieu et place de **MediaElement** pour la lecture de contenu multimédia. Le contrôle XAML léger, [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), a été introduit pour vous permettre d’afficher le contenu multimédia dans une page XAML. Nombre des API de statut et de contrôle de la lecture fournies par **MediaElement** sont désormais disponibles via le nouvel objet [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). **MediaElement** continue de fonctionner pour prendre en charge la compatibilité descendante, mais aucune fonction supplémentaire ne sera ajoutée à cette classe.

Cet article vous présente les fonctions **MediaPlayer** qu’une application standard de lecture de contenu multimédia utilise. Notez que **MediaPlayer** utilise la classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) en tant que conteneur pour l’ensemble des éléments multimédias. Cette classe vous permet de charger et de lire le contenu multimédia à partir de multiples sources différentes utilisant une interface unique, notamment les fichiers locaux, les flux de mémoire et les sources réseau. Il existe également des classes de niveau supérieur compatibles avec **MediaSource**, comme [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) et [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), qui fournissent des fonctions plus avancées comme des playlists et la capacité de gestion de sources multimédias avec plusieurs pistes audio, vidéo et de métadonnées. Pour plus d’informations sur **MediaSource** et les API associées, consultez la page [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).


## <a name="play-a-media-file-with-mediaplayer"></a>Lire un fichier multimédia avec MediaPlayer  
La lecture de contenu multimédia de base avec **MediaPlayer** est très simple à implémenter. Tout d’abord, créez une nouvelle instance de la classe **MediaPlayer**. Votre application peut présenter plusieurs instances **MediaPlayer** actives simultanément. Ensuite, définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) du lecteur sur un objet qui implémente l’interface [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource), comme un objet [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), un objet [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) ou un objet [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). Dans cet exemple, un objet **MediaSource** est créé à partir d’un fichier dans le stockage local de l’application, puis un objet **MediaPlaybackItem** est créé à partir de la source, puis affecté à la propriété **Source** du lecteur.

Contrairement à l’objet **MediaElement**, **MediaPlayer** ne démarre pas automatiquement la lecture par défaut. Vous pouvez démarrer la lecture en appelant [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play),en définissant la propriété [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) sur True, ou en attendant que l’utilisateur lance la lecture à l’aide des contrôles multimédias intégrés.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Lorsque vous avez terminé d’utiliser une instance **MediaPlayer** sur l’application, vous devez appeler la méthode [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) (projetée vers **Dispose** en C#) afin de nettoyer les ressources utilisées par le lecteur.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Utiliser MediaPlayerElement afin d’afficher du contenu vidéo en XAML
Vous pouvez lire du contenu multimédia dans une instance **MediaPlayer** sans l’afficher au format XAML, mais de nombreuses applications de lecture de contenus multimédias sont définies pour ce type d’affichage. Pour ce faire, utilisez le contrôle léger [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement). Tout comme **MediaElement**, **MediaPlayerElement** vous permet de spécifier si les contrôles de transport intégrés doivent être affichés.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Vous pouvez définir l’instance **MediaPlayer** à laquelle l’élément est lié en appelant [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

Vous pouvez également définir la source de lecture sur l’instance **MediaPlayerElement**. Le cas échéant, l’élément crée automatiquement une nouvelle instance **MediaPlayer** à laquelle vous pouvez accéder à l’aide de la propriété [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer).

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Si vous désactivez l’élément [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) de l’instance [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) en définissant [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) sur false, le lien entre **MediaPlayer** et [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) fourni par **MediaPlayerElement** est rompu; autrement dit, les contrôles de transport intégrés ne contrôleront plus automatiquement la lecture du lecteur. Vous devrez donc implémenter vos propres contrôles pour pouvoir contrôler le **MediaPlayer**.

## <a name="common-mediaplayer-tasks"></a>Tâches courantes de MediaPlayer
Cette section vous explique comment utiliser certaines des fonctionnalités de l’instance **MediaPlayer**.

### <a name="set-the-audio-category"></a>Définir la catégorie audio
Définissez la propriété [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) d’une instance **MediaPlayer** sur l’une des valeurs de l’énumération [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) afin d’indiquer au système le type de contenu multimédia lu. Les jeux doivent classer leurs flux musicaux en tant qu’éléments **GameMedia**, de manière à ce que la musique du jeu soit désactivée automatiquement si de la musique s’active sur une autre application en arrière-plan. Les applications de musique ou de vidéo doivent classer leurs flux en tant qu’éléments **Media** ou **Movie**, de manière à les rendre prioritaires par rapport aux flux **GameMedia**.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>Sortie vers un point de terminaison audio spécifique
Par défaut, la sortie audio d’une instance **MediaPlayer** est acheminée vers le point de terminaison audio par défaut du système, mais vous pouvez définir un point de terminaison audio spécifique que l’instance **MediaPlayer** doit utiliser pour la sortie. Dans l’exemple ci-dessous, [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) renvoie une chaîne qui identifie spécifiquement la catégorie de rendu audio des appareils. Ensuite, l’élément [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) de la méthode [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) est appelé afin de récupérer une liste de l’ensemble des appareils disponibles du type sélectionné. Vous pouvez déterminer par programme l’appareil à utiliser ou ajouter les appareils renvoyés sur une instance [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox), afin de laisser à l’utilisateur le choix de l’appareil.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

Dans l’événement [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) de la zone déroulante des appareils, la propriété [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) de l’instance **MediaPlayer** est définie sur l’appareil sélectionné, qui était stocké dans la propriété [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) de l’instance **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>Session de lecture
Comme décrit précédemment dans cet article, nombre des fonctions exposées par la classe **MediaElement** ont été déplacées vers la classe [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). Cela concerne les informations sur l’état de lecture du lecteur, comme la position actuelle de lecture, l’action du lecteur (pause ou lecture) et la vitesse de lecture actuelle. **MediaPlaybackSession** fournit également plusieurs événements vous procurant des informations sur les modifications de l’état, notamment sur le statut actuel de mise en mémoire tampon et de téléchargement du contenu lu et sur la taille naturelle et les proportions du contenu vidéo actuellement lu.

L’exemple suivant vous illustre l’implémentation d’un gestionnaire de clic du bouton qui permet d’avancer de 10secondes dans le contenu. Tout d’abord, l’objet **MediaPlaybackSession** associé au lecteur est récupéré avec la propriété [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession). Ensuite, la propriété [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) est définie sur la position actuelle de lecture plus 10secondes.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

L’exemple suivant illustre l’utilisation d’un bouton bascule permettant de passer de la vitesse de lecture normale à la vitesse double, en définissant la propriété [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) de la session.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

### <a name="detect-expected-and-unexpected-buffering"></a>Détecter une mise en mémoire tampon attendue et inattendue
L’objet **MediaPlaybackSession** décrit dans la section précédente fournit deux événements, **[BufferingStarted](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** et **[BufferingEnded](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)**, pour détecter quand le fichier multimédia en cours de lecture commence et termine la mise en mémoire tampon. Cela vous permet de mettre à jour votre interface utilisateur pour indiquer à l’utilisateur que la mise en mémoire tampon est en cours. Une mise en mémoire tampon initiale est attendue lorsqu’un fichier multimédia est ouvert pour la première fois ou que l’utilisateur passe à un nouvel élément dans une playlist. Une mise en mémoire tampon inattendue peut se produire lorsque la vitesse du réseau se dégrade ou que le système de gestion de contenu qui fournit le contenu rencontre des problèmes techniques. À partir de RS3, vous pouvez utiliser l’événement **BufferingStarted** pour déterminer si l’événement de mise en mémoire tampon est attendu, ou s’il est inattendu et interrompt la lecture. Vous pouvez utiliser ces informations comme données de télémétrie pour votre application ou votre service de fourniture de contenu multimédia. 

Enregistrez des gestionnaires pour les événements **BufferingStarted** et **BufferingEnded** pour recevoir des notifications sur l’état de la mise en mémoire tampon.

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

Dans le gestionnaire d’événements **BufferingStarted** effectuez un cast des arguments transmis dans l’événement sur un objet **[MediaPlaybackSessionBufferingStartedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** et vérifiez la propriété **[IsPlaybackInterruption](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)**. Si cette valeur est true, la mise en mémoire tampon qui a déclenché l’événement est inattendue et interrompt la lecture. Dans le cas contraire, il s’agit d’une mise en mémoire tampon initiale attendue. 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>Pincement et zoom sur du contenu vidéo
**MediaPlayer** vous permet de spécifier le rectangle source au sein du contenu vidéo à afficher. Dès lors, vous pouvez effectuer un zoom dans la vidéo. Le rectangle que vous spécifiez est relatif à un rectangle normalisé (0,0,1,1), où 0,0 correspond au coin supérieur gauche de l’image et 1,1 spécifie la largeur et la hauteur complètes de l’image. Ainsi, par exemple, pour définir le rectangle de zoom de manière à afficher le quadrant supérieur droit de la vidéo, vous définiriez le rectangle (0,5; 0; 0,5; 0,5).  Il est important que vous vérifiiez vos valeurs afin de vous assurer que votre rectangle source se trouve dans le rectangle normalisé (0,0,1,1). Toute tentative de définition de la valeur en dehors de cette plage provoquera l’envoi d’une exception.

Pour implémenter les fonctionnalités de pincement et de zoom à l’aide d’entrées tactiles multipoint, vous devez dans un premier temps spécifier les entrées prises en charge. Dans cet exemple, les entrées de mise à l’échelle et de translation sont demandées. L’événement [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) est déclenché lorsque l’une des entrées enregistrées se produit. L’événement [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) est utilisé pour redéfinir le zoom sur l’intégralité de l’image. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

Ensuite, déclarez un objet **Rect** qui stockera le rectangle source actuel de zoom.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

Le gestionnaire **ManipulationDelta** ajuste la mise à l’échelle ou la translation du rectangle de zoom. Si la valeur delta de mise à l’échelle est différente de 1, cela signifie que l’utilisateur à effectuer un pincement. Si la valeur est supérieure à 1, le rectangle source doit être réduit pour la prise en charge du zoom dans le contenu. Si la valeur est inférieure à 1, le rectangle source doit être agrandi pour la prise en charge du zoom arrière dans le contenu. Avant de définir les nouvelles valeurs de mise à l’échelle, le rectangle obtenu est examiné afin de garantir qu’il s’intègre parfaitement dans les limites (0,0,1,1).

Si la valeur de mise à l’échelle est 1, l’entrée de translation est traitée. Le rectangle est simplement translaté selon la division du nombre de pixels de l’entrée par la valeur de largeur et de hauteur du contrôle. Là encore, le rectangle obtenu est examiné afin de garantir qu’il s’intègre parfaitement dans les limites (0,0,1,1).

Enfin, l’élément [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) de l’instance **MediaPlaybackSession** est défini sur le nouveau rectangle ajusté; il spécifie la zone de la vidéo à afficher.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

Dans le gestionnaire d’événements [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped), le rectangle source est redéfini sur (0,0,1,1) pour rétablir l’affichage de l’intégralité de la vidéo.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

### <a name="handling-policy-based-playback-degradation"></a>Gestion de la dégradation de la lecture basée sur une stratégie

Dans certains cas, le système peut dégrader la lecture d’un élément multimédia, par exemple en réduisant la résolution (limitation), dans le cadre d’une stratégie plutôt qu’un problème de performances. Par exemple, la vidéo peut être dégradée par le système si elle est lue à l’aide d’un pilote vidéo non signé. Vous pouvez appeler [**MediaPlaybackSession.GetOutputDegradationPolicyState**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) pour déterminer si et pourquoi cette dégradation basée sur une stratégie se produit et en informer l’utilisateur ou enregistrer la raison à des fins de télémétrie.

L’exemple suivant illustre une implémentation d’un gestionnaire pour l’événement **MediaPlayer.MediaOpened** qui est déclenché lorsque le lecteur ouvre un nouvel élément multimédia. **GetOutputDegradationPolicyState** est appelé sur l’objet **MediaPlayer** transmis au gestionnaire. La valeur de [**VideoConstrictionReason**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) indique la raison pour laquelle la vidéo est limitée. Si la valeur n’est pas **None**, cet exemple enregistre la raison de la dégradation à des fins de télémétrie. Cet exemple illustre également la définition de la vitesse de transmission de l’objet **AdaptiveMediaSource** en cours de lecture sur la bande passante la plus faible pour économiser l’utilisation des données, étant donné que la vidéo est limitée et n’est pas affichée à une haute résolution. Pour plus d’informations sur l’utilisation de **AdaptiveMediaSource**, voir [Streaming adaptatif](adaptive-streaming.md).

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Utiliser MediaPlayerSurface afin d’afficher la vidéo sur une surface Windows.UI.Composition
À partir de Windows10, version 1607, vous pouvez utiliser l’instance **MediaPlayer** afin d’afficher le contenu vidéo sur une interface [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface), ce qui permet au lecteur d’intergir avec les API dans l’espace de noms [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition). L’infrastructure de composition peut être mise à profit pour travailler avec des graphiques dans la couche visuelle située entre les API XAML et les API graphiques DirectX de niveau inférieur. Cela permet la prise en charge de scénarios tels que le rendu de contenus vidéo dans tout contrôle XAML. Pour plus d’informations sur l’utilisation des API de composition, consultez la page [Couche visuelle](https://msdn.microsoft.com/windows/uwp/composition/visual-layer).

L’exemple suivant illustre l’affichage d’un contenu de lecteur vidéo sur un contrôle [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas). Les appels spécifiques au lecteur multimédias de cet exemple sont [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) et [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** indique au système la talle de la mémoire tampon à allouer pour l’affichage du contenu. **GetSurface** prend un élément [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) en tant qu’argument et récupère une instance de la classe [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface). Cette classe procure un accès aux éléments **MediaPlayer** et **Compositor** utilisés pour créer la surface et l’expose via la propriété [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface).

Le reste du code de cet exemple crée un élément [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual) sur lequel la vidéo s’affiche et définit la taille sur celle de l’élément de zone de dessin qui affiche les éléments visuels. Ensuite, un élément [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) est créé à partir de la classe [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) et affecté à la propriété [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) des éléments visuels. À ce stade, un élément [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) est créé et l’instance **SpriteVisual** est insérée dans la partie supérieure de son arborescence visuelle. Enfin, [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) est appelé afin d’affecter les éléments visuels du conteneur au contrôle **Canvas**.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Utilisez MediaTimelineController afin de synchroniser du contenu entre plusieurs couches.
Comme indiqué précédemment dans cet article, vous application peut disposer de plusieurs objets **MediaPlayer** actifs simultanément. Par défaut, chaque instance **MediaPlayer** créée fonctionne indépendamment. Pour certains scénarios, tels que la synchronisation d’une piste de commentaires sur une vidéo, vous voudrez synchroniser l’état du lecteur, la position de lecture et la vitesse de lecture de plusieurs couches. À partir de Windows10, version 1607, vous pouvez implémenter ce comportement à l’aide de la classe [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController).

### <a name="implement-playback-controls"></a>Implémenter des contrôles de lecture
L’exemple suivant vous explique l’utilisation d’une classe **MediaTimelineController** pour contrôler deuxinstances de **MediaPlayer**. Tout d’abord, chaque instance de **MediaPlayer** est instanciée et l’objet **Source** est défini sur un fichier multimédia. Ensuite, une nouvelle classe **MediaTimelineController** est créée. Pour chaque instance **MediaPlayer**, la classe [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) associée à chaque lecteur est désactivée par la définition de la propriété [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) sur False. Ensuite, la propriété [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) est définie sur l’objet du contrôleur de chronologie.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**Attention** La classe [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) procure une intégration automatique entre **MediaPlayer** et les contrôles de transport de média système, mais cette intégration automatique ne peut pas être utilisée avec des lecteurs multimédias contrôlés avec une classe **MediaTimelineController**. Par conséquent, vous devez désactiver le gestionnaire de commande du lecteur multimédia avant de définir son contrôleur de chronologie. À défaut, une exception sera transmise avec un message vous informant du blocage de l’association du contrôleur de chronologie de médias en raison de l’état actuel de l’objet. Pour plus d’informations sur l’intégration des lecteurs multimédias avec les contrôles de transport de média système, consultez la page [Intégrer avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md). Si vous utilisez une classe **MediaTimelineController**, vous pouvez toujours contrôler manuellement les contrôles de transport de média système. Pour plus d’informations, consultez la page [Contrôles de transport de média système](system-media-transport-controls.md).

Une fois que vous avez associé une classe **MediaTimelineController** à un ou plusieurs lecteurs multimédias, vous pouvez contrôler l’état de lecture à l’aide des méthodes exposées par le contrôleur. L’exemple suivant appelle [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start) pour démarrer la lecture de l’ensemble des lecteurs multimédias associés au début du contenu multimédia.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

Cet exemple illustre l’interruption et la reprise de la lecture sur l’ensemble des lecteurs multimédias associés.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

Pour définir l’avance rapide sur l’ensemble des lecteurs multimédias connectés, définissez la vitesse de lecture sur une valeur supérieure à 1.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

L’exemple suivant illustre l’utilisation d’un contrôle **Slider** afin d’afficher la position de lecture actuelle du contrôleur de chronologie par rapport à la durée du contenu sur l’un des lecteurs multimédias associés. Tout d’abord, un nouvel élément **MediaSource** est créé et un gestionnaire est enregistré pour l’événement [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted) de la source multimédia. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

Le gestionnaire **OpenOperationCompleted** est utilisé en tant qu’opportunité de découverte de la durée du contenu de la source multimédia. Une fois que la durée est déterminée, la valeur maximale du contrôle **Slider** est définie sur le nombre total de secondes de l’élément multimédia. La valeur est définie au sein d’un appel à [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), afin de garantir l’exécution sur le thread d’interface utilisateur.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

Ensuite, un gestionnaire pour l’événement [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged) du contrôleur de chronologie est enregistré. L’appel est effectué régulièrement par le système, environ 4fois par seconde.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

Dans le gestionnaire de **PositionChanged**, la valeur Slider est mise à jour en fonction de la position actuelle du contrôleur de chronologie.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

### <a name="offset-the-playback-position-from-the-timeline-position"></a>Décaler la position de lecture de la position de la chronologie
Dans certains cas, il est possible que vous vouliez décaler la position de lecture d’un ou de plusieurs lecteurs multimédias des autres lecteurs. Pour ce faire, définissez la propriété [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) de l’objet **MediaPlayer** que vous voulez décaler. L’exemple suivant utilise les durées du contenu de deuxlecteurs multimédias pour définir les valeurs minimum et maximum du contrôle à deuxcurseurs sur «plus» ou «moins» la longueur de l’élément.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

Dans l’événement [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) associé à chaque curseur, la propriété **TimelineControllerPositionOffset** de chaque lecteur est définie sur la valeur correspondante.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Notez que si la valeur de décalage d’un lecteur correspond à une position de lecture négative, le contenu demeure interrompu jusqu’à ce que le décalage atteigne la valeur de 0, puis la lecture redémarre. De la même manière, si la valeur de décalage correspond à une position de lecture supérieure à la durée de l’élément multimédia, l’image finale s’affiche, comme cela se produit à la fin de la lecture du contenu d’un lecteur multimédia.

## <a name="play-spherical-video-with-mediaplayer"></a>Lire du contenu vidéo sphérique avec MediaPlayer
À partir de Windows10, version1703, **MediaPlayer** prend en charge la projection équirectangulaire pour la lecture de contenu vidéo sphérique. Un contenu vidéo sphérique ne diffère pas d’un contenu vidéo 2D standard, étant donné que **MediaPlayer** restitue cette vidéo aussi longtemps que l’encodage vidéo est pris en charge. Dans le cas d’une vidéo sphérique contenant une balise de métadonnées qui indique que la vidéo utilise une projection équirectangulaire, **MediaPlayer** peut restituer la vidéo en utilisant un champ de vision et une orientation de vue spécifiés. Cette approche autorise les scénarios tels que la lecture d’une vidéo de réalité virtuelle avec un casque ou des lunettes d’affichage ou le simple défilement d’un contenu vidéo sphérique à l’aide d’une entrée à la souris ou au clavier.

Pour lire un contenu vidéo sphérique, suivez la procédure de lecture d’un contenu vidéo précédemment décrite dans cet article. Une étape supplémentaire consiste à enregistrer un gestionnaire pour l’événement [**MediaPlayer.MediaOpened**])https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened). Cet événement vous permet d’activer et de contrôler les paramètres de lecture de contenu vidéo sphérique.

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

Dans le gestionnaire **MediaOpened**, commencez par vérifier le format d’image de l’élément multimédia nouvellement ouvert en examinant la propriété [**PlaybackSession.SphericalVideoProjection.FrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat). Si cette propriété présente la valeur [**SphericaVideoFrameFormat.Equirectangular**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), le système peut projeter automatiquement le contenu vidéo. Commencez par définir la propriété [**PlaybackSession.SphericalVideoProjection.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) sur la valeur **true**. Vous pouvez également ajuster les propriétés telles que l’orientation de vue et le champ de vision, que le lecteur multimédia utilisera pour projeter le contenu vidéo. Dans cet exemple, le champ de vision est défini sur une largeur de 120degrés par la configuration de la propriété [**HorizontalFieldOfViewInDegrees**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees).

Si le contenu vidéo est sphérique, mais que son format n’est pas équirectangulaire, vous pouvez implémenter votre propre algorithme de projection en utilisant le mode serveur d’images du lecteur multimédia pour recevoir et traiter les images individuelles.

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

L’exemple de code ci-après indique comment ajuster l’orientation de vue d’une vidéo sphérique à l’aide des touches de direction gauche et droite.

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

Si votre application prend en charge les playlists de contenu vidéo, vous voudrez peut-être identifier les éléments de lecture qui contiennent du contenu vidéo sphérique dans votre interface utilisateur. Les playlists multimédias sont décrites en détail dans l’article [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md). L’exemple ci-après présente les procédures de création d’une playlist, d’ajout d’un élément et d’inscription d’un gestionnaire pour l’événement [**MediaPlaybackItem.VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged), qui se produit lorsque les pistes vidéo d’un élément multimédia sont résolues.

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

Dans le gestionnaire d’événements **VideoTracksChanged**, récupérez les propriétés d’encodage de toutes les pistes vidéo ajoutées en appelant [**VideoTrack.GetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.core.videotrack.GetEncodingProperties). Si la propriété [**SphericalVideoFrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) des propriétés d’encodage présente une autre valeur que [**SphericaVideoFrameFormat.None**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), la piste vidéo contient un contenu vidéo sphérique, et vous pouvez mettre à jour votre interface utilisateur en conséquence si vous le souhaitez.

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>Utiliser MediaPlayer en mode serveur d’images
À partir de Windows10, version1703, vous pouvez utiliser **MediaPlayer** en mode serveur d’images. Dans ce mode, **MediaPlayer** ne restitue pas automatiquement les images dans un objet **MediaPlayerElement** associé. À la place, votre application copie l’image actuelle de **MediaPlayer** dans un objet qui implémente [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). Le scénario principal autorisé par cette fonctionnalité consiste à utiliser des nuanceurs de pixels pour traiter les trames vidéo fournies par **MediaPlayer**. Votre application assure l’affichage de chaque image après le traitement, par exemple en affichant l’image dans un contrôle [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) XAML.

Dans l’exemple ci-après, un nouveau **MediaPlayer** est initialisé, et le contenu vidéo est chargé. Ensuite, un gestionnaire pour [**VideoFrameAvailable**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) est inscrit. Le mode serveur d’images est activé par la définition de la propriété [**IsVideoFrameServerEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) de l’objet **MediaPlayer** sur la valeur **true**. Enfin, la lecture multimédia est démarrée par l’appel de la méthode [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play).

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

L’exemple ci-après présente un gestionnaire pour **VideoFrameAvailable** qui utilise [Win2D](https://github.com/Microsoft/Win2D) pour ajouter un effet de flou simple à chaque image d’une vidéo, puis affiche les images traitées dans un contrôle [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) XAML.

Chaque fois que le gestionnaire **VideoFrameAvailable** est appelé, la méthode [**CopyFrameToVideoSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) est utilisée pour copier le contenu de l’image dans une interface [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). Vous pouvez également utiliser [**CopyFrameToStereoscopicVideoSurfaces**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) pour copier un contenu3D dans deux surfaces, afin de traiter le contenu de l’œil gauche séparément de celui de l’œil droit. Pour obtenir un objet qui implémente **IDirect3DSurface**, cet exemple crée un élément [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap), puis utilise cet objet pour créer un élément **CanvasBitmap** Win2D, qui implémente l’interface nécessaire. Un élément **CanvasImageSource** est un objet Win2D utilisable en tant que source d’un contrôle **Image**; par conséquent, un nouvel élément de ce type est créé et défini comme source du contrôle **Image** dans lequel le contenu sera affiché. Ensuite, un objet **CanvasDrawingSession** est créé. Cet objet est utilisé par Win2D pour restituer l’effet de flou.

Une fois que tous les objets nécessaires ont été instanciés, la méthode **CopyFrameToVideoSurface** est appelée et copie l’image actuelle de **MediaPlayer** dans l’objet **CanvasBitmap**. Ensuite, un objet **GaussianBlurEffect** Win2D est créé avec l’élément **CanvasBitmap** défini en tant que source de l’opération. Enfin, **CanvasDrawingSession.DrawImage** est appelé pour dessiner l’image source, avec l’effet de flou appliqué, dans l’objet **CanvasImageSource** qui a été associé au contrôle **Image**, ce qui entraîne son dessin dans l’interface utilisateur.

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

Pour plus d’informations sur Win2D, consultez le [référentiel GitHub Win2D](https://github.com/Microsoft/Win2D). Pour tester l’exemple de code ci-dessus, vous devrez ajouter le package NuGet Win2D à votre projet en suivant les instructions ci-après.

**Pour ajouter le package NuGet Win2D à votre projet d’effet**

1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur votre projet, puis sélectionnez **Gérer les packages NuGet**.
2.  Dans la partie supérieure de la fenêtre, sélectionnez l’onglet **Parcourir**.
3.  Dans la zone de recherche, entrez **Win2D**.
4.  Sélectionnez **Win2D.uwp**, puis **Installer** dans le volet droit.
5.  La boîte de dialogue **Examiner les modifications** vous indique le package à installer. Cliquez sur **OK**.
6.  Acceptez la licence du package.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Détecter les changements de niveau audio initiés par le système et y répondre
À partir de Windows10, version1803, votre application peut détecter quand le système baisse ou désactive le niveau audio d’un objet **MediaPlayer** en cours de lecture. Par exemple, le système peut baisser, ou «atténuer», le niveau de lecture audio lorsqu’une alarme sonne. Le système désactive votre application lorsqu’elle passe à l’arrière-plan si la fonctionnalité *backgroundMediaPlayback* n’a pas été déclarée dans le manifeste de l’application. La classe [**AudioStateMonitor**](https://docs.microsoft.comuwp/api/windows.media.audio.audiostatemonitor) vous permet de vous inscrire pour recevoir un événement lorsque le système modifie le volume d’un flux audio. Accédez à la propriété **AudioStateMonitor** d’un objet **MediaPlayer** et enregistrez un gestionnaire pour l’événement [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) afin d’être averti lorsque le niveau audio de cet objet **MediaPlayer** est modifié par le système.

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Lors de la gestion de l’événement **SoundLevelChanged**, vous pouvez effectuer différentes actions selon le type de contenu en cours de lecture. Si vous lisez actuellement de la musique, vous souhaitez peut-être continuer la lecture lorsque le volume est atténué. Toutefois, si vous lisez un podcast, vous souhaitez probablement suspendre la lecture lorsque le volume est atténué afin que l’utilisateur ne manque aucun contenu.

Cet exemple déclare une variable pour déterminer si le contenu en cours de lecture est un podcast. On suppose que vous la définissez sur la valeur appropriée lorsque vous sélectionnez le contenu de l’objet **MediaPlayer**. Nous créons également une variable de classe pour déterminer quand suspendre par programme la lecture lorsque le niveau audio est modifié.

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

Dans le gestionnaire d’événements **SoundLevelChanged**, vérifiez la propriété [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) de l’expéditeur **AudioStateMonitor** pour déterminer le nouveau niveau sonore. Cet exemple vérifie si le nouveau niveau sonore est défini sur le volume complet, ce qui signifie que le système a cessé de désactiver ou d’atténuer le volume, ou si le niveau sonore a été baissé mais un contenu autre qu’un podcast est en cours de lecture. Si l’une de ces conditions est remplie et que le contenu a été précédemment suspendu par programme, la lecture reprend. Si le nouveau niveau sonore est désactivé ou si le contenu actuel est un podcast et le niveau sonore est faible, la lecture est suspendue, et la variable est définie pour vérifier que la pause a été initiée par programme.

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

L’utilisateur peut décider de suspendre ou de continuer la lecture, même si le volume est atténué par le système. Cet exemple décrit les gestionnaires d’événements pour un bouton de lecture et de pause. Dans le gestionnaire de clics du bouton de pause, si la lecture avait déjà été suspendue par programme, nous mettons à jour la variable pour indiquer que l’utilisateur a suspendu le contenu. Dans le gestionnaire de clics du bouton de lecture, nous reprenons la lecture et supprimons notre variable de suivi.

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>Rubriquesassociées
* [Lecture de contenu multimédia](media-playback.md)
* [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md)
* [Intégrer avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md)
* [Créer, planifier et gérer des coupures de médias](create-schedule-and-manage-media-breaks.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md)





 

 




