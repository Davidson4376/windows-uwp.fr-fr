---
title: /serviceconfigs/{scid}/hoppers/{hoppername}
assetID: ba1e129d-b4c4-6535-46ce-fd184465c85f
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppername.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1db069f59eeba565a257531907d6be0b1dcd189d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598424"
---
# <a name="serviceconfigsscidhoppershoppername"></a>/serviceconfigs/{scid}/hoppers/{hoppername}

Prend en charge une opération POST pour créer des tickets de correspondance.

> [!IMPORTANT]
> Cet URI est prévu pour une utilisation avec contrat 103 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 103 ou version ultérieure sur chaque demande.

<a id="ID4ER"></a>


## <a name="domain"></a>domaine.
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID) pour la session.|
| hoppername | chaîne | Le nom de la hopper. |

<a id="ID4E2B"></a>


## <a name="valid-methods"></a>Méthodes valides

[POST (/hoppers/ /serviceconfigs/ {scid} {hoppername})](uri-serviceconfigsscidhoppershoppernamepost.md)

&nbsp;&nbsp;Crée le ticket de correspondance spécifiées.

<a id="ID4EFC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EHC"></a>


##### <a name="parent"></a>Parent  

[URI de Matchmaking](atoc-reference-matchtickets.md)
