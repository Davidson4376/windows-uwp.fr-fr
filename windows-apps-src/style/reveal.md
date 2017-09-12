---
author: mijacobs
description: "Révéler est un nouveau modèle d’interaction qui permet d’améliorer la mise au point et la convivialité de votre application."
title: "Révéler"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kisai
design-contact: conrwi
dev-contact: jevansa
doc-status: Published
ms.openlocfilehash: d50e3f47faad5fff0ef461a4b5312127a0b9ec9c
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2017
---
# <a name="reveal"></a>Révéler

> [!IMPORTANT]
> Cet article décrit les fonctionnalités qui ne sont pas encore disponibles pour le moment et qui sont susceptibles de faire l'objet de modifications significatives avant d'être diffusées au grand public. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Révéler est un effet visuel qui permet d'ajouter de la profondeur et une meilleure mise au point des éléments interactifs de votre application.

> **API importantes**: [classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush), [classe RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbackgroundbrush), [classe RevealBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealborderbrush), [classe RevealBrushHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrushhelper), [classe VisualState](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.VisualState)

Révéler effectue cette opération en révélant les morceaux d’éléments autour du contenu principal (ou mis au point) lorsque la souris ou le rectangle de focus se trouve sur les zones de votre choix.

![Éléments visuels de Révéler](images/Nav_Reveal_Animation.gif)

L'outil Révéler expose les bordures masquées qui se trouvent autour des objets, pour permettre aux utilisateurs de mieux comprendre l'espace avec lequel ils interagissent et voir les actions à leur disposition. Cela est particulièrement important dans les contrôles de liste et les contrôles avec plaques de fond.

## <a name="reveal-and-the-fluent-design-system"></a>Révéler et Fluent Design System

 Fluent Design System vous aide à créer une interface utilisateur moderne et audacieuse qui incorpore la lumière, la profondeur, le mouvement, les matières et la notion d'échelle. Révéler est un composant de Fluent Design System qui ajoute de la lumière à votre application. 

## <a name="what-is-reveal"></a>En quoi consiste l'outil Révéler?

Révéler se compose de deux composants visuels principaux: le comportement **Révéler l'élément sélectionné** et le comportement **Révéler la bordure**.

![Révéler les couches](images/RevealLayers.png)

La fonction Révéler l'élément sélectionné est directement liée au contenu sélectionné (via le pointeur ou l’entrée de la mise au point) et applique un halo léger autour de l’élément pointé ou en focus, vous indiquant ainsi que vous pouvez interagir avec celui-ci.

La fonction Révéler la bordure est appliquée à l’élément sélectionné et les éléments à proximité. Cela vous indique que vous pouvez réaliser des actions avec les objets à proximité similaires à celles de l'élément sélectionné.

Voici le détail de la recette de la fonction Révéler:

