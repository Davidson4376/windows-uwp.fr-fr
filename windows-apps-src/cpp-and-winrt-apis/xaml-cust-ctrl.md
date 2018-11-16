---
author: stevewhims
description: Cette rubrique vous guide à travers les étapes de création d’un contrôle personnalisé simple à l’aide de C++ / WinRT. Vous pouvez générer sur les informations fournies ici pour créer vos propres contrôles d’interface utilisateur riche et personnalisables.
title: Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, un contrôle personnalisé basé sur un modèle,
ms.localizationpriority: medium
ms.openlocfilehash: 5e06a28125b3cf3a760d7e9170512b6467c0170d
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6858792"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT

> [!IMPORTANT]
> Pour les principaux concepts et termes facilitant votre compréhension utiliser et créer des classes runtime avec [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), voir [utiliser des API avec C++ / WinRT](consume-apis.md) et [créer des API avec C++ / WinRT](author-apis.md).

Une des fonctionnalités plus puissantes de la plateforme Windows universelle (UWP) est la flexibilité que la pile de l’interface utilisateur (UI) fournit pour créer des contrôles personnalisés en fonction du type XAML, [**contrôle**](/uwp/api/windows.ui.xaml.controls.control) . L’infrastructure XAML UI offre des fonctionnalités telles que les [Propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties) et les propriétés jointes et [modèles de contrôle](/windows/uwp/design/controls-and-patterns/control-templates), rendant facile à créer des contrôles riche et personnalisables. Cette rubrique vous guide à travers les étapes de création d’un contrôle personnalisé (modélisé) avec C++ / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Créer une application vide (BgLabelControlApp)
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créer un **Visual C++** > **Windows universel** > **application vide (C++ / WinRT)** de projet et nommez-le *BgLabelControlApp*.

Nous allons créer une nouvelle classe pour représenter un contrôle personnalisé (modélisé). La création et l’utilisation de la classe se feront au sein de la même unité de compilation. Mais nous voulons être en mesure d’instancier cette classe à partir de balisage XAML, c’est pourquoi qu'il va être une classe runtime. Et nous allons utiliser C++/WinRT à la fois pour la créer et l’utiliser.

La première étape de création d’une nouvelle classe runtime consiste à ajouter un nouvel élément **Fichier Midl (.idl)** au projet. Nommez-le `BgLabelControl.idl`. Supprimez le contenu par défaut de `BgLabelControl.idl` et collez-le dans cette déclaration de classe runtime.

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

La liste ci-dessus illustre le modèle que vous suivrez lors de la déclaration d’une propriété de dépendance (DP). Il existe deux éléments pour chaque point de distribution. Tout d’abord, vous déclarez une propriété statique en lecture seule de type [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Il porte le nom de votre *propriété*et du point de distribution. Vous allez utiliser cette propriété statique dans votre implémentation. En second lieu, vous déclarez une propriété en lecture-écriture instance avec le type et le nom de votre point de distribution.

> [!NOTE]
> Si vous souhaitez un point de distribution avec un type à virgule flottante, puis le rendre `double` (`Double` dans [MIDL 3.0](/uwp/midl-3/)). Déclarer et implémenter un point de distribution de type `float` (`Single` dans un fichier MIDL), et la définition d’une valeur pour ce point de distribution dans le balisage XAML, puis génère l’erreur *n’a pas pu créer un «Windows.Foundation.Single» à partir du texte '<NUMBER>'*.

Enregistrez le fichier et générez le projet. Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer un fichier de métadonnées Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) décrivant la classe runtime. Puis, l’outil `cppwinrt.exe` est exécuté pour générer les fichiers de code source et vous aider à créer et utiliser votre classe runtime. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BgLabelControl** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` et `BgLabelControl.cpp`.

Copiez les fichiers stub `BgLabelControl.h` et `BgLabelControl.cpp` à partir de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` dans le dossier de projet, à savoir `\BgLabelControlApp\BgLabelControlApp\`. Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implémentez la classe de contrôle personnalisé **BgLabelControl**
Maintenant, nous allons ouvrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` et `BgLabelControl.cpp`, et implémenter notre classe runtime. Dans `BgLabelControl.h`, modifiez le constructeur pour définir la clé de style par défaut, de mettre en œuvre **étiquette** et **LabelProperty**, ajoutez un gestionnaire d’événement statique nommé **OnLabelChanged** pour traiter les modifications apportées à la valeur de la propriété de dépendance, et ajoutez un membre privé pour stocker le champ de stockage pour **LabelProperty**.

Après avoir ajouté, votre `BgLabelControl.h` se présente comme suit.

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

