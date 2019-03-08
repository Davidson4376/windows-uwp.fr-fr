---
title: /users/xuid({xuid})/history/titles
assetID: 2f8eb79a-42c2-0267-cbf2-8682bb28f270
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesv2.html
description: " /users/xuid({xuid})/history/titles"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7bb528f139e9db97f886f57fb98461ce15e77a42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645684"
---
# <a name="usersxuidxuidhistorytitles"></a>/users/xuid({xuid})/history/titles
 
Cet identificateur URI (Universal Resource) fournit l’accès à un historique de l’utilisateur liées à la réalisation de titre.
 
Le domaine pour ces URI est `achievements.xboxlive.com`.
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur dont l’historique de titre est accédée.| 
  
<a id="ID4EAC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET](uri-titlehistoryusersxuidhistorytitlesgetv2.md)

&nbsp;&nbsp;Obtient une liste de titres pour lequel l’utilisateur a déverrouillé ou a progressé sur ses réalisations. Cette API ne retourne pas l’historique complet de l’utilisateur des titres lu ou lancé.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Parent 

[URI de l’historique de réalisation titre](atoc-reference-titlehistoryv2.md)

   