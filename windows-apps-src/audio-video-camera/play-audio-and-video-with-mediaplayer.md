---
author: drewbatgit
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: "Cet article vous explique comment lire du contenu multimédia dans votre application Windows universelle avec MediaPlayer."
title: "Lire du contenu audio et vidéo avec MediaPlayer"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 79eabb1341d9a14ec924a1933917e92af797b65a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Lire du contenu audio et vidéo avec MediaPlayer

Cet article vous explique comment lire du contenu multimédia dans votre application Windows universelle avec la classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Avec la version 1607 de Windows10, des améliorations considérables ont été apportées aux API de lecture de contenus multimédias. Profitez notamment d’une conception simplifiée à un seul processus pour l’audio d’arrière-plan, de l’intégration automatique avec les contrôles de transport de média système, de la capacité de synchronisation de plusieurs lecteurs multimédias, de la capacité de connexion à une surface Windows.UI.Composition et d’une interface simple pour la création et la planification de coupures de médias dans votre contenu. Pour tirer parti de ces améliorations, la meilleure pratique recommandée pour la lecture de contenus multimédias consiste à utiliser la classe **MediaPlayer** en lieu et place de **MediaElement** pour la lecture de contenu multimédia. Le contrôle XAML léger, [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), a été introduit pour vous permettre d’afficher le contenu multimédia dans une page XAML. Nombre des API de statut et de contrôle de la lecture fournies par **MediaElement** sont désormais disponibles via le nouvel objet [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). **MediaElement** continue de fonctionner pour prendre en charge la compatibilité descendante, mais aucune fonction supplémentaire ne sera ajoutée à cette classe.

Cet article vous présente les fonctions **MediaPlayer** qu’une application standard de lecture de contenu multimédia utilise. Notez que **MediaPlayer** utilise la classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) en tant que conteneur pour l’ensemble des éléments multimédias. Cette classe vous permet de charger et de lire le contenu multimédia à partir de multiples sources différentes utilisant une interface unique, notamment les fichiers locaux, les flux de mémoire et les sources réseau. Il existe également des classes de niveau supérieur compatibles avec **MediaSource**, comme [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) et [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), qui fournissent des fonctions plus avancées comme des playlists et la capacité de gestion de sources multimédias avec plusieurs pistes audio, vidéo et de métadonnées. Pour plus d’informations sur **MediaSource** et les API associées, consultez la page [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).


##<a name="play-a-media-file-with-mediaplayer"></a>Lire un fichier multimédia avec MediaPlayer  
La lecture de contenu multimédia de base avec **MediaPlayer** est très simple à implémenter. Tout d’abord, créez une nouvelle instance de la classe **MediaPlayer**. Votre application peut présenter plusieurs instances **MediaPlayer** actives simultanément. Ensuite, définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) du lecteur sur un objet qui implémente l’interface [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource), comme un objet [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), un objet [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) ou un objet [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). Dans cet exemple, un objet **MediaSource** est créé à partir d’un fichier dans le stockage local de l’application, puis un objet **MediaPlaybackItem** est créé à partir de la source, puis affecté à la propriété **Source** du lecteur.

