---
author: serenaz
Description: Learn how to use page transitions in your UWP apps.
title: Transitions de page dans les applications UWP
template: detail.hbs
ms.author: sezhen
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: dc42199eba00071f5dbabd83a4ae524298a619ee
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818319"
---
# <a name="page-transitions"></a>Transitions de page

Les transitions de page sont des animations qui sont lues lorsque les utilisateurs naviguent entre les pages d’une application, en fournissant des commentaires comme la relation entre les pages. Les transitions de page aident les utilisateurs à comprendre s’ils se trouvent en haut d’une hiérarchie de navigation, se déplacent entre des pages sœur ou naviguent plus profondément dans la hiérarchie de pages.

Deux animations différentes sont fournies pour la navigation entre les pages dans une application: *actualisation de la page* et *exploration*, et sont représentées par des sous-classes de [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="page-refresh"></a>Actualisation de la page

L’actualisation de la page combine une animation qui glisse vers le haut et une animation d’apparition en fondu pour le contenu entrant. Cela doit retranscrire l’impression que l’utilisateur a recommencé.

Utilisez l'actualisation de la page lorsque l’utilisateur est dirigé vers le haut d’une pile de navigation, telle que la navigation entre des éléments [tabs](../controls-and-patterns/tabs-pivot.md) ou [left-nav](../controls-and-patterns/navigationview.md). Par défaut, [frame.Navigate ()](/uwp/api/windows.ui.xaml.controls.frame.navigate) utilise l'actualisation de la page.

![Animation d'actualisation de la page](images/page-refresh.gif)

L’animation d'actualisation de la page est représentée par la classe [EntranceNavigationTransitionInfoClass](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());
```

## <a name="drill"></a>Exploration

Utilisez l’exploration lorsque les utilisateurs naviguent plus profondément dans une application, comme l’affichage d’informations détaillées après la sélection d’un élément.

Cela doit retranscrire l’impression que l’utilisateur est allé encore plus loin dans l’application.

![Animation d’exploration](images/drill.gif)

L’animation d’exploration est représentée par la classe [DrillInNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>Suppression

La suppression de l’animation est utile si vous créez votre propre transition à l’aide des [animations connectées](connected-animation.md) ou des animations d’affichage/masquage implicites.

Pour éviter la lecture d’animations lors de la navigation, utilisez [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) à la place d’autres sous-types [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

## <a name="backwards-navigation"></a>Navigation vers l’arrière

Par défaut, [Frame.GoBack()](/uwp/api/windows.ui.xaml.controls.frame.goback) lit l’animation de navigation vers l’arrière correspondante basée sur l’animation jouée pour naviguer jusqu’à la page. Par exemple, une application qui utilise l’exploration pour naviguer dans une page verra une animation d’éloignement lorsque les utilisateurs navigueront vers l’arrière.

Pour lire une transition spécifique lors de la navigation vers l’arrière, utilisez `Frame.GoBack(NavigationTransitionInfo)`. Cela peut être utile lorsque vous modifiez le comportement de navigation basé dynamiquement sur la taille d’écran, par exemple, dans un scénario Maître/Détail réactif.

## <a name="related-topics"></a>Rubriquesassociées

- [Naviguer entre deux pages](../basics/navigate-between-two-pages.md)
- [Animations dans les applications UWP](index.md)
