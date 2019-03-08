---
title: RichPresenceRequest (JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
description: " RichPresenceRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c49da63ecd091a886a68f508af09e33fb9c58ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654124"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest (JSON)
Demande d’informations sur les informations de présence riche doivent être utilisées. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
L’objet RichPresenceRequest a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| id| chaîne| Le <b>friendlyName</b> de la chaîne de présence riche à utiliser.| 
| scid| chaîne| Scid qui nous indique où les chaînes de présence riche sont définies.| 
| params| tableau de chaînes| Tableau de <b>friendlyName</b> avec lequel terminer la chaîne de présence riche des chaînes. Doivent être spécifiés uniquement les noms conviviaux d’énumération, pas de statistiques. Si vous laissez ce vide supprime toute valeur précédente.| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

   