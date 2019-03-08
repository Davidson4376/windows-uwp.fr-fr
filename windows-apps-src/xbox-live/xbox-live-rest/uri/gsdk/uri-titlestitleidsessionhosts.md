---
title: /titles/{titleId}/sessionhosts
assetID: 92d9bdd2-5c8f-761b-3f9a-50f8db7b843c
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts.html
description: " /titles/{titleId}/sessionhosts"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c0a97d0c87f9204371daeaa825d6636ef6b8409c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658324"
---
# <a name="titlestitleidsessionhosts"></a>/titles/{titleId}/sessionhosts
Demande un sessionhost Xbox Live Compute à allouer pour un id de titre donné. Les domaines pour ces URI sont `gameserverds.xboxlive.com` et `gameserverms.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EU)
  * [Nom d’hôte](#ID4EIB)
  * [Méthodes valides](#ID4EPB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Description| 
| --- | --- | 
| titleId| ID du titre qui doit traiter la demande.| 
  
<a id="ID4EIB"></a>

 
## <a name="host-name"></a>Nom d'hôte
 
gameserverms.xboxlive.com
  
<a id="ID4EPB"></a>

 
## <a name="valid-methods"></a>Méthodes valides
  
[POST](uri-titlestitleidsessionhosts-post.md)
 
&nbsp;&nbsp;Créer la nouvelle demande de cluster.
   