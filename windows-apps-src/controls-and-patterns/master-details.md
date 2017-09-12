---
author: Jwmsft
Description: "Le modèle Maître/Détails affiche une liste principale et les détails de l’élément actuellement sélectionné. Ce modèle est souvent utilisé pour les listes de messages électroniques et de contacts ou les carnets d’adresses."
title: "Maître/détails"
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 49a586aac0c846cdad02f8448532238bd3eb8551
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="masterdetails-pattern"></a>Modèle Maître/Détails

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Le modèle Maître/Détails possède un volet principal (généralement avec un [affichage Liste](lists.md)) et un volet d’informations correspondant au contenu. Lorsqu’un élément de la liste principale est sélectionné, le volet d’informations est mis à jour. Ce modèle est souvent utilisé pour le courrier électronique et les carnets d’adresses.

> **API importantes**: [classe ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView), [classe SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)

![Exemple de modèle Maître/Détails](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>Est-ce le modèle approprié ?

Le modèle Maître/Détails fonctionne bien si vous souhaitez :

-   créer une application de messagerie, un carnet d’adresses ou une application basés sur une disposition liste/détails ;
-   rechercher et hiérarchiser une grande collection de contenu ;
-   permettre d’ajouter et de supprimer rapidement des éléments dans une liste tout en basculant entre les contextes.

## <a name="choose-the-right-style"></a>Choisir le style approprié

Lorsque vous implémentez le modèle Maître/Détails, nous vous recommandons d’utiliser le style empilé ou le style côte à côte, en fonction de l’espace d’écran disponible.

| Largeur de fenêtre disponible | Style recommandé |
|------------------------|-------------------|
| 320 epx-719 epx        | Mode Empilé           |
| 720 epx ou plus large       | Côte à côte      |

 
## <a name="stacked-style"></a>Style empilé

Le style empilé ne permet de visualiser qu’un seul volet à la fois : le volet principal ou le volet d’informations.

![Détail du volet principal en mode Empilé](images/patterns-md-stacked.png)

L’utilisateur commence au niveau du volet principal et descend dans le volet d’informations en sélectionnant un élément dans la liste principale. Pour l’utilisateur, les affichages Maître et Détails apparaissent dans deux pages distinctes.

### <a name="create-a-stacked-masterdetails-pattern"></a>Créer un modèle Maître/Détails empilé

L’une des façons de créer le modèle Maître/Détails empilé consiste à utiliser des pages distinctes pour le volet principal et pour le volet d’informations. Placez l’affichage Liste qui contient la liste principale dans une page, et l’élément de contenu du volet d’informations dans une autre page.

![Parties du modèle Maître/Détails de style empilé](images/patterns-md-stacked-parts.png)

Pour le volet principal, un contrôle d’[affichage Liste](lists.md) fonctionne bien pour présenter des listes pouvant contenir des images et du texte.

Pour le volet d’informations, utilisez l’élément de contenu le plus logique. Si vous disposez d’un grand nombre de champs distincts, pensez à utiliser une disposition en grille pour organiser les éléments dans un formulaire.

## <a name="side-by-side-style"></a>Style côte à côte

Dans le style côte à côte, le volet principal et le volet d’informations sont visibles en même temps.

![Modèle Maître/Détail](images/patterns-masterdetail-400x227.png)

La liste du volet principal possède un objet visuel de sélection pour indiquer l’élément sélectionné. La sélection d’un nouvel élément dans la liste principale entraîne la mise à jour du volet d’informations.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Créer un modèle Maître/Détails côte à côte

Pour le volet principal, un contrôle d’[affichage Liste](lists.md) fonctionne bien pour présenter des listes pouvant contenir des images et du texte.

Pour le volet d’informations, utilisez l’élément de contenu le plus logique. Si vous disposez d’un grand nombre de champs distincts, pensez à utiliser une disposition en grille pour organiser les éléments dans un formulaire.

## <a name="get-the-code-samples"></a>Obtenir les exemples de code

Pour obtenir un exemple de code affichant le modèle Maître/Détails, voir les exemples suivants: 

- [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) 
- [Exemple de ListView et GridView](http://go.microsoft.com/fwlink/p/?LinkId=619900)
- [Exemple de lecteurRSS](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>Articles connexes

- [Listes](lists.md)
- [Recherche](search.md)
- [Barre de l’application et barre de commandes](app-bars.md)
- [Classe ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
