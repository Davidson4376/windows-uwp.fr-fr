---
title: Annexe relative au mode multijoueur
description: Fournit des liens pour en savoir plus sur le service Xbox Live multijoueur 2015.
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 703e48c0f200c59fa6f9181905756ef834aabe4a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643384"
---
# <a name="multiplayer-appendix"></a>Annexe relative au mode multijoueur

Le système en mode multijoueur dans Xbox One permet jeu et l’assembly de joueurs en groupes. Le système est sécurisée, facile à utiliser et flexible, ce qui vous non seulement pour générer des fonctionnalités simples rapidement, mais également de créer des fonctionnalités plus complexes et de brancher vos propres services.

> **Remarque**  
Cet article est pour une utilisation avancée de l’API.  En tant que point de départ, veuillez examiner le [API du Gestionnaire de mode multijoueur](../multiplayer-manager.md) qui simplifie considérablement le développement.  Veuillez informer votre mère si vous recherchez un scénario non pris en charge dans le Gestionnaire de mode multijoueur.

La version actuelle du système en mode multijoueur est 2015 multijoueurs. Cette version résume le concept de « jeu tiers » et utilise l’annuaire de sessions multijoueur (MPSD) pour contrôler les sessions de jeu.

> **Remarque**  
La version précédente du système en mode multijoueur est le mode multijoueur 2014. Cette version est basée sur le concept de la partie jeu et de la participation à des jeux grâce à des parties. Cette version est maintenant déconseillée, bien que le code source pour qu’il est toujours fourni avec le XDK. La documentation multijoueur 2014 n’est plus incluse avec le XDK. Si vous avez besoin de cette documentation, utilisez une version 2014 du XDK.


## <a name="in-this-section"></a>Dans cette section

[Introduction](introduction-to-the-multiplayer-system.md)  
Présente le système multijoueur.

[Annuaire de sessions multijoueur (mpsd)](multiplayer-session-directory.md)  
Décrit les annuaire de sessions multijoueur (MPSD), qui centralise les métadonnées d’API d’un jeu multijoueur sur tous les titres et gère les sessions de jeu.

[Détails de la Session MPSD](mpsd-session-details.md)  
Fournit des détails d’une session MPSD.

[Problèmes courants lors de l’adaptation de vos titres pour le mode multijoueur 2015](common-issues-when-adapting-multiplayer.md)  
Décrit les problèmes courants à prendre en compte lorsque vous adaptez des titres pour fonctionner avec le mode multijoueur 2015.

[SmartMatch Matchmaking](smartmatch-matchmaking.md)  
Décrit le système de matchmaking utilisé par Xbox Live.

[Migration d’un arbitre](migrating-an-arbiter.md)  
Décrit le processus MPSD pour la migration de l’arbitre.

[À l’aide de SmartMatch Matchmaking](using-smartmatch-matchmaking.md)  
Décrit comment utiliser SmartMatch matchmaking.

[Comment multijoueur de](multiplayer-how-tos.md)  
Fournit des procédures pour l’utilisation du mode multijoueur 2015 dans les titres.

[Codes d’état de Session multijoueurs](multiplayer-session-status-codes.md)  
Définit les codes d’état de session multijoueur pour Xbox One.

[FAQ multijoueur 2015 et la résolution des problèmes](multiplayer-2015-faq.md)  
Définit des FAQ et résolution des problèmes pour le mode multijoueur.

[Xbox One annuaire de sessions multijoueur](xbox-one-multiplayer-session-directory.md) fournit une vue d’ensemble de la création d’une session multijoueur à l’aide du nouveau service Xbox un multijoueur Session Directory (MPSD).

[Flux pour multijoueur invite](flows-for-multiplayer-game-invites.md) décrit le flux des expériences impliqués dans l’invitation d’un autre lecteur à un jeu multijoueur.

[Session de jeu et le jeu de la visibilité et joinability tiers](game-session-and-game-party-visibility-and-joinability.md) décrit les différences entre la visibilité et joinability comme se rapporte à une session multijoueur.
