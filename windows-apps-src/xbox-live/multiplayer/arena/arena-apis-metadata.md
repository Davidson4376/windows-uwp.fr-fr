---
title: Métadonnées de l’interface utilisateur de l’API Arena
description: Contient une liste complète des métadonnées de l’interface utilisateur pour les API d’Arena Xbox.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arena, tournoi, l’expérience utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: 63151d2c584218090f98a3c5f8089bf24bdb9b22
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659764"
---
# <a name="arena-apis-a-comprehensive-list-of-ui-metadata"></a>Arena API : Une liste complète des métadonnées de l’interface utilisateur

Les API Arena retourner les métadonnées en exposant les détails du tournoi, correspondance et le lecteur à l’intérieur d’un jeu. Voici une liste complète.

DÉTAILS DU TOURNOI  | DÉTAILS DE CORRESPONDANCE | DÉTAILS DE L’ÉQUIPE  | DÉTAILS DE L’INSCRIPTION
--- | --- | --- | ---
Nom de l’organisateur | Heure de fin : date/heure de cette tournoi a atteint l’état « Terminé » ou « Annulé ». Cela est défini automatiquement lorsqu’un tournoi est mis à jour. | Correspondance précédente (indique le résultat de jeu de tournoi, classement, heure de fin, heure de début de correspondance, est un Bye, description, opposées ID d’équipe) | État de l’inscription (inconnu, en attente, retirée, rejetée, inscrit, terminé)
Nom du tournoi (128 caractères maximales.) | Est un Bye   | État de l’équipe (unknown, inscrit, waitlisted, mise en veille, archivé, lecture, terminé) | Raison de l’inscription (membre inconnu, fermé, déjà inscrit, tournoi complète, équipe éliminé, tournoi terminé)
Description de tournoi (1000 caractères max.) | États du jeu de résultats tournoi (aucun concours/annulée, win, perte, draw, rang, pas afficher) | La raison de l’équipe est dans un état terminé (rejeté, supprimées, éliminé, terminé) | Inscrit un nombre minimal et maximal d’équipes
Heure de début/fin d’inscription | État de l’arbitrage : Ne terminé, none, annulé, aucune des résultats, les résultats partiels | Date d’inscription, la date et l’heure une équipe a été inscrit. |
Vérifiez dans l’heure de début/fin | Correspond à descriptif 'étiquette' : (« final thermique 1 ») | Permanent de l’équipe | L’inscription n’est ouvert
Heure de début/fin lecture | Heure de début | Nom de l’équipe | Est de vérifier dans Open
A un prix | ID d’équipe contradictoires | Classement final d’équipe | Heure de début/fin d’inscription
Taille de l’équipe min/max | | Continuation URI — ramène à l’interface utilisateur de l’arène des membres de l’équipe | Vérifiez dans l’heure de début/fin
Mode de jeu | | Métadonnées de correspondance actuelle (description, heure de début, est Bye, ID d’équipe opposées) | Nombre d’équipes inscrit
Style de tournoi (élimination unique, tourniquet (Round Robin)) | | Résumé de l’équipe (état de l’équipe, classement) |
L’inscription n’est ouvert | | Identités |
Est de vérifier dans Open | | |
Nombre des équipes | | |
Lecture ouvert | | |
