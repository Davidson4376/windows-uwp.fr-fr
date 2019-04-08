---
Description: Pour afficher les données de performances pour les unités ad dans vos applications, utilisez le rapport de performances de publicité dans Partner Center.
title: Rapport sur les performances publicitaires
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a96f6f6593a8ccc6714f67b6f825a6416750b432
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640274"
---
# <a name="advertising-performance-report"></a>Rapport sur les performances publicitaires


Le **publier le rapport de performances** dans [partenaires](https://partner.microsoft.com/dashboard) montre comment votre [d’unités publicitaires](in-app-ads.md) effectuez, y compris les publicités de la Communauté. Ce rapport comporte les données provenant de plusieurs fournisseurs de publicités dans les applications UWP qui utilisent la [médiation publicitaire](in-app-ads.md#mediation).

Pour visualiser ce rapport, développez **Analyser** dans le menu de navigation de gauche, puis sélectionnez **Performances de la publicité**. Vous pouvez afficher ces données dans l’espace partenaires, ou télécharger les données du rapport à afficher en mode hors connexion en cliquant sur les icônes de flèche sur la page. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [Obtenir les données relatives aux performances publicitaires](../monetize/get-ad-performance-data.md) dans notre [API REST d’analyse](../monetize/access-analytics-data-using-windows-store-services.md).

Lorsque vous visualisez les rapports sur les performances publicitaires, notez que les données des rapports portant sur les trois derniers jours sont susceptibles de changer à mesure que nous recevons et traitons de nouvelles données provenant de différentes sources. En outre, les données datant de jusqu’à 90 jours peuvent faire l’objet d’un ajustement.

## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de 30D (30 jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12 mois, ou sur une plage de dates personnalisée que vous spécifiez.

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par unité publicitaire, application, fournisseur de publicité et type d’appareil. Vous pouvez choisir parmi les options suivantes :

* **Agrégation**: Choisissez la façon dont les données de rapport sont agrégées et comment il peut être filtré davantage. Par défaut, ce filtre est défini sur **Toutes les unités publicitaires**. Vous pouvez éventuellement modifier ce filtre sur **Toutes les applications** ou **Tous les fournisseurs de publicité**, ou vous pouvez choisir de pratiquer l'agrégation selon une application particulière dans laquelle vous utilisez des publicités.
* **Les fournisseurs AD**: Filtrer le rapport pour les données de performances pour certaines [les fournisseurs ad](in-app-ads.md#paid-networks). Par défaut, le rapport affiche les données provenant de tous les fournisseurs de publicités. Si vous avez choisi l’option **All ad providers** dans la liste déroulante **Agrégation**, cette option est désactivée.
* **APPAREIL**: Filtrer le rapport pour les données de performances pour certains types d’appareils. Par défaut, le rapport affiche les données de tous les types d’appareils.

## <a name="overall-performance"></a>Performances globales

Cette section affiche les mesures de performance publicitaire dans un graphique ou une carte du monde pour les unités publicitaires, les applications et les fournisseurs de publicités que vous avez sélectionnés dans les filtres du rapport.

Pour basculer vers un autre affichage des données, cliquez sur **Graphique** ou **Carte** en haut de la section **Performances globales**. Dans la vue carte, les nuances plus foncées représentent des valeurs plus élevées et les nuances plus légères représentent des valeurs inférieures. Vous pouvez pointer sur un pays ou une région sur la carte pour analyser la valeur de la métrique sélectionnée. Vous pouvez également effectuer un zoom avant sur une zone de la carte pour afficher les données des plus petits pays.

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
| CTR  |  Taux de clic : nombre de fois où quelqu’un a cliqué sur une publicité divisé par le nombre d’impressions. |
| La plupart des ordinateurs | Le pourcentage d’exposition publicitaire qui sont visibles dans votre application. Pour plus d’informations sur la façon dont cette valeur est calculée, consultez [optimiser la plupart des ordinateurs de vos unités ad](../monetize/optimize-ad-unit-viewability.md). |
| Crédits obtenus  | Si vous exécutez une campagne [publicité de la communauté](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), cette valeur indique le nombre de crédits que vous avez gagnés pour un espace publicitaire promotionnel en affichant la publicité de la communauté dans votre application.  |
| Crédits dépensés  | Si vous exécutez une campagne [publicité de la communauté](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), cette valeur indique le nombre de crédits que vous avez dépensés pour des publicités pour votre application.  |

## <a name="related-topics"></a>Rubriques connexes

* [Publicités dans l’application](in-app-ads.md)
* [Afficher des publicités dans votre application avec le kit SDK Microsoft Advertising](../monetize/display-ads-in-your-app.md)
* [Optimiser la plupart des ordinateurs de vos unités d’ad](../monetize/optimize-ad-unit-viewability.md)


 
