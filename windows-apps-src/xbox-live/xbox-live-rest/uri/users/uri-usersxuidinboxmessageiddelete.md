---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 80ec2a462648177cc6bfc846b9c84278821b0e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594104"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
Supprime un message de l’utilisateur dans la boîte de réception de l’utilisateur. Le domaine pour ces URI est `msg.xboxlive.com`.
 
  * [Remarques](#ID4EV)
  * [Paramètres d’URI](#ID4ECB)
  * [Autorisation](#ID4EPB)
  * [Corps de la demande](#ID4E1B)
  * [Codes d’état HTTP](#ID4EHC)
  * [JavaScript Object Notation (JSON) réponse](#ID4EAE)
  * [Effet des paramètres de confidentialité sur la ressource](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Notes 
 
L’opération de suppression est idempotent.
 
Le type de contenu uniquement que cette API prend en charge est « application/json », qui est requis dans les en-têtes HTTP de chaque appel. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI 
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid | entier non signé 64 bits | L’ID d’utilisateur Xbox (XUID) de l’acteur qui effectue la demande. | 
| messageId | string[50] | ID du message ne soient récupérées ou supprimées. | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Authorization 
 
Vous devez avoir votre propre utilisateur de revendication supprimer un message de l’utilisateur.
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>Corps de la requête 
 
Aucun objet n’est envoyés dans le corps de cette demande.
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>Codes d’état HTTP 
 
Le service renvoie un des codes d’état de cette section en réponse à une demande faite avec cette méthode sur cette ressource. Pour obtenir une liste complète des codes d’état HTTP standards utilisés avec les Services Xbox Live, consultez [codes d’état HTTP Standard](../../additional/httpstatuscodes.md).
 
| Code| Description| 
| --- | --- | --- | --- | --- | 
| 204| Réussite.| 
| 403| Le XUID ne peut pas être converti ou une revendication XUID valide est introuvable.| 
| 404| ID de message dans l’URI ne peut pas être analysée ou un XUID est manquant dans l’URI.| 
| 500| Erreur générale côté serveur.| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>JavaScript Object Notation (JSON) réponse 
 
En cas d’erreur, le service peut retourner un objet errorResponse, qui peut contenir des valeurs à partir de l’environnement du service.
 
| Propriété| Type| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| chaîne| Une indication de d'où provient l’erreur.| 
| errorCode| entier| Code numérique associé à l’erreur (peut être null).| 
| errorMessage| chaîne| Détails de l’erreur si configuré pour afficher les détails.| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Effet des paramètres de confidentialité sur la ressource 
 
Vous pouvez uniquement supprimer vos propres messages utilisateur. 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Parent  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Référence [codes d’état HTTP Standard](../../additional/httpstatuscodes.md)

   