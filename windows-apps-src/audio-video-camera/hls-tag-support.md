---
author: drewbatgit
ms.assetid: 
description: "Cet article répertorie les balises du protocole HTTP Live Streaming (HLS) prises en charge pour les applications UWP."
title: Prise en charge des balises HTTP Live Streaming (HLS)
translationtype: Human Translation
ms.sourcegitcommit: 61feba54418ad7ba8f089737ebf52d12c466b7a7
ms.openlocfilehash: 5575a56838eae98c0d79dff61e1afaa8cf87ee86

---

# <a name="http-live-streaming-hls-tag-support"></a>Prise en charge des balises HTTP Live Streaming (HLS)
Le tableau suivant répertorie les balises HLS prises en charge pour les applications UWP.

> [!NOTE] 
> Les balises personnalisées qui démarrent avec le préfixe « X » sont accessibles en tant que métadonnées synchronisées, tel que décrit dans l’article [Éléments, playlists et pistes multimédias](media-playback-with-mediasource.md).

|Balise |Introduite dans la version du protocole HLS|Version brouillon du document du protocole HLS|Requis sur le client|Version de juillet de Windows 10|Windows 10, version 1511|Windows 10, version 1606 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  Balises de base                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|OBLIGATOIRE|Prise en charge|Pris en charge|Prise en charge|
| 4.3.1.2.  EXT-X-VERSION |2|3|OBLIGATOIRE|Prise en charge|Pris en charge|Prise en charge
|4.3.2.  Balises de segments multimédias                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|OBLIGATOIRE|Prise en charge|Pris en charge|Prise en charge
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|FACULTATIVE|Prise en charge|Pris en charge|Prise en charge|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|FACULTATIVE|Prise en charge|Pris en charge|Prise en charge|
| 4.3.2.4.  EXT-X-KEY |1|0|FACULTATIVE|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp; MÉTHODE|1|0|Attribut|« NONE, AES-128 »|« NONE, AES-128 »|« NONE, AES-128, SAMPLE-AES »|
|&nbsp;&nbsp;&nbsp; URI|1|0|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp; IV|2|3|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
| 4.3.2.5.  EXT-X-MAP |5|9|FACULTATIVE|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp; URI|5|9|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|FACULTATIVE|Non prise en charge|Non prise en charge|Non prise en charge|
|4.3.3.  Balises de sélection multimédia                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|OBLIGATOIRE|Prise en charge|Pris en charge|Prise en charge|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|FACULTATIVE|Prise en charge|Pris en charge|Prise en charge|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|FACULTATIVE|Non prise en charge|Non prise en charge|Non prise en charge|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|FACULTATIVE|Prise en charge|Pris en charge|Prise en charge|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|FACULTATIVE|Prise en charge|Pris en charge|Prise en charge|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|FACULTATIVE|Non prise en charge|Non prise en charge|Non prise en charge|
|4.3.4.  Balises de sélection principale                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|FACULTATIVE|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  TYPE|4|7|Attribut|« AUDIO, VIDEO »|« AUDIO, VIDEO »|« AUDIO, VIDEO, SUBTITLES »|
|&nbsp;&nbsp;&nbsp;  URI|4|7|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  GROUP-ID|4|7|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  LANGUAGE|4|7|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp;  NAME|4|7|Attribut|Non prise en charge|Non prise en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  PAR DÉFAUT|4|7|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp;  AUTOSELECT|4|7|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp;  FORCED|5|9|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp;  CARACTÉRISTIQUES|5|9|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|OBLIGATOIRE|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  BANDWIDTH|1|0|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  PROGRAM-ID|1|0|Attribut|N/A|N/A|N/A|
|&nbsp;&nbsp;&nbsp;  AVERAGE-BANDWIDTH|7|14|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  RESOLUTION|2|3|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  FRAME-RATE|7|15|Attribut|N/A|N/A|N/A|
|&nbsp;&nbsp;&nbsp;  AUDIO|4|7|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  VIDEO|4|7|Attribut|Prise en charge|Pris en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  SUBTITLES|5|9|Attribut|Non prise en charge|Non prise en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  CLOSED-CAPTIONS|6|12|Attribut|Non prise en charge|Non prise en charge|Non prise en charge|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|FACULTATIVE|Non prise en charge|Non prise en charge|Non prise en charge|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|FACULTATIVE|Non prise en charge|Non prise en charge|Non prise en charge|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|FACULTATIVE|Pas de prise en charge|Pas de prise en charge|Pas de prise en charge|
|4.3.5.  Balises de playlist principale ou de média                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|FACULTATIVE|Pas de prise en charge|Prise en charge|Prise en charge|
| 4.3.5.2.  EXT-X-START  |6|12|FACULTATIVE|Pas de prise en charge|Prise en charge partielle|Prise en charge partielle|
|&nbsp;&nbsp;&nbsp;  TIME-OFFSET|6|12|Attribut|Pas de prise en charge|Prise en charge|Prise en charge|
|&nbsp;&nbsp;&nbsp;  PRECISE|6|12|Attribut|Pas de prise en charge|Valeur « NON » par défaut prise en charge|Valeur « NON » par défaut prise en charge|



## <a name="related-topics"></a>Rubriques connexes

* [Lecture de contenu multimédia](media-playback.md)
* [Diffusion en continu adaptative](adaptive-streaming.md)
 

 







<!--HONumber=Dec16_HO1-->


