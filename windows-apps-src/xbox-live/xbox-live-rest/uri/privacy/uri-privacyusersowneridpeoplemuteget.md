---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
description: " GET (/users/{ownerId}/people/mute)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 94e2bf4d04619ffa3348ae08fc37964cdc58e7b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661584"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
Obtient la liste de désactiver le son pour un utilisateur.

  * [Remarques](#ID4EQ)
  * [Paramètres d’URI](#ID4EZ)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EEB)
  * [Autorisation](#ID4ENB)
  * [En-têtes de demande nécessaires](#ID4ESC)
  * [Corps de la demande](#ID4EPE)
  * [Codes d’état HTTP](#ID4E1E)
  * [En-têtes de réponse requis](#ID4E3G)
  * [Corps de la réponse](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Notes

Si une cible est spécifiée, cet URI retourne uniquement cet utilisateur si l’utilisateur est dans la liste Muet, soit vide si l’utilisateur n’est pas.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| ownerId| chaîne| Obligatoire. Identificateur de l’utilisateur dont la ressource est accédée. Les valeurs possibles sont « me », <code>xuid({xuid})</code>, ou gt({gamertag}). Doit être l’utilisateur authentifié. Exemples de valeurs : <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. Taille maximale : aucun. |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource

Aucun.

<a id="ID4ENB"></a>


## <a name="authorization"></a>Authorization

Revendications d’autorisation utilisées | Revendication| Type| Obligatoire ?| Exemple de valeur|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| entier signé 64 bits| oui| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization | chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : <code>Xauth=&lt;authtoken></code>. Taille maximale : aucun.|
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La demande sera uniquement pas acheminée vers ce service après une vérification de la validité de l’en-tête, les revendications dans le jeton d’autorisation et ainsi de suite. Exemples de valeurs : <code>1</code>, <code>vnext</code>. Taille maximale : aucun.|
| Accept| chaîne| Types de contenu qui sont acceptables. Exemple de valeur : <code>application/json</code>. Taille maximale : aucun.|

<a id="ID4EPE"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| Demande réussie pour obtenir la liste muet.|
| 400| demande incorrecte| L’ID cible spécifié dans l’URI n’est pas valide.|
| 403| Interdit| Le propriétaire spécifié dans l’URI n’est pas l’utilisateur authentifié.|
| 404| Introuvable| Le propriétaire spécifié dans l’URI n’existe pas.|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| chaîne| Le type MIME du corps de la demande. Exemple de valeur : <code>application/json</code>|
| Content-Length| chaîne| Le nombre d’octets envoyés dans la réponse. Exemple de valeur : 34|
| Cache-Control| chaîne| Poli demande à partir du serveur pour spécifier le comportement de mise en cache. Exemple : <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>Exemple de réponse

Consultez [UserList](../../json/json-userlist.md).


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EJBAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ELBAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)
