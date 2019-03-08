---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
description: " GET (/qosservers)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 02d24dbf1d189b759784dbbfa7052e2c218ec27e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632064"
---
# <a name="get-qosservers"></a>GET (/qosservers)
URI appelée par un client pour obtenir la liste des serveurs de qualité de service disponibles pour une utilisation avec Xbox Live Compute. Les domaines pour ces URI sont `gameserverds.xboxlive.com` et `gameserverms.xboxlive.com`.
 
  * [En-têtes de demande nécessaires](#ID4EBB)
  * [En-têtes de réponse requis](#ID4EUC)
  * [Corps de la réponse](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nom d'hôte

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
Lorsque vous élaborez une demande, les en-têtes affichés dans le tableau suivant sont requis.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | 
| Content-Type| application/json| Type de données en cours de soumission.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Longueur de l’objet de demande.| 
| x-xbl-contract-version| 1| Version de contrat d’API.| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
Une réponse inclut toujours les en-têtes affichés dans le tableau suivant.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/json| Type de données dans le corps de réponse.| 
| Content-Length|  | Longueur du corps de réponse.| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| Tableau d’informations sur le serveur.| 
| serverFqdn| Le nom de domaine complet du serveur.| 
| serverSecureDeviceAddress| L’adresse du périphérique sécurisé du serveur.| 
| targetLocation| L’emplacement géographique du serveur.| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{ 
  "qosServers" : 
  [ 
    { "serverFqdn" : "xblqosncus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "North Central US" },
    { "serverFqdn" : "xblqoswus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "West US" },
  ]
}

      
```

   
<a id="ID4EBF"></a>

 
## <a name="see-also"></a>Voir également
 [/qosservers](uri-qosservers.md)

  