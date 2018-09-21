---
author: QuinnRadich
Description: The master/detail pattern displays a master list and the details for the currently selected item. This pattern is frequently used for email and contact lists/address books.
title: Maître/Détails
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 28a6160019fd3bf64dd1f75bc5c6df29cd3165ad
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4111901"
---
# <a name="masterdetails-pattern"></a>Modèle Maître/Détails

 

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
| 320epx-640epx        | Mode Empilé           |
| 641epx ou plus large       | Côte à côte      |

 
## <a name="stacked-style"></a>Style empilé

Le style empilé ne permet de visualiser qu’un seul volet à la fois : le volet principal ou le volet d’informations.

![Détail du volet principal en mode Empilé](images/patterns-md-stacked.png)

L’utilisateur commence au niveau du volet principal et descend dans le volet d’informations en sélectionnant un élément dans la liste principale. Pour l’utilisateur, les affichages Maître et Détails apparaissent dans deux pages distinctes.

### <a name="create-a-stacked-masterdetails-pattern"></a>Créer un modèle Maître/Détails empilé

L’une des façons de créer le modèle Maître/Détails empilé consiste à utiliser des pages distinctes pour le volet Maître et pour le volet Détails. Placez l'affichage Maître sur une page et le volet Détails dans une autre page.

![Parties du modèle Maître/Détails de style empilé](images/patterns-md-stacked-parts.png)

Pour la page d'affichage Maître, un contrôle [affichage Liste](lists.md) fonctionne bien pour présenter des listes pouvant contenir des images et du texte. 

Pour la page d’affichage Détails, utilisez l’[élément de contenu](../layout/layout-panels.md) le plus logique. Si vous disposez d’un grand nombre de champs distincts, pensez à utiliser une disposition **Grille** pour organiser les éléments dans un formulaire.

Pour la navigation entre les pages, voir [historique de navigation et navigation vers l’arrière pour les applications UWP](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Style côte à côte

Dans le style côte à côte, le volet principal et le volet d’informations sont visibles en même temps.

![Modèle Maître/Détail](images/patterns-masterdetail-400x227.png)

La liste du volet principal possède un objet visuel de sélection pour indiquer l’élément sélectionné. La sélection d’un nouvel élément dans la liste principale entraîne la mise à jour du volet d’informations.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Créer un modèle Maître/Détails côte à côte

L’une des façons de créer un modèle Maître/Détails côte à côte consiste à utiliser le contrôle [mode Fractionné](split-view.md). Placez l'affichage Maître dans le volet du mode Fractionné et l’affichage Détails dans le contenu du mode Fractionné.

![Parties du mode Fractionné Maître/Détail](images/patterns_md_splitview_parts.png)

Pour le volet Maître, un contrôle d’[affichage Liste](lists.md) fonctionne bien pour présenter des listes pouvant contenir des images et du texte.

Pour le contenu des détails, utilisez l’[élément de contenu](../layout/layout-panels.md) le plus logique. Si vous disposez d’un grand nombre de champs distincts, pensez à utiliser une disposition **Grille** pour organiser les éléments dans un formulaire.

## <a name="adaptive-layout"></a>Disposition adaptative

Pour implémenter un modèle Maître/Détails pour n’importe quelle taille d’écran, créez une interface utilisateur réactive avec une [disposition adaptative](../layout/layouts-with-xaml.md).

![Disposition adaptative Maître/Détail](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>Créer un modèle Maître/Détails adaptatif
Pour créer une disposition adaptative, définissez différents [**VisualStates**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.visualstate) pour votre interface utilisateur et déclarez des points d’arrêt pour les différents états avec des [**AdaptiveTriggers**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

Les exemples suivants implémentent le modèle Maître/Détails avec des dispositions adaptatives et illustrent la liaison de données avec des ressources statiques, de base de données et en ligne: 
- [Exemple de Maître/Détails](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [Exemples de Maître/Détails et de Sélection](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Exemple de Maître/Détails Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Exemple de base de données de commandes de clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [Exemple de lecteurRSS](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>Articles connexes

- [Listes](lists.md)
- [Recherche](search.md)
- [Barre de l’application et barre de commandes](app-bars.md)
- [Classe ListView](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [Classe SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)
