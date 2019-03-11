---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
description: " GET (/users/xuid({xuid})/history/titles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cadbf38385bbc321ef5bf23eb93c3fbc5c1a2417
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655584"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
Obtient une liste de titres pour lequel l’utilisateur a déverrouillé ou a progressé sur ses réalisations. Cette API ne retourne pas l’historique complet de l’utilisateur des titres lu ou lancé. Le domaine pour ces URI est `achievements.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EY)
  * [Paramètres de chaîne de requête](#ID4EDB)
  * [Autorisation](#ID4EFD)
  * [En-têtes de demande facultatif](#ID4EGE)
  * [Corps de la demande](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur dont l’historique de titre est accédée.| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Obligatoire| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| Non| entier signé 32 bits| Retourne des éléments après le nombre donné d’éléments. Par exemple, <b>skipItems = « 3 »</b> récupérera les éléments commençant par le quatrième élément récupéré. | 
| continuationToken| Non| chaîne| Retourner les éléments commençant le jeton de continuation donné. | 
| maxItems| Non| entier signé 32 bits| Nombre maximal d’éléments à retourner à partir de la collection, qui peut être combinée avec <b>skipItems</b> et <b>continuationToken</b> pour retourner une plage d’éléments. Le service peut fournir une valeur par défaut si <b>maxItems</b> n’est pas présent et peut renvoyer moins de <b>maxItems</b>, même si la dernière page de résultats n’a pas encore été retournée. | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>Authorization
 
| Revendication| Obligatoire ?| Description| Comportement si manquant| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Utilisateur| L’appelant est un utilisateur autorisé de Xbox LIVE.| L’appelant doit être un utilisateur valide sur Xbox LIVE.| 403 Forbidden| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc.| 
| <b>x-xbl-contract-version</b>| entier non signé 32 bits| S’il est présent et défini sur 2, la version V2 de cette API est utilisée. Sinon, V1.| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/history/titles](uri-titlehistoryusersxuidhistorytitlesv2.md)

  
<a id="ID4EPG"></a>

 
##### <a name="reference"></a>Référence 

[UserTitle (JSON)](../../json/json-usertitlev2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Paramètres de pagination](../../additional/pagingparameters.md)

   