---
author: stevewhims
description: Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements.
title: Créer des événements en C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, créer, événement
ms.localizationpriority: medium
ms.openlocfilehash: 192000f937989d7218931ce1465bd96d5d9b71c6
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1880880"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Créer des événements en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Cette rubrique montre comment créer un composant Windows Runtime qui contient une classe runtime représentant un compte bancaire, qui déclenche un événement lorsque son solde devient débiteur. Elle montre également une application de base qui utilise la classe runtime de compte bancaire, appelle une fonction pour ajuster le solde et gère tous les événements résultants.

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="windowsfoundationeventhandlerlttgt-and-typedeventhandlerlttgt"></a>Windows::Foundation::EventHandler&lt;T&gt; et TypedEventHandler&lt;T&gt;
Si vous souhaitez déclencher un événement à partir d’une classe runtime implémentée dans un composant Windows Runtime, vous devez utiliser [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) ou [**TypedEventHandler**](/uwp/api/windows.foundation.eventhandler) pour le type délégué de votre événement. Les paramètres de type doivent être de types Windows Runtime, de sorte que les types primitifs sont autorisés, ainsi que les classes runtime tierces.

Le compilateur vous aidera en affichant une erreur «*must be WinRT type*» (doit être de type WinRT) si vous oubliez cette contrainte.

## <a name="winrtdelegatelt-tgt"></a>winrt::delegate&lt;...T&gt;
Si vous souhaitez déclencher un événement à partir d’un type C++ (créé et utilisé au sein du même projet), vous pouvez utiliser [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) C++/WinRT pour le type délégué de votre événement. Dans ce cas, les paramètres de type du délégué n’ont pas besoin d’être des types Windows Runtime. Si vous effectuez un portage à partir d'un code base C++/CX où les événements et les délégués sont utilisés en interne (pas sur les fichiers binaires), **winrt::delegate** vous aidera à répliquer ce modèle en C++/WinRT.

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Créer un composant Windows Runtime (BankAccountWRC)
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet **Composant Windows Runtime Visual C++ (C++/WinRT)** et nommez-le *BankAccountWRC* (pour «composant Windows Runtime de compte bancaire»).

Le projet nouvellement créé contient un fichier nommé `Class.idl`. Renommez ce fichier `BankAccountWRC.idl` afin que, lors de la génération, le fichier de métadonnées Windows Runtime de votre composant soit nommé pour le composant lui-même. Dans `BankAccountWRC.idl`, définissez votre interface comme dans le listing ci-dessous.

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

Enregistrez le fichier et générez le projet. La génération ne va pas réussir tout de suite. Toutefois, pendant le processus de génération, l’outil `midl.exe` est exécuté pour créer le fichier de métadonnées Windows Runtime de votre composant (à savoir `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Puis, l’outil `cppwinrt.exe` est exécuté (avec l’option `-component`) pour générer les fichiers de code source vous aidant à créer votre composant. Ces fichiers incluent des stubs pour vous aider à implémenter la classe runtime `BankAccount` que vous avez déclarée dans votre fichier IDL. Ces stubs sont `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` et `BankAccount.cpp`.

Copiez les fichiers stub `BankAccount.h` et `BankAccount.cpp` à partir de `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` dans le dossier de projet, à savoir `\BankAccountWRC\BankAccountWRC\`. Dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit sur les fichiers stub que vous avez copiés, puis cliquez sur **Inclure dans le projet**. Cliquez également avec le bouton droit sur `Class.h` et `Class.cpp`, puis cliquez sur **Exclude From Project**.

Maintenant, nous allons ouvrir `BankAccount.h` et `BankAccount.cpp`, et implémenter notre classe runtime. Dans `BankAccount.h`, ajoutez deux membres privés à l’implémentation (*pas* à l’implémentation d’usine) de BankAccount.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> accountIsInDebitEvent;
        float balance{ 0.f };
    };
}
...
```

Dans `BankAccount.cpp`, implémentez les fonctions, comme illustré dans l’exemple de code ci-dessous. Dans C++/WinRT, un événement déclaré dans le fichier IDL est implémenté comme un ensemble de fonctions surchargées (de la même manière qu'une propriété est implémentée comme une paire de fonctions Get et Set surchargées). Une surcharge prend un délégué à enregistrer et retourne un jeton. L’autre prend un jeton et révoque l’inscription du délégué associé.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(event_token const& token)
    {
        accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        balance += value;
        if (balance < 0.f) accountIsInDebitEvent(*this, balance);
    }
}
```

Vous n’avez pas besoin d’implémenter la surcharge pour le révocateur d’événement (pour plus d’informations, voir [Révoquer un délégué inscrit](handle-events.md#revoke-a-registered-delegate))&mdash;la projection C++/WinRT s'en charge à votre place. Les autres surcharges ne sont pas incorporées dans la projection pour vous donner la possibilité de les implémenter de manière optimale pour votre scénario. Un appel de [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) et de [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) de ce type constitue une valeur par défaut efficace et concurrency/thread-safe. Mais si vous avez un très grand nombre d’événements, vous pouvez ne pas souhaiter un champ d’événement pour chacun, mais plutôt opter pour un type d’implémentation fragmentée à la place.

Vous pouvez également voir ci-dessus que l’implémentation de la fonction **AdjustBalance** déclenche l’événement **AccountIsInDebit** si le solde devient négatif.

Si un avertissement vous empêche de procéder à la génération, définissez la propriété de projet **C/C++** > **Général** > **Considérer les avertissements comme des erreurs** sur **Non (/WX-)** et générez de nouveau le project.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Créer une application de base (BankAccountCoreApp) pour tester le composant Windows Runtime
Créez à présent un nouveau projet (dans votre solution `BankAccountWRC` ou dans une nouvelle solution). Créez un projet **Application de base Visual C++ (C++/WinRT)** et nommez-le *BankAccountCoreApp*.

Ajoutez une référence, puis accédez à `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (ou ajoutez une référence de projet, si les deux projets se trouvent dans la même solution). Cliquez sur **Ajouter**, puis sur **OK**. À présent, générez BankAccountCoreApp. Si vous voyez une erreur indiquant que le fichier de charge utile `readme.txt` n’existe pas, excluez ce fichier du projet Composant Windows Runtime, régénérez-le, puis regénérez BankAccountCoreApp.

Pendant le processus de génération, l’outil `cppwinrt.exe` est exécuté pour traiter le fichier `.winmd` référencé dans les fichiers de code source contenant les types projetés afin de vous aider à utiliser votre composant. L’en-tête pour les types projetés des classes runtime de votre composant &mdash;nommé `BankAccountWRC.h`&mdash; est généré dans le dossier `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluez cet en-tête dans `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

Également dans `App.cpp`, ajoutez le code suivant pour instancier un BankAccount (à l’aide du constructeur par défaut du type projeté), enregistrez un gestionnaire d’événements et faites en sorte que le compte devienne débiteur.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    event_token eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        eventToken = bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            assert(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        bankAccount.AccountIsInDebit(eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Chaque fois que vous cliquez sur la fenêtre, vous retirez 1 du solde du compte bancaire. Pour démontrer que l’événement est déclenché comme prévu, insérez un point d’arrêt à l’intérieur de l’expression lambda, exécutez l’application, puis cliquez à l’intérieur de la fenêtre.

## <a name="related-topics"></a>Rubriquesassociées
* [Créer des API avec C++/WinRT](author-apis.md)
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Gérer des événements en utilisant des délégués en C++/WinRT](handle-events.md)
