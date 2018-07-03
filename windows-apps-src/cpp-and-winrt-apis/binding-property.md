---
author: stevewhims
description: Une propriété qui peut être efficacement liée à un contrôle XAML est appelée «propriété *observable*». Cette rubrique montre comment implémenter et utiliser une propriété observable, et comment y lier un contrôle XAML.
title: Contrôles XAML; liaison à une propriété C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, liaison, propriété
ms.localizationpriority: medium
ms.openlocfilehash: 25ea4c4caf5135b13b88eeea6f43bb36bd691c11
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "1863215"
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
    runtimeclass BookSku : Windows.UI.Xaml.DependencyObject, Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!IMPORTANT]
> Pour qu’une application réussisse les tests [Kit de certification des applications Windows](../debug-test-perf/windows-app-certification-kit.md) et soit intégrée avec succès dans le MicrosoftStore, la classe de base optimale de chaque classe runtime *déclarée dans l’application* doit être un type provenant d'un espace de noms Windows.*.

Pour satisfaire cette exigence, dérivez vos classes de modèle d’affichage de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject). Vous pouvez également déclarer une classe de base pouvant être liée dérivée de **DependencyObject**, et en dériver vos modèles d’affichage. Vous pouvez déclarer vos modèles de données en tant que structures C++; il n’est pas utile de les déclarer dans un fichier MIDL (tant que vous les utilisez uniquement à partir de vos modèles d’affichage et que vous ne leur liez pas de code XAML directement, auquel cas il s’agirait indubitablement de modèles d’affichage par définition, de toute manière).

Enregistrez le fichier et générez le projet. Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer un fichier de métadonnées Windows Runtime (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) décrivant la classe runtime. Puis, l’outil `cppwinrt.exe` est exécuté pour générer les fichiers de code source et vous aider à créer et utiliser votre classe runtime. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BookSku** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` et `BookSku.cpp`.

Copiez les fichiers stub `BookSku.h` et `BookSku.cpp` à partir de `\Bookstore\Bookstore\Generated Files\sources\` dans le dossier de projet, à savoir `\Bookstore\Bookstore\`. Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**.

## <a name="implement-booksku"></a>Implémenter **BookSku**
Maintenant, nous allons ouvrir `\Bookstore\Bookstore\BookSku.h` et `BookSku.cpp`, et implémenter notre classe runtime. Dans `BookSku.h`, ajoutez un constructeur qui prend un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), un membre privé pour stocker la chaîne de titre, et un autre pour l’événement que nous allons générer lorsque le titre change. Une fois qu’ils auront été ajoutés, votre `BookSku.h` se présentera comme suit.

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(hstring const& title);

        hstring Title();
        void Title(hstring const& value);
        event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(event_token const& token);
    
    private:
        hstring title;
        event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> propertyChanged;
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
    BookSku::BookSku(hstring const& title)
    {
        Title(title);
    }

    hstring BookSku::Title()
    {
        return title;
    }

    void BookSku::Title(hstring const& value)
    {
        if (title != value)
        {
            title = value;
            propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(event_token const& token)
    {
        propertyChanged.remove(token);
    }
}
```

Dans la fonction mutateur **Title**, nous vérifions si une autre valeur est définie et, si tel est le cas, nous mettons à jour le titre et déclenchons également l’événement [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) avec un argument égal au nom de la propriété qui a changé. De cette façon, l’interface utilisateur saura quelle valeur de propriété elle doit réinterroger.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Déclarer et implémenter **BookstoreViewModel**
Notre page XAML principale va établir une liaison à un modèle d’affichage principal. Et ce modèle d’affichage va avoir plusieurs propriétés, y compris une de type **BookSku**. Dans cette étape, nous allons déclarer et implémenter la classe runtime de notre modèle d’affichage principal.

Ajoutez un nouvel élément **Fichier Midl (.idl)** nommé `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel : Windows.UI.Xaml.DependencyObject
    {
        BookSku BookSku{ get; };
    }
}
```

Enregistrez et lancez la génération. Copiez `BookstoreViewModel.h` et `BookstoreViewModel.cpp` à partir du dossier `Generated Files` dans le dossier de projet, et incluez-les dans le projet. Ouvrez ces fichiers et implémentez la classe runtime comme suit.

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
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
Ouvrez `MainPage.idl`, qui déclare la classe runtime représentant notre page d’interface utilisateur principale. Ajoutez une instruction d’importation pour importer `BookstoreViewModel.idl`, et ajoutez une propriété en lecture seule nommée MainViewModel de type **BookstoreViewModel**.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace BookstoreCPPWinRT
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Régénérez le projet pour régénérer les fichiers de code source dans lesquels la classe runtime **MainPage** est implémentée (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` et `MainPage.cpp`). Copiez les stubs accesseur pour la propriété ViewModel en dehors des fichiers générés et dans `\Bookstore\Bookstore\MainPage.h` et `MainPage.cpp`.

Pour `\Bookstore\Bookstore\MainPage.h`, ajoutez un membre privé pour stocker le modèle d’affichage. Notez que la fonction accesseur de la propriété (et le membre m_mainViewModel) est implémentée en termes de **Bookstore::BookstoreViewModel**, qui est le type projeté. Le type d’implémentation se trouvant dans le même projet (unité de compilation), nous construisons m_mainViewModel via la surcharge du constructeur qui prend `nullptr_t`.

```cppwinrt
// MainPage.h
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

    private:
        void ClickHandler(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args);

        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };

        friend class MainPageT<MainPage>;
    };
}
...
```

Dans `\Bookstore\Bookstore\MainPage.cpp`, incluez `BookstoreViewModel.h` qui déclare le type d’implémentation. Appelez [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (avec le type d’implémentation) pour affecter une nouvelle instance du type projeté à m_mainViewModel. Affectez une valeur initiale pour le titre du livre. Implémentez l’accesseur pour la propriété MainViewModel. Et, pour finir, mettez à jour le titre du livre dans le gestionnaire d’événements du bouton.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "BookstoreViewModel.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }

    void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Lier le bouton à la propriété **Title**
Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Supprimez le nom du bouton et modifiez la valeur de sa propriété **Content** d’une chaîne littérale à une expression de liaison. Notez la propriété `Mode=OneWay` sur l’expression de liaison (à sens unique, du modèle d’affichage vers l’interface utilisateur). Sans cette propriété, l’interface utilisateur ne répondra pas aux événements de modification de propriété.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Lancez à présent le processus de génération et exécutez le projet. Cliquez sur le bouton pour exécuter le gestionnaire d’événements **Click**. Ce gestionnaire appelle la fonction mutateur du titre du livre; ce mutateur déclenche un événement pour informer l’interface utilisateur que la propriété **Title** a été modifiée; et le bouton réinterroge la valeur de cette propriété pour mettre à jour sa propre valeur **Content**.

## <a name="important-apis"></a>API importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Rubriquesassociées
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Créer des API avec C++/WinRT](author-apis.md)
