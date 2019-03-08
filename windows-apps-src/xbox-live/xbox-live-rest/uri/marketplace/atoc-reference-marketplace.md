---
title: URI du Marketplace
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
description: " URI du Marketplace"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9fd8112c6e16b3e9d9fb70c34381e88ba5aa6273
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593874"
---
# <a name="marketplace-uris"></a>URI du Marketplace

Cette section fournit des détails sur les adresses de l’identificateur URI (Universal Resource) et de méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour *place de marché* des services, également appelé divertissement Services de découverte (EDS).

Uniquement les jeux en cours d’exécution sur une Xbox 360, sur un appareil Windows Phone ou sur Xbox.com peuvent utiliser ce service.

Les domaines pour ces URI sont eds.xboxlive.com et inventory.xboxlive.com.

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/users/me/inventory](uri-inventory.md)

&nbsp;&nbsp;Accède à l’ensemble de l’inventaire actuellement associé à l’utilisateur spécifié.

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;Accède à l’ensemble complet des détails d’un élément spécifique de stock consommable.

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;Accède à l’ensemble complet des détails d’un élément d’inventaire spécifique.

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;Éléments accède à partir de plusieurs groupes différents supports.

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;Permet de parcourir des éléments au sein d’un groupe de support unique.

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;Accéder au jeton de classification du contenu.

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;Retourne les détails et les métadonnées d’offre sur un ou plusieurs éléments.

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;Accède au jeton de champs.

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;Répertorie tous les mediaGroups pris en charge pour la version d’EDS donnée.

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;Accède à la mediaItemTypes disponibles par groupe de support pour la version donnée d’EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;Champs d’accès à partir de laquelle on peut s’attendre à des données, pour un mediaitemtype donné et une version donnée d’EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;Accède aux raffineurs de requête pour le type d’élément de média donné.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;Accède aux valeurs acceptables pour le nom d’affinement de requête donnée et type d’élément de média donné.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;Accéder à la liste des valeurs secondaire pour une valeur d’affinement de requête donnée (par exemple, « subgenres d’un genre donné »).

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;Accède à toutes les mediaItemTypes pris en charge pour la version d’EDS donnée.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;Accès disponibles trier des commandes pour un type donné mediaitem et une version donnée d’EDS.

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;Permet de rechercher des éléments dans un groupe de support unique.

<a id="ID4EFD"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EHD"></a>


##### <a name="parent"></a>Parent

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>Informations supplémentaires

[Référence supplémentaire](../../additional/atoc-xboxlivews-reference-additional.md)
