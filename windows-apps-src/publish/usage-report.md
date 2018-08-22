---
author: jnHs
Description: The Usage report in the Windows Dev Center dashboard lets you see how customers are using your app.
title: Rapport d’utilisation
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, utilisation, événement personnalisé, rapport, télémétrie, sessions utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: 96d36ebbaa2b7f1a650e2b0f794a1976c1f525a6
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2800360"
---
# <a name="usage-report"></a>Rapport d’utilisation


Le rapport **Utilisation** disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients sur Windows10 (y compris Xbox) utilisent votre application, et vous présente des informations sur les événements personnalisés que vous avez définis. Vous pouvez visualiser ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **30D** (30jours), mais vous pouvez choisir d’afficher les données portant sur des périodes de 3, 6 ou 12mois, ou sur une plage de dates personnalisée que vous spécifiez.

Vous pouvez également développer l’option **Filtres** pour filtrer les données de cette page par version de package, par marché et/ou par type d’appareil.

-   **Version du package**: le paramétrage par défaut est **Toutes**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Marché**: le filtre par défaut est **Tous les marchés**, mais vous pouvez restreindre les données à un ou plusieurs marchés.
-   **Type d’appareil**: le paramétrage par défaut est **Tous**, mais vous pouvez choisir d’afficher uniquement les données relatives à un type d’appareil spécifique (PC, console ou tablette, par exemple).

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et aux filtres que vous avez sélectionnés (à l’exception des données **Nouveaux utilisateurs** du graphique **Utilisation** qui n’apparaissent pas si des filtres sont sélectionnés). Certaines sections vous permettent également d’appliquer des filtres supplémentaires.

> [!IMPORTANT]
> Ce rapport intègre uniquement les données d’utilisation des clients sur Windows10 (y compris Xbox) qui n’ont pas refusé de fournir des informations de télémétrie. Les données d’utilisation pour les jeux Xbox sont incluses ici, que le client ait été connecté à Xbox Live ou non. 


## <a name="usage"></a>Utilisation

Le graphique **Utilisation** affiche des détails sur la façon dont vos clients utilisent votre application au cours de la période sélectionnée. Notez que ce graphique n’effectue pas le suivi des utilisateurs uniques de votre application ni des sessions utilisateur uniques (autrement dit, les utilisateurs qui figurent dans ce graphique peuvent avoir utilisé votre application une ou plusieurs fois).

Ce graphique comporte quatre onglets visualisables qui vous présentent l’utilisation par jour ou par semaine (selon la durée que vous avez sélectionnée).

- **Utilisateurs**: affiche le nombre total de **sessions utilisateur** au cours de la période sélectionnée. Chaque session utilisateur représente une période de temps distincte, qui commence au lancement de l’application (début du processus) et qui se termine à l’arrêt de l’application (fin du processus) ou après une période d’inactivité. Pour cette raison, un client unique peut avoir plusieurs sessions utilisateur sur un même jour ou une même semaine. Le nombre total **d’utilisateurs actifs** (tout client utilisant l’application ce jour ou cette semaine-là) et de **nouveaux utilisateurs** (clients ayant utilisé votre application pour la première fois ce jour ou cette semaine-là) est également affiché. Notez que si vous avez appliqué des filtres à la page, la valeur **Nouveaux utilisateurs** n’apparaît pas dans ce graphique.
- **Appareils**: affiche le nombre d’appareils utilisés quotidiennement par tous les utilisateurs pour interagir avec votre application.
- **Durée**: indique le nombre total d’heures d’engagement (heures pendant lesquelles un utilisateur utilise activement votre application).
- **Rétention**: affiche le nombre total **d’utilisateurs actifs quotidiennement/d’utilisateurs actifs mensuels** au cours de la période sélectionnée.

Lors de la période de **30 D** est sélectionnée, vous pouvez voir marques en forme de cercle lors de l’affichage des onglets **des utilisateurs**, des **périphériques**ou **durée** . Ces représentent une augmentation significative ou réduire dans une valeur de donnée qui nous pensons que vous souhaiterez savoir à propos de. La date à laquelle le cercle apparaît représente la fin de la semaine dans lequel nous a détecté une augmentation significative ou une baisse par rapport à la semaine avant que. Pour afficher plus d’informations sur ce qui a changé, pointez sur le cercle.  

> [!TIP]
> Vous pouvez afficher des conseils supplémentaires liées à des modifications significatives au cours des 30 derniers jours dans le [rapport de détails](insights-report.md).


## <a name="user-sessions"></a>Sessions utilisateur

Le graphique **Sessions utilisateur** affiche le nombre total de sessions utilisateur de votre application par marché au cours de la période sélectionnée.

Comme dans le cas des informations **Sessions utilisateur** du graphique **Utilisation**, une session utilisateur représente une période donnée au cours de laquelle un client a interagi avec votre application, et ce graphique n’effectue pas le suivi des utilisateurs uniques de votre application.

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal de sessions utilisateur. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="package-version"></a>Version du package

Le graphique **Sessions utilisateur** affiche le nombre total de sessions utilisateur quotidiennes de votre application par version de package au cours de la période sélectionnée.

Comme dans le cas du graphique **Sessions utilisateur**, une session utilisateur représente une période donnée au cours de laquelle un client a interagi avec votre application, et ce graphique n’effectue pas le suivi des utilisateurs uniques de votre application.


## <a name="custom-events"></a>Événements personnalisés

Le graphique **Événements personnalisés** affiche le nombre total d’occurrences d’événements personnalisés que vous avez définis pour votre application. Ces informations peuvent concerner plusieurs occurrences pour un même client. Vous pouvez utiliser les filtres pour sélectionner des événements personnalisés spécifiques pour lesquels vous souhaitez visualiser ces données.

Les événements personnalisés sont implémentés à l’aide de la méthode [StoreServicesCustomEventLogger.Log](https://docs.microsoft.com/en-us/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) de [MicrosoftStore Services SDK](../monetize/microsoft-store-services-sdk.md).

Pour plus d’informations, consultez l’article [Consigner des événements personnalisés pour le Centre de développement](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Répartition des événements personnalisés

Le graphique **Répartition des événements personnalisés** affiche des détails supplémentaires sur la fréquence à laquelle chacun de vos événements personnalisés est survenu. Cela peut vous aider à déterminer si des événements se produisent plus souvent pour un marché, un type d’appareil ou des versions de package spécifiques.

Pour chaque événement, le graphique indique le nom de l’événement et le nombre d’événements correspondant à une combinaison spécifique de marché, de type d’appareil et de version du package de l’utilisateur. En règle générale, un événement est répertorié à plusieurs reprises avec différentes combinaisons de ces facteurs. 




 
