---
author: stevewhims
description: Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements.
title: Créer des événements en C++/WinRT
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, créer, événement
ms.localizationpriority: medium
ms.openlocfilehash: b7574f1a3406dae665ced80294f7bc1cf91aeb8c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832023"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Créer des événements en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

Cette rubrique montre comment créer un composant Windows Runtime qui contient une classe runtime représentant un compte bancaire, qui déclenche un événement lorsque son solde devient débiteur. Elle montre également une application de base qui utilise la classe runtime de compte bancaire, appelle une fonction pour ajuster le solde et gère tous les événements résultants.

> [!NOTE]
> Pour plus d’informations sur la disponibilité actuelle de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="windowsfoundationeventhandlerlttgt-and-typedeventhandlerlttgt"></a>Windows::Foundation::EventHandler&lt;T&gt; et TypedEventHandler&lt;T&gt;
Si vous souhaitez déclencher un événement à partir d’une classe runtime implémentée dans un composant Windows Runtime, vous devez utiliser [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) ou [**TypedEventHandler**](/uwp/api/windows.foundation.eventhandler) pour le type délégué de votre événement. Les paramètres de type doivent être de types Windows Runtime, de sorte que les types primitifs sont autorisés, ainsi que les classes runtime tierces.

Le compilateur vous aidera en affichant une erreur «*must be WinRT type*» (doit être de type WinRT) si vous oubliez cette contrainte.

## <a name="winrtdelegatelttgt"></a>winrt::delegate&lt;...T&gt;
Si vous souhaitez déclencher un événement à partir d’un type C++ (créé et utilisé au sein du même projet), vous pouvez utiliser **winrt::delegate&lt;...T&gt;** C++/WinRT pour le type délégué de votre événement. Dans ce cas, le paramètre de type n’a pas besoin d’être de type Windows Runtime.

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
        event<Windows::Foundation::EventHandler<float>> accountIsInDebitEvent;
        float balance{ 0.f };
    };
}
...
```

Dans `BankAccount.cpp`, implémentez les fonctions comme suit.

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

L’implémentation de la fonction **AdjustBalance** déclenche l’événement **AccountIsInDebit** si le solde devient négatif.

Si un avertissement vous empêche de procéder à la génération, définissez la propriété de projet **C/C++** > **Général** > **Considérer les avertissements comme des erreurs** sur **Non (/WX-)** et générez de nouveau le project.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Créer une application de base (BankAccountCoreApp) pour tester le composant Windows Runtime
Créez à présent un nouveau projet (dans votre solution `BankAccountWRC` ou dans une nouvelle solution). Créez un projet **Application de base Visual C++ (C++/WinRT)** et nommez-le *BankAccountCoreApp*.

Ajoutez une référence, puis accédez à `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`. Cliquez sur **Ajouter**, puis sur **OK**. À présent, générez BankAccountCoreApp. Si vous voyez une erreur indiquant que le fichier de charge utile `readme.txt` n’existe pas, excluez ce fichier du projet Composant Windows Runtime, régénérez-le, puis regénérez BankAccountCoreApp.

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
