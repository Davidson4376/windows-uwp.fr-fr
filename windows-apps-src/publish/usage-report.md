---
author: jnHs
Description: "Le rapport Utilisation disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application."
title: "Rapport sur l’utilisation"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
translationtype: Human Translation
ms.sourcegitcommit: 056642044953bab02f78912c7611ddcf5d6d48e6
ms.openlocfilehash: 476e7ee0c9c7ea7dce7f5e3a0389091ede9132c4

---

# Rapport sur l’utilisation


Le rapport **Utilisation** disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application et d’obtenir des informations sur les événements personnalisés que vous avez définis. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.

> **Remarque** Par le passé, le rapport **Utilisation** ne contenait des données que si vous aviez activé le Kit de développement logiciel (SDK) VisualStudio Application Insights dans votre application. Avec le rapport **Utilisation** mis à jour, cela n’est plus nécessaire.

## Appliquer les filtres


Dans la zone supérieure de la page, vous pouvez développer la section **Appliquer les filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par groupe de produits (versions de système d’exploitation associées).

-   **Date** : la valeur par défaut de ce filtre est **30 derniers jours**, mais vous pouvez étendre cette période aux **12 derniers mois**.
-   **Version du package** : la valeur par défaut de ce filtre est **Toutes**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Type d’appareil** : le paramètre par défaut est **Tous**, mais vous pouvez choisir d’afficher les données pour un seul type d’appareil donné.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la période sélectionnée dans **Appliquer les filtres**. Par défaut, ces données porteront sur toutes les versions de vos packages et sur tous les types d’appareils pris en charge, sauf si vous avez utilisé la section **Appliquer les filtres** pour ne visualiser que les données d’une version ou d’un type spécifique.

## Nombre total de sessions utilisateur

Le graphique **Nombre total de sessions utilisateur** affiche le nombre de sessions utilisateur quotidiennes de votre application au cours de la période sélectionnée.

Chaque session utilisateur représente une période distincte pendant laquelle un client a interagi avec votre application. Chaque session utilisateur est considérée comme prenant fin après une période d’inactivité, de sorte qu’un client unique peut être associé à plusieurs sessions utilisateur dans la même journée. Veuillez noter que ce graphique n’effectue pas le suivi des utilisateurs uniques pour votre application.

## Utilisateurs actifs

Le graphique **Utilisateurs actifs** affiche le nombre de clients qui ont utilisé votre application un jour spécifique de la période sélectionnée.

Chaque utilisateur actif représente un client ayant utilisé votre application le jour en question. Ce graphique n'effectue pas le suivi des sessions utilisateur uniques (autrement dit, les clients qui figurent dans ce graphique peuvent avoir utilisé votre application une ou plusieurs fois ce jour-là).

## Événements personnalisés

Le graphique **Événements personnalisés** affiche le nombre total d'occurrences des événements personnalisés que vous avez définis pour votre application. Ces informations peuvent concerner plusieurs occurrences pour un même client.

Les événements personnalisés sont implémentés à l’aide de [Journal](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) dans le [SDK d’engagement et de monétisation de la Boutique Microsoft](../monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).



 







<!--HONumber=Jun16_HO4-->


