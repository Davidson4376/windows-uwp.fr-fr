---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
description: " GET (/users/xuid({xuid})/achievements)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 720170e88808db7ef95b88896fbca4f1cda4a091
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655264"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
Obtient la liste des primes définis sur le titre, ceux déverrouillé par l’utilisateur ou ceux de que l’utilisateur est en cours d’exécution. Le domaine pour ces URI est `achievements.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [Paramètres de chaîne de requête](#ID4ECB)
  * [Autorisation](#ID4ENF)
  * [En-têtes de demande nécessaires](#ID4ESG)
  * [En-têtes de demande facultatif](#ID4ESH)
  * [Corps de la demande](#ID4EIBAC)
  * [Corps de la réponse](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur dont (ressource) est accessible. Doit correspondre à la XUID de l’utilisateur authentifié.| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Obligatoire| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| Non| entier signé 32 bits| Retourne des éléments après le nombre donné d’éléments. Par exemple, <b>skipItems = « 3 »</b> récupérera les éléments commençant par le quatrième élément récupéré. | 
| <b>continuationToken</b>| Non| chaîne| Retourner les éléments commençant le jeton de continuation donné. | 
| <b>maxItems</b>| Non| entier signé 32 bits| Nombre maximal d’éléments à retourner à partir de la collection, qui peut être combinée avec <b>skipItems</b> et <b>continuationToken</b> pour retourner une plage d’éléments. Le service peut fournir une valeur par défaut si <b>maxItems</b> n’est pas présent et peut renvoyer moins de <b>maxItems</b>, même si la dernière page de résultats n’a pas encore été retournée. | 
| <b>titleId</b>| Non| chaîne| Un filtre pour les résultats retournés. Accepte un ou plusieurs identificateurs de titre décimal, délimitée par des virgules.| 
| <b>unlockedOnly</b>| Non| Valeur booléenne| Filtrer les résultats retournés. Si la valeur <b>true</b>, retournent seulement les primes déverrouillées pour l’utilisateur. Valeur par défaut est <b>false</b>.| 
| <b>possibleOnly</b>| Non| Valeur booléenne| Filtrer les résultats retournés. Si la valeur <b>true</b>, retournera tous les résultats possibles, mais pas déverrouillé métadonnées - simplement les informations de réalisation de XMS. Valeur par défaut est <b>false</b>.| 
| <b>types</b>| Non| chaîne| Un filtre pour les résultats retournés. Peut être « Persistant » ou « Défi ». Valeur par défaut est pris en charge tous les types.| 
| <b>orderBy</b>| Non| chaîne| Spécifie l’ordre dans lequel retourner les résultats. Can be "Unordered", "Title", "UnlockTime", or "EndingSoon". La valeur par défaut est « Désordonnées ».| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>Authorization
 
| Revendication| Obligatoire ?| Description| Comportement si manquant| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Utilisateur| L’appelant est un utilisateur autorisé de Xbox LIVE.| L’appelant doit être un utilisateur valide sur Xbox LIVE.| 403 Forbidden| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Valeur par défaut : 1.| 
| <b>x-xbl-contract-version</b>| entier non signé 32 bits| S’il est présent et défini sur 2, la version V2 de cette API est utilisée. Sinon, V1.| 
| <b>Accept-Language</b>| chaîne| Liste des paramètres régionaux de votre choisis et de secours (par exemple, fr-FR, fr, en-GB, WW-fr, en-US). Le service de primes fonctionnera dans la liste jusqu'à ce qu’il trouve la correspondance des chaînes localisées. Si aucune n’est trouvée, il essaie de correspondre à l’emplacement défini dans le jeton d’utilisateur, qui provient de l’adresse IP de l’utilisateur. Si toujours aucune correspondance des chaînes localisées ne sont trouvés, il utilise les chaînes par défaut fournis par le développeur titre/serveur de publication. | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne un tableau de [prime (JSON)](../../json/json-achievementv2.md) objets et un [PagingInfo (JSON)](../../json/json-paginginfo.md) objet.
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
    "achievements":
    [{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "achievementState":"Achieved",
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
        }],
        "pagingInfo":
        {
                "continuationToken":null,
                "totalRecords":1
        }
}
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements](uri-achievementsusersxuidachievementsv2.md)

  
<a id="ID4E2CAC"></a>

 
##### <a name="reference"></a>Référence 

[Prime (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Paramètres de pagination](../../additional/pagingparameters.md)

   