---
author: stevewhims
description: Cette rubrique montre comment utiliser des API C++/WinRT, qu’elles soient implémentées par Windows, un fournisseur de composants tiers ou par vous-même.
title: Utiliser des API avec C++/WinRT
ms.author: stwhi
ms.date: 04/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projeté, projection, implémentation, classe runtime, activation
ms.localizationpriority: medium
ms.openlocfilehash: e777aca8f1d9f3892f67b10c785c056b14c8070c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832243"
---
# <a name="consume-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Utiliser des API avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

Cette rubrique montre comment utiliser des API C++/WinRT, qu’elles fassent partie de Windows ou qu’elles soient implémentées par un fournisseur de composants tiers ou par vous-même.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Si l’API se trouve dans un espace de noms Windows
Il s'agit du scénario le plus courant dans lequel vous utiliserez une API Windows Runtime. Voici un exemple de code simple.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
}
```

L’en-tête `winrt/Windows.Foundation.h` inclus fait partie du SDK, présent dans le dossier `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Les en-têtes dans ce dossier contiennent des API Windows projetées en C++/WinRT. Chaque fois que vous souhaitez utiliser un type à partir d’un espace de noms Windows, incluez l’en-tête de projection C++/WinRT correspondant à cet espace de noms. Les directives `using namespace` sont facultatives, mais pratiques.

Dans cet exemple, `winrt/Windows.Foundation.h` contient le type projeté pour la classe runtime [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Un *type projeté* est un wrapper sur une classe runtime, à des fins d’utilisation de ses API. Une *interface projetée* est un wrapper sur une interface Windows Runtime.

Dans l’exemple de code ci-dessus, après l’initialisation de C++/WinRT, nous construisons le type projeté **Uri** via l’un de ses constructeurs publiquement documentés ([**Uri(String)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_), dans cet exemple). Pour celui-ci, le cas le plus courant, c’est tout que vous avez à faire.

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Si l’API est implémentée dans un composant Windows Runtime
Cette section s’applique, que vous ayez créé le composant vous-même ou qu’il provienne d’un fournisseur.

> [!NOTE]
> Pour plus d’informations sur la disponibilité actuelle de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio pour C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

Dans votre projet d’application, référencez le fichier de métadonnées Windows Runtime (`.winmd`) du composant Windows Runtime et lancez le processus de génération. Lors de la génération, l’outil `cppwinrt.exe` génère une bibliothèque C++ standard qui décrit en détail &mdash;ou *projette*&mdash; la surface d’API pour le composant. En d’autres termes, la bibliothèque générée contient les types projetés pour le composant.

Ensuite, comme pour un type d’espace de noms Windows, incluez un en-tête et construisez le type projeté via l’un de ses constructeurs. Le code de démarrage de votre projet d’application inscrit la classe runtime, et le constructeur du type projeté appelle [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) pour activer la classe runtime à partir du composant référencé.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Pour obtenir plus d’informations, du code ainsi que la procédure à suivre pour utiliser des API implémentées dans un composant Windows Runtime, voir [Créer des événements en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>Si l’API est implémentée dans le projet d’utilisation
Un type qui est utilisé à partir de l’interface utilisateur XAML doit être une classe runtime, même s’il est dans le même projet que le code XAML.

Pour ce scénario, vous allez générer un type projeté à partir des métadonnées Windows Runtime de la classe runtime (`.winmd`). Là encore, vous allez inclure un en-tête, mais cette fois vous allez construire le type projeté via son constructeur `nullptr`. Ce constructeur n’effectuant aucune initialisation, vous devez ensuite affecter une valeur à l’instance via la fonction d’assistance [**winrt::make**](/uwp/cpp-ref-for-winrt/make), en transmettant tous les arguments constructeur nécessaires. Une classe runtime implémentée dans le même projet que le code d’utilisation n’a pas besoin d’être inscrite, ni instanciée via l’activation de Windows Runtime/COM.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

Pour obtenir plus d’informations, du code et la procédure d’utilisation d’une classe runtime implémentée dans le projet d’utilisation, voir [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Instanciation et retour des types et interfaces projetés
Voici un exemple de ce à quoi peuvent ressembler les types et interfaces projetés dans votre projet d’utilisation.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** est un type projeté; les interfaces projetées incluent **IMyRuntimeClass**, **IStringable** et **IClosable**. Cette rubrique vous a montré les différentes façons dont vous pouvez instancier un type projeté. Voici un résumé et un rappel utilisant **MyRuntimeClass** comme exemple.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- Vous pouvez accéder aux membres de toutes les interfaces d’un type projeté.
- Vous pouvez retourner un type projeté à un appelant.
- Les types et interfaces projetés dérivent de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Par conséquent, vous pouvez appeler [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) sur un type ou une interface projetés pour rechercher d’autres interfaces projetées, que vous pouvez également utiliser ou retourner à un appelant. La fonction membre **as** fonctionne comme [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521).

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="important-apis"></a>API importantes
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Rubriquesassociées
* [Créer des événements en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
