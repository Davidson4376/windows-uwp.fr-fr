---
title: POST (/handles/query?include=relatedInfo)
assetID: 66ecd1fe-24d4-4cd5-256d-8950ac658529
permalink: en-us/docs/xboxlive/rest/uri-handlesqueryincludepost.html
description: " POST (/handles/query?include=relatedInfo)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eb30561c91a085902dd9d79b6c4a2045dc709bfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632554"
---
# <a name="post-handlesqueryincluderelatedinfo"></a>POST (/handles/query?include=relatedInfo)
Crée des requêtes pour les descripteurs de session qui incluent des informations de session associée.

> [!IMPORTANT]
> Cette méthode est utilisée par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4ECB)
  * [Paramètres de chaîne de requête](#ID4EPB)
  * [Codes d’état HTTP](#ID4EAF)
  * [Corps de la demande](#ID4EHF)
  * [Corps de la réponse](#ID4EZF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST crée des requêtes pour les données de handle avec les informations de session spécifiées dans la chaîne de requête « include ». La valeur de chaîne de requête est conçue pour être extensible pour prendre en charge les options de requête futures, prenant en charge une liste délimitée par des virgules, par exemple, « ? inclure = relatedInfo, session ». La méthode POST peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetActivitiesForUsersAsync**.

Le champ de type dans le corps de demande pour cette méthode doit être défini à « activité ». Les éléments dans le corps de réponse mappent directement aux propriétés de la **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

Une requête peut être modifiée en utilisant les paramètres de chaîne de requête dans le tableau suivant.

| <b>Paramètre</b>| <b>Type</b>| <b>Description</b>|
| --- | --- | --- | --- |
| keyword| chaîne| Un mot clé, par exemple, « foo », qui doit se trouver dans des sessions ou des modèles si elles doivent être récupérés. |
| xuid| entier non signé 64 bits| ID de l’utilisateur de Xbox, par exemple, « 123 », pour les sessions à inclure dans la requête. Par défaut, l’utilisateur doit être actif pour qu’il soit inclus dans la session. |
| réservations| booléen| True pour inclure les sessions pour lequel l’utilisateur est défini comme un lecteur réservé mais n’a pas joint pour qu’il devienne un lecteur actif. Ce paramètre est utilisé uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation des sessions de serveur à serveur d’un utilisateur spécifique. |
| inactif| booléen| True pour inclure les sessions de l’utilisateur a accepté mais n’est pas en cours d’exécution. Le nombre de sessions dans lesquelles l’utilisateur est « prêt » mais pas « actif » comme étant inactives. |
| Privé| booléen| True pour inclure les sessions privées. Ce paramètre est utilisé uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation des sessions de serveur à serveur d’un utilisateur spécifique. |
| visibility| chaîne| État de visibilité pour les sessions. Les valeurs possibles sont définies par le <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b>. Si ce paramètre est défini sur « Ouvrir », la requête doit inclure les sessions ouvertes uniquement. Si elle est définie sur « privé », le <i>privé</i> paramètre doit être défini sur true. |
| version| entier signé 32 bits| La version maximale de la session qui doit être incluse. Par exemple, une valeur de 2 spécifie que seules sessions avec une version majeure de session de 2 ou inférieur doit être inclus. Le numéro de version doit être inférieure ou égale à la version mod 100 de la demande contrat. |
| Take| entier signé 32 bits| Le nombre maximal de sessions à récupérer. Ce nombre doit être compris entre 0 et 100. |


Paramètre *privé* ou *réservations* à true requiert un accès au niveau du serveur à la session. Vous pouvez également ces paramètres requièrent que XUID l’appelant prétendent correspondent au filtre XUID dans l’URI. Sinon, le code d’état HTTP/403 est retourné, si toutes ces sessions existent réellement ou non.

<a id="ID4EAF"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4EHF"></a>


## <a name="request-body"></a>Corps de la requête

<a id="ID4ENF"></a>


### <a name="sample-request"></a>Exemple de demande

Pour obtenir toutes les activités de graphique social de « favoris » d’un utilisateur, le corps POST ressemble à ceci :


```cpp
{
  "type": "activity",
  "scid": "B5B1F71F-A328-4371-89E0-C3AD222D0E92"  // optional filter on scid
  "owners": {
     "people": {
       "moniker": "favorites",
       "monikerXuid": "3210"
     }
  }
}

```


<a id="ID4EZF"></a>


## <a name="response-body"></a>Corps de la réponse

Les résultats sont retournés sous forme de tableau de structures de handle, avec un ID unique incorporé dans chaque handle. Les informations de session spécifique sont retournées dans le **relatedInfo** objet. Notez que ces informations ne sont pas retournées pour la méthode POST régulière sur cet URI.

<a id="ID4EDG"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "results": [{
        "id": "11111111-ebe0-42da-885f-033860a818f6",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "party",
            "name": "e3a836aeac6f4cbe9bcab985494d3175"
        },

    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": true,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    },
    {
        "id": "11111111-ebe0-42da-885f-033860a818f7",
        "type": "activity",
        "version": 1,
        "sessionRef": {
            "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
            "templateName": "TitleStorageTestDefault",
            "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
        },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
        "relatedInfo": {
            "visibility": "open",
            "joinRestriction": "followed",
            "closed": false,
            "maxMembersCount": 8,
            "membersCount": 4,
        }
    }]
}

```


<a id="ID4ENG"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EPG"></a>


##### <a name="parent"></a>Parent

[/handles/query](uri-handlesquery.md)
