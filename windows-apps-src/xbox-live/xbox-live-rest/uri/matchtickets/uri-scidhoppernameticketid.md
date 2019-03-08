---
title: /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9722d06af64c5fd24f53485a1bcfe765f89b08cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627314"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}

Prend en charge une opération de suppression d’un ticket de correspondance.

> [!IMPORTANT]
> Cet URI est prévu pour une utilisation avec contrat 103 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 103 ou version ultérieure sur chaque demande.

<a id="ID4ER"></a>


## <a name="domain"></a>domaine.
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Notes
Cet URI prend en charge le valeurs xuid, gt et me pour l’identificateur de propriétaire dans la configuration de l’utilisateur cible.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID) pour la session.|
| name| chaîne| Le nom de la hopper.|
| ticketId| GUID| L’ID de ticket.|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>Méthodes valides

[Supprimer (/serviceconfigs/ {scid} /hoppers/ {hoppername} /tickets/ {ticketid})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;Supprime un ticket de correspondance.

<a id="ID4ETC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EVC"></a>


##### <a name="parent"></a>Parent  

[URI de Matchmaking](atoc-reference-matchtickets.md)
