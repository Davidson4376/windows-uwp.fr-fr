---
author: drewbatgit
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: "Recherchez les encodeurs et décodeurs audio et vidéo installés sur un appareil."
title: "Rechercher les codecs installés"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows10, uwp, codec, encodeur, décodeur, requête"
ms.openlocfilehash: 1bde2c24db6faa843d9118f330867e7f66b22e7e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="query-for-installed-codecs"></a>Rechercher les codecs installés
Cet article vous montre comment rechercher les encodeurs et décodeurs audio et vidéo qui sont installés sur un appareil. L’article [Codecs pris en charge](supported-codecs.md) répertorie les codecs qui sont inclus dans le système d’exploitation de chaque famille d’appareils. Sur un appareil donné, l’utilisateur ou une application peut installer des codecs supplémentaires. La classe [CodecQuery](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery) permet d’obtenir la liste des encodeurs et décodeurs qui sont installés sur l’appareil sur lequel votre application s’exécute.

La classe **CodecQuery** est incluse dans l’espace de noms [windows.media.core](https://docs.microsoft.com/en-us/uwp/api/windows.media.core) et fournit un constructeur qui ne prend aucun argument.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

La méthode [FindAllAsync](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery#Windows_Media_Core_CodecQuery_FindAllAsync_Windows_Media_Core_CodecKind_Windows_Media_Core_CodecCategory_System_String_) de la classe **CodecQuery** retourne tous les codecs installés qui répondent aux paramètres spécifiés. Ces paramètres incluent une valeur de l’énumération [CodecKind](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeckind) qui spécifie si des codecs audio ou vidéo doivent être retournés, ainsi qu’une valeur [CodecCategory](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeccategory) qui spécifie si des codecs audio ou vidéo doivent être retournés.

Le paramètre final est une chaîne représentant un sous-type de média, par exemple vidéoH264 ou audio FLAC, qui doit être pris en charge par un codec retourné par **FindAllAsync**. Vous pouvez spécifier une chaîne Null ou vide pour ce paramètre afin de retourner les codecs de tous les sous-types de média. L’exemple suivant répertorie tous les encodeurs vidéo installés sur l’appareil actuel.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

Si vous spécifiez un sous-type de média, vous devez utiliser la représentation de chaîne de l’un des GUID de sous-type indiqués dans [GUID de sous-type audio](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) ou [GUID de sous-type vidéo](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx). La classe [CodecSubtypes](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecsubtypes) contient les propriétés de la plupart des sous-types de média pris en charge qui retournent la représentation de chaîne du GUID de sous-type. Vous pouvez également spécifier un code FOURCC pour le paramètre de sous-type de média. Pour plus d’informations, voir [Codes FOURCC](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx). 

L’exemple suivant utilise le code FOURCC pour rechercher les décodeurs vidéo qui prennent en charge la vidéoH.264. Un exemple de scénario pour cette opération consiste à vérifier si un format vidéo est pris en charge avant de tenter de lire un fichier vidéo.

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

L’exemple suivant recherche les encodeurs audio qui prennent en charge le sous-type de média FLAC. Cet exemple crée ensuite un objet AudioEncodinAudioEncodingProperties et un élément MediaEncodingProfile pour le sous-type de média. Vous pouvez l’utiliser pour enregistrer des données audio au format spécifié ou pour transcoder des données audio à partir d’un autre format. Pour plus d’informations, voir [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) et [Transcoder des fichiers multimédias](transcode-media-files.md).

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Rubriques connexes

* [Codecs pris en charge](supported-codecs.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcoder des fichiers multimédias](transcode-media-files.md)
 

 




