---
author: shawjohn
Description: "Le rapport sur les installations du tableau de bord du Centre de développement Windows vous indique le nombre d’installations de votre application sur des appareils Windows 10."
title: Rapport sur les installations
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, application, installations, installation, rapport, analyse
ms.assetid: 46c08fd2-00bd-4be5-b29f-01a3b5fea4c2
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 9e4dec61e275443db42b50ef9a231b5b3c46ffe4
ms.lasthandoff: 02/08/2017

---

# <a name="installs-report"></a>Rapport sur les installations

Le rapport sur les **installations** du tableau de bord du Centre de développement Windows vous indique le nombre d’installations, par des clients, de votre application sur des appareils Windows 10. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion.


## <a name="apply-filters"></a>Appliquer des filtres


Dans la zone supérieure de la page, vous pouvez développer l’option **Appliquer les filtres** pour filtrer toutes les données de cette page par date, type d’appareil et/ou version de package.

-   **Date** : le filtre par défaut est **30 derniers jours**, mais vous pouvez étendre la définition sur **	12 derniers mois**.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**, mais vous pouvez sélectionner un type spécifique d’appareil (**PC**, **Téléphone**, **Tablette**, **Machine virtuelle**, **IoT**, **Holographique**, **Console**, **Autre** ou **Inconnu**).
-   **Version du package** : le filtre par défaut est **Toutes les versions**, mais vous pouvez sélectionner une version spécifique de package.


## <a name="installs-daily"></a>Installations quotidiennes


Le tableau **Installations quotidiennes** indique le nombre total d’installations quotidiennes de votre application sur la période sélectionnée.

Cette valeur comprend les éléments suivants :
-   **Installations sur plusieurs appareils Windows 10.** Par exemple, si un client installe votre application sur 2 PC Windows 10 et un téléphone Windows 10, 3 installations sont comptabilisées.
-   **Réinstallations.** Par exemple, si un client installe votre application aujourd’hui, la désinstalle demain puis la réinstalle le mois prochain, 2 installations sont consignées.

Le total ne comprend pas les éléments suivants :
-   **Installations sur des appareils non Windows 10.** Par exemple, si un client installe votre application sur un appareil qui n’exécute pas Windows 10, aucune installation n’est comptabilisée.
-   **Désinstallations.** Par exemple, si un client désinstalle votre application, nous ne le retirons pas du nombre total d’installations.
-   **Mises à jour.** Par exemple, si un client installe votre application aujourd’hui et installe une mise à jour une semaine plus tard, une seule (et non deux) installation est comptabilisée.
-   **Préinstallations.** Par exemple, si un client achète un appareil sur lequel votre application est préinstallée, nous ne comptabilisons pas d’installation.
-   **Installations initiées par le système.** Par exemple, si Windows installe votre application automatiquement, pour quelque raison que ce soit, nous ne comptabilisons pas d’installation.

> **Remarque**  Actuellement, il nous est impossible de récupérer automatiquement les données d’**Installations quotidiennes** via une API.

## <a name="markets"></a>Marchés


Le graphique **Marchés** indique le nombre total d’installations survenues par marché au cours de la période sélectionnée. Par défaut, nous affichons les données pour l’ensemble des marchés. Toutefois, vous pouvez les filtrer par marché.


## <a name="package-version"></a>Version du package


Le graphique **Version du package** présente le nombre total d’installations et d’événements survenus par version du package au cours de la période sélectionnée.



 

 

