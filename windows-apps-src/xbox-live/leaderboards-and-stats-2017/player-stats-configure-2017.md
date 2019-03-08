---
title: Configurer des statistiques et classements 2017
description: Découvrez comment configurer des statistiques de proposées Xbox Live et les classements dans partenaires avec 2017 de plateforme de données.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ea2baf4bc27e6d1cfd5beb9ef0386acda72a39d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598854"
---
# <a name="configuring-featured-stats-or-leaderboards-in-partner-center-with-data-platform-2017"></a>Configuration des statistiques proposées ou des classements dans partenaires avec une plateforme de données 2017

Avec 2017 de plateforme de données, il vous suffit de configurer un stat dans deux cas :

* La statistique est utilisée comme base pour un classement global, ou
* La statistique est une statistique de lecteur proposée qui s’affichera sur la page hub du jeu.

Dans les deux cas, vous devez configurer ensemble statistiques et classements. Chaque classement est basé sur un état, et vous ne pouvez pas configurer un stat sans configurer également un classement global associé, même si vous ne souhaitez pas utiliser un classement pour un stat lecteur proposée.

Vous n’avez pas besoin configurer les statistiques qui ne sont pas proposées statistiques et ne sont pas utilisés par un classement global.

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>Configurer un classement global et un stat lecteur associé

Tout d’abord, vous accédez à la section de statistiques pour votre titre comme indiqué ci-dessous.

![](../images/omega/dev_center_player_stats_creators.png)

Puis cliquez sur le `Create your first featured stat/leaderboard` bouton et puis remplissez cette boîte de dialogue et cliquez sur Enregistrer.

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

Le `Display name` champ est ce que les utilisateurs voient dans le GameHub.  Cette chaîne peut être localisée dans la `Localize strings` section.  Cliquez sur `Show Options` dans la `Localize strings` section pour afficher les détails sur la façon de localiser ces chaînes.

Le `ID` champ est le nom stat et sera la façon dont vous ferez référence à votre stat lors de la mise à jour à partir de votre code de titre.   Consultez le [la mise à jour des statistiques](player-stats-updating.md) section ci-dessous pour plus de détails.

Le `Format` est le format de données de la statistique.

Le `Display Logic` offre le choix entre `Player progression`, `Personal best`, et `Counter`:
- Progression du lecteur : Représente un niveau lecteur individuels ou progression dans le jeu.  La dernière valeur définie est ce que voient les utilisateurs.  Par exemple, placer à cycle en cours.
- Meilleur personnel : Représente le meilleur score actuel, qu'un lecteur a publié. La valeur maximum ou minimum définie en fonction de l’ordre de tri est ce que voient les utilisateurs.  Par exemple, le plus rapide présentation.
- Compteur : Peut être ajouté aux autres pour calculer un nombre cumulatif du joueur.  

Le `Sort` champ vous permet de modifier l’ordre de tri du classement.

Vous pouvez également choisir de rendre cette statistique un `Featured Stat`, mais en cliquant sur `Feature on players' profiles`.  

## <a name="configure-featured-stats"></a>Configurer des statistiques en vedette

Lorsque vous définissez un stat player, vous avez la possibilité de vérifier `Featured Stat`.  Veuillez noter les exigences suivantes

| Type de développeur | Condition requise |
|----------------|-------------|
| Programme Créateurs Xbox Live | Il n’existe aucune exigence pour indiquer que les statistiques des statistiques proposées.  Bien que vous êtes limité à un maximum de 10 |
| ID@Xbox et les partenaires Microsoft | Vous devez désigner entre 3 et 10 statistiques proposées |

## <a name="next-steps"></a>Étapes suivantes

Ensuite, vous devez mettre à jour la statistique à partir du code du titre.  Consultez [la mise à jour des statistiques](player-stats-updating.md) pour plus de détails.