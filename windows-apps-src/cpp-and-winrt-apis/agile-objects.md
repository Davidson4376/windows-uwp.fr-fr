---
author: stevewhims
description: Un objet agile est un objet qui est accessible à partir de n’importe quel thread. Vos types C++/WinRT sont agiles par défaut, mais vous pouvez le refuser.
title: Objets agiles avec C++/WinRT
ms.author: stwhi
ms.date: 10/20/2018
ms.topic: article
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, agile, objet, agilité, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 2fa129a60c7dfcc170a9ddeec318a062fb8cbe56
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6270226"
---
# <a name="agile-objects-in-cwinrt"></a>Objets agiles en C++/WinRT

Dans la grande majorité des cas, une instance d’une classe Windows Runtime sont accessibles à partir de n’importe quel thread (comme la plupart des objets C++ peuvent). Une telle classe Windows Runtime est *agile*. Seul un petit nombre de classes Windows Runtime fournies avec Windows n’est pas agiles, mais lorsque vous les utilisez vous devez prendre en considération leur modèle de thread et leur comportement de rassemblement (rassemblement consiste à transmettre les données sur une limite de cloisonnement). Il s’agit d’une bonne valeur par défaut pour chaque objet Windows Runtime être agile, afin que vos propres [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) types sont agiles par défaut.

Toutefois, vous pouvez la refuser. Vous avez peut-être une bonne raison de vouloir qu’un objet de votre type réside, par exemple, dans un thread unique cloisonné. Cela est généralement lié aux exigences de réentrance. Mais, de plus en plus, même les API d’interface utilisateur proposent des objets agiles. En règle générale, l’agilité est l’option la plus simple et la plus performante. En outre, lorsque vous implémentez une usine d’activation, elle doit être agile même si votre classe runtime correspondante ne l’est pas.

> [!NOTE]
> Windows Runtime est basé sur COM. En termes COM, une classe agile est inscrite avec `ThreadingModel` = *Both*. Pour plus d’informations sur les modèles de thread et cloisonnements de COM, voir la [compréhension et à l’aide des modèles de thread COM](https://msdn.microsoft.com/library/ms809971).

## <a name="code-examples"></a>Exemples de code

Nous allons utiliser un exemple d’implémentation d’une classe runtime pour illustrer comment C++ / WinRT prend en charge l’agilité.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Étant donné que nous ne l’avons pas refusé, cette implémentation est agile. La structure de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implémente [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) et [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). L’implémentation **IMarshal** utilise **CoCreateFreeThreadedMarshaler** pour faire ce qu’il convient pour le code hérité qui ne connaît pas **IAgileObject**.

Ce code vérifie l’agilité d’un objet. L’appel à [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) lève une exception si `myimpl` n’est pas agile.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

Plutôt que de gérer une exception, vous pouvez appeler [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) à la place.

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

Vous pouvez choisir explicitement de refuser la prise en charge des objets agiles en transmettant la structure de marqueur [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non_agile) en tant qu’argument de modèle à votre classe de base.

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

Si ou vous refusiez non l’agilité, vous pouvez implémenter vous-même **IMarshal** . Par exemple, vous pouvez utiliser le marqueur **winrt::non_agile** afin d’éviter l’implémentation de l’agilité par défaut et implémenter **IMarshal** vous-même&mdash;par exemple, pour prendre en charge la sémantique marshaler par valeur.

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

L’appel [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agilerefget-function) retourne un proxy qui peut être utilisé en toute sécurité dans le contexte de thread dans lequel **get** est appelé.

## <a name="important-apis"></a>API importantes

* [Interface IAgileObject](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [Interface IMarshal](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [Modèle de structure winrt::agile_ref](/uwp/cpp-ref-for-winrt/agile-ref)
* [Modèle de structure winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Modèle de fonction winrt::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [Structure de marqueur winrt::non_agile](/uwp/cpp-ref-for-winrt/non_agile)
* [Fonction winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Fonction winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>Rubriquesassociées

* [Présentation et utilisation des modèles de thread COM](https://msdn.microsoft.com/library/ms809971)
