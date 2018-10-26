---
author: drewbatgit
ms.assetid: 9347AD7C-3A90-4073-BFF4-9E8237398343
description: Cet article répertorie la prise en charge des codecs et formats audio et vidéo pour les applications UWP.
title: Codecs pris en charge
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 57a604b1b3996019bcf6e39bc88c9a59a74cb51c
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562834"
---
# <a name="supported-codecs"></a>Codecs pris en charge

Cet article répertorie les codecs audio, vidéo et d’image ainsi que la disponibilité par défaut des formats d’applications UWP, pour chaque famille d’appareils. Notez que ces tableaux répertorient les codecs inclus avec l’installation Windows10 pour la famille considérée d’appareils. Les utilisateurs et les applications peuvent installer les codecs supplémentaires mis à disposition, le cas échéant. Durant l’exécution, vous pouvez identifier le jeu de codecs disponible pour un appareil spécifique. Pour plus d’informations, voir [Rechercher les codecs installés sur un appareil](codec-query.md).

Dans les tableaux ci-dessous, la lettre «D» indique la prise en charge de décodeurs et la lettre «E» la prise en charge d’encodeurs.

## <a name="audio-codec--format-support"></a>Prise en charge des codecs et formats audio

Les tableaux suivants indiquent la prise en charge des codecs et formats audio pour chaque famille d’appareils.

> [!NOTE] 
> Lorsque la prise en charge AMR-NB est indiquée, le codec n’est pas pris en charge sur les références serveur.

 

### <a name="desktop"></a>Bureau

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-3</th>
<th align="left">MPEG-2</th>
<th align="left">ADTS</th>
<th align="left">ASF</th>
<th align="left">RIFF</th>
<th align="left">AVI</th>
<th align="left">AC-3</th>
<th align="left">AMR</th>
<th align="left">3GP</th>
<th align="left">FLAC</th>
<th align="left">WAV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">HE-AAC v1 / AAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">HE-AAC v2 / eAAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AAC-LC</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">AC3</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">EAC3 / EC3</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">ALAC</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AMR-NB</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">FLAC</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">G.711 (A-Law, µ-law)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">GSM 6.10</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="odd">
<td align="left">IMA ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">LPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">MP3</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-1/2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MS ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">WMA 1/2/3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMA Pro</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMA Voice</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="mobile"></a>Mobile

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-3</th>
<th align="left">MPEG-2</th>
<th align="left">ADTS</th>
<th align="left">ASF</th>
<th align="left">RIFF</th>
<th align="left">AVI</th>
<th align="left">AC-3</th>
<th align="left">AMR</th>
<th align="left">3GP</th>
<th align="left">FLAC</th>
<th align="left">WAV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">HE-AAC v1 / AAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">HE-AAC v2 / eAAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AAC-LC</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">AC3</td>
<td align="left">D, uniquement sur Lumia Icon, 830, 930, 1520</td>
<td align="left"></td>
<td align="left">D, uniquement sur Lumia Icon, 830, 930, 1520</td>
<td align="left"></td>
<td align="left">D, uniquement sur Lumia Icon, 830, 930, 1520</td>
<td align="left">D, uniquement sur Lumia Icon, 830, 930, 1520</td>
<td align="left">D, uniquement sur Lumia Icon, 830, 930, 1520</td>
<td align="left">D, uniquement sur Lumia Icon, 830, 930, 1520</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">EAC3 / EC3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">ALAC</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AMR-NB</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">FLAC</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">G.711 (A-Law, µ-law)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">GSM 6.10</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="odd">
<td align="left">IMA ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">LPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">MP3</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-1/2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MS ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">WMA 1/2/3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMA Pro</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMA Voice</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="iot-core-x86"></a>IoT Core (x86)

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-3</th>
<th align="left">MPEG-2</th>
<th align="left">ADTS</th>
<th align="left">ASF</th>
<th align="left">RIFF</th>
<th align="left">AVI</th>
<th align="left">AC-3</th>
<th align="left">AMR</th>
<th align="left">3GP</th>
<th align="left">FLAC</th>
<th align="left">WAV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">HE-AAC v1 / AAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">HE-AAC v2 / eAAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AAC-LC</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">AC3</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">EAC3 / EC3</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">ALAC</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AMR-NB</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">FLAC</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">G.711 (A-Law, µ-law)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">GSM 6.10</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="odd">
<td align="left">IMA ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">LPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">MP3</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-1/2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MS ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">WMA 1/2/3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMA Pro</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMA Voice</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="iot-core-arm"></a>IoT Core (ARM)

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-3</th>
<th align="left">MPEG-2</th>
<th align="left">ADTS</th>
<th align="left">ASF</th>
<th align="left">RIFF</th>
<th align="left">AVI</th>
<th align="left">AC-3</th>
<th align="left">AMR</th>
<th align="left">3GP</th>
<th align="left">FLAC</th>
<th align="left">WAV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">HE-AAC v1 / AAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">HE-AAC v2 / eAAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AAC-LC</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">AC3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">EAC3 / EC3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">ALAC</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AMR-NB</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">FLAC</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">G.711 (A-Law, µ-law)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">GSM 6.10</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="odd">
<td align="left">IMA ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">LPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">MP3</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-1/2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MS ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">WMA 1/2/3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMA Pro</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMA Voice</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="xbox"></a>Xbox

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-3</th>
<th align="left">MPEG-2</th>
<th align="left">ADTS</th>
<th align="left">ASF</th>
<th align="left">RIFF</th>
<th align="left">AVI</th>
<th align="left">AC-3</th>
<th align="left">AMR</th>
<th align="left">3GP</th>
<th align="left">FLAC</th>
<th align="left">WAV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">HE-AAC v1 / AAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">HE-AAC v2 / eAAC+</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AAC-LC</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">AC3</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">EAC3 / EC3</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">ALAC</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">AMR-NB</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">FLAC</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">G.711 (A-Law, µ-law)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">GSM 6.10</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="odd">
<td align="left">IMA ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">LPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">MP3</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-1/2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MS ADPCM</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">WMA 1/2/3</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMA Pro</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMA Voice</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

