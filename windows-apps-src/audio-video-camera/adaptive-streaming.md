---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: Cet article décrit comment ajouter la lecture de contenu multimédia en streaming adaptatif à une application de plateforme Windows universelle (UWP). Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en streaming HTTP (HLS) et de contenu en streaming dynamique sur HTTP (DASH).
title: Streaming adaptatif
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef8e3ab4abd9ee9159dc7d5aa757f55e00817a51
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7563814"
---
# <a name="adaptive-streaming"></a>Streaming adaptatif


Cet article décrit comment ajouter la lecture de contenu multimédia en streaming adaptatif à une application de plateforme Windows universelle (UWP). Cette fonctionnalité prend en charge la lecture de contenu vidéo en flux continu HTTP (HLS) et de contenu à diffusion en continu dynamique sur HTTP (DASH). À partir de Windows10, version1803, le Smooth Streaming est pris en charge par **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**.

Pour obtenir la liste des balises du protocole HLS prises en charge, consultez l’article [Prise en charge des balises HLS](hls-tag-support.md). 

Pour obtenir la liste des profils DASH pris en charge, consultez l’article [Prise en charge du profil DASH](dash-profile-support.md). 

> [!NOTE] 
> Le code de cet article a été adapté à partir de l’[exemple de diffusion en continu adaptative](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) UWP.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Streaming adaptatif avec MediaPlayer et MediaPlayerElement

Pour lire du contenu multimédia en streaming adaptatif dans une application UWP, créez un objet **URI** pointant sur un fichier manifeste HLS ou DASH. Créez une instance de la classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Appelez [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) pour créer un objet **MediaSource**, puis définissez-le sur la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) de **MediaPlayer**. Appelez [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) pour démarrer la lecture du contenu multimédia.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

L’exemple ci-dessus lit l’audio du contenu multimédia, mais n’affiche pas automatiquement le contenu dans votre interface utilisateur. La plupart des applications qui lisent du contenu vidéo veulent restituer le contenu dans une page XAML.  Pour ce faire, ajoutez un contrôle [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) à votre page XAML.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Appelez [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) pour créer un objet **MediaSource** à partir de l’URI d’un fichier manifeste HLS ou DASH. Ensuite, définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) de **MediaPlayerElement**. **MediaPlayerElement** crée automatiquement un objet **MediaPlayer** pour le contenu. Vous pouvez appeler **Play** sur **MediaPlayer** pour démarrer la lecture du contenu.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> À compter de Windows10, version1607, nous vous recommandons d’utiliser la classe **MediaPlayer** pour lire des éléments multimédias. **MediaPlayerElement** est un contrôle XAML léger utilisé pour afficher le contenu d’un **MediaPlayer** dans une page XAML. Le contrôle **MediaElement** continue d’être pris en charge pour la compatibilité descendante. Pour plus d’informations sur l’utilisation de **MediaPlayer** et **MediaPlayerElement** pour lire du contenu multimédia, voir [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md). Pour plus d’informations sur l’utilisation de **MediaSource** et des API associées pour utiliser du contenu multimédia, voir [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Streaming adaptatif avec AdaptiveMediaSource

Si votre application requiert des fonctionnalités de streaming adaptatif plus avancées, telles que la fourniture d’en-têtes HTTP personnalisés, la surveillance des débits de téléchargement et de lecture en cours ou l’ajustement des ratios qui déterminent le moment où le système change les débits du flux adaptatif, utilisez l’objet **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**.

Les API de diffusion en continu adaptative figurent dans l’espace de noms [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279). Les exemples de cet article utilisent les API des espaces de noms ci-après.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>Initialiser un élément AdaptiveMediaSource à partir d’un URI

Initialisez l’élément **AdaptiveMediaSource** avec l’URI d’un fichier manifeste de diffusion en continu adaptative en appelant la méthode [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). La valeur [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) renvoyée par cette méthode vous permet de savoir si la source du contenu multimédia a été correctement créée. Si tel est le cas, vous pouvez définir l’objet en tant que source de flux pour votre **MediaPlayer** en créant un objet **MediaSource** en appelant [**MediaSource.CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource), puis en l’affectant à la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source) du lecteur multimédia. Dans cet exemple, la propriété [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) est interrogée pour déterminer le débit maximal pris en charge pour ce flux, puis cette valeur est définie en tant que débit initial. Cet exemple inscrit également les gestionnaires des différents événements **AdaptiveMediaSource** qui sont décrits plus loin dans cet article.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>Initialiser un élément AdaptiveMediaSource à l’aide d’un objet HttpClient

