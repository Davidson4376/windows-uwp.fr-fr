---
title: InitialUploadRequest (JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
description: " InitialUploadRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2fbb2417f743d97b487ad16abd241fcb5eea62bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646634"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest (JSON)
Le corps d’un clip de jeu POST transférer les demandes. 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
L’objet InitialUploadRequest a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| chaîne| L’ID de chaîne pour le texte à utiliser comme nom de l’élément. Cela est géré et localisé dans le fichier de configuration pour le titre par le développeur du titre.| 
| <b>userCaption</b>| chaîne| Facultatif. Autre nom entré par l’utilisateur pour le jeu clip jusqu'à une longueur maximale de 250 caractères.| 
| <b>sessionRef</b>| chaîne| Facultatif. Référence de la session de jeu pendant laquelle l’enregistrement a été effectué.| 
| <b>dateRecorded</b>| DateTime| Le démarrage de l’enregistrement, au format UTC. Marshalé en tant que chaîne au format ISO 8601 (consultez <a href="https://www.w3.org/TR/NOTE-datetime">Formats de Date et heure</a> pour plus d’informations).| 
| <b>durationInSeconds</b>| entier non signé 32 bits| La longueur de l’élément en secondes.| 
| <b>expectedBlocks</b>| entier non signé 32 bits| Facultatif. Nombre de blocs dans lequel le fichier sera divisé. Omettre si le fichier sera transmis dans une demande unique.| 
| <b>fileSize</b>| entier non signé 32 bits| Taille du fichier en octets de la vidéo qui sera téléchargé.| 
| <b>type</b>| [Énumération de GameClipType](../enums/gvr-enum-gamecliptypes.md)| Le type d’image, marshalé en tant que valeur de chaîne de l’énumération qui est délimitée par des virgules.| 
| <b>source</b>| [Énumération de GameClipSource](../enums/gvr-enum-gameclipsource.md)| Spécifie la manière dont l’élément a été alimenté, marshalés en tant que valeur de chaîne de l’énumération.| 
| <b>visibility</b>| [Énumération de GameClipVisibility](../enums/gvr-enum-gameclipvisibility.md)| Spécifie la visibilité de l’élément de jeu une fois qu’elle est publiée dans le système.| 
| <b>titleData</b>| chaîne| Facultatif. Sac de propriétés pour les propriétés du titre spécifiques associés à cette image. Stockées et retournées en tant que-est. Les développeurs de titre peuvent utiliser ce champ pour conserver leurs propres métadonnées relatives à un élément.| 
| <b>titleData</b>| chaîne| Facultatif. Sac de propriétés pour les propriétés spécifiques à la console associés à cette image. Stockées et retournées en tant que-est. Plateforme de la console pouvez utiliser ce champ pour conserver leurs propres métadonnées relatives à un élément.| 
| <b>systemProperties</b>| chaîne| Facultatif. Sac de propriétés pour les propriétés spécifiques à la console associés à cette image. Stockées et renvoyée telle quelle. Plateforme de la console pouvez utiliser ce champ pour conserver leurs propres métadonnées relatives à un élément.| 
| <b>usersInSession</b>| tableau de chaînes| Facultatif. Une liste des utilisateurs dans la session active.| 
| <b>thumbnailSource</b>| [Énumération de ThumbnailSource](../enums/gvr-enum-thumbnailsource.md)| Facultatif. La source de la miniature.| 
| <b>thumbnailOffsetMillseconds</b>| entier signé 32 bits| Spécifie le décalage (en millisecondes) pour les miniatures générées décalage. Uniquement spécifié lorsque <b>thumbnailSource</b> a la valeur Offset.| 
| <b>savedByUser</b>| Valeur booléenne| Facultatif. Définit l’élément à enregistrer dans le quota de l’utilisateur plutôt que le stockage FIFO. La valeur par défaut est false.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
   "greatestMomentId": "123abc",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   