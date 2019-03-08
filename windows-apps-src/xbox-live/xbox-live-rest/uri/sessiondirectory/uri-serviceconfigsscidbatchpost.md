---
title: POST (/serviceconfigs/{scid}/batch)
assetID: b821a6eb-1add-ef91-bdf5-10e107082197
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatchpost.html
description: " POST (/serviceconfigs/{scid}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e718fda26264667a7bf08c9254a462eb268dcd74
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603444"
---
# <a name="post-serviceconfigsscidbatch"></a>POST (/serviceconfigs/{scid}/batch)
Crée une requête par lot sur plusieurs Xbox ID utilisateur pour la configuration du service.

> [!IMPORTANT]
> Cette méthode est utilisée par le mode multijoueur 2015 et s’applique uniquement à cette version multijoueur et versions ultérieures. Il est prévu pour une utilisation avec le contrat de modèle 104/105 ou version ultérieure et requiert un élément d’en-tête de Version X-Xbl-contrat : 104/105 ou version ultérieure sur chaque demande.

  * [Remarques](#ID4ET)
  * [Paramètres d’URI](#ID4ELB)
  * [Paramètres de chaîne de requête](#ID4EVB)
  * [Codes d’état HTTP](#ID4EGF)
  * [Corps de la demande](#ID4ENF)
  * [Corps de la réponse](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Notes

Cette méthode HTTP/REST crée des requêtes par lots sur plusieurs Xbox ID utilisateur à la configuration du service au niveau du code (SCID). Cette méthode peut être encapsulée par **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**.

Pour le mode multijoueur 2015, vous pouvez combiner plusieurs requêtes en un seul appel si toutes les requêtes sont le même mais affaire à des ID utilisateur différents Xbox (XUIDs), comme représenté dans le *xuid* paramètre de chaîne de requête. Notez que les paramètres de chaîne de requête et les réponses, sont les mêmes pour les requêtes régulières et des requêtes par lots.

Pour une requête par lot, les sessions appartenant à chaque XUID sont écrits dans la réponse dans le même ordre que les *xuid* paramètres sont présentés dans la demande. Il est possible pour la même session d’apparaître de plusieurs fois dans la réponse, une fois pour chaque *xuid* auquel il correspond.

<a id="ID4ELB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| scid| GUID| Identificateur de configuration de service (SCID). Partie 1 de l’identificateur de session.|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

Une requête peut être modifiée en utilisant les paramètres de chaîne de requête dans le tableau suivant.

| <b>Paramètre</b>| <b>Type</b>| <b>Description</b>|
| --- | --- | --- | --- | --- | --- | --- |
| keyword| chaîne| Un mot clé, par exemple, « foo », qui doit se trouver dans des sessions ou des modèles si elles doivent être récupérés. |
| xuid| entier non signé 64 bits| ID de l’utilisateur de Xbox, par exemple, « 123 », pour les sessions à inclure dans la requête. Par défaut, l’utilisateur doit être actif pour qu’il soit inclus dans la session. |
| réservations| booléen| True pour inclure les sessions pour lequel l’utilisateur est défini comme un lecteur réservé mais n’a pas joint pour qu’il devienne un lecteur actif. Ce paramètre est utilisé uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation des sessions de serveur à serveur d’un utilisateur spécifique. |
| inactif| booléen| True pour inclure les sessions de l’utilisateur a accepté mais n’est pas en cours d’exécution. Le nombre de sessions dans lesquelles l’utilisateur est « prêt » mais pas « actif » comme étant inactives. |
| Privé| booléen| True pour inclure les sessions privées. Ce paramètre est utilisé uniquement lors de l’interrogation de vos propres sessions, ou lors de l’interrogation des sessions de serveur à serveur d’un utilisateur spécifique. |
| visibility| chaîne| État de visibilité pour les sessions. Les valeurs possibles sont définies par le <b>MultiplayerSessionVisibility</b>. Si ce paramètre est défini sur « Ouvrir », la requête doit inclure les sessions ouvertes uniquement. Si elle est définie sur « privé », le <i>privé</i> paramètre doit être défini sur true. |
| version| entier signé 32 bits| La version maximale de la session qui doit être incluse. Par exemple, une valeur de 2 spécifie que seules sessions avec une version majeure de session de 2 ou inférieur doit être inclus. Le numéro de version doit être inférieure ou égale à la version mod 100 de la demande contrat. |
| Take| entier signé 32 bits| Le nombre maximal de sessions à récupérer. Ce nombre doit être compris entre 0 et 100. |


Paramètre *privé* ou *réservations* à true requiert un accès au niveau du serveur à la session. Vous pouvez également ces paramètres requièrent que XUID l’appelant prétendent correspondent au filtre XUID dans l’URI. Sinon, le code d’état HTTP/403 est retourné, si toutes ces sessions existent réellement ou non.

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP
Le service retourne un code d’état HTTP tel qu’il s’applique à MPSD.  
<a id="ID4ENF"></a>


## <a name="request-body"></a>Corps de la requête


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>Corps de la réponse

Le retour à partir de cette méthode est un tableau JSON des références de session, avec certaines données incluses session en ligne.


```cpp
{
    "results": [ {
      "xuid": "9876",    // If the session was found from a xuid, that xuid.
      "startTime": "2009-06-15T13:45:30.0900000Z",
      "sessionRef": {
        "scid": "foo",
        "templateName": "bar",
        "name": "session-seven"
      },
      "accepted": 4,    // Approximate number of non-reserved members.
      "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
      "visibility": "open",    // or "private", "visible", or "full"
      "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
      "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
      "keywords": [ "one", "two" ]
    }
  ]
}

```


<a id="ID4EDG"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EFG"></a>


##### <a name="parent"></a>Parent

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)
