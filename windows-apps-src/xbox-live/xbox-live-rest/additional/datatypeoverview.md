---
title: Vue d’ensemble des types de données
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
description: " Vue d’ensemble des types de données"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 62932a921d51a988a5533d7ee08f4968bb67a29d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659654"
---
# <a name="data-type-overview"></a>Vue d’ensemble des types de données
 
Les Services Xbox Live utilise une variété de types de données liées à l’identité et d’authentification. Cette rubrique fournit une vue d’ensemble de ces types.
 
| Type| Description| 
| --- | --- | 
| Gamertag| Un nom de l’écran unique, explicite pour l’utilisateur.| 
| Lecteur| Un objet JSON contenant XUID et gamertag, ainsi que les index du joueur dans la session (ou « siège »), si le joueur participe toujours à la session et un blob de petite taille des données personnalisées de l’utilisateur.| 
| profile| Informations sur l’utilisateur accessibles via des adresses d’URI de profil et les méthodes HTTP, généralement l’utilisateur UserSettings, mais également éventuellement notamment gamercard, gamertag, XUID et ainsi de suite.| 
| Paramètre| Un des paramètres spécifiques à titre d’un objet UserSettings.| 
| Userclaims.| Un objet JSON simple contenant XUID et identité de l’utilisateur.| 
| UserSettings| Un objet JSON contenant une collection de paramètres spécifiques au titre ou des préférences de l’utilisateur authentifié actuel. UserSettings peut contenir des données arbitraires, éventuellement liées à l’activité de dans le jeu.| 
| XUID| L’ID d’utilisateur Xbox utilisateur, un entier long non signé unique. Pas destinés à être contrôlable de visu.| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>Référence [lecteur (JSON)](../json/json-player.md)

 [UserClaims (JSON)](../json/json-userclaims.md)

 [UserSettings (JSON)](../json/json-usersettings.md)

   