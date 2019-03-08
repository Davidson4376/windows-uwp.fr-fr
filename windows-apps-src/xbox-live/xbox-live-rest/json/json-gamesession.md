---
title: GameSession (JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
description: " GameSession (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca7276ccdc13d896d19873811b4fa9df9a831cd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646514"
---
# <a name="gamesession-json"></a>GameSession (JSON)
Un objet JSON qui représente les données du jeu d’une session multijoueur. 
<a id="ID4ER"></a>

  
 
L’objet JSON de GameSession a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| creationTime| DateTime| La date et l’heure lorsque la session a été créée, au format UTC. | 
| customData| tableau d’entiers non signés 8 bits| 1 024 octets des données de session de jeu spécifique. Cette valeur est opaque pour le serveur. | 
| displayName| chaîne| La session de nom du jeu complet, avec une longueur maximale de 128 caractères. Cette valeur est opaque pour le serveur. | 
| hasEnded| Valeur booléenne| True si la session s’est terminée et false sinon. Définition de ce champ à la valeur true marque la session de jeu en lecture seule, empêchant de données supplémentaires soumises à la session. | 
| IsClosed| Valeur booléenne| True si la session est fermée, et aucuns plus de joueurs ne peuvent être ajoutées et false sinon. Si cette valeur est true, les demandes pour rejoindre la session sont rejetées. | 
| maxPlayers| entier signé 32 bits| Nombre maximal de lecteurs qui peut être simultanément dans la session. La plage de valeurs est 2-16. Une fois que la session contient le nombre maximal de lecteurs, plus les demandes pour rejoindre la session sont rejetées. | 
| playersCanBeRemovedBy| PlayerAcl| Une valeur qui indique le lecteur qui est autorisé à supprimer d’autres joueurs à partir de la session. Les valeurs possibles sont cliquez sur Self et AnyPlayer. | 
| liste| tableau d’objets de lecteur| Un tableau de lecteurs dans la session. La liste contient des lecteurs en cours et qui ont été précédemment dans la session, mais qui ont quitté. L’ordre des lecteurs dans la liste ne change jamais. Nouveaux lecteurs sont ajoutés à la fin du tableau. | 
| seatsAvailable| entier signé 32 bits| Le nombre de joueurs qui peuvent toujours rejoindre la session avant que le nombre maximal de lecteurs soit atteinte. Cette valeur est en lecture seule et est toujours inférieure à la valeur du champ maxPlayers. | 
| sessionId| chaîne| L’ID de session attribué par le MPSD lors de la session est créée. Cette valeur est généralement incluse dans l’URI lors de l’accès aux informations stockées dans une session.| 
| titleId| entier non signé 32 bits| L’ID du titre de la création de la session de jeu.| 
| Type Variant| entier signé 32 bits| La variante de jeu. Cette valeur est opaque pour le serveur.| 
| visibility| VisibilityLevel| Une valeur qui indique la visibilité de la session. Les valeurs possibles sont : PlayersCurrentlyInSession PlayersEverInSession et tout le monde.| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "maxPlayers": 4,
    "seatsAvailable": 4,
    "isClosed": false,
    "hasEnded": false,
    "roster": [],
    "visibility": "PlayersCurrentlyInSession",
    "playersCanBeRemovedBy": "Self",
 }
    
```

  
<a id="ID4ENF"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EPF"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>Référence 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [Lecteur JSON)](json-player.md)

   