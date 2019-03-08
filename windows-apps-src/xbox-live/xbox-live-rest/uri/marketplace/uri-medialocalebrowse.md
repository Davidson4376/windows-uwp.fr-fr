---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
description: " /media/{marketplaceId}/browse"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f692fb66580e20ffeefb3595b8cf9d795f504311
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589684"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
Permet de parcourir des éléments au sein d’un groupe de support unique. L’API de parcourir permet aux clients rechercher des éléments à partir d’un groupe de support unique. Pages de données sont accessibles à l’aide de manière non séquentielle le paramètre skipItems au lieu d’utiliser le jeton de continuation.
 
Cette API permet également la navigation dans les enfants d’un élément donné. Par exemple, en passant un ID et un paramètre MediaItemType pour un jeu Xbox 360, cela permet de navigation et diltering sur les enfants de cet élément, telles que les éléments de l’Avatar ou DLC pour le jeu.
 
Cette API accepte raffineurs de requête.
 
Certains scénarios pour la récupération des enfants :
 
   * Album aux pistes
   * Série à saisons
   * Saisons à épisodes
   * Effectuer le suivi vers la vidéo de musique
   * Artistes aux Albums
   * Jeux pour les modules complémentaires de jeu (DLC, Avatar, thèmes, etc.).
  
Le domaine pour ces URI est `eds.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[GET (media/{marketplaceId}/browse)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;Permet de parcourir des éléments au sein d’un groupe de support unique. 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la place de marché](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   