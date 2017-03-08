---
author: mcleanbyron
Description: "Pour lancer une expérience dans votre application de plateforme Windows universelle (UWP) avec des tests A/B, vous devez créer un projet et définir vos variables distantes dans le tableau de bord du Centre de développement."
title: "Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement"
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Microsoft Store Services SDK, tests A/B, expériences"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ad24e9a52458b826f3ce1bc7aba3dbdcf302f87d
ms.lasthandoff: 02/07/2017

---

# <a name="create-a-project-and-define-remote-variables-in-the-dev-center-dashboard"></a>Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement

Pour débuter l’expérimentation, créez un [projet](run-app-experiments-with-a-b-testing.md#terms) d’expérimentation pour votre application dans le tableau de bord du Centre de développement et définissez les variables distantes auxquelles votre application peut accéder.

Les instructions suivantes décrivent les étapes de base pour créer un projet. Pour découvrir une procédure pas à pas détaillée illustrant le processus de création d’un projet, puis exécuter une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="instructions"></a>Instructions

1. Connectez-vous au [tableau de bord du Centre de développement](https://dev.windows.com/overview).
2. Sous **Vos applications**, sélectionnez l’application pour laquelle vous voulez créer une expérience.
3. Dans le volet de navigation, sélectionnez **Services** puis **Expérimentation**.
4. Dans la page **Expérimentation**, cliquez sur le bouton **Nouveau projet** dans la section **Projets**. Si vous avez déjà créé un ou plusieurs projets, ceux-ci sont répertoriés dans la section **Projets**.
5. Dans la page **Nouveau projet**, entrez le nom de votre nouveau projet.
6. Dans la section **Variables distantes**, ajoutez les [variables](run-app-experiments-with-a-b-testing.md#terms) que vous voulez rendre accessibles à toutes les expériences du projet et définissez les valeurs par défaut de chaque variable. Les valeurs par défaut spécifiées ici sont utilisées pour le groupe de contrôle des expériences et pour les utilisateurs qui ne participent pas à l’expérience.
  1. Si la section **Variables distantes** est réduite, cliquez sur **Afficher** sur l’en-tête de section.
  2. Cliquez sur **Ajouter une variable** afin de créer chaque variable que vous voulez rendre disponible pour n’importe quelle expérience de ce projet, puis tapez le nom et la valeur par défaut de la variable.
  3. Une fois les variables ajoutées, cliquez sur **Enregistrer**.
3. Dans la section **Intégration du SDK**, notez la valeur indiquée dans [ID du projet](run-app-experiments-with-a-b-testing.md#terms). Lorsque vous [codez votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md), vous devez indiquer cet ID de projet dans votre code pour pouvoir recevoir des données de variation et signaler des événements d’affichage et de conversion au Centre de développement.

>**Remarque**&nbsp;&nbsp;Vous ne pouvez pas modifier, ajouter ou supprimer des variables distantes si une expérience est active dans le projet. Cette limitation protège l’intégrité des données du groupe de contrôle de l’expérience active.


## <a name="next-steps"></a>Étapes suivantes

Une fois le projet créé, vous pouvez [coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md) pour commencer à récupérer les valeurs des variables distantes dans votre application, et [créer une expérience dans le projet](define-your-experiment-in-the-dev-center-dashboard.md).

## <a name="related-topics"></a>Rubriques connexes

* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)

