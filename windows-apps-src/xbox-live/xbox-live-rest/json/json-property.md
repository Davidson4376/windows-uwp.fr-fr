---
title: Property (JSON)
assetID: 93de547e-d936-6fcc-92cb-e4dd284dd609
permalink: en-us/docs/xboxlive/rest/json-property.html
description: " Property (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7e2a721886509c49c60d663d491f8d49bc3c95e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659704"
---
# <a name="property-json"></a>Property (JSON)
Contient des données de propriété fournies par le client pour les critères de requête matchmaking.
<a id="ID4EN"></a>


## <a name="property"></a>Propriété

L’objet de propriété a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| id| chaîne| Un id pour cette propriété.|
| type| entier signé 32 bits | Type de la propriété. Valeurs prises en charge sont : <ul><li>0 = entier</li><li>1 = chaîne</li></ul>| 
| value| chaîne| Valeur de cette propriété.|

<a id="ID4EGC"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
    "id":"1",
    "value":"20",
    "type":0
}

```


<a id="ID4EPC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ERC"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)
