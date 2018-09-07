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
ms.openlocfilehash: 1b0aa1a53ce69369c442cef7f3b5912631593242
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3660651"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Créer des événements en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Cette rubrique montre comment créer un composant Windows Runtime qui contient une classe runtime représentant un compte bancaire, qui déclenche un événement lorsque son solde devient débiteur. Elle montre également une application de base qui utilise la classe runtime de compte bancaire, appelle une fonction pour ajuster le solde et gère tous les événements résultants.

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Créer un composant Windows Runtime (BankAccountWRC)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créer un **Visual C++** > **Windows universel** > **composant Windows Runtime (C++ / WinRT)** de projet et nommez-le *BankAccountWRC* (pour «compte bancaire composant Windows Runtime»).

Le projet nouvellement créé contient un fichier nommé `Class.idl`. Renommez ce fichier `BankAccount.idl` (renommer le `.idl` fichier renomme automatiquement le dépendant `.h` et `.cpp` fichiers trop). Remplacez le contenu du `BankAccount.idl` avec le listing ci-dessous.

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

Enregistrez le fichier. La génération du projet ne réussite pour le moment, mais générez maintenant est particulièrement utile pour faire car elle génère des fichiers de code source dans lequel vous allez implémenter la classe runtime **BankAccount** . Par conséquent, lancez-vous et construire maintenant (les erreurs de génération vous devriez voir à ce stade ont à faire avec `Class.h` et `Class.g.h` n’a ne pas été trouvée). Pendant le processus de génération, le `midl.exe` outil est exécuté pour créer le fichier de métadonnées de votre composant Windows Runtime (qui est `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Puis, l’outil `cppwinrt.exe` est exécuté (avec l’option `-component`) pour générer les fichiers de code source vous aidant à créer votre composant. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BankAccount** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` et `BankAccount.cpp`.

Dans l’Explorateur de fichiers, copiez les fichiers stub `BankAccount.h` et `BankAccount.cpp` à partir du dossier `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` dans le dossier qui contient vos fichiers de projet, qui est `\BankAccountWRC\BankAccountWRC\`et remplacez les fichiers dans la destination. Maintenant, nous allons ouvrir `BankAccount.h` et `BankAccount.cpp`, et implémenter notre classe runtime. Dans `BankAccount.h`, ajoutez deux membres privés à l’implémentation (*pas* à l’implémentation d’usine) de BankAccount.

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

Comme vous pouvez le voir ci-dessus, l’événement est implémenté en termes du modèle de structure [**winrt::event**](/uwp/cpp-ref-for-winrt/event) paramétré par un type délégué particulier.

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

Si un avertissement vous empêche de construction, puis résolvez-les ou définir la propriété de projet **C/C++** > **Général** > **Treat Warnings As Errors** **No (/ WX-)** et générez de nouveau le projet.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Créer une application de base (BankAccountCoreApp) pour tester le composant Windows Runtime

Créez à présent un nouveau projet (dans votre solution `BankAccountWRC` ou dans une nouvelle solution). Créer un **Visual C++** > **Windows universel** > **Core App (C++ / WinRT)** de projet et nommez-le *BankAccountCoreApp*.

Ajoutez une référence, puis accédez à `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou ajouter une référence de projet à projet, si les deux projets se trouvent dans la même solution). Cliquez sur **Ajouter**, puis sur **OK**. À présent, générez BankAccountCoreApp. Dans l’éventualité peu probable que vous voyez une erreur qui le fichier de charge utile `readme.txt` n’existe pas, excluez ce fichier à partir du projet de composant Windows Runtime, régénérez-le, puis regénérez BankAccountCoreApp.

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

Chaque fois que vous cliquez sur la fenêtre, vous retirez 1 du solde du compte bancaire. Pour démontrer que l’événement est déclenché comme prévu, insérez un point d’arrêt à l’intérieur de l’expression lambda qui gère l’événement **AccountIsInDebit** , exécutez l’application et cliquez à l’intérieur de la fenêtre.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Les délégués paramétrés et signaux simples, entre un ABI

