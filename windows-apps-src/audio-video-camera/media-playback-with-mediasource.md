---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: Cet article vous montre comment utiliser MediaSource, qui offre une méthode courante de référencement et de lecture de contenu multimédia à partir de différentes sources telles que des fichiers locaux ou distants, et présente un modèle commun d’accès aux données multimédias, quel que soit le format multimédia sous-jacent.
title: Éléments, playlists et pistes multimédias
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3c3929c2b3765bd90dbe0be687834e94b4f222fc
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7970799"
---
# <a name="media-items-playlists-and-tracks"></a>Éléments, playlists et pistes multimédias


 Cet article vous montre comment utiliser la classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), qui offre une méthode courante de référencement et de lecture de contenu multimédia à partir de différentes sources telles que des fichiers locaux ou distants, et présente un modèle commun d’accès aux données multimédias, quel que soit le format multimédia sous-jacent. La classe [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) étend les fonctionnalités de **MediaSource**, vous permettant ainsi de gérer et de sélectionner à partir de plusieurs pistes audio, vidéo et de métadonnées contenues dans un élément multimédia. [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) vous permet de créer des listes de lecture à partir d’un ou plusieurs éléments de la lecture multimédia.


## <a name="create-and-play-a-mediasource"></a>Créer et lire un MediaSource