Si vous avez besoin de définir des en-têtes HTTP personnalisés pour l’obtention du fichier manifeste, vous pouvez créer un objet [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), définir les en-têtes souhaités, puis transmettre l’objet dans la surcharge de **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

L’événement [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) est déclenché lorsque le système est sur le point de récupérer une ressource à partir du serveur. La classe [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) transmise dans le gestionnaire d’événements expose les propriétés qui fournissent des informations sur la ressource demandée, telles que le type et l’URI de la ressource.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>Modifier les propriétés de demande de ressource à l’aide de l’événement DownloadRequested

Vous pouvez utiliser le gestionnaire d’événements **DownloadRequested** pour modifier la demande de ressource en mettant à jour les propriétés de l’objet [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) fourni par les arguments d’événement. Dans l’exemple ci-après, l’URI à partir de laquelle la ressource sera récupérée est modifié par la mise à jour des propriétés [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) de l’objet de résultat. Vous pouvez également réécrire le décalage et la longueur de la plage d’octets de la demande pour les segments multimédias ou, comme l’indique l’exemple ci-dessous, modifier l’URI de ressource pour télécharger la ressource complète et définir le décalage et la longueur de la plage d’octets sur la valeur Null.

Vous pouvez remplacer le contenu de la ressource demandée en définissant les propriétés [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) ou [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) de l’objet de résultat. Dans l’exemple ci-après, le contenu de la ressource de manifeste est remplacé par la définition de la propriété **Buffer**. Notez que si vous mettez à jour la demande de ressource avec des données obtenues en mode asynchrone, par exemple dans le cadre d’une récupération de données à partir d’un serveur distant ou d’une authentification utilisateur asynchrone, vous devez appeler [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) pour obtenir un report, puis [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) une fois l’opération terminée pour signaler au système que l’opération de demande de téléchargement peut continuer.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>Utiliser les événements de vitesse de transmission pour gérer les changements de vitesse de transmission et y répondre

