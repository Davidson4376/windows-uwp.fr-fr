---
author: jnHs
Description: "Le rapport d’intégrité disponible dans le tableau de bord du centre de développement Windows vous permet d’obtenir des données liées aux performances et à la qualité de votre application, y compris les incidents et blocages."
title: "Rapport d’intégrité"
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: dd1d0f01968a71dc2120316eb2c33baf0c9d0841
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="health-report"></a>Rapport d’intégrité


Le rapport **Intégrité** disponible dans le tableau de bord du centre de développement Windows vous permet d’obtenir des données liées aux performances et à la qualité de votre application, y compris les incidents et blocages. Vous pouvez afficher ces données dans votre tableau de bord (**Analyse** > **Intégrité**) ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Le cas échéant, vous pouvez afficher les traces de pile pour un débogage supplémentaire. Vous pouvez également récupérer ces données par programme à l’aide de [l’API REST d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).


> **Remarque**  Si vous avez déjà publié des applications et affiché les données de performances, vous remarquerez peut-être une augmentation du nombre d’incidents et d’événements signalés ici. En effet, nous sommes en mesure d’intégrer davantage de données dans ce rapport pour vous offrir une vue plus complète.

## <a name="apply-filters"></a>Appliquer les filtres


Dans la zone supérieure de la page, vous pouvez développer l’option **Appliquer des filtres** pour filtrer toutes les données de cette page par plage de dates, version du package, type d’appareil et/ou version du système d’exploitation.

-   **Date**: le filtre par défaut est **72derniers jours**, mais vous pouvez choisir jusqu'à **30derniers jours**.
-   **Version du package**: la valeur par défaut de ce filtre est **Toutes les versions**. Si votre application comporte plusieurs versions de package, vous pouvez en choisir une.
-   **Type d’appareil**: la valeur par défaut est **Tous les appareils**, mais vous pouvez choisir un appareil spécifique (**PC**, **Téléphone**, **Console**, **IoT**, **Holographique** ou **Inconnu**).
-   **Version du système d’exploitation**: la valeur par défaut est **Toutes les versions de système d’exploitation**, mais vous pouvez choisir une version de système d’exploitation spécifique.

Les informations figurant dans tous les graphiques répertoriés ci-après reflètent la période sélectionnée dans la section **Appliquer des filtres**. Par défaut, ces données porteront sur toutes les versions de vos packages, sauf si vous avez utilisé l'option **Appliquer des filtres** pour ne visualiser qu’une seule version.

## <a name="total-crashes-and-events"></a>Nombre total d’incidents et d’événements


Le graphique **Occurrences des échecs** (anciennement appelé **Nombre total d’incidents et d’événements**) affiche le nombre d’incidents et d’événements quotidiens recensés par les clients sur votre application au cours de la période sélectionnée. Les différents événements survenus dans votre application font l’objet d’un suivi par type: incidents, absences de réponse, exceptions JavaScript ou défaillances mémoire.

Vous pouvez également filtrer les résultats par marché et/ou par version du système d’exploitation.

## <a name="markets"></a>Marchés


Le graphique **Marchés** présente le nombre total d’incidents et d’événements survenus par marché au cours de la période sélectionnée. Par défaut, les marchés ayant donné lieu au plus grand nombre d’occurrences apparaissent en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique.

## <a name="package-version"></a>Version du package


Le graphique **Version du package** présente le nombre total d’incidents et d’événements survenus par version du package au cours de la période sélectionnée. Par défaut, la version du package ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique.

## <a name="failures"></a>Échecs


Le graphique **Échecs** présente le nombre total d’incidents et d’événements survenus par nom d’échec au cours de la période sélectionnée. Par défaut, l’échec ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique. Pour chaque échec, le pourcentage du nombre total d’échecs apparaît également.

Pour afficher le rapport **Détails de l’échec** d’un échec particulier, sélectionnez le nom de l’échec. Si vous avez inclus des fichiers de symboles PDB, le rapport **Détails de l’échec** inclut le nombre d’occurrences de l’échec au cours du dernier mois, ainsi que le journal de l’échec répertoriant les détails relatifs aux occurrences (date, version du package, type d’appareil, modèle d’appareil, version de système d’exploitation) et un lien vers la trace de la pile.

 

 
