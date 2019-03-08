---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
description: " POST (/users/batch/profile/settings)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f859a58e32624223d59d918d46f6230a3abd6db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662224"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
Obtenir le profil d’un ou plusieurs utilisateurs. Le domaine pour ces URI est `profile.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Autorisation](#ID4EFB)
  * [En-têtes de demande nécessaires](#ID4EOB)
  * [Corps de la demande](#ID4EZC)
  * [Corps de la réponse](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
Il s’agit de l’URL du profil uniquement qualifié complet autorisés dans. Toutes les autres API de profil à partir de clients sont bloquées.
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>Authorization
 
Pour accéder à un profil, un jeton d’authentification normal et les revendications sont nécessaires.
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | 
| x-xbl-contract-version| entier non signé 32 bits| La version de contrat doit être définie sur 2, pour distinguer cet appel de l’API de 360 Xbox.| 
| content-type| chaîne| Valeur = <code>application/json</code>| 
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>Corps de la requête
 
<a id="ID4E6C"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
POST /users/batch/profile/settings
   {
      "userIds":[
         "2533274791381930"
       ],
      "settings":[
         "GameDisplayName",
         "GameDisplayPicRaw",
         "Gamerscore",
         "Gamertag"
      ]
   }
      
```

   
<a id="ID4EJD"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EPD"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
      "profileUsers":[
         {
            "id":"2533274791381930",
            "settings":[
               {
                  "id":"GameDisplayName",
                  "value":"John Smith"
               },
               {
                  "id":"GameDisplayPicRaw",
                  "value":"https://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
               },
               {
                  "id":"Gamerscore",
                  "value":"0"
               },
               {
                  "id":"Gamertag",
                  "value":"CracklierJewel9"
               }
            ]
         }
      ]
   }
         
```

   
<a id="ID4EZD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E2D"></a>

 
##### <a name="parent"></a>Parent 

[/users/batch/profile/settings](uri-usersbatchprofilesettings.md)

  
<a id="ID4EFE"></a>

 
##### <a name="reference"></a>Référence 

[Profil (JSON)](../../json/json-profile.md)

   