---
author: jebishop
ms.assetid: fb8ae71d-5c88-4c85-9257-a9607d5179b1
title: "éclairage"
description: "Les objets Light sont utilisés en association avec SceneLightingEffect pour simuler l’éclairage dynamique et la réflectivité."
ms.author: jimwalk
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 3f922cae8aa0787f8be6496997df1021dda8e142
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2017
---
# <a name="lighting"></a>Éclairage

Les objets [**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) sont utilisés en association avec [**SceneLightingEffect**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) pour simuler l’éclairage dynamique et la réflectivité.

Vous pouvez appliquer des éclairages aux [**éléments visuels**](https://msdn.microsoft.com/library/windows/apps/Dn706858) et aux [**éléments UIElements**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) XAML.

## <a name="applying-lights-to-xaml-uielements"></a>Application d’éclairages aux éléments UIElements XAML

Les objets [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) permettent d’appliquer des classes [**CompositionLight**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionLight) pour éclairer les éléments UIElements XAML de manière dynamique. XamlLight offre des méthodes permettant de cibler des éléments UIElements ou d’autres pinceaux XAML, d’appliquer des éclairages aux arborescences d’éléments UIElements et de faciliter la gestion de la durée de vie des ressources CompositionLight selon qu’elles sont ou non en cours d’utilisation.

* Si vous ciblez un objet **Brush** avec une classe XamlLight, les parties des éléments UIElements qui utilisent cet objet Brush sont éclairées.
* Si vous ciblez un élément **UIElement** avec une classe XamlLight, la totalité de l’élément UIElement et tous ses éléments UIElements enfants sont éclairés.

## <a name="creating-and-using-a-xamllight"></a>Création et utilisation d’une classe XamlLight

[**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) est une classe de base permettant de créer des éclairages personnalisés.

Voici un exemple de classe XamlLight personnalisée qui utilise une propriété jointe pour cibler des éléments spécifiques:

```csharp
public sealed class FancyOrangeSpotLight : XamlLight
{
    // Register an attached proeprty that enables apps to set a UIElement or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(Boolean),
        typeof(FancyOrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );
    public static void SetIsTarget(DependencyObject target, Boolean value)
    {
        target.SetValue(IsTargetProperty, value);
    }
    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (Boolean) target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj,
                                            DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (Boolean)e.NewValue;

        if (isAdding)
        {
            if (obj is UIElement)
            {
                XamlLight.AddTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.AddTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
        else
        {
            if (obj is UIElement)
            {
                XamlLight.RemoveTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.RemoveTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
    }

    protected override void OnConnected(UIElement newElement)
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        CompositionLight = Window.Current.Compositor.CreateSpotLight();
        CompositionLight.InnerConeColor = Colors.Orange;
        CompositionLight.OuterConeColor = Colors.Yellow;
        CompositionLight.InnerConeAngleInDegrees = 30;
        CompositionLight.OuterConeAngleInDegrees = 45;
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen. The CompositionLight should be disposed when no longer required.
        CompositionLight.Dispose();
        CompositionLight = null;
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return typeof(FancyPointerTrackerLight).FullName;
    }
}
```

Et voici un exemple présentant différentes utilisations possibles de l’éclairage personnalisé défini ci-dessus:

```xml
<Page>
    <Page.Lights>
        <local:SimpleOrangeSpotLight />
    </Page.Lights>

    <StackPanel>
        <!-- this border will be lit by a FancyOrangeSpotLight, but not its children -->
        <Border BorderThickness="1">
            <Border.BorderBrush>
                <SolidColorBrush Color="Orange" local:FancyOrangeSpotLight.IsTarget="true" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border and its children will be lit by FancyOrangeSpotLight -->
        <Border BorderThickness="2" local:FancyOrangeSpotLight.IsTarget="true">
            <Border.BorderBrush>
                <SolidColorBrush Color="Purple" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>

        <!-- this border will not be lit -->
        <Border BorderThickness="2">
            <Border.BorderBrush>
                <SolidColorBrush Color="Green" />
            </Border.BorderBrush>
            <TextBlock Text="hello world" />
        </Border>
    </StackPanel>
<Page>
```

> [!Important]
> La définition de UIElement.Lights dans le balisage tel qu’indiqué dans l’exemple ci-dessus est uniquement prise en charge pour les applications ciblant au minimum la version Windows10 Creators Update ou une version ultérieure. Pour les applications ciblant des versions antérieures, les éclairages doivent être créés dans le code-behind.

## <a name="additional-resources"></a>Ressources supplémentaires

* Exemples d’interface utilisateur et de composition avancés dans le [GitHub WindowsUIDevLabs](https://github.com/microsoft/windowsuidevlabs).