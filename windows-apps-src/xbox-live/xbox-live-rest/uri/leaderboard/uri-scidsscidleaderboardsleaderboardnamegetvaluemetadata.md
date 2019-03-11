---
title: GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
assetID: ee69a9e3-ea91-3cf5-a10a-811762cba892
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnamegetvaluemetadata.html
description: " GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fad57c5e989d933777c913030faaa594c6bbd059
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623434"
---
# <a name="get-scidsscidleaderboardsleaderboardnameincludevaluemetadata"></a>GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
 
Obtient un classement global prédéfini, ainsi que toutes les métadonnées associées aux valeurs de classement.
 
Le domaine pour ces URI est `leaderboards.xboxlive.com`.
 
  * [Remarques](#ID4EY)
  * [Paramètres d’URI](#ID4EHB)
  * [Paramètres de chaîne de requête](#ID4ESB)
  * [Autorisation](#ID4EVD)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EQE)
  * [En-têtes de demande nécessaires](#ID4EZE)
  * [En-têtes de demande facultatif](#ID4EOG)
  * [Codes d’état HTTP](#ID4EOH)
  * [En-têtes de réponse](#ID4EFDAC)
  * [Corps de la réponse](#ID4ECFAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Notes
 
Le ? inclure = valuemetadata paramètre de requête autorise la réponse inclure toutes les métadonnées associées aux valeurs de classement. Les métadonnées de la valeur contient le client spécifié données associées à la valeur, tels que le modèle et la couleur d’une voiture permet d’atteindre un meilleur moment sur une piste de course.
 
Métadonnées de la valeur sont définie sur la statistique de l’utilisateur que le classement est basé sur, pas sur le classement lui-même.
 
Leaderboard APIs sont en lecture seule et par conséquent prennent uniquement en charge le verbe GET. Ils reflètent les résultats triés et « pages » de statistiques indexées qui sont dérivés de statistiques d’utilisateur individuels qui ont été écrits par le biais de la plateforme de données. Index de classement complète peuvent être très volumineux et les appelants ne demandent jamais de voir un dans son intégralité, par conséquent, cet URI prend en charge plusieurs arguments de chaîne de requête qui permettent à un appelant de préciser de quel type de vue il souhaite voir par rapport à ce classement.
 
Opérations GET ne peut pas modifier toutes les ressources donc cela génère les mêmes résultats si exécutée une ou plusieurs fois.
  
<a id="ID4EHB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| scid| GUID| Identificateur de la configuration de service qui contient la ressource sollicitée.| 
| leaderboardname| chaîne| Identificateur unique de la ressource de classement prédéfinis accédée.| 
  
<a id="ID4ESB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête
 
| Paramètre| Type| Description| 
| --- | --- | --- | --- | --- | --- | 
| include=valuemetadata| chaîne| Indique que la réponse inclut des métadonnées de valeur associées aux valeurs de classement.| 
| maxItems| entier non signé 32 bits| Nombre maximal d’enregistrements de classement à renvoyer dans une page de résultats. Si non spécifié, un numéro par défaut s’affichera (10). La valeur maximale de maxItems est toujours pas définie, mais nous souhaitons éviter de grands jeux de données, donc cette valeur doit cibler probablement le plus grand jeu qui SYNTONISEUR que l’interface utilisateur peut gérer par appel.| 
| skipToRank| entier non signé 32 bits| Retourner une page de résultats en commençant par le rang de classement spécifié. Le reste des résultats seront triés par rang. Cette chaîne de requête peut retourner un jeton de liaison qui peut être transmis dans la requête suivante pour obtenir une « page » de résultats.| 
| skipToUser| chaîne| Retourner une page de tableau de résultats autour de la xuid gamer spécifié, quel que soit le rang ou le score de cet utilisateur. La page vont être classée par rang de centile avec l’utilisateur spécifié dans la dernière position de la page des vues prédéfinies, ou au milieu des vues de classement stat. Il n’existe aucun continuationToken fourni pour ce type de requête.| 
| continuationToken| chaîne| Si un appel précédent a renvoyé un élément continuationToken, puis l’appelant peut transmettre ce jeton « tel quel » dans une chaîne de requête pour obtenir la page suivante de résultats.| 
  
<a id="ID4EVD"></a>

 
## <a name="authorization"></a>Authorization
 
Il est logique d’autorisation implémentée pour les scénarios d’isolation du contenu et de contrôle d’accès.
 
   * Statistiques des classements et utilisateur peuvent être lues à partir de clients sur n’importe quelle plateforme, sous réserve que l’appelant envoie un jeton XSTS valide avec la demande. Écritures sont évidemment limités aux clients pris en charge par la plateforme de données.
   * Les développeurs de titre peuvent marquer des statistiques comme ouvertes ou limitées avec XDP ou partenaires. Classements sont ouverts de statistiques. Ouvrir les statistiques sont accessibles par les applications web, ainsi qu’iOS, Android, Windows, Windows Phone et Smartglass, tant que l’utilisateur est autorisé au bac à sable. Autorisation de l’utilisateur à un bac à sable est gérée via XDP ou partenaires.
  
Pseudo-code pour le contrôle se présente comme suit :
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EQE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource
 
Aucune vérification de la confidentialité est effectuée lors de la lecture des données de classement.
  
<a id="ID4EZE"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Chaîne. Informations d’identification pour l’authentification HTTP. Exemple de valeur : <b>XBL3.0 x =&lt;userhash > ;&lt; jeton ></b>| 
| X-Xbl-Contract-Version| Chaîne. Indique quelle version de l’API à utiliser. Cette valeur doit être définie à « 3 » afin d’inclure des métadonnées de la valeur dans la réponse.| 
| X-RequestedServiceVersion| Chaîne. Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Valeur par défaut : 1.| 
| Accept| Chaîne. Types de contenu qui sont acceptables. Exemple de valeur : <b>application/json</b>| 
  
<a id="ID4EOG"></a>

 
## <a name="optional-request-headers"></a>En-têtes de demande facultatif
 
| En-tête| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| Chaîne. Balise d’entité, utilisé si le client prend en charge la mise en cache. Exemple de valeur : <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4EOH"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Expression du motif| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| La session a été récupérée.| 
| 304| Non modifié| Ressource ne pas été modifié depuis son dernier demandé.| 
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.| 
| 401| Non autorisé| La demande requiert une authentification utilisateur.| 
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.| 
| 404| Introuvable| Impossible de trouver la ressource spécifiée.| 
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.| 
| 408| Expiration du délai de la demande| Version de la ressource n’est pas pris en charge ; doit être rejetée par la couche MVC.| 
  
<a id="ID4EFDAC"></a>

 
## <a name="response-headers"></a>En-têtes de réponse
 
| En-tête| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| chaîne| Obligatoire. Le type MIME du corps de la réponse. Exemple : <b>application/json</b>.| 
| Content-Length| chaîne| Obligatoire. Le nombre d’octets envoyés dans la réponse. Exemple : <b>232</b>.| 
| ETag| chaîne| Facultatif. Utilisé pour l’optimisation du cache. Exemple : <b>686897696a7c876b7e</b>.| 
  
<a id="ID4ECFAC"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
<a id="ID4EIFAC"></a>

 
### <a name="response-members"></a>Membres de la réponse
 
pagingInfo | pagingInfo| section| Facultatif. Présenter uniquement lorsque maxItems est spécifié dans la demande.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| continuationToken| entier non signé 64 bits| Obligatoire. Spécifie quelle valeur exploités par le <b>skipItems</b> interroger le paramètre pour obtenir la page suivante de résultats pour cet URI si vous le souhaitez. Si aucun <b>pagingInfo</b> est retournée, alors il n’existe aucune page suivante de données doivent être obtenues.| 
| totalItems| entier non signé 64 bits| Obligatoire. Nombre total d’entrées dans le classement. Exemple de valeur : 1234567890| 
 
leaderboardInfo | leaderboardInfo| section| Obligatoire. Toujours retourné. Contient les métadonnées concernant le classement demandé.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| chaîne| Non utilisé.| 
| totalCount| chaîne (entier non signé 64 bits)| Obligatoire. Nombre total d’entrées dans le classement. Exemple de valeur : 1234567890| 
| columnDefinition| Objet JSON| Obligatoire. Décrit la colonne dans le classement.| 
| columnDefinition.displayName| chaîne| Obligatoire. Un nom descriptif de la colonne dans le classement.| 
| columnDefinition.statName| chaîne| Obligatoire. Le nom de la statistique utilisateur basé sur le classement.| 
| columnDefinition.type| chaîne| Obligatoire. Le type de données de la statistique de l’utilisateur dans la colonne.| 
 
userList | userList| section| Obligatoire. Toujours retourné. Contient les scores réel de l’utilisateur pour le classement demandé.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Gamertag| chaîne| Obligatoire. L’identité de l’utilisateur dans l’entrée de classement.| 
| xuid| entier non signé 64 bits| Obligatoire. L’ID d’utilisateur Xbox de l’utilisateur dans l’entrée de classement.| 
| percentile| chaîne| Obligatoire. Rang de centile de l’utilisateur dans le classement.| 
| rang| chaîne| Obligatoire. Classement numérique de l’utilisateur dans le classement.| 
| value| chaîne| Obligatoire. Valeur de l’utilisateur de la statistique sur laquelle repose le classement.| 
| valueMetadata| chaîne| Obligatoire. Une chaîne de virgules séparées par des paires de chaînes, au format «\"nom\" : \"valeur\",... »

S’il n’existe aucune métadonnée, cette valeur est une chaîne vide. | 
  
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 
L’URI de demande suivant illustre est ignorée au rang sur un classement global.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured?include=valuemetadata&maxItems=3&skipToRank=15000
         
```

 
Afin de retourner les métadonnées de valeur, l’en-tête suivant doit également être spécifié.
 

```cpp
X-Xbl-Contract-Version : 3
          
```

 
L’URI ci-dessus retourne la réponse JSON suivante.
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "15003",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columnDefinition" : 
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            }
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": "1234567890123444",
            "percentile": 0.64,
            "rank": 15000,
            "value": "1002",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2000, \"israining\" : false}"
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": "1234567890123555",
            "percentile": 0.64,
            "rank": 15001,
            "value": "993",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2020, \"israining\" : true}"
 
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": "1234567890123666",
            "percentile": 0.64,
            "rank": 15002,
            "value": "700",
            "valuemetadata" : "{\"color\" : \"red\",\"weight\" : 300, \"israining\" : false}"
        }
    ]
}
         
```

   
<a id="ID4E6LAC"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EBMAC"></a>

 
##### <a name="parent"></a>Parent 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   