---
title: GET (/public/scids/{scid}/clips)
assetID: 15b3e873-1f96-b1da-2f79-6dac1369a4c0
permalink: en-us/docs/xboxlive/rest/uri-publicscidclipsget.html
description: " GET (/public/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5bce1dd261e0ad1172068a0287519cd0480da85f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589804"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
Liste des éléments publics. Le domaine pour cet URI est `gameclipsmetadata.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4ECB)
  * [Paramètres de chaîne de requête](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Cette API permet de différentes manières d’éléments de liste sont publics. La liste d’éléments est retournée en sur les vérifications de la confidentialité et isolation du contenu par rapport à la demande XUID.
 
Les requêtes sont optimisées par identificateur de configuration de service (SCID). Spécifiant autres filtres ou autres que les valeurs par défaut répertoriés ci-dessous, les ordres de tri peut dans certains cas prendre plus de temps à retourner. Il s’agit plus évidente pour les jeux plus volumineux de vidéos. Les requêtes ne peut pas spécifier un ordre de tri croissant.
 
Le qualificateur est nécessaire pour obtenir aux clips ofpublic regroupement spécifique. L’utilisateur demandeur doit avoir accès à la SCID demandé, sinon HTTP 403 sera retourné.
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| chaîne| Identificateur de la configuration de service principal des éléments publics.| 
| titleid| chaîne| N ° titre des éléments publics. Ne peut pas être spécifié dans le même URI que le SCID. Si spécifié, sera utilisé pour rechercher le SCID principal.| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| <b>?achievementId={achievementId}</b>| Clips les plus récents de correspondance spécifié <b>achievementId</b>.| Tri/filtrage supplémentaire n’est pas pris en charge.| 
| <b>?greatestMomentId={greatestMomentId}</b>| Clips les plus récents de correspondance spécifié <b>greatestMomentId</b>.| Tri/filtrage supplémentaire n’est pas pris en charge.| 
| <b>?qualifier=created </b>| Plus récente| Obligatoire.| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Parent 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   