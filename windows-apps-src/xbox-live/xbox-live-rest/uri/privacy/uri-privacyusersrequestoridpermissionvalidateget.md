---
title: GET (/users/{requestorId}/permission/validate)
assetID: 8d22c668-af9a-1d24-8d65-830c2ce913d7
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidateget.html
description: " GET (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 07ccac63b3e6681ea35405314b0cd8b93aa60f9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594754"
---
# <a name="get-usersrequestoridpermissionvalidate"></a>GET (/users/{requestorId}/permission/validate)
Obtient une réponse Oui ou non sur indique si l’utilisateur est autorisé à effectuer l’action spécifiée avec un utilisateur cible.

  * [Paramètres d’URI](#ID4EQ)
  * [Paramètres de chaîne de requête](#ID4E2)
  * [Autorisation](#ID4EDC)
  * [En-têtes de demande nécessaires](#ID4EID)
  * [Corps de la demande](#ID4ETE)
  * [Codes d’état HTTP](#ID4E5E)
  * [En-têtes de réponse requis](#ID4ETG)
  * [Corps de la réponse](#ID4EKAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| requestorId| chaîne| Obligatoire. Identificateur de l’utilisateur qui effectue l’action. Les valeurs possibles sont <code>xuid({xuid})</code> et <code>me</code>. Il doit s’agir d’un utilisateur connecté Exemple de valeur : <code>xuid(0987654321)</code>.|

<a id="ID4E2"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- |
| Paramètre| énumération de chaîne| La valeur PermissionId à vérifier par rapport aux. Exemple de valeur : "CommunicateUsingText".|
| target| chaîne| Identificateur de l’utilisateur dont l’action doit être effectuée. Les valeurs possibles sont <code>xuid({xuid})</code>. Exemples de valeurs : <code>xuid(0987654321)</code>|

<a id="ID4EDC"></a>


## <a name="authorization"></a>Authorization

Revendications d’autorisation utilisées | Revendication| Type| Obligatoire ?| Exemple de valeur|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Xuid| entier signé 64 bits| oui| 1234567890|

<a id="ID4EID"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs : <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemple de valeur : 1.|

<a id="ID4ETE"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 400| La requête n’est pas valide.| Exemples : ID de paramètre incorrect, URI incorrect, etc.|
| 404| L’utilisateur spécifié dans l’URI n’existe pas.| Impossible de trouver la ressource spécifiée.|

<a id="ID4ETG"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| chaîne| Le type MIME du corps de la demande. Exemple de valeur : <code>application/json</code>|
| Content-Length| chaîne| Le nombre d’octets envoyés dans la réponse. Exemple de valeur : 34|
| Cache-Control| chaîne| Poli demande à partir du serveur pour spécifier le comportement de mise en cache. Exemple : <code>no-cache, no-store</code>|

<a id="ID4EKAAC"></a>


## <a name="response-body"></a>Corps de la réponse

Consultez [PermissionCheckResponse (JSON)](../../json/json-permissioncheckresponse.md).

<a id="ID4EWAAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}

```


<a id="ID4EABAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ECBAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [Énumération de PermissionId](../../enums/privacy-enum-permissionid.md)
