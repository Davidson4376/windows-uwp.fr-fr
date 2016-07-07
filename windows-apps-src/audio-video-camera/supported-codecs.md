---
author: drewbatgit
ms.assetid: 9347AD7C-3A90-4073-BFF4-9E8237398343
description: "Cet article répertorie la prise en charge des codecs et formats audio et vidéo pour les applications UWP."
title: Codecs pris en charge
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 1481f8bbb2c53f667d253e23af1422297e19c63d

---

# Codecs pris en charge

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Cet article répertorie la prise en charge des codecs et formats audio et vidéo pour les applications UWP.

Dans les tableaux ci-dessous, la lettre « D » indique la prise en charge de décodeurs et la lettre « E », la prise en charge d’encodeurs.

## Prise en charge des codecs et formats audio

Les tableaux suivants indiquent la prise en charge des codecs et formats audio pour chaque famille d’appareils.

**Remarque**  
-   Lorsque la prise en charge AMR-NB est indiquée, le codec n’est pas pris en charge sur les références serveur.

 

### Bureau

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
<td align="left">HE-AAC v2 / eAAC+</td>
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

 

### Mobile

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

 

### IoT Core (x86)

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
<td align="left">HE-AAC v2 / eAAC+</td>
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

 

### IoT Core (ARM)

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
<td align="left">HE-AAC v2 / eAAC+</td>
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

 

### Xbox

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

 

## Prise en charge des codecs et formats vidéo

Les tableaux suivants indiquent la prise en charge des codecs et formats vidéo pour chaque famille d’appareils.

**Remarque**  
-   Lorsque la prise en charge H.265 est indiquée, elle n’est pas forcément assurée par tous les appareils de la famille d’appareils.
-   Lorsque la prise en charge MPEG-2/MPEG-1 est indiquée, elle est uniquement assurée avec l’installation de l’application Windows universelle de DVD Microsoft en option.

 

### Bureau

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

 

### Mobile

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

 

### IoT Core (x86)

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

 

### IoT (ARM)

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

 

### Xbox

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

 

 

 







<!--HONumber=Jun16_HO4-->


