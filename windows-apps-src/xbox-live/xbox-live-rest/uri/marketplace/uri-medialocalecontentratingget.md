---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
description: " GET (/media/{marketplaceId}/contentRating)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d1cb9d09de8671d4cd3d61e96a8335412237e5c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641584"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
Obtenir le jeton de classification du contenu. Le domaine pour ces URI est `eds.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4ELB)
  * [Paramètres de chaîne de requête](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Appliquer des contrôles parentaux sur le contenu enfants sont autorisés à voir est une tâche compliquée. Non seulement chaque type d’élément média possède son propre système d’évaluation, mais ces systèmes de contrôle d’accès peuvent varier d’un pays à l’autre. Cela signifie qu’il n’y a plusieurs éléments différents de données qui doivent être utilisés pour filtrer correctement tous les éléments.
 
Au lieu de spécifier tous les paramètres dans tous les appels d’API, cette API génère une valeur à passer dans **combinedContentRating** paramètres dans d’autres API et continuer à communiquer les mêmes informations. Il est conçu pour faciliter les API à utiliser et à gérer, car plusieurs paramètres passés à cette API sont réduites en une valeur unique et réutilisable pour les autres API.
 
Bien que les valeurs exactes retournés par cette API peuvent changer par la suite, ils ne doivent changer très rarement (par exemple, entre les versions de Services de découverte divertissement (EDS)) et par conséquent, peut être mis en cache pendant de longues périodes de temps. Toute API acceptant un **combinedContentRating** paramètre donnera un message d’erreur explicite si la valeur passée n’est pas valide, qui est une indication que l’appelant doit simplement appeler cette API pour obtenir une valeur mise à jour. Si une API accepte un **combinedContentRating** paramètre, mais l’un n’est pas fourni, aucun filtrage de contenu s’effectuera en fonction de contrôle parental. 

> [!NOTE] 
> Cela ne signifie pas que le contenu uniquement « sécurisée » est retournée--cela signifie que tout le contenu est retourné, y compris le contenu potentiellement explicite. 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| Valeur booléenne| Facultatif. Filtre musique explicite.| 
| filterFamilyOnlyApps| Valeur booléenne| Facultatif. Filtrez les applications non marquées comme famille conviviales.| 
| filterUnrated| Valeur booléenne| Facultatif. Détermine si le contenu sans contrôle d’accès doit être inclus dans la réponse ou non.| 
| maxGameRating| entier signé 32 bits| Facultatif. Filtre les jeux.| 
| maxMovieRating| entier signé 32 bits| Facultatif. Filtre les films supérieur à un niveau spécifique.| 
| maxTVRating| entier signé 32 bits| Facultatif. Filtre TV.| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [URI de la place de marché](atoc-reference-marketplace.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   