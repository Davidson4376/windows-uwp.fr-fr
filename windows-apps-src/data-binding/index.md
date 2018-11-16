---
author: mcleblanc
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: Liaison de données
description: La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données et éventuellement rester synchronisée avec ces données.
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 37193d28bbb060bc7e315a15dd83fc0084a6b861
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7119138"
---
# <a name="data-binding"></a>Liaison de données

La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données et éventuellement rester synchronisée avec ces données. La liaison de données vous permet de séparer les problématiques liées aux données de celles liées à l’interface utilisateur, ce qui se traduit par un modèle conceptuel plus simple et l’amélioration de la lisibilité, de la testabilité et de la gestion de la maintenance de votre application. Dans le balisage, vous pouvez choisir d’utiliser l’[extension de balisage {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou l’[extension de balisage {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Vous pouvez même utiliser une combinaison des deux dans la même application, voire pour un même élément d’interface utilisateur. {x: Bind} est une nouveauté de Windows 10 et offre de meilleures performances.

| Rubrique | Description |
|-------|-------------|
| [Vue d’ensemble de la liaison de données](data-binding-quickstart.md) | Cette rubrique vous montre comment lier un contrôle (ou un autre élément d’interface utilisateur) à un élément individuel ou lier un contrôle d’éléments à ou un contrôle de liste à une collection d’éléments dans une application de plateforme Windows universelle (UWP). Elle explique également comment contrôler le rendu des éléments, implémenter un affichage détails en fonction d’une sélection et convertir des données pour l’affichage. Pour obtenir des informations plus détaillées, voir [Présentation détaillée de la liaison de données](data-binding-in-depth.md). | 
| [Présentation détaillée de la liaison de données](data-binding-in-depth.md) | Cette rubrique décrit en détail les fonctionnalités de liaison de données. |
| [Exemples de données sur l’aire de conception et pour la création d’un prototype](displaying-data-in-the-designer.md) | Pour que vos contrôles soient remplis avec les données dans le concepteur Visual Studio (de sorte que vous puissiez travailler sur la disposition de votre application, les modèles et les autres propriétés visuelles), vous pouvez utiliser vos exemples de données au moment de la conception de diverses manières. Les exemples de données peuvent également être vraiment utiles et permettre de gagner du temps si vous créez une application de croquis (ou de prototype). Vous pouvez utiliser des exemples de données dans votre croquis ou prototype au moment de l’exécution pour illustrer vos idées sans avoir à vous connecter aux données actives/réelles. |
| [Lier des données hiérarchiques et créer un affichage maître/détails](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | Vous pouvez effectuer un affichage maître/détails (également appelé affichage liste/détails) de données hiérarchiques sur plusieurs niveaux en liant les contrôles d’éléments aux instances [<strong>CollectionViewSource</strong>](https://msdn.microsoft.com/library/windows/apps/BR209833) qui sont liées dans une chaîne. |
| [Liaison de données et MVVM](data-binding-and-mvvm.md) | Cette rubrique décrit le modèle de conception de l’architecture de l’interface utilisateur Model-View-ViewModel (MVVM). Liaison de données est au cœur du modèle MVVM et permet de couplage faible entre le code de l’interface utilisateur et sans interface utilisateur. |
