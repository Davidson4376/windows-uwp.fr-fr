---
author: jnHs
Description: "Pour afficher les données de performances pour les unités publicitaires dans vos applications, utilisez les rapports sur les performances publicitaires au niveau du compte et de l’application dans le tableau de bord du Centre de développementWindows."
title: Rapport sur les performances publicitaires
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: c46533c6762ddd2f47a4dbd40c253bc3d8f346d7
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires


Le **rapport sur les performances publicitaires** vous renseigne sur l’efficacité de vos [unités publicitaires](monetize-with-ads.md#available-ad-units), comprenant les publicités de la communauté et les annonces des affiliés. Ce rapport comporte les données provenant de plusieurs fournisseurs de publicités dans les applications UWP qui utilisent la [médiation publicitaire](monetize-with-ads.md#mediation). 

Pour visualiser ce rapport, développez **Analyser** dans le menu de navigation de gauche, puis sélectionnez **Performances de la publicité**. 

Pour que vous puissiez procéder à analyse plus approfondie de vos données, nous vous fournissons un lien **Télécharger le rapport** qui vous permet de télécharger des fichiersCSV (valeurs séparées par des virgules) que vous pouvez ouvrir dans MicrosoftExcel ou dans un autre programme. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir les données relatives aux performances publicitaires](../monetize/get-ad-performance-data.md) dans [l’API REST d’analyse du WindowsStore](../monetize/access-analytics-data-using-windows-store-services.md).

Lorsque vous visualisez les rapports sur les performances publicitaires, notez que les données des rapports portant sur les trois derniers jours sont susceptibles de changer à mesure que nous recevons et traitons de nouvelles données provenant de différentes sources. En outre, les données datant de jusqu’à 90jours peuvent faire l’objet d’un ajustement.


## <a name="overall-performance"></a>Performances globales

En haut du rapport, vous pouvez utiliser les filtres ci-après pour ajuster l’étendue des données affichées dans le rapport:

