---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: baf965dcbd23bf00d7d0953726f9f20852324e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662384"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
Obtient une configuration de service la portée définie par une liste délimitée par des virgules des noms de statistiques utilisateur pour le compte de l’utilisateur spécifié.
Le domaine pour ces URI est `userstats.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EEB)
  * [Paramètres de chaîne de requête](#ID4EPB)
  * [Autorisation](#ID4EUC)
  * [En-têtes de demande nécessaires](#ID4EPD)
  * [En-têtes de demande facultatif](#ID4EYE)
  * [Corps de la demande](#ID4E3F)
  * [Codes d’état HTTP](#ID4EHG)
  * [Corps de la réponse](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

les clients ont besoin d’un moyen pour lire et écrire des statistiques de titre pour le compte de lecteurs vers notre nouveau système de statistiques de lecteur.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid| GUID| Xbox utilisateur ID (XUID) de l’utilisateur au nom duquel pour accéder à la configuration de service.|
| scid| GUID| Identificateur de la configuration de service qui contient la ressource sollicitée.|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- |
| statNames| chaîne| Le seul paramètre de chaîne de requête est le nom URI de nom délimité par des virgules utilisateur statistique. Par exemple, l’URI suivant informe le service que quatre statistiques sont demandées pour le compte de l’id d’utilisateur spécifié dans l’URI. `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`Il y aura une limite sur le nombre de statistiques qui peuvent être demandés dans un seul appel, et cette limite sera Étudiez attentivement « constitue-t-il un » pour développeur Visual Studio plus de commodité. Faisabilité de longueur d’URI. Par exemple, la limite peut être déterminée par des caractères de 600 à la zone de texte Nom de statistique (y compris les virgules) ou 10 statistiques maximales. L’activation d’une opération GET simple comme celui-ci permet la mise en cache HTTP pour les statistiques fréquemment demandées, ce qui réduit le volume d’appels à partir de clients bavardes. |

<a id="ID4EUC"></a>


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


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : « XBL3.0 x =&lt;userhash > ;&lt; jeton > ».|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nom/numéro du service auquel cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’authentification et ainsi de suite. Valeur par défaut : 1.|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4EHG"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 304| Non modifié| Ressource ne pas été modifié depuis son dernier demandé.|
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.|
| 401| Non autorisé| La demande requiert une authentification utilisateur.|
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.|
| 404| Introuvable| Impossible de trouver la ressource spécifiée.|
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.|
| 408| Expiration du délai de la demande| Version de la ressource n’est pas pris en charge ; doit être rejetée par la couche MVC.|

<a id="ID4E5BAC"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4EECAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "user": {
    "xuid": "123456789",
        "gamertag": "WarriorSaint",
        "stats": [
            {
                "statname": "Wins",
                "type": "Integer",
                "value": 40
            },
            {
                "statname": "Kills",
                "type": "Integer",
                "value": 700
            },
            {
                "statname": "KDRatio",
                "type": "Double",
                "value": 2.23
            },
            {
                "statname": "Headshots",
                "type": "Integer",
                "value": 173
            }
        ],
    }
}

```


<a id="ID4EOCAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EQCAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
