---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04376505b76296e052ea431f2a4e5fcfeac7b9e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613934"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

Prend en charge une opération GET pour récupérer des statistiques pour une hopper.

> [!IMPORTANT]
> Cet URI est prévu pour une utilisation avec contrat 103 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 103 ou version ultérieure sur chaque demande.

<a id="ID4ER"></a>


## <a name="domain"></a>domaine.
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Notes
Cet URI prend en charge le valeurs xuid, gt et me pour l’identificateur de propriétaire dans la configuration de l’utilisateur cible. Seul l’auteur d’un ticket peut supprimer le ticket ou récupérer l’état de l’URI.  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID) pour la session.|
| name| chaîne| Le nom de la hopper.|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>Méthodes valides

[OBTENIR (/hoppers/ /serviceconfigs/ {scid} {name} / statistiques)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;Obtient les statistiques pour une hopper.

<a id="ID4EQC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ESC"></a>


##### <a name="parent"></a>Parent  

[URI de Matchmaking](atoc-reference-matchtickets.md)
