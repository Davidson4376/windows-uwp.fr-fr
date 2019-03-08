---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername})
assetID: 05b2449f-3ef4-4fdf-df32-e72bcfc473d2
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinernameget.html
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aaff80d0b3ec3caa0439ec23f3deeff280f0309d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632154"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinername"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername})
Répertorie les valeurs acceptables pour le nom d’affinement de requête donnée et type d’élément de média donné. Le domaine pour ces URI est `eds.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| chaîne| Obligatoire. Une des valeurs à partir de [obtenir (/media/ {marketplaceId} / métadonnées/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
| queryrefinername| chaîne| Obligatoire. Nom de l’affinement de requête pour lequel les valeurs sont nécessaires, tels que « Genre » ou « Dix ans ». Consultez QueryRefiners.| 
  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

  
<a id="ID4EMB"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   