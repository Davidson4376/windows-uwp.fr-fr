---
author: jnHs
Description: "Le rapport Utilisation disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application."
title: "Rapport sur l’utilisation"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: f15b72a814c713a389a1f6126e2261959a16ee6c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="usage-report"></a>Rapport sur l’utilisation


Le rapport **Utilisation** disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients sur Windows10 utilisent votre application et d’obtenir des informations sur les événements personnalisés que vous avez définis. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.

> **Remarque**  Par le passé, le rapport **Utilisation** ne contenait des données que si vous aviez activé le Kit de développement logiciel (SDK) VisualStudio Application Insights dans votre application. Avec le rapport **Utilisation** mis à jour, cela n’est plus nécessaire.

## <a name="apply-filters"></a>Appliquer les filtres


Dans la zone supérieure de la page, vous pouvez développer la section **Appliquer les filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par groupe de produits (versions de système d’exploitation associées).

-   **Date**: la valeur par défaut de ce filtre est **30derniers jours**, mais vous pouvez étendre cette période aux **3derniers mois**.
-   **Version du package**: la valeur par défaut de ce filtre est **Toutes**. Si votre application comporte plusieurs packages, vous pouvez en choisir un ici.
-   **Type d’appareil** : le paramètre par défaut est **Tous**, mais vous pouvez choisir d’afficher les données pour un seul type d’appareil donné.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la période sélectionnée dans **Appliquer les filtres**. Par défaut, ces données porteront sur toutes les versions de vos packages et sur tous les types d’appareils pris en charge, sauf si vous avez utilisé la section **Appliquer les filtres** pour ne visualiser que les données d’une version ou d’un type spécifique.

> **Remarque** Seules les données d’utilisation des clients sur Windows 10 sont incluses dans ce rapport.

## <a name="total-user-sessions"></a>Nombre total de sessions utilisateur

Le graphique **Nombre total de sessions utilisateur** affiche le nombre de sessions utilisateur quotidiennes de votre application au cours de la période sélectionnée.

Chaque session utilisateur représente une période distincte pendant laquelle un client a interagi avec votre application. Chaque session utilisateur est considérée comme prenant fin après une période d’inactivité, de sorte qu’un client unique peut être associé à plusieurs sessions utilisateur dans la même journée. Veuillez noter que ce graphique n’effectue pas le suivi des utilisateurs uniques pour votre application.

## <a name="active-users"></a>Utilisateurs actifs

Le graphique **Utilisateurs actifs** affiche le nombre de clients qui ont utilisé votre application un jour spécifique de la période sélectionnée.

Chaque utilisateur actif représente un client ayant utilisé votre application le jour en question. Ce graphique n'effectue pas le suivi des sessions utilisateur uniques (autrement dit, les clients qui figurent dans ce graphique peuvent avoir utilisé votre application une ou plusieurs fois ce jour-là).

## <a name="custom-events"></a>Événements personnalisés

Le graphique **Événements personnalisés** affiche le nombre total d'occurrences des événements personnalisés que vous avez définis pour votre application. Ces informations peuvent concerner plusieurs occurrences pour un même client.

Les événements personnalisés sont implémentés à l’aide de la méthode [StoreServicesCustomEventLogger.Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) de [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).



 