L’objet **AdaptiveMediaSource** fournit des événements qui vous permettent de réagir lorsque les vitesses de transmission de téléchargement ou de lecture changent. Dans cet exemple, les débits actuels sont simplement mis à jour dans l’interface utilisateur. Notez que vous pouvez modifier les ratios qui déterminent le moment où le système change les débits du flux adaptatif. Pour plus d’informations, voir la propriété [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>Gérer les événements d’achèvement et d’échec de téléchargement
L’objet **AdaptiveMediaSource** déclenche l’événement [**DownloadFailed**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) lorsque le téléchargement d’une ressource demandée échoue. Vous pouvez utiliser cet événement pour mettre à jour votre interface utilisateur en réponse à l’échec. Vous pouvez également utiliser cet événement pour consigner des informations statistiques sur l’opération de téléchargement et sur l’échec. 

L’objet [**AdaptiveMediaSourceDownloadFailedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) transmis dans le gestionnaire d’événements contient des métadonnées sur le téléchargement de la ressource ayant échoué, comme le type de la ressource, l’URI de la ressource et la position exacte de l’échec dans le flux. L’élément [**RequestId**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) récupère un identificateur unique généré par le système pour la demande qui peut servir à mettre en corrélation les informations d’état concernant une demande spécifique entre plusieurs événements.

La propriété [**Statistics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) renvoie un objet [**AdaptiveMediaSourceDownloadStatistics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) qui fournit des informations détaillées sur le nombre d’octets reçus au moment de l’événement et sur le minutage des différents jalons de l’opération de téléchargement. Vous pouvez consigner ces informations afin d’identifier les problèmes de performances dans votre implémentation de diffusion en continu adaptative.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


L’événement [**DownloadCompleted**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) survient lorsque le téléchargement d’une ressource s’achève, et fournit des données comparables à celles de l’événement **DownloadFailed**. Là encore, un élément **RequestId** est fourni pour la mise en corrélation d’événements concernant une seule demande. En outre, un objet **AdaptiveMediaSourceDownloadStatistics** est fourni pour activer la journalisation des statistiques de téléchargement.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>Collecter les données de télémétrie de diffusion en continu adaptative avec AdaptiveMediaSourceDiagnostics
L’élément **AdaptiveMediaSource** expose une propriété [**Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource?branch=master.Diagnostics) qui renvoie un objet [**AdaptiveMediaSourceDiagnostics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics). Utilisez cet objet pour vous inscrire à l’événement [**DiagnosticAvailable**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable). Cet événement est destiné à être utilisé pour la collecte de télémétrie et ne doit pas servir à modifier le comportement de l’application au moment de l’exécution. Cet événement de diagnostic est déclenché pour de très nombreuses raisons. Pour déterminer le motif du déclenchement de l’événement, examinez la propriété [**DiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) de l’objet [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) transmis dans cet événement. Les raisons possibles englobent les erreurs d’accès à la ressource demandée et les erreurs d’analyse du fichier manifeste de diffusion en continu. Pour obtenir la liste des situations susceptibles de déclencher un événement de diagnostic, consultez l’article concernant [**AdaptiveMediaSourceDiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype). De même que les arguments d’autres événements de diffusion en continu adaptative, l’élément **AdaptiveMediaSourceDiagnosticAvailableEventArgs** fournit une propriété **RequestId** pour la mise en corrélation des informations de demande entre différents événements.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Différer la liaison de contenu de diffusion en continu adaptative pour les éléments d’une liste de lecture à l’aide de MediaBinder
La classe [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) vous permet de différer la liaison de contenu multimédia dans un objet [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955). À partir de Windows10, version1703, vous pouvez fournir un élément [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) en guise de contenu lié. Le processus de liaison différée d’une source multimédia adaptative est en grande partie identique à la liaison d’autres types d’éléments multimédias, qui est décrite dans l’article [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md). 

Créez une instance **MediaBinder**, définissez une chaîne [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) déterminée par l’application pour identifier le contenu à lier, puis inscrivez-vous à l’événement [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding). Créez un élément **MediaSource** à partir de l’objet **Binder** en appelant la méthode [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder). Ensuite, créez un élément **MediaPlaybackItem** à partir de l’objet **MediaSource** et ajoutez-le à la liste de lecture.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

Dans le gestionnaire d’événements **Binding**, utilisez la chaîne de jeton pour identifier le contenu à lier, puis créez la source multimédia adaptative en appelant l’une des surcharges de **[CreateFromStreamAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** ou de **[CreateFromUriAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)**. Étant donné qu’il s’agit de méthodes asynchrones, vous devez commencer par appeler la méthode [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) pour demander au système d’attendre la fin de l’opération avant de poursuivre.  Définissez la source multimédia adaptative en tant que contenu lié en appelant la méthode **[SetAdaptiveMediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)**. Enfin, appelez la méthode [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) une fois l’opération terminée pour demander au système de continuer.

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

Si vous voulez enregistrer les gestionnaires d’événements pour la source multimédia adaptative liée, vous pouvez effectuer cette opération dans le gestionnaire de l’événement [**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) de l’élément **MediaPlaybackList**. La propriété [**CurrentMediaPlaybackItemChangedEventArgs.NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) contient le nouvel élément **MediaPlaybackItem** en cours de lecture dans la liste. Obtenez une instance de l’objet **AdaptiveMediaSource** représentant le nouvel élément en accédant à la propriété [**Source**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) de l’objet **MediaPlaybackItem**, puis à la propriété [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) de la source multimédia. Cette propriété présentera la valeur Null si le nouvel élément de lecture n’est pas un objet **AdaptiveMediaSource**; vous devez donc vous assurer que la définition de la valeur Null est détectée avant d’essayer d’inscrire des gestionnaires pour l’un des événements de l’objet.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>Rubriquesassociées
* [Lecture de contenu multimédia](media-playback.md)
* [Prise en charge des balises HLS](hls-tag-support.md) 
* [Prise en charge du profil DASH](dash-profile-support.md) 
* [Lire du contenu audio et vidéo avec MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Lire du contenu multimédia en arrière-plan](background-audio.md) 





