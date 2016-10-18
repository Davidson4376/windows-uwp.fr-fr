---
author: jnHs
Description: "Le rapport Évaluations disponible dans le tableau de bord du Centre de développement Windows vous permet de visualiser la répartition des évaluations de votre application par les clients dans le Windows Store."
title: "Rapport Évaluations"
ms.assetid: CAFEC20B-04FB-48C8-B663-1238C0B85ECD
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 1613c8a5e5a28ba431fcfb186a0fcd5fe9bd7582

---

# Rapport Évaluations


Le rapport **Évaluations** disponible dans le tableau de bord du Centre de développement Windows vous permet de visualiser la répartition des évaluations de votre application par les clients dans le Windows Store. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de l’[API REST d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une évaluation correspond au nombre d’étoiles (compris entre 1 et 5) qu’un client a attribué à votre application dans le WindowsStore. Le rapport **Évaluations** ne fournit aucune information sur les commentaires formulés sous forme d’avis ; ces derniers sont consultables dans le [rapport sur les avis](reviews-report.md).

## Appliquer les filtres


Dans la zone supérieure de la page, vous pouvez développer l'option **Appliquer les filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par marché.

-   **Date** : la valeur par défaut de ce filtre est **30 derniers jours**, mais vous pouvez étendre cette période aux **12 derniers mois**.
-   **Marché** : le filtre par défaut est **Tous les marchés**. Vous pouvez choisir un marché spécifique si vous souhaitez ne visualiser que les évaluations des clients sur ce marché.
-   **Type d’appareil** : le filtre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement l’évaluation laissée par les clients utilisant celui-ci.

Les informations figurant dans tous les graphiques répertoriés ci-après refléteront la période sélectionnée dans la section **Appliquer des filtres**, ainsi que d’autres filtres que vous avez choisi ici.

## Évaluation moyenne


Le graphique **Évaluation moyenne** affiche l'évaluation moyenne de votre application sur la période sélectionnée.

## Nombre d'évaluations


Le graphique **Nombre d'évaluations** affiche le nombre total d'évaluations de votre application au cours de la période sélectionnée.

## Évaluations nouvelles et révisées


Le graphique **Évaluations nouvelles et révisées** présente le nombre d'évaluations de chaque type (nouvelles ou révisées) effectuées au cours de la période sélectionnée.

-   La section **Nouvelles évaluations** spécifie les évaluations qui ont été soumises par les clients et qui n'ont pas été modifiées.
-   La section **Évaluations révisées** indique les évaluations qui ont été modifiées par le client.

>**Remarque** Une évaluation est signalée comme révisée même dans les cas où le client s’est contenté de changer ou d’ajouter un texte ou un titre dans son avis sans modifier son évaluation proprement dite.

## Évaluation moyenne dans le temps


Le graphique **Évaluation moyenne dans le temps** illustre l'évolution de l'évaluation moyenne de l'application au cours de la période sélectionnée.

Au lieu de calculer la moyenne de toutes les évaluations laissées au cours de la période sélectionnée (comme dans le graphique **Évaluation moyenne** graphique), le graphique **Évaluation moyenne dans le temps** indique l’évaluation laissée par les clients un jour ou une semaine donné(e) au cours de la période. Ce graphique vous permet d’identifier les tendances ou de déterminer dans quelle mesure les mises à jour ou autres modifications ont affecté ou non les évaluations.

Si vous avez filtré les informations sur les **30 derniers jours** ou les **3 derniers mois**, le graphique affiche votre évaluation moyenne par jour. Si vous les avez filtrées sur les **6 derniers mois** ou les **12 derniers mois**, le graphique affiche votre évaluation moyenne par semaine (le lundi marquant le début de chaque nouvelle semaine ; l'évaluation moyenne indiquée concerne la semaine précédente).

## Marchés


Le graphique **Marchés** présente l’évaluation moyenne et le nombre d’évaluations par marché au cours de la période sélectionnée.

> **Remarque** Si vous avez utilisé l’option **Filtres de page** pour spécifier un marché donné, ce graphique n’apparaîtra pas dans le rapport **Évaluations**. Pour visualiser ce graphique, modifiez la valeur de l’option **Filtres de page** de façon à afficher tous les marchés.

Par défaut, les marchés ayant fait l’objet du plus grand nombre d’évaluations apparaissent en premier dans le graphique, mais vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Nombre d’évaluations** de ce graphique. Vous pouvez également trier les données par **Évaluation moyenne** ou par **Marché** en cliquant sur ces colonnes.

> **Remarque** Vous constaterez probablement une différence dans le nombre d’évaluations si vous comparez le rapport **Évaluations** dans le Centre de développement Windows avec le rapport Avis dans l’application mobile de l’ancien Centre de développement. Cela s’explique par le fait que l’application affiche uniquement les avis laissés par les clients utilisant Windows Phone8.1 et antérieur. Ce peut être également la conséquence du travail effectué par Microsoft pour supprimer les avis du Windows Store, qui ont été identifiés comme indésirables, inappropriés, offensants ou comme violant la politique. Nous pensons que cette action va améliorer l’expérience utilisateur.

 

 



<!--HONumber=Aug16_HO3-->


