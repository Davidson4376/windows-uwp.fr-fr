---
title: /titles/{titleId}/sessions/{sessionId}/allocationStatus
assetID: 55611f4b-4ba4-fa9a-ce44-fcc4a6df1b35
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus.html
description: " /titles/{titleId}/sessions/{sessionId}/allocationStatus"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aa66b81d1e363488a6969ab31d7091b695f09ae4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603304"
---
# <a name="titlestitleidsessionssessionidallocationstatus"></a>/titles/{titleId}/sessions/{sessionId}/allocationStatus
Pour l’id de titre donné et un id de session, obtenir l’état de la demande de ticket. Les domaines pour ces URI sont `gameserverds.xboxlive.com` et `gameserverms.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EU)
  * [Nom d’hôte](#ID4EPB)
  * [Méthodes valides](#ID4EWB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Description| 
| --- | --- | 
| titleId| ID du titre qui doit traiter la demande.| 
| sessionId| l’ID de la session à rechercher.| 
  
<a id="ID4EPB"></a>

 
## <a name="host-name"></a>Nom d'hôte
 
gameserverms.xboxlive.com
  
<a id="ID4EWB"></a>

 
## <a name="valid-methods"></a>Méthodes valides
  
[GET](uri-titlestitleidsessionssessionidallocationstatus-get.md)
 
&nbsp;&nbsp;Retourne l’état d’allocation de la sessionhost identifié par son ID de session.
   