- La fonction Révéler la bordure se trouve par-dessus tout le contenu, mais sur des bords désignés
- Le texte et le contenu s'affichent directement sous Révéler la bordure
- Révéler l'élément sélectionné se trouve sous le contenu et le texte
- La plaque de fond (qui active la fonction Révéler l'élément sélectionné)
- L’arrière-plan (l'arrière-plan de la commande)

<!--
<div class=”microsoft-internal-note”>
To create your own Reveal lighting effect for static comps or prototype purposes, see the full [uni design guidance](http://uni/DesignDepot.FrontEnd/#/ProductNav/3020/1/dv/?t=Resources%7CToolkit%7CReveal&f=Neon) for this effect in illustrator.
</div>
-->

## <a name="how-to-use-it"></a>Mode d’utilisation

Révéler expose la géométrie qui se trouve autour de votre curseur lorsque vous en avez besoin et l'efface à l'aide d'un fondu lorsque vous passez à un autre élément.

Révéler fonctionne de manière optimale sur le contenu principal de l'application, qui est doté de bordures et de plaques de fond implicites. Révéler doit également être utilisé sur les commandes de collecte et de liste.

## <a name="controls-that-automatically-use-reveal"></a>Contrôles utilisant automatiquement Révéler

- [**ListView**](../controls-and-patterns/lists.md)
- [**TreeView**](../controls-and-patterns/tree-view.md)
- [**NavigationView**](../controls-and-patterns/navigationview.md)
- [**AutoSuggestBox**](../controls-and-patterns/auto-suggest-box.md)

## <a name="enabling-reveal-on-other-common-controls"></a>Activer Révéler sur d’autres contrôles courants

Si vous avez besoin d'appliquer Révéler pour votre scénario (ces contrôles se trouvent dans le contenu principal et/ou sont utilisés dans des contrôles de liste ou de collecte), nous mettons à votre disposition des styles de ressource qui vous permettent d'activer Révéler dans ces situations.

Ces contrôles n’utilisent pas Révéler par défaut car il s'agit de contrôles mineurs, en général des contrôles annexes aux éléments essentiels de votre application; toutefois, chaque application est différente et si ces contrôles sont ceux qui sont le plus utilisés dans votre application, nous proposons certains styles qui vous seront utiles pour ce faire:

| Nom du contrôle   | Nom de la ressource |
|----------|:-------------:|
| Bouton |  ButtonRevealStyle |
| ToggleButton | ToggleButtonRevealStyle |
| RepeatButton | RepeatButtonRevealStyle |
| AppBarButton | AppBarButtonRevealStyle |
| SemanticZoom | SemanticZoomRevealStyle |
| ComboBoxItem | ComboxBoxItemRevealStyle |

Pour appliquer ces styles, mettez simplement à jour la propriété Style comme suit:

```XAML
<Button Content="Button Content" Style="{StaticResource ButtonRevealStyle}"/>
```

> [!NOTE]
> La version16190 du SDK ne rend pas ces styles automatiquement disponibles. Pour les obtenir, vous devez les copier manuellement à partir de generic.xaml dans votre application. Generic.xaml est généralement situé dans C:\Program Files (x86) \Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\10.0.16190.0\Generic. Ce problème sera résolu dans une prochaine build. 

## <a name="enabling-reveal-on-custom-controls"></a>Activer Révéler sur les contrôles personnalisés

Pour activer Révéler sur les contrôles personnalisés ou remodélisés, vous pouvez accéder au style de ce contrôle dans les états visuels du modèle de ce contrôle, et définir Révéler sur la grille racine:

```xaml
<VisualState x:Name="PointerOver">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="PointerOver" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPointerOver}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPointerOver}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPointerOver}"/>
  </VisualState.Setters>
</VisualState>
```

Pour obtenir l’effet de l’extraction de Révéler, vous devrez également ajouter le même RevealBrushHelper à PressedState:

```xaml
<VisualState x:Name="Pressed">
  <VisualState.Setters>
    <Setter Target="RootGrid.(RevealBrushHelper.State)" Value="Pressed" />
    <Setter Target="RootGrid.Background" Value="{ThemeResource ButtonRevealBackgroundPressed}"/>
    <Setter Target="ContentPresenter.BorderBrush" Value="{ThemeResource ButtonRevealBorderBrushPressed}"/>
    <Setter Target="ContentPresenter.Foreground" Value="{ThemeResource ButtonForegroundPressed}"/>
  </VisualState.Setters>
</VisualState>
```


Le fait d'utiliser notre ressource système Révéler signifie que nous traiterons toutes les modifications de thème à votre place.

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées
- Activez Révéler sur les éléments avec lesquels l’utilisateur peut effectuer des actions: Révéler ne doit pas être activé sur un contenu statique
- Affichez Révéler dans des listes ou des collectes de données
- Appliquez Révéler au contenu faisant clairement partie des plaques de fond
- N'appliquez pas Révéler sur des arrière-plans, du texte ou des images statiques, avec lesquels l'utilisateur ne peut pas interagir
- N'appliquez pas Révéler sur les contenus flottants indépendants
- N'utilisez pas Révéler dans des situations ponctuelles et isolées, comme pour le contenu des boîtes de dialogue ou des notifications
- N’utilisez pas Révéler pour des décisions de sécurité, car cela pourrait distraire l'utilisateur tandis que vous souhaitez lui communiquer un message important

## <a name="related-articles"></a>Articles connexes

- [Classe RevealBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.revealbrush)
- [**Acrylique**](acrylic.md)
- [**Effets de composition**](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
