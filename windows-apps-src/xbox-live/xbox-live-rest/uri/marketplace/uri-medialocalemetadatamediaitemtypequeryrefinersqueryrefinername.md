---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
assetID: 1e2f5fd3-3d14-9a97-ffbf-ab2a18ff4f15
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 356c7d6eb30eb14156e434f717530d6f4f48a990
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611474"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinername"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
Accède aux valeurs acceptables pour le nom d’affinement de requête donnée et type d’élément de média donné. Le domaine pour ces URI est `eds.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| chaîne| Obligatoire. Une des valeurs à partir de [obtenir (/media/ {marketplaceId} / métadonnées/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
| queryrefinername| chaîne| Obligatoire. Nom de l’affinement de requête pour lequel les valeurs sont nécessaires, tels que « Genre » ou « Dix ans ». Consultez QueryRefiners.| 
  
<a id="ID4EKC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername})](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinernameget.md)

&nbsp;&nbsp;Répertorie les valeurs acceptables pour le nom d’affinement de requête donnée et type d’élément de média donné.
 
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la place de marché](atoc-reference-marketplace.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   