---
description: Cette rubrique montre comment créer des API C++/WinRT à l’aide de la structure de base **winrt::implements**, directement ou indirectement.
title: Créer des API avec C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: windows10, uwp, standard, c++, cpp, winrt, projeté, projection, implémentation, implémenter, classe runtime, activation
ms.localizationpriority: medium
ms.openlocfilehash: 7fd543d7c3ad9dec878cc02b14a79c254d91b4be
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7968767"
---
# <a name="author-apis-with-cwinrt"></a>Créer des API avec C++/WinRT

Cette rubrique montre comment créer [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) API à l’aide de la [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) de base struct, directement ou indirectement. Les synonymes de *créer* dans ce contexte sont *produire* ou *implémenter*. Cette rubrique couvre les scénarios suivants pour l’implémentation d’API sur un type C++/WinRT, dans cet ordre.

- Vous ne créez *pas* une classe Windows Runtime (classe runtime); vous souhaitez simplement implémenter une ou plusieurs interfaces Windows Runtime pour une utilisation locale au sein de votre application. Vous dérivez directement de **winrt::implements** dans ce cas, et implémentez des fonctions.
- Si vous *créez* une classe runtime. Vous pouvez créer un composant qui sera utilisé à partir d’une application. Ou vous pouvez créer un type qui sera utilisé à partir de l’interface utilisateur XAML et, dans ce cas, vous implémentez et utilisez une classe runtime au sein de la même unité de compilation. Dans ces cas, laissez les outils générer pour vous des classes qui dérivent de **winrt::implements**.

Dans les deux cas, le type qui implémente vos API C++/WinRT APIs est appelé le *type d’implémentation*.

> [!IMPORTANT]
> Il est important de distinguer le concept d’un type d’implémentation de celui d’un type projeté. Le type projeté est décrit dans [Utiliser des API avec C++/WinRT](consume-apis.md).

## <a name="if-youre-not-authoring-a-runtime-class"></a>Si vous ne créez *pas* une classe runtime
Le scénario le plus simple est celui où vous implémentez une interface Windows Runtime pour une utilisation locale. Vous n’avez pas besoin d’une classe runtime, simplement d’une classe C++ ordinaire. Par exemple, vous pouvez écrire une application basée autour de [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication).

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

Dans Visual Studio, le **Visual C++** > **Windows universel** > **Core App (C++ / WinRT)** modèle de projet illustre le modèle de **CoreApplication** . Le modèle commence par transmettre une implémentation de [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) à [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

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

De ce fait, en tant que développeur, implémentez l’interface **IFrameworkViewSource**. C++/WinRT possède le modèle de structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) pour faciliter l’implémentation d’une (ou plusieurs) interface(s), sans avoir recours à la programmation de style COM. Vous dérivez simplement votre type de **impléments**, puis implémentez les fonctions de l’interface. Voici comment procéder.

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

Dans la mesure où votre type **App** *est un* **IFrameworkViewSource**, vous pouvez simplement en transmettre un à **Run**.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App{});
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Si vous créez une classe runtime dans un composant Windows Runtime
Si votre type est empaqueté dans un composant Windows Runtime pour une utilisation à partir d’une application, il doit alors s’agir d’une classe runtime.

> [!TIP]
> Nous vous recommandons de déclarer chaque classe runtime dans son propre fichier de langage IDL (Interface Definition Language) (.idl), afin d’optimiser les performances de génération lorsque vous modifiez un fichier IDL et pour la correspondance logique d’un fichier IDL avec ses fichiers de code source générés. Visual Studio fusionne tous les fichiers `.winmd` résultants dans un seul fichier avec le même nom que l’espace de noms racine. Ce dernier fichier `.winmd` sera celui auquel les consommateurs de votre composant feront référence.

En voici un exemple.

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

Ce fichierIDL déclare une classe Windows Runtime (runtime). Une classe runtime est un type qui peut être activé et utilisé via des interfaces COM modernes, généralement au-delà des limites exécutables. Lorsque vous ajoutez un fichier IDL à votre projet et build, la chaîne d’outils C++/WinRT (`midl.exe` et `cppwinrt.exe`) génère un type d’implémentation pour vous. Dans l’exemple de fichier IDL ci-dessus, le type d’implémentation est un stub de structure C++ nommé **winrt::MyProject::implementation::MyRuntimeClass** dans les fichiers de code source intitulés `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` et `MyRuntimeClass.cpp`.

