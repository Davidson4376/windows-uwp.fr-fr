---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 85a6470a64ceef3b154384d1ca859fb28733aad3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632574"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
Accède à un réseau social classement (rang).
Le domaine pour ces URI est `leaderboards.xboxlive.com`.

  * [Paramètres d’URI](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid| chaîne| Identificateur de l’utilisateur.|
| scid| chaîne| Identificateur de la configuration de service qui contient la ressource sollicitée.|
| statname| chaîne| Identificateur unique de la ressource stat utilisateur en cours d’accès.|
| tous les\|favoris| énumération| S’il faut les classer le stat valeurs (scores) pour tous les contacts connus de l’utilisateur actuel ou uniquement les contacts désignés en tant que favori personnes par cet utilisateur.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>Méthodes valides

[GET (/ users/xuid ({xuid}) /scids/ {scid} /stats/ {statname) tel {toutes les\|favori})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;Retourne un classement de réseaux sociaux en les classant que le stat valeurs (scores) pour tous les contacts connus de l’utilisateur actuel ou uniquement les contacts désignés en tant que favori personnes par cet utilisateur.

<a id="ID4EYC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent

[URI de Leaderboards](atoc-reference-leaderboard.md)
