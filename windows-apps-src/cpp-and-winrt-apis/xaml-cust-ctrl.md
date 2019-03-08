---
description: Cette rubrique vous guide tout au long des étapes de création d’un contrôle personnalisé simple à l’aide de C++ / c++ / WinRT. Vous pouvez créer sur les informations ici pour créer vos propres contrôles d’interface utilisateur riche et personnalisables.
title: Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c ++, cpp, winrt, la projection, XAML, un contrôle personnalisé et basé sur un modèle,
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce4f7eea074233c625a2cc92ef773f0b06c2be9f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635144"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT

> [!IMPORTANT]
> Pour les concepts essentiels et les conditions qui prennent en charge votre compréhension de la manière de consommer et créer des classes de runtime avec [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), consultez [consommer des API avec C / c++ / WinRT](consume-apis.md) et [API auteur avec C++ / c++ / WinRT](author-apis.md).

Une des fonctionnalités plus puissantes de la plateforme Windows universelle (UWP) est la flexibilité qui fournit de la pile de l’interface utilisateur (IU) pour créer des contrôles personnalisés basés sur le XAML [ **contrôle** ](/uwp/api/windows.ui.xaml.controls.control) type. L’infrastructure XAML UI fournit des fonctionnalités telles que [propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties) et [propriétés jointes](/windows/uwp/xaml-platform/custom-attached-properties), et [modèles de contrôle](/windows/uwp/design/controls-and-patterns/control-templates), qui facilitent la création contrôles riches et personnalisables. Cette rubrique vous guide tout au long des étapes de création d’un contrôle personnalisé (basé sur un modèle) avec C / c++ / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Créer une application vide (BgLabelControlApp)
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créer un **Visual C++** > **Windows universel** > **application vide (C++ / c++ / WinRT)** de projet et nommez-le *BgLabelControlApp* . Dans une section ultérieure de cette rubrique, vous serez dirigé pour générer votre projet (ne pas générer en attendant).

Nous allons créer une nouvelle classe pour représenter un contrôle personnalisé (basé sur un modèle). La création et l’utilisation de la classe se feront au sein de la même unité de compilation. Mais nous voulons être en mesure d’instancier cette classe à partir du balisage XAML et pour qui il doit être une classe d’exécution de la raison. Et nous allons utiliser C++/WinRT à la fois pour la créer et l’utiliser.

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

La liste ci-dessus illustre le modèle qui vous suivez lorsque vous déclarez une propriété de dépendance (DP). Il existe deux éléments pour chaque point de distribution. Tout d’abord, vous déclarez une propriété statique en lecture seule de type [ **DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Il porte le nom de votre point de distribution plu *propriété*. Vous allez utiliser cette propriété statique dans votre implémentation. En second lieu, vous déclarez une propriété en lecture-écriture instance avec le type et le nom de votre point de distribution. Si vous souhaitez créer un *propriété jointe* (au lieu d’un point de distribution), puis consultez les exemples de code dans [propriétés jointes personnalisées](/windows/uwp/xaml-platform/custom-attached-properties).

> [!NOTE]
> Si vous souhaitez un point de distribution avec un type à virgule flottante, puis d’en faire `double` (`Double` dans [MIDL 3.0](/uwp/midl-3/)). Déclaration et l’implémentation d’un point de distribution de type `float` (`Single` dans MIDL), et puis en définissant une valeur pour ce point de distribution dans le balisage XAML, génère l’erreur *Impossible de créer un « Windows.Foundation.Single » à partir du texte «<NUMBER>'*.

Enregistrez le fichier et générez le projet. Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer un fichier de métadonnées Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) décrivant la classe runtime. Puis, l’outil `cppwinrt.exe` est exécuté pour générer les fichiers de code source et vous aider à créer et utiliser votre classe runtime. Ces fichiers incluent des stubs pour vous aider à démarrer la mise en œuvre le **BgLabelControl** classe runtime que vous avez déclaré dans votre fichier IDL. Ces stubs sont `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` et `BgLabelControl.cpp`.

Copiez les fichiers stub `BgLabelControl.h` et `BgLabelControl.cpp` à partir de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` dans le dossier de projet, à savoir `\BgLabelControlApp\BgLabelControlApp\`. Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implémentez le **BgLabelControl** classe de contrôle personnalisé
Maintenant, nous allons ouvrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` et `BgLabelControl.cpp`, et implémenter notre classe runtime. Dans `BgLabelControl.h`, modifiez le constructeur pour définir l’implémentation de clé, de style par défaut **étiquette** et **LabelProperty**, ajoutez un gestionnaire d’événement statique nommé **OnLabelChanged** à traiter les modifications à la valeur de la propriété de dépendance, et ajouter un membre privé pour stocker le champ de stockage pour **LabelProperty**.

Après avoir ajouté, votre `BgLabelControl.h` ressemble à ceci.

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

