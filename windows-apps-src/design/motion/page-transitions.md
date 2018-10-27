---
author: Jwmsft
Description: Learn how to use page transitions in your UWP apps.
title: Transitions de page dans les applications UWP
template: detail.hbs
ms.author: jimwalk
ms.date: 04/08/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: 62e39e8e2cf1caa5673a925481848147cf445188
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703316"
---
# <a name="page-transitions"></a>Transitions de page

Les transitions de page dirigent les utilisateurs entre les pages d’une application, en fournissant des commentaires comme la relation entre les pages. Les transitions de page aident les utilisateurs à comprendre s’ils se trouvent en haut d’une hiérarchie de navigation, se déplacent entre des pages sœur ou naviguent plus profondément dans la hiérarchie de pages.

Deux animations différentes sont fournies pour la navigation entre les pages dans une application: *Actualisation de la page* et *Exploration*, et sont représentées par des sous-classes de [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="page-refresh"></a>Actualisation de la page

L’actualisation de la page combine une animation qui glisse vers le haut et une animation d’apparition en fondu pour le contenu entrant. Utilisez l'actualisation de la page lorsque l’utilisateur est dirigé vers le haut d’une pile de navigation, telle que la navigation entre des éléments tabs ou left-nav.

Cela doit retranscrire l’impression que l’utilisateur a recommencé.

![Animation d'actualisation de la page](images/page-refresh.gif)

L’animation d'actualisation de la page est représentée par la classe [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Remarque**: un [**Cadre**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) utilise automatiquement [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) pour animer la navigation entre deux pages. Par défaut, l’animation est l'actualisation de la page.

## <a name="drill"></a>Exploration

Utilisez l’exploration lorsque les utilisateurs naviguent plus profondément dans une application, comme l’affichage d’informations détaillées après la sélection d’un élément.

Cela doit retranscrire l’impression que l’utilisateur est allé encore plus loin dans l’application.

![Animation d’exploration](images/drill.gif)

L’animation d’exploration est représentée par la classe [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Glissement horizontal

Utilisez diapositive horizontal pour montrer que les pages sœur s’affichent en regard des autres. Le contrôle [NavigationView](../controls-and-patterns/navigationview.md) utilise automatiquement cette animation de navigation supérieure, mais si vous créez votre propre expérience de navigation horizontale, vous pouvez implémenter diapositive horizontal avec SlideNavigationTransitionInfo.

Retranscrire est que l’utilisateur navigue entre les pages qui se trouvent en regard des autres. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>Suppression

Pour éviter la lecture d’animations lors de la navigation, utilisez [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) à la place d’autres sous-types **NavigationTransitionInfo**.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

La suppression de l’animation est utile si vous créez votre propre transition à l’aide des [animations connectées](connected-animation.md) ou des animations d’affichage/masquage implicites.

## <a name="backwards-navigation"></a>Navigation vers l’arrière

Vous pouvez utiliser `Frame.GoBack(NavigationTransitionInfo)` pour lire une transition spécifique lors de la navigation vers l’arrière.

Cela peut être utile lorsque vous modifiez le comportement de navigation basé dynamiquement sur la taille d’écran; par exemple, dans un scénario Maître/Détail réactif.

## <a name="related-topics"></a>Rubriquesconnexes

- [Naviguer entre deux pages](../basics/navigate-between-two-pages.md)
- [Animations dans les applications UWP](index.md)
