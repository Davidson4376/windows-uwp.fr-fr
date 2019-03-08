---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
description: " /media/{marketplaceId}/details"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58e5247c3fd52e84a3a9bab28c6926f74e864e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655704"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
Retourne les détails et les métadonnées d’offre sur un ou plusieurs éléments. Le domaine pour ces URI est `eds.xboxlive.com`.
 
Les détails de l’API diffère de l’API connexes et l’API Parcourir (lorsque passin dans un ID) comme ces API retournent des informations sur les autres éléments qui sont associés à l’ID fiven, explicitement ou implicitement, tandis que le détails de l’API retourne des informations supplémentaires sur le même élément.
 
Plusieurs ID des types d’éléments multimédias différents peuvent être passés dans un seul appel (à condition qu’ils ne sont pas de type ProviderContentID - voir ci-dessous), mais elles doivent appartenir au même groupe de support. Toutefois, il existe deux scénarios client où l’appelant ne sait pas le groupe de support. L’API prend en charge en permettant à une valeur sepcial « Inconnu » pour le groupe de support dans les situations suivantes :
 
   * type d’ID = XboxHexTitle, qui retourne les éléments de type d’application ou type de partie
   * type d’ID = ProviderContentId, qui retourne les éléments MovieType ou TVType
  
Le graphique suivant résume le mappage d’ensemble de quel ID de types peuvent être fournis avec les groupes de support :
 
| Type d’ID| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| Inconnu| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| canonique| Y| Y| Y| Y| Y| Y| Y| N| 
| ZuneCatalog| N| N| Y| Y| Y| Y| N| N| 
| ZuneMediaInstance| N| N| Y| N| Y| Y| N| N| 
| Offre| Y| Y| Y| N| Y| Y| N| N| 
| AMG| N| N| N| N| Y| N| N| N| 
| MediaNet| N| N| N| N| Y| N| N| N| 
| XboxHexTitle| Y| Y| N| N| N| N| N| Y| 
| ProviderContentId| N| N| Y| N| N| Y| N| Y| 
 
  * [Notes de paramètre](#ID4EEH)
  * [Paramètres d’URI](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>Notes de paramètre
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
Il est utilisé pour le fournisseur de recherche ID spécifique. Exemple Id de Netflix ou Hulu Id.
 
Lorsque le type d’ID est ProviderContentId, une seule valeur est acceptée. ProviderContentIds étant le seul type d’ID qui peut contenir le '.' caractères. Étant donné que le '.' caractère est également le séparateur que nous utilisons entre ID, il existe une ambiguïté entre ce qui est un delimieter entre ID et ce qui fait partie de l’ID lui-même. Le reste de l’API fonctionne de la même façon pour ProviderContentIds, à l’exception de la fonctionnalité de recherche en bloc.
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| chaîne| Obligatoire. Chaîne de valeur obtenue à partir de la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[OBTENIR (/media/ {marketplaceId} / détails)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;Retourne les détails et les métadonnées d’offre sur un ou plusieurs éléments. 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>Parent 

[URI de la place de marché](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>Informations supplémentaires 

[En-têtes communs EDS](../../additional/edscommonheaders.md)

 [Paramètres d’EDS](../../additional/edsparameters.md)

 [EDS interroger raffineurs](../../additional/edsqueryrefiners.md)

 [Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)

   