---
description: Cette rubrique montre comment créer des API C++/WinRT à l’aide du struct de base **winrt::implements**, directement ou indirectement.
title: Créer des API avec C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projeté, projection, implémentation, implémenter, classe runtime, activation
ms.localizationpriority: medium
ms.openlocfilehash: 18dc65198d476204cfd54bd241fbd3c9ac401155
ms.sourcegitcommit: 7ece8a9a9fa75e2e92aac4ac31602237e8b7fde5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485174"
---
# <a name="author-apis-with-cwinrt"></a>Créer des API avec C++/WinRT

Cette rubrique montre comment créer des API [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) à l’aide du struct de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), directement ou indirectement. Dans ce contexte, *créer* est synonyme de *produire* ou *implémenter*. Cette rubrique couvre les scénarios suivants pour l’implémentation d’API sur un type C++/WinRT, dans cet ordre.

> [!NOTE]
> Cette rubrique aborde le sujet des composants Windows Runtime, mais seulement dans le contexte de C++/WinRT. Si vous recherchez du contenu sur les composants Windows Runtime qui couvre tous les langages de Windows Runtime, consultez [Composants Windows Runtime](/windows/uwp/winrt-components/).

- Vous ne créez *pas* une classe Windows Runtime (classe runtime) ; vous souhaitez simplement implémenter une ou plusieurs interfaces Windows Runtime pour une utilisation locale au sein de votre application. Dans ce cas, vous dérivez directement à partir de **winrt::implements**, et implémentez des fonctions.
- Vous créez *effectivement* une classe runtime. Vous pourriez créer un composant qui sera utilisé à partir d’une application. Ou vous pourriez créer un type qui sera utilisé à partir de l’interface utilisateur XAML et, dans ce cas, vous implémentez et utilisez une classe runtime au sein de la même unité de compilation. Dans ces cas, laissez les outils générer pour vous des classes qui dérivent à partir de **winrt::implements**.

Dans les deux cas, le type qui implémente vos API C++/WinRT APIs est appelé *type d’implémentation*.

> [!IMPORTANT]
> Il est important de distinguer le concept de type d’implémentation de celui de type projeté. Le type projeté est décrit dans [Utiliser des API avec C++/WinRT](consume-apis.md).

## <a name="if-youre-not-authoring-a-runtime-class"></a>Si vous ne créez *pas* de classe runtime

