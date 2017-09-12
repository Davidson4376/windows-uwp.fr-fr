---
author: JnHs
Description: "Obtenez des analyses détaillées de vos applications Windows dans le tableau de bord ou par le biais d’autres méthodes."
title: "Analyser les performances de l’application"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, analyse, rapports, tableau de bord, applications
ms.openlocfilehash: 57e4a30258fa25411bb461cac56aa18d2f74981d
ms.sourcegitcommit: a93b1da07b386a682435de58a8129d7b4ee90c14
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2017
---
# <a name="analyze-app-performance"></a>Analyser les performances de l’application

Vous pouvez voir les analyses détaillées de vos applications dans le tableau de bord du Centre de développement Windows. Les statistiques et les graphiques vous permettent de savoir où en sont vos applications: combien de clients vous avez atteint, la façon dont ils utilisent votre application et ce qu’ils en pensent. Vous pouvez également obtenir des métriques sur l’intégrité de l’application, l’utilisation des publicités, etc.

Vous pouvez visualiser les rapports d’analyse dans le tableau de bord ou [télécharger les rapports dont vous avez besoin](download-analytic-reports.md) pour analyser les données hors connexion. Nous vous proposerons également plusieurs méthodes vous permettant [d’accéder à vos données d’analyse sans utiliser le tableau de bord](#no-dashboard).

## <a name="view-key-analytics-for-all-your-apps"></a>Visualiser les principales analyses de toutes vos applications

Pour visualiser les principales analyses relatives à vos applications les plus téléchargées, développez **Analyser**, puis sélectionnez **Vue d’ensemble**. Par défaut, la page **Vue d’ensemble de l’analyse** affiche des informations sur les cinq applications qui ont été acquises le plus souvent au cours de leur durée de vie. Pour choisir d’autres applications publiées à afficher, sélectionnez **Filtres**.

## <a name="view-individual-reports-for-each-app"></a>Visualiser des rapports individuels pour chaque application

Cette section détaille les informations présentées dans chacun des rapports suivants:

-   [Rapport sur les acquisitions](acquisitions-report.md)
-   [Rapport sur les acquisitions d’extensions](add-on-acquisitions-report.md)
-   [Rapport d’utilisation](usage-report.md)
-   [Rapport d’intégrité](health-report.md)
-   [Rapport sur les avis](reviews-report.md)
-   [Rapport sur les commentaires](feedback-report.md)
-   [Rapport sur les performances publicitaires](advertising-performance-report.md)
-   [Rapport de campagne de publicité](promote-your-app-report.md)

> [!NOTE]
> Étant donné que les données qui figurent dans ces rapports dépendent des fonctionnalités et de l’implémentation propres à votre application, certains rapports n’en contiennent pas.

<span id="no-dashboard"/>
## <a name="access-analytics-data-without-using-the-dev-center-dashboard"></a>Accéder aux données d’analyse sans utiliser le tableau de bord du Centre de développement

Outre les rapports d’analyse du tableau de bord, vous pouvez accéder à vos données d’analyse de plusieurs autres façons.

### <a name="windows-store-analytics-api"></a>API d’analyse du WindowsStore

Utilisez [l’API d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md) pour récupérer par programme les données d’analyse de vos applications. Cette API REST permet de récupérer les données pour les acquisitions de modules complémentaires et d’applications, les erreurs, ainsi que les évaluations et avis relatifs aux applications. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels à partir de votre application ou service.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Pack de contenu du Centre de développement Windows pour PowerBI

Utilisez le [pack de contenu du Centre de développement Windows pour Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) pour découvrir et surveiller vos données d’analyse du Centre de développement dans PowerBI. PowerBI est un service d’analyse métier basé sur le cloud qui vous offre une vue unique de vos données d’entreprise.

Utilisez les ressources suivantes pour commencer à utiliser PowerBI pour accéder à vos données d’analyse.

* [S’inscrire à PowerBI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Découvrez comment utiliser PowerBI](https://powerbi.microsoft.com/guided-learning/)
* [Découvrez comment utiliser le pack de contenu du Centre de développement Windows pour PowerBI pour se connecter à vos données d’analyse](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Pour connecter le pack de contenu du Centre de développement Windows pour Power BI, nous vous recommandons de spécifier les informations d’identification à partir d’un répertoire Azure AD associé à votre compte du Centre de développement. Si vous utilisez vos informations d’identification de compte Microsoft, vos données d’analyse dans PowerBI ne sont pas actualisées automatiquement et vous devez vous connecter à PowerBI pour actualiser vos données. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](http://go.microsoft.com/fwlink/p/?LinkId=703757). Pour plus d’informations sur l’association de votre compte du Centre de développement avec Azure AD, consultez [Gérer les utilisateurs de compte](manage-account-users.md).

### <a name="dev-center-app"></a>Application Centre de développement

Installez l’application [Centre de développement](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) pour visualiser rapidement des informations sur l’intégrité et les performances de vos applications sur tout appareil Windows10.

