---
author: jnHs
Description: The Health report in the Windows Dev Center dashboard lets you get data related to the performance and quality of your app, including crashes and unresponsive events.
title: Rapport d’intégrité
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.author: wdg-dev-content
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, intégrité, incidents, blocages, intégrité de l’application, données d’intégrité, trace de pile, fichier cab, échec, échecs, pdb, symboles
ms.localizationpriority: medium
ms.openlocfilehash: 5f5bf63eae4b1504642e764265a7936bcd67c645
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3128580"
---
# <a name="health-report"></a>Rapport d’intégrité

Le rapport **Intégrité** disponible dans le tableau de bord du centre de développement Windows vous permet d’obtenir des données liées aux performances et à la qualité de votre application, y compris les incidents et blocages. Vous pouvez visualiser ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Le cas échéant, vous pouvez visualiser les traces de pile et/ou les fichiersCAB pour un débogage supplémentaire.

Vous pouvez également récupérer par programme les données de ce rapport à l’aide de l’[API REST d’analyse du MicrosoftStore](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **72H** (72heures), mais vous pouvez choisir **30D** à la place si vous souhaitez afficher les données relatives aux 30derniers jours. Notez que les données s’affichent dans votre fuseau horaire local pour l’affichage de **72 H** et au format UTC pour l’affichage de **30D** .

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par version de package, par marché et/ou par type d’appareil.

-   **Version du package**: la valeur par défaut de ce filtre est **Toutes**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Marché**: le filtre par défaut est **Tous les marchés**, mais vous pouvez restreindre les données à un ou plusieurs marchés.
-   **Type d’appareil**: le paramétrage par défaut est **Tous**, mais vous pouvez choisir d’afficher uniquement les données relatives à un type d’appareil spécifique. Notez que la catégorie **Autre** inclut les appareils dont la marque/le modèle est reconnu(e), mais que nous ne sommes pas en mesure d’inclure dans une des catégories prédéfinies pour ce filtre. Pour ces appareils, le modèle d’appareil peut être affiché dans la section **Journal des échecs** du rapport **Détails de l’échec**.  
-   **Version du système d’exploitation**: la valeur par défaut est **Toutes les versions de système d’exploitation**, mais vous pouvez choisir une version de système d’exploitation spécifique.
-   **OS release version**: la valeur par défaut est **All OS release versions**, mais vous pouvez choisir une version spécifique de la **version du système d'exploitation** sélectionnée.
-   **Bac à sable**: la valeur par défaut est **Retail**, mais pour les produits qui utilisent plusieurs bacs à sable de développement (comme les jeux qui s’intègrent à Xbox Live), vous pouvez choisir un bac à sable spécifique ici. (Si votre produit n’utilise pas de bacs à sable, ce filtre affichera uniquement **Retail** et ne sera pas applicable.)
-   **Architecture**: la valeur par défaut est **Toutes les architectures**, mais vous pouvez choisir un type d’architecture système spécifique. Ce filtre est disponible uniquement lorsque **30D** est sélectionné.
-   **PRAID**: le paramètre par défaut est **Tous**, mais si vous avez défini plusieurs ID d’application relative du package (PRAID) lors de la création de votre package d’application, vous pouvez choisir d’afficher uniquement les données associées à un seul PRAID. Ce filtre n’apparaît pas si vous n’avez pas défini plusieurs PRAID.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="failure-hits"></a>Occurrences des échecs

Le graphique **Occurrences des échecs** affiche le nombre d’incidents et d’événements quotidiens recensés par les clients sur votre application au cours de la période sélectionnée. Les différents événements survenus dans votre application font l’objet d’un suivi par type: incidents, absences de réponse, exceptions JavaScript et défaillances mémoire.

Lorsque le **30D** période est activée, vous pouvez voir les marques de cercle. Ces représentent une augmentation importante ou la diminution une valeur donnée dont nous pensons que vous devrez connaître. La date à laquelle le cercle apparaît représente la fin de la semaine dans lequel nous avons détecté une augmentation significative ou une baisse par rapport à la semaine. Pour afficher plus d’informations sur ce qui a changé, survolez le cercle.  

> [!TIP]
> Vous pouvez afficher des informations plus liées à des modifications importantes au cours des 30 derniers jours dans le [rapport de perspectives](insights-report.md).

## <a name="failure-hits-by-market"></a>Occurrences des échecs par marché

Le graphique **Occurrences des échecs par marché** présente le nombre total d’incidents et d’événements survenus par marché au cours de la période sélectionnée.

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal de sessions utilisateur. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="package-version"></a>Version du package

Le graphique **Version du package** présente le nombre total d’incidents et d’événements survenus par version du package au cours de la période sélectionnée. Par défaut, la version du package ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique.

## <a name="failures"></a>Échecs

Le graphique **Échecs** présente le nombre total d’incidents et d’événements survenus par nom d’échec au cours de la période sélectionnée. Chaque nom d’échec est constitué de quatre parties: une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom de l’image/du pilote où l’échec s’est produit et le nom de la fonction associée. Par défaut, l’échec ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique. Pour chaque échec, le pourcentage du nombre total d’échecs apparaît également.

> [!TIP]
> Dans certains cas, une entrée peut être affichée pour **Inconnu** dans cette section. Cela se produit lorsqu’en dépit de nos efforts nous ne pouvons pas collecter tous les détails pour un ou plusieurs échecs, qui seront tous regroupés sous **Inconnu**. Le plus souvent, cela se produit en raison de contraintes de stockage, mais cela peut également être le résultat de paramètres de confidentialité d’un appareil, de problèmes de connexion réseau, de vidages sur incident partiels/incorrects ou d’autres facteurs.
>
> Si vous voyez **!unknown** dans un nom d’échec, cela signifie que les symboles n’étaient pas présents et que nous n’avons pas pu identifier le nom d’échec. Veillez à inclure les symboles dans votre package pour obtenir une analyse précise des échecs. Voir [Configurer un package d’application](../packaging/packaging-uwp-apps.md#configure-an-app-package). En revanche, si les noms d’échec incluent **!unknown_error_in_** et **!unknown_function**, cela signifie que nous n’avons pas pu collecter des informations complètes pour diverses autres raisons.

Pour afficher le rapport **Détails de l’échec** pour un échec particulier, sélectionnez le nom de l’échec. Si vous avez inclus des fichiers de symboles, le rapport **Détails de l’échec** inclut le nombre d’occurrences de l’échec au cours du dernier mois, ainsi qu’un journal des échecs répertoriant les détails relatifs aux occurrences (date, version du package, type d’appareil, modèle d’appareil, version de système d’exploitation) et un lien vers la trace de la pile et/ou le fichierCAB, s’il est disponible.

> [!TIP]
> Les fichiersCAB sont uniquement disponibles lorsque l’échec s’est produit sur un ordinateur utilisant la build WindowsInsider. Par conséquent, certains échecs n’incluront pas l’option de téléchargement de fichiersCAB. Pour afficher uniquement les échecs qui ont des fichiers CAB, sélectionnez **des défaillances de téléchargements** dans le filtre de section. Vous pouvez également cliquer sur l’en-tête de **liaisons** dans le **journal des échecs** pour trier les résultats afin que les échecs qui incluent des fichiers CAB s’affichent en haut de la liste.

Sur la page de **Détails de l’échec** , vous verrez également le graphique de la **prévalence de pile** , qui indique la partie supérieure empile qui ont contribué à l’échec, triés par pourcentage et le graphique de **configuration de l’appareil (30D)** , qui fournit des détails sur le configuration des appareils qui enregistré l’échec. 


## <a name="crash-free-sessions-and-devices-30d"></a>Sessions sur incident clair et des appareils (30D)

Le graphique de **périphériques et les sessions de libérer d’incident** indique le pourcentage d’appareils ou les sessions utilisateur qui n’a pas rencontré un incident au cours des 30 derniers jours. Ces informations vous permet de comprendre comment à grande échelle votre tombe en panne qui affectent vos utilisateurs. Par exemple, une application peut contenir 10 000 incidents dans un jour. Si 90 % de vos appareils sont affectés, puis vous probablement qui classifier comme critique et agir pour y remédier immédiatement. Toutefois, si représente uniquement les 5 % des appareils à l’aide de votre application, la priorité peut être inférieure.

Ce graphique comporte deux onglets:
- **Appareils clair sur incident**: affiche le pourcentage de périphériques uniques qui n’a pas rencontré un échec sur chaque jour (au cours des 30 derniers jours).
- **Sessions clair sur incident**: affiche le pourcentage de sessions utilisateur uniques qui n’a pas rencontré un échec sur chaque jour (au cours des 30 derniers jours).


 

 
