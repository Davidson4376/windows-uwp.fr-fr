---
title: Paramètres de pagination
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
description: " Paramètres de pagination"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe80e1666f9eab8caf7e0bbdb2b537bd7661c9a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627294"
---
# <a name="paging-parameters"></a>Paramètres de pagination
 
Certains URI de Services Xbox Live retournent des collections d’objets JavaScript Objet Notation (JSON). Ces collections peuvent être contactées via en spécifiant les paramètres de pagination dans le cadre de la chaîne de requête associée à l’URI. Obtenir la liste complète des paramètres de pagination suit. Tous les URI qui autorise les paramètres de pagination sont liés au bas de cette page.
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête 
 
| Paramètre| Obligatoire| Type| Description| 
| --- | --- | --- | --- | 
| continuationToken| Non| chaîne| Retourner les éléments commençant le jeton de continuation donné. | 
| maxItems| Non| entier signé 32 bits| Nombre maximal d’éléments à retourner à partir de la collection, qui peut être combinée avec <b>skipItems</b> et <b>continuationToken</b> pour retourner une plage d’éléments. Le service peut fournir une valeur par défaut si <b>maxItems</b> n’est pas présent et peut renvoyer moins de <b>maxItems</b>, même si la dernière page de résultats n’a pas encore été retournée. | 
| skipItems| Non| entier signé 32 bits| Retourne des éléments après le nombre donné d’éléments. Par exemple, <b>skipItems = « 3 »</b> récupérera les éléments commençant par le quatrième élément récupéré. | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>Référence [GET (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   