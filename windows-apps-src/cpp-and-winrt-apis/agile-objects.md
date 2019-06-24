---
description: Un objet agile est un objet qui est accessible à partir de n’importe quel thread. Vos types C++/WinRT sont agiles par défaut, mais vous pouvez le refuser.
title: Objets agiles avec C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, agile, objet, agilité, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 82dff619e6fa3934f69b93090bee90de6359ca07
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360332"
---
# <a name="agile-objects-in-cwinrt"></a>Objets agiles en C++/WinRT

Dans la grande majorité des cas, une instance d’une classe Windows Runtime est accessible à partir de n’importe quel thread (comme le sont la plupart des objets C++ standard). Une telle classe Windows Runtime est *agile*. Seul un petit nombre de classes Windows Runtime fournies avec Windows ne sont pas agiles mais, lorsque vous les utilisez, vous devez prendre en considération leur modèle de thread et leur comportement de marshaling (consistant à transmettre des données à travers une délimitation de cloisonnement). Il s’agit d’une bonne valeur par défaut pour que chaque objet Windows Runtime soit agile, de façon que vos propres types [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) soient agiles par défaut.

Mais vous pouvez le refuser. Vous avez peut-être une bonne raison de vouloir qu’un objet de votre type réside, par exemple, dans un thread unique cloisonné. Cela est généralement lié à des exigences de réentrance. Toutefois, de plus en plus, même les API d’interface utilisateur proposent des objets agiles. En règle générale, l’agilité est l’option la plus simple et la plus performante. Par ailleurs, lorsque vous implémentez une fabrique d’activation, celle-ci doit être agile même si votre classe Runtime correspondante ne l’est pas.

> [!NOTE]
> Le Windows Runtime est basé sur COM. En termes de COM, une classe agile est inscrite avec `ThreadingModel` = *Both*. Pour plus d’informations sur les modèles de thread COM, voir [Présentation et utilisation des modèles de thread COM](/previous-versions/ms809971(v=msdn.10)).

## <a name="code-examples"></a>Exemples de code

Nous allons utiliser un exemple d’implémentation pour illustrer comment C++/WinRT prend en charge l’agilité.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Cette implémentation est agile étant donné que nous n’avons pas refusé qu’elle le soit. Le struct de base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implémente [**IAgileObject**](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject) et [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). L’implémentation de **IMarshal** utilise **CoCreateFreeThreadedMarshaler** pour faire ce qu’il convient pour le code hérité qui ne connaît pas **IAgileObject**.

Ce code vérifie l’agilité d’un objet. L’appel à [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) lève une exception si `myimpl` n’est pas agile.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

Au lieu de gérer une exception, vous pouvez appeler [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function).

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** ne possède aucune méthode propre. Vous ne pouvez donc pas en faire grand chose. La variante suivante est plus courante.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** est une *interface de marqueur*. Le succès ou l’échec de l’interrogation de **IAgileObject** est la seule information obtenue.

## <a name="opting-out-of-agile-object-support"></a>Refus de prise en charge des objets agiles

Vous pouvez choisir explicitement de refuser la prise en charge des objets agiles en transmettant le struct de marqueur [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) en tant qu’argument de modèle à votre classe de base.

Si vous dérivez directement à partir de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

Si vous créez une classe Runtime.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

Peu importe où le struct de marqueur apparaît dans le pack de paramètre variadique.

Que vous acceptiez ou refusiez l’agilité, vous pouvez implémenter **IMarshal** vous-même. Par exemple, vous pouvez utiliser le marqueur **winrt::non_agile** pour éviter l’implémentation de l’agilité par défaut, et implémenter **IMarshal** vous-même&mdash;par exemple, pour prendre en charge la sémantique de marshaling par valeur.

## <a name="agile-references-winrtagileref"></a>Références agiles (winrt::agile_ref)

Si vous utilisez un objet qui n’est pas agile, mais devez le transmettre dans un contexte potentiellement agile, une option consiste à utiliser le modèle de struct [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) afin d’obtenir une référence agile à une instance d’un type non agile ou à une interface d’un objet non agile.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

Vous pouvez également utiliser la fonction d’assistance [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile).

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

L’appel [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) retourne un proxy qui peut être utilisé en toute sécurité dans le contexte de thread dans lequel la commande **get** est appelée.

## <a name="important-apis"></a>API importantes

* [Interface IAgileObject](https://docs.microsoft.com/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [Interface IMarshal](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [Modèle de struct winrt::agile_ref](/uwp/cpp-ref-for-winrt/agile-ref)
* [Modèle de struct winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Modèle de fonction winrt::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [Struct de marqueur winrt::non_agile](/uwp/cpp-ref-for-winrt/non-agile)
* [Fonction winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Fonction winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>Rubriques connexes

* [Présentation et utilisation des modèles de thread COM](/previous-versions/ms809971(v=msdn.10))
