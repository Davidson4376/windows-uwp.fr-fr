---
author: Xansky
Description: You can use the Windows Dev Center dashboard to run experiments for your Universal Windows Platform (UWP) apps with A/B testing.
title: Exécuter des expériences d’application avec des testsA/B
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: 90ff6ba9a3767570aa9e885d491b38053a1d5d52
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4748692"
---
# <a name="run-app-experiments-with-ab-testing"></a>Exécuter des expériences d’application avec des tests A/B

Vous pouvez utiliser le tableau de bord du Centre de développement Windows pour définir des variables distantes que vous pouvez récupérer au moment de l’exécution à partir de vos applications de plateforme Windows universelle (UWP). Vous pouvez également tester les variantes de ces valeurs avec vos utilisateurs pour identifier celles qui sont les plus efficaces pour générer le comportement utilisateur souhaité. Votre application peut utiliser des variables distantes pour configurer des expériences d’applications telles que des achats in-app, un flux de connexion, des légendes et des emplacements de publicité.

Votre testA/B doit avoir pour fonction d’identifier une variante de vos valeurs de variables distantes susceptible de vous faire bénéficier de meilleurs taux de conversion (par exemple, davantage d’achats dans l’application) en fournissant une expérience d’application plus conviviale. Une fois que vous avez identifié la variante performante, vous pouvez mettre aussitôt fin à l’expérience et activer cette variante pour tous vos utilisateurs cibles à partir du tableau de bord du Centre de développement, sans avoir à republier votre application.

## <a name="create-and-run-an-ab-test"></a>Créer et exécuter un testA/B

Pour créer et exécuter un testA/B, procédez comme suit:

1. [Créez un projet et définissez des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Ce projet contient les variables et les valeurs de variables par défaut pour vos expériences.  
2. [Codez votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md). Utilisez une API du Microsoft Store Services SDK pour obtenir des valeurs de variables distances du projet que vous avez créé dans le tableau de bord, utiliser ces données pour modifier le comportement de la fonctionnalité que vous testez et envoyer des événements d’affichage et de conversion au Centre de développement.
3. [Définissez votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md). Créez une expérience dans votre projet définissant les objectifs uniques et les variantes de votre testA/B.
4. [Exécutez et gérez votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md). Activez votre expérience, puis utilisez le tableau de bord pour passer en revue les résultats de l’expérience et terminer cette dernière.

Pour découvrir une procédure pas à pas illustrant le processus de bout en bout, voir [Créer et exécuter votre première expérience avec des testsA/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="requirements"></a>Configuration requise

Les tests A/B test dans le Centre de développement Windows sont uniquement pris en charge pour les applications UWP.

Avant d’être en mesure d’exécuter des expériences avec des tests A/B, vous devez configurer votre ordinateur de développement:

* Suivez les instructions mentionnées [ici](../get-started/get-set-up.md) pour configurer votre ordinateur de développement pour le développement UWP.
* [Installez le Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk). Outre l’API relative aux expériences, ce SDK fournit des API pour d’autres fonctionnalités, telles que l’affichage d’annonces publicitaires et l’orientation de vos clients vers le Hub de commentaires pour vous permettre de recueillir des commentaires concernant votre application.

## <a name="best-practices"></a>Meilleures pratiques

Pour obtenir des résultats optimaux, suivez les recommandations ci-après lorsque vous exécutez des expériences avec les tests A/B:

* Envisagez de n’exécuter ces expériences qu’avec deux variantes avec une distribution aléatoire à fractionnement de type 50/50 pour les affectations de variante.
* Exécutez les expériences pendant au moins 2 à 4semaines afin de collecter suffisamment de données exploitables et significatives d’un point de vue statistique.

<span id="terms" />

## <a name="related-terms"></a>Termes associés

|  Terme  |  Définition  |
|--------|--------------|
| Projet    |   Collection de variables distantes avec valeurs par défaut auxquelles votre application peut accéder à l’aide du Microsoft Store Services SDK. Un projet peut également contenir une ou plusieurs expériences qui partagent les mêmes variables distantes.  |
| Expérience    |   Ensemble de paramètres définissant un testA/B que vos utilisateurs recevront. Les expériences sont définies dans l’étendue d’un projet, et chaque expérience se compose des éléments suivants: <p></p><ul><li>un *événement d’affichage*, indiquant que l’utilisateur commence à visualiser une variante faisant partie intégrante de votre expérience;</li><li>un ou plusieurs objectifs avec des *événements de conversion* indiquant le moment où un objectif a été atteint;</li><li>une ou plusieurs *variantes* définissant les données de variables utilisées par votre expérience. La variante de *contrôle* utilise les valeurs de variables par défaut définies dans le projet pour l’expérimentation. Outre la variante de contrôle, ces expériences ont généralement au moins une variante supplémentaire avec des valeurs de variables qui sont propres à l’expérience. </li></ul>          |
| ID de projet    |   ID unique qui associe votre application à un projet de votre compte du Centre de développement. Vous devez utiliser cet ID pour vous connecter au service des tests A/B dans le code de votre application pour recevoir des données de variante et signaler des événements d’affichage et de conversion au Centre de développement. Pour plus d’informations, voir [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md).<p></p><p>Chaque projet et toutes les expériences qu’il contient, sont associés à exactement un ID de projet. Vous pouvez utiliser des ID de projet pour distinguer les différents ensembles d’expériences. Par exemple, vous pouvez publier un ensemble d’expériences à l’intention des testeurs de votre organisation, et un autre ensemble uniquement à l’intention des utilisateurs externes de votre application.  Une application peut faire référence à plusieurs ID de projet si elle implémente plusieurs expériences.</p>         |
| Variante    |   Collection d’une ou de plusieurs variables que vous testez dans votre expérience. Chaque expérience doit contenir au moins une variable et deux variantes (dont le contrôle). Une expérience peut avoir jusqu’à cinq variantes.           |
| Variable    |  Valeur que votre application utilise pour initialiser une propriété ou une autre valeur dans votre application. Au cours d’une expérience, la valeur de la variable change d’une variante à une autre. Une fois une expérience terminée, la variable se voit attribuer la même valeur que la variante que vous choisissez de publier à tous les utilisateurs de votre application. Les variables peuvent avoir les types suivants: chaîne, booléen, double et entier.
| Événement d’affichage    |  Chaîne arbitraire qui représente une activité lorsque l’utilisateur commence à afficher une variante faisant partie de votre expérience. En général, il s’agit du nom d’un événement dans votre code. Le code de votre application envoie cette chaîne d’événement d’affichage au Centre de développement lorsque l’utilisateur commence à afficher une variante. Pour plus d’informations, voir [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md).
| Événement de conversion    |  Chaîne arbitraire qui représente un objectif pour un objectif d’une expérience. En général, il s’agit du nom d’un événement dans votre code. Le code de votre application envoie cette chaîne d’événement de conversion au Centre de développement lorsque l’utilisateur atteint un objectif. Pour plus d’informations, voir [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md).  

## <a name="related-topics"></a>Rubriques connexes

* [Créer un projet et définir des variables distantes dans le tableau de bord du Centre de développement](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
