---
author: jnHs
Description: "Vous pouvez voir les analyses détaillées de vos applications dans le tableau de bord du Centre de développement Windows."
title: Analyses
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
translationtype: Human Translation
ms.sourcegitcommit: fa6e5945855defc99e9f5636543ec072eb777a5a
ms.openlocfilehash: c628b1fb29601ff1d4ff3629da45409586274f6b

---

# Analyses

Vous pouvez voir les analyses détaillées de vos applications dans le tableau de bord du Centre de développement Windows. Les statistiques et les graphiques vous permettent de savoir où en sont vos applications: combien de clients vous avez atteint, la façon dont ils utilisent votre application et ce qu’ils en pensent. Vous obtenez également des informations sur l’intégrité de l’application, l’utilisation des publicités, etc. Affichez des rapports dans le tableau de bord ou [téléchargez les rapports dont vous avez besoin](download-analytic-reports.md) pour analyser les données hors connexion. Nous vous proposerons également plusieurs méthodes vous permettant [d’accéder à vos données d’analyse sans utiliser le tableau de bord](#no-dashboard).

> [!NOTE]
> Outre les rapports du tableau de bord, vous pouvez accéder par programme à certaines données d’analyse à l’aide de [l’API REST d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

## Analyse de toutes vos applications

Pour afficher les principales analyses concernant les applications les plus téléchargées, dans le menu de navigation supérieur, sélectionnez **Analyse** > **Vue d’ensemble**. Par défaut, la page **Vue d’ensemble de l’analyse** affiche des informations sur les cinq applications qui ont été acquises le plus souvent au cours de leur durée de vie. Pour choisir d’autres applications à afficher, sélectionnez **Modifier les filtres**.

## Rapports disponibles pour chaque application

Cette section détaille les informations présentées dans chacun des rapports suivants :

-   [Rapport sur les acquisitions](acquisitions-report.md)
-   [Rapport sur l’intégrité](health-report.md)
-   [Rapport sur les évaluations](ratings-report.md)
-   [Rapport sur les révisions](reviews-report.md)
-   [Rapport sur les commentaires](feedback-report.md)
-   [Rapport sur l'utilisation](usage-report.md)
-   [Rapport sur les acquisitions de modules complémentaires](add-on-acquisitions-report.md)
-   [Rapport de médiation publicitaire](ad-mediation-report.md)
-   [Rapport sur les performances publicitaires](advertising-performance-report.md)
-   [Rapport de performances des annonces des affiliés](affiliates-performance-report.md)
-   [Rapport de publicité sur l’installation d’applications](app-install-ads-reports.md)
-   [Rapport sur les canaux et conversions](channels-and-conversions-report.md)

> [!NOTE]
> Les données figurant dans ces rapports dépendant des fonctionnalités et de l’implémentation propres à votre application, certains rapports n’en contiennent pas.

## Filtres de page et de section

Chaque rapport intègre des filtres qui vous aident à analyser vos données en profondeur. En haut de la page se trouve **Appliquer les filtres**. Ces filtres servent à restreindre ou à développer l’étendue des informations et des graphiques présentés sur la page.

Dans chaque graphique, vous pouvez voir des filtres de section. Ceux-ci limitent les données affichées uniquement pour ce graphique particulier.

Ces filtres varient selon les rapports. Les rubriques de cette section expliquent les filtres disponibles et décrivent les autres données disponibles sur la page de chaque rapport.

<span id="no-dashboard"/>
## Accéder aux données d’analyse sans utiliser le tableau de bord du Centre de développement

Outre les rapports d’analyse du tableau de bord, il existe plusieurs façons d’accéder à vos données d’analyse.

### API d’analyse du WindowsStore

Utilisez [l’API d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md) pour récupérer par programme les données d’analyse de vos applications. Cette API REST permet de récupérer les données pour les acquisitions de modules complémentaires et d’applications, les erreurs, ainsi que les évaluations et avis relatifs aux applications. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels à partir de votre application ou service.

### Pack de contenu du Centre de développement Windows pour PowerBI

Utilisez le [pack de contenu du Centre de développement Windows pour Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) pour découvrir et surveiller vos données d’analyse du Centre de développement dans PowerBI. PowerBI est un service d’analyse métier basé sur le cloud qui vous offre une vue unique de vos données d’entreprise.

Utilisez les ressources suivantes pour commencer à utiliser PowerBI pour accéder à vos données d’analyse.

* [S’inscrire à PowerBI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Découvrez comment utiliser PowerBI](https://powerbi.microsoft.com/guided-learning/)
* [Découvrez comment utiliser le pack de contenu du Centre de développement Windows pour PowerBI pour se connecter à vos données d’analyse](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Pour connecter le pack de contenu du Centre de développement Windows pour Power BI, nous vous recommandons de spécifier les informations d’identification à partir d’un répertoire Azure AD associé à votre compte du Centre de développement. Si vous utilisez vos informations d’identification de compte Microsoft, vos données d’analyse dans PowerBI ne sont pas actualisées automatiquement et vous devez vous connecter à PowerBI pour actualiser vos données. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](http://go.microsoft.com/fwlink/p/?LinkId=703757). Pour plus d’informations sur l’association de votre compte du Centre de développement avec Azure AD, consultez [Gérer les utilisateurs de compte](manage-account-users.md).

### Application Centre de développement

Installez l’application [Centre de développement](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) pour afficher rapidement des informations sur l’intégrité et les performances de vos applications sur tout appareil Windows10.

## Rubriques connexes
- [Publier des applications Windows](index.md)



<!--HONumber=Nov16_HO1-->