Créez une instance de **MediaSource** en appelant l’une des méthodes de fabrique exposées par la classe :

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)
-   [**CreateFromDownloadOperation**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

Après avoir créé un objet **MediaSource**, vous pouvez le lire avec un [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) en définissant la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010). À compter de Windows10, version1607, vous pouvez attribuer un **MediaPlayer** à un [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) en appelant [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) pour afficher le contenu du lecteur multimédia dans une page XAML. Cette méthode est préférée à l’utilisation de **MediaElement**. Pour plus d’informations sur l’utilisation de **MediaPlayer**, voir [**Lire du contenu audio et vidéo avec MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

L’exemple suivant montre comment lire un fichier multimédia sélectionné par l’utilisateur dans un **MediaPlayer** à l’aide de **MediaSource**.

Vous devez inclure les espaces de noms [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) et [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) pour pouvoir effectuer ce scénario.

[!code-cs[Using](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetUsing)]

Déclarez une variable de type **MediaSource**. Pour les exemples de cet article, la source multimédia est déclarée en tant que membre de classe. Elle est donc accessible à partir de plusieurs emplacements.

[!code-cs[DeclareMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Déclarez une variable pour stocker l’objet **MediaPlayer** et, si vous voulez afficher le contenu multimédia en XAML, ajoutez un contrôle **MediaPlayerElement** à votre page.

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

Pour permettre à l’utilisateur de sélectionner un fichier multimédia à lire, utilisez un objet [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847). Avec l’objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) retourné de la méthode [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) du sélecteur, initialisez un nouveau MediaObject en appelant [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909). Enfin, définissez la source du média en tant que source de lecture de **MediaElement** en appelant la méthode [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085).

[!code-cs[PlayMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

Par défaut, l’objet **MediaPlayer** ne commence pas automatiquement la lecture quand la source du média est définie. Vous pouvez démarrer manuellement la lecture en appelant [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play).

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

Vous pouvez également définir la propriété [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) de **MediaPlayer** sur true pour indiquer au lecteur de commencer la lecture dès que la source du média est définie.

[!code-cs[AutoPlay](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Créer un objet MediaSource à partir d’un objet DownloadOperation
À partir de Windows, version1803, vous pouvez créer un objet **MediaSource** à partir d’un objet **DownloadOperation**.

[!code-cs[CreateMediaSourceFromDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetCreateMediaSourceFromDownload)]

Notez que vous pouvez créer un objet **MediaSource** à partir d’un téléchargement sans le démarrer ou définir sa propriété **IsRandomAccessRequired** sur true, mais vous devez effectuer ces deux opérations avant de tenter d’associer l’objet **MediaSource** à un objet **MediaPlayer** ou **MediaPlayerElement** pour la lecture.

[!code-cs[StartDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetStartDownload)]


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>Gérer plusieurs pistes audio, vidéo et de métadonnées avec MediaPlaybackItem

L’utilisation d’un objet [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) pour la lecture est pratique car il offre une méthode courante de lecture multimédia à partir de différents types de sources. Un comportement plus avancé est toutefois disponible en créant un [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) à partir de **MediaSource**. Il inclut la possibilité d’accéder et de gérer plusieurs pistes audio, vidéo et de données pour un élément multimédia.

Déclarez une variable pour stocker votre **MediaPlaybackItem**.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Créez un objet **MediaPlaybackItem** en appelant le constructeur et en le transmettant à un objet **MediaSource** initialisé.

Si votre application prend en charge plusieurs pistes audio, vidéo ou de données dans un élément de lecture multimédia, inscrivez des gestionnaires d’événements pour les événements [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) ou [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952).

Enfin, définissez la source de la lecture de **MediaElement** ou **MediaPlayer** sur votre **MediaPlaybackItem**.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> Un objet **MediaSource** ne peut être associé qu’à un seul objet **MediaPlaybackItem**. Après avoir créé un **MediaPlaybackItem** à partir d’une source, toute tentative de création d’un autre élément de lecture à partir de la même source entraîne une erreur. De plus, après avoir créé un **MediaPlaybackItem** à partir d’une source de média, vous ne pouvez pas définir l’objet **MediaSource** directement en tant que source d’un **MediaPlayer**, mais vous devez plutôt utiliser le **MediaPlaybackItem**.

L’événement [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) est déclenché après qu’un **MediaPlaybackItem** contenant plusieurs pistes vidéo a été attribué en tant que source de lecture, et peut être redéclenché si la liste des pistes vidéo change pour l’élément. Le gestionnaire de cet événement vous permet de mettre à jour votre interface utilisateur, permettant ainsi à l’utilisateur de basculer entre les pistes disponibles. Cet exemple utilise un objet [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) pour afficher les pistes vidéo disponibles.

[!code-xml[VideoComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetVideoComboBox)]

Dans le gestionnaire **VideoTracksChanged**, parcourez toutes les pistes de la liste [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) de l’élément de lecture. Un nouveau [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349) est créé pour chaque piste. Si la piste ne porte déjà une étiquette, une étiquette est générée à partir de l’index de piste. La propriété [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) de l’élément de zone de liste déroulante est définie sur l’index de piste pour pouvoir l’identifier ultérieurement. Enfin, l’élément est ajouté à la zone de liste déroulante. Notez que ces opérations sont effectuées dans le cadre d’un appel [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317), car toutes les modifications de l’interface utilisateur doivent être apportées sur le thread d’interface utilisateur et que cet événement est déclenché sur un autre thread.

[!code-cs[VideoTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

Dans le gestionnaire [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) de la zone de liste déroulante, l’index de piste est récupéré à partir de la propriété **Tag** de l’élément sélectionné. La définition de la propriété [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) de la liste [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) de l’élément de lecture multimédia amène **MediaElement** ou **MediaPlayer** à basculer la piste vidéo active sur l’index spécifié.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

La gestion des éléments multimédias avec des pistes audio multiples est strictement identique à celle des pistes vidéo. Gérez le [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948) pour mettre à jour votre interface utilisateur avec les pistes audio trouvées dans la liste [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) de l’élément de lecture. Lorsque l’utilisateur sélectionne une piste audio, définissez la propriété [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) de la liste **AudioTracks** pour que **MediaElement** ou **MediaPlayer** bascule la piste audio active sur l’index spécifié.

[!code-xml[AudioComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

En plus de l’audio et de la vidéo, un objet **MediaPlaybackItem** peut contenir zéro ou plusieurs objets [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580). Une piste de métadonnées synchronisée peut contenir du texte de sous-titre. Elle peut également contenir des données personnalisées propriétaires de votre application. Une piste de métadonnées synchronisée contient une liste d’indicateurs représentés par des objets qui héritent de [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899), un [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) ou un [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) par exemple. Chaque indicateur a une heure de début et une durée qui déterminent quand l’indicateur est activé et pour combien de temps.

Comme les pistes audio et vidéo, les pistes de métadonnées synchronisées d’un élément multimédia peuvent être détectées en gérant l’événement [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) d’un **MediaPlaybackItem**. Toutefois, avec les pistes de métadonnées synchronisées, l’utilisateur souhaitera peut-être activer plusieurs pistes de métadonnées simultanément. De plus, en fonction de votre scénario d’application, vous souhaiterez peut-être activer ou désactiver automatiquement des pistes de métadonnées, sans l’intervention de l’utilisateur. À des fins d’illustration, cet exemple ajoute un objet [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795) pour chaque piste de métadonnées d’un élément multimédia pour permettre à l’utilisateur d’activer et de désactiver la piste. La propriété **Tag** de chaque bouton est définie sur l’index de la piste de métadonnées associée pour pouvoir l’identifier lorsque le bouton est activé.

[!code-xml[MetaStackPanel](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Étant donné que plusieurs pistes de métadonnées peuvent être actives simultanément, vous ne définissez pas simplement l’index actif de la liste de pistes de métadonnées. Appelez plutôt le **MediaPlaybackItem** de la méthode [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) de l’objet, en indiquant l’index de la piste que vous souhaitez activer/désactiver, puis en spécifiant une valeur à partir de l’énumération [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016). Le mode de présentation que vous choisissez dépend de l’implémentation de votre application. Dans cet exemple, la piste de métadonnées est définie sur **PlatformPresented** lorsqu’elle est activée. Pour les pistes de texte, cela signifie que le système affiche automatiquement les indicateurs de texte dans la piste. Lorsque le bouton d’activation/de désactivation est désactivé, le mode de présentation est défini sur **Disabled**, ce qui signifie qu’aucun texte ne s’affiche et qu’aucun événement d’indicateur n’est déclenché. Les événements d’indicateur sont présentés plus loin dans cet article.

[!code-cs[ToggleChecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

Comme vous traitez les pistes de métadonnées, vous pouvez accéder à l’ensemble des repères de la piste avec les propriétés [**Cues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.Cues) ou [**ActiveCues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.ActiveCues). Pour ce faire, mettez à jour votre interface utilisateur pour afficher les emplacements de repère d’un élément multimédia.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>Gérer les codecs non pris en charge et les erreurs inconnues à l’ouverture des éléments multimédias
À compter de Windows10, version1607, vous pouvez vérifier si le codec nécessaire pour lire un élément multimédia est pris en charge entièrement ou partiellement sur l’appareil sur lequel s’exécute votre application. Dans le gestionnaire des événements de modification de pistes **MediaPlaybackItem**, comme [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracksChanged), vérifiez d’abord si la modification de la piste consiste en l’insertion d’une nouvelle piste. Auquel cas, vous pouvez obtenir une référence à la piste insérée à l’aide de l’index transmis dans le paramètre **IVectorChangedEventArgs.Index** avec la collection de pistes appropriée du paramètre **MediaPlaybackItem**, comme la collection [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracks).

Une fois que vous avez une référence à la piste insérée, vérifiez le [**DecoderStatus**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrackSupportInfo.DecoderStatus) de la propriété [**SupportInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.SupportInfo) de la piste. Si la valeur est [**FullySupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), le codec approprié nécessaire pour lire la piste est présent sur l’appareil. Si la valeur est [**Degraded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), la piste peut être lue par le système, mais la lecture est détériorée d’une certaine façon. Par exemple, une piste audio5.1 peut être lue à la place comme une piste stéréo bicanale. Si c’est le cas, vous pouvez mettre à jour votre interface utilisateur pour alerter l’utilisateur de la détérioration. Si la valeur est [**UnsupportedSubtype**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) ou [**UnsupportedEncoderProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), la piste ne peut pas être lue du tout avec les codecs actuels de l’appareil. Vous pouvez alerter l’utilisateur et ignorez la lecture de l’élément ou implémenter une interface utilisateur pour permettre à l’utilisateur de télécharger le codec correct. La méthode [**GetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.GetEncodingProperties) de la piste peut être utilisée pour déterminer le codec nécessaire pour la lecture.

Enfin, vous pouvez vous inscrire à l’événement [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) de la piste, qui est déclenché si la piste est prise en charge sur l’appareil, mais qu’elle ne peut pas s’ouvrir en raison d’une erreur inconnue dans le pipeline.

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

Dans le gestionnaire d’événements [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed), vous pouvez vérifier si l’état de **MediaSource** est inconnu et si tel est le cas, vous pouvez sélectionner par programmation une autre piste à lire, autoriser l’utilisateur à choisir une autre piste ou abandonner la lecture.

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>Définir les propriétés d’affichage utilisées par les contrôles de transport de média système
À compter de Windows10, version1607, le contenu multimédia lu dans un [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) est automatiquement intégré aux contrôles de transport de média système par défaut. Vous pouvez spécifier les métadonnées que les contrôles de transport de média système doivent afficher en mettant à jour les propriétés d’affichage d’un **MediaPlaybackItem**. Obtenez un objet qui représente les propriétés d’affichage d’un élément en appelant [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties). Déterminez si l’élément de lecture est de la musique ou une vidéo en définissant la propriété [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type). Ensuite, définissez les propriétés [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) ou [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) de l’objet. Appelez [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923) pour mettre à jour les propriétés de l’élément sur les valeurs que vous avez indiquées. En règle générale, une application récupère les valeurs d’affichage de manière dynamique à partir d’un service web, mais l’exemple suivant illustre ce processus avec des valeurs codées en dur.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="add-external-timed-text-with-timedtextsource"></a>Ajouter du texte synchronisé externe avec TimedTextSource

Dans certains scénarios, vous pouvoir disposer de fichiers externes contenant du texte synchronisé associé à un élément multimédia, des fichiers distincts contenant des sous-titres pour différents paramètres régionaux par exemple. Utilisez la classe [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) pour charger les fichiers de texte synchronisé externe à partir d’un flux ou d’une URI.

Cet exemple utilise une collection **Dictionary** pour stocker une liste de sources de texte synchronisé pour l’élément multimédia à l’aide de l’URI source et de l’objet **TimedTextSource** en tant que paire clé/valeur afin d’identifier les pistes une fois qu’elles ont été résolues.

[!code-cs[TimedTextSourceMap](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Créez un objet **TimedTextSource** pour chaque fichier de texte synchronisé externe en appelant la méthode [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190). Ajoutez une entrée pour le **Dictionary** de la source de texte synchronisé. Ajoutez un gestionnaire pour l’événement [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) à gérer si l’élément n’a pas pu être chargé ou pour définir des propriétés supplémentaires lorsque l’élément a bien été chargé.

Inscrivez tous vos objets **TimedTextSource** avec **MediaSource** en les ajoutant à la collection [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916). Notez que les sources de texte synchronisé externe sont ajoutées directement à **MediaSource** et non au **MediaPlaybackItem** créé à partir de la source. Pour mettre à jour votre interface utilisateur afin de refléter les pistes de texte externe, inscrivez et gérez l’événement **TimedMetadataTracksChanged** comme décrit précédemment dans cet article.

[!code-cs[TimedTextSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

Dans le gestionnaire de l’événement [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540), vérifiez la propriété **Error** de [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) transmise au gestionnaire pour déterminer si une erreur s’est produite lors de la tentative de chargement des données de texte synchronisé. Si l’élément a bien été résolu, vous pouvez utiliser ce gestionnaire pour mettre à jour des propriétés supplémentaires de la piste résolue. Cet exemple ajoute une étiquette à chaque piste en fonction de l’URI préalablement enregistré dans le **dictionnaire**.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## <a name="add-additional-metadata-tracks"></a>Ajouter des pistes de métadonnées supplémentaires

Vous pouvez créer des pistes de métadonnées personnalisées de manière dynamique dans le code et les associer à une source de média. Les pistes que vous créez peuvent contenir du texte de sous-titre. Elles peuvent également contenir vos données d’application propriétaires.

Créez un [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) en appelant le constructeur et en spécifiant un ID, l’identificateur de langue et une valeur de l’énumération [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578). Enregistrez des gestionnaires pour les événements [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) et [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584). Ces événements sont déclenchés lorsque l’heure de début d’un indicateur est atteinte et lorsque la durée d’un indicateur s’est écoulée, respectivement.

Créez un nouvel objet d’indicateur, adapté au type de piste de métadonnées que vous avez créée, puis définissez l’ID, l’heure de début et la durée de la piste. Cet exemple crée une piste de données, un ensemble d’objet [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) est donc généré et une mémoire tampon contenant des données spécifiques à l’application est fournie pour chaque indicateur. Pour inscrire la nouvelle piste, ajoutez-la à la collection [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) de l’objet **MediaSource**.

À partir de Windows10, version1703, la propriété **DataCue.Properties** expose un objet [**PropertySet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset) qui vous permet de stocker des propriétés personnalisées dans des paires clé/données pouvant être récupérées dans les événements **CueEntered** et **CueExited**.  

[!code-cs[AddDataTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

L’événement **CueEntered** est déclenché lorsque l’heure de début d’un indicateur est atteinte alors que la piste associée dispose d’un mode de présentation **ApplicationPresented**, **Hidden** ou **PlatformPresented.**. Les événements d’indicateur ne sont pas déclenchés pour les pistes de métadonnées lorsque le mode de présentation de la piste est **Disabled**. Cet exemple présente simplement les données personnalisées associées à l’indicateur dans la fenêtre de débogage.

[!code-cs[DataCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

Cet exemple ajoute une piste de texte personnalisé en spécifiant **TimedMetadataKind.Caption** lors de la création de la piste et de l’utilisation d’objets [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) pour ajouter des indicateurs à la piste.

[!code-cs[AddTextTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>Lire une liste d’éléments multimédias avec MediaPlaybackList

[**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) vous permet de créer une playlist d’éléments multimédias, qui sont représentés par des objets **MediaPlaybackItem**.

**Remarque**éléments dans un [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) sont rendus à l’aide de la lecture sans blanc. Le système utilise les métadonnées fournies dans les fichiers codés MP3 ou AAC pour déterminer la compensation de délai ou de remplissage nécessaire pour la lecture sans blanc. Si les fichiers codés MP3 ou AAC ne fournissent pas ces métadonnées, le système détermine alors le délai ou le remplissage de manière heuristique. Pour les formats sans perte, tels que PCM, FLAC ou ALAC, le système n’exécute aucune action, car ces encodeurs n’introduisent ni retard ni remplissage.

Pour commencer, déclarez une variable pour stocker votre **MediaPlaybackList**.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Créez un objet **MediaPlaybackItem** pour chaque élément multimédia que vous voulez ajouter à votre liste en suivant la procédure décrite précédemment dans cet article. Initialisez votre objet **MediaPlaybackList** et ajoutez-y les éléments de lecture multimédia. Inscrivez un gestionnaire pour l’événement [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957). Cet événement vous permet de mettre à jour votre interface utilisateur afin de refléter l’élément multimédia en cours de lecture. Vous pouvez également vous inscrire à l’événement [ItemOpened](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened), qui se déclenche lorsqu’un élément de la liste est correctement ouvert, ainsi qu’à l’événement [ItemFailed](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed), déclenché en cas d’échec de l’ouverture d’un élément de la liste.

À partir de Windows10, version1703, vous pouvez spécifier le nombre maximal d’objets **MediaPlaybackItem** de la liste **MediaPlaybackList** que le système gardera ouverts après leur lecture en définissant la propriété [MaxPlayedItemsToKeepOpen](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen). Lorsqu’un élément **MediaPlaybackItem** reste ouvert, sa lecture peut démarrer instantanément lorsque l’utilisateur bascule vers cet élément, car il n’est pas nécessaire de recharger ce dernier. Toutefois, le fait de garder des éléments ouverts augmente également la consommation de mémoire de votre application. Veillez donc à trouver un bon équilibre entre réactivité et utilisation de la mémoire lorsque vous définissez cette valeur. 

Pour activer la lecture de votre liste, définissez la source de lecture de **MediaPlayer** sur votre liste **MediaPlaybackList**.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

Dans le gestionnaire d’événements **CurrentItemChanged**, mettez à jour votre interface utilisateur afin de refléter l’élément en cours de lecture, qui peut être récupéré à l’aide de la propriété [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) de l’objet [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) transmis dans l’événement. N’oubliez pas que si vous mettez à jour l’interface utilisateur à partir de cet événement, vous devez le faire dans le cadre d’un appel à [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) afin que les mises à jour soient effectuées sur le thread d’interface utilisateur.

À partir de Windows10, version1703, vous pouvez vérifier la propriété [CurrentMediaPlaybackItemChangedEventArgs.Reason](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) pour obtenir une valeur indiquant le motif de la modification de l’élément, tel que le basculement automatique de l’application vers un autre élément, la fin de la lecture de l’élément précédent ou l’apparition d’une erreur.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]


Appelez [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) ou [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) pour que le lecteur multimédia lise l’élément précédent ou suivant de votre **MediaPlaybackList**.

[!code-cs[PrevButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNextButton)]

Définissez la propriété [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) pour spécifier si le lecteur multimédia doit lire les éléments de votre liste dans un ordre aléatoire.

[!code-cs[ShuffleButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Définissez la propriété [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) pour spécifier si le lecteur multimédia doit lire votre liste en boucle.

[!code-cs[RepeatButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRepeatButton)]


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>Gérer l’échec d’éléments multimédias dans une liste de lecture
L’événement [**ItemFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.ItemFailed) est déclenché quand un élément dans la liste ne parvient pas à s’ouvrir. La propriété [**ErrorCode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError.ErrorCode) de l’objet [**MediaPlaybackItemError**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError) transmis au gestionnaire énumère la cause spécifique de l’échec dans la mesure du possible, y compris les erreurs réseau, les erreurs de décodage ou les erreurs de chiffrement.

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

### <a name="disable-playback-of-items-in-a-playback-list"></a>Désactiver la lecture des éléments d’une liste de lecture
À partir de Windows10, version1703, vous pouvez désactiver la lecture d’un ou de plusieurs éléments d’une liste **MediaPlaybackItemList** en définissant la propriété [IsDisabledInPlaybackList](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) d’un objet [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) sur la valeur «false». 

Un scénario courant d’utilisation de cette fonctionnalité concerne les applications qui diffusent de la musique en continu à partir d’Internet. L’application peut écouter les modifications d’état de connexion réseau de l’appareil et désactiver la lecture des éléments qui ne sont pas complètement téléchargés. Dans l’exemple ci-après, un gestionnaire est inscrit pour l’événement [NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged).

[!code-cs[RegisterNetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRegisterNetworkStatusChanged)]

Dans le gestionnaire de l’événement **NetworkStatusChanged**, vérifiez si [GetInternetConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) renvoie la valeur Null, indiquant que le réseau n’est pas connecté. Si tel est le cas, parcourez tous les éléments de la liste de lecture, et si la propriété [TotalDownloadProgress](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) de l’élément présente une valeur inférieure à 1, signalant que l’élément n’est pas intégralement téléchargé, désactivez l’élément. Si la connexion réseau est activée, parcourez tous les éléments de la liste de lecture et activez chacun d’eux.

[!code-cs[NetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNetworkStatusChanged)]

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Différer la liaison de contenu multimédia pour les éléments d’une liste de lecture à l’aide de MediaBinder
Dans les exemples précédents, un objet **MediaSource** est créé à partir d’un fichier, d’une URL ou d’un flux, après quoi un élément **MediaPlaybackItem** est créé et ajouté à une liste **MediaPlaybackList**. Dans certains scénarios, par exemple lorsque la visualisation d’un contenu est facturée à l’utilisateur, vous voudrez peut-être différer la récupération du contenu d’un objet **MediaSource** jusqu’à ce que l’élément de la liste de lecture soit réellement prêt à être lu. Pour implémenter ce scénario, créez une instance de la classe [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder). Définissez la propriété [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) sur une chaîne définie par l’application qui identifie le contenu dont vous souhaitez différer la récupération, puis inscrivez un gestionnaire pour l’événement [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding). Ensuite, créez un élément **MediaSource** à partir de l’objet **Binder** en appelant la méthode [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder). Puis créez un élément **MediaPlaybackItem** à partir de l’objet **MediaSource** et ajoutez-le à la liste de lecture en suivant la procédure habituelle.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

Lorsque le système détermine que le contenu associé à **MediaBinder** doit être récupéré, il déclenche l’événement **Binding**. Dans le gestionnaire de cet événement, vous pouvez récupérer l’instance **MediaBinder** à partir de l’objet [**MediaBindingEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs) transmis dans l’événement. Récupérez la chaîne que vous avez spécifiée pour la propriété **Token** et utilisez-la pour déterminer le contenu à récupérer. L’objet **MediaBindingEventArgs** fournit des méthodes pour la définition du contenu lié dans différentes représentations, notamment [**SetStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**SetStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**SetStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference) et [**SetUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.seturi). 

[!code-cs[BinderBinding](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBinding)]

Notez que si vous effectuez des opérations asynchrones, telles que des requêtes web, dans le gestionnaire d’événements **Binding**, vous devrez appeler la méthode [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) pour demander au système d’attendre la fin de votre opération avant de poursuivre. Appelez la méthode [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) une fois l’opération terminée pour demander au système de continuer.

À partir de Windows10, version1703, vous pouvez fournir un objet [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) en tant que contenu lié en appelant [**SetAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource). Pour plus d’informations sur l’utilisation de la diffusion en continu adaptative dans votre application, consultez l’article [Streaming adaptatif](adaptive-streaming.md).



## <a name="related-topics"></a>Articles connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Intégrer aux contrôles de transport de média système](integrate-with-systemmediatransportcontrols.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md)