* **Date**: permet de filtrer le rapport en fonction d’une période prédéfinie ou d’une plage de dates personnalisée. Par défaut, le rapport affiche les données des 30derniers jours.
* **Agrégation**: permet de sélectionner la façon dont ces données sont agrégées et dont elles peuvent être filtrées davantage. Par défaut, le rapport affiche les données provenant de toutes les unités publicitaires, et un lien **Choisissez des unités publicitaires** au bas de la section vous permet de sélectionner jusqu’à six unités publicitaires à comparer. Si vous le souhaitez, vous pouvez redéfinir le filtre **Agrégation** sur l’option **Toutes les applications** ou **All ad providers**. Si vous procédez ainsi, le lien de cette section devient **Choisir les applications** ou **Choose ad providers** et vous permet de choisir jusqu’à six applications ou fournisseurs à comparer. Vous pouvez également choisir d’agréger les données en fonction d’une application spécifique dans laquelle vous utilisez des publicités.
* **Fournisseurs de publicités**: permet de filtrer le rapport en fonction des données de performances de certains fournisseurs de publicités. Pour plus d’informations sur les fournisseurs de publicités disponibles, consultez la section [Médiation publicitaire](monetize-with-ads.md#mediation) de l’article [Monétiser à l’aide des publicités](monetize-with-ads.md). Par défaut, le rapport affiche les données provenant de tous les fournisseurs de publicités. Si vous avez choisi l’option **All ad providers** dans la liste déroulante **Agrégation**, cette option est désactivée.
* **Appareil**: permet de filtrer le rapport en fonction des données de performances de certains types d’appareils. Par défaut, le rapport affiche les données de tous les types d’appareils.


## <a name="report-views"></a>Affichages du rapport

Sous les filtres, le rapport affiche les données issues de diverses métriques de performances publicitaires sous forme de graphique, de carte du monde et de tableau pour les unités publicitaires utilisées dans l’application actuelle.

Pour analyser les données de l’une de ces métriques dans une vue sous forme de graphique ou de carte du monde, cliquez sur **Graphique** ou sur **Carte**. Par défaut, les vues graphique et carte affichent les données de performances de l’ensemble des unités publicitaires, applications ou fournisseurs de publicités (selon les options que vous avez sélectionnées dans la liste déroulante **Agrégation**), mais vous avez la possibilité de sélectionner jusqu’à six unités publicitaires, applications ou fournisseurs de publicités à comparer.

Dans la vue carte, les nuances plus foncées représentent les valeurs les plus élevées, tandis que les nuances plus claires représentent les valeurs les plus faibles. Vous pouvez pointer sur un pays ou une région sur la carte pour analyser la valeur de la métrique sélectionnée. Vous pouvez également effectuer un zoom avant sur une zone de la carte pour afficher les données des plus petits pays.

Pour examiner toutes les métriques de performances relatives aux unités publicitaires intégrées dans votre application, reportez-vous au tableau figurant sous les vues graphique et carte.

> [!NOTE]
> Si vous avez créé des unités publicitaires pour une application à l’aide de MicrosoftpubCenter, il se peut qu’elles n’aient pas toutes été correctement mappées sur vos applications dans le Centre de développement. Dans ce rapport, les unités publicitaires sont associées à des noms d’application que vous avez spécifiés dans pubCenter, avec la chaîne **(pubCenter)** ajoutée au nom de l’application.


## <a name="performance-metrics"></a>Métriques de performances

Ce rapport peut inclure des données relatives aux métriques de performances ci-après. Les métriques affichées dans le rapport varient selon le fournisseur de publicités.

|  Métrique  |  Description  |
|----------|---------------|
| Estimation des revenus  |  Estimation des revenus générés par les publicités diffusées dans votre application. |
| eCPM  |  Coût par impression électronique effectif pour mille impressions. |
| Demandes  | Nombre de fois où une demande de publicité a été envoyée à partir de votre application.  |
| Impressions  | Nombre de fois où une publicité a été affichée dans votre application.  |
| Taux de remplissage  | Pourcentage de demandes de publicité envoyées à partir de votre application dans lesquelles une publicité était affichée.  |
| Clics  |  Nombre de fois où quelqu’un a cliqué sur une publicité dans votre application. |
| CTR  |  Taux de clic: nombre de fois où quelqu’un a cliqué sur une publicité divisé par le nombre d’impressions. |
| Crédits obtenus  | Pour les [annonces de la Communauté](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), cela indique le nombre de crédits que vous avez gagnés pour un espace publicitaire promotionnel en affichant des annonces de la Communauté dans votre application.  |
| Crédits dépensés  | Dans le cas des [publicités de la communauté](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), cette valeur indique le nombre de crédits que vous avez dépensés pour des publicités pour votre application.  |


## <a name="affiliates-performance"></a>Performances des affiliés

Si vous avez [accepté de participer au programme des annonces des affiliés de Microsoft](about-affiliate-ads.md), vous pouvez visualiser ici les données de performances relatives aux annonces des affiliés qui apparaissent dans votre application. Ces informations sont mises à jour quotidiennement. 


En haut du rapport, vous pouvez utiliser les filtres ci-après pour ajuster l’étendue des données affichées dans le rapport:
- **Date**: permet de filtrer le rapport en fonction d’une période prédéfinie ou d’une plage de dates personnalisée. Par défaut, le rapport affiche les données des 30derniers jours.
- **Appareil**: permet de filtrer le rapport en fonction des données de performances de certains types d’appareils. Par défaut, le rapport affiche les données de tous les types d’appareils.

Par défaut, ce rapport fournit un résumé sous forme graphique et tabulaire des données de performances relatives aux annonces des affiliés dans toutes les applications que vous avez inscrites au programme d’annonces des affiliés de Microsoft. Vous pouvez sélectionner **Choisir les applications** pour sélectionner jusqu’à six applications à comparer.

Les données de performances des affiliés sont obtenues à partir des sept métriques de performances ci-après que nous suivons pour les annonces des affiliés intégrées dans votre application:

-   **Revenus estimés (approuvés)**: l’estimation des revenus sous forme de commissions que vous avez reçues pour les achats approuvés, faits par des utilisateurs ayant cliqué sur les annonces des affiliés dans votre application.
-   **Revenus estimés (en attente d’approbation)**: somme d’argent que vous pourriez recevoir en guise de commission pour les achats en attente d’approbation.
-   **Impressions**: nombre de fois où une annonce des affiliés a été affichée dans votre application.
-   **Clics** : nombre de fois où quelqu’un a cliqué sur une annonce des affiliés dans votre application.
-   **Taux de clic** : nombre de fois où quelqu’un a cliqué sur une annonce des affiliés divisé par le nombre d’impressions des annonces des affiliés.
-   **Achats (approuvés)** : le nombre d’achats approuvés effectués par les utilisateurs ayant cliqué sur les annonces des affiliés dans votre application.
-   **Achats (en attente d’approbation)**: le nombre d’achats en attente d’approbation effectués par les utilisateurs ayant cliqué sur les annonces des affiliés dans votre application.

> [!NOTE]
> Après qu’un utilisateur achète un produit dans le Windows Store, il existe un délai d’attente de 45jours avant l’approbation de l’achat pour le programme d’annonce des affiliés. En raison de ce délai d’attente, les données **Revenus estimés (approuvés)**, **Revenus estimés (en attente d’approbation)**, **Achats (approuvés)** et **Achats (en attente d’approbation)** pour un jour donné peuvent changer après l’approbation ou le rejet des achats.


 
