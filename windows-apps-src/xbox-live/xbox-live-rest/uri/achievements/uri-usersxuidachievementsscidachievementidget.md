---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 19083937d48d8c312215f1734513d83c59f52f0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627504"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
Obtient les détails de la prime. Le domaine pour ces URI est `achievements.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
  * [Autorisation](#ID4EAB)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4E4C)
  * [En-têtes de demande nécessaires](#ID4EPG)
  * [En-têtes de demande facultatif](#ID4EPH)
  * [Corps de la demande](#ID4ECBAC)
  * [Codes d’état HTTP](#ID4ENBAC)
  * [Corps de la réponse](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| Xbox utilisateur ID (XUID) de l’utilisateur dont la ressource est accédée. Doit correspondre à la XUID de l’utilisateur authentifié.| 
| scid| GUID| Identificateur unique de la configuration de service dont la réalisation est accédée.| 
| achievementid| entier non signé 32 bits| Identificateur unique (au sein de la SCID spécifié) de la prime est accédée.| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>Authorization
 
Revendications d’autorisation utilisées | Revendication| Obligatoire ?| Description| Comportement si manquant| 
| --- | --- | --- | --- | --- | --- | --- | 
| Utilisateur| Oui| Un utilisateur valide sur Xbox LIVE pour lequel la requête est effectuée.| 403 Forbidden| 
| Titre| Non| Titre de l’appelant.| Dépend de AuthZ. À compter du 1er mai 2013, AuthZ ne fournit pas une revendication lorsque manquant et par conséquent refuse l’accès à n’importe quel SCIDs ne pas marqués comme étant publics.| 
| Bac à sable| Non| Le bac à sable à partir de laquelle les résultats doivent être récupérés.| Dépend de AuthZ. À compter du 1er mai 2013, AuthZ ne fournit pas une revendication par défaut lorsque manquant.| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource
 
Effet des paramètres de confidentialité sur la ressource | Utilisateur demande| Cibler le paramètre de confidentialité de l’utilisateur| Comportement| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Comme décrit.| 
| Friend| Tout le monde| OK| 
| Friend| amis uniquement| OK| 
| Friend| bloqué| Il est interdit.| 
| non friend utilisateur| Tout le monde| OK| 
| non friend utilisateur| amis uniquement| Il est interdit.| 
| non friend utilisateur| bloqué| Il est interdit.| 
| site tiers| Tout le monde| OK| 
| site tiers| amis uniquement| Il est interdit.| 
| site tiers| bloqué| Il est interdit.| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.| 
| x-xbl-contract-version| chaîne| Par défaut, V1.| 
| Accept-Language| chaîne| Liste des paramètres régionaux de votre choisis et de secours (par exemple, fr-FR, fr, en-GB, WW-fr, en-US). Le service de primes fonctionnera dans la liste jusqu'à ce qu’il trouve la correspondance des chaînes localisées. Si aucune n’est trouvée, il essaie de correspondre à l’emplacement défini dans le jeton d’utilisateur, qui provient de l’adresse IP de l’utilisateur. Si toujours aucune correspondance des chaînes localisées ne sont trouvés, il utilise les chaînes par défaut fournis par le développeur titre/serveur de publication. | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>Corps de la requête
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La session a été récupérée.| 
| 301| Déplacé de façon permanente| Le service a été déplacé vers un autre URI.| 
| 307| Redirection temporaire| L’URI pour cette ressource a été changé temporairement.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge ; doit être rejetée par la couche MVC.| 
| 408| Expiration du délai de la demande| La demande a pris trop de temps.| 
| 410| Effectué| La ressource demandée n’est plus disponible.| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
    "achievement":
    {
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
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   