---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "Cet article décrit comment doter une application de plateforme Windows universelle (UWP) d’une fonctionnalité de lecture de contenu multimédia à diffusion en continu adaptative. Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en flux continu HTTP (HLS) et de contenu à diffusion en continu dynamique sur HTTP (DASH)."
title: Diffusion en continu adaptative
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8ebf90b02fcfbb4349ba2b303d9c91727b731ad7

---

# Diffusion en continu adaptative

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cet article décrit comment doter une application de plateforme Windows universelle (UWP) d’une fonctionnalité de lecture de contenu multimédia à diffusion en continu adaptative. Cette fonctionnalité prend actuellement en charge la lecture de contenu vidéo en flux continu HTTP (HLS) et de contenu à diffusion en continu dynamique sur HTTP (DASH).

## Diffusion en continu adaptative simple avec MediaElement

Pour afficher un contenu multimédia à diffusion en continu adaptative dans une application XAML, ajoutez un contrôle [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) à votre page.

[!code-xml[MediaElementXAML](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml#SnippetMediaElementXAML)]

Définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) de l’élément **MediaElement** sur l’URI d’un fichier manifeste DASH ou HLS.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetManifestSource)]

## Diffusion en continu adaptative avec AdaptiveMediaSource

Si votre application requiert des fonctionnalités de diffusion en continu adaptative plus avancées, telles que la fourniture d’en-têtes HTTP personnalisés, la surveillance des débits de téléchargement et de lecture en cours ou l’ajustement des ratios qui déterminent le moment où le système change les débits du flux adaptatif, utilisez l’objet [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

Les API de diffusion en continu adaptative figurent dans l’espace de noms [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279).

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

Initialisez l’élément **AdaptiveMediaSource** avec l’URI d’un fichier manifeste de diffusion en continu adaptative en appelant la méthode [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). La valeur [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) renvoyée par cette méthode vous permet de savoir si la source du contenu multimédia a été correctement créée. Si tel est le cas, vous pouvez définir l’objet comme source de flux pour votre élément **MediaElement** en appelant la méthode [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029). Dans cet exemple, la propriété [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) est interrogée pour déterminer le débit maximal pris en charge pour ce flux, puis cette valeur est définie en tant que débit initial. Cet exemple inscrit également les gestionnaires pour les événements [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) et [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) qui sont décrits plus loin dans cet article.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

Si vous avez besoin de définir des en-têtes HTTP personnalisés pour l’obtention du fichier manifeste, vous pouvez créer un objet [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), définir les en-têtes souhaités, puis transmettre l’objet dans la surcharge de **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

L’événement [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) se déclenche lorsque le système est sur le point de récupérer une ressource à partir du serveur. La classe [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) transmise dans le gestionnaire d’événements expose les propriétés qui fournissent des informations sur la ressource demandée, telles que le type et l’URI de la ressource.

Vous pouvez utiliser le gestionnaire d’événements **DownloadRequested** pour modifier la demande de ressource en mettant à jour les propriétés de l’objet [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) fourni par les arguments d’événement. Dans l’exemple ci-après, l’URI à partir de laquelle la ressource sera récupérée est modifié par la mise à jour des propriétés [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) de l’objet de résultat.

Vous pouvez remplacer le contenu de la ressource demandée en définissant les propriétés [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) ou [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) de l’objet de résultat. Dans l’exemple ci-après, le contenu de la ressource de manifeste est remplacé par la définition de la propriété **Buffer**. Notez que si vous mettez à jour la demande de ressource avec des données obtenues en mode asynchrone, par exemple dans le cadre d’une récupération de données à partir d’un serveur distant ou d’une authentification utilisateur asynchrone, vous devez appeler [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) pour obtenir un report, puis [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) une fois l’opération terminée pour signaler au système que l’opération de demande de téléchargement peut continuer.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

L’objet **AdaptiveMediaSource** fournit des événements qui vous permettent de réagir lorsque les débits de téléchargement ou de lecture changent. Dans cet exemple, les débits actuels sont simplement mis à jour dans l’interface utilisateur. Notez que vous pouvez modifier les ratios qui déterminent le moment où le système change les débits du flux adaptatif. Pour plus d’informations, voir la propriété [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

 

 







<!--HONumber=Jun16_HO4-->