Contrairement à l’objet **MediaElement**, **MediaPlayer** ne démarre pas automatiquement la lecture par défaut. Vous pouvez démarrer la lecture en appelant [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play),en définissant la propriété [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) sur True, ou en attendant que l’utilisateur lance la lecture à l’aide des contrôles multimédias intégrés.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Lorsque vous avez terminé d’utiliser une instance **MediaPlayer** sur l’application, vous devez appeler la méthode [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) (projetée vers **Dispose** en C#) afin de nettoyer les ressources utilisées par le lecteur.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##<a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Utiliser MediaPlayerElement afin d’afficher du contenu vidéo dans XAML
Vous pouvez lire du contenu multimédia dans une instance **MediaPlayer** sans l’afficher au format XAML, mais de nombreuses applications de lecture de contenus multimédias sont définies pour ce type d’affichage. Pour ce faire, utilisez le contrôle léger [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement). Tout comme **MediaElement**, **MediaPlayerElement** vous permet de spécifier si les contrôles intégrés de transport doivent être affichés.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Vous pouvez définir l’instance **MediaPlayer** à laquelle l’élément est lié en appelant [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

Vous pouvez également définir la source de lecture sur l’instance **MediaPlayerElement**. Le cas échéant, l’élément crée automatiquement une nouvelle instance **MediaPlayer** à laquelle vous pouvez accéder à l’aide de la propriété [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer).

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Si vous désactivez l’élément [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) de l’instance [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) en définissant [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) sur false, le lien entre **MediaPlayer** et [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) fourni par **MediaPlayerElement** est rompu; autrement dit, les contrôles de transport intégrés ne contrôleront plus automatiquement la lecture du lecteur. Vous devrez donc implémenter vos propres contrôles pour pouvoir contrôler le **MediaPlayer**.

##<a name="common-mediaplayer-tasks"></a>Tâches courantes de MediaPlayer
Cette section vous explique comment utiliser certaines des fonctionnalités de l’instance **MediaPlayer**.

###<a name="set-the-audio-category"></a>Définir la catégorie audio
Définissez la propriété [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) d’une instance **MediaPlayer** sur l’une des valeurs de l’énumération [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) afin d’indiquer au système le type de contenu multimédia lu. Les jeux doivent classer leurs flux musicaux en tant qu’éléments **GameMedia**, de manière à ce que la musique du jeu soit désactivée automatiquement si de la musique s’active sur une autre application en arrière-plan. Les applications de musique ou de vidéo doivent classer leurs flux en tant qu’éléments **Media** ou **Movie**, de manière à les rendre prioritaires par rapport aux flux **GameMedia**.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###<a name="output-to-a-specific-audio-endpoint"></a>Sortie vers un point de terminaison audio spécifique
Par défaut, la sortie audio d’une instance **MediaPlayer** est acheminée vers le point de terminaison audio par défaut du système, mais vous pouvez définir un point de terminaison audio spécifique que l’instance **MediaPlayer** doit utiliser pour la sortie. Dans l’exemple ci-dessous, [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) renvoie une chaîne qui identifie spécifiquement la catégorie de rendu audio des appareils. Ensuite, l’élément [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) de la méthode [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) est appelé afin de récupérer une liste de l’ensemble des appareils disponibles du type sélectionné. Vous pouvez déterminer par programme l’appareil à utiliser ou ajouter les appareils renvoyés sur une instance [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox), afin de laisser à l’utilisateur le choix de l’appareil.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

Dans l’événement [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) de la zone déroulante des appareils, la propriété [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) de l’instance **MediaPlayer** est définie sur l’appareil sélectionné, qui était stocké dans la propriété [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) de l’instance **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###<a name="playback-session"></a>Session de lecture
Comme décrit précédemment dans cet article, nombre des fonctions exposées par la classe **MediaElement** ont été déplacées vers la classe [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). Cela concerne les informations sur l’état de lecture du lecteur, comme la position actuelle de lecture, l’action du lecteur (pause ou lecture) et la vitesse de lecture actuelle. **MediaPlaybackSession** fournit également plusieurs événements vous procurant des informations sur les modifications de l’état, notamment sur le statut actuel de mise en mémoire tampon et de téléchargement du contenu lu et sur la taille naturelle et les proportions du contenu vidéo actuellement lu.

L’exemple suivant vous illustre l’implémentation d’un gestionnaire de clic du bouton qui permet d’avancer de 10secondes dans le contenu. Tout d’abord, l’objet **MediaPlaybackSession** associé au lecteur est récupéré avec la propriété [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession). Ensuite, la propriété [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) est définie sur la position actuelle de lecture plus 10secondes.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

L’exemple suivant illustre l’utilisation d’un bouton bascule permettant de passer de la vitesse de lecture normale à la vitesse double, en définissant la propriété [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) de la session.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###<a name="pinch-and-zoom-video"></a>Pincement et zoom sur du contenu vidéo
**MediaPlayer** vous permet de spécifier le rectangle source au sein du contenu vidéo à afficher. Dès lors, vous pouvez effectuer un zoom dans la vidéo. Le rectangle que vous spécifiez est relatif à un rectangle normalisé (0,0,1,1), où 0,0 correspond au coin supérieur gauche de l’image et 1,1 spécifie la largeur et la hauteur complètes de l’image. Ainsi, par exemple, pour définir le rectangle de zoom de manière à afficher le quadrant supérieur droit de la vidéo, vous définiriez le rectangle (0,5; 0; 0,5; 0,5).  Il est important que vous vérifiiez vos valeurs afin de vous assurer que votre rectangle source se trouve dans le rectangle normalisé (0,0,1,1). Toute tentative de définition de la valeur en dehors de cette plage provoquera l’envoi d’une exception.

Pour implémenter les fonctionnalités de pincement et de zoom à l’aide d’entrées tactiles multipoint, vous devez dans un premier temps spécifier les entrées prises en charge. Dans cet exemple, les entrées de mise à l’échelle et de translation sont demandées. L’événement [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) est déclenché lorsque l’une des entrées enregistrées se produit. L’événement [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) est utilisé pour redéfinir le zoom sur l’intégralité de l’image. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

Ensuite, déclarez un objet **Rect** qui stockera le rectangle source actuel de zoom.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

Le gestionnaire **ManipulationDelta** ajuste la mise à l’échelle ou la translation du rectangle de zoom. Si la valeur delta de mise à l’échelle est différente de 1, cela signifie que l’utilisateur à effectuer un pincement. Si la valeur est supérieure à 1, le rectangle source doit être réduit pour la prise en charge du zoom dans le contenu. Si la valeur est inférieure à 1, le rectangle source doit être agrandi pour la prise en charge du zoom arrière dans le contenu. Avant la définition des nouvelles valeurs de mise à l’échelle, le rectangle obtenu est examiné afin de garantir qu’il s’intègre parfaitement dans les limites (0,0,1,1).

Si la valeur de mise à l’échelle est 1, l’entrée de translation est traitée. Le rectangle est simplement translaté selon la division du nombre de pixels de l’entrée par la valeur de largeur et de hauteur du contrôle. Là encore, le rectangle obtenu est examiné afin de garantir qu’il s’intègre parfaitement dans les limites (0,0,1,1).

Enfin, l’élément [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) de l’instance **MediaPlaybackSession** est défini sur le nouveau rectangle ajusté; il spécifie la zone de la vidéo à afficher.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

Dans le gestionnaire d’événement [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped), le rectangle source est redéfini sur (0,0,1,1) pour rétablir l’affichage de l’intégralité de la vidéo.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
        
##<a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Utilisez MediaPlayerSurface afin d’afficher la vidéo sur une surface Windows.UI.Composition
À partir de Windows10, version 1607, vous pouvez utiliser l’instance **MediaPlayer** afin d’afficher le contenu vidéo sur une interface [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface), ce qui permet au lecteur d’intergir avec les API dans l’espace de noms [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition). L’infrastructure de composition peut être mise à profit pour travailler avec des graphiques dans la couche visuelle située entre les API XAML et les API graphiques DirectX de niveau inférieur. Cela permet la prise en charge de scénarios tels que le rendu de contenus vidéo dans tout contrôle XAML. Pour plus d’informations sur l’utilisation des API de composition, consultez la page [Couche visuelle](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer).

L’exemple suivant illustre l’affichage d’un contenu de lecteur vidéo sur un contrôle [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas). Les appels spécifiques au lecteur multimédias de cet exemple sont [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) et [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** indique au système la talle de la mémoire tampon à allouer pour l’affichage du contenu. **GetSurface** prend un élément [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) en tant qu’argument et récupère une instance de la classe [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface). Cette classe procure un accès aux éléments **MediaPlayer** et **Compositor** utilisés pour créer la surface et l’expose via la propriété [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface).

Le reste du code de cet exemple crée un élément [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual) sur lequel la vidéo s’affiche et définit la taille sur celle de l’élément de zone de dessin qui affiche les éléments visuels. Ensuite, un élément [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) est créé à partir de la classe [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) et affecté à la propriété [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) des éléments visuels. À ce stade, un élément [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) est créé et l’instance **SpriteVisual** est insérée dans la partie supérieure de son arborescence visuelle. Enfin, [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) est appelé afin d’affecter les éléments visuels du conteneur au contrôle **Canvas**.

[!code-cs[Compositeur](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
##<a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Utilisez MediaTimelineController afin de synchroniser du contenu entre plusieurs couches.
Comme indiqué précédemment dans cet article, vous application peut disposer de plusieurs objets **MediaPlayer** actifs simultanément. Par défaut, chaque instance **MediaPlayer** créée fonctionne indépendamment. Pour certains scénarios, tels que la synchronisation d’une piste de commentaires sur une vidéo, vous voudrez synchroniser l’état du lecteur, la position de lecture et la vitesse de lecture de plusieurs couches. À partir de Windows10, version 1607, vous pouvez implémenter ce comportement à l’aide de la classe [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController).

###<a name="implement-playback-controls"></a>Implémenter des contrôles de lecture
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

###<a name="offset-the-playback-position-from-the-timeline-position"></a>Décaler la position de lecture de la position de la chronologie
Dans certains cas, il est possible que vous vouliez décaler la position de lecture d’un ou de plusieurs lecteurs multimédias des autres lecteurs. Pour ce faire, définissez la propriété [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) de l’objet **MediaPlayer** que vous voulez décaler. L’exemple suivant utilise les durées du contenu de deuxlecteurs multimédias pour définir les valeurs minimum et maximum du contrôle à deuxcurseurs sur «plus» ou «moins» la longueur de l’élément.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

Dans l’événement [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) associé à chaque curseur, la propriété **TimelineControllerPositionOffset** de chaque lecteur est définie sur la valeur correspondante.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Notez que si la valeur de décalage d’un lecteur correspond à une position de lecture négative, le contenu demeure interrompu jusqu’à ce que le décalage atteigne la valeur de 0, puis la lecture redémarre. De la même manière, si la valeur de décalage correspond à une position de lecture supérieure à la durée de l’élément multimédia, l’image finale s’affiche, comme cela se produit à la fin de la lecture du contenu d’un lecteur multimédia.

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md)
* [Intégrer avec les contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md)
* [Créer, planifier et gérer des coupures de médias](create-schedule-and-manage-media-breaks.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md)





 

 




