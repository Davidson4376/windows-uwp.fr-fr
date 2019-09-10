---
description: Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements.
title: Créer des événements en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, créer, événement
ms.localizationpriority: medium
ms.openlocfilehash: e8bb86bd8d52ff96f010bf41758f1e4602330d52
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393480"
---
# <a name="author-events-in-cwinrt"></a>Créer des événements en C++/WinRT

Cette rubrique montre comment créer un composant Windows Runtime qui contient une classe runtime représentant un compte bancaire, qui déclenche un événement lorsque son solde devient débiteur. Elle montre également une application de base qui utilise la classe runtime de compte bancaire, appelle une fonction pour ajuster le solde et gère tous les événements résultants.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Créer un composant Windows Runtime (BankAccountWRC)

Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Composant Windows Runtime (C++/WinRT)** et nommez-le *BankAccountWRC* (pour « composant Windows Runtime de compte bancaire »). Ne générez pas encore le projet.

Le projet nouvellement créé contient un fichier nommé `Class.idl`. Renommez ce fichier `BankAccount.idl` (la modification du nom du fichier `.idl` renomme également automatiquement les fichiers `.h` et `.cpp` associés). Remplacez le contenu de l’élément `BankAccount.idl` par le listing ci-dessous.

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

Enregistrez le fichier. Le projet ne sera pas généré complètement pour le moment, mais cette étape est utile car elle régénère les fichiers de code source dans lesquels vous allez implémenter la classe runtime **BankAccount**. Continuons et générons le projet maintenant (les erreurs de génération auxquelles vous pouvez vous attendre à ce stade sont liées à des éléments `Class.h` et `Class.g.h` introuvables).

Pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer le fichier de métadonnées Windows Runtime de votre composant (à savoir `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Puis, l’outil `cppwinrt.exe` est exécuté (avec l’option `-component`) pour générer les fichiers de code source vous aidant à créer votre composant. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime **BankAccount** que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` et `BankAccount.cpp`.

Cliquez avec le bouton droit de la souris sur le nœud du projet, puis cliquez sur **Ouvrir le dossier dans l'Explorateur de fichiers**. Le dossier du projet s’ouvre dans l'Explorateur de fichiers. De là, copiez les fichiers stub `BankAccount.h` et `BankAccount.cpp` du dossier `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` vers le dossier contenant vos fichiers projet, c’est-à-dire `\BankAccountWRC\BankAccountWRC\`, puis remplacez les fichiers dans la destination. Maintenant, nous allons ouvrir `BankAccount.h` et `BankAccount.cpp`, et implémenter notre classe runtime. Dans `BankAccount.h`, ajoutez deux membres privés à l’implémentation (*pas* à l’implémentation d’usine) de BankAccount.

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

Comme vous pouvez le voir ci-dessus, l'événement est implémenté en fonction du modèle de structure [**winrt::event**](/uwp/cpp-ref-for-winrt/event), paramétré par un type de délégué particulier.

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

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
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

> [!NOTE]
> Pour savoir ce qu’est un revoker d’événement automatique, consultez [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate). L’implémentation d’un revoker d’événement automatique est gratuite pour votre événement. En d’autres termes, vous n’avez pas besoin d’implémenter la surcharge pour le revoker d’événement puisque l’implémentation vous est fournie par la projection C++/WinRT.

Les autres surcharges (inscription et révocation manuelle) *ne sont pas* intégrées à la projection. Vous pouvez donc les implémenter de manière optimale pour votre scénario. Le fait d’appeler [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) et [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) comme indiqué dans ces implémentations constitue une valeur par défaut efficace et concurrency/thread-safe. Mais si vous avez un très grand nombre d’événements, vous pouvez ne pas souhaiter un champ d’événement pour chacun, mais plutôt opter pour un type d’implémentation fragmentée à la place.

Vous pouvez également voir ci-dessus que l’implémentation de la fonction **AdjustBalance** déclenche l’événement **AccountIsInDebit** si le solde devient négatif.

Si un avertissement vous empêche de procéder à la génération, corrigez les erreurs ou définissez la propriété de projet **C/C++**  > **Général** > **Considérer les avertissements comme des erreurs** sur **Non (/WX-)** et générez de nouveau le projet.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Créer une application de base (BankAccountCoreApp) pour tester le composant Windows Runtime

Créez à présent un nouveau projet (dans votre solution `BankAccountWRC` ou dans une nouvelle solution). Créez un projet **Application de base (C++/WinRT)** et nommez-le *BankAccountCoreApp*.

Ajoutez une référence, puis accédez à `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou ajoutez une référence de projet à projet, si les deux projets se trouvent dans la même solution). Cliquez sur **Ajouter**, puis sur **OK**. À présent, générez BankAccountCoreApp. Dans le cas improbable où vous voyez une erreur indiquant que le fichier de charge utile `readme.txt` n’existe pas, excluez ce fichier du projet Composant Windows Runtime, régénérez-le, puis regénérez BankAccountCoreApp.

