---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
description: " GET (/users/{ownerId}/people/avoid)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 745893b4b975b5fbf64fe76591ec15d18af59d73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641614"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
Obtient la liste Évitez d’un utilisateur.

  * [Remarques](#ID4EQ)
  * [Paramètres d’URI](#ID4EZ)
  * [Autorisation](#ID4EEB)
  * [En-têtes de demande nécessaires](#ID4EJC)
  * [Codes d’état HTTP](#ID4EYD)
  * [En-têtes de réponse requis](#ID4E1F)
  * [Corps de la réponse](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Notes

Si une cible est spécifiée, retourne uniquement cet utilisateur s’ils sont sur la liste rouge, soit vide si elles ne sont pas.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| ownerId| chaîne| Obligatoire. Identificateur de l’utilisateur dont la ressource est accédée. Les valeurs possibles sont <code>xuid({xuid})</code>. Doit être l’utilisateur authentifié. Exemple de valeur : <code>xuid(2603643534573581)</code>. Taille maximale : aucun. |

<a id="ID4EEB"></a>


## <a name="authorization"></a>Authorization

Revendications d’autorisation utilisées | Revendication| Type| Obligatoire ?| Exemple de valeur|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| entier signé 64 bits| oui| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization | chaîne| Informations d’identification pour l’authentification HTTP. Exemple de valeur : <code>Xauth=&lt;authtoken></code>. Taille maximale : aucun.|
| Accept| chaîne| Types de contenu qui sont acceptables. Exemple de valeur : <code>application/json</code>. Taille maximale : aucun.|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 400| demande incorrecte| L’ID cible spécifié dans l’URI n’est pas valide.|
| 403| Interdit| Le propriétaire spécifié dans l’URI n’est pas l’utilisateur authentifié.|
| 404| Introuvable| Le propriétaire spécifié dans l’URI n’existe pas.|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| chaîne| Le type MIME du corps de la demande. Exemple de valeur : <code>application/json</code>. Taille maximale : aucun.|
| Content-Length| chaîne| Le nombre d’octets envoyés dans la réponse. Exemple de valeur : 34. Taille maximale : aucun.|
| Cache-Control| chaîne| Poli demande à partir du serveur pour spécifier le comportement de mise en cache. Exemple de valeur : <code>application/json</code>. Taille maximale : aucun.|

<a id="ID4ESH"></a>


## <a name="response-body"></a>Corps de la réponse

<a id="ID4EYH"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EDAAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EFAAC"></a>


##### <a name="parent"></a>Parent

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)
