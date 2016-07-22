---
author: mcleanbyron
Description: "Vous pouvez utiliser le tableau de bord du Centre de développement Windows pour exécuter des expériences pour vos applications de plateformeWindows universelle (UWP) avec des tests A/B."
title: "Exécuter des expériences d’application avec des tests A/B"
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 88fd0516e3c10b657884b93377480b62c1758992

---

# Exécuter des expériences d’application avec des tests A/B

Vous pouvez utiliser le tableau de bord du Centre de développement Windows pour exécuter des expériences pour vos applications de plateformeWindows universelle (UWP) avec des tests A/B.

Dans un test A/B, vous effectuez des expériences avec des affectations de variable de programme dans vos applications par le biais du Centre de développement. En créant une logique d’application articulée autour de variables de programme pouvant faire l’objet de tests A/B, vous pouvez activer des variantes de votre application pour des segments aléatoires de vos utilisateurs cibles. Votre test A/B a pour fonction d’identifier une variante susceptible de vous faire bénéficier de meilleurs taux de conversion (par exemple, davantage d’achats dans l’application).

Une fois que vous avez identifié la variante répondant le mieux à vos objectifs d’entreprise, vous pouvez mettre aussitôt fin à l’expérience et activer cette variante pour tous vos utilisateurs cibles à partir du tableau de bord du Centre de développement sans avoir à republier votre application.

## Créer et exécuter un test A/B

Pour créer et exécuter un test A/B, procédez comme suit:

1. [Définissez votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md). Chaque expérience se compose des éléments suivants:
  * un *événement d’affichage*, indiquant que l’utilisateur commence à visualiser une variante faisant partie intégrante de votre expérience ,
  * un ou plusieurs objectifs avec des *événements de conversion* indiquant le moment où un objectif a été atteint ;
  * une ou plusieurs *variantes* définissant les paramètres utilisés par votre expérience.
2. [Codez votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md). Utilisez une API du Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft pour obtenir les paramètres de variante pour l’expérience, utiliser ces données pour modifier le comportement de la fonctionnalité que vous testez et envoyer des événements d’affichage et de conversion au Centre de développement.
3. [Exécutez et gérez votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md). Utilisez le tableau de bord pour passer en revue les résultats de l’expérience et pour terminer cette dernière.

Pour découvrir une procédure pas à pas illustrant le processus de bout en bout, voir [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configuration requise

Les tests A/B test dans le Centre de développement Windows sont uniquement pris en charge pour les applications UWP.

Avant d’être en mesure d’exécuter des expériences avec des tests A/B, vous devez configurer votre ordinateur de développement:

* Suivez les instructions [de cette rubrique](../get-started/get-set-up.md) pour configurer votre ordinateur de développement pour le développement UWP.
* Installez le [Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](http://aka.ms/store-em-sdk). Outre l’API relative aux expériences, ce SDK fournit des API pour d’autres fonctionnalités, telles que l’affichage d’annonces publicitaires et l’orientation de vos clients vers le Hub de commentaires pour vous permettre de recueillir des commentaires concernant votre application. Pour plus d’informations sur ce SDK, voir [Monétiser votre application avec le Kit de développement logiciel (SDK) d’engagement et de monétisation de la Boutique Microsoft](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).

## Meilleures pratiques

Pour obtenir des résultats optimaux, suivez les recommandations ci-après lorsque vous exécutez des expériences avec les tests A/B:

* Envisagez de n’exécuter ces expériences qu’avec deux variantes avec une distribution aléatoire à fractionnement de type 50/50 pour les affectations de variante.
* Exécutez les expériences pendant au moins 2 à 4semaines afin de collecter suffisamment de données exploitables et significatives d’un point de vue statistique.

## Rubriques connexes

* [Définir votre expérience dans le tableau de bord du Centre de développement](define-your-experiment-in-the-dev-center-dashboard.md)
* [Coder votre application à des fins d’expérimentation](code-your-experiment-in-your-app.md)
* [Gérer votre expérience dans le tableau de bord du Centre de développement](manage-your-experiment.md)
* [Créer et exécuter votre première expérience avec des tests A/B](create-and-run-your-first-experiment-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


