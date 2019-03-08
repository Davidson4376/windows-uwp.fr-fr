---
title: Reward (JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
description: " Reward (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d58d34263e7e0e90091c41c1df4fd5e078f5055
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607494"
---
# <a name="reward-json"></a>Reward (JSON)
La récompense associée à la réalisation.
<a id="ID4EN"></a>


## <a name="reward"></a>Récompense

L’objet de récompense a la spécification suivante.

| Membre| Type| Description|
| --- | --- | --- |
| name| chaîne| Nom de la récompense orientées utilisateur.|
| description| chaîne| Description de la récompense orientées utilisateur.|
| value| chaîne| Valeur de la récompenses.|
| type| Énumération de RewardType| Le type de la récompense : <ul><li>non valide (0) : Un type de récompense inconnu et non pris en charge a été configuré.</li><li>Score de joueur (1) : La récompense ajoute des points au score de joueur du joueur.</li><li>dans l’application (2) : La récompense est définie et remise par le titre.</li><li>Art (3) : La récompense est un élément multimédia numérique.</li></ul> | 
| valueType| Énumération de ProgressValueDataType| Le type de valeur. Consultez [exigence (JSON)](json-requirement.md) pour plus d’informations.|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON


```json
{
  "name":null,
  "description":null,
  "value":"10",
  "type":"Gamerscore",
  "valueType":"Int"
}

```


<a id="ID4EKD"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EMD"></a>


##### <a name="parent"></a>Parent

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)
