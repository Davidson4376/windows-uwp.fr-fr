---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f868fdf4f3d5cd36000784d9c5a3437fa5d67ffa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593854"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
Obtenir le profil d’un utilisateur ou les utilisateurs, avec un Moniker de personnes prennent en charge. Le domaine pour ces URI est `profile.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EKB)
  * [Paramètres de chaîne de requête](#ID4EVB)
  * [En-têtes de demande nécessaires](#ID4EQC)
  * [Corps de la demande](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes
 
**userList** et **userIds** sont mutuellement exclusives des paramètres. Si les deux ou les deux sont spécifiées, vous obtiendrez un **BadRequest** précédent. **userList** est un tableau pour l’évolutivité dans les scénarios où plusieurs listes nommées sont utiles pour la demande. **userIds** se compose de chaînes décimales pour XUIDs - JSON est incorrect à la sérialisation des entiers non signés 64 bits. Enfin, paramètres Xbox One seront nommés paramètres, dont le nom lisible normal, plutôt que les entiers non signés 64 bits ou des constantes obscures comme **XONLINE_PROFILE_ASDF**.
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| userId| chaîne| Puis-je être 'xuid(12345)', 'gt(myGamertag)' ou 'me'.| 
| userList| chaîne| Une liste nommée de personnes pour obtenir les paramètres de. Actuellement, les personnes est la seule liste pris en charge.| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| paramètres| chaîne| Liste délimitée par des virgules des noms des paramètres.| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| entier signé 32 bits| Valeur = 2| 
| content-type| chaîne| Valeur = <code>application/json</code>| 
  
<a id="ID4E2D"></a>

 
## <a name="request-body"></a>Corps de la requête
 
<a id="ID4EBE"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
GET /users/me/profile/settings/people/people?settings=GameDisplayName,GameDisplayPicRaw,Gamerscore,Gamertag
      
```

  
<a id="ID4EKE"></a>

  
 
<a id="ID4EME"></a>

 
##### <a name="response-body"></a>Corps de la réponse 
La réponse est un **ReadMultiSettingsResponseV2** objet. En supposant que l’utilisateur appelant n'a qu’une seule friend :
  

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
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F&format=png&w=64&h=64"
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

   
<a id="ID4E3E"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4E5E"></a>

 
##### <a name="parent"></a>Parent 

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [Profil (JSON)](../../json/json-profile.md)

   