---
author: mcleanbyron
Description: "Après avoir défini votre expérience dans le tableau de bord du Centre de développement et codé cette expérience dans votre application, vous voici prêt à activer l’expérience et à en visualiser les résultats dans le tableau de bord du Centre de développement."
title: "Gérer votre expérience dans le tableau de bord du Centre de développement"
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Microsoft Store Services SDK, tests A/B, expériences"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: bc73d2b63b94f9700fc5013d3ea51a92cabd7ca3
ms.lasthandoff: 02/07/2017

---

# <a name="manage-your-experiment-in-the-dev-center-dashboard"></a>Gérer votre expérience dans le tableau de bord du Centre de développement

Après avoir [défini votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md) et [codé votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md), vous voici prêt à activer votre expérience et à en examiner les résultats dans le tableau de bord du Centre de développement. Après avoir obtenu toutes les données dont vous avez besoin, vous pourrez mettre fin à votre expérience et décider si vous souhaitez continuer à utiliser les valeurs des variables dans la variante de contrôle pour toutes vos applications, ou si vous voulez utiliser les valeurs des variables dans l’une de vos autres variantes.

> **Remarque**&nbsp;&nbsp; Lorsque vous activez une expérience, le Centre de développement lance immédiatement la collecte de données de toutes les applications consignant des données pour votre expérience. L’apparition des données de l’expérience dans le tableau de bord peut cependant prendre plusieurs heures.

Pour découvrir une procédure pas à pas illustrant le processus de création et d’exécution d’une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Activer votre expérience

Une fois que vous êtes satisfait des paramètres de votre expérience dans le tableau de bord et que vous avez mis à jour le code de votre application, vous êtes prêt à activer l’expérience afin de commencer à collecter les données correspondantes à partir de votre application. Quand l’expérience est active, votre application peut récupérer les valeurs de variante, et signaler les événements d’affichage et de conversion au Centre de développement.

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview).
2. Sous **Vos applications**, sélectionnez l’application présentant l’expérience que vous souhaitez activer.
3. Dans le volet de navigation, sélectionnez **Services**, puis **Expérimentation**.
4. Dans la table des projets de la section **Projets**, développez le projet qui contient votre expérience, puis effectuez l’une des opérations suivantes :
  * Cliquez sur le lien **Activer** lien correspondant à votre expérience. Votre expérience est ajoutée à la section **Active experiments** (Expériences actives) en haut de la page.
  * Cliquez sur le nom de l’expérience, faites défiler la page de l’expérience vers le bas, puis cliquez sur **Activer**.

> **Important**&nbsp;&nbsp;Une fois que vous avez activé une expérience, vous ne pouvez plus en modifier les paramètres, sauf si vous avez coché la case **Editable experiment** (Expérience modifiable) quand vous avez créé l’expérience. Nous vous recommandons de coder l’expérience dans votre application avant d’activer votre expérience.


## <a name="review-the-results-of-your-experiment"></a>Passer en revue les résultats de votre expérience

1. Dans le Centre de développement, revenez à la page **Expérimentation** de votre application.
2. Dans la section **Active experiments** (Expériences actives), cliquez sur le nom de votre expérience active pour accéder à la page correspondante.
3. Dans le cas d’une expérience active ou terminée, les deux premières sections de cette page fournissent les résultats de votre expérience :
  * La section **Résumé des résultats** répertorie les objectifs de votre expérience et le taux de conversion pour chaque variante.
  * La section **Détails des résultats** fournit des informations supplémentaires sur chacun des objectifs de votre expérience, notamment les vues, les conversions, les utilisateurs uniques, le taux de conversion, le pourcentage d’écart, la confiance et l’importance. La *confiance* est une mesure statistique de la fiabilité d’une estimation, qui calcule la marge d’erreur. L’*importance* est une mesure statistique, reposant sur la taille de l’échantillon, qui détermine la probabilité qu’un résultat ne soit pas dû au hasard, mais qu’il soit plutôt attribué à une cause spécifique.

  >**Remarque**&nbsp;&nbsp;Le Centre de développement signale uniquement le premier événement de conversion pour chaque utilisateur sur une période de 24 heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application au cours d’une période de 24 heures, seul le premier événement de conversion est signalé. Cette approche est destinée à éviter qu’un utilisateur unique avec de nombreux événements de conversion ne fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs.


## <a name="complete-your-experiment"></a>Terminer votre expérience

1. Dans le tableau de bord, revenez à la page de votre expérience. Pour obtenir les instructions correspondantes, voir la section précédente.
2. Dans la section **Résumé des résultats**, effectuez l’une des opérations suivantes :
  * Si vous souhaitez mettre fin à l’expérience et continuer à utiliser les valeurs des variables dans la variante de contrôle de votre application, cliquez sur **Conserver**.
  * Si vous souhaitez mettre fin à l’expérience, mais utiliser les valeurs des variables dans une autre variante de votre application, cliquez sur **Basculer** sous la variante vers laquelle vous voulez basculer.
3. Cliquez sur **OK** pour confirmer que vous souhaitez mettre fin à l’expérience.


## <a name="related-topics"></a>Rubriques connexes

* [Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)

