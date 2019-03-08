---
title: URI de présence
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
description: " URI de présence"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a46ecd48c2b0bf523ab234a5f20cf9ed6669e75
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632604"
---
# <a name="presence-uris"></a>URI de présence
 
Cette section fournit des détails sur les adresses de l’identificateur URI (Universal Resource) et les méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour *présence*.
 
Uniquement les jeux en cours d’exécution sur une Xbox 360, sur un appareil Windows Phone ou sur Windows peuvent utiliser ce service.
 
Le domaine pour ces URI est userpresence.xboxlive.com.
 
Vous pouvez vous abonner aux changements de présence d’un utilisateur à l’aide du service de l’activité en temps réel (RTA).
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;Présence d’accès pour un lot d’utilisateurs.

[/users/me](uri-usersme.md)

&nbsp;&nbsp;Accéder à la présence d’utilisateur actuel.

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;Accède à la PresenceRecord pour mon groupe.

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;Accéder à la présence d’un autre utilisateur ou un client.

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;Accéder à la présence d’un titre ou un utilisateur d’un titre.

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;Accède à la PresenceRecord pour un groupe.

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;Accède à l’enregistrement de la présence des utilisateurs de diffusion spécifiés par le moniker de groupes liés à la XUID qui s’affiche dans l’URI.

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;Accède au nombre d’utilisateurs diffusion spécifiés par le moniker de groupes liés à la XUID qui s’affiche dans l’URI.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   