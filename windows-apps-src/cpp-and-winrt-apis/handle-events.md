---
description: Cette rubrique montre comment inscrire et révoquer des délégués de gestion d’événements à l’aide de C++/WinRT.
title: Gérer des événements en utilisant des délégués en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projeté, projection, gérer, événement, délégué
ms.localizationpriority: medium
ms.openlocfilehash: 00870a196517f975d2736298513be7567f3dd29e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64745054"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>Gérer des événements en utilisant des délégués en C++/WinRT

Cette rubrique montre comment inscrire et révoquer des délégués de gestion d’événements à l’aide de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Vous pouvez gérer un événement à l’aide de n’importe quel objet de type fonction C++ standard.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="register-a-delegate-to-handle-an-event"></a>Inscrire un délégué pour gérer un événement

Un exemple simple consiste à gérer l’événement de clic d’un bouton. Il est courant d’utiliser un balisage XAML pour inscrire une fonction membre afin de gérer l’événement, comme suit.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

Au lieu de le faire de manière déclarative dans le balisage, vous pouvez inscrire de manière impérative une fonction membre pour gérer un événement. Cela n’est peut-être pas évident dans l’exemple de code ci-dessous, mais l’argument de l’appel [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) est une instance du délégué [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). Dans ce cas, nous utilisons la surcharge de constructeur **RoutedEventHandler** qui prend un objet et un pointeur-vers-fonction-membre.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> Lorsque vous inscrivez le délégué, l’exemple de code ci-dessus passe un pointeur *ceci* brut (pointant vers l’objet actif). Pour savoir comment établir une référence forte ou faible à l’objet actif, consultez la sous-section **Si vous utilisez une fonction membre en tant que délégué** dans la section [Accès sécurisé au pointeur *this* avec un délégué de gestion des événements](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

Il existe d’autres façons de construire un **RoutedEventHandler**. Vous trouverez ci-dessous le bloc de syntaxe extrait de la rubrique de documentation relative à [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (choisir *C++/WinRT* dans la liste déroulante **Langage** en haut à droite de la page web). Notez les différents constructeurs : l’un d’entre eux prend une expression lambda ; un autre une fonction gratuite, et un autre (celui que nous avons utilisé ci-dessus) prend un objet et un pointeur-vers-fonction-membre.

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

La syntaxe de l’opérateur d’appel de fonction est également intéressante. Elle vous indique ce que doivent être les paramètres de votre délégué. Comme vous pouvez le constater, dans ce cas, la syntaxe de l’opérateur d’appel de fonction correspond aux paramètres de notre **MainPage::ClickHandler**.

> [!NOTE]
> Pour un événement donné, afin de déterminer les détails de son délégué et les paramètres de ce délégué, consultez d’abord la rubrique de documentation pour l’événement lui-même. Prenons [l’événement UIElement.KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown) comme exemple. Consultez cette rubrique, puis choisissez *C++/WinRT* dans la liste déroulante **Language**. Dans le bloc de syntaxe au début de la rubrique, vous verrez ceci.
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> Ces informations nous indiquent que l’événement **UIElement.KeyDown** (la rubrique active) a un type de délégué **KeyEventHandler**, puisque c’est le type que vous transmettez lorsque vous inscrivez un délégué avec ce type d’événement. Donc, maintenant, suivez le lien dans la rubrique vers le type [Délégué KeyEventHandler](/uwp/api/windows.ui.xaml.input.keyeventhandler). Ici, le bloc de syntaxe contient un opérateur d’appel de fonction. Et, comme mentionné ci-dessus, elle vous indique ce que doivent être les paramètres de votre délégué.
> 
> ```cppwinrt
> void operator()(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
> ```
>
>  Comme vous pouvez le voir, le délégué doit être déclaré avec un **IInspectable** comme expéditeur et une instance de la [classe KeyRoutedEventArgs](/uwp/api/windows.ui.xaml.input.keyroutedeventargs) comme arguments.
>
> Pour prendre un autre exemple, examinons [l’événement Popup.Closed](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed). Son type de délégué est [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler). Par conséquent, votre délégué prendra **IInspectable** comme expéditeur et un autre **IInspectable** (car c’est le paramètre type de **EventHandler**) comme arguments.

Si vous n’effectuez pas beaucoup de tâches dans votre gestionnaire d’événements, vous pouvez utiliser une fonction lambda au lieu d’une fonction membre. Là encore, l’exemple de code ci-dessous n’est peut-être pas très parlant, mais un délégué **RoutedEventHandler** est construit à partir d’une fonction lambda qui, à nouveau, doit correspondre à la syntaxe de l’opérateur d’appel de fonction dont nous avons discuté plus haut.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        Button().Content(box_value(L"Clicked"));
    });
}
```

Vous pouvez choisir d’être un peu plus explicite lorsque vous construisez votre délégué. Par exemple, si vous souhaitez le transmettre ou l’utiliser plusieurs fois.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>Révoquer un délégué inscrit

Lorsque vous inscrivez un délégué, un jeton vous est généralement retourné. Vous pouvez par la suite utiliser ce jeton pour révoquer votre délégué ; ce qui signifie que le délégué est désinscrit de l’événement et ne sera pas appelé si l’événement est de nouveau déclenché. Par souci de simplicité, aucun des exemples de code ci-dessus ne montre comment procéder. Toutefois, l’exemple de code suivant stocke le jeton dans le membre de données privées de la structure et révoque son gestionnaire dans le destructeur.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            // ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

Au lieu d’une référence forte, comme dans l’exemple ci-dessus, vous pouvez stocker une référence faible sur le bouton (voir [Références fortes et faibles en C++/WinRT](weak-references.md)).

Autre possibilité, lorsque vous inscrivez un délégué, vous pouvez spécifier **winrt::auto_revoke** (qui est une valeur de type [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) pour demander un révocateur d’événement (de type [**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)). Le révocateur d'événement contient pour vous une référence faible à la source de l’événement (l’objet qui déclenche l’événement). Vous pouvez révoquer manuellement en appelant la fonction membre **event_revoker::revoke** ; mais le révocateur d'événement appelle cette fonction lui-même automatiquement lorsqu'il est hors de portée. La fonction **revoke** vérifie si la source d’événement existe toujours et, si tel est le cas, révoque votre délégué. Dans cet exemple, il n’est pas nécessaire de stocker la source d’événement ni d’avoir un destructeur.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

Vous trouverez ci-dessous le bloc de syntaxe extrait de la rubrique de documentation relative à l’événement [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Il montre les trois fonctions différentes d’inscription et de révocation. Vous pouvez voir exactement quel type de révocateur d’événement vous devez déclarer à partir de la surcharge tierce.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> Dans l’exemple de code ci-dessus, `Button::Click_revoker` est un alias de type pour `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. Un modèle semblable s’applique à tous les événements C++/WinRT. Chaque événement Windows Runtime a une surcharge de fonction revoke qui retourne un revoker d’événement, et le type de ce revoker est un membre de la source d’événements. Pour prendre un autre exemple, l’événement [**CoreWindow::SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) a une surcharge de fonction d’inscription qui retourne une valeur de type **CoreWindow::SizeChanged_revoker**.