Dans `BgLabelControl.cpp`, définir les membres statiques comme suit.

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // Call members of the projected type via theControl.

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

Dans cette procédure pas à pas, nous n’utilisez pas **OnLabelChanged**. Mais il est là afin que vous puissiez voir comment inscrire une propriété de dépendance avec un rappel de propriété modifiée. L’implémentation de **OnLabelChanged** montre également comment vous procurer un type projeté dérivé à partir d’un type projeté de base (le type projeté de base est **DependencyObject**, dans ce cas). Et il montre comment obtenir un pointeur vers le type qui implémente le type projeté. Cette opération deuxième naturellement n’est possible que dans le projet qui implémente le type projeté (autrement dit, le projet qui implémente la classe runtime).

> [!NOTE]
> Si vous n’avez pas installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809) ou une version ultérieure, vous devez appeler [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) dans le Gestionnaire d’événements ont été modifiés propriété dépendance ci-dessus, au lieu de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>Conception du style par défaut pour **BgLabelControl**

Dans son constructeur, **BgLabelControl** définit une clé de style par défaut pour lui-même. Mais quelle *est* un style par défaut? Un contrôle personnalisé (modélisé) doit avoir un style par défaut&mdash;contenant un modèle de contrôle par défaut&mdash;qu’elle peut utiliser pour être rendue au cas où le consommateur du contrôle n’a pas affecter un style et/ou le modèle. Dans cette section, nous allons ajouter un fichier de balisage pour le projet qui contient notre style par défaut.

Sous le nœud de votre projet, créez un dossier et nommez-le «Thèmes». Sous `Themes`, ajouter un nouvel élément de type **Visual C++** > **XAML** > **Vue XAML**et nommez-le «Generic.xaml». Les noms de dossier et le fichier doivent être comme suit afin que l’infrastructure XAML rechercher le style par défaut pour un contrôle personnalisé. Supprimez le contenu par défaut du `Generic.xaml`et le coller dans le balisage ci-dessous.

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

Dans ce cas, la seule propriété qui définit le style par défaut est le modèle de contrôle. Le modèle se compose d’un carré (dont en arrière-plan est liée à la propriété **Background** qui disposent de toutes les instances du type de XAML, [**contrôle**](/uwp/api/windows.ui.xaml.controls.control) ) et un élément de texte (dont le texte est lié à la propriété de dépendance **BgLabelControl::Label** ).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Ajouter une instance de **BgLabelControl** à la page principale de l’interface utilisateur

Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Immédiatement après l’élément de **bouton** (à l’intérieur de l' **élément StackPanel**), ajoutez le balisage suivant.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

En outre, ajoutez les éléments suivants incluent la directive à `MainPage.h` afin que le type de **MainPage** (il s’agit d’une combinaison de compilation de balisage XAML et le code impératif) soit informé du type de contrôle personnalisé **BgLabelControl** .

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Lancez à présent le processus de génération et exécutez le projet. Vous verrez que le modèle de contrôle par défaut est obligatoire pour le pinceau d’arrière-plan et à l’étiquette de l’instance **BgLabelControl** dans le balisage.

Cette procédure pas à pas vous a montré un exemple simple d’un contrôle personnalisé (modélisé) en C++ / WinRT. Vous pouvez rendre vos propres contrôles personnalisés au hasard riches et complètes. Par exemple, un contrôle personnalisé peut prendre la forme d’un élément aussi compliquée qu’une grille de données modifiable, un lecteur vidéo ou un visualiseur de géométrie 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Mise en œuvre *substituables* des fonctions telles que **MeasureOverride** et **OnApplyTemplate**

Vous dérivez un contrôle personnalisé à partir de la classe runtime de [**contrôle**](/uwp/api/windows.ui.xaml.controls.control) , qui a lui-même davantage dérive de classes runtime de base. Et il existe des méthodes substituables de **contrôle**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement)et [**UIElement**](/uwp/api/windows.ui.xaml.uielement) que vous pouvez remplacer dans votre classe dérivée. Voici un exemple de code que vous montrant comment effectuer cette opération.

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

Les fonctions *Overridable* se présentent différemment dans les projections de langage différentes. En c#, par exemple, fonctions substituables apparaissent généralement en tant que fonctions virtuelles protégées. En C++ / WinRT, elles sont protégées ni virtuel, mais vous pouvez toujours remplacer et fournir votre propre implémentation, comme indiqué ci-dessus.

## <a name="important-apis"></a>API importantes
* [Classe de contrôle](/uwp/api/windows.ui.xaml.controls.control)
* [Classe DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [Classe FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)
* [Classe UIElement](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Rubriquesconnexes
* [Modèles de contrôles](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties)
