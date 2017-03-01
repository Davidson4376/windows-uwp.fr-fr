---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "Cet article décrit comment ajouter la lecture de contenu multimédia en streaming adaptatif à une application de plateforme Windows universelle (UWP). Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en streaming HTTP (HLS) et de contenu en streaming dynamique sur HTTP (DASH)."
title: Streaming adaptatif
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3afd0440d8e552ebc3459c5fe30dd766db3ae8b9
ms.lasthandoff: 02/07/2017

---

# <a name="adaptive-streaming"></a>Streaming adaptatif

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132).\]

Cet article décrit comment ajouter la lecture de contenu multimédia en streaming adaptatif à une application de plateforme Windows universelle (UWP). Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en streaming HTTP (HLS) et de contenu en streaming dynamique sur HTTP (DASH).

Pour obtenir la liste des balises de protocole HLS prises en charge, voir [Prise en charge des balises HLS](hls-tag-support.md). 

> [!NOTE] 
> Le code de cet article a été adapté à partir de l’[exemple de streaming adaptatif](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) UWP.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Streaming adaptatif avec MediaPlayer et MediaPlayerElement

Pour lire du contenu multimédia en streaming adaptatif dans une application UWP, créez un objet **URI** pointant sur un fichier manifeste HLS ou DASH. Créez une instance de la classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Appelez [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) pour créer un objet **MediaSource**, puis définissez-le sur la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) de **MediaPlayer**. Appelez [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) pour démarrer la lecture du contenu multimédia.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

L’exemple ci-dessus lit l’audio du contenu multimédia, mais n’affiche pas automatiquement le contenu dans votre interface Utilisateur. La plupart des applications qui lisent du contenu vidéo veulent restituer le contenu dans une page XAML.  Pour ce faire, ajoutez un contrôle [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) à votre page XAML.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Appelez [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) pour créer un **MediaSource** à partir de l’URI d’un fichier manifeste HLS ou DASH. Ensuite, définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) de **MediaPlayerElement**. **MediaPlayerElement** crée automatiquement un objet **MediaPlayer** pour le contenu. Vous pouvez appeler **Play** sur **MediaPlayer** pour démarrer la lecture du contenu.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> À compter de Windows 10, version 1607, nous vous recommandons d’utiliser la classe **MediaPlayer** pour lire des éléments multimédias. **MediaPlayerElement** est un contrôle XAML léger utilisé pour afficher le contenu d’un **MediaPlayer** dans une page XAML. Le contrôle **MediaElement** continue d’être pris en charge pour la compatibilité descendante. Pour plus d’informations sur l’utilisation de **MediaPlayer** et **MediaPlayerElement** pour lire du contenu multimédia, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). Pour plus d’informations sur l’utilisation de **MediaSource** et des API associées pour utiliser du contenu multimédia, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Streaming adaptatif avec AdaptiveMediaSource

Si votre application requiert des fonctionnalités de streaming adaptatif plus avancées, telles que la fourniture d’en-têtes HTTP personnalisés, la surveillance des débits de téléchargement et de lecture en cours ou l’ajustement des ratios qui déterminent le moment où le système change les débits du flux adaptatif, utilisez l’objet [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

Les API de streaming adaptatif figurent dans l’espace de noms [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279).

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

Initialisez l’élément **AdaptiveMediaSource** avec l’URI d’un fichier manifeste de streaming adaptatif en appelant la méthode [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). La valeur [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) renvoyée par cette méthode vous permet de savoir si la source du contenu multimédia a été correctement créée. Si tel est le cas, vous pouvez définir l’objet comme source de flux pour votre **MediaPlayer** en appelant [**SetMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn652653). Dans cet exemple, la propriété [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) est interrogée pour déterminer le débit maximal pris en charge pour ce flux, puis cette valeur est définie en tant que débit initial. Cet exemple inscrit également les gestionnaires pour les événements [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) et [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) qui sont décrits plus loin dans cet article.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

Si vous avez besoin de définir des en-têtes HTTP personnalisés pour l’obtention du fichier manifeste, vous pouvez créer un objet [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), définir les en-têtes souhaités, puis transmettre l’objet dans la surcharge de **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

L’événement [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) se déclenche lorsque le système est sur le point de récupérer une ressource à partir du serveur. La classe [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) transmise dans le gestionnaire d’événements expose les propriétés qui fournissent des informations sur la ressource demandée, telles que le type et l’URI de la ressource.

Vous pouvez utiliser le gestionnaire d’événements **DownloadRequested** pour modifier la demande de ressource en mettant à jour les propriétés de l’objet [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) fourni par les arguments d’événement. Dans l’exemple ci-après, l’URI à partir de laquelle la ressource sera récupérée est modifié par la mise à jour des propriétés [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) de l’objet de résultat.

Vous pouvez remplacer le contenu de la ressource demandée en définissant les propriétés [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) ou [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) de l’objet de résultat. Dans l’exemple ci-après, le contenu de la ressource de manifeste est remplacé par la définition de la propriété **Buffer**. Notez que si vous mettez à jour la demande de ressource avec des données obtenues en mode asynchrone, par exemple dans le cadre d’une récupération de données à partir d’un serveur distant ou d’une authentification utilisateur asynchrone, vous devez appeler [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) pour obtenir un report, puis [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) une fois l’opération terminée pour signaler au système que l’opération de demande de téléchargement peut continuer.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

L’objet **AdaptiveMediaSource** fournit des événements qui vous permettent de réagir lorsque les débits de téléchargement ou de lecture changent. Dans cet exemple, les débits actuels sont simplement mis à jour dans l’interface utilisateur. Notez que vous pouvez modifier les ratios qui déterminent le moment où le système change les débits du flux adaptatif. Pour plus d’informations, voir la propriété [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="related-topics"></a>Rubriques connexes
* [Lecture de contenu multimédia](media-playback.md)
* [Prise en charge des balises HLS](hls-tag-support.md) 
* [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md) 






