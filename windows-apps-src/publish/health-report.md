---
Description: Le rapport d’intégrité disponible dans le tableau de bord du centre de développement Windows vous permet d’obtenir des données liées aux performances et à la qualité de votre application, y compris les incidents et blocages.
title: Rapport d’intégrité
ms.assetid: 4F671543-1E91-4E59-88A3-638E3E64539A
---

# Rapport d’intégrité


Le rapport **Intégrité** disponible dans le tableau de bord du centre de développement Windows vous permet d’obtenir des données liées aux performances et à la qualité de votre application, y compris les incidents et blocages. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Le cas échéant, vous pouvez afficher les traces de pile pour un débogage supplémentaire. Vous pouvez également récupérer ces données par programme à l’aide de l’[API REST d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

> **Remarque** Si vous avez déjà publié des applications et affiché les données de performances dans vos anciens tableaux de bord, vous remarquerez peut-être ici une augmentation du nombre d’incidents et d’événements signalés. En effet, nous sommes en mesure d’intégrer davantage de données dans ce rapport pour vous offrir une vue plus complète.

## Appliquer des filtres


Dans la zone supérieure de la page, vous pouvez développer l'option **Appliquer des filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par version de package.

-   **Date** : le filtre par défaut est **72 derniers jours**, mais vous pouvez choisir jusqu'à **6 derniers mois**.
-   **Version du package** : la valeur par défaut de ce filtre est **Toutes les versions**. Si votre application comporte plusieurs versions de package, vous pouvez en choisir une.

Les informations figurant dans tous les graphiques répertoriés ci-après refléteront la période sélectionnée dans la section **Appliquer des filtres**. Par défaut, ces données porteront sur toutes les versions de vos packages, sauf si vous avez utilisé l'option **Appliquer des filtres** pour ne visualiser qu’une seule version.

## Nombre total d’incidents et d’événements


Le graphique **Nombre total d'incidents et d'événements** affiche le nombre d'incidents et d'événements quotidiens recensés par les clients sur votre application au cours de la période sélectionnée. Les différents événements survenus dans votre application font l'objet d'un suivi par type : incidents, absences de réponse, exceptions JavaScript ou défaillances mémoire.

Vous pouvez également filtrer les résultats par marché et/ou par version du système d'exploitation.

## Répartition des incidents et des événements


Le graphique **Répartition des incidents et des événements** vous permet de visualiser des graphiques assurant le suivi de détails spécifiques relatifs aux configurations des clients en cas d'incident ou d'absence de réponse. Cliquez sur les titres de section pour obtenir des informations sur les éléments suivants :

-   Version du système d'exploitation
-   Type d'appareil
-   Mémoire (Mo)
-   Stockage de masse (Go)
-   Vitesse du processeur (GHz)

Vous pouvez également filtrer les résultats par **type d’incident** : pannes, absences de réponse, exceptions JavaScript ou défaillances mémoire. (Le paramètre par défaut consiste à afficher tous les types d’incidents.)

## Marché


Le graphique **Marché** présente le nombre total d'incidents et d'événements survenus par marché au cours de la période sélectionnée. Par défaut, les marchés ayant donné lieu au plus grand nombre d’incidents apparaissent en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Incidents** de ce graphique.

## Journal des échecs


Si vous avez inclus des fichiers de symboles PDB, le graphique **Journal des échecs** affiche les détails relatifs aux occurrences de symboles spécifiques, notamment le nombre total d'incidents et le nombre quotidien moyen d'incidents par symbole.

Ces informations sont obtenues sur la base d’un pourcentage du nombre total d'événements. La partie supérieure de ce graphique indique le pourcentage d'événements échantillonnés afin de fournir ces données.

 

 


<!--HONumber=Mar16_HO1-->


