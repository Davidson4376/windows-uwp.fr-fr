---
author: jnHs
Description: To view performance data for the ad units in your apps, use the advertising performance report on the Windows Dev Center dashboard.
title: Rapport sur les performances publicitaires
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 01/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: high
ms.openlocfilehash: b19ed22e00721bd06f902ea3087a52eb58e5ab8a
ms.sourcegitcommit: 67cb03db41556cf0d58993073654cd0706aede84
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2018
---
# <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires


Le **rapport sur les performances publicitaires** vous renseigne sur l’efficacité de vos [unités publicitaires](in-app-ads.md), comprenant les publicités de la communauté. Ce rapport comporte les données provenant de plusieurs fournisseurs de publicités dans les applications UWP qui utilisent la [médiation publicitaire](in-app-ads.md#mediation).

Pour visualiser ce rapport, développez **Analyser** dans le menu de navigation de gauche, puis sélectionnez **Performances de la publicité**. Vous pouvez afficher ces données dans votre tableau de bord, ou télécharger les données du rapport pour les consulter hors connexion en cliquant sur les icônes en forme de flèche de la page. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir les données relatives aux performances publicitaires](../monetize/get-ad-performance-data.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

Lorsque vous visualisez les rapports sur les performances publicitaires, notez que les données des rapports portant sur les trois derniers jours sont susceptibles de changer à mesure que nous recevons et traitons de nouvelles données provenant de différentes sources. En outre, les données datant de jusqu’à 90jours peuvent faire l’objet d’un ajustement.

## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de 30D (30jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12mois, ou sur une plage de dates personnalisée que vous spécifiez.

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par unité publicitaire, application, fournisseur de publicité et type d’appareil. Vous pouvez choisir parmi les options suivantes:

* **Agrégation**: choisissez la façon dont les données du rapport sont agrégées et dont elles peuvent être filtrées davantage. Par défaut, ce filtre est défini sur **Toutes les unités publicitaires**. Vous pouvez éventuellement modifier ce filtre sur **Toutes les applications** ou **Tous les fournisseurs de publicité**, ou vous pouvez choisir de pratiquer l'agrégation selon une application particulière dans laquelle vous utilisez des publicités.
* **Fournisseurs de publicités**: permet de filtrer le rapport en fonction des données de performances de certains [fournisseurs de publicités](in-app-ads.md#paid-networks). Par défaut, le rapport affiche les données provenant de tous les fournisseurs de publicités. Si vous avez choisi l’option **All ad providers** dans la liste déroulante **Agrégation**, cette option est désactivée.
* **Appareil**: permet de filtrer le rapport en fonction des données de performances de certains types d’appareils. Par défaut, le rapport affiche les données de tous les types d’appareils.

## <a name="overall-performance"></a>Performances globales

Cette section affiche les mesures de performance publicitaire dans un graphique ou une carte du monde pour les unités publicitaires, les applications et les fournisseurs de publicités que vous avez sélectionnés dans les filtres du rapport.

Pour basculer vers un autre affichage des données, cliquez sur **Graphique** ou **Carte** en haut de la section **Performances globales**. Dans la vue Carte, les nuances plus foncées représentent les valeurs les plus élevées, tandis que les nuances les plus claires représentent les valeurs les plus faibles. Vous pouvez pointer sur un pays ou une région sur la carte pour analyser la valeur de la métrique sélectionnée. Vous pouvez également effectuer un zoom avant sur une zone de la carte pour afficher les données des plus petits pays.

Vous pouvez affiner les données affichées dans le graphique ou la carte en cliquant sur l’icône de filtre en regard de la liste déroulante **Graphique** ou **Carte**. Ce filtre permet de choisir jusqu'à six différentes unités publicitaires, applications ou fournisseurs de publicités pour les comparer dans l’affichage graphique ou de la carte. Le type de données que vous pouvez choisir à partir de ce filtre varie selon ce que vous avez sélectionné pour le filtre de niveau supérieur **Agrégation** en haut du rapport.


## <a name="overall-performance-breakdown"></a>Répartition des performances globales

Cette section affiche un tableau qui contient toutes les mesures de performances publicitaires pour le jeu de données spécifié par les filtres que vous avez sélectionnés dans le rapport.

## <a name="performance-metrics"></a>Indicateurs de performances

Le **Rapport sur les performances publicitaires** comprend des données correspondant aux indicateurs de performance suivants. Certains indicateurs ne sont affichés que pour certains fournisseurs de publicités.

|  Métrique  |  Description  |
|----------|---------------|
| Estimation des revenus  |  Estimation des revenus générés par les publicités diffusées dans votre application. |
| eCPM  |  Coût par impression électronique effectif pour mille impressions. |
| Demandes  | Nombre de fois où une demande de publicité a été envoyée à partir de votre application.  |
| Impressions  | Nombre de fois où une publicité a été affichée dans votre application.  |
| Taux de remplissage  | Pourcentage de demandes de publicité envoyées à partir de votre application dans lesquelles une publicité était affichée.  |
| Clics  |  Nombre de fois où quelqu’un a cliqué sur une publicité dans votre application. |
| CTR  |  Taux de clic: nombre de fois où quelqu’un a cliqué sur une publicité divisé par le nombre d’impressions. |
| Crédits obtenus  | Si vous exécutez une campagne [publicité de la communauté](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), cette valeur indique le nombre de crédits que vous avez gagnés pour un espace publicitaire promotionnel en affichant la publicité de la communauté dans votre application.  |
| Crédits dépensés  | Si vous exécutez une campagne [publicité de la communauté](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), cette valeur indique le nombre de crédits que vous avez dépensés pour des publicités pour votre application.  |

## <a name="related-topics"></a>Rubriquesassociées

* [Publicités dans l’application](in-app-ads.md)
* [Afficher des publicités dans votre application avec le SDK MicrosoftAdvertising](../monetize/display-ads-in-your-app.md)


 
