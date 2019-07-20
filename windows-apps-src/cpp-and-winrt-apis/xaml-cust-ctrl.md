---
description: Cette rubrique vous guide tout au long des étapes de création d’un contrôle personnalisé simple à l’aide de C++/WinRT. Vous pouvez vous baser sur les informations présentées ici pour créer vos propres contrôles d’interface utilisateur riches en fonctionnalités et personnalisables.
title: Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, personnalisé, basé sur modèle, contrôle
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c0b2d8fb17b90bc55834f6bf2200b22af9352ef6
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270080"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Contrôles XAML personnalisés (basés sur un modèle) avec C++/WinRT

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec [C++/WinRT,](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

L’une des fonctionnalités les plus puissantes de la plateforme Windows universelle (UWP) est la flexibilité que fournit la pile de l’interface utilisateur (IU) pour créer des contrôles personnalisés basés sur le type [**Control**](/uwp/api/windows.ui.xaml.controls.control) XAML. Le framework IU XAML fournit des fonctionnalités comme les [propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties) et les [propriétés jointes](/windows/uwp/xaml-platform/custom-attached-properties), et des [modèles de contrôle](/windows/uwp/design/controls-and-patterns/control-templates), qui facilitent la création de contrôles personnalisables riches en fonctionnalités. Cette rubrique vous guide tout au long des étapes de création d’un contrôle personnalisé (basé sur modèle) à l’aide de C++/WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Créer une application vide (BgLabelControlApp)
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Application vide (C++/WinRT)** et nommez-le *BgLabelControlApp*. Dans une section ultérieure de cette rubrique, vous serez dirigé pour générer votre projet (ne le générez pas avant).

Nous allons créer une nouvelle classe pour représenter un contrôle personnalisé (basé sur modèle). La création et l’utilisation de la classe se feront au sein de la même unité de compilation. Mais nous voulons être en mesure d’instancier cette classe à partir du balisage XAML, c’est pourquoi il s’agira d’une classe runtime. Et nous allons utiliser C++/WinRT à la fois pour la créer et l’utiliser.

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

La liste ci-dessus illustre le modèle qui vous suivez lorsque vous déclarez une propriété de dépendance (DP). Il existe deux éléments pour chaque DP. Tout d’abord, vous déclarez une propriété statique en lecture seule de type [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Elle porte le nom de votre DP plus *Property*. Vous allez utiliser cette propriété statique dans votre implémentation. Ensuite, vous déclarez une propriété en lecture-écriture instance avec le type et le nom de votre DP. Si vous souhaitez créer une *propriété jointe* (au lieu d’une propriété de dépendance (DP)), consultez les exemples de code dans [Propriétés jointes personnalisées](/windows/uwp/xaml-platform/custom-attached-properties).

> [!NOTE]
> Si vous voulez une propriété de dépendance (DP) avec un type à virgule flottante, alors donnez-lui la valeur `double` (`Double` dans [MIDL 3.0](/uwp/midl-3/)). La déclaration et l’implémentation d’une propriété de dépendance (DP) de type `float` (`Single` dans MIDL), puis la définition d’une valeur pour cette propriété de dépendance dans le balisage XAML, génère l’erreur *Impossible de créer un « Windows.Foundation.Single » à partir du texte '<NUMBER>'* .

Enregistrez le fichier et générez le projet. Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer un fichier de métadonnées Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) décrivant la classe runtime. Puis, l’outil `cppwinrt.exe` est exécuté pour générer les fichiers de code source et vous aider à créer et utiliser votre classe runtime. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BgLabelControl** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` et `BgLabelControl.cpp`.

Copiez les fichiers stub `BgLabelControl.h` et `BgLabelControl.cpp` à partir de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` dans le dossier de projet, à savoir `\BgLabelControlApp\BgLabelControlApp\`. Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implémenter la classe de contrôle personnalisé **BgLabelControl**
Maintenant, nous allons ouvrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` et `BgLabelControl.cpp`, et implémenter notre classe runtime. Dans `BgLabelControl.h`, modifiez le constructeur pour définir la clé de style par défaut, implémentez **Label** et **LabelProperty**, ajoutez un gestionnaire d’événements statiques nommé **OnLabelChanged** pour traiter les modifications apportées à la valeur de la propriété de dépendance, et ajoutez un membre privé pour stocker le champ de sauvegarde pour **LabelProperty**.

Une fois ces éléments ajoutés, votre `BgLabelControl.h` se présente comme suit.

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

Dans `BgLabelControl.cpp`, définissez les membres statiques comme suit.

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

Dans cette procédure pas à pas, nous n’utiliserons pas **OnLabelChanged**. Mais il y figure afin que vous puissiez voir comment inscrire une propriété de dépendance avec un rappel de modification de propriété. L’implémentation de **OnLabelChanged** montre également comment obtenir un type dérivé projeté à partir d’un type projeté de base (le type de base projeté est **DependencyObject**, dans ce cas). Et cela montre comment obtenir un pointeur vers le type qui implémente le type projeté. Cette deuxième opération n’est possible que dans le projet qui implémente le type projeté (autrement dit, le projet qui implémente la classe de runtime).

> [!NOTE]
> Si vous n’avez pas installé le Kit de développement logiciel (SDK) Windows version 10.0.17763.0 (Windows 10, version 1809) ou version ultérieure, vous devez appeler [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) dans le gestionnaire d’événements de propriété de dépendance modifiée ci-dessus, au lieu de [**winrt::get_ Self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>Concevoir le style par défaut pour **BgLabelControl**

Dans son constructeur, **BgLabelControl** définit une clé de style par défaut pour lui-même. Mais *qu’est-ce* qu’un type par défaut ? Un contrôle personnalisé (basé sur modèle) doit avoir un style par défaut&mdash;contenant un modèle de contrôle par défaut&mdash;qu’il peut utiliser pour effectuer lui-même son rendu, au cas où le consommateur du contrôle ne définit pas un style et/ou un modèle. Dans cette section, nous allons ajouter un fichier de balisage pour le projet contenant notre style par défaut.

Sous le nœud de votre projet, créez un dossier et nommez-le « Themes ». Sous `Themes`, ajoutez un nouvel élément de type **Visual C++**  > **XAML** > **Vue XAML** et nommez-le « Generic.xaml ». Les noms du dossier et du fichier doivent être ainsi pour que le framework XAML recherche le style par défaut d’un contrôle personnalisé. Supprimez le contenu par défaut de `Generic.xaml` et collez le balisage ci-dessous.

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

Dans ce cas, la seule propriété que définit le style par défaut est le modèle de contrôle. Le modèle se compose d’un carré (dont l’arrière-plan est lié à la propriété **Background** que toutes les instances du type [**Control**](/uwp/api/windows.ui.xaml.controls.control) XAML ont) et un élément de texte (dont le texte est lié à la propriété de dépendance **BgLabelControl::Label**).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Ajouter une instance de **BgLabelControl** à la page principale de l’interface utilisateur

Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Immédiatement après l’élément **Button** (dans **StackPanel**), ajoutez le balisage suivant.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

En outre, ajoutez la directive include suivante à `MainPage.h` afin que le type **MainPage** (une combinaison de la compilation du balisage XAML et de code impératif) tienne compte du type de contrôle personnalisé **BgLabelControl**. Si vous souhaitez utiliser **BgLabelControl** à partir d’une autre page XAML, ajoutez cette même directive include pour le fichier d’en-tête de cette page. Ou bien, placez simplement une seule directive include dans votre fichier d’en-tête précompilé.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Lancez à présent le processus de génération et exécutez le projet. Vous verrez que le modèle de contrôle par défaut se lie au pinceau d’arrière-plan et à l’étiquette, de l’instance **BgLabelControl** du balisage.

Cette procédure pas à pas vous a montré un exemple simple d’un contrôle personnalisé (basé sur modèle) en C++/WinRT. Vous pouvez rendre vos propres contrôles personnalisés arbitrairement riches et complets. Par exemple, un contrôle personnalisé peut prendre la forme de quelque chose compliqué comme une grille de données modifiable, un lecteur vidéo ou un visualiseur de géométrie 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implémentation de fonctions *substituables*, comme **MeasureOverride** et **OnApplyTemplate**

Vous dérivez un contrôle personnalisé de la classe de runtime [**Control**](/uwp/api/windows.ui.xaml.controls.control), qui elle-même dérive des classes de runtime de base. Et il existe des méthodes substituables de **Control**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement) et [**UIElement**](/uwp/api/windows.ui.xaml.uielement) que vous pouvez remplacer dans votre classe dérivée. Voici un exemple de code illustrant comment procéder.

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

Les fonctions *substituables* se présentent différemment dans des projections de langage différentes. En C#, par exemple, les fonctions substituables apparaissent généralement sous forme de fonctions virtuelles protégées. En C++/WinRT, elles ne sont ni protégées ni virtuelles, mais vous pouvez quand même les remplacer et fournir votre propre implémentation, comme indiqué ci-dessus.

## <a name="important-apis"></a>API importantes
* [Classe Control](/uwp/api/windows.ui.xaml.controls.control)
* [Classe DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [Classe FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)
* [Classe UIElement](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Rubriques connexes
* [Modèles de contrôles](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties)
