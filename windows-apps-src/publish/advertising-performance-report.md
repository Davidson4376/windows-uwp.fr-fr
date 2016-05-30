---
author: jnHs
Description: Pour afficher les données de performances pour les unités publicitaires dans vos applications, utilisez les rapports sur les performances publicitaires au niveau du compte et de l’application dans le tableau de bord du Centre de développement Windows.
title: Rapport sur les performances publicitaires
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
---

# Rapport sur les performances publicitaires


Pour afficher les données de performances pour les unités publicitaires dans vos applications, vous pouvez utiliser les rapports suivants dans le tableau de bord du centre de développement Windows &#58;

-   [Rapport sur les performances publicitaires au niveau de l’application](advertising-performance-report.md#app-level-advertising-performance-report). Ce rapport fournit des données de performances pour les unités publicitaires Microsoft dans l’application actuellement sélectionnée dans le tableau de bord.
-   [Rapport sur les performances publicitaires au niveau du compte](advertising-performance-report.md#account-level-advertising-performance-report). Ce rapport fournit des données de performances détaillées pour les unités publicitaires Microsoft et les annonces de la communauté pour toutes les applications qui sont enregistrées sur votre compte de développeur.
-   [Rapport sur les performances publicitaires au niveau du tableau de bord](advertising-performance-report.md#dashboard-level-advertising-performance-report) Ce rapport, présent sur la page de **présentation du tableau de bord**, fournit des données de performances détaillées pour les unités publicitaires Microsoft pour toutes les applications qui sont enregistrées sur votre compte de développeur.

Par défaut, les rapports sont filtrés sur les performances des 30 derniers jours, sur tous les appareils. Pour modifier ces filtres, cliquez sur **Filtres de page** et choisissez une autre période ou un type d’appareil spécifique. 

> **Remarque** Il peut exister des différences entre les rapports de performances publicitaires dans le Centre de développement et pubCenter. Les données de performances publicitaires dans le Centre de développement sont agrégées en fonction de l’UTC (pas de votre fuseau horaire spécifique), tandis que les rapports pubCenter sont agrégés en fonction de votre fuseau horaire spécifique.

Les sections suivantes fournissent des détails complémentaires à propos de ces rapports.

## Rapport sur les performances publicitaires au niveau de l’application.

Ce rapport fournit, sous forme de graphiques, de cartes du monde et de tableaux, des données de performances pour les unités publicitaires Microsoft dans l’application actuellement sélectionnée dans le tableau de bord. Pour afficher ce rapport, sélectionnez l’une de vos applications dans le tableau de bord et cliquez sur **Analyse**&gt;**Performances des publicités** dans le volet de navigation.

Les données sont obtenues à partir des métriques de performances suivantes que nous suivons pour les publicités intégrées dans votre application :

-   **Estimation des revenus** : estimation des revenus générés par les publicités diffusées dans votre application.
-   **Coût par impression électronique** : coût effectif pour mille impressions.
-   **Demandes** : nombre de fois où une demande de publicité a été envoyée à partir de votre application.
-   **Impressions** : nombre de fois où une publicité a été affichée dans votre application.
-   **Taux de remplissage** : pourcentage de demandes de publicité envoyées à partir de votre application dans lesquelles une publicité était affichée.
-   **Clics** : nombre de fois où quelqu’un a cliqué sur une publicité dans votre application.
-   **Taux de clic** : nombre de fois où quelqu’un a cliqué sur une publicité divisé par le nombre d’impressions.

Pour passer en revue toutes ces métriques de performances pour les unités publicitaires intégrées dans votre application, reportez-vous au tableau sous les vues graphique et carte.

Pour analyser les données pour l’un de ces métriques dans une vue de carte graphique ou monde, cliquez sur **graphique** ou **carte**. Cliquez sur les en-têtes au-dessus du graphique ou de la carte pour basculer entre les différentes métriques. Par défaut, les vues graphique et carte affichent les données de performances pour toutes les unités publicitaires dans votre application, mais vous pouvez cliquer sur **Choisissez des unités publicitaires** pour sélectionner jusqu’à six unités publicitaires individuelles à comparer.

Dans la vue carte, les nuances plus foncées représentent des valeurs plus élevées et les nuances plus légères représentent des valeurs inférieures. Vous pouvez pointer sur un pays ou une région sur la carte pour analyser la valeur de la métrique sélectionnée. Vous pouvez également effectuer un zoom avant sur une zone de la carte pour afficher les données des plus petits pays.

## Rapport sur les performances publicitaires au niveau du compte

Ce rapport fournit des données de performances pour les unités publicitaires Microsoft et les annonces de la communauté utilisées dans les applications qui sont enregistrées sur votre compte de développeur. Pour afficher ce rapport, accédez à votre page de présentation du tableau de bord et cliquez sur **Performances des publicités** dans le volet de navigation.

Cette page contient les sections suivantes.

### Microsoft Advertising

Ce rapport fournit des données de performances pour toutes les unités publicitaires Microsoft utilisées dans vos applications. Il inclut également des données de performances pour les unités publicitaires pubCenter qui n’ont pas été correctement mappées à vos applications du Centre de développement.

Ce rapport montre les sept mêmes mesures de performances et affichages (sous forme de graphiques, cartes du monde et tableaux) que le rapport sur les performances publicitaires au niveau de l’application décrit ci-dessus. Vous pouvez appliquer les filtres suivants à ce rapport :

-   **Toutes les unités publicitaires**. Lorsque vous sélectionnez ce filtre, vous pouvez choisir d’afficher les données à partir de toutes les unités publicitaires ou d’un maximum de six unités publicitaires spécifiques.
-   **Toutes les applications**. Lorsque vous sélectionnez ce filtre, vous pouvez choisir d’afficher les données à partir de toutes les applications ou d’un maximum de six applications spécifiques.
-   **Application individuelle**. Lorsque vous sélectionnez une application, vous pouvez choisir d’afficher les données à partir de toutes les unités publicitaires utilisées par l’application ou d’un maximum de six unités publicitaires spécifiques utilisées par l’application.

Si vous avez créé des unités publicitaires pour une application à l’aide de Microsoft pubCenter, il se peut qu’elles n’aient pas toutes été correctement mappées à vos applications dans le Centre de développement. Dans ce rapport, les unités publicitaires sont associées à des noms d’application que vous avez spécifiés dans pubCenter, avec la chaîne **(pubCenter)** ajoutée au nom de l’application.

Si vous avez créé des unités publicitaires pour une application à l’aide de pubCenter et que les données correspondantes ne sont disponibles dans aucune des deux pages de rapports, cliquez sur **Lier un compte pubCenter** pour lier ce compte à votre compte du Centre de développement. Entrez l’adresse de messagerie que vous avez associée à ce compte pubCenter et votre code de liaison du compte. Pour connaître le code de liaison du compte, connectez-vous à ce compte pubCenter et accédez à la page **Mes informations**.

Pour plus d’informations sur la migration de comptes pubCenter vers le Centre de développement, voir [Intégration de pubCenter au Centre de développement](pubcenter-dev-center-integration.md).

### Annonces de la communauté Microsoft

Cette section fournit, sous forme de graphiques et de cartes du monde, des données de performances pour les annonces de la communauté dans l’application actuellement sélectionnée dans le tableau de bord. Pour en savoir plus sur les annonces de la communauté, voir [À propos des annonces de la communauté](about-community-ads.md).

Les données sont obtenues à partir des métriques de performances suivantes que nous suivons pour les publicités intégrées dans votre application :

-   **Demandes** : nombre de fois où une demande d’annonce de la communauté a été envoyée à partir de votre application.
-   **Taux de remplissage** : pourcentage de demandes d’annonce de la communauté envoyées à partir de votre application dans lesquelles une publicité était affichée.
-   **Clics** : nombre de fois où quelqu’un a cliqué sur une annonce de la communauté dans votre application.
-   **Taux de clic** : nombre de fois où quelqu’un a cliqué sur une annonce de la communauté divisé par le nombre d’impressions.
-   **Crédits obtenus** : nombre de crédits d’annonces de la communauté que vous avez gagnés grâce à cette application. Pour en savoir plus sur la façon d’obtenir des crédits, voir [À propos des annonces de la communauté](about-community-ads.md).
-   **Crédits dépensés** : nombre de crédits d’annonces de la communauté que vous avez dépensés pour cette application. Pour en savoir plus sur la façon de dépenser des crédits, voir [À propos des annonces de la communauté](about-community-ads.md).

Pour analyser les données pour l’un de ces métriques dans une vue de carte graphique ou monde, cliquez sur **graphique** ou **carte**. Cliquez sur les en-têtes au-dessus du graphique ou de la carte pour basculer entre les différentes métriques. Dans la vue carte, les nuances plus foncées représentent des valeurs plus élevées et les nuances plus légères représentent des valeurs inférieures. Vous pouvez pointer sur un pays ou une région sur la carte pour analyser la valeur de la métrique sélectionnée. Vous pouvez également effectuer un zoom avant sur une zone de la carte pour afficher les données des plus petits pays.

## Rapport sur les performances publicitaires au niveau du tableau de bord

La section **Performances des publicités** de votre page **Présentation du tableau de bord** fournit un résumé des données de performances pour toutes les unités publicitaires Microsoft utilisées dans vos applications. Ce rapport est similaire à la section **Microsoft Advertising** du rapport sur les performances publicitaires au niveau du compte. Il comporte cependant les différences suivantes :

-   Il fournit les données uniquement sous forme de graphique. Pour afficher les données sous forme de tableau, utilisez le rapport sur les performances publicitaires au niveau du compte.
-   Les seuls filtres fournis sont valides pour toutes les applications ou pour les applications individuelles. Pour filtrer par unités publicitaires, utilisez le rapport sur les performances publicitaires au niveau du compte.


 

 


<!--HONumber=May16_HO2-->


