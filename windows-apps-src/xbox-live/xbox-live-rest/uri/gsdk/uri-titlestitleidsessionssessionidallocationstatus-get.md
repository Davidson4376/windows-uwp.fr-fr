---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 793d634bc1e3dc431b3797759751afb6dfd9c00a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650854"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
Retourne l’état d’allocation de la sessionhost identifié par son ID de session. Les domaines pour ces URI sont `gameserverds.xboxlive.com` et `gameserverms.xboxlive.com`.
 
  * [En-têtes de demande nécessaires](#ID4E4)
  * [En-têtes de réponse requis](#ID4EEB)
  * [Corps de la réponse](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
Aucun.
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
Aucun.
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | 
| description| Retourne une chaîne (à gauche dans pour descendante compatibilité) vide.| 
| clusterId| Retourne une chaîne (à gauche dans pour descendante compatibilité) vide.| 
| hostName| L’URL de l’hôte de session.| 
| status| Indique en file d’attente, traitée ou annulée.| 
| sessionHostId| L’ID d’hôte de session.| 
| sessionId| Le client fourni (au moment de l’allocation) ID de session.| 
| secureContext| L’adresse du périphérique sécurisé.| 
| portMappings| Les mappages de port pour l’instance.| 
| Région| L’emplacement de l’instance.| 
| ticketId| L’ID de session actuel (à gauche dans pour descendante compatibilité).| 
| gameHostId| Le sessionHostId actuel (à gauche dans pour descendante compatibilité).| 
 
<a id="ID4EGD"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "status",:"Fulfilled",
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g==",
        "sessionId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "description": "",
        "clusterId": "",
        "sessionHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0",
        "ticketId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "gameHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0"

      
```

  
<a id="remarks"></a>

 
### <a name="remarks"></a>Notes
 
Un titre doit réessayer uniquement l’appel au service lors de la réception des codes de réponse suivants :
 
   * 200 : réussite 
   * 400 – demande contient des paramètres non valides 
   * 401 : non autorisé 
   * 404 : l’ID de titre ou l’ID de ticket était introuvable ou non valide 
   * 500 : erreur inattendue du serveur. 
    