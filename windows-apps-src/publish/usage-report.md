---
author: jnHs
Description: "Le rapport Utilisation disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application."
title: "Rapport sur l’utilisation"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 08/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: b3e7154a9dc8b7ecea8a319300ab482f61e0bb68
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2017
---
# <a name="usage-report"></a>Rapport sur l’utilisation


Le rapport **Utilisation** disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients sur Windows10 utilisent votre application, et vous présente des informations sur les événements personnalisés que vous avez définis. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **30D** (30jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12mois, ou sur une plage de dates personnalisée que vous spécifiez.

Vous pouvez également développer l’option **Filtres** pour filtrer les données de cette page par version de package, par marché et/ou par type d’appareil.

-   **Version du package**: le paramétrage par défaut est **Toutes**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Marché**: le filtre par défaut est **Tous les marchés**, mais vous pouvez restreindre les données aux acquisitions relatives à un ou plusieurs marchés.
-   **Type d’appareil** : le paramètre par défaut est **Tous**, mais vous pouvez choisir d’afficher les données pour un seul type d’appareil donné.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et aux filtres que vous avez sélectionnés (à l’exception des données **Nouveaux utilisateurs** du graphique **Utilisation** qui n’apparaissent pas si des filtres sont sélectionnés). Certaines sections vous permettent également d’appliquer des filtres supplémentaires.

> [!IMPORTANT]
> Ce rapport intègre uniquement les données d’utilisation des clients sur Windows10 qui n’ont pas refusé de fournir des informations de télémétrie.


##<a name="usage"></a>Utilisation

Le graphique **Utilisation** affiche des détails sur la façon dont vos clients utilisent votre application au cours de la période sélectionnée. Notez que ce graphique n’effectue pas le suivi des utilisateurs uniques de votre application ni des sessions utilisateur uniques (autrement dit, les utilisateurs qui figurent dans ce graphique peuvent avoir utilisé votre application une ou plusieurs fois).

Ce graphique comporte quatre onglets visualisables qui vous présentent l’utilisation par jour ou par semaine (selon la durée que vous avez sélectionnée).

- **Utilisateurs**: affiche le nombre total de **sessions utilisateur** au cours de la période sélectionnée. Chaque session utilisateur représente une période distincte pendant laquelle un client a interagi avec votre application. Chaque session utilisateur est considérée comme prenant fin après une période d’inactivité, de sorte qu’un même client peut être associé à plusieurs sessions utilisateur dans la même journée ou semaine. Le nombre total **d’utilisateurs actifs** (tout client utilisant l’application ce jour ou cette semaine-là) et de **nouveaux utilisateurs** (clients ayant utilisé votre application pour la première fois ce jour ou cette semaine-là) est également affiché. Notez que si vous avez appliqué des filtres à la page, la valeur **Nouveaux utilisateurs** n’apparaît pas dans ce graphique.
- **Appareils**: affiche le nombre d’appareils utilisés quotidiennement par tous les utilisateurs pour interagir avec votre application.
- **Durée**: indique le nombre total de minutes d’engagement (minutes pendant lesquelles un utilisateur utilise activement votre application).
- **Rétention**: affiche le nombre total **d’utilisateurs actifs quotidiennement/d’utilisateurs actifs mensuels** au cours de la période sélectionnée.


## <a name="user-sessions"></a>Sessions utilisateur

Le graphique **Sessions utilisateur** affiche le nombre total de sessions utilisateur de votre application par marché au cours de la période sélectionnée.

Comme dans le cas des informations **Sessions utilisateur** du graphique **Utilisation**, une session utilisateur représente une période donnée au cours de laquelle un client a interagi avec votre application, et ce graphique n’effectue pas le suivi des utilisateurs uniques de votre application.

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal de sessions utilisateur. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="package-version"></a>Version du package

Le graphique **Sessions utilisateur** affiche le nombre total de sessions utilisateur quotidiennes de votre application par version de package au cours de la période sélectionnée.

Comme dans le cas du graphique **Sessions utilisateur**, une session utilisateur représente une période donnée au cours de laquelle un client a interagi avec votre application, et ce graphique n’effectue pas le suivi des utilisateurs uniques de votre application.


## <a name="custom-events"></a>Événements personnalisés

Le graphique **Événements personnalisés** affiche le nombre total d’occurrences d’événements personnalisés que vous avez définis pour votre application. Ces informations peuvent concerner plusieurs occurrences pour un même client. Vous pouvez utiliser les filtres pour sélectionner des événements personnalisés spécifiques pour lesquels vous souhaitez visualiser ces données.

Les événements personnalisés sont implémentés à l’aide de la méthode [StoreServicesCustomEventLogger.Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) de [MicrosoftStore Services SDK](../monetize/microsoft-store-services-sdk.md).

Pour plus d’informations, consultez l’article [Consigner des événements personnalisés pour le Centre de développement](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Répartition des événements personnalisés

Le graphique **Répartition des événements personnalisés** affiche des détails supplémentaires sur la fréquence à laquelle chacun de vos événements personnalisés est survenu. Cela peut vous aider à déterminer si des événements se produisent plus souvent pour un marché, un type d’appareil ou des versions de package spécifiques.

Pour chaque événement, le graphique indique le nom de l’événement et le nombre d’événements correspondant à une combinaison spécifique de marché, de type d’appareil et de version du package de l’utilisateur. En règle générale, un événement est répertorié à plusieurs reprises avec différentes combinaisons de ces facteurs. 




 
