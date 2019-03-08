---
title: URI de réputation
assetID: d1cb76c0-86a4-8c51-19f6-5f223b517d46
permalink: en-us/docs/xboxlive/rest/atoc-reference-reputation.html
description: " URI de réputation"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93d6d6e6acfd8fa39bd9d26c87ed99362d2c88d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614004"
---
# <a name="reputation-uris"></a>URI de réputation
 
Cette section fournit des détails sur les adresses de l’identificateur URI (Universal Resource) et de méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour le **Microsoft.Xbox.Services.Social.ReputationService** . Le domaine de la réputation URI est reputation.xboxlive.com. Une représentation sous forme de URI classique peut être https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback. 
 
Le service de réputation utilise des commentaires, comme décrit dans [commentaires (JSON)](../../json/json-feedback.md), pour calculer un score de réputation. Ce score est enregistré dans la zone de statistiques pour l’utilisateur sous la clé ReputationOverall. Pour plus d’informations sur la récupération des statistiques de l’utilisateur, consultez [obtenir (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md). 
 
Jeux sur toutes les plateformes peuvent utiliser le service de réputation.
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;Utilisée à partir de votre titre si vous souhaitez ajouter une option de commentaires dans votre jeu, par opposition à l’aide de l’interpréteur de commandes.

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;Utilisé par service de votre titre d’envoyer des commentaires sous forme de lot en dehors de l’interface de votre titre.

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;Permet à l’équipe de mise en œuvre accéder aux scores de réputation de l’utilisateur actuel.

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;Réinitialise entièrement les données de réputation d’un utilisateur de test. Fins de test uniquement.

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;Permet à l’équipe de mise en œuvre accéder aux scores de réputation de l’utilisateur spécifié.
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   