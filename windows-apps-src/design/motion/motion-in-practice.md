---
author: jwmsft
Description: Learn how Fluent motion fundamentals come together in your app.
title: Mouvement en pratique - Animation dans les applications UWP
label: Motion in practice
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 36081c14cfb75a1cedb103ba17eff4a05f5e4e83
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843726"
---
# <a name="bringing-it-together"></a>Synthèse

Le minutage, l'accélération, la direction et la gravité fonctionnent ensemble pour former la base du mouvement Fluent. Chacun doit être pris en compte par rapport aux autres et correctement appliqué dans le contexte de votre application.

Voici 3manières d’appliquer les principes de base du mouvement Fluent dans votre application.

:::row:::
    :::column:::
        **Implicit animation**

        Automatic tween and timing between values in a parameter change to achieve very simple Fluent motion using the standardized values.
    :::column-end:::
    :::column:::
        **Built-in animation**

        System components, such as common controls and shared motion, are "Fluent by default". Fundamentals have been applied in a manner consistent with their implied usage.
    :::column-end:::
    :::column:::
        **Custom animation following guidance recommendations**

        There may be times when the system does not yet provide an exact motion solution for your scenario. In those cases, use the baseline fundamental recommendations as a starting point for your experiences.
    :::column-end:::
:::row-end:::

**Exemple de transition**

![Animation fonctionnelle](images/pageRefresh.gif)

:::row:::
    :::column:::
        <b>Direction Forward Out:</b><br>
        Fade out: 150m; Easing: Default Accelerate

        <b>Direction Forward In:</b><br>
        Slide up 150px: 300ms; Easing: Default Decelerate
    :::column-end:::
    :::column:::
         <b>Direction Backward Out:</b><br>
        Slide down 150px: 150ms; Easing: Default Accelerate

        <b>Direction Backward In:</b><br>
        Fade in: 300ms; Easing: Default Decelerate
    :::column-end:::
:::row-end:::

**Exemple d'objet**

 ![Mouvement de 300ms](images/control.gif)

:::row:::
    :::column:::
        <b>Direction Expand:</b><br>
        Grow: 300ms; Easing: Standard
    :::column-end:::
    :::column:::
        <b>Direction Contract:</b><br>
        Grow: 150ms; Easing: Default Accelerate
    :::column-end:::
:::row-end:::

## <a name="implicit-animations"></a>Animations implicites

> Animations implicites nécessitent Windows 10, version 1809 ([Kit de développement logiciel 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou une version ultérieure.


Animations implicites sont un moyen simple pour atteindre le mouvement Fluent par l’interpolation automatiquement entre les anciennes et nouvelles valeurs lors d’une modification de paramètre.

Vous pouvez animer implicitement les modifications apportées aux propriétés suivantes:

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **Opacity**
  - **Rotation**
  - **Échelle**
  - **Translation**

- [Bordure](/uwp/api/windows.ui.xaml.controls.border), [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)ou [Panneau](/uwp/api/windows.ui.xaml.controls.panel)
  - **Arrière-plan**

Chaque propriété qui peut avoir des modifications implicitement animées possède une propriété de _transition_ correspondante. Pour animer la propriété, vous affectez un type de transition à la propriété correspondante de la _transition_ . Ce tableau indique les propriétés de _transition_ et le type de transition à utiliser pour chacun d’eux.

| Propriété animée | Propriété de transition | Type de transition implicite |
| -- | -- | -- |
| [UIElement.Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.uielement.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.scale) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.uielement.vector3transition) |
| [Border.Background](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter.Background](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel.Background](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

Cet exemple montre comment utiliser la propriété Opacity et transition pour créer un bouton apparition en fondu lorsque le contrôle est activé et la disparition en fondu lorsqu’il est désactivé.

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>Articles associés

- [Présentation du mouvement](index.md)
- [Minutage et accélération](timing-and-easing.md)
- [Direction et gravité](directionality-and-gravity.md)