---
title: Recherche inversée EDS pour la vidéo
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
description: " Recherche inversée EDS pour la vidéo"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d535dec8a95eba4d486bfc6946e187e2da66ae49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598434"
---
# <a name="eds-reverse-lookup-for-video"></a>Recherche inversée EDS pour la vidéo
 
  * [Étapes de recherche inversées](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>Étapes de recherche inversées
 
La recherche inversée divertissement Services de découverte (EDS) est pris en charge pour tous les types de médias vidéo (**MediaItemType.Movie**, **MediaItemType.TVSeries**, **MediaItemType.TVEpisode**, **MediaItemType.TVSeason**, et **MediaItemType.TVShow**), ainsi que **MediaItemType.Unknown**.
 
Recherche inversée requiert 4 paramètres à passer : 
   * `idType=ScopedMediaId`
   * `ids=` l’ID de média du fournisseur
   * `ScopeIdType=Title`
   * `ScopeId=` l’ID de titre du fournisseur
 
 
La recherche inversée nécessite généralement 2 étapes : 
   * Récupérer l’id du média fournisseur (par exemple, à partir d’un appel de détails) s’il n’est pas disponible. 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * Émettre l’appel de recherche inversée à l’aide de la **ProviderMediaId** champ à partir de la réponse précédente : 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
Si le **ProviderMediaId** champ n’a pas été récupéré à partir d’EDS, puis le champ doit être codée URL devant être transmis correctement au EDS.
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[URI de la place de marché](../uri/marketplace/atoc-reference-marketplace.md)

   