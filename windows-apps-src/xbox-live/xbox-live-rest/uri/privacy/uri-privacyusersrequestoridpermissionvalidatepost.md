---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
description: " POST (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: edd91560ffb5d81b30da4b1453612cc5853a456f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623424"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
Obtient un ensemble de réponses Oui ou non sur indique si l’utilisateur est autorisé à effectuer les actions spécifiées avec un ensemble d’utilisateurs cibles.

  * [Remarques](#ID4EQ)
  * [Paramètres d’URI](#ID4ECB)
  * [Autorisation](#ID4ENB)
  * [En-têtes de demande nécessaires](#ID4ESC)
  * [Corps de la demande](#ID4E4D)
  * [Codes d’état HTTP](#ID4ETE)
  * [En-têtes de réponse requis](#ID4EIG)
  * [Corps de la réponse](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Notes

Le corps de la requête prend une liste d’utilisateurs et une liste de paramètres, et le résultat est un résultat autorisé/bloqué pour chaque paire/paramètre utilisateur.

Dans le réseau des scénarios multijoueurs (où les vérifications de communications confidentialité doivent être effectuées entre et les utilisateurs qui ont un ID d’utilisateur Xbox (XUID) hors réseau qui ne le faites pas), reportez-vous à [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md) pour les types de l’utilisateur.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| requestorId| chaîne| Obligatoire. Identificateur de l’utilisateur qui effectue l’action. Les valeurs possibles sont <code>xuid({xuid})</code> et <code>me</code>. Il doit s’agir d’un utilisateur connecté Exemple de valeur : <code>xuid(0987654321)</code>.|

<a id="ID4ENB"></a>


## <a name="authorization"></a>Authorization

Revendications d’autorisation utilisées | Revendication| Type| Obligatoire ?| Exemple de valeur|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| entier signé 64 bits| oui| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs : <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemple de valeur : 1.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>Corps de la requête

<a id="ID4EDE"></a>


### <a name="required-members"></a>Membres requis

Consultez [PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md).


```cpp
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ViewTargetGameHistory",
        "ViewTargetProfile"
    ]
}

```


<a id="ID4ETE"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 400| La requête n’est pas valide.| Exemples : ID de paramètre incorrect, URI incorrect, etc.|
| 404| L’utilisateur spécifié dans l’URI n’existe pas.| Impossible de trouver la ressource spécifiée.|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| chaîne| Le type MIME du corps de la demande. Exemple de valeur : <code>application/json</code>|
| Content-Length| chaîne| Le nombre d’octets envoyés dans la réponse. Exemple de valeur : 34|
| Cache-Control| chaîne| Poli demande à partir du serveur pour spécifier le comportement de mise en cache. Exemple : <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>Corps de la réponse

Consultez [PermissionCheckBatchResponse (JSON)](../../json/json-permissioncheckbatchresponse.md).

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>Exemple de réponse


```cpp
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRest", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}

```


<a id="ID4EVAAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EXAAC"></a>


##### <a name="parent"></a>Parent

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [Énumération de PermissionId](../../enums/privacy-enum-permissionid.md)
