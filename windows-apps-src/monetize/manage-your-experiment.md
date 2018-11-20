---
author: Xansky
Description: After you define your experiment in Partner Center and code your experiment in your app, you are ready to active your experiment and use Partner Center to review the results of your experiment.
title: Gérer votre expérience dans l’Espace partenaires
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, MicrosoftStore Services SDK, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: 9d1cdb80a2278850f18cecc631fef0b5dff0fefc
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7279901"
---
# <a name="manage-your-experiment-in-partner-center"></a>Gérer votre expérience dans l’Espace partenaires

Après vous [Définissez votre expérience dans l’espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md) et le [code de votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md), vous êtes prêt à activer votre expérience et à utiliser l’espace partenaires pour passer en revue les résultats de votre expérience. Après avoir obtenu toutes les données dont vous avez besoin, vous pourrez mettre fin à votre expérience et décider si vous souhaitez continuer à utiliser les valeurs des variables dans la variante de contrôle pour toutes vos applications, ou si vous voulez utiliser les valeurs des variables dans l’une de vos autres variantes.

> [!NOTE]
> Lorsque vous activez une expérience, l’espace partenaires lance immédiatement la collecte de données de toutes les applications consignant des données pour votre expérience. Toutefois, il peut prendre plusieurs heures pour les données d’expérience s’affiche dans l’espace partenaires.

Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Activer votre expérience

Lorsque vous êtes satisfait des paramètres de votre expérience dans l’espace partenaires et que vous avez mis à jour le code de votre application, vous êtes prêt à activer votre expérience, afin de pouvoir commencer à collecter les données correspondantes à partir de votre application. Quand l’expérience est active, votre application peut récupérer les valeurs de variante et signaler des événements d’affichage et de conversion vers l’espace partenaires.

1. Connectez-vous à l'[Espace partenaires](https://partner.microsoft.com/dashboard).
2. Sous **Vos applications**, sélectionnez l’application présentant l’expérience que vous souhaitez activer.
3. Dans le volet de navigation, sélectionnez **Services**, puis **Expérimentation**.
4. Dans la table des projets de la section **Projets**, développez le projet qui contient votre expérience, puis effectuez l’une des opérations suivantes:
  * Cliquez sur le lien **Activer** lien correspondant à votre expérience. Votre expérience est ajoutée à la section **Active experiments** (Expériences actives) en haut de la page.
  * Cliquez sur le nom de l’expérience, faites défiler la page de l’expérience vers le bas, puis cliquez sur **Activer**.

> [!IMPORTANT]
> Une fois que vous avez activé une expérience, vous ne pouvez plus en modifier les paramètres, sauf si vous avez cliqué sur la case **Editable experiment (Expérience modifiable)** quand vous avez créé l’expérience. Nous vous recommandons de coder l’expérience dans votre application avant d’activer votre expérience.

## <a name="review-the-results-of-your-experiment"></a>Passer en revue les résultats de votre expérience

1. Dans l’espace partenaires, revenez à la page **d’expérimentation** pour votre application.
2. Dans la section **Active experiments** (Expériences actives), cliquez sur le nom de votre expérience active pour accéder à la page correspondante.
3. Dans le cas d’une expérience active ou terminée, les deux premières sections de cette page fournissent les résultats de votre expérience:
  * La section **Résumé des résultats** répertorie les objectifs de votre expérience et le taux de conversion pour chaque variante.
  * La section **Détails des résultats** fournit des informations supplémentaires sur chacun des objectifs de votre expérience, notamment les vues, les conversions, les utilisateurs uniques, le taux de conversion, le pourcentage d’écart, la confiance et l’importance. La *confiance* est une mesure statistique de la fiabilité d’une estimation, qui calcule la marge d’erreur. L’*importance* est une mesure statistique, reposant sur la taille de l’échantillon, qui détermine la probabilité qu’un résultat ne soit pas dû au hasard, mais qu’il soit plutôt attribué à une cause spécifique.

> [!NOTE]
> L’espace partenaires signale uniquement le premier événement de conversion pour chaque utilisateur sur une période de 24 heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application au cours d’une période de 24heures, seul le premier événement de conversion est signalé. Cette approche est destinée à éviter qu’un utilisateur unique avec de nombreux événements de conversion ne fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs.


## <a name="complete-your-experiment"></a>Terminer votre expérience

1. Dans l’espace partenaires, revenez à la page de votre expérience. Pour obtenir les instructions correspondantes, voir la section précédente.
2. Dans la section **Résumé des résultats**, effectuez l’une des opérations suivantes:
  * Si vous souhaitez mettre fin à l’expérience et continuer à utiliser les valeurs des variables dans la variante de contrôle de votre application, cliquez sur **Conserver**.
  * Si vous souhaitez mettre fin à l’expérience, mais utiliser les valeurs des variables dans une autre variante de votre application, cliquez sur **Basculer** sous la variante vers laquelle vous voulez basculer.
3. Cliquez sur **OK** pour confirmer que vous souhaitez mettre fin à l’expérience.


## <a name="related-topics"></a>Rubriques connexes

* [Créez un projet et définir des variables distantes dans l’espace partenaires](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans l’Espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)
