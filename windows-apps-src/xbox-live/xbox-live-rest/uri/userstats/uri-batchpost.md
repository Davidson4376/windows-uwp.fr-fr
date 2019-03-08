---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
description: " POST (/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a854fc830c87afbf675a379599916bf3db919539
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645834"
---
# <a name="post-batch"></a>POST (/batch)
POST, méthode qui fonctionne comme une méthode GET pour les requêtes de lots complexes pour plusieurs statistiques du lecteur entre plusieurs titres. Le domaine pour ces URI est `userstats.xboxlive.com`.
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>Notes
 
Les développeurs de titre peuvent marquer des statistiques comme ouvertes ou limitées avec XDP ou partenaires. Classements sont ouverts de statistiques. Ouvrir les statistiques sont accessibles par les applications web, ainsi qu’iOS, Android, Windows, Windows Phone et Smartglass, tant que l’utilisateur est autorisé au bac à sable. Autorisation de l’utilisateur à un bac à sable est gérée via XDP ou partenaires.
  
  * [Remarques](#ID4ET)
  * [Remarques](#ID4EFB)
  * [Autorisation](#ID4EUB)
  * [En-têtes de demande nécessaires](#ID4ETC)
  * [En-têtes de demande facultatif](#ID4E3D)
  * [Corps de la demande](#ID4EAF)
  * [Codes d’état HTTP](#ID4EWF)
  * [Corps de la réponse](#ID4ENBAC)
 
<a id="ID4EFB"></a>

 
## <a name="remarks"></a>Notes
 
L’appelant fournit un corps de message avec un tableau d’utilisateurs, la configuration de service ID (SCIDs) et une liste de noms de statistiques par SCIDs pour lequel récupérer des statistiques.
 
Vous pouvez s’avérer plus utile de consulter le simple et unique-statistic [obtenir](uri-usersxuidscidsscidstatsget.md) avant de lire cette page en mode batch plus complexe, de méthode.
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>Authorization
 
Il est logique d’autorisation implémentée pour les scénarios d’isolation du contenu et de contrôle d’accès.
 
   * Statistiques des classements et utilisateur peuvent être lues à partir de clients sur n’importe quelle plateforme, sous réserve que l’appelant envoie un jeton XSTS valide avec la demande. Les écritures sont évidemment limités aux clients pris en charge par le.
   * Les développeurs de titre peuvent marquer des statistiques comme ouvertes ou limitées avec XDP ou partenaires. Classements sont ouverts de statistiques. Ouvrir les statistiques sont accessibles par les applications web, ainsi qu’iOS, Android, Windows, Windows Phone et Smartglass, tant que l’utilisateur est autorisé au bac à sable. Autorisation de l’utilisateur à un bac à sable est gérée via XDP ou partenaires.
  
L’exemple suivant est un pseudo-code de la vérification :
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nom/numéro du service auquel cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Corps de la requête
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>Exemple de demande
 
le corps POST suivant informe le service que quatre statistiques sont demandées à partir de deux SCIDs différentes pour deux utilisateurs différents.
 

```cpp
{    
"requestedusers": [
                1234567890123460,
                1234567890123234
            ],
            "requestedscids": [
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c1212",
                    "requestedstats": [
                        "Test4FirefightKills",
                        "Test4FirefightHeadshots"
                    ]
                },
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c0343",
                    "requestedstats": [
                        "OverallTestKills",
                        "TestHeadshots"
                    ]
                }
            ] 
}
      
```

   
<a id="ID4EWF"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La session a été récupérée.| 
| 304| Non modifié| Ressource ne pas été modifié depuis son dernier demandé.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande| Version de la ressource n’est pas pris en charge ; doit être rejetée par la couche MVC.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{    
   "users":[          
       {    
      "xuid": "123456789"
        "gamertag": "WarriorSaint",
        "scids":[
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c1212",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":7
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":4
             }]
                        },
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c0343",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":3434
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":41
             }]
          }],
                   },
    {    
                   "gamertag":"TigerShark",
                   "xuid":1234567890123234
        "scids":[
          {
             "scid":"123456789",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":63
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":12
             }]
                        },
          {
"scid":"987654321",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":375
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":34
             }]
          }],
                   }]
}
         
```

   
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Parent 

[/batch](uri-batch.md)

   