---
author: stevewhims
description: Cette rubrique montre comment inscrire et révoquer des délégués de gestion d’événements à l’aide de C++/WinRT.
title: Gérer des événements en utilisant des délégués en C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projeté, projection, gérer, événement, délégué
ms.localizationpriority: medium
ms.openlocfilehash: 7af66c3f0586f2fb99a2a742f6da0144ed69d253
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4085204"
---
# <a name="handle-events-by-using-delegates-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Gérer des événements en utilisant des délégués en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Cette rubrique montre comment inscrire et révoquer des délégués de gestion d’événements à l’aide de C++/WinRT. Vous pouvez gérer un événement à l’aide de n’importe quel objet de type fonction C++ standard.

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="register-a-delegate-to-handle-an-event"></a>Inscrire un délégué pour gérer un événement
Un exemple simple consiste à gérer l’événement de clic d’un bouton. Il est courant d’utiliser un balisage XAML pour inscrire une fonction membre afin de gérer l’événement, comme suit.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
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

Il existe d’autres façons de construire un **RoutedEventHandler**. Vous trouverez ci-dessous le bloc de syntaxe extrait de la rubrique de documentation relative à [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (choisir *C++/WinRT* dans la liste déroulante **Langage** de la page). Notez les différents constructeurs: l’un d’entre eux prend une expression lambda; un autre une fonction gratuite, et un autre (celui que nous avons utilisé ci-dessus) prend un objet et un pointeur-vers-fonction-membre.

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

Si vous n’effectuez pas beaucoup de tâches dans votre gestionnaire d’événements, vous pouvez utiliser une fonction lambda au lieu d’une fonction membre. Là encore, l’exemple de code ci-dessous n’est peut-être pas très parlant, mais un délégué **RoutedEventHandler** est construit à partir d’une fonction lambda qui, à nouveau, doit correspondre à la syntaxe de l’opérateur d’appel de fonction.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const&, RoutedEventArgs const&)
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

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const&)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>Révoquer un délégué inscrit
Lorsque vous inscrivez un délégué, un jeton vous est généralement retourné. Vous pouvez par la suite utiliser ce jeton pour révoquer votre délégué; ce qui signifie que le délégué est désinscrit de l’événement et ne sera pas appelé si l’événement est de nouveau déclenché. Par souci de simplicité, aucun des exemples de code ci-dessus ne montre comment procéder. Toutefois, l’exemple de code suivant stocke le jeton dans le membre de données privées de la structure et révoque son gestionnaire dans le destructeur.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            ...
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

Au lieu d’une référence forte, comme dans l’exemple ci-dessus, vous pouvez stocker une référence faible sur le bouton (voir [Références faibles en C++/WinRT](weak-references.md)).

Lorsque vous inscrivez un délégué, vous pouvez également spécifier **winrt::auto_revoke** (qui est une valeur de type [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) pour demander un révocateur d’événement (de type [**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)). Le révocateur d’événement conserve une référence faible à la source d’événement (l’objet qui déclenche l’événement) pour vous. Vous pouvez révoquer manuellement en appelant la fonction membre **event_revoker::revoke**; mais le révocateur d'événement appelle cette fonction lui-même automatiquement lorsqu'il est hors de portée. La fonction **revoke** vérifie si la source d’événement existe toujours et, si tel est le cas, révoque votre délégué. Dans cet exemple, il n’est pas nécessaire de stocker la source d’événement ni d’avoir un destructeur.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const&, RoutedEventArgs const&)
        {
            ...
        });
    }

