---
Description: Découvrez comment utiliser les transitions de page dans vos applications UWP.
title: Transitions de page dans les applications UWP
template: detail.hbs
ms.date: 04/08/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9b3244c24ff4fa8e3c85ee9970536b1b35d8efd5
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444194"
---
# <a name="page-transitions"></a>Transitions de page

Les transitions de page dirigent les utilisateurs entre les pages d’une application, en fournissant des commentaires comme la relation entre les pages. Les transitions de page aident les utilisateurs à comprendre s’ils se trouvent en haut d’une hiérarchie de navigation, se déplacent entre des pages sœur ou naviguent plus profondément dans la hiérarchie de pages.

Deux animations différentes sont fournies pour la navigation entre les pages dans une application : *Actualisation de la page* et *Exploration*, et sont représentées par des sous-classes de [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/PageTransition">ouvrez l’application et voir les Transitions de Page en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>Actualisation de la page

L’actualisation de la page combine une animation qui glisse vers le haut et une animation d’apparition en fondu pour le contenu entrant. Utilisez l'actualisation de la page lorsque l’utilisateur est dirigé vers le haut d’une pile de navigation, telle que la navigation entre des éléments tabs ou left-nav.

Cela doit retranscrire l’impression que l’utilisateur a recommencé.

![Animation d'actualisation de la page](images/page-refresh.gif)

L’animation d'actualisation de la page est représentée par la classe [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo).

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Remarque**: Un [ **Frame** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) utilise automatiquement [ **NavigationThemeTransition** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) pour animer la navigation entre les deux pages. Par défaut, l’animation est l'actualisation de la page.

## <a name="drill"></a>Exploration

Utilisez l’exploration lorsque les utilisateurs naviguent plus profondément dans une application, comme l’affichage d’informations détaillées après la sélection d’un élément.

Cela doit retranscrire l’impression que l’utilisateur est allé encore plus loin dans l’application.

![Animation d’exploration](images/drill.gif)

L’animation d’exploration est représentée par la classe [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo).

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>Diapositive horizontal

Utilisez diapositive horizontal pour montrer que les pages sœurs s’affichent en regard de chacun des autres. Le [NavigationView](../controls-and-patterns/navigationview.md) contrôle utilise automatiquement cette animation de navigation supérieure, mais si vous générez votre propre expérience de navigation horizontale, vous pouvez implémenter une diapositive horizontale avec SlideNavigationTransitionInfo.

Le sentiment souhaité est que l’utilisateur navigue entre les pages qui se trouvent en regard de chacun des autres. 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
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

Cela peut être utile lorsque vous modifiez le comportement de navigation basé dynamiquement sur la taille d’écran ; par exemple, dans un scénario Maître/Détail réactif.

## <a name="related-topics"></a>Rubriques connexes

- [Naviguer entre les deux pages](../basics/navigate-between-two-pages.md)
- [Mouvement dans les applications UWP](index.md)
