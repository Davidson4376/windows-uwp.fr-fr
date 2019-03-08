---
title: BatchRequest (JSON)
assetID: 2ca34506-8801-efc5-7c83-3c9ec5572276
permalink: en-us/docs/xboxlive/rest/json-batchrequest.html
description: " BatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51073c71700613edcd7c22e18cc0c00a9222d7e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654154"
---
# <a name="batchrequest-json"></a>BatchRequest (JSON)
Un tableau de propriétés permettant de filtrer les informations de présence, tels que les utilisateurs, appareils et des titres.
<a id="ID4EN"></a>


## <a name="batchrequest"></a>BatchRequest

L’objet BatchRequest a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| utilisateurs| tableau de chaînes| XUIDs liste d’utilisateurs dont vous souhaitez en savoir plus, avec un maximum de XUIDs 1100 à la fois la présence.|
| deviceTypes| tableau de chaînes| Liste des types de périphérique utilisés par les utilisateurs que vous souhaitez connaître. Si le tableau est vide, par défaut à tous les types possibles de l’appareil (autrement dit, aucun sont filtrées).|
| titres| tableau d’entier non signé 32 bits| Liste des appareils dont les utilisateurs que vous voulez savoir sur les types. Si le tableau est vide, par défaut pour tous les titres possibles (autrement dit, aucun sont filtrées).|
| level| chaîne| Valeurs possibles : <ul><li>utilisateur - get utilisateur nœuds</li><li>APPAREIL - utilisateur de get et les nœuds de l’appareil</li><li>titre - obtenir les informations au niveau de titre de base</li><li>All : obtenir des informations de présence riche, des informations sur le média ou les deux</li></ul>La valeur par défaut est « title ».| 
| onlineOnly| Valeur booléenne| Si cette propriété est true, l’opération de traitement par lots filtre les enregistrements pour les utilisateurs en mode hors connexion (y compris ceux qui sont masqués). S’il n’est pas fourni, les utilisateurs en ligne et hors connexion seront affichera.|

<a id="ID4EAD"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}


```


<a id="ID4EJD"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ELD"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>Référence   