Dans cette procédure pas à pas, nous ne l’utiliserons **OnLabelChanged**. Mais il n’y figure afin que vous puissiez voir comment inscrire une propriété de dépendance avec un rappel de modification de propriété. L’implémentation de **OnLabelChanged** montre également comment obtenir un type dérivé projeté à partir d’un type projeté de base (le type de base prévu est **DependencyObject**, dans ce cas). Et il montre comment obtenir un pointeur vers le type qui implémente le type projeté. Cette deuxième opération naturellement seulement sera possible dans le projet qui implémente le type prévu (autrement dit, le projet qui implémente la classe d’exécution).

> [!NOTE]
> Si vous n’avez pas installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809) ou version ultérieure, vous devez appeler [ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) dans le Gestionnaire d’événements modifiés dépendance propriété ci-dessus, au lieu de [ **winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>Concevoir le style par défaut **BgLabelControl**

Dans son constructeur, **BgLabelControl** définit une clé de style par défaut pour lui-même. Mais qu’en *est* un style par défaut ? Un contrôle personnalisé (basé sur un modèle) doit avoir un style par défaut&mdash;contenant un modèle de contrôle par défaut&mdash;qu’elle peut utiliser pour restituer lui-même avec, au cas où le consommateur du contrôle ne définit pas un style et/ou le modèle. Dans cette section, nous allons ajouter un fichier de balisage pour le projet contenant notre style par défaut.

Sous le nœud de votre projet, créez un dossier et nommez-le « Thèmes ». Sous `Themes`, ajouter un nouvel élément de type **Visual C++** > **XAML** > **XAML vue**et nommez-le « Generic.xaml ». Les noms de dossiers et de fichiers ont être similaire à celle-ci dans l’ordre pour l’infrastructure XAML rechercher le style par défaut pour un contrôle personnalisé. Supprimez le contenu par défaut du `Generic.xaml`et le coller dans le balisage ci-dessous.

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

Dans ce cas, la seule propriété qui définit le style par défaut est le modèle de contrôle. Le modèle se compose d’un carré (dont l’arrière-plan est lié à la **en arrière-plan** propriété que toutes les instances de le XAML [ **contrôle** ](/uwp/api/windows.ui.xaml.controls.control) type a) et un élément de texte (dont la propriété texte qui est lié à la **BgLabelControl::Label** propriété de dépendance).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Ajouter une instance de **BgLabelControl** à la page principale de l’interface utilisateur

Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Immédiatement après le **bouton** élément (à l’intérieur de la **StackPanel**), ajoutez le balisage suivant.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

En outre, ajoutez la directive à include suivante `MainPage.h` afin que le **MainPage** type (il s’agit d’une combinaison de la compilation du balisage XAML et le code impératif) tient compte de la **BgLabelControl** type de contrôle personnalisé. Si vous souhaitez utiliser **BgLabelControl** à partir d’une autre page XAML, ajoutez ce même directive #include pour le fichier d’en-tête pour cette page, trop. Ou bien, vous pouvez également simplement placer une seule directive include dans votre fichier d’en-tête précompilé.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Lancez à présent le processus de génération et exécutez le projet. Vous verrez que le modèle de contrôle par défaut est de la liaison pour le pinceau d’arrière-plan et à l’étiquette, de la **BgLabelControl** instance dans le balisage.

Cette procédure pas à pas vous a montré un exemple simple d’un contrôle personnalisé (basé sur un modèle) en C / c++ / WinRT. Vous pouvez rendre vos propres contrôles personnalisés arbitrairement riche et complet. Par exemple, un contrôle personnalisé peut prendre la forme de quelque chose comme complexes comme une grille de données modifiable, un lecteur vidéo ou un visualiseur de géométrie 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implémentation *substituable* fonctions, telles que **MeasureOverride** et **OnApplyTemplate**

Vous dérivez un contrôle personnalisé à partir de la [ **contrôle** ](/uwp/api/windows.ui.xaml.controls.control) classe runtime, qui elle-même davantage dérive les classes de runtime de base. Et il existe des méthodes substituables de **contrôle**, [ **FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement), et [ **UIElement** ](/uwp/api/windows.ui.xaml.uielement) que vous pouvez remplacer dans votre classe dérivée. Voici un exemple de code vous montrant comment procéder.

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

*Substituables* fonctions se présentent différemment dans les projections de langage différent. Dans C#, par exemple, les fonctions substituables apparaissent généralement sous forme de fonctions virtuelles protégées. En C / c++ / WinRT, ils sont ni virtuel ni protégé, mais vous pouvez les substituer et fournir votre propre implémentation, comme indiqué ci-dessus.

## <a name="important-apis"></a>API importantes
* [Classe de contrôle](/uwp/api/windows.ui.xaml.controls.control)
* [Classe DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement (classe)](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement (classe)](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Rubriques connexes
* [Modèles de contrôle](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties)
