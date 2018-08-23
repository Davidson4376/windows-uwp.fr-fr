---
author: stevewhims
description: Une propriété qui peut être efficacement liée à un contrôle XAML est appelée «propriété *observable*». Cette rubrique montre comment implémenter et utiliser une propriété observable, et comment y lier un contrôle XAML.
title: Contrôles XAML; liaison à une propriété C++/WinRT
ms.author: stwhi
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, liaison, propriété
ms.localizationpriority: medium
ms.openlocfilehash: 6343832801926254c64fcefc269ce7fda9ed6dfc
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2817506"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>Contrôles XAML; liaison à une propriété [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Une propriété qui peut être efficacement liée à un contrôle XAML est appelée «propriété *observable*». Ce concept est basé sur le modèle de conception logicielle appelé «*modèle observateur*». Cette rubrique montre comment implémenter des propriétés observables en C++/WinRT et y lier des contrôles XAML.

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>Que signifie «*observable*» pour une propriété?
Supposons qu’une classe runtime nommée **BookSku** a une propriété nommée **Title**. Si **BookSku** choisit de déclencher l’événement [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) chaque fois que la valeur de **Title** change, alors **Title** est une propriété observable. C’est le comportement de **BookSku** (déclenchement ou non de l’événement) qui détermine lesquelles de ses propriétés, le cas échéant, sont observables.

Un élément de texte XAML, ou contrôle, peut lier et gérer ces événements en récupérant la ou les valeur(s) mise(s) à jour et en se mettant à jour lui-même pour afficher la nouvelle valeur.

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="create-a-blank-app-bookstore"></a>Créer une application vide (Bookstore)
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Application vide Visual C++ (C++/WinRT)** et nommez-le *Bookstore*.

Nous allons créer une nouvelle classe pour représenter un livre qui a une propriété de titre observable. La création et l’utilisation de la classe se feront au sein de la même unité de compilation. Mais nous voulons être en mesure d’établir une liaison à cette classe à partir de XAML, c’est pourquoi il s’agira d’une classe runtime. Et nous allons utiliser C++/WinRT à la fois pour la créer et l’utiliser.

La première étape de création d’une nouvelle classe runtime consiste à ajouter un nouvel élément **Fichier Midl (.idl)** au projet. Nommez-le `BookSku.idl`. Supprimez le contenu par défaut de `BookSku.idl` et collez-le dans cette déclaration de classe runtime.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> Vos classes de modèle d’affichage&mdash;en fait, n’importe quelle classe runtime que vous déclarez dans votre application&mdash;ne doivent pas dériver une classe de base. La classe **BookSku** déclarée ci-dessus est un exemple. Il implémente une interface, mais elle ne dériver une classe de base.
>
> N’importe quelle classe runtime que vous déclarez dans l’application qui *est* dérive d’une base de classe est appelée une *modulable* classe. Et classes modulables de contrainte. D’une application pour les tests de [Certification Kit d’application](../debug-test-perf/windows-app-certification-kit.md) utilisés par Visual Studio et par Microsoft Store pour valider les envois (et par conséquent, pour l’application à être ingérées avec succès dans Microsoft Store), une classe modulable doit finalement dériver une classe de base Windows. Ce qui signifie que la classe à la très racine de la hiérarchie d’héritage doit être un type d’origine dans un espace de noms Windows.*. Si vous n’avez pas besoin de dériver une classe de runtime de la classe de base&mdash;par exemple, pour implémenter une classe **BindableBase** pour tous vos modèles d’affichage de dériver de&mdash;puis vous pouvez dériver de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).
>
> Un modèle d’affichage est une abstraction d’une vue, et si elle est liée directement à la vue (le code XAML). Un modèle de données est une abstraction de données, et il a consommés uniquement à partir de vos modèles d’affichage et ne pas directement liés à XAML. Par conséquent, vous pouvez déclarer vos modèles de données pas comme les classes d’exécution, mais comme classes ou les structures C++. Ils ne doivent être déclarées dans MIDL, et vous pouvez utiliser la hiérarchie d’héritage vous le souhaitez.

Enregistrez le fichier et générez le projet. Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer un fichier de métadonnées Windows Runtime (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) décrivant la classe runtime. Puis, l’outil `cppwinrt.exe` est exécuté pour générer les fichiers de code source et vous aider à créer et utiliser votre classe runtime. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BookSku** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` et `BookSku.cpp`.

Copiez les fichiers stub `BookSku.h` et `BookSku.cpp` à partir de `\Bookstore\Bookstore\Generated Files\sources\` dans le dossier de projet, à savoir `\Bookstore\Bookstore\`. Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

## <a name="implement-booksku"></a>Implémenter **BookSku**
Maintenant, nous allons ouvrir `\Bookstore\Bookstore\BookSku.h` et `BookSku.cpp`, et implémenter notre classe runtime. Dans `BookSku.h`, ajoutez un constructeur qui prend un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), un membre privé pour stocker la chaîne de titre, et un autre pour l’événement que nous allons générer lorsque le titre change. Une fois ces modifications, votre `BookSku.h` se présente comme suit.

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

Dans `BookSku.cpp`, implémentez les fonctions comme suit.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

Dans la fonction mutateur **titre** , nous vérifier si une valeur est définie qui est différente de la valeur actuelle. Et, dans ce cas, nous mettre à jour le titre et également déclencher l’événement [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) avec un argument correspondant au nom de la propriété qui a changé. De cette façon, l’interface utilisateur saura quelle valeur de propriété elle doit réinterroger.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Déclarer et implémenter **BookstoreViewModel**
Notre page XAML principale va établir une liaison à un modèle d’affichage principal. Et ce modèle d’affichage va avoir plusieurs propriétés, y compris une de type **BookSku**. Dans cette étape, nous allons déclarer et implémenter la classe runtime de notre modèle d’affichage principal.

Ajoutez un nouvel élément **Fichier Midl (.idl)** nommé `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