## <a name="video-codec--format-support"></a>Prise en charge des codecs et formats vidéo

Les tableaux suivants indiquent la prise en charge des codecs et formats vidéo pour chaque famille d’appareils.

> [!NOTE] 
> Lorsque la prise en charge H.265 est indiquée, elle n’est pas forcément assurée par tous les appareils de la famille d’appareils.
> Lorsque la prise en charge MPEG-2/MPEG-1 est indiquée, elle est uniquement assurée avec l’installation de l’application Windows universelle de DVD Microsoft en option.

 

### <a name="desktop"></a>Bureau

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">FOURCC</th>
<th align="left">fMP4</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-2 PS</th>
<th align="left">MPEG-2 TS</th>
<th align="left">MPEG-1</th>
<th align="left">3GPP</th>
<th align="left">3GPP2</th>
<th align="left">AVCHD</th>
<th align="left">ASF</th>
<th align="left">AVI</th>
<th align="left">MKV</th>
<th align="left">DV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">MPEG-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MPEG-4 (Part 2)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.265</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">H.264</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.263</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">VC-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMV7/8/9</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMV9 Screen</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">DV</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left">D</td>
</tr>
<tr class="odd">
<td align="left">Motion JPEG</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="mobile"></a>Mobile

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">FOURCC</th>
<th align="left">fMP4</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-2 PS</th>
<th align="left">MPEG-2 TS</th>
<th align="left">MPEG-1</th>
<th align="left">3GPP</th>
<th align="left">3GPP2</th>
<th align="left">AVCHD</th>
<th align="left">ASF</th>
<th align="left">AVI</th>
<th align="left">MKV</th>
<th align="left">DV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">MPEG-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MPEG-4 (Part 2)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.265</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">H.264</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.263</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">VC-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMV7/8/9</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMV9 Screen</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">DV</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Motion JPEG</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="iot-core-x86"></a>IoT Core (x86)

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">FOURCC</th>
<th align="left">fMP4</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-2 PS</th>
<th align="left">MPEG-2 TS</th>
<th align="left">MPEG-1</th>
<th align="left">3GPP</th>
<th align="left">3GPP2</th>
<th align="left">AVCHD</th>
<th align="left">ASF</th>
<th align="left">AVI</th>
<th align="left">MKV</th>
<th align="left">DV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">MPEG-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MPEG-4 (Part 2)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.265</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">H.264</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.263</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">VC-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMV7/8/9</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMV9 Screen</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">DV</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Motion JPEG</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="iot-arm"></a>IoT (ARM)

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">FOURCC</th>
<th align="left">fMP4</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-2 PS</th>
<th align="left">MPEG-2 TS</th>
<th align="left">MPEG-1</th>
<th align="left">3GPP</th>
<th align="left">3GPP2</th>
<th align="left">AVCHD</th>
<th align="left">ASF</th>
<th align="left">AVI</th>
<th align="left">MKV</th>
<th align="left">DV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">MPEG-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MPEG-4 (Part 2)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.265</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">H.264</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.263</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">VC-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMV7/8/9</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMV9 Screen</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">DV</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Motion JPEG</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

