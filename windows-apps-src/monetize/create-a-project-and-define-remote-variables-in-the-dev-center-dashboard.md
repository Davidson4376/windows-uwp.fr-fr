---
author: Xansky
Description: Before you can run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must create a project and define your remote variables in Partner Center.
title: Créez un projet d’expérience dans l’Espace partenaires
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, MicrosoftStore Services SDK, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: 19a59110fa094aeae3d40dca1372fde9889c108e
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7169274"
---
# <a name="create-an-experiment-project-in-partner-center"></a>Créez un projet d’expérience dans l’Espace partenaires

Pour vous familiariser avec l’expérimentation, créez un [projet](run-app-experiments-with-a-b-testing.md#terms) d’expérimentation pour votre application dans l’espace partenaires et définir des variables distantes que votre application peut accéder.

Les instructions suivantes décrivent les étapes de base pour créer un projet. Pour découvrir une procédure pas à pas détaillée illustrant le processus de création d’un projet, puis exécuter une expérience de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="instructions"></a>Instructions

1. Connectez-vous à l'[Espace partenaires](https://partner.microsoft.com/dashboard).
2. Sous **Vos applications**, sélectionnez l’application pour laquelle vous voulez créer une expérience.
3. Dans le volet de navigation, sélectionnez **Services** puis **Expérimentation**.
4. Dans la page **Expérimentation**, cliquez sur le bouton **Nouveau projet** dans la section **Projets**. Si vous avez déjà créé un ou plusieurs projets, ceux-ci sont répertoriés dans la section **Projets**.
5. Dans la page **Nouveau projet**, entrez le nom de votre nouveau projet.
6. Dans la section **Variables distantes**, ajoutez les [variables](run-app-experiments-with-a-b-testing.md#terms) que vous voulez rendre accessibles à toutes les expériences du projet et définissez les valeurs par défaut de chaque variable. Les valeurs par défaut spécifiées ici sont utilisées pour le groupe de contrôle des expériences et pour les utilisateurs qui ne participent pas à l’expérience.
  1. Si la section **Variables distantes** est réduite, cliquez sur **Afficher** sur l’en-tête de section.
  2. Cliquez sur **Ajouter une variable** afin de créer chaque variable que vous voulez rendre disponible pour n’importe quelle expérience de ce projet, puis tapez le nom et la valeur par défaut de la variable.
  3. Une fois les variables ajoutées, cliquez sur **Enregistrer**.
3. Dans la section **Intégration du SDK**, notez la valeur indiquée dans [ID du projet](run-app-experiments-with-a-b-testing.md#terms). Lorsque vous [codez votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md), vous devez référencer cet ID de projet dans votre code afin de pouvoir recevoir des données de variante et signaler des événements d’affichage et de conversion vers l’espace partenaires.

> [!NOTE]
> Vous ne pouvez pas modifier, ajouter ou supprimer des variables distantes si une expérience est active dans le projet. Cette limitation protège l’intégrité des données du groupe de contrôle de l’expérience active.


## <a name="next-steps"></a>Étapes suivantes

Une fois le projet créé, vous pouvez [coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md) pour commencer à récupérer les valeurs des variables distantes dans votre application, et [créer une expérience dans le projet](define-your-experiment-in-the-dev-center-dashboard.md).

## <a name="related-topics"></a>Rubriques connexes

* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans l’Espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans l’Espace partenaires](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Exécuter des expériences d’application avec des tests A/B](run-app-experiments-with-a-b-testing.md)