Si votre événement doit être accessible via une interface binaire d’application (ABI)&mdash;par exemple, entre un composant et son utilisation application&mdash;ensuite votre événement doit utiliser un type de délégué Windows Runtime. L’exemple ci-dessus utilise le type de délégué [**Windows::Foundation::EventHandler\ < T\ >**](/uwp/api/windows.foundation.eventhandler) Windows Runtime. [**TypedEventHandler\ < TSender, TResult\ >**](/uwp/api/windows.foundation.eventhandler) est un autre exemple d’un type de délégué Windows Runtime.

Les paramètres de type de ces types de deux délégués avoir franchisse ABI, de sorte que les paramètres de type doivent être des types Windows Runtime, trop. Qui inclut les classes runtime interne et tierce, ainsi que des types primitifs tels que les nombres et les chaînes. Le compilateur vous aide à une erreur «*doit être WinRT type*» si vous oubliez cette contrainte.

Si vous n’avez pas besoin de transmettre des paramètres ou des arguments avec votre événement, vous pouvez définir votre propre type de délégué Windows Runtime simple. L’exemple ci-dessous montre une version simplifiée de la classe runtime de **BankAccount** . Il déclare un type délégué nommé **SignalDelegate** , puis il qui utilise pour déclencher un événement de type de signal au lieu d’un événement avec un paramètre.

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Les délégués paramétrés, signaux simples et rappels au sein d’un projet

Si votre événement est utilisé uniquement en interne au sein de votre C++ / WinRT du projet (pas sur les fichiers binaires), puis vous utilisez toujours le modèle de structure [**winrt::event**](/uwp/cpp-ref-for-winrt/event) , mais vous devez le paramétrer avec C++ / WinRT non Windows Runtime [**winrt::delegate&lt;… T&gt; **](/uwp/cpp-ref-for-winrt/delegate) modèle de structure, qui est un délégué efficace, avec décompte des références. Il prend en charge n’importe quel nombre de paramètres, et ils ne sont pas limités aux types Windows Runtime.

L’exemple ci-dessous montre tout d’abord un délégué signature qui ne prend aucun paramètre (essentiellement un signal simple) et puis qui prend une chaîne.

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

Notez la façon dont vous pouvez ajouter à l’événement autant de délégués abonnement comme vous le souhaitez. Il existe toutefois une certaine surcharge associée à un événement. Si vous avez uniquement besoin est un rappel simple avec uniquement un délégué abonnement unique, vous pouvez ensuite utiliser [**winrt::delegate&lt;… T&gt; **](/uwp/cpp-ref-for-winrt/delegate) sur son propre.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si vous effectuez un portage à partir de C++ / CX codebase où les événements et les délégués sont utilisés en interne au sein d’un projet, puis **winrt::delegate** vous aidera à répliquer ce modèle en C++ / WinRT.

## <a name="design-guidelines"></a>Recommandations en matière de conception

Nous recommandons que vous transmettez des événements et délégués pas, en tant que paramètres de fonction. **Ajouter** une fonction de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) est la seule exception, car vous devez passer un délégué dans ce cas. La raison de cette directive est étant donné que les délégués peuvent prendre différentes formes entre les différentes langues de Windows Runtime (en termes de si elles prennent en charge l’inscription du un client ou plusieurs). Événements, avec leur modèle abonné à plusieurs, constituent une option beaucoup plus prévisible et cohérente.

La signature d’un délégué de gestionnaire d’événements doit être composé de deux paramètres: *expéditeur* (**IInspectable**) et les *arguments* (certains événements type d’argument, par exemple [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Notez que ces instructions ne s’appliquent nécessairement si vous créez une API interne. Bien que les API internes deviennent souvent publique au fil du temps.

## <a name="related-topics"></a>Rubriquesassociées
* [Créer des API avec C++/WinRT](author-apis.md)
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
