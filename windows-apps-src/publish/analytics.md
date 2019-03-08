---
Description: Obtenir analytique détaillées pour vos applications Windows, dans l’espace partenaires ou par d’autres méthodes.
title: Analyser les performances de l’application
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, analytique, rapports, tableau de bord, applications, données, mesures
ms.localizationpriority: medium
ms.openlocfilehash: 6f76b1f897c345fb71beec8e37e592165922b2ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625444"
---
# <a name="analyze-app-performance"></a>Analyser les performances de l’application

Vous pouvez afficher une analytique détaillée de vos applications dans [partenaires](https://partner.microsoft.com/dashboard). Les statistiques et les graphiques vous permettent de savoir où en sont vos applications : combien de clients vous avez atteint, la façon dont ils utilisent votre application et ce qu’ils en pensent. Vous pouvez également obtenir des métriques sur l’intégrité de l’application, l’utilisation des publicités, etc.

Vous pouvez afficher des rapports analytiques directement dans l’espace partenaires ou [télécharger les rapports que vous avez besoin](download-analytic-reports.md) pour analyser vos données en mode hors connexion. Nous offrons également plusieurs façons pour vous [accéder à vos données d’analytique en dehors de partenaires](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Visualiser les principales analyses de toutes vos applications

Pour visualiser les principales analyses relatives à vos applications les plus téléchargées, développez **Analyser**, puis sélectionnez **Vue d’ensemble**. Par défaut, la page Vue d’ensemble affiche des informations sur les cinq applications qui ont été acquises le plus souvent au cours de leur durée de vie. Pour choisir d’autres applications publiées à afficher, sélectionnez **Filtres**.

## <a name="view-individual-reports-for-each-app"></a>Visualiser des rapports individuels pour chaque application

Cette section détaille les informations présentées dans chacun des rapports suivants :

-   [Rapport sur les acquisitions](acquisitions-report.md)
-   [Rapport sur les acquisitions d’extensions](add-on-acquisitions-report.md)
-   [Rapport d’utilisation](usage-report.md)
-   [Rapport d’intégrité](health-report.md)
-   [Rapport de contrôle d’accès](ratings-report.md)
-   [Rapport Avis](reviews-report.md)
-   [Rapport Commentaires](feedback-report.md)
-   [Rapport d’analytique de Xbox](xbox-analytics-report.md)
-   [Rapport d’Insights](insights-report.md)
-   [Rapport sur les performances publicitaires](advertising-performance-report.md)
-   [Rapport de campagne publicitaire](promote-your-app-report.md)


> [!NOTE]
> Les données figurant dans ces rapports dépendant des fonctionnalités et de l’implémentation propres à votre application, certains rapports n’en contiennent pas.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Données d’analytique de l’accès en dehors de partenaires

Outre l’affichage des rapports sur les partenaires, vous pouvez accéder à analytique d’application par d’autres moyens.

### <a name="microsoft-store-analytics-api"></a>API d'analyse du Microsoft Store

Utilisez [l’API d’analyse du Store](../monetize/access-analytics-data-using-windows-store-services.md) pour récupérer par programme les données d’analyse de vos applications. Cette API REST permet de récupérer les données pour les acquisitions de modules complémentaires et d’applications, les erreurs, ainsi que les évaluations et avis relatifs aux applications. Cette API utilise Azure Active Directory (Azure AD) pour authentifier les appels en provenance de votre application ou service.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Pack de contenu du Centre de développement Windows pour Power BI

Utilisez le [pack de contenu de centre de développement Windows pour Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) à Explorer et analyser vos données d’analytique de partenaires dans Power BI. Power BI est un service d’analyse métier basé sur le cloud qui vous offre une vue unique de vos données d’entreprise.

Utilisez les ressources suivantes pour commencer à utiliser Power BI pour accéder à vos données d’analyse.

* [S’inscrire à Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Découvrez comment utiliser Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Découvrez comment utiliser le pack de contenu de centre de développement Windows pour Power BI pour se connecter à vos données d’analytique](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Pour vous connecter au pack de contenu de centre de développement Windows pour Power BI, nous vous recommandons de spécifier les informations d’identification à partir d’un annuaire Azure AD qui est associé à votre compte espace partenaires. Si vous utilisez vos informations d’identification de compte Microsoft, vos données d’analyse dans Power BI ne sont pas actualisées automatiquement et vous devez vous connecter à Power BI pour actualiser vos données. Si votre organisation utilise déjà Office 365 ou d’autres services professionnels de Microsoft, vous disposez déjà d’Azure AD. Sinon, vous pouvez l’[obtenir gratuitement](https://go.microsoft.com/fwlink/p/?LinkId=703757). Pour plus d’informations sur la configuration de l’association, consultez [associer Azure Active Directory avec votre compte espace partenaires](associate-azure-ad-with-dev-center.md).
