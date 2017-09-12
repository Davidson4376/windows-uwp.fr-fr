---
author: jnHs
Description: "Le rapport d’intégrité disponible dans le tableau de bord du centre de développement Windows vous permet d’obtenir des données liées aux performances et à la qualité de votre application, y compris les incidents et blocages."
title: "Rapport d’intégrité"
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
ms.author: wdg-dev-content
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 67f87405f7738dec591c71678ecfa5122e673502
ms.sourcegitcommit: ae93435e1f9c010a054f55ed7d6bd2f268223957
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2017
---
# <a name="health-report"></a>Rapport d’intégrité


Le rapport **Intégrité** disponible dans le tableau de bord du centre de développement Windows vous permet d’obtenir des données liées aux performances et à la qualité de votre application, y compris les incidents et blocages. Vous pouvez visualiser ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Le cas échéant, vous pouvez visualiser les traces de pile et/ou les fichiersCAB pour un débogage supplémentaire.

Vous pouvez également récupérer les données de ce rapport par programme à l’aide de [l’API REST d’analyse du WindowsStore](../monetize/access-analytics-data-using-windows-store-services.md).


## <a name="apply-filters"></a>Appliquer des filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est de **72H** (72heures), mais vous pouvez choisir **30D** à la place si vous souhaitez afficher les données relatives aux 30derniers jours.

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par version de package, par marché et/ou par type d’appareil.

-   **Version du package**: la valeur par défaut de ce filtre est **Toutes**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Marché**: le filtre par défaut est **Tous les marchés**, mais vous pouvez restreindre les données aux acquisitions relatives à un ou plusieurs marchés.
-   **Type d’appareil**: le paramétrage par défaut est **Tous**, mais vous pouvez choisir d’afficher uniquement les données relatives à un type d’appareil spécifique.
-   **Version du système d’exploitation**: la valeur par défaut est **Toutes les versions de système d’exploitation**, mais vous pouvez choisir une version de système d’exploitation spécifique.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


## <a name="failure-hits"></a>Occurrences des échecs

Le graphique **Occurrences des échecs** affiche le nombre d’incidents et d’événements quotidiens recensés par les clients sur votre application au cours de la période sélectionnée. Les différents événements survenus dans votre application font l’objet d’un suivi par type: incidents, absences de réponse, exceptions JavaScript et défaillances mémoire.


## <a name="failure-hits-by-market"></a>Occurrences des échecs par marché

Le graphique **Occurrences des échecs par marché** présente le nombre total d’incidents et d’événements survenus par marché au cours de la période sélectionnée.

Vous pouvez visualiser ces données sous la forme visuelle **Carte** ou **Tableau**. La représentation sous forme de tableau affiche cinq marchés à la fois, soit dans l’ordre alphabétique, soit par nombre maximal ou minimal de sessions utilisateur. Vous pouvez également télécharger ces données afin de visualiser d’un seul coup d’œil les informations relatives à la totalité des marchés.


## <a name="package-version"></a>Version du package

Le graphique **Version du package** présente le nombre total d’incidents et d’événements survenus par version du package au cours de la période sélectionnée. Par défaut, la version du package ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique.

## <a name="failures"></a>Échecs

Le graphique **Échecs** présente le nombre total d’incidents et d’événements survenus par nom d’échec au cours de la période sélectionnée. Par défaut, l’échec ayant donné lieu au plus grand nombre d’occurrences apparaît en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Occurrences** de ce graphique. Pour chaque échec, le pourcentage du nombre total d’échecs apparaît également.

Pour afficher le rapport **Détails de l’échec** d’un échec particulier, sélectionnez le nom de l’échec. Si vous avez inclus des fichiers de symboles PDB, le rapport **Détails de l’échec** inclut le nombre d’occurrences de l’échec au cours du dernier mois, ainsi qu’un journal des échecs répertoriant les détails relatifs aux occurrences (date, version du package, type d’appareil, modèle d’appareil, version de système d’exploitation) et un lien vers la trace de la pile et/ou le fichierCAB, s’il est disponible.

> [!TIP]
> Les fichiersCAB sont uniquement disponibles lorsque l’échec s’est produit sur un ordinateur utilisant la build WindowsInsider. Par conséquent, certains échecs n’incluront pas l’option de téléchargement de fichiersCAB. Vous pouvez cliquer sur l’en-tête **Liens** du **Journal des échecs** pour trier les résultats afin que les échecs qui incluent des fichiersCAB apparaissent au début de la liste.

 

 
