---
title: /handles/{handleId}/session
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
description: " /handles/{handleId}/session"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7b6990917437c22dd4d9282492e2a0eab37893b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628144"
---
# <a name="handleshandleidsession"></a>/handles/{handleId}/session
Prend en charge les opérations GET et PUT pour une session, à l’aide de la poignée de déréférencement. 

> [!NOTE] 
> Cet URI est utilisé en mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure.  

 

> [!NOTE] 
> Cet URI est actuellement en externe accessible uniquement par les consoles Xbox One et serveurs à l’aide d’un identificateur de service.  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>domaine.
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| ID unique de la poignée pour la session.| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/handles/{handleId}/session)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;Obtient un objet de session pour l’identificateur de handle spécifié. 

[PLACER (/handles/ {handle-id} / session)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;Crée ou met à jour d’une session par le déréférencement d’un handle.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[URI du répertoire de session](atoc-reference-sessiondirectory.md)

   