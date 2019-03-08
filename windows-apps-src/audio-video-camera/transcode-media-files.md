---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Vous pouvez utiliser les API Windows.Media.Transcoding pour transcoder des fichiers vidéo d’un format vers un autre.
title: Transcoder des fichiers multimédias
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a6eb19ca5954b3ce71ecbaefe3339bee78f8717
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616764"
---
# <a name="transcode-media-files"></a>Transcoder des fichiers multimédias



Vous pouvez utiliser les API [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) pour transcoder des fichiers vidéo d’un format vers un autre.

Le *transcodage* consiste à convertir un fichier multimédia numérique, tel qu’un fichier vidéo ou audio, d’un format vers un autre. Lors de ce processus, le fichier est généralement décodé puis réencodé. Par exemple, vous pouvez souhaiter convertir un fichier Windows Media au format MP4 pour qu’il puisse être lu sur un appareil mobile qui prend en charge le format MP4. Ou encore, vous pouvez souhaiter convertir un fichier vidéo à haute définition en fichier de résolution inférieure. Dans ce cas, le fichier réencodé peut utiliser le même codec que le fichier d’origine, mais il aura un profil d’encodage différent.

## <a name="set-up-your-project-for-transcoding"></a>Configurer votre projet pour le transcodage

Outre les espaces de noms référencés par le modèle de projet par défaut, vous devrez référencer ces espaces de noms pour transcoder les fichiers multimédias en utilisant le code dans cet article.

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Sélectionner les fichiers source et de destination

La façon dont votre application détermine les fichiers source et de destination pour le transcodage dépend de votre implémentation. Cet exemple utilise un élément [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)et un élément [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) pour permettre à l’utilisateur de choisir un fichier source et de destination.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Créer un profil d’encodage multimédia

Le profil d’encodage contient les paramètres qui déterminent le mode d’encodage du fichier de destination. C’est à ce stade du processus de transcodage d’un fichier que les options proposées sont les plus nombreuses.

La classe [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) fournit des méthodes statiques pour la création de profils d’encodage prédéfinis :

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Méthodes de création de profils d’encodage exclusif audio

Méthode  |Profil  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Audio Apple Lossless Audio Codec (ALAC)         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Audio Free Lossless Audio Codec (FLAC).         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |Audio AAC (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |Audio MP3         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |Audio WAV         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Audio Windows Media (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Méthodes de création de profils d’encodage audio/vidéo

Méthode  |Profil  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |Vidéo High Efficiency Video Coding (HEVC), également appelée vidéo H.265 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |Vidéo MP4 (vidéo H.264 plus audio AAC) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Vidéo Windows Media (WMV) |


Le code suivant crée un profil pour la vidéo MP4.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

La méthode statique [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) permet de créer un profil d’encodage MP4. Le paramètre de cette méthode détermine la résolution cible de la vidéo. Dans ce cas, [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) signifie 1 280 x 720 pixels à 30 images par seconde. (« 720p » correspond à 720 lignes de balayage progressif par image.) Les autres méthodes pour la création de profils prédéfinis tous les suivent ce modèle.

Une autre possibilité consiste à créer un profil qui correspond à un fichier multimédia existant à l’aide de la méthode [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047). Ou bien, si vous avez une idée précise des paramètres d’encodage que vous voulez utiliser, vous pouvez créer un objet [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) et compléter les détails du profil.

## <a name="transcode-the-file"></a>Transcoder le fichier

Pour transcoder le fichier, créez un objet [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) et appelez la méthode [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936). Transmettez le fichier source, le fichier de destination et le profil d’encodage. Appelez ensuite la méthode [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) sur l’objet [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) qui a été renvoyé à partir de l’opération de transcodage asynchrone.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Répondre à la progression du transcodage

Vous pouvez enregistrer des événements pour répondre en cas de modification de la progression de l’élément [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) asynchrone. Ces événements font partie de l’infrastructure de programmation asynchrone pour les applications de plateforme Windows universelle (UWP) et ne sont pas propres à l’API de transcodage.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>Encoder un flux de métadonnées
À partir de Windows 10, version 1803, vous pouvez inclure des métadonnées chronométrées lorsque des fichiers multimédias transcodage. Contrairement à la vidéo transcodage exemples ci-dessus, qui utilisent l’encodage de média intégré Profiler des méthodes de création, comme [ **MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), vous devez créer manuellement l’encodage de métadonnées profil pour prendre en charge le type de métadonnées que vous encodez.

Cette première étape de création d’un profil d’incoding de métadonnées consiste à créer un [**TimedMetadataEncodingProperties**] objet qui décrit l’encodage des métadonnées à être transcodé. La propriété sous-type est un GUID qui spécifie le type des métadonnées. Les détails d’encodage pour chaque type de métadonnées est propriétaires et n’est pas fournie par Windows. Dans cet exemple, le GUID pour les métadonnées de GoPro (gprs) est utilisé. Ensuite, [ **SetFormatUserData** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) est appelée pour définir un blob binaire de données décrivant le format de flux de données qui est spécifique au format de métadonnées. Ensuite, un **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) est créé à partir des propriétés de l’encodage, et une étiquette de piste et le nom sont pour autoriser une application de lecture du flux endcoded identifier le flux de métadonnées et éventuellement afficher le nom de flux de données dans l’interface utilisateur. 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

Après avoir créé le **TimedMetadataStreamDescriptor**, vous pouvez créer un **MediaEncodingProfile** qui décrit la vidéo, audio et les métadonnées à encoder dans le fichier. Le **TimedMetadataStreamDescriptor** créé dans le dernier exemple est passé dans cet exemple de fonction d’assistance et est ajouté à la **MediaEncodingProfile** en appelant [  **SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks).

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




