---
description: Un objet agile est un objet qui est accessible à partir de n’importe quel thread. Vos types C++/WinRT sont agiles par défaut, mais vous pouvez le refuser.
title: Objets agiles avec C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, agile, objet, agilité, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 82dff619e6fa3934f69b93090bee90de6359ca07
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360332"
---
# <a name="agile-objects-in-cwinrt"></a>Objets agiles en C / c++ / WinRT

Dans la grande majorité des cas, une instance d’une classe Windows Runtime est accessible à partir de n’importe quel thread (à l’instar des objets C++ standard plus peuvent). Une telle classe Windows Runtime est *agile*. Seul un petit nombre de classes Windows Runtime fournis avec Windows est non agiles, mais lorsque vous les consommez vous devez prendre en considération leur modèle de thread et le comportement de marshaling (marshaling est passer des données sur une limite de cloisonnement). Il est bon par défaut pour chaque objet Windows Runtime être agile, donc votre propre [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) types sont agiles par défaut.

Mais vous pouvez le refuser. Une bonne raison pour exiger un objet de votre type réside, par exemple, dans un thread unique cloisonné donné peut avoir. Cela est généralement lié aux exigences de réentrance. Mais, de plus en plus, même les API d’interface utilisateur proposent des objets agiles. En règle générale, l’agilité est l’option la plus simple et la plus performante. En outre, lorsque vous implémentez une usine d’activation, elle doit être agile même si votre classe runtime correspondante ne l’est pas.

> [!NOTE]
> Windows Runtime est basé sur COM. En termes COM, une classe agile est inscrite avec `ThreadingModel` = *Both*. Pour plus d’informations sur les modèles et les cloisonnements de thread COM, consultez [compréhension et Using COM Threading Models](/previous-versions/ms809971(v=msdn.10)).

## <a name="code-examples"></a>Exemples de code

Nous allons utiliser un exemple d’implémentation d’une classe runtime pour illustrer comment C++ / c++ / WinRT prend en charge l’agilité.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Étant donné que nous ne l’avons pas refusé, cette implémentation est agile. La structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implémente [**IAgileObject**](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject) et [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). L’implémentation **IMarshal** utilise **CoCreateFreeThreadedMarshaler** pour faire ce qu’il convient pour le code hérité qui ne connaît pas **IAgileObject**.

Ce code vérifie l’agilité d’un objet. L’appel à [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) lève une exception si `myimpl` n’est pas agile.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

Plutôt que de gérer une exception, vous pouvez appeler [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) à la place.

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** ne possède aucune méthode propre, vous ne pouvez donc pas faire grand chose avec. La variante suivante est plus courante.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** est une *interface de marqueurs*. Le simple succès ou échec résultant de l’interrogation de **IAgileObject** est l’étendue des informations et de l’utilité obtenue.

## <a name="opting-out-of-agile-object-support"></a>Refus de prise en charge des objets agiles

Vous pouvez choisir explicitement de refuser la prise en charge des objets agiles en transmettant la structure de marqueur [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) en tant qu’argument de modèle à votre classe de base.

Si vous dérivez directement de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

Si vous créez une classe runtime.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

Peu importe où la structure de marqueur apparaît dans le pack de paramètres variadiques.

Si vous refuser l’agilité, vous pouvez implémenter **IMarshal** vous-même. Par exemple, vous pouvez utiliser la **winrt::non_agile** marqueur pour éviter l’implémentation d’agilité par défaut et implémenter **IMarshal** vous-même&mdash;par exemple, pour prendre en charge la sémantique de marshaler par valeur.

## <a name="agile-references-winrtagileref"></a>Références agiles (winrt::agile_ref)

Si vous utilisez un objet qui n’est pas agile, mais que vous devez le transmettre dans un contexte potentiellement agile, alors une option consiste à utiliser le modèle de structure [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) afin d’obtenir une référence agile à une instance d’un type non agile ou à une interface d’un objet non agile.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

Ou, vous pouvez utiliser la fonction d’assistance [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile).

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

Dans les deux cas, `agile` peut désormais être librement transmis à un thread dans un cloisonnement différent et y être utilisé.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

L’appel [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) retourne un proxy qui peut être utilisé en toute sécurité dans le contexte de thread dans lequel **get** est appelé.

## <a name="important-apis"></a>API importantes

* [Interface de IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [IMarshal (interface)](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [winrt::agile_ref struct template](/uwp/cpp-ref-for-winrt/agile-ref)
* [modèle de struct WinRT::Implements](/uwp/cpp-ref-for-winrt/implements)
* [modèle de fonction WinRT::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile marker struct](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown::as function](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as function](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>Rubriques connexes

* [Comprendre et utiliser des modèles de thread COM.](/previous-versions/ms809971(v=msdn.10))
