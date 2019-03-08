---
title: DeviceEndpoint (JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
description: " DeviceEndpoint (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0eaa21072ebf14b6f6d959ff40af34724a45522f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618874"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint (JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
L’objet DeviceEndpoint a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| deviceName| chaîne| Facultatif. Un nom convivial pour l’appareil, le cas échéant. Actuellement, cette valeur n’est pas utilisée.| 
| endpointUri| chaîne| Obligatoire. URL vers laquelle la plateforme client (Windows ou Windows Phone) a obtenu à partir de son service de notification push (WNS ou MPNS).| 
| Paramètres régionaux| chaîne| Obligatoire. La langue souhaitée des notifications envoyées à ce point de terminaison. Peut être une liste de valeurs séparées par des virgules dans l’ordre de préférence. Exemple : « fr-fr, en-US, fr ».| 
| Plateforme| chaîne| Facultatif. Valeurs actuellement prises en charge sont « WindowsPhone » et « Windows ». Si non spécifié, il est dérivé le jeton d’appareil.| 
| platformVersion| chaîne| Facultatif. Le format de cette chaîne est spécifique à chaque plateforme. Actuellement, cette valeur n’est pas utilisée.| 
| systemId| GUID| Obligatoire. Identificateur unique pour l’instance « application » (combinaison utilisateur/appareil). Mise en œuvre de meilleures pratiques est pour une application générer un GUID aléatoire sur install/première exécution et continuer à utiliser cette valeur sur les exécutions suivantes de l’application.| 
| titleId| entier non signé 32 bits| Obligatoire. L’ID de titre de la partie émettre l’appel au service.| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "https://cloud.notify.windows.com/.../",
  "platform": "Windows",
  "platformVersion": "1.0",
  "deviceName": "Test Endpoint",
  "locale": "en-US",
  "titleId": 1297290219
}
    
```

  
<a id="ID4EPD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ERD"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Référence   