---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders)
assetID: 225e8cb2-44eb-6b7b-eaa0-1ea2d2602f6f
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypesortordersget.html
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5cec9bf585e1d885c4c1b6950e94923908cc06e8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618354"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypesortorders"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders)
Répertorie les ordres de tri disponibles pour un type donné mediaitem et une version donnée d’EDS. Le domaine pour ces URI est `eds.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| chaîne| Obligatoire. Une des valeurs à partir de [obtenir (/media/ {marketplaceId} / métadonnées/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

  
<a id="ID4EMB"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   