Vous pouvez envisager de révoquer les gestionnaires dans un scénario de navigation de page. Si vous naviguez à plusieurs reprises dans une page, puis revenez en arrière, vous pourriez alors révoquer tous les gestionnaires lorsque vous quittez la page. Autre possibilité, si vous réutilisez la même instance de page, vérifiez la valeur de votre jeton et ne faites l’inscription que si elle n’a pas encore été définie (`if (!m_token){ ... }`). Une troisième option consiste à stocker un révocateur d’événement dans la page en tant que membre de données. Et une quatrième option, comme décrit plus loin dans cette rubrique, consiste à capturer une référence forte ou faible à l’objet *this* dans votre fonction lambda.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Types délégués pour les actions et opérations asynchrones

Les exemples ci-dessus utilisent le type délégué **RoutedEventHandler**, mais il existe bien entendu beaucoup d’autres types de délégués. Par exemple, les actions et opérations asynchrones (avec et sans progression) sont terminées et/ou les événements de progression qui attendent les délégués du type correspondant. Par exemple, l’événement de progression d’une opération asynchrone avec progression (à savoir tout ce qui implémente [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)) requiert un délégué de type [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler). Voici un exemple de code de création d’un délégué de ce type à l’aide d’une fonction lambda. L’exemple montre également comment créer un délégué [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler).

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& /* sender */, RetrievalProgress const& args)
        {
            uint32_t bytes_retrieved = args.BytesRetrieved;
            // use bytes_retrieved;
        });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const /* asyncStatus */)
        {
            SyndicationFeed syndicationFeed = sender.GetResults();
            // use syndicationFeed;
        });

    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

Comme l’indique le commentaire sur la « coroutine » ci-dessus, au lieu d’utiliser un délégué avec les événements terminés des actions et des opérations asynchrones, vous trouverez sans doute plus naturel d’utiliser des coroutines. Pour plus d’informations et des exemples de code, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md).

> [!NOTE]
> Il n’est pas correct d’implémenter plusieurs *gestionnaires d’achèvement* pour une action ou opération asynchrone. Vous pouvez soit avoir un délégué unique pour son événement terminé, soit le `co_await`. Si vous avez les deux, le deuxième échoue.

Si vous utilisez les délégués au lieu d’une coroutine, vous pouvez opter pour une syntaxe plus simple.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>Types de délégués qui renvoient une valeur

Certains types de délégués doivent retourner une valeur. Par exemple, [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler), qui retourne une chaîne. Voici un exemple de création d’un délégué de ce type (notez que la fonction lambda retourne une valeur).

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Accès sécurisé au pointeur *this* avec un délégué de gestion des événements.

Si vous gérez un événement avec la fonction membre d’un objet, ou à partir d’une fonction lambda au sein de la fonction membre d’un objet, vous devez penser aux durées de vie relatives du destinataire d’événement (l’objet qui gère l’événement) et de la source d’événement (l’objet qui déclenche l’événement). Pour plus d’informations et d’exemples de code, consultez [Références fortes et faibles en C++/WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>API importantes
* [winrt::auto_revoke_t marker struct](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [Fonction winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [Fonction winrt::implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>Rubriques connexes
* [Créer des événements en C++/WinRT](author-events.md)
* [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md)
* [Références fortes et faibles en C++/WinRT](weak-references.md)
