---
title: Référence de serveur de jeu Universal Resource identificateur (URI)
assetID: bbd7e3f3-77ac-6ffd-8951-fe4b8b48eb4c
permalink: en-us/docs/xboxlive/rest/atoc-gsdk-uri-reference.html
description: " Référence de serveur de jeu Universal Resource identificateur (URI)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a9a0a38cff9214485b2d7e8b1f8a28acb3207444
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662594"
---
# <a name="game-server-universal-resource-identifier-uri-reference"></a>Référence de serveur de jeu Universal Resource identificateur (URI)
URI utilisé par les clients pour créer des instances de serveur de Kit de développement de serveur de jeu pour un titre. Les domaines pour ces URI sont `gameserverds.xboxlive.com` et `gameserverms.xboxlive.com`.
 
<a id="ID4EY"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/qosservers](uri-qosservers.md)

&nbsp;&nbsp;URI appelée par un client pour obtenir la liste des serveurs de qualité de service disponibles pour une utilisation avec Xbox Live Compute.

[/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

&nbsp;&nbsp;URI qui permet à un client créer une instance de serveur Xbox Live Compute pour un titre.

[/titles/{titleId}/variants](uri-titlestitleidvariants.md)

&nbsp;&nbsp;URI appelée par un client pour obtenir les variantes disponibles pour un titre.

[/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

&nbsp;&nbsp;Demande un sessionhost Xbox Live Compute à allouer pour un id de titre donné.

[/titles/{titleId}/sessions/{sessionId}/allocationStatus](uri-titlestitleidsessionssessionidallocationstatus.md)

&nbsp;&nbsp;Pour l’id de titre donné et un id de session, obtenir l’état de la demande de ticket.
 