private:
    winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> m_event_revoker;
};
```

Vous trouverez ci-dessous le bloc de syntaxe extrait de la rubrique de documentation relative à l’événement [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Il montre les trois fonctions différentes d’inscription et de révocation. Vous pouvez voir exactement quel type de révocateur d’événement vous devez déclarer à partir de la surcharge tierce.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

Un modèle semblable s’applique à tous les événements C++/WinRT.

Vous pouvez envisager de révoquer les gestionnaires dans un scénario de navigation de page. Si vous naviguez à plusieurs reprises dans une page, puis revenez en arrière, vous pourriez alors révoquer tous les gestionnaires lorsque vous quittez la page. Autre possibilité, si vous réutilisez la même instance de page, vérifiez la valeur de votre jeton et ne faites l’inscription que si elle n’a pas encore été définie (`if (!m_token){ ... }`). Une troisième option consiste à stocker un révocateur d’événement dans la page en tant que membre de données. Et une quatrième option, comme décrit plus loin dans cette rubrique, consiste à capturer une référence forte ou faible à l’objet *this* dans votre fonction lambda.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Types délégués pour les actions et opérations asynchrones
Les exemples ci-dessus utilisent le type délégué **RoutedEventHandler**, mais il existe bien entendu beaucoup d’autres types de délégués. Par exemple, les actions et opérations asynchrones (avec et sans progression) sont terminées et/ou les événements de progression qui attendent les délégués du type correspondant. Par exemple, l’événement de progression d’une opération asynchrone avec progression (à savoir tout ce qui implémente [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)) requiert un délégué de type [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler). Voici un exemple de code de création d’un délégué de ce type à l’aide d’une fonction lambda. L’exemple montre également comment créer un délégué [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler).

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const&, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

Comme l’indique le commentaire sur la «coroutine» ci-dessus, au lieu d’utiliser un délégué avec les événements terminés des actions et des opérations asynchrones, vous trouverez sans doute plus naturel d’utiliser des coroutines. Pour plus d’informations et des exemples de code, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md).

Toutefois, si vous vous en tenez aux délégués, vous pouvez opter pour une syntaxe plus simple.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const)
{
    ....
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

## <a name="using-the-this-object-in-an-event-handler"></a>Utiliser l’objet *this* dans un gestionnaire d’événements
Si vous gérez un événement à partir d’une fonction lambda au sein de la fonction membre d’un objet, vous devez penser aux durées de vie relatives du destinataire d’événement (l’objet qui gère l’événement) et de la source d’événement (l’objet qui déclenche l’événement).

Dans de nombreux cas, un destinataire survit à toutes les dépendances sur son pointeur *this* depuis une fonction lambda donnée. Certains de ces cas sont évidents, par exemple, lorsqu’une page d’interface utilisateur gère un événement déclenché par un contrôle qui se trouve sur la page. Le bouton ne survit pas à la page, de ce fait le gestionnaire non plus. Cela est vrai chaque fois que le destinataire est le propriétaire de la source (comme membre de données, par exemple), ou chaque fois que le destinataire et la source sont frère/sœur et appartiennent directement à un autre objet. Si vous êtes certain que vous avez un cas où le gestionnaire ne survivra pas au *this* dont il dépend, vous pouvez capturer *this* normalement, sans considération pour la durée de vie forte ou faible.

Mais il existe cependant des cas où *this* ne survit pas à son utilisation dans un gestionnaire (y compris les gestionnaires d’événements de progression et d’achèvement déclenchés par des actions et opérations asynchrones).

- Si vous créez une coroutine pour implémenter une méthode asynchrone, alors cela est possible.
- Dans de rares cas avec certains objets d’infrastructure de l’interface utilisateur XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), par exemple), cela est possible, si le destinataire est finalisé sans désinscription de la source d’événement.

Dans ces cas, une violation d’accès résulte du code dans un gestionnaire ou dans une tentative de continuation d’une coroutine pour utiliser l’objet *this* non valide.

> [!IMPORTANT]
> Si vous rencontrez une de ces situations, vous devrez penser à la durée de vie de l’objet *this*; et si l’objet *this* capturé survit à la capture. Si ce n’est pas le cas, capturez-le avec une référence forte ou faible, selon le cas. Voir [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) et [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function).
> Ou &mdash;si cela a du sens pour votre scénario et si les considérations liées au thread le rendent même possible&mdash;, alors une autre option consiste à révoquer le gestionnaire une fois que le destinataire a terminé avec l’événement, ou dans le destructeur du destinataire.

Cet exemple de code utilise l’événement [**SwapChainPanel.CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) à titre d’illustration. Il inscrit un gestionnaire d’événements utilisant une expression lambda qui capture une référence faible pour le destinataire. Pour plus d’informations sur les références faibles, voir [Références faibles en C++/WinRT](weak-references.md). 

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weakReferenceToThis{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strongReferenceToThis = weakReferenceToThis.get())
        {
            strongReferenceToThis->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

Dans la clause de capture lamba, une variable temporaire est créée, qui représente une référence faible pour *this*. Dans le corps de l’expression lambda, si une référence forte à *this* peut être obtenue, alors la fonction **OnCompositionScaleChanged** est appelée. De cette façon, à l’intérieur de **OnCompositionScaleChanged**, *this* peut être utilisé en toute sécurité.

## <a name="important-apis"></a>API importantes
* [structure de marqueur WinRT::auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [Fonction winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Fonction winrt::implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>Rubriquesassociées
* [Créer des événements en C++/WinRT](author-events.md)
* [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md)
* [Références faibles en C++/WinRT](weak-references.md)