Le scénario le plus simple est celui où vous implémentez une interface Windows Runtime pour une utilisation locale. Vous n’avez pas besoin d’une classe runtime, simplement d’une classe C++ ordinaire. Par exemple, vous pouvez écrire une application basée autour de [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication).

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension Visual Studio (VSIX) C++/WinRT et le package NuGet (qui fournissent ensemble le modèle de projet et la prise en charge de la build), voir [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Dans Visual Studio, le modèle de projet**Visual C++**  > **Windows Universal** > **Core App (C++/WinRT)** illustre le modèle **CoreApplication**. Le modèle commence par transmettre une implémentation de [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) à [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** utilise l’interface pour créer la première vue de l’application. D’un point de vue conceptuel, **IFrameworkViewSource** se présente comme suit.

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

Toujours d’un point de vue conceptuel, l’implémentation de **CoreApplication::Run** effectue ceci.

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

De ce fait, en tant que développeur, implémentez l’interface **IFrameworkViewSource**. C++/WinRT possède le modèle de structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) pour faciliter l’implémentation d’une (ou plusieurs) interface(s), sans avoir recours à la programmation de style COM. Vous dérivez simplement votre type de **implements**, puis implémentez les fonctions de l’interface. Voici comment procéder.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

Voilà pour ce qui est de **IFrameworkViewSource**. L’étape suivante consiste à retourner un objet qui implémente l’interface **IFrameworkView**. Vous pouvez choisir d’implémenter cette interface également sur **App**. L’exemple de code suivant représente une application minimale qui affichera au moins une fenêtre active sur le bureau.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

Dans la mesure où votre type **App***est un* **IFrameworkViewSource**, vous pouvez simplement en transmettre un à **Run**.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Si vous créez une classe runtime dans un composant Windows Runtime

Si votre type est empaqueté dans un composant Windows Runtime pour une utilisation à partir d’une application, il doit alors s’agir d’une classe runtime. Vous déclarez une classe runtime dans un fichier Microsoft Interface Definition Language (IDL) (.idl) (consultez [Factorisation des classes runtime dans des fichiers Midl (.idl)](#factoring-runtime-classes-into-midl-files-idl)).

Chaque fichier IDL génère un fichier `.winmd`, puis Visual Studio fusionne tous ces fichiers dans un seul fichier avec le même nom que votre espace de noms racine. Ce dernier fichier `.winmd` sera celui auquel les consommateurs de votre composant feront référence.

Voici un exemple de déclaration d’une classe runtime dans un fichier IDL.

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

Ce fichier IDL déclare une classe Windows Runtime (runtime). Une classe runtime est un type qui peut être activé et utilisé via des interfaces COM modernes, généralement au-delà des limites exécutables. Lorsque vous ajoutez un fichier IDL à votre projet et build, la chaîne d’outils C++/WinRT (`midl.exe` et `cppwinrt.exe`) génère un type d’implémentation pour vous. Pour obtenir un exemple du workflow de fichier IDL en action, consultez [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md).

Dans l’exemple de fichier IDL ci-dessus, le type d’implémentation est un stub de structure C++ nommé **winrt::MyProject::implementation::MyRuntimeClass** dans les fichiers de code source intitulés `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` et `MyRuntimeClass.cpp`.

Le type d’implémentation ressemble à ceci.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Notez le modèle de polymorphisme F-Bound utilisé (**MyRuntimeClass** s’utilise lui-même comme un argument de modèle pour sa base **MyRuntimeClassT**). On parle aussi de motif de modèle curieusement récursif (Curiously Recurring Template Pattern ou CRTP). Si vous remontez la chaîne héritée, vous trouverez **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

Par conséquent, dans ce scénario, à la racine de la hiérarchie héritée se trouve de nouveau le modèle de structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

Pour obtenir plus d’informations, du code ainsi que la procédure à suivre pour créer des API dans un composant Windows Runtime, voir [Créer des événements en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Si vous créez une classe runtime qui sera référencée dans votre interface utilisateur XAML

Si votre type est référencé par votre interface utilisateur XAML, alors il doit s’agir d’une classe runtime, même si elle se trouve dans le même projet que le code XAML. Bien qu’elle soit généralement activée au-delà des limites exécutables, une classe runtime peut être utilisée au sein de l’unité de compilation qui l’implémente.

Dans ce scénario, vous créez *et* utilisez les API. La procédure d’implémentation de votre classe runtime est essentiellement identique à celle d’un composant Windows Runtime. De ce fait, reportez-vous à la section précédente&mdash;[Si vous créez une classe runtime dans un composant Windows Runtime](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component). La seule différence est que, à partir du fichier IDL, la chaîne d’outils C++/WinRT génère non seulement un type d’implémentation, mais également un type projeté. Il est important d’être conscient que « **MyRuntimeClass** » dans ce scénario peut être ambigu ; il existe plusieurs entités de types différents portant ce nom.

- **MyRuntimeClass** est le nom d’une classe runtime. Mais il s’agit en réalité d’une abstraction : déclarée dans IDL et implémentée dans un langage de programmation.
- **MyRuntimeClass** est le nom de la structure C++ **winrt::MyProject::implementation::MyRuntimeClass**, qui est l’implémentation C++/WinRT de la classe runtime. Comme nous l’avons vu, s’il existe des projets d’implémentation et d’utilisation distincts, alors cette structure existe uniquement dans le projet d’implémentation. Il s’agit du *type d’implémentation* ou de l’*implémentation*. Ce type est généré (par l’outil `cppwinrt.exe`) dans les fichiers `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` et `MyRuntimeClass.cpp`.
- **MyRuntimeClass** est le nom du type projeté dans la forme de la structure C++ **winrt::MyProject::MyRuntimeClass**. S’il existe des projets d’implémentation et d’utilisation distincts, alors cette structure existe uniquement dans le projet d’utilisation. Il s’agit du *type projeté* ou de la *projection*. Ce type est généré (par `cppwinrt.exe`) dans le fichier `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h`.

![Type projeté et type d’implémentation](images/myruntimeclass.png)

Voici les parties du type projeté qui sont pertinentes pour cette rubrique.

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

Pour obtenir un exemple de procédure d’implémentation de l’interface **INotifyPropertyChanged** sur une classe runtime, voir [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md).

La procédure d’utilisation de votre classe runtime dans ce scénario est décrite dans [Utiliser des API avec C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).

## <a name="factoring-runtime-classes-into-midl-files-idl"></a>Factorisation des classes runtime dans des fichiers Midl (.idl)

Les modèles d’élément et de projet Visual Studio génèrent un fichier IDL distinct pour chaque classe runtime. Ainsi, vous bénéficiez d’une correspondance logique entre un fichier IDL et ses fichiers de code source générés.

Toutefois, si vous regroupez toutes les classes runtime de votre projet dans un seul fichier IDL, vous pouvez considérablement réduire le temps de génération. Si vous avez des dépendances `import` complexes (ou circulaires), le regroupement peut alors être nécessaire, le cas échéant. De plus, le regroupement peut faciliter la création et la révision de vos classes runtime.

## <a name="runtime-class-constructors"></a>Constructeurs de classes runtime

Voici quelques points à retenir des listings que nous avons vus ci-dessus.

- Chaque constructeur que vous déclarez dans votre fichier IDL entraîne la génération d’un constructeur à la fois sur votre type d’implémentation et sur votre type projeté. Les constructeurs déclarés dans le fichier IDL servent à utiliser la classe runtime à partir d’une unité de compilation *différente*.
- Que vous ayez ou non déclaré un ou plusieurs constructeurs dans le fichier IDL, une surcharge de constructeur qui prend **std::nullptr_t** est générée sur votre type projeté. Appeler le constructeur **std::nullptr_t** est *la première des deux étapes* nécessaires pour utiliser la classe runtime à partir de *la même unité* de compilation. Pour obtenir plus d’informations et un exemple de code, voir [Utiliser des API avec C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Si vous utilisez la classe runtime à partir de *la même* unité de compilation, vous pouvez également implémenter des constructeurs autres que ceux par défaut, directement sur le type d’implémentation (qui, n’oubliez pas, se trouve dans `MyRuntimeClass.h`).

> [!NOTE]
> Si vous pensez que votre classe runtime sera utilisée à partir d’une unité de compilation différente (qui est commune), incluez le ou les constructeur(s) dans votre fichier IDL (au moins un constructeur par défaut). De cette manière, vous obtiendrez également une implémentation d’usine avec votre type d’implémentation.
> 
> Si vous souhaitez créer et utiliser votre classe runtime uniquement au sein de la même unité de compilation, ne déclarez aucun constructeur dans votre fichier IDL. Vous n’avez pas besoin d’une implémentation d’usine, et aucune ne sera générée. Le constructeur par défaut de votre type d’implémentation sera supprimé, mais vous pouvez facilement le modifier et utiliser la valeur par défaut à la place.
> 
> Si vous souhaitez créer et utiliser votre classe runtime uniquement au sein de la même unité de compilation et que vous avez besoin de paramètres de constructeur, créez le ou les constructeur(s) dont vous avez besoin directement sur votre type d’implémentation.

## <a name="runtime-class-methods-properties-and-events"></a>Événements, propriétés et méthodes de classe runtime

Nous avons vu que le workflow consiste à utiliser le fichier IDL pour déclarer votre classe runtime et ses membres, puis que les outils génèrent des prototypes et des implémentations de stub pour vous. Comme pour ces prototypes générés automatiquement pour les membres de votre classe runtime, vous *pouvez* les modifier afin qu’ils fassent passer différents types à partir des types que vous déclarez dans votre fichier IDL. Vous pouvez cependant le faire que tant que le type que vous déclarez dans le fichier IDL peut être transféré vers le type que vous déclarez dans la version implémentée.

Voici quelques exemples.

- Vous pouvez assouplir les contraintes sur les types des paramètres. Par exemple, si dans le fichier IDL votre méthode prend une **SomeClass**, vous pouvez choisir de changer cela en **IInspectable** dans votre implémentation. Ceci fonctionne, car une **SomeClass** peut être transférée à **IInspectable** (bien sûr, l’inverse ne fonctionnerait pas).
- Vous pouvez accepter un paramètre copiable par valeur, à la place d’un paramètre copiable par référence. Par exemple, changez `SomeClass const&` en `SomeClass const&`. Ceci est nécessaire quand vous devez éviter de capturer une référence dans une coroutine (consultez [Passage de paramètres](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)).
- Vous pouvez assouplir les contraintes sur la valeur de retour. Par exemple, vous pouvez changer **void** en [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget).

Les deux derniers sont très pratiques quand vous écrivez un gestionnaire d’événements asynchrones.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>Instanciation et retour des types d’implémentation et interfaces

Dans cette section, nous allons prendre comme exemple un type d’implémentation nommé **MyType**, qui implémente les interfaces [**IStringable**](/uwp/api/windows.foundation.istringable) et [**IClosable**](/uwp/api/windows.foundation.iclosable).

Vous pouvez dériver **MyType** directement à partir de [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) (ce n’est pas une classe runtime).

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

Vous pouvez également le générer à partir du fichier IDL (c’est une classe runtime).

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

Pour passer de **MyType** à un objet **IStringable** ou **IClosable** que vous pouvez utiliser ou retourner dans le cadre de votre projection, vous devez appeler le modèle de fonction [**winrt::make**](/uwp/cpp-ref-for-winrt/make). **make** renvoie l’interface par défaut du type d’implémentation.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> Toutefois, si vous référencez votre type à partir de votre interface utilisateur XAML, un type d’implémentation et un type projeté se retrouveront dans le même projet. Dans ce cas, **make** renvoie une instance du type projeté. Pour obtenir un exemple de code de ce scénario, voir [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

Nous ne pouvons utiliser que `istringable` (dans l’exemple de code ci-dessus) pour appeler les membres de l’interface **IStringable**. Mais une interface C++/WinRT (qui est une interface projetée) dérive de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Par conséquent, vous pouvez appeler dessus [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) pour rechercher d’autres types ou interfaces projets que vous pouvez également utiliser ou retourner.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Si vous avez besoin d’accéder à tous les membres de l’implémentation, puis renvoyer ultérieurement une interface à un appelant, utilisez le modèle de fonction [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** retourne un [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) qui encapsule le type d’implémentation. Vous pouvez accéder aux membres de toutes ses interfaces (à l’aide de l’opérateur flèche), vous pouvez le renvoyer à un appelant en l’état, ou vous pouvez appeler **as** sur lui et renvoyer l’objet d’interface résultant à un appelant.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

La classe **MyType** ne fait pas partie de la projection ; il s’agit de l’implémentation. Mais de cette façon vous pouvez appeler ses méthodes d’implémentation directement, sans la surcharge d’un appel de fonction virtuelle. Dans l’exemple ci-dessus, même si **MyType::ToString** utilise la même signature que la méthode projetée sur **IStringable**, nous appelons la méthode non virtuelle directement, sans traverser la limite de l’interface binaire d’application (ABI). **com_ptr** pointant simplement un curseur sur la structure **MyType**, vous pouvez également accéder à tous les autres détails internes de **MyType** via la variable `myimpl` et l’opérateur flèche.

Dans le cas où vous disposez d’un objet d’interface et que vous savez qu’il s’agit d’une interface sur votre implémentation, vous pouvez revenir à l’implémentation à l’aide du modèle de fonction [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self). Là encore, il s’agit d’une technique qui évite les appels de fonctions virtuelles et vous permet d’accéder directement à l’implémentation.

> [!NOTE]
> Si vous n’avez pas installé le Kit de développement logiciel (SDK) Windows version 10.0.17763.0 (Windows 10, version 1809) ou version ultérieure, vous devez appeler [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) au lieu de [**winrt::get_ Self**](/uwp/cpp-ref-for-winrt/get-self).

Voici un exemple : Il existe un autre exemple dans [Implémenter la classe de contrôle personnalisé **BgLabelControl**](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class).

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Toutefois, seul l’objet d’interface d’origine conserve une référence. Si *vous* souhaitez la conserver, vous pouvez appeler [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function).

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

Le type d’implémentation lui-même ne dérivant pas de **winrt::Windows::Foundation::IUnknown**, il n’a pas de fonction **as**. Vous pouvez quand même en instancier une et accéder aux membres de toutes ses interfaces. Mais, si vous procédez de la sorte, ne retournez pas l’instance du type d’implémentation brute à l’appelant. Au lieu de cela, utilisez une des techniques présentées ci-dessus et retournez une interface projetée ou un **com_ptr**.

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

Si vous disposez d’une instance de votre type d’implémentation et que vous devez la transmettre à une fonction qui attend le type projeté correspondant, vous pouvez procéder ainsi. Un opérateur de conversion existant sur votre type d’implémentation (à condition que le type d’implémentation ait été généré par l’outil `cppwinrt.exe`) rend cela possible. Vous pouvez passer une valeur de type d’implémentation directement à une méthode qui attend une valeur du type projeté correspondant. À partir d’une fonction de membre de type d’implémentation, vous pouvez transmettre `*this` à une méthode qui attend une valeur du type projeté correspondant.

```cppwinrt
// MyProject::MyType is the projected type; the implementation type would be MyProject::implementation::MyType.

void MyOtherType::DoWork(MyProject::MyType const&){ ... }

...

void FreeFunction(MyProject::MyOtherType const& ot)
{
    MyType myimpl;
    ot.DoWork(myimpl);
}

...

void MyType::MemberFunction(MyProject::MyOtherType const& ot)
{
    ot.DoWork(*this);
}
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Dérivation à partir d’un type qui possède un constructeur autre que par défaut

[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) est un exemple de constructeur autre que par défaut. Comme il n’existe aucun constructeur par défaut, pour construire un **ToggleButtonAutomationPeer**, vous devez transmettre un *owner*. Par conséquent, si vous dérivez de **ToggleButtonAutomationPeer**, vous devez fournir un constructeur qui prend un *owner* et le transmet à la base. Voyons à quoi cela ressemble dans la pratique.

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

Le constructeur généré pour votre type d’implémentation ressemble à ceci.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

Tout ce qui manque, c’est de transmettre ce paramètre de constructeur à la classe de base. Vous vous souvenez du modèle de polymorphisme F-Bound que nous avons mentionné ci-dessus ? Une fois que vous êtes familiarisé avec les détails de ce modèle tel qu’utilisé en C++/WinRT, vous pouvez déterminer le nom de votre classe de base (ou vous pouvez simplement consulter le fichier d’en-tête de votre classe d’implémentation). Voici comment appeler le constructeur de classe de base dans ce cas.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

Le constructeur de classe de base attend un **ToggleButton**. Et **MySpecializedToggleButton** *est un* **ToggleButton**.

Tant que vous n’aurez pas apporté les modifications décrites ci-dessus (pour transmettre ce paramètre de constructeur à la classe de base), le compilateur marquera votre constructeur et signalera qu’aucun constructeur par défaut approprié n’est disponible sur un type nommé (dans ce cas) **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** . C’est en fait la classe de base de la classe de base de votre type d’implémentation.

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>Espaces de noms : types projetés, types d’implémentation et fabriques

Comme vous l’avez vu précédemment dans cette rubrique, une classe runtime C++/WinRT existe sous la forme de plusieurs classes C++ dans plusieurs espaces de noms. Ainsi, le nom **MyRuntimeClass** a une signification dans l’espace de noms **winrt::MyProject** et une signification différente dans l’espace de noms **winrt::MyProject::implementation**. Notez bien quel espace de noms vous avez actuellement dans le contexte, puis utilisez des préfixes d’espace de noms si vous avez besoin d’un nom provenant d’un autre espace de noms. Examinons plus en détail les espaces de noms en question.

- **winrt::MyProject**. Cet espace de noms contient des types projetés. Un objet d’un type projeté est un proxy ; il s’agit en fait d’un pointeur intelligent vers un objet de référence, où cet objet peut être implémenté ici dans votre projet ou dans une autre unité de compilation.
- **winrt::MyProject::implementation**. Cet espace de noms contient des types d’implémentation. Un objet d’un type d’implémentation n’est pas un pointeur, c’est une valeur : un objet de la pile C++ complet. Ne construisez pas directement un type d’implémentation ; au lieu de cela, appelez [ **winrt::make**](/uwp/cpp-ref-for-winrt/make), en passant votre type d’implémentation comme paramètre de modèle. Nous avons montré des exemples de **winrt::make** en action précédemment dans cette rubrique, et il en existe un autre exemple dans [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage). Consultez également [Diagnostic des allocations directes](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).
- **winrt::MyProject::factory_implementation**. Cet espace de noms contient des fabriques. Un objet de cet espace de noms prend en charge [**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory).

Ce tableau montre la qualification d’espace de noms minimale à utiliser dans différents contextes.

|Espace de noms qui est dans le contexte|Pour spécifier le type projeté|Pour spécifier le type projeté|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> Quand vous voulez retourner un type projeté depuis votre implémentation, veillez à ne pas instancier le type d’implémentation en écrivant `MyRuntimeClass myRuntimeClass;`. Les techniques et le code corrects pour ce scénario sont présentés plus haut dans cette rubrique, dans la section [Instanciation et retour des types d’implémentation et interfaces](#instantiating-and-returning-implementation-types-and-interfaces).
>
> Le problème avec `MyRuntimeClass myRuntimeClass;` dans ce scénario est qu’elle crée un objet **winrt::MyProject::implementation::MyRuntimeClass** sur la pile. Cet objet (de type d’implémentation) se comporte comme le type projeté à certains égards: vous pouvez appeler des méthodes sur celui-ci de la même façon et il se convertit même en un type projeté. L’objet se détruit cependant, conformément aux règles C++ normales, quand l’étendue quitte. Ainsi, si vous avez retourné un type projeté (un pointeur intelligent) à cet objet, ce pointeur est maintenant ambigu.
>
> Ce type de bogue de corruption de mémoire est difficile à diagnostiquer. Pour les versions Debug, une assertion C++/WinRT permet donc d’intercepter cette erreur à l’aide d’un détecteur de pile. Les coroutines sont cependant allouées sur le tas : vous n’obtenez donc pas d’aide sur cette erreur si vous la faites à l’intérieur d’une coroutine. Pour plus d'informations, consultez [Diagnostic des allocations directes](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>Utilisation des types projetés et des types d’implémentation avec différentes fonctionnalités C++/WinRT

Voici les différents endroits où une fonctionnalité C++/WinRT attend un type et quel sorte de type elle attend (type projeté, type d’implémentation ou les deux).

|Fonctionnalité|Accepte|Remarques|
|-|-|-|
|`T` (représentant un pointeur intelligent)|Projeté|Consultez l’avertissement dans [Espaces de noms : types projetés, types d’implémentation et fabriques](#namespaces-projected-types-implementation-types-and-factories) sur l’utilisation du type d’implémentation par erreur.|
|`agile_ref<T>`|Les deux|Si vous utilisez le type d’implémentation, l’argument du constructeur doit être `com_ptr<T>`.|
|`com_ptr<T>`|Implémentation|L’utilisation du type projeté génère l’erreur : `'Release' is not a member of 'T'`.|
|`default_interface<T>`|Les deux|Si vous utilisez le type d’implémentation, la première interface implémentée est retournée.|
|`get_self<T>`|Implémentation|L’utilisation du type projeté génère l’erreur : `'_abi_TrustLevel': is not a member of 'T'`.|
|`guid_of<T>()`|Les deux|Retourne le GUID de l’interface par défaut.|
|`IWinRTTemplateInterface<T>`<br>|Projeté|L’utilisation du type d’implémentation se compile, mais c’est une erreur : consultez l’avertissement dans [Espaces de noms : types projetés, types d’implémentation et fabriques](#namespaces-projected-types-implementation-types-and-factories).|
|`make<T>`|Implémentation|L’utilisation du type projeté génère l’erreur : `'implements_type': is not a member of any direct or indirect base class of 'T'`|
| `make_agile(T const&amp;)`|Les deux|Si vous utilisez le type d’implémentation, l’argument doit être `com_ptr<T>`.|
| `make_self<T>`|Implémentation|L’utilisation du type projeté génère l’erreur : `'Release': is not a member of any direct or indirect base class of 'T'`|
| `name_of<T>`|Projeté|Si vous utilisez le type d’implémentation, vous obtenez le GUID de l’interface par défaut sous forme de chaîne.|
| `weak_ref<T>`|Les deux|Si vous utilisez le type d’implémentation, l’argument du constructeur doit être `com_ptr<T>`.|

## <a name="opt-in-to-uniform-construction-and-direct-implementation-access"></a>Accepter la construction uniforme et l'accès à l’implémentation direct

Cette section décrit une fonctionnalité C++/WinRT 2.0 approuvée, bien qu’elle soit activée par défaut pour les nouveaux projets. Pour un projet existant, vous devez accepter en configurant l’outil `cppwinrt.exe`. Dans Visual Studio, définissez la propriété du projet **Propriétés communes** > **C++/WinRT** > **Optimisé** sur *Oui*. `<CppWinRTOptimized>true</CppWinRTOptimized>` est alors ajouté à votre fichier projet. Cela revient à ajouter le commutateur lors de l’appel de `cppwinrt.exe` à partir de la ligne de commande.

Le commutateur `-opt[imize]` active ce qu'on appelle souvent une *construction uniforme*. Une construction uniforme (ou *unifiée*) vous permet d'utiliser la projection du langage C++/WinRT pour créer et utiliser vos types d'implémentation (types implémentés par votre composant, à des fins de consommation par les applications) efficacement et sans difficultés de chargement.

Avant de décrire cette fonctionnalité, commençons par présenter la *situation* sans construction uniforme. Pour l'illustrer, nous allons commencer par cet exemple de classe Windows Runtime.

```idl
// MyClass.idl
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void Method();
        static void StaticMethod();
    }
}
```

En tant que développeur C++ habitué à utiliser la bibliothèque C++/WinRT, vous souhaiterez peut-être utiliser cette classe comme suit.

```cppwinrt
using namespace winrt::MyProject;

MyClass c;
c.Method();
MyClass::StaticMethod();
```

Un choix parfaitement raisonnable si le code consommateur affiché ne réside pas dans le même composant que celui qui implémente cette classe. Dans le cadre d’une projection de langage, C++/WinRT vous protège en tant que développeur de ABI (interface binaire d’application basée sur COM que Windows Runtime définit). C++/WinRT n’appelle pas directement dans l’implémentation ; il utilise ABI.

Par conséquent, sur la ligne de code où vous construisez un objet **MyClass** (`MyClass c;`), la projection C++/WinRT appelle [**RoGetActivationFactory**](/windows/win32/api/roapi/nf-roapi-rogetactivationfactory) pour récupérer la classe ou la fabrique d’activation, puis utilise cette fabrique pour créer l’objet. La dernière ligne utilise également la fabrique pour effectuer ce qui s'apparente à un appel de méthode statique. Tout cela implique que vos classes soient inscrites et que votre module implémente le point d’entrée [**DllGetActivationFactory**](/previous-versions/br205771(v=vs.85)). C++/WinRT dispose d’un cache de fabrique très rapide, et donc, une application consommant votre composant ne rencontre aucun problème. Mais au sein de votre composant, vous venez de faire quelque chose d'un peu problématique.

Premièrement, quelle que soit la vitesse du cache de fabrique C++/WinRT, un appel via **RoGetActivationFactory** (voire les appels suivants via le cache de fabrique) sera toujours plus lent qu'un appel direct dans l'implémentation. Un appel à **RoGetActivationFactory** suivi de [ **IActivationFactory::ActivateInstance**](/windows/win32/api/activation/nf-activation-iactivationfactory-activateinstance), suivi de [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)) n'est évidemment pas aussi efficace qu'une expression C++ `new` pour un type défini localement. Par conséquent, les développeurs C++/WinRT chevronnés sont habitués à utiliser les fonctions d'assistance [**WinRT::Make**](/uwp/cpp-ref-for-winrt/make) ou [**WinRT::make_self**](/uwp/cpp-ref-for-winrt/make-self) lors de la création d’objets dans un composant.

```cppwinrt
// MyClass c;
MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
```

Mais, comme vous pouvez le constater, ce n’est pas aussi pratique ou concis. Vous devez utiliser une fonction d’assistance pour créer l’objet, de même que vous devez lever l’ambiguïté entre le type d’implémentation et le type projeté.

Deuxièmement, l’utilisation de la projection pour créer la classe implique que sa fabrique d’activation soit mise en cache. En règle générale, c'est ce que vous voulez, mais si la fabrique réside dans le même module (DLL) que l'appelant, vous épinglez efficacement le DLL et l'empêchez de se décharger. Dans de nombreux cas, cela n’a pas d’importance, mais certains composants système *doivent* prendre en charge le déchargement.

C’est là qu’intervient la *construction uniforme*. Que le code de création réside dans un projet qui utilise simplement la classe ou dans le projet qui *implémente* la classe, vous pouvez librement utiliser la même syntaxe pour créer l’objet.

```cppwinrt
// MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
MyClass c;
```

Lorsque vous créez votre projet de composant avec le commutateur `-opt[imize]`, l’appel par le biais de la projection de langage est compilé vers le même appel de la fonction **winrt::make** qui crée directement le type d’implémentation. Votre syntaxe est ainsi aussi simple que prévisible ; elle évite toute perte de performance liée à un appel par le biais de la fabrique, de même que l'épinglage du composant au cours du processus. En plus des projets de composants, cela s'avère aussi utile pour les applications XAML. Le contournement de **RoGetActivationFactory** pour les classes implémentées dans la même application vous permet de les construire (sans avoir besoin de les inscrire) de la même manière que si elles se trouvaient en dehors de votre composant.

La construction uniforme s'applique à *tout* appel traité par la fabrique en arrière-plan. Concrètement, cela signifie que l’optimisation sert à la fois les constructeurs et les membres statiques. Voici à nouveau l'exemple d’origine.

```cppwinrt
MyClass c;
c.Method();
MyClass::StaticMethod();
```

Sans `-opt[imize]`, les première et dernière instructions requièrent des appels via l’objet de fabrique. *Avec* `-opt[imize]`, aucune d'elles n'en a besoin. Et ces appels sont compilés directement par rapport à l’implémentation, et peuvent même être intégrés. Ce qui nous amène à aborder un autre terme souvent utilisé en matière de `-opt[imize]`, à savoir l'accès *direct à l'implémentation*.

Les projections de langage sont pratiques, mais un accès direct à l'implémentation vous permet de produire le code le plus efficace possible. C++/WinRT peut le faire à votre place, sans vous forcer à vous départir de la sécurité et de la productivité de la projection.

Il s'agit là d'un changement cassant car le composant doit coopérer pour permettre à la projection de langage d'atteindre ses types d'implémentation et d'y accéder directement. C++/WinRT étant une bibliothèque à en-tête uniquement, vous pouvez y jeter un coup d'œil pour voir ce qui se passe. Sans `-opt[imize]`, le constructeur **MyClass** et le membre **StaticMethod** sont définis par la projection comme suit.

```cppwinrt
namespace winrt::MyProject
{
    inline MyClass::MyClass() :
        MyClass(impl::call_factory<MyClass>([](auto&& f){
            return f.template ActivateInstance<MyClass>(); }))
    {
    }
    inline void MyClass::StaticMethod()
    {
        impl::call_factory<MyClass, MyProject::IClassStatics>([&](auto&& f) {
            return f.StaticMethod(); });
    }
}
```

Il n’est pas nécessaire de suivre tout ce qui est indiqué ci-dessus, l’objectif étant de montrer que les deux appels impliquent un appel à une fonction nommée **call_factory**. Cela vous indique que ces appels impliquent le cache de fabrique et qu’ils n’accèdent pas directement à l’implémentation. *Avec*`-opt[imize]`, ces mêmes fonctions ne sont pas définies. En fait, elles sont déclarées par la projection et leurs définitions reviennent au composant.

Le composant peut ensuite fournir les définitions qui appellent directement dans l’implémentation. Il s'agit là d'un changement cassant. Ces définitions sont générées pour vous lorsque vous utilisez `-component` et `-opt[imize]`, et elles apparaissent dans un fichier nommé `Type.g.cpp`, où *Type* correspond au nom de la classe de runtime implémentée. C’est la raison pour laquelle vous pouvez rencontrer diverses erreurs d'éditeur la première fois que vous activez `-opt[imize]` dans un projet existant. Vous devez inclure ce fichier généré dans votre implémentation afin d'assembler les éléments.

Dans notre exemple, `MyClass.h` peut se présenter comme suit (avec ou sans utilisation de `-opt[imize]`).

```cppwinrt
// MyClass.h
#pragma once
#include "MyClass.g.h"
 
namespace winrt::MyProject::implementation
{
    struct MyClass : ClassT<MyClass>
    {
        MyClass() = default;
 
        static void StaticMethod();
        void Method();
    };
}
namespace winrt::MyProject::factory_implementation
{
    struct MyClass : ClassT<MyClass, implementation::MyClass>
    {
    };
}
```

Votre `MyClass.cpp` est l'endroit où tout prend forme.

```cppwinrt
#include "pch.h"
#include "MyClass.h"
#include "MyClass.g.cpp" // !!It's important that you add this line!!
 
namespace winrt::MyProject::implementation
{
    void MyClass::StaticMethod()
    {
    }
 
    void MyClass::Method()
    {
    }
}
```

Par conséquent, pour utiliser la construction uniforme dans un projet existant, vous devez modifier le fichier `.cpp` de l’implémentation pour `#include <Sub/Namespace/Type.g.cpp>` après l’inclusion (et la définition) de la classe d’implémentation. Ce fichier fournit les définitions des fonctions que la projection n'a pas définies. Voici à quoi ressemblent ces définitions dans le fichier `MyClass.g.cpp`.

```cppwinrt
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

Et cela complète parfaitement la projection avec des appels efficaces directement dans l’implémentation, en évitant les appels au cache de fabrique et en satisfaisant l’éditeur de liens.

La dernière chose dont se charge `-opt[imize]` pour vous, c'est de modifier l’implémentation `module.g.cpp` de votre projet (le fichier qui vous aide à implémenter les exports **DllGetActivationFactory** et **DllCanUnloadNow** de telle sorte que les builds incrémentielles tendront à être plus rapides en éliminant le couplage de type fort précédemment requis par C++/WinRT 1.0. On parle souvent de *fabriques avec types effacés*. Sans `-opt[imize]`, le fichier `module.g.cpp` généré pour votre composant commence par inclure les définitions de toutes vos classes d’implémentation, &mdash;`MyClass.h`dans cet exemple. Il crée ensuite directement la fabrique d’implémentation pour chaque classe comme suit.

```cppwinrt
if (requal(name, L"MyProject.MyClass"))
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
```

Là encore, vous n'êtes pas tenu de suivre tous les détails. Notez cependant que cela requiert la définition complète de toutes les classes implémentées par votre composant. Cela peut avoir un effet important sur votre boucle interne, car toute modification apportée à une implémentation entraîne une recompilation de `module.g.cpp`. Avec `-opt[imize]`, ce n'est plus le cas. Au lieu de cela, deux choses se produisent au niveau du fichier `module.g.cpp` généré. La première c'est qu'il n'inclut plus de classes d’implémentation. Dans cet exemple, il n’inclut pas `MyClass.h`. En fait, il crée les fabriques d’implémentation sans connaître leur implémentation.

```cppwinrt
void* winrt_make_MyProject_MyClass();
 
if (requal(name, L"MyProject.MyClass"))
{
    return winrt_make_MyProject_MyClass();
}
```

Évidemment, il n’est pas nécessaire d’inclure leurs définitions, et il incombe à l’éditeur de liens de résoudre la définition de la fonction **winrt_make_Component_Class**. Vous n’avez pas à vous en soucier, car le fichier `MyClass.g.cpp` généré pour vous (et que vous avez précédemment inclus pour prendre en charge la construction uniforme) définit également cette fonction. Voici l’intégralité du fichier `MyClass.g.cpp` généré pour cet exemple.

```cppwinrt
void* winrt_make_MyProject_MyClass()
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

Comme vous pouvez le constater, la fonction **winrt_make_MyProject_MyClass** crée directement la fabrique de votre implémentation. Ainsi, vous pouvez modifier toutes les implémentations, sans recompilation de `module.g.cpp`. Ce n'est que lorsque vous ajoutez ou supprimez des classes Windows Runtime que `module.g.cpp` est mis à jour et doit être recompilé.

## <a name="overriding-base-class-virtual-methods"></a>Remplacement des méthodes virtuelles de classe de base

Votre classe dérivée peut avoir des problèmes avec les méthodes virtuelles si la classe de base et la classe dérivée sont définies par l’application tandis que la méthode virtuelle est définie dans une classe Windows Runtime grand-parent. Dans la pratique, cela se produit si vous dérivez à partir de classes XAML. Le reste de cette section reprend là où l’exemple de la section [Classes dérivées](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#derived-classes) s’est arrêté.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

La hiérarchie est la suivante : [**Windows::UI::Xaml::Controls::Page**](/uwp/api/windows.ui.xaml.controls.page) \<- **BasePage** \<- **DerivedPage**. La méthode **BasePage::OnNavigatedFrom** remplace [**Page::OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) correctement, mais **DerivedPage::OnNavigatedFrom** ne remplace pas **BasePage::OnNavigatedFrom**.

Ici, **DerivedPage** réutilise la vtable **IPageOverrides** provenant de **BasePage**, ce qui signifie qu’elle ne parvient pas à remplacer la méthode **IPageOverrides::OnNavigatedFrom**. L’une des solutions possibles nécessite que **BasePage** soit une classe de modèle. De plus, l’intégralité de son implémentation doit avoir lieu dans un fichier d’en-tête, ce qui rend la procédure beaucoup trop compliquée.

Pour contourner ce problème, déclarez la méthode **OnNavigatedFrom** comme étant explicitement virtuelle dans la classe de base. Ainsi, lorsque l’entrée de vtable pour **DerivedPage::IPageOverrides::OnNavigatedFrom** appelle **BasePage::IPageOverrides::OnNavigatedFrom**, le producteur appelle **BasePage::OnNavigatedFrom**, laquelle (puisqu’elle est virtuelle) appelle **DerivedPage::OnNavigatedFrom**.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        // Note the `virtual` keyword here.
        virtual void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

Pour que cela fonctionne, tous les membres de la hiérarchie de classe doivent être d’accord sur les types de paramètre et de valeur de retour de la méthode **OnNavigatedFrom**. S’ils sont en désaccord, vous devez utiliser la version ci-dessus comme méthode virtuelle et encapsuler les alternatives.

## <a name="important-apis"></a>API importantes
* [Modèle de structure winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Fonction winrt::com_ptr::copy_from](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [Modèle de fonction winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)
* [Modèle de fonction WinRT::get_self](/uwp/cpp-ref-for-winrt/get-self)
* [Modèle de structure winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Modèle de fonction winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)
* [Fonction winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Rubriques connexes
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
