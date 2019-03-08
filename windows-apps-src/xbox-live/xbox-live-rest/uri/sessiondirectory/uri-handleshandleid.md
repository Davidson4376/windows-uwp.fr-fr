---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
description: " /handles/{handleId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a312d3744e96755a899d73307a47c01e3dc79fd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632634"
---
# <a name="handleshandleid"></a>/handles/{handleId}
Prend en charge les opérations DELETE et GET pour les descripteurs de session spécifiés par l’identificateur. 

> [!NOTE] 
> Cet URI est utilisé en mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>domaine.
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | 
| handleId| GUID| ID unique de la poignée pour la session.| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;Supprime les handles spécifiés par l’ID de handle.

[GET (/handles/ {id de handle})](uri-handleshandleidget.md)

&nbsp;&nbsp;Récupère les handles spécifiés par l’ID de handle.
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Parent 

[URI du répertoire de session](atoc-reference-sessiondirectory.md)

   