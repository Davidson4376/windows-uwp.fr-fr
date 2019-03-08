---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
description: " HopperStatsResults (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 38e345fc20e92cdf6446c6ae1100e347fe634eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646204"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
Un objet JSON qui représente les statistiques pour une hopper. 
<a id="ID4EN"></a>

  
 
L’objet JSON de HopperStatsResults a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| hopperName| chaîne| Le nom de la hopper sélectionné.| 
| waitTime| entier signé 32 bits| Nombre moyen de temps pour le hopper (un nombre entier de secondes) de mise en correspondance. | 
| Remplissage| entier signé 32 bits| Le nombre de personnes en attente pour les correspondances dans le hopper.| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON 
 

```json
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }
  
    
```

  
<a id="ID4EGB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EIB"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>Référence 

[OBTENIR (/hoppers/ /serviceconfigs/ {scid} {name} / statistiques)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   