Enregistrez et lancez la génération. Copiez `BookstoreViewModel.h` et `BookstoreViewModel.cpp` à partir du dossier `Generated Files` dans le dossier de projet, et incluez-les dans le projet. Ouvrez les fichiers et implémenter la classe d’exécution, comme indiqué ci-dessous. Note comment, dans `BookstoreViewModel.h`, nous allons notamment `BookSku.h`, qui déclare le type de mise en œuvre (**winrt::Bookstore::implementation::BookSku**).

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel final : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();

        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> Le type de `m_bookSku` est le type projeté (**winrt::Bookstore::BookSku**), et le paramètre de modèle que vous utilisez avec **make** est le type d’implémentation (**winrt::Bookstore::implementation::BookSku**). Même dans ce cas, **make** renvoie une instance du type projeté.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Ajouter une propriété de type **BookstoreViewModel** à **MainPage**
Ouvrez `MainPage.idl`, qui déclare la classe runtime représentant notre page d’interface utilisateur principale. Ajoutez une instruction d’importation pour importer `BookstoreViewModel.idl`, et ajoutez une propriété en lecture seule nommée MainViewModel de type **BookstoreViewModel**. Supprimer également la propriété **MyProperty** . Notez également la `import` directive dans la liste ci-dessous.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Enregistrez le fichier. La génération du projet ne sont pas succès pour le moment, mais générez maintenant est particulièrement utile car elle régénère les fichiers de code source dans lequel la classe **MainPage** d’exécution est implémentée (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` et `MainPage.cpp`). Ainsi, continuez et construire maintenant. L’erreur de génération souhaité à ce stade est **'MainViewModel': n’est pas un membre de 'winrt::Bookstore::implementation::MainPage'**.

Si vous omettez l’inclusion de `BookstoreViewModel.idl` (voir la liste de `MainPage.idl` ci-dessus), puis vous verrez l’erreur **attendu \ < «MainViewModel» à proximité**. Une autre info-bulle est pour vous assurer que vous laissez tous les types dans le même espace de noms: l’espace de noms qui est indiqué dans les exemples de code.

Pour résoudre l’erreur susceptibles de se voir, vous devez maintenant copier le stub accesseur de la propriété **MainViewModel** hors les fichiers générés (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` et `MainPage.cpp`) et en `\Bookstore\Bookstore\MainPage.h` et `MainPage.cpp`.

Dans `\Bookstore\Bookstore\MainPage.h`, incluez `BookstoreViewModel.h`, qui déclare le type de mise en œuvre (**winrt::Bookstore::implementation::BookstoreViewModel**). Ajouter un membre privé pour stocker le modèle d’affichage. Notez que la fonction accesseur de la propriété (et le membre m_mainViewModel) est implémentée en termes de **Bookstore::BookstoreViewModel**, qui est le type projeté. Le type d’implémentation est dans le même projet (unité de compilation) en tant que l’application, de sorte que nous construire m_mainViewModel via la surcharge du constructeur qui prend `nullptr_t`. Supprimer également la propriété **MyProperty** .

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

Dans `\Bookstore\Bookstore\MainPage.cpp`, appelez [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (avec le type d’implémentation) pour affecter une nouvelle instance du type projetée à m_mainViewModel. Affectez une valeur initiale pour le titre du livre. Implémentez l’accesseur pour la propriété MainViewModel. Et, pour finir, mettez à jour le titre du livre dans le gestionnaire d’événements du bouton. Supprimer également la propriété **MyProperty** .

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Lier le bouton à la propriété **Title**
Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Comme indiqué dans la liste ci-dessous, supprimez le nom à partir du bouton et modifier la valeur de sa propriété **contenu** à partir d’un littéral à une expression de liaison. Notez la propriété `Mode=OneWay` sur l’expression de liaison (à sens unique, du modèle d’affichage vers l’interface utilisateur). Sans cette propriété, l’interface utilisateur ne répondra pas aux événements de modification de propriété.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Lancez à présent le processus de génération et exécutez le projet. Cliquez sur le bouton pour exécuter le gestionnaire d’événements **Click**. Ce gestionnaire appelle la fonction mutateur du titre du livre; ce mutateur déclenche un événement pour informer l’interface utilisateur que la propriété **Title** a été modifiée; et le bouton réinterroge la valeur de cette propriété pour mettre à jour sa propre valeur **Content**.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>À l’aide de l’extension de balisage {Binding} avec C + / WinRT
Pour la version actuellement de C + / WinRT, afin de pouvoir utiliser l’extension de balisage {Binding} vous devrez implémenter les interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) et [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) .

## <a name="important-apis"></a>API importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Rubriquesassociées
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Créer des API avec C++/WinRT](author-apis.md)
