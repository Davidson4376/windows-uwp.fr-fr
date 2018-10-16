---
author: stevewhims
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Présentation détaillée de la liaison de données
description: La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données et éventuellement rester synchronisée avec ces données.
ms.author: stwhi
ms.date: 10/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 906fb2d0d5d466f4fd691afd35ed96198929225c
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4623864"
---
# <a name="data-binding-in-depth"></a>Présentation détaillée de la liaison de données

**API importantes**

-   [**Extension de balisage {x:Bind}**](../xaml-platform/x-bind-markup-extension.md)
-   [**Classe Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

> [!NOTE]
> Cette rubrique décrit en détail les fonctionnalités de liaison de données. Pour une brève présentation pratique, voir [Vue d’ensemble de la liaison de données](data-binding-quickstart.md).

La liaison de données est un moyen dont dispose l’interface utilisateur de votre application pour afficher des données et éventuellement rester synchronisée avec ces données. La liaison de données vous permet de séparer les problématiques liées aux données de celles liées à l’interface utilisateur, ce qui se traduit par un modèle conceptuel plus simple et l’amélioration de la lisibilité, de la testabilité et de la gestion de la maintenance de votre application.

Vous pouvez utiliser la liaison de données pour simplement afficher des valeurs à partir d’une source de données lorsque l’interface utilisateur est affichée pour la première fois, et non pas pour répondre aux modifications apportées à ces valeurs. Il s’agit d’un mode de liaison appelée *à usage unique*, et elle fonctionne bien pour une valeur qui ne change pas pendant l’exécution. Par ailleurs, vous pouvez choisir de «observer» les valeurs et mettre à jour l’interface utilisateur lorsqu’ils sont modifiés. Plus appelée *à sens unique*, et elle fonctionne bien pour les données en lecture seule. Enfin, vous pouvez choisir d’observer les valeurs et de mettre à jour l’interface utilisateur de telle sorte que les modifications apportées par l’utilisateur aux valeurs de l’interface utilisateur soient transmises automatiquement à la source de données. Ce mode est appelé *bidirectionnelle*, et elle fonctionne bien pour les données en lecture-écriture. Voici quelques exemples.

-   Vous pouvez utiliser le mode à usage unique pour lier une [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) à la photo de l’utilisateur actuel.
-   Vous pouvez utiliser le mode à sens unique pour lier un [**contrôle ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) à une collection d’articles d’actualité en temps réel regroupés par section de journal.
-   Vous pouvez utiliser le mode bidirectionnel pour lier une [**zone de texte**](https://msdn.microsoft.com/library/windows/apps/BR209683) au nom d’un client dans un formulaire.

Quel que soit le mode, il existe deux types de liaison et qu’ils sont tous deux généralement déclarés dans le balisage de l’interface utilisateur. Vous pouvez choisir d’utiliser l’[extension de balisage {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou l’[extension de balisage {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Vous pouvez même utiliser une combinaison des deux dans la même application, voire pour un même élément d’interface utilisateur. {x:Bind}, une nouveauté de Windows10, offre de meilleures performances. Toutes les informations présentées dans cette rubrique s’appliquent à ces deux types de liaison, sauf explicitement indiqué.

**Exemples d’applications illustrant {x:Bind}**

-   [Exemple avec {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989).
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame).
-   [Exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=619992).

**Exemples d’applications illustrant {Binding}**

-   Téléchargez l’application [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950).
-   Téléchargez l’application [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952).

## <a name="every-binding-involves-these-pieces"></a>Chaque liaison implique les éléments suivants

-   Une *source de liaison*. C’est la source de données de la liaison ; il peut s’agir d’une instance de n’importe quelle classe qui présente des membres dont vous souhaitez afficher les valeurs dans votre interface utilisateur.
-   Une *cible de liaison*. Il s’agit d’une propriété [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) de l’élément [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) de votre interface utilisateur qui affiche les données.
-   Un *objet de liaison*. C’est l’élément qui transfère les valeurs de données de la source à la cible, et éventuellement de la cible à la source. L’objet de liaison est créé au moment du chargement du XAML à partir de votre extension de balisage [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) ou [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782).

Dans les sections suivantes, nous allons examiner de plus près la source de liaison, la cible de liaison et l’objet de liaison. Nous relierons ensuite ces sections avec un exemple de liaison du contenu d’un bouton à une propriété de chaîne nommée **NextButtonText**, qui appartient à une classe nommée **HostViewModel**.

### <a name="binding-source"></a>Source de liaison

Voici une implémentation très rudimentaire d’une classe que nous pourrions utiliser comme source de liaison.

Si vous utilisez des [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), puis ajouter de nouveaux éléments de **Fichier Midl (.idl)** au projet, nommé comme indiqué dans C++ / WinRT exemple l’intégralité du code ci-dessous. Remplacez le contenu de ces nouveaux fichiers par le code [MIDL 3.0](/uwp/midl-3/intro) indiqué dans la liste, générez le projet pour générer `HostViewModel.h` et `.cpp`, puis ajoutez le code pour les fichiers générés pour correspondre à la liste. Pour plus d’informations sur ces fichiers générés et comment les copier dans votre projet, consultez [contrôles XAML; liaison à C++ / WinRT propriété](/windows/uwp/cpp-and-winrt-apis/binding-property).

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

Cette implémentation de **HostViewModel** et sa propriété **NextButtonText** conviennent uniquement à une liaison ponctuelle. Toutefois, les liaisons à sens unique et bidirectionnelles sont très courantes, et avec ces types de liaison, l’interface utilisateur se met à jour automatiquement en réponse aux modifications apportées aux valeurs de données de la source de liaison. Pour que ces types de liaison fonctionnent correctement, vous devez rendre votre source de liaison «observable» par l’objet de liaison. Par conséquent, dans notre exemple, si nous voulons créer une liaison à sens unique ou bidirectionnelle à la propriété **NextButtonText**, toutes les modifications de la valeur de cette propriété qui surviennent au moment de l’exécution doivent être rendues observables par l’objet de liaison.

Une manière de le faire consiste à dériver d’un objet [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) la classe qui représente votre source de liaison et à exposer une valeur de données par l’intermédiaire d’une propriété [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362). C’est de cette façon qu’un élément [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) devient observable. Les éléments **FrameworkElements** constituent de bonnes sources de liaison sans aucune modification.

Une méthode moins lourde pour rendre une classe observable (méthode obligatoire pour les classes possédant déjà une classe de base) consiste à implémenter [**System.ComponentModel.INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx). Cela implique en fait simplement l’implémentation d’un événement unique nommé **PropertyChanged**. Vous trouverez ci-dessous un exemple utilisant **HostViewModel**.

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

La propriété **NextButtonText** est maintenant observable. Lorsque vous créez une liaison à sens unique ou bidirectionnelle à cette propriété (nous vous expliquerons comment ultérieurement), l’objet de liaison qui en résulte s’abonne à l’événement **PropertyChanged**. Lorsque cet événement est déclenché, le gestionnaire de l’objet de liaison reçoit un argument contenant le nom de la propriété qui a changé. Voilà comment l’objet de liaison connaît la propriété dont il doit lire à nouveau la valeur.

Afin que vous n’êtes pas obligé d’implémenter le modèle présenté ci-dessus plusieurs fois, si vous utilisez c#, alors vous pouvez simplement dériver de la classe de base de **BindableBase** que vous trouverez dans l’exemple [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) (dans le dossier «Common»). En voici un exemple:

```csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> Pour C++ / WinRT, n’importe quelle classe runtime que vous déclarez dans votre application qui dérive d’une classe de base est appelé une *composables* classe. Et il existe des contraintes autour de classes composables. Pour une application de passer les tests du [Kit de Certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) utilisés par Visual Studio et du Microsoft Store pour valider les soumissions (et donc de l’application être correctement ingérés par le Microsoft Store), une classe composable doit au final dériver d’une classe de base de Windows. Cela signifie que la classe à la racine très de la hiérarchie d’héritage doit être un type provenant d’un espace de noms Windows.*. Si vous n’avez pas besoin de dériver une classe runtime à partir d’une classe de base&mdash;par exemple, pour implémenter une classe **BindableBase** pour l’ensemble de vos modèles d’affichage de dériver de&mdash;, alors vous pouvez dériver de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).

Le déclenchement de l’événement **PropertyChanged** à l’aide d’un argument [**String.Empty**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.empty.aspx) ou **null** indique que toutes les propriétés autres que les propriétés d’indexeur doivent être lues à nouveau. Vous pouvez déclencher l’événement pour indiquer que les propriétés d’indexeur de l’objet ont été modifiées en utilisant un argument « Item\[*indexer*\] » pour des indexeurs spécifiques (où *indexer* correspond à la valeur d’index) ou une valeur « Item\[\] » pour tous les indexeurs.

Une source de liaison peut être traitée comme un objet simple dont les propriétés contiennent des données ou comme une collection d’objets. Dans le code c# et Visual Basic, vous pouvez créer une liaison ponctuelle à un objet qui implémente [**List (Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx) pour afficher une collection qui ne change pas au moment de l’exécution. Pour une collection observable (permettant d’observer quand des éléments sont ajoutés à la collection et supprimés de celle-ci), créez une liaison à sens unique à [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) à la place. Dans le code C++, vous pouvez établir une liaison à [**Vector&lt;T&gt;**](https://msdn.microsoft.com/library/dn858385.aspx) à la fois pour les collections observables et non observables. Pour lier vos propres classes de collection, utilisez les recommandations figurant dans le tableau suivant.

|Scénario|C# et VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|Créer une liaison à un objet.|Il peut s’agir de tout objet.|Il peut s’agir de tout objet.|L’objet doit avoir [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) ou implémenter [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).|
|Obtenir des notifications de modification de propriété à partir d’un objet lié.|Objet doit implémenter [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx).| Objet doit implémenter [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899).|Objet doit implémenter [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899).|
|Lier à une collection.| [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx)|[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)ou [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). Voir [contrôles d’éléments XAML; liaison à C++ / WinRT collection](../cpp-and-winrt-apis/binding-collection.md) et [Collections avec C++ / WinRT](../cpp-and-winrt-apis/collections.md).| [**Vecteur&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx)|
|Obtenir des notifications de modification de collection à partir d’une collection liée.|[**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx)|[**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable).|[**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/br226052.aspx)|
|Implémenter une collection prenant en charge la liaison.|Étendre [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx) ou implémenter [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx), [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/5y536ey6.aspx)(de [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)), [**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ienumerable.aspx) ou [**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/9eekhta0.aspx)(de **Object**). La liaison à des éléments **IList(Of T)** et **IEnumerable(Of T)** génériques n’est pas prise en charge.|Implémentez [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Voir [contrôles d’éléments XAML; liaison à C++ / WinRT collection](../cpp-and-winrt-apis/binding-collection.md) et [Collections avec C++ / WinRT](../cpp-and-winrt-apis/collections.md).|Implémenter [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; ou **IIterable**&lt;**IInspectable**\*&gt;. La liaison à des éléments génériques **IVector&lt;T&gt;** et **IIterable&lt;T&gt;** n’est pas prise en charge.|
| Implémenter une collection qui prend en charge les notifications de modification de collection. | Étendre [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) ou implémenter des éléments [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) et [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (non génériques).|Implémentez [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)ou [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).|Implémenter [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) et [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974).|
|Implémenter une collection prenant en charge un chargement incrémentiel.|Étendre [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) ou implémenter des éléments [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) et [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (non génériques). En outre, implémenter [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).|Implémentez [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)ou [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). En outre, implémenter [ **ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)|Implémenter [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) et [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).|

Vous pouvez lier des contrôles de listes à des sources de données très importantes et parvenir toutefois à atteindre un niveau de performance élevé à l’aide du chargement incrémentiel. Vous pouvez par exemple lier les contrôles de listes aux résultats de la requête d’image dans Bing sans avoir à charger tous les résultats simultanément. Vous pouvez charger une partie des résultats immédiatement, puis charger d’autres résultats si nécessaire. Pour prendre en charge du chargement incrémentiel, vous devez implémenter [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916) sur une source de données qui prend en charge les notifications de modification de collection. Lorsque le moteur de liaison de données demande davantage de données, votre source de données doit effectuer les demandes appropriées, intégrer les résultats, puis envoyer les notifications appropriées afin de mettre à jour l’interface utilisateur.

### <a name="binding-target"></a>Cible de liaison

Dans les deux exemples ci-dessous, la propriété **Button.Content** est la cible de liaison et sa valeur est définie sur une extension de balisage qui déclare l’objet de liaison. L’extension [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) est illustrée en premier, puis [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Déclarer les liaisons dans le balisage de liaisons dans le balisage est le scénario le plus courant (cela s’avère pratique, lisible et offre une compatibilité avec les outils). Cependant, vous pouvez éviter le balisage et créer de façon impérative (par programme) une instance de la classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) à la place si nécessaire.

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

Si vous utilisez C++ / extensions de composant WinRT ou Visual C++ (C++ / CX), vous devez ajouter l’attribut [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) à n’importe quelle classe runtime que vous souhaitez utiliser l’extension de balisage [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) avec.

> [!IMPORTANT]
> Si vous utilisez des [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), puis l’attribut [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) est disponible si vous avez installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809), ou une version ultérieure. Sans cet attribut, vous devez implémenter les interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) et [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) pour qu’ils soient en mesure d’utiliser l’extension de balisage [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) .

### <a name="binding-object-declared-using-xbind"></a>Objet de liaison déclaré à l’aide de {x:Bind}

Nous avons une étape à accomplir avant de créer notre balisage [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783). Nous devons exposer notre classe de source de liaison à partir de la classe qui représente notre page de balisage. Que faire, nous ajoutons une propriété (de type **HostViewModel** dans le cas présent) à notre classe de page **MainPage** .

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

Nous pouvons alors examiner de plus près le balisage qui déclare l’objet de liaison. L’exemple ci-dessous utilise la même cible de liaison **Button.Content** que dans la section «Cible de liaison» précédemment et montre qu’elle est liée à la propriété **HostViewModel.NextButtonText**.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notez la valeur que nous spécifions pour **Path**. Cette valeur est interprétée dans le contexte de la page elle-même, et dans ce cas, le chemin commence en référençant la propriété **ViewModel** que nous venons d’ajouter à la page **MainPage** . Cette propriété renvoie une instance **HostViewModel**, et nous pouvons donc ajouter un point à cet objet pour accéder à la propriété **HostViewModel.NextButtonText**. Nous spécifions également **Mode** pour remplacer la valeur ponctuelle par défaut de [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783).

La propriété [**Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path) prend en charge une diversité d’options de liaison à des propriétés imbriquées, des propriétés attachées ainsi qu’à des indexeurs de chaînes et d’entiers. Pour plus d’informations, voir [Syntaxe de PropertyPath](https://msdn.microsoft.com/library/windows/apps/Mt185586). La réalisation d’une liaison à des indexeurs de chaînes revient à effectuer une liaison à des propriétés dynamiques sans avoir besoin d’implémenter [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). Pour les autres paramètres, voir l’[extension de balisage {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783).

Pour illustrer que la propriété **HostViewModel.NextButtonText** est observable en effet, ajoutez un gestionnaire d’événements **Click** pour le bouton et mettre à jour la valeur de **HostViewModel.NextButtonText**. Générer, exécuter et cliquez sur le bouton pour afficher la valeur du **contenu** du bouton Mettre à jour.

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> Modifications apportées aux [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text) sont envoyées à une source dépendante bidirectionnelle lorsque la [**zone de texte**](https://msdn.microsoft.com/library/windows/apps/BR209683) perd le focus et non après chaque séquence de touches utilisateur.

**DataTemplate et x:DataType**

À l’intérieur d’un modèle [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (qu’il s’agisse d’un modèle d’élément, d’un modèle de contenu ou d’un modèle d’en-tête), la valeur de **Path** n’est pas interprétée dans le contexte de la page, mais dans celui de l’objet de données qui est basé sur le modèle. Pour que ses liaisons puissent être validées (et qu’un code efficace puisse être généré pour elles) au moment de la compilation, un modèle **DataTemplate** doit déclarer le type de son objet de données à l’aide de **x:DataType**. L’exemple ci-dessous peut être utilisé en tant que modèle **ItemTemplate** d’un contrôle d’éléments lié à une collection d’objets **SampleDataGroup**.

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Objets faiblement typés dans votre chemin d'accès**

Considérez par exemple que vous avez un type nommé SampleDataGroup, qui implémente une propriété de chaîne nommée Title. Et vous disposez d’une propriété MainPage.SampleDataGroupAsObject, qui est de type objet, mais qui retourne une instance de SampleDataGroup. La liaison `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` génère alors une erreur de compilation parce que la propriété Title est introuvable sur l’objet de type. La solution à ce problème consiste à ajouter un transtypage à votre syntaxe du chemin d’accès comme suit : `<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`. Voici un autre exemple où Element est déclaré comme objet mais est en fait un TextBlock : `<TextBlock Text="{x:Bind Element.Text}"/>`. Un transtypage résout le problème: `<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`.

**En cas de chargement asynchrone des données**

Le code pour prendre en charge **{x:Bind}** est généré au moment de la compilation dans les classes partielles pour vos pages. Ces fichiers se trouvent dans votre dossier `obj`, et portent des noms tels que `<view name>.g.cs` (pour C#). Le code généré inclut un gestionnaire pour l’événement [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706) de votre page, et ce gestionnaire appelle la méthode **Initialize** sur une classe générée qui représente les liaisons de votre page. Ensuite, **Initialize** appelle **Update** pour commencer à déplacer les données entre la source et la cible de liaison. **Loading** est déclenché juste avant la première passe de mesure du contrôle de page ou d’utilisateur. Si vos données sont chargées de façon asynchrone, elles peuvent ne pas être prêtes au moment où **Initialize** est appelée. Ainsi, une fois que vous avez chargé les données, vous pouvez forcer l’initialisation des liaisons uniques en appelant `this.Bindings.Update();`. Si vous avez uniquement besoin des liaisons à usage unique pour les données chargées de manière asynchrone, il est préférable de les initialiser de cette manière plutôt que d’avoir des liaisons à sens unique et d’écouter les modifications. Si vos données ne subissent pas de modifications affinées et si elles sont susceptibles d’être mises à jour dans le cadre d’une action spécifique, vous pouvez rendre vos liaisons uniques et forcer une mise à jour manuelle à tout moment avec un appel à **Update**.

> [!NOTE]
> **{x: Bind}** n’est pas adaptée aux scénarios à liaison tardive, telle que la navigation de la structure du dictionnaire d’un objet JSON, ni canard frappe. «Duck tapant» est un formulaire de faible de typage basé sur les correspondances lexicales sur les noms de propriété (un, «si elle parcourt nageant derrière et quacks comme un canard, il est un canard»). Avec canard frappe, une liaison à la propriété **Age** serait tout aussi satisfaite avec une **personne** ou un **vin** objet (en supposant que ces types de chaque avaient une propriété **Age** ). Pour ces scénarios, utilisez l’extension de balisage **{Binding}** .

### <a name="binding-object-declared-using-binding"></a>Objet de liaison déclaré à l’aide de {Binding}

Si vous utilisez C++ / extensions de composant WinRT ou Visual C++ (C++ / CX) ensuite, pour utiliser l’extension de balisage [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) , vous devez ajouter l’attribut [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) à n’importe quelle classe runtime que vous souhaitez lier. Pour utiliser [{x: Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), vous n’avez pas besoin cet attribut.

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> Si vous utilisez des [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), puis l’attribut [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) est disponible si vous avez installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809), ou une version ultérieure. Sans cet attribut, vous devez implémenter les interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) et [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) pour qu’ils soient en mesure d’utiliser l’extension de balisage [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) .

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) suppose, par défaut, que vous créiez une liaison à la propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) de votre page de balisage. Nous allons donc définir la propriété **DataContext** de notre page en tant qu’instance de notre classe de source de liaison (de type **HostViewModel** dans le cas présent). L’exemple ci-dessous illustre le balisage qui déclare l’objet de liaison. Nous utilisons la même cible de liaison **Button.Content** que dans la section «Cible de liaison» précédemment et nous la lions à la propriété **HostViewModel.NextButtonText**.

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

Notez la valeur que nous spécifions pour **Path**. Cette valeur est interprétée dans le contexte de la propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) de la page, qui dans cet exemple est définie en tant qu’instance de **HostViewModel**. Le chemin référence la propriété **HostViewModel.NextButtonText**. Nous pouvons omettre **Mode**, car la valeur ponctuelle par défaut de [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) fonctionne ici.

La valeur par défaut de [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) pour un élément d’interface utilisateur est la valeur héritée de son parent. Vous pouvez bien entendu remplacer cette valeur par défaut en définissant explicitement la propriété **DataContext**, qui est ensuite héritée par défaut par les enfants. Définir **DataContext** explicitement sur un élément s’avère utile lorsque vous voulez que plusieurs liaisons utilisent la même source.

Un objet de liaison présente une propriété **Source**, dont la valeur par défaut est la propriété [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) de l’élément d’interface utilisateur sur lequel la liaison est déclarée. Vous pouvez remplacer cette valeur par défaut en définissant **Source**,**RelativeSource** ou **ElementName** explicitement sur la liaison (voir [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) pour plus d’informations).

À l’intérieur d’un [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), le [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) est automatiquement définie pour l’objet de données qui est basé sur un modèle. L’exemple ci-dessous peut être utilisé en tant que modèle **ItemTemplate** d’un contrôle d’éléments lié à une collection de tout type présentant des propriétés de chaîne nommées **Title** et **Description**.

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> Par défaut, les modifications apportées aux [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text) sont envoyées à une source de dépendante bidirectionnelle lorsque la [**zone de texte**](https://msdn.microsoft.com/library/windows/apps/BR209683) perd le focus. Pour que les modifications soient envoyées après chaque séquence de touches de l’utilisateur, attribuez la valeur **PropertyChanged** à **UpdateSourceTrigger** sur la liaison dans le balisage. Vous pouvez également contrôler entièrement le moment où les modifications sont envoyées à la source en définissant **UpdateSourceTrigger** sur **Explicit**. Vous gérez ensuite les événements sur la zone de texte (généralement [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683)), appelez [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.getbindingexpression) sur la cible pour obtenir un objet [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.aspx) et appelez enfin [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.updatesource.aspx) pour mettre à jour la source de données par programmation.

La propriété [**Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path) prend en charge une diversité d’options de liaison à des propriétés imbriquées, des propriétés attachées ainsi qu’à des indexeurs de chaînes et d’entiers. Pour plus d’informations, voir [Syntaxe de PropertyPath](https://msdn.microsoft.com/library/windows/apps/Mt185586). La réalisation d’une liaison à des indexeurs de chaînes revient à effectuer une liaison à des propriétés dynamiques sans avoir besoin d’implémenter [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). La propriété [**ElementName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.elementname) est utile pour les liaisons d’élément à élément. La propriété [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.relativesource) a plusieurs usages et offre notamment une solution plus performante que la liaison de modèle à l’intérieur d’un modèle [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). Pour les autres paramètres, voir l’[extension de balisage {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) et la classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820).

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>Et si la source et la cible ne sont pas du même type?

Si vous voulez contrôler la visibilité d’un élément d’interface utilisateur en fonction de la valeur d’une propriété booléenne, effectuer le rendu d’un élément d’interface utilisateur avec une couleur qui est une fonction de la plage ou de la tendance d’une valeur numérique, ou afficher une valeur de date et/ou d’heure dans une propriété d’élément d’interface utilisateur qui attend une chaîne, vous devez convertir les valeurs d’un type à un autre. Dans certains cas, la solution consiste à exposer une autre propriété du type approprié à partir de votre classe de source de liaison et à faire en sorte que la logique de conversion y reste encapsulée et puisse être testée. Cependant, cette solution n’est ni souple ni évolutive quand vous avez un nombre important ou d’importantes combinaisons de propriétés sources et cibles. Dans ce cas, vous avez deux options:

* Si vous utilisez {x:Bind}, vous pouvez effectuer une liaison directe à une fonction pour effectuer cette conversion
* Sinon, vous pouvez spécifier un convertisseur de valeur qui est un objet conçu pour effectuer la conversion 

## <a name="value-converters"></a>Convertisseurs de valeurs

Voici un convertisseur de valeurs, adapté à une liaison ponctuelle ou à sens unique, qui convertit une valeur [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx) en valeur de chaîne contenant le mois. La classe implémente [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

```csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

Et voici comment ce convertisseur est utilisé dans votre balisage d’objet de liaison.

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

Le moteur de liaison appelle les méthodes [**Convert**](https://msdn.microsoft.com/library/windows/apps/hh701934) et [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/hh701938) si le paramètre [**Converter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converter) est défini pour la liaison. Lorsque les données sont transmises à partir de la source, le moteur de liaison appelle **Convert** et transmet à la cible les données renvoyées. Lorsque les données sont transmises à partir de la cible (pour une liaison bidirectionnelle), le moteur de liaison appelle **ConvertBack** et transmet à la source les données renvoyées.

Le convertisseur est également doté de paramètres optionnels: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterlanguage), qui autorise la spécification du langage à utiliser dans la conversion, et [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterparameter), qui autorise la transmission d’un paramètre pour la logique de conversion. Pour obtenir un exemple qui utilise un paramètre de convertisseur, voir [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

> [!NOTE]
> S’il existe une erreur dans la conversion, ne levez pas d’exception. Retournez plutôt [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyproperty.unsetvalue), qui arrêtera le transfert de données.

Pour afficher une valeur par défaut à utiliser chaque fois que la source de liaison ne peut pas être résolue, définissez la propriété **FallbackValue** sur l’objet de liaison dans le balisage. Cette méthode s’avère utile pour gérer les erreurs de conversion et de mise en forme. Elle est également utile pour la liaison aux propriétés sources qui peuvent ne pas exister sur tous les objets dans une collection liée de types hétérogènes.

Si vous liez un contrôle de texte à une valeur autre qu’une chaîne, le moteur de liaison de données convertit la valeur en chaîne. Si la valeur est un type de référence, le moteur de liaison de données récupère la valeur de chaîne en appelant [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) ou [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) s’ils sont disponibles, et sinon appelle [**Object.ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx). Notez, toutefois, que le moteur de liaison ignore toute implémentation de **ToString** qui masque l’implémentation de la classe de base. Les implémentations de sous-classe doivent plutôt remplacer la méthode **ToString** de la classe de base. De même, dans les langages natifs, tous les objets managés semblent implémenter [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) et [**IStringable**](https://msdn.microsoft.com/library/Dn302135). Toutefois, tous les appels à **GetStringRepresentation** et **IStringable.ToString** sont routés vers **Object.ToString** ou une substitution de cette méthode, et jamais vers une nouvelle implémentation de **ToString** qui masque l’implémentation de la classe de base.

> [!NOTE]
> Depuis Windows10, version1607, l’infrastructure XAML fournit un convertisseur intégré permettant de convertir une valeur booléenne en valeur Visibility. Le convertisseur mappe **true** à la valeur d’énumération **Visible**et **false** à la valeur d’énumération **Collapsed**. Vous pouvez ainsi lier une propriété Visibility à une valeur booléenne sans avoir à créer de convertisseur. Pour utiliser le convertisseur intégré, la version du SDK cible de votre application doit être14393 ou une version ultérieure. Vous ne pouvez pas l’utiliser si votre application cible des versions antérieures de Windows10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

## <a name="function-binding-in-xbind"></a>Liaison de fonction dans {x:Bind}

Avec {x:Bind}, l’étape finale d’un chemin de liaison peut être une fonction. Cela peut servir à effectuer des conversions et des liaisons qui dépendent de plusieurs propriétés. Consultez [ **fonctions dans x: Bind**](function-bindings.md)

<span id="resource-dictionaries-with-x-bind"/>

## <a name="resource-dictionaries-with-xbind"></a>Dictionnaires de ressources avec {x:Bind}

L’[extension de balisage {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) dépend de la génération du code et a donc besoin d’un fichier code-behind contenant un constructeur qui appelle **InitializeComponent** (pour initialiser le code généré). Vous réutilisez le dictionnaire de ressources en instanciant son type (afin que **InitializeComponent** soit appelé) au lieu de référencer son nom de fichier. Voici un exemple de la marche à suivre si vous avez un dictionnaire de ressources existant dans lequel vous souhaitez utiliser {x:Bind}.

TemplatesResourceDictionary.xaml

```xaml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

```csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

```xaml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

## <a name="event-binding-and-icommand"></a>Liaison d’événement et ICommand

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) prend en charge une fonctionnalité appelée liaison d’événement. Avec cette fonctionnalité, vous pouvez spécifier le gestionnaire d’un événement à l’aide d’une liaison, ce qui offre une option en plus de la gestion des événements à l’aide d’une méthode sur le fichier code-behind. Supposons que vous ayez une propriété **RootFrame** sur votre classe **MainPage**.

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

Vous pouvez alors lier l’événement **Click** d’un bouton à une méthode sur l’objet **Frame** renvoyé par la propriété **RootFrame** comme suit. Notez que nous avons également lié la propriété **IsEnabled** du bouton à un autre membre du même élément **Frame**.

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

Les méthodes surchargées ne peuvent pas être utilisées pour gérer un événement avec cette technique. En outre, si la méthode qui gère l’événement comporte des paramètres, ceux-ci doivent tous être attribuables à partir des types de l’ensemble des paramètres de l’événement, respectivement. Dans notre exemple, la méthode [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) n’est pas surchargée et elle ne comporte aucun paramètre (mais elle serait toujours valide même avec deux paramètres **object**). En revanche, la méthode [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) est surchargée et ne peut donc pas être utilisée avec cette technique.

La technique de liaison d’événement est similaire à l’implémentation et l’utilisation de commandes (une commande est une propriété qui renvoie un objet implémentant l’interface [**ICommand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.icommand.aspx)). Les extensions de balisage [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) et [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) fonctionnent toutes deux avec les commandes. Pour ne pas avoir à implémenter plusieurs fois le modèle de commande, vous pouvez utiliser la classe d’assistance **DelegateCommand**, disponible dans l’exemple [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) (dans le dossier «Common»).

## <a name="binding-to-a-collection-of-folders-or-files"></a>Liaison à une collection de dossiers ou de fichiers

Vous pouvez utiliser les API dans l’espace de noms [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) pour récupérer des données liées aux dossiers et aux fichiers. Toutefois, les différentes méthodes **GetFilesAsync**, **GetFoldersAsync** et **GetItemsAsync** ne retournent pas de valeurs qui conviennent pour la liaison aux contrôles de listes. Vous devez plutôt lier les valeurs retournées des méthodes [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428) et [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) de la classe [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501). L’exemple de code suivant provenant de l’[exemple StorageDataSource et GetVirtualizedFilesVector](http://go.microsoft.com/fwlink/p/?linkid=228621) illustre le modèle d’utilisation classique. Pensez à déclarer la fonctionnalité **picturesLibrary** dans le manifeste de votre package d’application et à vérifier que le dossier de votre bibliothèque d’images contient des images.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var library = Windows.Storage.KnownFolders.PicturesLibrary;
    var queryOptions = new Windows.Storage.Search.QueryOptions();
    queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
    queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

    var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

    var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
        fileQuery,
        Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
        190,
        Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
        false
        );

    var dataSource = fif.GetVirtualizedFilesVector();
    this.PicturesListView.ItemsSource = dataSource;
}
```

Généralement, vous utiliserez cette approche pour créer un affichage en lecture seule des informations sur le fichier et le dossier. Vous pouvez créer des liaisons bidirectionnelles aux propriétés de fichiers et de dossiers, par exemple pour permettre aux utilisateurs de noter une chanson dans un affichage de musique. Toutefois, les modifications apportées ne sont pas conservées tant que vous n’avez pas appelé la méthode **SavePropertiesAsync** appropriée (par exemple, [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)). Vous devez valider les modifications lorsque l’élément ne correspond plus à la zone active afin de ne pas déclencher la réinitialisation de la sélection.

Notez que la liaison bidirectionnelle utilisant cette technique ne fonctionne qu’avec les emplacements indexés, tels que Musique. Vous pouvez déterminer si un emplacement est indexé en appelant la méthode [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627).

Notez également qu’un vecteur virtualisé peut retourner **null** pour certains éléments avant qu’il ne renseigne leur valeur. Par exemple, vous devriez rechercher la valeur **null** avant d’utiliser la valeur [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) d’un contrôle de liste lié à un vecteur virtualisé, ou utiliser [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768) à la place.

## <a name="binding-to-data-grouped-by-a-key"></a>Liaison de données groupées en fonction d’une clé

Si vous prenez une collection plate d’éléments (livres, par exemple, représentées par une classe **BookSku** ) et vous groupez les éléments à l’aide d’une propriété commune en tant que clé (par exemple, la propriété **BookSku.AuthorName** ), puis le résultat est appelé des données groupées. Lorsque vous groupez les données, la collection n’est plus plate. Les données groupées sont une collection d’objets de groupe, où chaque objet de groupe a

- une clé, et
- une collection d’éléments dont la propriété correspond à cette clé.

Pour prendre l’exemple de livres à nouveau, le résultat de regroupement par nom de l’auteur aboutit à une collection de groupes où chaque groupe possède

- une clé, qui est le nom de l’auteur, et
- une collection des s **BookSku**dont la propriété **AuthorName** correspond à la clé du groupe.

En règle générale, pour afficher une collection, vous liez la propriété [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) d’un contrôle d’éléments (tel que [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) directement à une propriété qui renvoie une collection. S’il s’agit d’une collection plate d’éléments, vous n’avez pas besoin d’effectuer d’opération particulière. Mais s’il s’agit d’une collection d’objets de groupe (comme c’est le cas pour une liaison à des données groupées), vous avez besoin des services d’un objet intermédiaire appelé [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), qui se trouve entre le contrôle d’éléments et la source de liaison. Vous liez l’objet **CollectionViewSource** à la propriété qui renvoie les données groupées et vous liez le contrôle d’éléments à l’objet **CollectionViewSource**. L’objet **CollectionViewSource** a pour autre avantage d’assurer le suivi de l’élément actif, de sorte que vous pouvez synchroniser plusieurs contrôles d’éléments en permanence en les liant tous au même objet **CollectionViewSource**. Vous pouvez également accéder à l’élément actif par programme via la propriété [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) de l’objet renvoyé par la propriété [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.view).

Pour activer la fonctionnalité de regroupement d’un objet [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), définissez [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.issourcegrouped) sur **true**. La nécessité de définir également la propriété [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.itemspath) dépend de la manière dont vous créez vos objets de groupe. Vous pouvez créer un objet de groupe de deux manières: à l’aide d’un modèle «is-a-group» ou d’un modèle «has-a-group». Dans le modèle «is-a-group», l’objet de groupe dérive d’un type de collection (par exemple, **List&lt;T&gt;**) et correspond donc en fait au groupe d’éléments. Avec ce modèle, vous n’avez pas besoin de définir **ItemsPath**. Dans le modèle « has-a-group », l’objet de groupe comporte une ou plusieurs propriétés d’un type de collection (par exemple, **List&lt;T&gt;**), de sorte que le groupe comporte un groupe d’éléments sous la forme d’une propriété (ou plusieurs groupes d’éléments sous la forme de plusieurs propriétés). Avec ce modèle, vous devez définir **ItemsPath** sur le nom de la propriété qui contient le groupe d’éléments.

L’exemple suivant illustre le modèle «has-a-group». La classe de page comporte une propriété nommée [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713), qui renvoie une instance de notre modèle d’affichage. L’objet [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) se lie à la propriété **Authors** du modèle d’affichage (**Authors** est la collection d’objets de groupe) et indique que c’est la propriété **Author.BookSkus** qui contient les éléments groupés. Enfin, la classe [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) est liée à l’objet **CollectionViewSource** et son style de groupe est défini de manière à pouvoir afficher les éléments en groupes.

```csharp
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

Vous pouvez implémenter le modèle «is-a-group» de deux manières. La première consiste à créer votre propre classe de groupe. Dérivez la classe de **List&lt;T&gt;** (où *T* est le type des éléments). Exemple : `public class Author : List<BookSku>`. La deuxième consiste à utiliser une expression [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) afin de créer dynamiquement des objets de groupe (et une classe de groupe) à partir des valeurs de propriétés similaires des éléments **BookSku**. Cette approche, consistant à conserver simplement une liste plate d’éléments et à les regrouper à la volée, est courante pour les applications qui accèdent aux données à partir d’un service cloud. Elle vous offre la possibilité de regrouper les ouvrages par auteur et par genre (par exemple) sans avoir à recourir à des classes de groupes spécifiques, comme **Author** et **Genre**.

L’exemple suivant illustre le modèle «is-a-group» avec [LINQ](http://msdn.microsoft.com/library/bb397926.aspx). Cette fois-ci, nous regroupons les ouvrages par genre, avec le nom du genre affiché dans les en-têtes des groupes. Cela est indiqué par le chemin de la propriété «Key» en référence à la valeur du groupe [**Key**](https://msdn.microsoft.com/library/windows/apps/bb343251.aspx).

```csharp
using System.Linq;
...
private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
{
    get
    {
        if (this.genres == null)
        {
            this.genres = from book in this.bookSkus
                          group book by book.genre into grp
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

Gardez à l’esprit que pour utiliser [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) avec des modèles de données, nous devons indiquer le type en cours de liaison en définissant une valeur **x:DataType**. Si le type est générique, nous ne pouvons pas l’exprimer dans le balisage et nous devons par conséquent utiliser[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) à la place dans le modèle d’en-tête du style de groupe.

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

Un contrôle [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) offre à vos utilisateurs un excellent moyen de visualiser et de parcourir les données groupées. L’exemple d’application [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) montre comment utiliser le contrôle **SemanticZoom**. Dans cette application, vous pouvez afficher une liste d’ouvrages regroupés par auteur (vue avec zoom avant) ou vous pouvez effectuer un zoom arrière pour afficher une liste de raccourcis pour les auteurs (vue avec zoom arrière). Grâce à cette liste, vous pouvez vous déplacer beaucoup plus rapidement que si vous faisiez défiler la liste des ouvrages. Les vues avec zoom avant et zoom arrière sont en fait des contrôles **ListView** ou **GridView** liés au même objet **CollectionViewSource**.

![Illustration d’un contrôle SemanticZoom](images/sezo.png)

Lorsque vous liez des données hiérarchiques, telles que des sous-catégories au sein de catégories, vous pouvez choisir d’afficher les niveaux hiérarchiques dans votre interface utilisateur avec une série de contrôles d’éléments. Une sélection au sein d’un contrôle d’éléments détermine le contenu des contrôles d’éléments suivants. Vous pouvez assurer la synchronisation des listes en liant chaque liste à son propre objet [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) et les instances **CollectionViewSource** ensemble dans une chaîne. C’est ce qu’on appelle un affichage maître/détails (ou liste/détails). Pour plus d’informations, voir [Comment lier des données hiérarchiques et créer un affichage maître/détails](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>Diagnostic et débogage des problèmes de liaison de données

Votre balisage de liaison contient les noms des propriétés (et parfois les champs et les méthodes, dans le cas de C#). Lorsque vous renommez une propriété, vous devez donc également modifier les liaisons qui y font référence. Toute omission donnera lieu à un exemple type de bogue de liaison de données et votre application ne sera pas compilée ou exécutée correctement.

Les objets de liaison créés par [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) et [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) sont en grande partie équivalents du point de vue fonctionnel. Cependant, {x:Bind} comporte des informations de type pour la source de liaison et génère du code source lors de la compilation. Avec {x:Bind}, vous bénéficiez du même type de détection de problèmes qu’avec le reste de votre code. Cela inclut la validation de vos expressions de liaison au moment de la compilation et le débogage en définissant des points d’arrêt dans le code source généré en tant que classe partielle pour votre page. Ces classes se trouvent dans les fichiers de votre dossier `obj`, avec des noms comme `<view name>.g.cs` (pour C#)). Si vous rencontrez un problème avec une liaison, activez l’option **Point d'arrêt sur les exceptions non gérées** dans le débogueur Microsoft Visual Studio. Le débogueur interrompra alors l’exécution et vous pourrez déboguer le problème en question. Le code généré par {x:Bind} suit le même modèle pour chaque partie du graphique des nœuds de la source de liaison et vous pouvez utiliser les informations de la fenêtre **Pile des appels** pour vous aider à déterminer la séquence d’appels qui a donné lieu au problème.

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) ne comporte pas d’informations de type pour la source de liaison. Cependant, lorsque vous exécutez votre application avec le débogueur attaché, toute erreur de liaison apparaît dans la fenêtre **Sortie** de Visual Studio.

## <a name="creating-bindings-in-code"></a>Création de liaisons dans le code

**Remarque** Cette section s’applique uniquement à [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), car vous ne pouvez pas créer de liaisons [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) dans le code. Toutefois, il est possible de profiter des avantages de {x:Bind} avec [**DependencyObject.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.registerpropertychangedcallback.aspx), qui vous permet de vous inscrire aux notifications de modification des propriétés de dépendance.

Vous pouvez également connecter des éléments d’interface utilisateur aux données en utilisant un code procédural et non XAML. Pour ce faire, créez un objet [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820), définissez les propriétés appropriées, puis appelez [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257.aspx) ou [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244376.aspx). La création de liaisons par programme est utile quand vous voulez choisir les valeurs des propriétés de liaison au moment de l’exécution ou partager une liaison unique entre plusieurs contrôles. Notez, toutefois, que vous ne pouvez pas modifier les valeurs des propriétés de liaison après avoir appelé **SetBinding**.

L’exemple suivant explique comment implémenter une liaison dans le code.

```xaml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

```vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

## <a name="xbind-and-binding-feature-comparison"></a>Comparaison des fonctionnalités de {x:Bind} et {Binding}

| Fonctionnalité | {x:Bind} | {Binding} | Notes |
|---------|----------|-----------|-------|
| Path est la propriété par défaut. | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Propriété Path | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | Dans x:Bind, Path a pour racine Page par défaut et non DataContext. | 
| Indexeur | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Se lie à l’élément spécifié dans la collection. Seuls les index basés sur des entiers sont pris en charge. | 
| Propriétés jointes | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Les propriétés jointes sont spécifiées à l’aide de parenthèses. Si la propriété n’est pas déclarée dans un espace de noms XAML, vous devez la faire précéder d’un espace de noms xml mappé sur un espace de noms de code au début du document. | 
| Transtypage | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | Inutile. | Les conversions de type (transtypage) sont spécifiées à l’aide de parenthèses. Si la propriété n’est pas déclarée dans un espace de noms XAML, vous devez la faire précéder d’un espace de noms xml mappé sur un espace de noms de code au début du document. | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Les convertisseurs doivent être déclarés à la racine de Page/ResourceDictionary ou dans le fichier App.xaml. | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Les convertisseurs doivent être déclarés à la racine de Page/ResourceDictionary ou dans le fichier App.xaml. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Utilisée lorsque le nœud terminal de l’expression de liaison présente la valeur null. Utilisez des guillemets simples pour une valeur de chaîne. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Utilisée lorsqu’une partie du chemin de la liaison (à l’exception du nœud terminal) présente la valeur null. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Avec {x:Bind}, vous créez une liaison à un champ; Path a pour racine Page par défaut, de sorte que tout élément nommé est accessible via son champ. | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | Avec {x:Bind}, nommez l’élément et utilisez son nom dans Path. | 
| RelativeSource : TemplatedParent | Pas nécessaire | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | Avec {x: Bind} TargetType sur ControlTemplate indique que la liaison à un parent de modèle. Pour {Binding} liaison de modèle standard peut être utilisée dans les modèles de contrôle pour la plupart des utilisations. Toutefois, faites appel à TemplatedParent quand vous devez utiliser un convertisseur ou une liaison bidirectionnelle.&lt; | 
| Source | Pas nécessaire | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | Pour {x: Bind}, vous pouvez utiliser directement l’élément nommé, utilisez une propriété ou un chemin statique. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode peut être défini sur OneTime (liaison ponctuelle), OneWay (liaison à sens unique) ou TwoWay (liaison bidirectionnelle). La valeur par défaut est OneTime pour {x:Bind} et OneWay pour {Binding}. | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger peut avoir la valeur Default, PropertyChanged ou LostFocus. {x:Bind} ne prend pas en charge UpdateSourceTrigger=Explicit. {x:Bind} utilise le comportement PropertyChanged dans tous les cas, sauf pour TextBox.Text, où il utilise le comportement LostFocus. | 


