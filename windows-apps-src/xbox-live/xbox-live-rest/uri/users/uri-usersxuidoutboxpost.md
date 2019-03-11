---
title: POST (/users/xuid({xuid})/outbox)
assetID: de991d88-efe0-04f2-f6b2-0bc3e68bfd46
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutboxpost.html
description: " POST (/users/xuid({xuid})/outbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b225e8441fee3d499f172e2e5701096cdbc161a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594284"
---
# <a name="post-usersxuidxuidoutbox"></a>POST (/users/xuid({xuid})/outbox)
Envoie un message spécifié à une liste de destinataires.
Le domaine pour ces URI est `msg.xboxlive.com`.

  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4EAB)
  * [Autorisation](#ID4ENB)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EYB)
  * [Corps de la demande](#ID4E3F)
  * [Codes d’état HTTP](#ID4ETCAC)
  * [Corps de la réponse](#ID4E1EAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Notes

Le type de contenu uniquement que cette API prend en charge est « application/json », qui est requis dans les en-têtes HTTP de chaque appel.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Paramètres d’URI

| Paramètre| Type| Description|
| --- | --- | --- |
| xuid | entier non signé 64 bits | L’ID d’utilisateur Xbox (XUID) de l’acteur qui effectue la demande. |

<a id="ID4ENB"></a>


## <a name="authorization"></a>Authorization

Vous devez disposer de votre propre revendication d’utilisateur et un abonnement valide gold pour envoyer un message de l’utilisateur.

<a id="ID4EYB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource

Avec succès en envoyant un message de l’utilisateur à un lecteur, si ce lecteur est un ami ou non, entraîne un code de résultat 200. Toutefois, si vous envoyez un message à une personne qui vous a bloqué, le destinataire ne sera pas reçu le message, et vous ne recevrez aucune indication selon que votre message n’a pas été réussi.

Il existe également des limites sur le nombre de messages peut être transmis par jour et à combien leurs amis et non-amis, comme suit.

   * 20 personnes étrangères par message
   * 200 inconnus par 24 heures
   * Nombre total de 250 messages par 24 heures
   * 2500 des destinataires au total 24 heures

| Utilisateur demande| Cibler le paramètre de confidentialité de l’utilisateur| Comportement|
| --- | --- | --- | --- | --- | --- |
| Me| -| Comme décrit.|
| Friend| Tout le monde| 200 OK|
| Friend| amis uniquement| 200 OK|
| Friend| bloqué| 200 OK|
| non friend utilisateur| Tout le monde| 200 OK|
| non friend utilisateur| amis uniquement| 200 OK|
| non friend utilisateur| bloqué| 200 OK|
| site tiers| Tout le monde| 200 OK|
| site tiers| amis uniquement| 200 OK|
| site tiers| bloqué| 200 OK|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Corps de la requête

| Propriété| Type| Longueur maximale| Consommateurs| Notes|
| --- | --- | --- | --- | --- |
| en-tête| En-tête|  | Tous| En-tête de message utilisateur|
| messageText| chaîne| 250| Toutes les plateformes sauf Windows 8| Texte du message utilisateur (UTF-8)|

#### <a name="header"></a>En-tête

| Propriété| Type| Longueur maximale| Consommateurs| Notes|
| --- | --- | --- | --- | --- |
| destinataires| [] Utilisateur| 20| Tous| Liste des destinataires du message|

#### <a name="user"></a>Utilisateur

| Propriété| Type| Longueur maximale| Consommateurs| Notes|
| --- | --- | --- | --- | --- |
| xuid| ulong|  | Tous| XUID du destinataire. Pas utilisé si l’identité est envoyée.|
| Gamertag| chaîne| 15| Tous| Identité du destinataire. Pas utilisé si XUID est envoyé.|

#### <a name="sample-request-body"></a>Exemple de corps de demande 

```cpp
{
          "header":
          {
            "recipients":
            [{"gamertag":"GoTeamEmily"},
            {"gamertag":"Longstreet360"}]
          },
          "messageText":"random user text"
        }

```


<a id="ID4ETCAC"></a>


## <a name="http-status-codes"></a>Codes d’état HTTP

Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).

| Code| Description|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Réussite.|
| 400| Liste de destinataires est vide ou dépasse la longueur maximale ; ou gamertag et XUID ont été spécifiées ; ou messageText est trop long.|
| 403| XUID ne peut pas être converti.|
| 404| Gamertag n’est pas valide ou l’utilisateur ne peut pas être trouvé.|
| 409| Utilisateur a atteint la limite quotidienne imposée par le système.|
| 500| Erreur générale côté serveur.|

<a id="ID4E1EAC"></a>


## <a name="response-body"></a>Corps de la réponse

Aucun objet n’est envoyés dans le corps de la réponse.

<a id="ID4EJFAC"></a>


## <a name="see-also"></a>Voir également

<a id="ID4ELFAC"></a>


##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/outbox](uri-usersxuidoutbox.md)


<a id="ID4EZFAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Référence [codes d’état HTTP Standard](../../additional/httpstatuscodes.md)
