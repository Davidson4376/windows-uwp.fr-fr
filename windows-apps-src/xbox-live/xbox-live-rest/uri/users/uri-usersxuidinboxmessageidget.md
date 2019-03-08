---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29b4c57468148a431a10e0d74f85d360ff0992b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618054"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
Récupère le texte de message détaillé pour un message utilisateur particulier, marquer comme étant en lecture sur le service.
Le domaine pour ces URI est `msg.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EEB)
  * [Autorisation](#ID4ERB)
  * [Corps de la demande](#ID4E3B)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EJC)
  * [Codes d’état HTTP](#ID4EUC)
  * [JavaScript Object Notation (JSON) réponse](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

L’opération d’obtention peut uniquement être effectuée sur les types de messages utilisateur, système et FriendRequest.

Cet URI doit être actualisée sur Xbox.com. Actuellement, la Xbox 360 ne met pas à jour l’état lu/non lu jusqu'à ce qu’un utilisateur se déconnecte et reconnectez-vous.

Le type de contenu uniquement que cette API prend en charge est « application/json », qui est requis dans les en-têtes HTTP de chaque appel.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid | entier non signé 64 bits | L’ID d’utilisateur Xbox (XUID) de l’acteur qui effectue la demande. |
| messageId | string[50] | ID du message ne soient récupérées ou supprimées. |

<a id="ID4ERB"></a>


## <a name="authorization"></a>Authorization

Vous devez avoir votre propre utilisateur de revendication extraire un message de l’utilisateur.

<a id="ID4E3B"></a>


## <a name="request-body"></a>Corps de la requête

Aucun objet n’est envoyés dans le corps de cette demande.

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource

Vous pouvez uniquement récupérer vos propres messages utilisateur.

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Description|
| --- | --- | --- | --- | --- |
| 200| Réussite.|
| 400| Le XUID ne peut pas être converti correctement.|
| 403| Le XUID ne peut pas être converti ou une revendication XUID valide est introuvable.|
| 404| Un XUID valide est manquant, ou l’ID de message est introuvable ou est mal analysé.|
| 500| Erreur générale de côté serveur ou le type de message n’est pas valide pour GET.|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON) réponse

Si elle est appelée avec succès, le service retourne les données de résultats au format JSON. L’objet racine est un objet UserMessageHeader.

#### <a name="usermessageheader"></a>UserMessageHeader

| Propriété| Type| Longueur maximale| Notes|
| --- | --- | --- | --- |
| en-tête| En-tête|  | Objet JSON|
| messageText| chaîne| 256| UTF-8|

#### <a name="header"></a>En-tête

| Propriété| Type| Longueur maximale| Notes|
| --- | --- | --- | --- |
| envoyé| DateTime|  | Date et heure que le message a été envoyé. (Fourni par le service).|
| expiration| DateTime|  | Date et heure de qu'expiration du message. (Tous les messages ont une durée de vie maximale est déterminée par la suite.)|
| messageType| chaîne| 13| Types de messages : Utilisateur, système, FriendRequest.|
| senderXuid| ulong|  | XUID de l’expéditeur.|
| expéditeur| chaîne| 15| Identité de l’expéditeur.|
| hasAudio| bool|  | Indique si le message comporte une pièce jointe audio (voix).|
| hasPhoto| bool|  | Indique si le message comporte une pièce jointe de photo.|
| hasText| bool|  | Si le message contienne de texte.|

#### <a name="sample-response"></a>Exemple de réponse

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>Réponse d’erreur

En cas d’erreur, le service peut retourner un objet errorResponse, qui peut contenir des valeurs à partir de l’environnement du service.

| Propriété| Type| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| chaîne| Une indication de d'où provient l’erreur.|
| errorCode| entier| Code numérique associé à l’erreur (peut être null).|
| errorMessage| chaîne| Détails de l’erreur si configuré pour afficher les détails.|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Référence [codes d’état HTTP Standard](../../additional/httpstatuscodes.md)
