---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 674e52d15d115560d4813edcd9687c2fe9694c9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650764"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
Retourne un classement de réseaux sociaux en les classant que le stat valeurs (scores) pour tous les contacts connus de l’utilisateur actuel ou uniquement les contacts désignés en tant que favori personnes par cet utilisateur.
Le domaine pour ces URI est `leaderboards.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EAB)
  * [Paramètres de chaîne de requête](#ID4ELB)
  * [Autorisation](#ID4EQD)
  * [En-têtes de demande nécessaires](#ID4EGE)
  * [En-têtes de demande facultatif](#ID4EXF)
  * [Corps de la demande](#ID4ETG)
  * [Codes d’état HTTP](#ID4ECEAC)
  * [En-têtes de réponse requis](#ID4E1HAC)
  * [En-têtes de réponse facultative](#ID4EDJAC)
  * [Corps de la réponse](#ID4EDKAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

API de classement est en lecture seule et par conséquent prennent uniquement en charge le verbe GET (par exemple, HTTPS). Ils reflètent les résultats triés et « pages » de statistiques indexées qui sont dérivés de statistiques d’utilisateur individuels qui ont été écrits par le biais de la plateforme de données. Index de classement complète peuvent être très volumineux et les appelants ne demandent jamais de voir un dans son intégralité, par conséquent, cet URI prend en charge plusieurs arguments de chaîne de requête qui permettent à un appelant de préciser de quel type de vue il souhaite voir par rapport à ce classement.

Opérations GET ne peut pas modifier toutes les ressources donc cela génère les mêmes résultats si exécutée une ou plusieurs fois.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid| chaîne| Identificateur de l’utilisateur.|
| scid| chaîne| Identificateur de la configuration de service qui contient la ressource sollicitée.|
| statname| chaîne| Identificateur unique de la ressource stat utilisateur en cours d’accès.|
| tous|favori| énumération| S’il faut les classer le stat valeurs (scores) pour tous les contacts connus de l’utilisateur actuel ou uniquement les contacts désignés en tant que favori personnes par cet utilisateur.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- |
| maxItems| entier non signé 32 bits| Nombre maximal d’enregistrements de classement à renvoyer dans une page de résultats. Si non spécifié, un numéro par défaut s’affichera (10). La valeur maximale de maxItems est toujours pas définie, mais nous souhaitons éviter de grands jeux de données, donc cette valeur doit cibler probablement le plus grand jeu qui SYNTONISEUR que l’interface utilisateur peut gérer par appel. |
| skipToRank| entier non signé 32 bits| Retourner une page de résultats en commençant par le rang de classement spécifié. Le reste des résultats seront triés par rang. Cette chaîne de requête peut retourner un jeton de liaison qui peut être transmis dans la requête suivante pour obtenir une « page » de résultats. |
| skipToUser| chaîne| Retourner une page de tableau de résultats autour de la xuid gamer spécifié, quel que soit le rang ou le score de cet utilisateur. La page vont être classée par rang de centile avec l’utilisateur spécifié dans la dernière position de la page des vues prédéfinies, ou au milieu des vues de classement stat. Il existe aucune <b>continuationToken</b> fourni pour ce type de requête. |
| continuationToken| chaîne| Si un appel précédent a renvoyé un <b>continuationToken</b>, puis l’appelant peut transmettre ce jeton « tel quel » dans une chaîne de requête pour obtenir la page suivante de résultats. |
| sort| chaîne| Indiquez si vous souhaitez classer la liste des lecteurs à partir de l’ordre de valeur de faible à élevé (« croissant ») ou élevé à faible valeur ordre (« décroissant »). Il s’agit d’un paramètre optionnel ; la valeur par défaut est l’ordre décroissant.|

<a id="ID4EQD"></a>


## <a name="authorization"></a>Authorization

L’autorisation de Xuid est requise.

Logique d’autorisation est implémentée dans le cadre de l’isolation du contenu et des scénarios de contrôle d’accès.

Statistiques leaderboards et utilisateur peuvent être lues à partir de clients sur n’importe quelle plateforme, sous réserve que l’appelant envoie un jeton XSTS valide avec leur demande. Écritures sont limitées (évidemment) pour les clients pris en charge par la plateforme de données.

Les développeurs de titre peuvent marquer des statistiques comme ouvertes ou limitées avec XDP ou partenaires. Classements sont ouverts de statistiques. Ouvrir les statistiques sont accessibles par les applications web, ainsi qu’iOS, Android, Windows, Windows Phone et Smartglass, tant que l’utilisateur est autorisé au bac à sable. Autorisation de l’utilisateur à un bac à sable est gérée via XDP ou partenaires.

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Description|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| Chaîne. Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».|
| Content-Type| Chaîne. Le type MIME du corps de la demande. Exemple de valeur : « application/json ».|
| X-RequestedServiceVersion| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.|
| Accept| Chaîne. Valeurs acceptables Content-Type. Exemple de valeur : « application/json ».|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| Chaîne. Balise d’entité - utilisé si le client prend en charge la mise en cache. Exemple de valeur : « 686897696a7c876b7e ».|

<a id="ID4ETG"></a>


## <a name="request-body"></a>Corps de la requête

Pour optimiser la capacité d’un appelant à comprendre les données qu’il arrive pour l’affichage correct, chaque valeur stat pour chaque utilisateur est retournée sous forme de chaîne au format dans lequel il doit être affiché et mis en forme pour faire correspondre les paramètres régionaux spécifiés dans le langage d’accept en-tête de la requête. Un localisée « nom complet » se produira également pour statname pour que le classement.

<a id="ID4E2G"></a>


### <a name="required-members"></a>Membres requis

| Membre| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| section| Facultatif. Retourné lorsque le rang de la dernière écriture dans la page est inférieur à totalItems. Cette section n’est pas également retournée quand skipToUser est spécifié dans la demande.|
| continuationToken| chaîne| Obligatoire. Spécifie quelle valeur exploités par le paramètre de requête « continuationToken » pour obtenir la page suivante des résultats pour cet URI si vous le souhaitez. Si aucun pagingInfo n’est retourné. il n’existe aucune « page suivant » de données doivent être obtenues.|
| totalItems| entier non signé 64 bits| Obligatoire. Nombre total d’entrées dans le classement.|
| <b>leaderboardInfo</b>| section| Obligatoire. Toujours retourné. Contient les métadonnées concernant le classement demandé.|
| displayName| chaîne| Obligatoire. Nom complet localisé pour le classement prédéfini. Exemple de valeur : « Total des indicateurs capturés ».|
| totalCount| chaîne| Obligatoire. Nombre total d’entrées dans le classement.|
| Colonnes| tableau| Obligatoire.|
| displayName| chaîne| Obligatoire. Correspond à une colonne dans le classement.|
| statName| chaîne| Obligatoire. Correspond à une colonne dans le classement.|
| type| chaîne| Obligatoire. Correspond à une colonne dans le classement.|
| <b>userList</b>| section| Obligatoire. Toujours retourné. Contient les scores réel de l’utilisateur pour le classement demandé.|
| Gamertag| chaîne| Obligatoire. Correspond à l’utilisateur dans l’entrée de classement.|
| xuid| entier signé 32 bits| Obligatoire. Correspond à l’utilisateur dans l’entrée de classement.|
| percentile| chaîne| Obligatoire. Correspond à l’utilisateur dans l’entrée de classement.|
| rang| chaîne| Obligatoire. Correspond à l’utilisateur dans l’entrée de classement.|
| Valeurs| tableau| Obligatoire. Chaque valeur séparées par des virgules correspond à une colonne dans le classement.|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 304| Non modifié|  |
| 400| demande incorrecte| | Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.|
| 401| Non autorisé| | La demande requiert une authentification utilisateur.|
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.|
| 404| Introuvable| Impossible de trouver la ressource spécifiée.|
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge ; doit être rejetée par la couche MVC.|
| 408| Expiration du délai de la demande| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| chaîne| Le type mime du corps de la réponse. Exemples de valeurs : « application/json ».|
| Content-Length| chaîne| Le nombre d’octets envoyés dans la réponse. Exemple de valeur : "232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>En-têtes de réponse facultative

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple de valeur : « 686897696a7c876b7e ».|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>Corps de la réponse

Demande pour le classement des réseaux sociaux, aucune pagination :

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "pagingInfo": null,
    "leaderboardInfo": {
        "displayName": "Kills",
        "totalCount": 3,
        "columns": [
            {
                "displayName": "Kills",
                "statName": "enemydefeats",
                "type": "integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag":"xxxSniper39",
            "xuid":"1234567890123555",
            "percentile":1.0,
            "rank":1,
            "values": [
                "47"
            ]
        },
        {
            "gamertag":"WarriorSaint",
            "xuid":"2533274916402282",
            "percentile":0.66,
            "rank":2,
            "values": [
                "42"
            ]
        },
        {
            "gamertag":"WhockaWhocka",
            "xuid":"1234567890123666",
            "percentile":0.33,
            "rank":3,
            "values": [
                "12"
            ]
        }
    ]
}

```


<a id="ID4EXKAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EZKAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite}](uri-usersxuidscidstatnamepeople.md)
