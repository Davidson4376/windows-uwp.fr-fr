---
author: daneuber
title: Éclairage XAML
description: Les objets Light sont utilisés en association avec SceneLightingEffect pour simuler l’éclairage dynamique et la réflectivité.
ms.author: jimwalk
ms.date: 06/28/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppcx
- cppwinrt
ms.openlocfilehash: fae2a222ae20c64e8e8adb2abfa973d62216239e
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5700668"
---
# <a name="xaml-lighting"></a>Éclairage XAML

Les objets [**CompositionLight**](/uwp/api/Windows.UI.Composition.CompositionLight) sont utilisés en association avec [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) pour simuler l’éclairage dynamique et la réflectivité.

Vous pouvez appliquer des éclairages aux [**éléments visuels**](https://msdn.microsoft.com/library/windows/apps/Dn706858) et aux [**éléments UIElements**](/uwp/api/Windows.UI.Xaml.UIElement) XAML.

## <a name="applying-lights-to-xaml-uielements"></a>Application d’éclairages aux éléments UIElements XAML

Les objets [**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight) permettent d’appliquer des classes [**CompositionLight**](/uwp/api/Windows.UI.Composition.CompositionLight) pour éclairer les éléments UIElements XAML de manière dynamique. XamlLight offre des méthodes permettant de cibler des éléments UIElements ou pinceaux XAML, appliquer des éclairages aux arborescences d’éléments d’interface utilisateur, et utilisent de ressources selon qu’il s’agisse actuellement en faciliter la gestion de la durée de vie de CompositionLight.

- Si vous ciblez un objet **Brush** avec une classe XamlLight, les parties des éléments UIElements qui utilisent cet objet Brush sont éclairées.
- Si vous ciblez un élément **UIElement** avec une classe XamlLight, la totalité de l’élément UIElement et tous ses éléments UIElements enfants sont éclairés.

## <a name="creating-and-using-a-xamllight"></a>Création et utilisation d’une classe XamlLight

[**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight) est une classe de base permettant de créer des éclairages personnalisés.

Cet exemple montre la définition de classe XamlLight personnalisée qui applique un projecteur multicolore à ciblées UIElements et pinceaux.

```csharp
public sealed class OrangeSpotLight : XamlLight
{
    // Register an attached property that lets you set a UIElement
    // or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(bool),
        typeof(OrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );

    public static void SetIsTarget(DependencyObject target, bool value)
    {
        target.SetValue(IsTargetProperty, value);
    }

    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (bool)target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj, DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (bool)e.NewValue;

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
        if (CompositionLight == null)
        {
            // OnConnected is called when the first target UIElement is shown on the screen.
            // This lets you delay creation of the composition object until it's actually needed.
            var spotLight = Window.Current.Compositor.CreateSpotLight();
            spotLight.InnerConeColor = Colors.Orange;
            spotLight.OuterConeColor = Colors.Yellow;
            spotLight.InnerConeAngleInDegrees = 30;
            spotLight.OuterConeAngleInDegrees = 45;
            CompositionLight = spotLight;
        }
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen.
        // The CompositionLight should be disposed when no longer required.
        // For SDK 15063, see Remarks in the XamlLight class documentation.
        if (CompositionLight != null)
        {
            CompositionLight.Dispose();
            CompositionLight = null;
        }
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light.
        // In most cases you should use the type's FullName.
        return typeof(OrangeSpotLight).FullName;
    }
}
```

```vb
Public NotInheritable Class OrangeSpotLight
    Inherits XamlLight

    ' Register an attached property that lets you set a UIElement
    ' or Brush as a target for this light type in markup.
    Public Shared ReadOnly IsTargetProperty As DependencyProperty = DependencyProperty.RegisterAttached(
            "IsTarget",
            GetType(Boolean),
            GetType(OrangeSpotLight),
            New PropertyMetadata(Nothing, New PropertyChangedCallback(AddressOf OnIsTargetChanged)
            )
        )

    Public Shared Sub SetIsTarget(target As DependencyObject, value As Boolean)
        target.SetValue(IsTargetProperty, value)
    End Sub

    Public Shared Function GetIsTarget(target As DependencyObject) As Boolean
        Return DirectCast(target.GetValue(IsTargetProperty), Boolean)
    End Function

    ' Handle attached property changed to automatically target And untarget UIElements And Brushes.
    Public Shared Sub OnIsTargetChanged(obj As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim isAdding = DirectCast(e.NewValue, Boolean)

        If isAdding Then
            If TypeOf obj Is UIElement Then
                XamlLight.AddTargetElement(GetIdStatic(), TryCast(obj, UIElement))
            ElseIf TypeOf obj Is Brush Then
                XamlLight.AddTargetBrush(GetIdStatic(), TryCast(obj, Brush))
            End If
        Else
            If TypeOf obj Is UIElement Then
                XamlLight.RemoveTargetElement(GetIdStatic(), TryCast(obj, UIElement))
            ElseIf TypeOf obj Is Brush Then
                XamlLight.RemoveTargetBrush(GetIdStatic(), TryCast(obj, Brush))
            End If
        End If
    End Sub

    Protected Overrides Sub OnConnected(newElement As UIElement)
        If CompositionLight Is Nothing Then
            ' OnConnected Is called when the first target UIElement Is shown on the screen.
            ' This lets you delay creation of the composition object until it's actually needed.
            Dim spotLight = Window.Current.Compositor.CreateSpotLight()
            spotLight.InnerConeColor = Colors.Orange
            spotLight.OuterConeColor = Colors.Yellow
            spotLight.InnerConeAngleInDegrees = 30
            spotLight.OuterConeAngleInDegrees = 45
            CompositionLight = spotLight
        End If
    End Sub

    Protected Overrides Sub OnDisconnected(oldElement As UIElement)
        ' OnDisconnected Is called when there are no more target UIElements on the screen.
        ' The CompositionLight should be disposed when no longer required.
        If CompositionLight IsNot Nothing Then
            CompositionLight.Dispose()
            CompositionLight = Nothing
        End If
    End Sub

    Protected Overrides Function GetId() As String
        Return GetIdStatic()
    End Function

    Private Shared Function GetIdStatic() As String
        ' This specifies the unique name of the light.
        ' In most cases you should use the type's FullName.
        Return GetType(OrangeSpotLight).FullName
    End Function
End Class
```

```cppwinrt
// For the C++/WinRT code example below, you'll need to add a Midl File (.idl) file to your project.

// OrangeSpotLight.idl
namespace MyApp
{
    [default_interface]
    runtimeclass OrangeSpotLight : Windows.UI.Xaml.Media.XamlLight
    {
        OrangeSpotLight();
        static Windows.UI.Xaml.DependencyProperty IsTargetProperty{ get; };
        static Boolean GetIsTarget(Windows.UI.Xaml.DependencyObject target);
        static void SetIsTarget(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// OrangeSpotLight.h
struct OrangeSpotLight : OrangeSpotLightT<OrangeSpotLight>
{
    OrangeSpotLight() = default;

    winrt::hstring GetId();

    static Windows::UI::Xaml::DependencyProperty IsTargetProperty() { return m_isTargetProperty; }

    static bool GetIsTarget(Windows::UI::Xaml::DependencyObject const& target)
    {
        return winrt::unbox_value<bool>(target.GetValue(m_isTargetProperty));
    }

    static void SetIsTarget(Windows::UI::Xaml::DependencyObject const& target, bool value)
    {
        target.SetValue(m_isTargetProperty, winrt::box_value(value));
    }

    void OnConnected(Windows::UI::Xaml::UIElement const& newElement);
    void OnDisconnected(Windows::UI::Xaml::UIElement const& oldElement);

    static void OnIsTargetChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

    inline static winrt::hstring GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's full name.
        return winrt::xaml_typename<MyApp::OrangeSpotLight>().Name;
    }

private:
    static Windows::UI::Xaml::DependencyProperty m_isTargetProperty;
};

// OrangeSpotLight.cpp
Windows::UI::Xaml::DependencyProperty OrangeSpotLight::m_isTargetProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsTarget",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<MyApp::OrangeSpotLight>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false), Windows::UI::Xaml::PropertyChangedCallback{ &OrangeSpotLight::OnIsTargetChanged } }
);

void OrangeSpotLight::OnConnected(Windows::UI::Xaml::UIElement const& /* newElement */)
{
    if (!CompositionLight())
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        auto spotLight{ Windows::UI::Xaml::Window::Current().Compositor().CreateSpotLight() };
        spotLight.InnerConeColor(Windows::UI::Colors::Orange());
        spotLight.OuterConeColor(Windows::UI::Colors::Yellow());
        spotLight.InnerConeAngleInDegrees(30);
        spotLight.OuterConeAngleInDegrees(45);
        CompositionLight(spotLight);
    }
}

void OrangeSpotLight::OnDisconnected(Windows::UI::Xaml::UIElement const& /* oldElement */)
{
    // OnDisconnected is called when there are no more target UIElements on the screen.
    // Dispose of composition resources when no longer in use.
    if (CompositionLight())
    {
        CompositionLight(nullptr);
    }
}

winrt::hstring OrangeSpotLight::GetId()
{
    return OrangeSpotLight::GetIdStatic();
}

void OrangeSpotLight::OnIsTargetChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto uie{ d.try_as<Windows::UI::Xaml::UIElement>() };
    auto brush{ d.try_as<Windows::UI::Xaml::Media::Brush>() };

    auto isAdding = winrt::unbox_value<bool>(e.NewValue());
    if (isAdding)
    {

        if (uie)
        {
            Windows::UI::Xaml::Media::XamlLight::AddTargetElement(OrangeSpotLight::GetIdStatic(), uie);
        }
        else if (brush)
        {
            Windows::UI::Xaml::Media::XamlLight::AddTargetBrush(OrangeSpotLight::GetIdStatic(), brush);
        }
    }
    else
    {
        if (uie)
        {
            Windows::UI::Xaml::Media::XamlLight::RemoveTargetElement(OrangeSpotLight::GetIdStatic(), uie);
        }
        else if (brush)
        {
            Windows::UI::Xaml::Media::XamlLight::RemoveTargetBrush(OrangeSpotLight::GetIdStatic(), brush);
        }
    }
}

// MainPage.h
...
#include "OrangeSpotLight.h"
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();

        OrangeSpotLight::SetIsTarget(spotlitBrush(), true);
        OrangeSpotLight::SetIsTarget(spotlitUIElement(), true);
    }
...
};
```

```cppcx
// OrangeSpotLight.h:
public ref class OrangeSpotLight sealed :
    public Windows::UI::Xaml::Media::XamlLight
{
public:
    OrangeSpotLight();

    static property Windows::UI::Xaml::DependencyProperty^ IsTargetProperty
    {
        Windows::UI::Xaml::DependencyProperty^ get() { return m_isTargetProperty; }
    };
    static void SetIsTarget(Windows::UI::Xaml::DependencyObject^ target, bool value);
    static bool GetIsTarget(Windows::UI::Xaml::DependencyObject^ target);

protected:
    virtual void OnConnected(Windows::UI::Xaml::UIElement^ newElement) override;
    virtual void OnDisconnected(Windows::UI::Xaml::UIElement^ oldElement) override;
    virtual Platform::String^ GetId() override;

private:
    static Windows::UI::Xaml::DependencyProperty^ m_isTargetProperty;
    static void OnIsTargetChanged(Windows::UI::Xaml::DependencyObject^ obj, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e);

    inline static Platform::String^ GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return OrangeSpotLight::typeid->FullName;
    }
};

//OrangeSpotLight.cpp:

// Register an attached property that lets you set a UIElement
// or Brush as a target for this light type in markup.
DependencyProperty^ OrangeSpotLight::m_isTargetProperty = DependencyProperty::RegisterAttached(
    "IsTarget",
    bool::typeid,
    OrangeSpotLight::typeid,
    ref new PropertyMetadata(0.0, ref new PropertyChangedCallback(OnIsTargetChanged))
);

OrangeSpotLight::OrangeSpotLight()
{
}

void OrangeSpotLight::SetIsTarget(DependencyObject^ target, bool value)
{
    target->SetValue(IsTargetProperty, value);
}

bool OrangeSpotLight::GetIsTarget(DependencyObject^ target)
{
    return static_cast<bool>(target->GetValue(IsTargetProperty));
}

// Handle attached property changed to automatically target and untarget UIElements and Brushes.
void OrangeSpotLight::OnIsTargetChanged(DependencyObject^ obj, DependencyPropertyChangedEventArgs^ e)
{
    auto isAdding = static_cast<bool>(e->NewValue);

    if (isAdding)
    {
        if (dynamic_cast<UIElement^>(obj))
        {
            XamlLight::AddTargetElement(GetIdStatic(), static_cast<UIElement^>(obj));
        }
        else if (dynamic_cast<Brush^>(obj))
        {
            XamlLight::AddTargetBrush(GetIdStatic(), static_cast<Brush^>(obj));
        }
    }
    else
    {
        if (dynamic_cast<UIElement^>(obj))
        {
            XamlLight::RemoveTargetElement(GetIdStatic(), static_cast<UIElement^>(obj));
        }
        else if (dynamic_cast<Brush^>(obj))
        {
            XamlLight::RemoveTargetBrush(GetIdStatic(), static_cast<Brush^>(obj));
        }
    }
}
void OrangeSpotLight::OnConnected(UIElement^ newElement)
{
    if (CompositionLight == nullptr)
    {
        // OnConnected is called when the first target UIElement is shown on the screen.
        // This lets you delay creation of the composition object until it's actually needed.
        auto spotLight = Window::Current->Compositor->CreateSpotLight();
        spotLight->InnerConeColor = Colors::Orange;
        spotLight->OuterConeColor = Colors::Yellow;
        spotLight->InnerConeAngleInDegrees = 30;
        spotLight->OuterConeAngleInDegrees = 45;
        CompositionLight = spotLight;
    }
}

void OrangeSpotLight::OnDisconnected(UIElement^ oldElement)
{
    // OnDisconnected is called when there are no more target UIElements on the screen.
    // The CompositionLight should be disposed when no longer required.
    // For SDK 15063, see Remarks in the XamlLight class documentation.
    if (CompositionLight != nullptr)
    {
        delete CompositionLight;
        CompositionLight = nullptr;
    }
}

Platform::String^ OrangeSpotLight::GetId()
{
    return GetIdStatic();
}
```

Vous pouvez ensuite appliquer ce lumière à n’importe quel élément UIElement XAML ou le pinceau pour les déclencher. Cet exemple montre différentes utilisations potentielles.

> [!Important]
> Pour [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), supprimez les deux occurrences de `local:OrangeSpotLight.IsTarget="True"` à partir du balisage ci-dessous. Les propriétés jointes sont déjà définies dans le code-behind.

```xaml
<StackPanel Width="100">
    <StackPanel.Lights>
        <local:OrangeSpotLight/>
    </StackPanel.Lights>

    <!-- This border is lit by the OrangeSpotLight, but its content is not. -->
    <Border BorderThickness="4" Margin="2">
        <Border.BorderBrush>
            <SolidColorBrush x:Name="spotlitBrush" Color="White" local:OrangeSpotLight.IsTarget="True"/>
        </Border.BorderBrush>
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>

    <!-- This border and its content are lit by the OrangeSpotLight. -->
    <Border x:Name="spotlitUIElement" BorderThickness="4" BorderBrush="PaleGreen" Margin="2"
            local:OrangeSpotLight.IsTarget="True">
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>

    <!-- This border and its content are not lit by the OrangeSpotLight. -->
    <Border BorderThickness="4" BorderBrush="PaleGreen" Margin="2">
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>
</StackPanel>
```

Les résultats de ce code XAML se présente comme suit.

![Exemples d’éléments éclairés par une lumière xaml](images/orange-spot-light.png)

> [!Important]
> La définition de UIElement.Lights dans le balisage tel qu’indiqué dans l’exemple ci-dessus est uniquement prise en charge pour les applications ciblant au minimum la version Windows10 Creators Update ou une version ultérieure. Pour les applications ciblant des versions antérieures, les éclairages doivent être créés dans le code-behind.

## <a name="additional-resources"></a>Ressources supplémentaires

* Exemples d’interface utilisateur et de composition avancés dans le [GitHub WindowsUIDevLabs](https://github.com/microsoft/windowsuidevlabs).
