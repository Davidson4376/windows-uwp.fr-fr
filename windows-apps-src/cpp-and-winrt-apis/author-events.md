---
author: stevewhims
description: Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements.
title: Créer des événements en C++/WinRT
ms.author: stwhi
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, créer, événement
ms.localizationpriority: medium
ms.openlocfilehash: 3b52bf8e33bbf111dd02c695d8c3baf77e1338ac
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2891091"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Créer des événements en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Cette rubrique montre comment créer un composant Windows Runtime qui contient une classe runtime représentant un compte bancaire, qui déclenche un événement lorsque son solde devient débiteur. Elle montre également une application de base qui utilise la classe runtime de compte bancaire, appelle une fonction pour ajuster le solde et gère tous les événements résultants.

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Créer un composant Windows Runtime (BankAccountWRC)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Composant Windows Runtime Visual C++ (C++/WinRT)** et nommez-le *BankAccountWRC* (pour «composant Windows Runtime de compte bancaire»).

Le projet nouvellement créé contient un fichier nommé `Class.idl`. Renommez ce fichier `BankAccount.idl` (renommer le `.idl` fichier renomme automatiquement le dépendant `.h` et `.cpp` fichiers trop). Remplacez le contenu de `BankAccount.idl` avec la liste ci-dessous.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

Enregistrez le fichier. La génération du projet ne sont pas succès pour le moment, mais générez maintenant est particulièrement utile car il génère les fichiers de code source dans lequel vous allez implémenter la classe runtime de **compte bancaire** . Ainsi, continuez et construire maintenant (les erreurs de build souhaité à ce stade faire avec `Class.h` et `Class.g.h` introuvable). Au cours du processus de génération, le `midl.exe` outil est exécutée pour créer le fichier de métadonnées de votre composant Windows Runtime (qui est `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Puis, l’outil `cppwinrt.exe` est exécuté (avec l’option `-component`) pour générer les fichiers de code source vous aidant à créer votre composant. Ces fichiers incluent stub pour vous aider à l’implémentation de la classe runtime **compte bancaire** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` et `BankAccount.cpp`.

Dans l’Explorateur de fichiers, copiez les fichiers stub `BankAccount.h` et `BankAccount.cpp` à partir du dossier `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` dans le dossier qui contient vos fichiers de projet, qui est `\BankAccountWRC\BankAccountWRC\`, remplacez les fichiers à la destination. Maintenant, nous allons ouvrir `BankAccount.h` et `BankAccount.cpp`, et implémenter notre classe runtime. Dans `BankAccount.h`, ajoutez deux membres privés à l’implémentation (*pas* à l’implémentation d’usine) de BankAccount.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        float m_balance{ 0.f };
    };
}
...
```

Comme vous pouvez le constater ci-dessus, l’événement est implémenté en termes de modèle struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event) , paramétré par un type particulier de délégué.

