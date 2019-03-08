---
title: URI de matchmaking
assetID: 667b02a9-6f34-8165-001b-ee8782575202
permalink: en-us/docs/xboxlive/rest/atoc-reference-matchtickets.html
description: " URI de matchmaking"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c52abca7ed49d4a5e14520095ae944938b86f093
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590464"
---
# <a name="matchmaking-uris"></a>URI de matchmaking
 
Cette section fournit des informations sur les adresses de l’identificateur URI (Universal Resource) et les méthodes de protocole HTTP (Hypertext Transport) associées à partir de Xbox Live Services pour le service. 
 
<a id="ID4E6"></a>

 
## <a name="domain"></a>domaine.
momatch.xboxlive.com  
<a id="ID4EEB"></a>

 
## <a name="service-version"></a>Version du service
 
Les appelants de ces URI HTTP/REST doivent passer la valeur 103 ou version ultérieure pour X-Xbl--Version de contrat, l’en-tête HTTP qui spécifie la version de service des Services de découverte divertissement (EDS). 
  
<a id="ID4ELB"></a>

 
## <a name="system-objects-and-properties"></a>Propriétés et des objets système
 
Actuellement, toute la configuration du service matchmaking se produit manuellement, à l’aide de la partie de configuration de service de la [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) ou [partenaires](https://partner.microsoft.com/dashboard). Certaines informations matchmaking sont également répercutées dans les objets définis pour le MPSD. 
 
Les principaux objets JSON utilisés pour la configuration matchmaking sont définis dans [MatchTicket (JSON)](../../json/json-matchticket.md) et [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md). Notez que des tickets correspondent tous doit définir un **ticketSessionRef** objet pour fournir une référence à une session multijoueur contenant le lecteur ou des lecteurs qui souhaitent mettre en correspondance avec d’autres utilisateurs. 
  
<a id="ID4EBC"></a>

 
## <a name="in-this-section"></a>Dans cette section

[/serviceconfigs/{scid}/hoppers/{hoppername}](uri-serviceconfigsscidhoppershoppername.md)

&nbsp;&nbsp;Prend en charge une opération POST pour créer des tickets de correspondance. 

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)

&nbsp;&nbsp;Prend en charge une opération GET pour récupérer des statistiques pour une hopper.

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)

&nbsp;&nbsp;Prend en charge une opération de suppression d’un ticket de correspondance.
 
<a id="ID4ENC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EPC"></a>

   [MatchTicket (JSON)](../../json/json-matchticket.md)

 [HopperStatsResults (JSON)](../../json/json-hopperstatsresults.md)

 [URI du répertoire de session](../sessiondirectory/atoc-reference-sessiondirectory.md)

  
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Parent 

[Référence de l’identificateur (URI) Universal Resource](../atoc-xboxlivews-reference-uris.md)

   