---
title: Joindre un tournoi
description: Découvrez comment créer une interface utilisateur aux joueurs tournois de votre jeu.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, arena, tournoi, l’expérience utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: 1cdcfc7243463a35eccfaeb13a3b9b92b616952b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613504"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>Joindre un tournoi à l’aide de l’interface utilisateur de domaine

Les API Arena peut fournir votre titre avec les données relatives à l’inscription, archivage et Infos sur l’équipe, mais ils *ne* fournissent les fonctionnalités à *exécuter* les tâches associées. Votre titre est prévu d’utiliser l’interface utilisateur de domaine ou d’un organisateur de tournoi par des tiers (TO), ou pour créer leur propre prise en charge de la gestion de tournoi.

## <a name="xbox-arena-ui-team-formation"></a>Arena Xbox l’interface utilisateur : Formation de l’équipe

L’interface utilisateur de domaine fournit une méthode aux joueurs d’un formulaire ou d’une jointure une équipe. Il n’existe aucune configuration requise pour le titre.

###### <a name="ui-example-create-a-team"></a>Exemple d’interface utilisateur : Créer une équipe

![Former un écran de l’équipe](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>Lors de la constitution d’une équipe, un joueur peut :

* Envoyer des invitations à participer à un ou plusieurs de vos amis ou de membres d’un club.
* Rechercher des membres de l’équipe en publiant une publicité LFG.
* Enregistrer ou annuler l’inscription d’une équipe.
* Supprimer un membre d’une équipe.

## <a name="xbox-arena-ui-registration"></a>Arena Xbox l’interface utilisateur : Inscription

L’interface utilisateur de domaine fournit une méthode permettant aux joueurs d’inscrire une équipe. Il n’existe aucune configuration requise pour le titre.

###### <a name="ui-example-register-a-team"></a>Exemple d’interface utilisateur : Inscrire une équipe

![Inscrire un écran de l’équipe](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>Lorsque vous inscrivez pour un tournoi, un utilisateur peut :

* Annuler l’inscription pour un tournoi *avant* l’inscription a fermé.
* Annuler l’inscription d’une équipe à l’archivage ou lorsque le tournoi est en cours.
* Annuler l’inscription d’une équipe entière. *Notez qu’un utilisateur individuel, en laissant une équipe annule l’inscription de l’équipe entière.*
* Enregistrer comme un capitaine.
* Comprendre les exigences de tournoi et les règles d’engagement avant d’inscrire.
* Recevez une notification que l’inscription a réussi.
* Consultez l’état de tournoi modifier à « enregistré » en temps réel.
* Afficher un aperçu du support à l’heure de que début d’un tournoi.
* Consultez les joueurs qui se sont déjà inscrits pour le tournoi.
* Empêcher l’inscription si elles ne répondent pas aux qualifications tournoi.
* Empêcher l’inscription si un lecteur a été exclu.

> [!div class="nextstepaction"]
> [Engagement de correspondance](arena-ux-match-engagement.md)
