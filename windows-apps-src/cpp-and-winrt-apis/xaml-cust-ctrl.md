---
author: stevewhims
description: Cette rubrique vous guide tout au long des étapes de création d’un contrôle personnalisé simple à l’aide de C + / WinRT. Vous pouvez créer les informations ici pour créer vos propres contrôles d’interface utilisateur riche et personnalisables.
title: XAML (basé sur un modèle) des contrôles personnalisés avec C + / WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c ++, RPC, winrt, projection, XAML, personnalisé, basé sur un modèle, contrôle
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2843691"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Des contrôles personnalisés (basé sur un modèle) XAML avec [C + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Une des fonctionnalités plus puissantes de la plateforme Windows universel (UWP) est la souplesse offrant la pile de l’interface utilisateur (IU) pour créer des contrôles personnalisés en fonction du type XAML [contrôle](/uwp/api/windows.ui.xaml.controls.control) . L’infrastructure UI XAML fournit des fonctionnalités telles que les [Propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties) et propriétés attachées et [modèles de contrôle](/windows/uwp/design/controls-and-patterns/control-templates), qui facilitent la création de contrôles riche et personnalisables. Cette rubrique vous guide tout au long des étapes de création d’un contrôle personnalisé (modélisé) avec C + / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Créer une application vide (BgLabelControlApp)
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créer un **App de vide Visual C++ (C + / WinRT)** de projet et nommez-le *BgLabelControlApp*.

Nous allons créer une nouvelle classe pour représenter un contrôle personnalisé (modélisé). La création et l’utilisation de la classe se feront au sein de la même unité de compilation. Mais nous voulons être en mesure d’instancier cette classe à partir du balisage XAML et pour qui il doit être une classe runtime de motif. Et nous allons utiliser C++/WinRT à la fois pour la créer et l’utiliser.

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

La liste ci-dessus montre le modèle que vous suivez lorsque vous déclarez une propriété de dépendance (DP). Il existe deux éléments à chaque point de distribution. Tout d’abord, vous déclarez une propriété en lecture seule statique de type [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty). Elle porte le nom de votre *propriété*et du point de distribution. Vous allez utiliser cette propriété statique dans votre implémentation. Deuxièmement, vous déclarez une propriété en lecture-écriture avec le type et le nom de votre point de distribution.

> [!NOTE]
> Si vous souhaitez un point de distribution à un type à virgule flottante, puis rendre `double` (`Double` dans [MIDL 3.0](/uwp/midl-3/)). Déclaration et l’implémentation d’un point de distribution de type `float` (`Single` dans MIDL), et une valeur pour ce point de distribution en XAML, les résultats de l’erreur *n’a pas pu créer un «Windows.Foundation.Single» à partir du texte '<NUMBER>'*.

Enregistrez le fichier et générez le projet. Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer un fichier de métadonnées Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) décrivant la classe runtime. Puis, l’outil `cppwinrt.exe` est exécuté pour générer les fichiers de code source et vous aider à créer et utiliser votre classe runtime. Ces fichiers incluent stub pour vous aider à l’implémentation de la classe runtime **BgLabelControl** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` et `BgLabelControl.cpp`.

Copiez les fichiers stub `BgLabelControl.h` et `BgLabelControl.cpp` à partir de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` dans le dossier de projet, à savoir `\BgLabelControlApp\BgLabelControlApp\`. Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implémentation de la classe de contrôle personnalisé **BgLabelControl**
Maintenant, nous allons ouvrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` et `BgLabelControl.cpp`, et implémenter notre classe runtime. Dans `BgLabelControl.h`, modifiez le constructeur pour définir la clé de style par défaut, implémentez **étiquette** et **LabelProperty**, ajouter un gestionnaire d’événement statique nommée **OnLabelChanged** pour traiter les modifications apportées à la valeur de la propriété de dépendance et ajouter un membre privé pour stocker le champ de stockage pour **LabelProperty**.

Dans cette procédure pas à pas, nous n’utilisant **OnLabelChanged**. Mais il n’y figure afin que vous pouvez voir comment enregistrer une propriété de dépendance avec un rappel de modification de propriété.

Après avoir ajouté les, votre `BgLabelControl.h` se présente comme suit.

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

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

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

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>Conception du style par défaut pour **BgLabelControl**

Dans son constructeur, **BgLabelControl** définit une clé de style par défaut pour lui-même. Mais quel *est* un style par défaut? Un contrôle personnalisé (modélisé) doit avoir un style par défaut&mdash;contenant un modèle de contrôle par défaut&mdash;qui permet de s’afficher au cas où un style ou le modèle n’affecte pas le consommateur du contrôle. Dans cette section, nous allons ajouter un fichier de balisage pour le projet qui contient notre style par défaut.

Sous le nœud de votre projet, créez un nouveau dossier et nommez-le «Thèmes». Sous `Themes`, ajoutez un nouvel élément de type **Visual C++** > **XAML** > **Vue XAML**et nommez-la «Generic.xaml». Les noms de dossiers et de fichiers doivent être comme suit dans l’ordre pour l’infrastructure XAML rechercher le style par défaut pour un contrôle personnalisé. Supprimez le contenu par défaut du `Generic.xaml`et le coller dans le balisage ci-dessous.

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

Dans ce cas, la seule propriété qui définit le style par défaut est le modèle de contrôle. Le modèle se compose d’un carré (dont arrière-plan est lié à la propriété **Background** dont toutes les instances du type XAML [contrôle](/uwp/api/windows.ui.xaml.controls.control) ) et un élément de texte (dont le texte est lié à la propriété de dépendance **BgLabelControl::Label** ).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Ajouter une instance de **BgLabelControl** dans la page principale de l’interface utilisateur

Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Immédiatement après l’élément **Button** (à l’intérieur de l' **objet StackPanel**), ajoutez le balisage suivant.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

En outre, ajouter ce qui suit inclure la directive de `MainPage.h` afin que le type de **MainPage** (une combinaison de la compilation de balisage XAML et le code impératif) a connaissance du type de contrôle personnalisé **BgLabelControl** .

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Lancez à présent le processus de génération et exécutez le projet. Vous verrez que le modèle de contrôle par défaut est obligatoire pour le pinceau d’arrière-plan et l’étiquette, de l’instance **BgLabelControl** dans le balisage.

Cette procédure pas à pas a montré un exemple simple d’un contrôle personnalisé (basé sur un modèle) dans C + / WinRT. Vous pouvez rendre vos propres contrôles personnalisés arbitrairement riches et complètes. Par exemple, un contrôle personnalisé peut prendre la forme d’un élément aussi compliquée qu’un visualiseur de géométrie 3D, un lecteur vidéo ou une grille de données modifiables.

## <a name="important-apis"></a>API importantes
* [Contrôle](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>Rubriques associées
* [Modèles de contrôles](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propriétés de dépendance personnalisées](/windows/uwp/xaml-platform/custom-dependency-properties)
