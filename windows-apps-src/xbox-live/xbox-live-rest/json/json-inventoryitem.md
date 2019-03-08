---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
description: " inventoryItem (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 240e527258923cff146009810c190e401e0574d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628494"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
L’article en stock core représente l’élément standard sur lequel un droit peut être accordé.
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

L’objet inventoryItem a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| url| chaîne| Identificateur unique de cet article en stock spécifique.|
| itemType| chaîne| Type de l’élément. Valeurs actuelles sont <ul><li><b>Inconnu</b></li><li><b>Jeu</b></li><li><b>Movie</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>Poster</b></li><li><b>Podcast</b></li><li><b>Image</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>Thème</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>Album</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>Bundle</b></li><li><b>XnaCommunityGame</b></li><li><b>Promotional</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>App</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>Abonnement</b><br/><br/> **Remarque :** Jeux sont désignées par **GameV2**, sont consommables **GameConsumable**, et est durable DLC **GameContent**. |
  | Conteneurs | chaîne | Il s’agit de l’ensemble des « conteneurs » qui contiennent cet élément. L’inventaire d’un utilisateur peut être interrogée pour les éléments qui appartiennent à un conteneur spécifique. Ces conteneurs sont déterminées lors de l’élément est ajouté à l’inventaire par fournisseur. |
  | obtenu | DateTime | Date et heure de que l’élément a été ajouté à l’inventaire de l’utilisateur. |
  | startDate | DateTime | Date et heure de l’élément est devenu ou devient disponible pour utilisation. |
  | endDate | DateTime | Date et heure de l’élément est devenu ou deviennent inutilisables. |
  | État | chaîne | L’état de l’élément. Valeurs autorisées sont **activé**, **Suspended**, **expiré**, **Canceled**, **renouvelé**.  |
  | trial | Valeur booléenne | Obligatoire. True si ce droit est une version d’évaluation ; Sinon, false. Si vous achetez la version d’évaluation d’un droit et ensuite achetez la version complète, vous recevrez les deux. |
  | trialTimeRemaining | TimeSpan | Nullable. Combien de temps il reste sur la version d’évaluation, en quelques minutes. |
  | Consommables | tableau | Si les éléments est consommable, contient une représentation inline de l’identificateur unique (lien) pour l’article en stock consommable, ainsi que sa quantité actuelle. |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
inventoryItem {
  "url": string,
  "itemType": "Music" | "Video" | "Game" | "AvatarItem" | "Subscription" | "DLC" | "Consumable" | ...,
  "obtained": DateTime,
  "beginDate": DateTime,
  "endDate": DateTime,
  "state": "Unavailable" | "Available" | "Suspended" | "Expired",
  "trial": true,
  "trialTimeRemaining":"23:12:14",
  ("consumable": {"url": string, "quantity": int})
}

```


<a id="ID4EVAAC"></a>


## <a name="consumable-inventory-item"></a>Élément d’inventaire consommables

L’entité consommable présente l’ensemble minimal de propriétés pour un élément consommable.

| Membre| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| url| chaîne| Identificateur unique de l’article en stock consommable spécifique.|
| quantity| entier signé 32 bits| La quantité actuelle de cet article en stock.|


```json
consumableInventoryItem {
  "url": string,
  "quantity": int
}

```


<a id="ID4E4BAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E6BAC"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>Référence

[/users/me/inventory](../uri/marketplace/uri-inventory.md)

 [/inventory/consumables/{itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)
