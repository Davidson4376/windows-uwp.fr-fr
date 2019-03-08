---
title: GameSessionSummary (JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
description: " GameSessionSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8dace19404ae7c8b1d1ef296a21c874e4dd14c6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613384"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary (JSON)
Un objet JSON représentant les données de synthèse pour une session de jeu. 
<a id="ID4EN"></a>

  
 
L’objet JSON de GameSessionSummary a la spécification suivante.
 
| Membre| Type| Description| 
| --- | --- | --- | 
| creationTime| DateTime| La date et l’heure lorsque la session a été créée, au format UTC. | 
| customData| tableau d’entiers non signés 8 bits| 1 024 octets des données de session de jeu spécifique. Cette valeur est opaque pour le serveur. | 
| displayName| chaîne| La session de nom du jeu complet, avec une longueur maximale de 128 caractères. Cette valeur est opaque pour le serveur. | 
| hasEnded| Valeur booléenne| True si la session s’est terminée et false sinon. Définition de ce champ à la valeur true marque la session de jeu en lecture seule, empêchant de données supplémentaires soumises à la session. | 
| sessionId| chaîne de l’ID de session. | 
| titleId| entier non signé 32 bits| L’ID du titre de la création de la session de jeu.| 
| Type Variant| entier signé 32 bits| La variante de jeu. Cette valeur est opaque pour le serveur.| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>Exemple de syntaxe JSON
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "hasEnded": false,
}
    
```

  
<a id="ID4ERD"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4ETD"></a>

 
##### <a name="parent"></a>Parent 

[Référence d’objet JavaScript Objet Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Référence 

[GameSession (JSON)](json-gamesession.md)

   