Pendant le processus de génération, l’outil `cppwinrt.exe` est exécuté pour traiter le fichier `.winmd` référencé dans les fichiers de code source contenant les types projetés afin de vous aider à utiliser votre composant. L’en-tête pour les types projetés des classes runtime de votre composant &mdash; nommé `BankAccountWRC.h` &mdash; est généré dans le dossier `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluez cet en-tête dans `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Également dans `App.cpp`, ajoutez le code suivant pour instancier un BankAccount (à l’aide du constructeur par défaut du type projeté), enregistrez un gestionnaire d’événements et faites en sorte que le compte devienne débiteur.

`WINRT_ASSERT` est une définition de macro qui s’étend à [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

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
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
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

Chaque fois que vous cliquez sur la fenêtre, vous retirez 1 du solde du compte bancaire. Pour démontrer que l’événement est déclenché comme prévu, insérez un point d’arrêt à l’intérieur de l’expression lambda qui gère l’événement **AccountIsInDebit**, exécutez l’application, puis cliquez à l’intérieur de la fenêtre.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Délégués paramétrables et signaux simples dans une ABI

Si votre événement doit être accessible via une interface binaire d'application (ABI)&mdash;par exemple entre un composant et son application consommatrice&mdash;votre événement doit utiliser un type de délégué Windows Runtime. L’exemple ci-dessus utilise le type de délégué Windows Runtime [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler). [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) est un autre exemple de type de délégué Windows Runtime.

Comme les paramètres de type pour ces deux types de délégués doivent traverser l'ABI, ils doivent donc aussi être de type Windows Runtime. Cela inclut les classes runtime de premier et de troisième niveaux, ainsi que les types primitifs tels que les nombres et les chaînes de caractères. Le compilateur vous aide en affichant une erreur « *must be WinRT type* » (doit être de type WinRT) si vous oubliez cette contrainte.

Si vous n'avez pas besoin de passer des paramètres ou des arguments avec votre événement, vous pouvez définir votre propre type de délégué Windows Runtime simple. L’exemple ci-dessous montre une version plus simple de la classe runtime **BankAccount**. Il déclare un type de délégué nommé **SignalDelegate** puis l'utilise pour déclencher un événement de type signal au lieu d'un événement avec un paramètre.

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Délégués paramétrés, signaux simples et rappels au sein d'un projet

Si votre événement n'est utilisé qu'en interne dans votre projet C++/WinRT (et non dans des binaires), vous utilisez toujours le modèle de structure [**winrt::event**](/uwp/cpp-ref-for-winrt/event), mais vous le paramétrez avec le modèle [**winrt::delegate&lt; C++/WinRT non Windows Runtime... Le modèle de structure T&gt;** ](/uwp/cpp-ref-for-winrt/delegate) est un délégué efficace, avec décompte des références. Il prend en charge un certain nombre de paramètres et ne se limite pas aux types Windows Runtime.

L'exemple ci-dessous montre d'abord une signature de délégué qui ne prend aucun paramètre (essentiellement un simple signal), puis une signature qui prend une chaîne.

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

Notez comment vous pouvez ajouter à l'événement autant de délégués que vous le souhaitez. Mais un événement implique une surcharge de travail. Si vous n'avez besoin que d'un simple rappel avec un seul délégué abonné, alors vous pouvez utiliser [**winrt::delegate&lt;... T&gt;** ](/uwp/cpp-ref-for-winrt/delegate) seul.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si vous effectuez un portage à partir d'un code base C++/CX où les événements et les délégués sont utilisés en interne au sein d’un projet, **winrt::delegate** vous aidera à répliquer ce modèle en C++/WinRT.

## <a name="design-guidelines"></a>Recommandations en matière de conception

Nous vous recommandons de passer les événements, et non les délégués, comme paramètres de fonction. La fonction **add** de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) est la seule exception car vous devez passer un délégué dans ce cas. Cette directive s'explique par le fait que les délégués peuvent prendre différentes formes dans différents langages Windows Runtime (s’ils prennent en charge une seule ou plusieurs inscriptions de clients). Les événements, avec leur modèle à abonnés multiples, constituent une option beaucoup plus prévisible et cohérente.

La signature d'un délégué de gestionnaire d'événement doit être composée de deux paramètres : *sender* (**IInspectable**) et *args* (un type d'argument d'événement, par exemple [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Notez que ces instructions ne s’appliquent pas obligatoirement si vous concevez une API interne. Bien que les API internes finissent souvent par devenir publiques.

## <a name="related-topics"></a>Rubriques connexes
* [Créer des API avec C++/WinRT](author-apis.md)
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
