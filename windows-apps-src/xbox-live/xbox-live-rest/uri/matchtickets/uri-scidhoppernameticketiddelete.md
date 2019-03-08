---
title: DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
description: " DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fdd28cb94b31102d9af98aa95afde45424dadce9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589664"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})

Supprime un ticket de correspondance.

> [!IMPORTANT]
> Cette méthode est conçue pour une utilisation avec contrat 103 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 103 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4E2)
  * [Autorisation](#ID4EGB)
  * [Codes d’état HTTP](#ID4EOC)
  * [Corps de la demande](#ID4EXC)
  * [Corps de la réponse](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST supprime l’ID du ticket spécifié le hopper nommé à la configuration du service au niveau du code (SCID). Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID) pour la session.|
| name| chaîne| Le nom de la hopper.|
| ticketId| GUID| L’ID de ticket.|

<a id="ID4EGB"></a>


## <a name="authorization"></a>Authorization

| Type| Obligatoire| Description| Cas d’absence de réponse|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (ID utilisateur)| oui| L’utilisateur qui effectue la requête doit être un membre de la session de ticket référencé par le ticket.| 403|
| Privilèges et le Type d’appareil| oui| Lorsque le type de périphérique de l’utilisateur est défini dans la console, seuls les utilisateurs disposant des privilèges multijoueur dans leurs revendications sont autorisés à effectuer des appels vers le service.| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EXC"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4ECD"></a>


## <a name="response-body"></a>Corps de la réponse

Aucun objet n’est envoyés dans le corps de la réponse.

<a id="ID4EPD"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ERD"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)
