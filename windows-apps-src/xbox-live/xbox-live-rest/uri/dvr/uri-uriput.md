---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
description: " PUT (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 862b956e29222bb9d28f98510f13d42fd1a51b6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633114"
---
# <a name="put-uri"></a>PUT (/{uri})
Charger les données d’élément de jeu.
Les domaines pour ces URI sont `gameclipsmetadata.xboxlive.com` et `gameclipstransfer.xboxlive.com`, selon la fonction de l’URI en question.

  * [Remarques](#ID4EX)
  * [Paramètres d’URI](#ID4EQB)
  * [Paramètres de chaîne de requête](#ID4ERC)
  * [En-têtes de demande nécessaires](#ID4EBE)
  * [En-têtes de demande facultatif](#ID4ENG)
  * [Corps de la demande](#ID4EWH)
  * [Codes d’état HTTP](#ID4ECAAC)
  * [En-têtes de réponse requis](#ID4EYEAC)
  * [En-têtes de réponse facultative](#ID4ELHAC)
  * [Corps de la réponse](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Notes

Après le **InitialUploadResponse** est retourné, le chargement est effectué via le **uploadUri** retournée dans cet objet. Le client doit fractionner le fichier dans **expectedBlocks** blocs séquentiels, non supérieurs à 2 Mo. Ils peuvent être téléchargés en parallèle.

Si vous téléchargez le fichier dans les blocs, le serveur retournera un code d’état HTTP accepté (202) pour chaque demande, jusqu'à ce qu’il a reçu des blocs tout attendus, dans ce cas, elle valide tous les blocs comme un seul fichier, retournant créé (201). Dans ce cas, la réponse ne contient pas d’objet et le serveur peut planifier un traitement supplémentaire. En cas d’erreur, un **ServiceErrorResponse** objet peut être retourné avec un code d’état HTTP approprié.

Sur un code d’erreur récupérable, le client doit réessayer en utilisant un mécanisme de temporisation nouvelle tentative standard.

> [!NOTE] 
> Même si un téléchargement se termine correctement, un traitement supplémentaire se produira que peut rejeter le clip pour des raisons non lié à la télécharger ou les métadonnées en complément de processus.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- | --- |
| <b>uri</b>| chaîne| Le <b>uploadUri</b> champ au sein de la <b>InitialUploadResponse</b> objet.|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Paramètre| Type| Description|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| entier non signé 32 bits| Obligatoire si <b>expectedBlocks</b> est défini. Nombre de bloc index de base zéro déterminer le classement de bloc dans le fichier. Par exemple, si <b>expectedBlocks</b> est 7, puis <b>blockNum</b> peut être comprise entre 0 et 6. |
| <b>uploadId</b>| chaîne| Obligatoire. ID opaque dans <b>GameClipsServiceUploadResponse</b> objet.|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>En-têtes de demande nécessaires

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Authorization| chaîne| Informations d’identification pour l’authentification HTTP. Exemples de valeurs <b>Xauth=&lt;authtoken></b>|
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.|
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.|
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.|
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>En-têtes de demande facultatif

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| chaîne| Encodages de compression acceptable. Exemples de valeurs : gzip, deflate, d’identité.|
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple de valeur : « 686897696a7c876b7e ».|

<a id="ID4EWH"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Expression du motif| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| La session a été récupérée.|
| 301| Déplacé de façon permanente| Le service a été déplacé vers un autre URI.|
| 307| Redirection temporaire| Le service a été déplacé vers un autre URI.|
| 400| demande incorrecte| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.|
| 401| Non autorisé| La demande requiert une authentification utilisateur.|
| 403| Interdit| La demande n’est pas autorisée pour l’utilisateur ou le service.|
| 404| Introuvable| Impossible de trouver la ressource spécifiée.|
| 406| Non Acceptable| Version de la ressource n’est pas pris en charge.|
| 408| Expiration du délai de la demande| La demande a pris trop de temps.|
| 410| Effectué| La ressource demandée n’est plus disponible.|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>En-têtes de réponse requis

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| chaîne| Nom/numéro du service Xbox LIVE à laquelle cette demande doit être dirigée de build. La requête sera uniquement être acheminée à ce service après avoir vérifié la validité de l’en-tête, les revendications dans le jeton d’authentification, etc. Exemples : 1, vnext.|
| Content-Type| chaîne| Type MIME du corps de la réponse. Exemple : <b>application/json</b>.|
| Cache-Control| chaîne| Demande poli pour spécifier le comportement de mise en cache.|
| Accept| chaîne| Valeurs acceptables de Content-Type. Exemple : <b>application/json</b>.|
| Retry-After| chaîne| Indique le client d’essayer ultérieurement dans le cas d’un serveur non disponible.|
| Varier| chaîne| Indique comment mettre en cache des réponses à des proxys en aval.|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>En-têtes de réponse facultative

| En-tête| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| chaîne| Utilisé pour l’optimisation du cache. Exemple : « 686897696a7c876b7e ».|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>Corps de la réponse

Aucun objet n’est envoyés dans le corps de la réponse.

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>Parent

[/{uri}](uri-uri.md)
