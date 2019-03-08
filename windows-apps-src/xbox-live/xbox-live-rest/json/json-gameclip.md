---
title: GameClip (JSON)
assetID: 204cb702-4ce4-85a8-f231-3b4fb243405f
permalink: en-us/docs/xboxlive/rest/json-gameclip.html
description: " GameClip (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4cfc2ac0a635e4aacdc9eeefb5097c6bd946a518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646504"
---
# <a name="gameclip-json"></a>GameClip (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclip"></a>GameClip
 
L’objet clip de jeu a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| <b>gameClipId</b>| chaîne| L’ID affecté à l’élément de jeu.| 
| <b>state</b>| GameClipState| L’état de l’élément de jeu dans le système.| 
| <b>dateRecorded</b>| DateTime| Date et heure auxquelles l’enregistrement a débuté, au format UTC (format ISO 8601).| 
| <b>lastModified</b>| DateTime| Dernière heure de modification de l’élément de jeu ou ses métadonnées, au format UTC (format ISO 8601).| 
| <b>userCaption</b>| chaîne| Entré par l’utilisateur non localisé chaîne pour l’élément de jeu.| 
| <b>type</b>| GameClipTypes| Le type d’élément. Peut être plusieurs valeurs et sera par des virgules si c’est le cas.| 
| <b>source</b>| GameClipSource| Comment l’élément a été alimenté.| 
| <b>visibility</b>| GameClipVisibility| La visibilité de l’élément de jeu une fois qu’elle est publiée dans le système.| 
| <b>durationInSeconds</b>| entier non signé 32 bits| La durée de l’élément de jeu en secondes.| 
| <b>scid</b>| chaîne| SCID auquel le clip de jeu est associé.| 
| <b>Contrôle d’accès</b>| nombre à virgule flottante double précision| Le contrôle d’accès associé à l’élément de jeu, dans la plage 0.0 à 5.0.| 
| <b>ratingCount</b>| entier non signé 32 bits| Le nombre de fois où que cet élément a été évalué.| 
| <b>Affichage</b>| entier non signé 32 bits| Le nombre de vues associées à l’élément de jeu.| 
| <b>titleData</b>| chaîne| Conteneur de propriétés du titre spécifique.| 
| <b>titleData</b>| chaîne| Le jeu de propriétés de la console spécifique.| 
| <b>thumbnails</b>| tableau de GameClipThumbnail| Tableau d’objets de GameClipThumbnail.| 
| <b>gameClipUris</b>| tableau de GameClipUri| Tableau d’objets de GameClipUri.| 
| <b>xuid</b>| chaîne| XUID du propriétaire de l’élément de jeu, marshalé en tant que chaîne.| 
| <b>clipName</b>| chaîne| La version localisée du nom de l’élément, selon les paramètres régionaux d’entrée de la demande est recherché à partir du système de gestion de titre.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   