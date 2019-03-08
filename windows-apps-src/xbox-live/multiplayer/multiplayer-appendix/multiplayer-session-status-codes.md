---
title: Codes d’état de session multijoueur
description: Décrit les codes d’état renvoyés par le service Xbox Live lorsque vous demandez une session multijoueur.
ms.assetid: 4ab320d6-8050-41a9-9f00-faaad3b128fd
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une multijoueur 2015, les codes d’état, session
ms.localizationpriority: medium
ms.openlocfilehash: 8fbddd0070eb24d6fc050c59fa2a0197f98ee08c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646304"
---
# <a name="multiplayer-session-status-codes"></a>Codes d’état de session multijoueur

Cette rubrique répertorie les codes d’état multijoueur concernant les sessions.

| Remarque                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------|
| Les codes d’état 4xx retournant la session toujours retournent la session entière, même si l’URI pointe vers un élément de session. |


| Code d'état | Chaîne              | Content-Type     | Corps de message    | Description |
|----|
| 200         | OK                  | application/json | session | Lecture (GET) avec succès ou de mise à jour (PUT).                                                                                                                                                                                                                                                                                                             |
| 201         | Créé le             | application/json | session | Créé avec succès.                                                                                                                                                                                                                                                                                                                                 |
| 202         | Accepté            | text/plain       | aucune    | La demande a été acceptée, mais n’a pas encore été effectuée.                                                                                                                                                                                                                                                                                             |
| 204         | Aucun contenu          |                  |         | Sur GET pour une session, la session n’existe pas. Sur l’obtention d’un élément de session, la session existe mais n’est pas le cas de l’élément. PUT pour une session, la session a été supprimée suite à l’opération PUT. PUT ou DELETE pour un élément de session, la session existe lors de l’opération a commencé, mais la session ou l’élément n’existe plus. |
| 304         | Non modifié        |                  |         | Sur GET avec un en-tête If-None-Match, la session n’a pas changé.                                                                                                                                                                                                                                                                                        |
| 400         | Demande incorrecte         | text/plain       | message | La demande est supposée n’est pas valide sur le premier examen. Un champ obligatoire est manquant ou le fichier JSON est incorrect. Le corps inclut des détails supplémentaires.                                                                                                                                                                                        |
| 403         | Interdit           | text/plain       | message | La demande peut être valide dans certains contextes, mais n’est pas valide pour son contexte. Échec de l’autorisation.                                                                                                                                                                                                                                                |
|             |                     | application/json | session | La session ne peut pas être mis à jour par l’utilisateur, mais il peut être lu.                                                                                                                                                                                                                                                                                           |
| 404         | Introuvable           | text/plain       | message | La session n’est pas accessible, car l’URI n’est pas valide ; Impossible de trouver le handle, SCID ou modèle de session ; Impossible de trouver un hopper ; un élément de session n’est pas accessible, car la session ne se ferme pas ; ou la recherche de l’élément n’est pas valide pour la session.                                                                                 |
| 405         | Méthode non autorisée  | text/plain       | message | L’URI de demande est plausible, mais le verbe est incorrect. Par exemple, la demande est une opération POST lorsqu’une opération PUT est nécessaire.                                                                                                                                                                                                                 |
| 409         | Conflit            | text/plain       | message | La session n’a pas pu être mis à jour, car la demande n’est pas compatible avec la session. Par exemple, les constantes dans la demande sont en conflit avec des constantes dans la session ou le modèle de session ou membres autres que l’appelant ont été ajoutés ou supprimés à partir d’une session de grande taille.                                                                         |
| 412         | Échoué de la précondition |                  |         | L’en-tête If-Match ou l’en-tête If-None-Match (pour une opération autre que GET), n’a pas pu être satisfaite.                                                                                                                                                                                                                                           |
|             |                     | application/json | session | L’en-tête If-Match sur une opération PUT ou DELETE pour une session existante n’a pas pu être satisfaite. L’état actuel de la session est retourné avec la valeur ETag actuelle.                                                                                                                                                                      |
| 429 | Trop de demandes | application/json | message | L’appel de service a été limitée en raison du dépassement de la limitation des restrictions du débit affiné. Pour plus d’informations, consultez [bien plus précis limitation du débit](../../using-xbox-live/best-practices/fine-grained-rate-limiting.md). |
| 503         | Service non disponible | text/plain       | aucune    | Le service est surchargé et que la demande doit être retentée ultérieurement. Ce code inclut un en-tête Retry-After qui le client doit respecter.                                                                                                                                                                                                              |
