---
Description: Get detailed analytics for your Windows apps, in Partner Center or via other methods.
title: Analyser les performances de l’application
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, analytique, rapports, tableau de bord, applications, données, les mesures
ms.localizationpriority: medium
ms.openlocfilehash: f6a6d79745ec98af2c7f562297092eea3feda659
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8696348"
---
# <a name="analyze-app-performance"></a>Analyser les performances de l’application

Vous pouvez afficher analytique détaillées de vos applications dans [L’espace partenaires](https://partner.microsoft.com/dashboard). Les statistiques et les graphiques vous permettent de savoir où en sont vos applications: combien de clients vous avez atteint, la façon dont ils utilisent votre application et ce qu’ils en pensent. Vous pouvez également obtenir des métriques sur l’intégrité de l’application, l’utilisation des publicités, etc.

Vous pouvez afficher les rapports d’analyse directement dans l’espace partenaires ou [télécharger les rapports que vous avez besoin](download-analytic-reports.md) pour analyser les données en mode hors connexion. Nous proposerons également plusieurs méthodes pour vous à [accéder à vos données d’analytique en dehors de l’espace partenaires](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Visualiser les principales analyses de toutes vos applications

Pour visualiser les principales analyses relatives à vos applications les plus téléchargées, développez **Analyser**, puis sélectionnez **Vue d’ensemble**. Par défaut, la page Vue d’ensemble affiche des informations sur les cinq applications qui ont été acquises le plus souvent au cours de leur durée de vie. Pour choisir d’autres applications publiées à afficher, sélectionnez **Filtres**.

## <a name="view-individual-reports-for-each-app"></a>Visualiser des rapports individuels pour chaque application

Cette section détaille les informations présentées dans chacun des rapports suivants:

-   [Rapport sur les acquisitions](acquisitions-report.md)
-   [Rapport sur les acquisitions d’extensions](add-on-acquisitions-report.md)
-   [Rapport d’utilisation](usage-report.md)
-   [Rapport sur l’intégrité](health-report.md)
-   [Rapport sur les évaluations](ratings-report.md)
-   [Rapport sur les révisions](reviews-report.md)
-   [Rapport sur les commentaires](feedback-report.md)
-   [Rapport d’analyse Xbox](xbox-analytics-report.md)
-   [Rapport de perspectives](insights-report.md)
-   [Rapport sur les performances publicitaires](advertising-performance-report.md)
-   [Rapport de campagne de publicité](promote-your-app-report.md)


> [!NOTE]
> Étant donné que les données qui figurent dans ces rapports dépendent des fonctionnalités et de l’implémentation propres à votre application, certains rapports n’en contiennent pas.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Accéder aux données d’analytique en dehors de l’espace partenaires

En plus de l’affichage des rapports dans l’espace partenaires, vous pouvez accéder analytique d’application dans d’autres cas possibles.

### <a name="microsoft-store-analytics-api"></a>API d'analyse du Microsoft Store

Utilisez [l’API d’analyse du Store](../monetize/access-analytics-data-using-windows-store-services.md) pour récupérer par programme les données d’analyse de vos applications. Cette API REST permet de récupérer les données pour les acquisitions de modules complémentaires et d’applications, les erreurs, ainsi que les évaluations et avis relatifs aux applications. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels à partir de votre application ou service.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Pack de contenu du Centre de développement Windows pour PowerBI

Utiliser le [pack de contenu du centre de développement Windows pour Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) pour découvrir et surveiller vos données d’analytique de l’espace partenaires dans Power BI. PowerBI est un service d’analyse métier basé sur le cloud qui vous offre une vue unique de vos données d’entreprise.

Utilisez les ressources suivantes pour commencer à utiliser PowerBI pour accéder à vos données d’analyse.

* [S’inscrire à PowerBI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Découvrez comment utiliser PowerBI](https://powerbi.microsoft.com/guided-learning/)
* [Découvrez comment utiliser le pack de contenu du Centre de développement Windows pour PowerBI pour se connecter à vos données d’analyse](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Pour connecter le pack de contenu du centre de développement Windows pour Power BI, nous vous recommandons de spécifier les informations d’identification à partir d’un annuaire Azure AD associé à votre compte espace partenaires. Si vous utilisez vos informations d’identification de compte Microsoft, vos données d’analyse dans PowerBI ne sont pas actualisées automatiquement et vous devez vous connecter à PowerBI pour actualiser vos données. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](http://go.microsoft.com/fwlink/p/?LinkId=703757). Pour plus d’informations sur la configuration de l’association, voir [Associer Azure Active Directory à votre compte espace partenaires](associate-azure-ad-with-dev-center.md).
