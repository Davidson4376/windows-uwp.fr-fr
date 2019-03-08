---
title: Codes d’état HTTP standard
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
description: " Codes d’état HTTP standard"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c77972e88dbf4950f716594ee61220d1734a7fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635554"
---
# <a name="standard-http-status-codes"></a>Codes d’état HTTP standard
 
Le protocole HTTP (Hypertext Transport) standard décrit plusieurs des codes d’état retournés par le serveur en réponse à une demande du client. Méthodes de Xbox Live Services retournent les codes d’état de conformité de protocole HTTP pour décrire l’état de la demande.
 
Voici une liste des codes d’état renvoyés par les Services Xbox Live et leurs significations classiques.
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Codes d’état des Services Xbox Live
 
| Code| Expression du motif| Description| 
| --- | --- | --- | 
| 200| OK| La demande a réussi.| 
| 201| Créé le| L’entité a été créée.| 
| 202| Accepté| La demande a été acceptée, mais n’a pas encore été effectuée.| 
| 204| Aucun contenu| La demande est terminée, mais n’a pas de contenu à retourner.| 
| 301| Déplacé de façon permanente| Le service a été déplacé vers un autre URI.| 
| 302| Trouvé| La ressource demandée réside temporairement sous un autre URI.| 
| 307| Redirection temporaire| L’URI pour cette ressource a été changé temporairement.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande| La demande a pris trop de temps.| 
| 409| Conflit| La demande a échoué en raison d’un conflit avec l’état actuel de la ressource.| 
| 410| Effectué| La ressource demandée n’est plus disponible.| 
| 412| Échoué de la précondition| Le serveur ne répond pas aux parmi les conditions préalables que le demandeur est mis sur la demande.| 
| 416| Plage demandée non satisfaisante| La plage demandée n’est pas disponible.| 
| 500| erreur de serveur interne| Le serveur a rencontré une situation inattendue qui l’a empêché de répondre à la demande.| 
| 501| Non implémenté| Le serveur ne prend pas en charge les fonctionnalités requises pour répondre à la demande.| 
| 503| Service non disponible| La demande a été limitée, la demande, puis réessayez la valeur de nouvelle tentative du client en secondes (par exemple, 5 secondes plus tard).| 
 

> [!NOTE] 
> Certaines ressources et les méthodes fournissent des informations spécifiques sur la signification des codes d’état particulier dans le contexte de cette ressource ou de la méthode. Pour plus d’informations, reportez-vous à la documentation pour les ressources ou les méthodes que vous utilisez. 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>Parent  

[Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>Référence [Universal Resource identificateur (URI) référence](../uri/atoc-xboxlivews-reference-uris.md)

 [Référence supplémentaire](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpswwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>Liens externes [W3.org : HTTP/1.1 Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   