Dans `BankAccount.cpp`, implémentez les fonctions, comme illustré dans l’exemple de code ci-dessous. Dans C++/WinRT, un événement déclaré dans le fichier IDL est implémenté comme un ensemble de fonctions surchargées (de la même manière qu'une propriété est implémentée comme une paire de fonctions Get et Set surchargées). Une surcharge prend un délégué à enregistrer et retourne un jeton. L’autre prend un jeton et révoque l’inscription du délégué associé.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token)
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

Vous n’avez pas besoin d’implémenter la surcharge pour le révocateur d’événement (pour plus d’informations, voir [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate))&mdash;la projection C++/WinRT s'en charge à votre place. Les autres surcharges ne sont pas incorporées dans la projection pour vous donner la possibilité de les implémenter de manière optimale pour votre scénario. Un appel de [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) et de [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) de ce type constitue une valeur par défaut efficace et concurrency/thread-safe. Mais si vous avez un très grand nombre d’événements, vous pouvez ne pas souhaiter un champ d’événement pour chacun, mais plutôt opter pour un type d’implémentation fragmentée à la place.

Vous pouvez également voir ci-dessus que l’implémentation de la fonction **AdjustBalance** déclenche l’événement **AccountIsInDebit** si le solde devient négatif.

Si des avertissements vous empêchent de génération, puis les résoudre ou définir la propriété projet **C/C++** > **Général** > **Considérer les avertissements comme des erreurs** à **No (/ WX-)** et générez le projet à nouveau.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Créer une application de base (BankAccountCoreApp) pour tester le composant Windows Runtime

Créez à présent un nouveau projet (dans votre solution `BankAccountWRC` ou dans une nouvelle solution). Créez un projet **Application de base Visual C++ (C++/WinRT)** et nommez-le *BankAccountCoreApp*.

Ajoutez une référence et accédez à `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou ajouter une référence de projet au projet, si les deux projets sont dans la même solution). Cliquez sur **Ajouter**, puis sur **OK**. À présent, générez BankAccountCoreApp. Dans le cas improbable que vous voyez une erreur que le fichier de la charge utile `readme.txt` n’existe pas, exclure ce fichier à partir du projet de composant d’exécution Windows, reconstruire, puis reconstruire BankAccountCoreApp.

Pendant le processus de génération, l’outil `cppwinrt.exe` est exécuté pour traiter le fichier `.winmd` référencé dans les fichiers de code source contenant les types projetés afin de vous aider à utiliser votre composant. L’en-tête pour les types projetés des classes runtime de votre composant &mdash;nommé `BankAccountWRC.h`&mdash; est généré dans le dossier `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluez cet en-tête dans `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Également dans `App.cpp`, ajoutez le code suivant pour instancier un BankAccount (à l’aide du constructeur par défaut du type projeté), enregistrez un gestionnaire d’événements et faites en sorte que le compte devienne débiteur.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Chaque fois que vous cliquez sur la fenêtre, vous retirez 1 du solde du compte bancaire. Pour montrer que l’événement est déclenché comme prévu, placez un point d’arrêt à l’intérieur de l’expression lambda qui gère l’événement **AccountIsInDebit** , exécuter l’application et cliquez à l’intérieur de la fenêtre.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Les délégués paramétrées et signaux simples, entre un ABI

Si l’événement doit être accessible via une interface binaire d’application (ABI)&mdash;entre un composant et son application consommatrice par&mdash;puis l’événement doit utiliser un type de délégué Windows Runtime. L’exemple ci-dessus utilise le type de délégué [**Windows::Foundation::EventHandler\ < T\ >**](/uwp/api/windows.foundation.eventhandler) Windows Runtime. [**TypedEventHandler\ < TSender, TResult\ >**](/uwp/api/windows.foundation.eventhandler) est un autre exemple d’un type de délégué Windows Runtime.

Les paramètres de type pour les types de deux délégués ont qu’il coupe l’ABI, afin que les paramètres de type doivent être des types de Windows Runtime, trop. Qui inclut les classes d’exécution préalable et tiers, ainsi que des types primitifs tels que les nombres et les chaînes. Le compilateur vous aide à une erreur «*doit être de type WinRT*» si vous avez oublié de cette contrainte.

Si vous n’avez pas besoin de transmettre des paramètres ou des arguments avec l’événement, vous pouvez définir votre propre type de délégué Windows Runtime simple. L’exemple ci-dessous montre une version plus simple de la classe runtime de **compte bancaire** . Il déclare un type délégué nommé **SignalDelegate** , puis il l’utilise pour déclencher un événement de type de signal au lieu d’un événement avec un paramètre.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Les délégués paramétrées, signaux simples et rappels dans un projet

Si l’événement est utilisé uniquement en interne au sein de votre C + / WinRT project (pas dans des fichiers binaires), puis vous toujours utilisez le modèle de struct [**winrt::event**](/uwp/cpp-ref-for-winrt/event) , mais vous devez le paramétrer avec C + / Windows Runtime [**winrt::delegate de WinRT&lt;... T&gt; **](/uwp/cpp-ref-for-winrt/delegate) modèle struct, qui est un délégué efficace, contenant des références. Elle prend en charge un nombre quelconque de paramètres, et elles ne sont pas limités aux types de Windows Runtime.

L’exemple ci-dessous montre tout d’abord un délégué signature qui ne prend aucun paramètre (essentiellement un signal simple), puis celle qui accepte une chaîne.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

Notez la façon dont vous pouvez ajouter à l’événement des délégués abonnement autant que vous le souhaitez. Il existe toutefois une surcharge associée à un événement. Si vous avez seulement besoin est un rappel simple avec uniquement un seul délégué abonnement, vous pouvez ensuite utiliser [**winrt::delegate&lt;... T&gt; **](/uwp/cpp-ref-for-winrt/delegate) sur son propre.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si vous êtes portage de C + / CX codebase où les délégués et les événements sont utilisés en interne au sein d’un projet, puis **winrt::delegate** vous aidera à répliquer ce modèle dans C + / WinRT.

## <a name="design-guidelines"></a>Recommandations en matière de conception

Nous vous recommandons de passer pas délégués et les événements en tant que paramètres de la fonction. **Ajouter** une fonction de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) est la seule exception, car vous devez passer un délégué dans ce cas. La raison de cette règle est, car les délégués peuvent prendre les formes dans différentes langues Windows Runtime (en termes de si elles prennent en charge l’inscription d’un client ou plusieurs). Événements, avec leur modèle abonné multiples, constituent une option plus prévisible et cohérente.

La signature d’un délégué de gestionnaire d’événements doit être composé de deux paramètres: *l’expéditeur* (**IInspectable**) et *args* (certains argument type d’événement, par exemple [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Notez que ces instructions ne s’appliquent nécessairement si vous concevez une API interne. Bien que les API internes deviennent souvent public au fil du temps.

## <a name="related-topics"></a>Rubriquesassociées
* [Créer des API avec C++/WinRT](author-apis.md)
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
