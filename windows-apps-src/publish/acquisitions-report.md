---
author: jnHs
Description: "Le rapport Acquisitions disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application, et vous fournit des informations sur la plateforme ainsi que des données démographiques."
title: Rapport sur les acquisitions
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
translationtype: Human Translation
ms.sourcegitcommit: 7b73682ea36574f8b675193a174d6e4b4ef85841
ms.openlocfilehash: afac76ac5b1e1ef2a0ffef3dddf10e8e0023a275

---

# Rapport sur les acquisitions


Le rapport **Acquisitions** disponible dans le tableau de bord du Centre de développement Windows vous permet de déterminer la façon dont les clients utilisent votre application, et vous fournit des informations sur la plateforme ainsi que des données démographiques. Vous pouvez afficher ces données dans votre tableau de bord ou [télécharger le rapport](download-analytic-reports.md) et le consulter hors connexion. Vous pouvez également récupérer ces données par programme à l’aide de la méthode [obtenir des acquisitions d’applications](../monetize/get-app-acquisitions.md) dans [l’API REST d’analyse du Windows Store](../monetize/access-analytics-data-using-windows-store-services.md).

Dans ce rapport, une acquisition signifie qu’un nouveau client a obtenu une licence pour votre application (qu’elle soit payante ou gratuite).

> **Important** Le rapport **Acquisitions** n’inclut aucune donnée sur les remboursements, reprises, rétrofacturations, etc. Pour évaluer les revenus générés par l’application, visitez [Synthèse des paiements](payout-summary.md). Dans la section **Réservé**, cliquez sur le lien **Télécharger les transactions réservées**.



## Appliquer des filtres


Dans la zone supérieure de la page, vous pouvez développer l'option **Appliquer les filtres** pour filtrer toutes les données de cette page par plage de dates et/ou par type d'appareil.

-   **Date** : la valeur par défaut de ce filtre est **30 derniers jours**, mais vous pouvez étendre cette période aux **12 derniers mois**.
-   **Type d'appareil** : le paramètre par défaut est **Tous les appareils**. Si vous souhaitez afficher les données d'acquisitions relatives à un certain type d'appareil uniquement, vous pouvez en choisir un ici.

Les informations figurant dans les graphiques répertoriés ci-après correspondent à la période sélectionnée dans la section **Appliquer les filtres**.

Les informations figurant dans tous les graphiques répertoriés ci-après refléteront la période sélectionnée dans la section **Appliquer des filtres**. Par défaut, ces données porteront sur tous les types d'appareil, sauf si vous avez utilisé l'option **Appliquer les filtres** pour ne visualiser qu'un seul type.

## Acquisitions


Le graphique **Acquisitions** affiche le nombre total d’acquisitions quotidiennes ou hebdomadaires de votre application au cours de la période sélectionnée. (Lorsque vous utilisez l'option **Appliquer des filtres** pour filtrer les données sur une plus longue période, les données sont rassemblées par semaine.)

Vous pouvez également afficher le nombre d’acquisitions total sur toute la durée de vie de votre application. Vous avez alors accès au total cumulé de l'ensemble des acquisitions effectuées depuis la première publication de votre application.

Vous pouvez également filtrer les résultats par marché et/ou par version du système d'exploitation.

## Données démographiques sur les clients


Le graphique **Données démographiques sur les clients** présente des informations démographiques sur les personnes ayant acheté votre application. Vous pouvez voir le nombre d’acquisitions (au cours de la période sélectionnée) effectuées par des personnes appartenant à un certain groupe d’âge et par sexe.

> **Remarque** Certains clients ont choisi de ne pas partager ces informations. Si nous n’avons pas pu déterminer le groupe d’âge ni le sexe, l’acquisition entre dans la catégorie **Inconnu**.

 

## Marchés


Le graphique **Marchés** présente le nombre total d'acquisitions survenues par marché au cours de la période sélectionnée. Par défaut, les marchés ayant donné lieu au plus grand nombre d’acquisitions apparaissent en premier dans le graphique. Vous pouvez inverser cet ordre en cliquant sur la flèche dans la colonne **Acquisitions** de ce graphique.

## Version du système d’exploitation


Le graphique **Version du système d'exploitation** présente le nombre total d'acquisitions en fonction du système d'exploitation du client (ou via une [acquisition en volume par des organisations](organizational-licensing.md)). Il arrive que nous ne soyons pas en mesure de fournir ces informations. Auquel cas, la version du système d'exploitation entre dans la catégorie **Inconnu**.



 

 



<!--HONumber=Nov16_HO1-->