### <a name="xbox"></a>Xbox

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec/conteneur</th>
<th align="left">FOURCC</th>
<th align="left">fMP4</th>
<th align="left">MPEG-4</th>
<th align="left">MPEG-2 PS</th>
<th align="left">MPEG-2 TS</th>
<th align="left">MPEG-1</th>
<th align="left">3GPP</th>
<th align="left">3GPP2</th>
<th align="left">AVCHD</th>
<th align="left">ASF</th>
<th align="left">AVI</th>
<th align="left">MKV</th>
<th align="left">DV</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">MPEG-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">MPEG-2</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">MPEG-4 (Part 2)</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.265</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">H.264</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">H.263</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D/E</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">VC-1</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left">D</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">WMV7/8/9</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">WMV9 Screen</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">DV</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Motion JPEG</td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left">D</td>
<td align="left"></td>
<td align="left"></td>
</tr>
</tbody>
</table>

## <a name="image-codec--format-support"></a>Prise en charge des codecs et formats d’image 

<table>
<colgroup>
<col width="7%" />
<col width="7%" />
<col width="7%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Codec</th>
<th align="left">Ordinateur de bureau</th>
<th align="left">Autres familles d’appareils</th>
</tr>
</thead>
<tr class="odd">
<td align="left">BMP</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
</tr>
<tr class="even">
<td align="left">DDS</td>
<td align="left">D/E<sup>1</sup></td>
<td align="left">D/E<sup>1</sup></td>
</tr>
<tr class="odd">
<td align="left">DNG</td>
<td align="left">D<sup>2</sup></td>
<td align="left">D<sup>2</sup></td>
</tr>
<tr class="even">
<td align="left">GIF</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">ICO</td>
<td align="left">D</td>
<td align="left">D</td>
</tr>
<tr class="even">
<td align="left">JPEG</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">JPEG-XR</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
</tr>
<tr class="even">
<td align="left">PNG</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
</tr>
<tr class="odd">
<td align="left">TIFF</td>
<td align="left">D/E</td>
<td align="left">D/E</td>
</tr>
<tr class="even">
<td align="left">RAW (appareil photo)</td>
<td align="left">D<sup>3</sup></td>
<td align="left">Non</td>
</tr>
</table>

<sup>1</sup> Les images DDS utilisant BC1 par le biais de la compression BC5 sont prises en charge.  
<sup>2</sup> Les images DNG avec un aperçu incorporé non RAW sont prises en charge.  
<sup>3</sup> Seuls certains formats RAW d’appareil photo sont pris en charge.  

Pour plus d’informations sur les codecs d’image, voir [Codecs WIC natifs](https://msdn.microsoft.com/library/windows/desktop/gg430027.aspx).