---
title: POST (/titles/{Title Id}/sessionhosts)
assetID: 8558b336-1af9-8143-9752-477ceb3a8e4e
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts-post.html
description: " POST (/titles/{Title Id}/sessionhosts)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47e3ecbf0a519b92ae467199e5d454523864310a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655144"
---
# <a name="post-titlestitle-idsessionhosts"></a>POST (/titles/{Title Id}/sessionhosts)
Créer la nouvelle demande de cluster. Le domaine pour ces URI est `gameserverms.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EX)
  * [En-têtes de demande nécessaires](#ID4EGB)
  * [Corps de la demande](#ID4E5B)
  * [En-têtes de réponse requis](#ID4ELD)
  * [Corps de la réponse](#ID4ESD)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Description| 
| --- | --- | 
| titleId| ID du titre qui doit traiter la demande.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nom d'hôte

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>En-têtes de demande nécessaires
 
Lorsque vous élaborez une demande, les en-têtes affichés dans le tableau suivant sont requis.
 
| En-tête| Valeur| Description| 
| --- | --- | --- | --- | --- | 
| Content-Type| application/json| Type de données en cours de soumission.| 
  
<a id="ID4E5B"></a>

 
## <a name="request-body"></a>Corps de la demande
 
La demande doit contenir un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| C’est l’appelant spécifié identificateur. Elle est affectée à l’hôte de session qui est allouée et retournée. Par la suite, vous pouvez référencer le sessionhost spécifique à cet identificateur. Il doit être globalement unique (par exemple, GUID).| 
| SandboxId| Le bac à sable, vous avez intérêt à allouer dans l’hôte de session.| 
| cloudGameId| L’identificateur de jeu de cloud.| 
| Emplacements| La liste ordonnée des emplacements préférés, vous aimeriez être allouées à partir de la session.| 
| sessionCookie| Il s’agit d’un appelant spécifié chaîne opaque. Il est associé à la sessionhost et peut être référencée dans votre code de jeu. Utilisez ce membre pour passer d’une petite quantité d’informations à partir du client au serveur (taille maximale est de 4 Ko).| 
| gameModelId| L’identificateur du mode de jeu.| 
 
<a id="ID4EDD"></a>

 
### <a name="sample-request"></a>Exemple de demande
 

```cpp
{
        "sessionId": "3536d3e6-e85d-4f47-b898-9617d19dabcd",
        "sandboxId": "ISST.0",
        "cloudGameId": "1b7f9925-369c-4301-b1f7-1125dce25776",
        "locations": [
        "West US",
        "East US",
        "West Europe"
        ],
        "sessionCookie": "Caller provided opaque string",
        "gameModeId": "2162d32c-7ac8-40e9-9b1f-56676b8b2513"
        }
      
```

   
<a id="ID4ELD"></a>

 
## <a name="required-response-headers"></a>En-têtes de réponse requis
 
Aucun.
  
<a id="ID4ESD"></a>

 
## <a name="response-body"></a>Corps de la réponse
 
Si l’appel réussit, le service retourne un objet JSON avec les membres suivants.
 
| Membre| Description| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| hostName| Le nom d’hôte de l’instance.| 
| portMappings| Les mappages de port.| 
| Région| Région de l’instance est hébergée dans.| 
| secureContext| L’adresse du périphérique sécurisé.| 
 
<a id="ID4ESE"></a>

 
### <a name="sample-response"></a>Exemple de réponse
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g=="
        }
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Notes
 
Un titre doit réessayer uniquement l’appel au service lors de la réception des codes de réponse suivants :
 
   * 200 : réussite : réponse renvoyée.
   * 400 : paramètres non valides ou le corps de la demande incorrecte.
   * 401 : non autorisé
   * 404, id de titre n’a pas les abonnements qui lui est assignées.
   * 409, lors de la demande identiques sont apportées (même sessionId) au même moment, cette réponse est possible. Si une demande d’allocation est effectuée et un hôte de session a déjà l’ID de session spécifié et il est déjà Active nous retournera les informations détaillant ce sessionhost. Si l’hôte de session Toutefois, n’est pas Active encore, vous recevrez un conflit.
   * 500 : erreur inattendue du serveur.
   * 503 : aucun sessionhosts StandingBy. Relancez la demande lorsque certains de ces ressources sont gratuits.
   
<a id="ID4EFG"></a>

 
## <a name="see-also"></a>Voir également
 [/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

  