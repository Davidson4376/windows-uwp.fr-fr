---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
description: " GET (/users/xuid({xuid})/inbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 05f75510f15f6e6c5f1b1673673428c00f7a6c16
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632244"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
Récupère un nombre spécifié de résumés de message utilisateur à partir du service.
Le domaine pour ces URI est `msg.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EEB)
  * [Paramètres de chaîne de requête](#ID4EIC)
  * [Autorisation](#ID4EGE)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4ETE)
  * [Codes d’état HTTP](#ID4E5E)
  * [JavaScript Object Notation (JSON) réponse](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

Un résumé des messages utilisateur contient uniquement l’objet du message. Pour les messages générés par l’utilisateur, il est actuellement les 20 premiers caractères du texte du message. Messages de système peuvent fournir un substitution du sujet, telles que « Système LIVE ».

Les messages sont retournés dans l’ordre inverse de l’ordre qu’ils ont été envoyés ; Autrement dit, les messages plus récente sont renvoyées en premier.

Le type de contenu uniquement que cette API prend en charge est « application/json », qui est requis dans les en-têtes HTTP de chaque appel.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid| entier non signé 64 bits| L’ID d’utilisateur Xbox (XUID) de l’acteur qui effectue la demande.|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>Paramètres de chaîne de requête

| Propriété| Type| Longueur maximale| Notes|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| entier| 100| Nombre maximal de messages retournés.|
| continuationToken| chaîne|  | Chaîne retournée dans un appel précédent de l’énumération ; permet de continuer l’énumération.|
| skipItems| entier| 100| Nombre de messages à ignorer ; ignoré si continuationToken est présent.|

<a id="ID4EGE"></a>


## <a name="authorization"></a>Authorization

Vous devez avoir votre propre utilisateur de revendication récupérer une synthèse de message utilisateur.

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource

Vous pouvez uniquement énumérer vos propres messages utilisateur.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| La demande a réussi.|
| 400| Service n’a pas pu comprendre demande incorrectement formée. En général, un paramètre non valide.|
| 403| La demande n’est pas autorisée pour l’utilisateur ou le service.|
| 404| Il manque un XUID valide dans l’URI.|
| 409| La collection sous-jacente modifiée en fonction du jeton de continuation qui a été passé.|
| 416| Le nombre d’éléments à ignorer est supérieur au nombre d’éléments disponibles.|
| 500| Erreur générale côté serveur.|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON) réponse

Si elle est appelée avec succès, le service retourne les données de résultats au format JSON.

| Propriété| Type| Longueur maximale| Notes|
| --- | --- | --- | --- |
| résultats| Message[]| 100| Tableau de messages de l’utilisateur|
| pagingInfo| PagingInfo|  | La pagination des informations pour le jeu de résultats actuel|

#### <a name="message"></a>Message

| Propriété| Type| Longueur maximale| Notes|
| --- | --- | --- | --- |
| en-tête| En-tête|  | En-tête de message utilisateur|
| messageSummary| chaîne| 20| UTF-8 ; généralement 20 premiers caractères du message|

#### <a name="header"></a>En-tête

| Propriété| Type| Longueur maximale| Notes|
| --- | --- | --- | --- |
| id| chaîne| 50| Identificateur de message, utilisé pour la récupération des détails du message ou la suppression des messages.|
| isRead| bool|  | Indicateur qui spécifie si l’utilisateur a déjà lu les détails du message.|
| envoyé| DateTime|  | Date et heure UTC que le message a été envoyé. (Fourni par le service).|
| expiration| DateTime|  | Date et heure UTC que le message expire. (Tous les messages ont une durée de vie maximale est déterminée par la suite.)|
| messageType| chaîne| 50| Types de messages : User, System, FriendRequest, Video, QuickChat, VideoChat, PartyChat, Title, GameInvite.|
| senderXuid| ulong|  | XUID de l’expéditeur.|
| expéditeur| chaîne| 15| Identité de l’expéditeur.|
| hasAudio| bool|  | Indique si le message comporte une pièce jointe audio (voix).|
| hasPhoto| bool|  | Indique si le message comporte une pièce jointe de photo.|
| hasText| bool|  | Si le message contienne de texte.|

#### <a name="paging-info"></a>Informations de pagination

| Propriété| Type| Longueur maximale| Notes|
| --- | --- | --- | --- |
| continuationToken| chaîne| 100| Si vous le souhaitez, retourné par serveur. Autorise les appels ultérieurs à continuer l’énumération.|
| totalItems| entier|  | Nombre total de messages dans la boîte de réception.|

#### <a name="sample-response"></a>Exemple de réponse

```cpp
{
          "results":
          [
            {
              "header":
              {
                "expiration":"2011-10-11T23:59:59.9999999",
                "id":"opaqueBlobOfText",
                "messageType":"User",
                "isRead":false,
                "senderXuid":"123456789",
                "sender":"Striker",
                "sent":"2011-05-08T17:30:00Z",
                "hasAudio":false,
                "hasPhoto":false,
                "hasText":true
              },
            "messageSummary":"first 20 chars"
          },
          ...
        ],
        "pagingInfo":
          {
          "continuationToken":"opaqueBlobOfText"
          "totalItems":5,
          }
        }

```

#### <a name="error-response"></a>Réponse d’erreur

En cas d’erreur, le service peut retourner un objet errorResponse, qui peut contenir des valeurs à partir de l’environnement du service.

| Propriété| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| chaîne| Une indication de d'où provient l’erreur.|
| errorCode| entier| Code numérique associé à l’erreur (peut être null).|
| errorMessage| chaîne| Détails de l’erreur si configuré pour afficher les détails.|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Référence [codes d’état HTTP Standard](../../additional/httpstatuscodes.md)