Le type d’implémentation ressemble à ceci.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        hstring Name();
        void Name(hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Notez le modèle de polymorphisme F-Bound utilisé (**MyRuntimeClass** s’utilise lui-même comme un argument de modèle pour sa base **MyRuntimeClassT**). On parle aussi de motif de modèle curieusement récursif (Curiously Recurring Template Pattern ou CRTP). Si vous remontez la chaîne d’héritage, vous trouverez **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

Par conséquent, dans ce scénario, à la racine de la hiérarchie d’héritage se trouve de nouveau le modèle de structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

Pour obtenir plus d’informations, du code ainsi que la procédure à suivre pour créer des API dans un composant Windows Runtime, voir [Créer des événements en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Si vous créez une classe runtime qui sera référencée dans votre interface utilisateur XAML
Si votre type est référencé par votre interface utilisateur XAML, alors il doit s’agir d’une classe runtime, même si elle se trouve dans le même projet que le code XAML. Bien qu’elle soit généralement activée au-delà des limites exécutables, une classe runtime peut être utilisée au sein de l’unité de compilation qui l’implémente.

Dans ce scénario, vous créez *et* utilisez les API. La procédure d’implémentation de votre classe runtime est essentiellement identique à celle d’un composant Windows Runtime. De ce fait, reportez-vous à la section précédente &mdash;[Si vous créez une classe runtime dans un composant Windows Runtime](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component). La seule différence est que, à partir du fichier IDL, la chaîne d’outils C++/WinRT génère non seulement un type d’implémentation, mais également un type projeté. Il est important d'être conscient que «**MyRuntimeClass**» dans ce scénario peut être ambigu; il existe plusieurs entités portant ce nom, de types différents.

- **MyRuntimeClass** est le nom d’une classe runtime. Mais il s’agit en réalité d’une abstraction: déclarée dans IDL et implémentée dans un langage de programmation.
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

Pour obtenir un exemple de procédure d’implémentation de l’interface **INotifyPropertyChanged** sur une classe runtime, voir [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md).

La procédure d’utilisation de votre classe runtime dans ce scénario est décrite dans [Utiliser des API avec C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).

## <a name="runtime-class-constructors"></a>Constructeurs de classes runtime
Voici quelques points à retenir des listings que nous avons vus ci-dessus.

- Chaque constructeur que vous déclarez dans votre fichier IDL entraîne la génération d’un constructeur à la fois sur votre type d’implémentation et sur votre type projeté. Les constructeurs déclarés dans le fichier IDL servent à utiliser la classe runtime à partir d’une unité de compilation *différente*.
- Qu’un ou plusieurs constructeurs soient déclarés ou non dans le fichier IDL, une surcharge de constructeur qui prend `nullptr_t` est générée sur votre type projeté. Appeler le constructeur `nullptr_t` est *la première des deux étapes* requises pour utiliser la classe runtime à partir de *la même* unité de compilation. Pour obtenir plus d’informations et un exemple de code, voir [Utiliser des API avec C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Si vous utilisez la classe runtime à partir de *la même* unité de compilation, vous pouvez également implémenter des constructeurs autres que ceux par défaut, directement sur le type d’implémentation (qui, n’oubliez pas, se trouve dans `MyRuntimeClass.h`).

> [!NOTE]
> Si vous pensez que votre classe runtime sera utilisée à partir d’une unité de compilation différente (qui est commune), incluez le ou les constructeur(s) dans votre fichier IDL (au moins un constructeur par défaut). De cette manière, vous obtiendrez également une implémentation d’usine avec votre type d’implémentation.
> 
> Si vous souhaitez créer et utiliser votre classe runtime uniquement au sein de la même unité de compilation, ne déclarez aucun constructeur dans votre fichier IDL. Vous n’avez pas besoin d’une implémentation d’usine, et aucune ne sera générée. Le constructeur par défaut de votre type d’implémentation sera supprimé, mais vous pouvez facilement le modifier et utiliser la valeur par défaut à la place.
> 
> Si vous souhaitez créer et utiliser votre classe runtime uniquement au sein de la même unité de compilation et que vous avez besoin de paramètres de constructeur, créez le ou les constructeur(s) dont vous avez besoin directement sur votre type d’implémentation.

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

Ou vous pouvez le générer à partir du fichier IDL (c’est une classe runtime).

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

Pour passer de **MyType** à un objet **IStringable** ou **IClosable** que vous pouvez utiliser ou retourner dans le cadre de votre projection, vous pouvez appeler le modèle de fonction [**winrt::make**](/uwp/cpp-ref-for-winrt/make). **rendre** renvoie l’implémentation interface du type par défaut.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> Toutefois, si vous référencez votre type à partir de votre interface utilisateur XAML, un type d’implémentation et un type projeté se retrouveront dans le même projet. Dans ce cas, le **rendre** renvoie une instance du type projeté. Pour obtenir un exemple de code de ce scénario, voir [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

Nous ne pouvons utiliser que `istringable` (dans l’exemple de code ci-dessus) pour appeler les membres de l’interface **IStringable**. Mais une interface C++/WinRT (qui est une interface projetée) dérive de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Par conséquent, vous pouvez appeler [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) sur ce dernier pour interroger pour d’autres types projetés ou les interfaces, que vous pouvez également utiliser ou retourner.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Si vous avez besoin d’accéder à tous les membres de l’implémentation, puis renvoyer ultérieurement une interface à un appelant, utilisez le modèle de fonction [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** retourne un [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) qui encapsule le type d’implémentation. Vous pouvez accéder aux membres de toutes ses interfaces (à l’aide de l’opérateur flèche), vous pouvez le renvoyer à un appelant en l’état, ou vous pouvez appeler **as** sur lui et renvoyer l'objet d’interface résultant à un appelant.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

La classe **MyType** ne fait pas partie de la projection; il s’agit de l’implémentation. Mais de cette façon vous pouvez appeler ses méthodes d’implémentation directement, sans la surcharge d’un appel de fonction virtuelle. Dans l’exemple ci-dessus, même si **MyType::ToString** utilise la même signature que la méthode projetée sur **IStringable**, nous appelons la méthode non virtuelle directement, sans traverser la limite de l’interface binaire d’application (ABI). **com_ptr** pointant simplement un curseur sur la structure **MyType**, vous pouvez également accéder à tous les autres détails internes de **MyType** via la variable `myimpl` et l’opérateur flèche.

Dans le cas où vous avez un objet d’interface et que vous savez qu’il s’agit d’une interface sur votre implémentation, puis vous pouvez revenir à l’implémentation à l’aide du modèle de fonction [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) . Là encore, il s’agit d’une technique qui évite les appels de fonctions virtuelles et vous permet d’accéder directement à l’implémentation.

> [!NOTE]
> Si vous n’avez pas encore installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809) ou une version ultérieure, puis vous devez appeler [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) au lieu de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

Voici un exemple: Il existe un autre exemple dans [implémentez la classe de contrôle personnalisé **BgLabelControl** ](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class).

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Toutefois, seul l’objet d’interface d’origine conserve une référence. Si *vous* souhaitez la conserver, vous pouvez appeler [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function).

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

Si vous disposez d’une instance de votre type d’implémentation et que vous devez la transmettre à une fonction qui attend le type projeté correspondant, vous pouvez procéder ainsi. Un opérateur de conversion existant sur votre type d’implémentation (à condition que le type d’implémentation a été généré par le `cppwinrt.exe` outil) qui rend cela possible.

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Dérivation à partir d’un type qui possède un constructeur non défini par défaut
[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)**](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) est un exemple d’un constructeur non défini par défaut. Il n’existe aucun constructeur par défaut, de ce fait, pour construire un **ToggleButtonAutomationPeer**, vous devez transmettre un *owner*. Par conséquent, si vous dérivez de **ToggleButtonAutomationPeer**, vous devez fournir un constructeur qui prend un *owner* et le transmet à la base. Voyons à quoi cela ressemble dans la pratique.

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

Tout ce qui manque, c’est de transmettre ce paramètre de constructeur à la classe de base. Vous vous souvenez du modèle de polymorphisme F-Bound que nous avons mentionné ci-dessus? Une fois que vous êtes familiarisé avec les détails de ce modèle tel qu’utilisé en C++/WinRT, vous pouvez déterminer le nom de votre classe de base (ou vous pouvez simplement consulter le fichier d’en-tête de votre classe d’implémentation). Voici comment appeler le constructeur de classe de base dans ce cas.

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

Tant que vous n'aurez pas apporté les modifications décrites ci-dessus (pour transmettre ce paramètre de constructeur à la classe de base), le compilateur marquera votre constructeur et signalera qu’aucun constructeur par défaut approprié n’est disponible sur un type nommé (dans ce cas) **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;**. C’est en fait la classe de base de la classe de base de votre type d’implémentation.

## <a name="important-apis"></a>API importantes
* [Modèle de structure winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [fonction WinRT::com_ptr::copy_from](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)
* [Modèle de fonction winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)
* [modèle de fonction WinRT::get_self](/uwp/cpp-ref-for-winrt/get-self)
* [Modèle de structure winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Modèle de fonction winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)
* [Fonction winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Rubriquesassociées
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
