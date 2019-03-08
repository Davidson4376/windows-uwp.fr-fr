---
title: GET (/serviceconfigs/{scid}/hoppers/{name}/stats)
assetID: 4de5b07d-93e1-8ff0-05dd-1d3bb1802088
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestatsget.html
description: " GET (/serviceconfigs/{scid}/hoppers/{name}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 95de95b35de496331dd3fe0a4c69f18e047c1020
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621694"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

Obtient les statistiques pour une hopper.

> [!IMPORTANT]
> Cette méthode est conçue pour une utilisation avec contrat 103 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 103 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4E5)
  * [Autorisation](#ID4EJB)
  * [Codes d’état HTTP](#ID4E3C)
  * [Corps de la demande](#ID4EFD)
  * [Corps de la réponse](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes
Cette méthode HTTP/REST Obtient des statistiques à partir de la hopper nommé à la configuration du service au niveau du code (SCID). Cette méthode peut être encapsulée par le **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID) pour la session.|
| name| chaîne| Le nom de la hopper.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Authorization

| Type| Obligatoire| Description| Cas d’absence de réponse|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (ID utilisateur)| oui| L’utilisateur qui effectue la requête doit être un membre de la session de ticket référencé par le ticket. | 403|
| Privilèges et le Type d’appareil| oui| Lorsque le type de périphérique de l’utilisateur est défini dans la console, seuls les utilisateurs disposant des privilèges multijoueur dans leurs revendications sont autorisés à effectuer des appels vers le service. | 403|
| ID de titre/preuve de Type d’achat/appareil| oui| Le titre est mis en correspondance dans doit autoriser matchmaking pour la revendication titre spécifié, la combinaison de type de périphérique. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4EQD"></a>


## <a name="response-body"></a>Corps de la réponse

| Membre| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| chaîne| Le nom de la hopper sélectionné.|
| waitTime| entier signé 32 bits| Nombre moyen de temps pour le hopper (un nombre entier de secondes) de mise en correspondance. |
| Remplissage| entier signé 32 bits| Le nombre de personnes en attente pour les correspondances dans le hopper.|

<a id="ID4E1D"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }


```


<a id="ID4EJE"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ELE"></a>


##### <a name="parent"></a>Parent  

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)
