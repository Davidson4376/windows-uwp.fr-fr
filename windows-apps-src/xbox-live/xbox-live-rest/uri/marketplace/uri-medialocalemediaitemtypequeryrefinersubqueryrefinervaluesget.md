---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
assetID: 0fcbef77-4607-765e-72e1-d2e7620e2c61
permalink: en-us/docs/xboxlive/rest/uri-medialocalemediaitemtypequeryrefinersubqueryrefinervaluesget.html
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 183d15a306d0f15fcb5f9f7fad8411772f763c13
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661814"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinersubqueryrefinervalues"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
Obtenir la liste des valeurs secondaire pour une valeur d’affinement de requête donnée (par exemple, « subgenres d’un genre donné »). Le domaine pour ces URI est `eds.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EDB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
La valeur d’affinement de requête est passée comme paramètre de chaîne de requête nommé **queryRefinerValue**, ce qui est fait pour autoriser des valeurs d’affinement de requête avec des caractères interdits dans les ressources URI à passer.
 
Cette API est uniquement pris en charge pour la musique.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EOB"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQB"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

  
<a id="ID4E1B"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   