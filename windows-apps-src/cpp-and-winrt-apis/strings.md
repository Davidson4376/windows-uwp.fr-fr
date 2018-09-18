---
author: stevewhims
description: Avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues C++ standard, ou vous pouvez utiliser le type winrt::hstring.
title: Gestion des chaînes en C++/WinRT
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, chaîne
ms.localizationpriority: medium
ms.openlocfilehash: 332edcf17f2b6bbf595def67c9df7043f21828c7
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4020619"
---
# <a name="string-handling-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Gestion des chaînes en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues de la bibliothèque C++ standard comme **std::wstring** (remarque: pas avec des types de chaînes étroites comme **std::string**). C++/WinRT possède un type de chaîne personnalisée appelé [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (défini dans la bibliothèque de base C++/WinRT, à savoir `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`). Et c’est en fait le type de chaîne que les constructeurs, fonctions et propriétés Windows Runtime prennent et renvoient. Mais, dans de nombreux cas &mdash;grâce aux constructeurs de conversion et aux opérateurs de conversion de **hstring**&mdash; vous pouvez choisir de tenir compte ou non de **hstring** dans votre code client. Si vous *créez* des API, vous devrez probablement connaître **hstring**.

Il existe de nombreux types de chaîne en C++. Des variantes existent dans de nombreuses bibliothèques en plus de **std::basic_string** de la bibliothèque C++ standard. C++17 possède des utilitaires de conversion de chaînes et **std::basic_string_view** pour combler les écarts entre tous les types de chaîne.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) fournit une convertibilité avec **std::wstring_view** afin de garantir l’interopérabilité pour laquelle **std::basic_string_view** a été conçu.

## <a name="using-stdwstring-and-optionally-winrthstringuwpcpp-ref-for-winrthstring-with-uriuwpapiwindowsfoundationuri"></a>Utilisation de **std::wstring** (et éventuellement [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)) avec [**Uri**](/uwp/api/windows.foundation.uri)
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) est construit à partir d’un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

Mais **hstring** a des [constructeurs de conversion](/uwp/api/windows.foundation.uri#hstringhstring-constructor) qui vous permettent de l’utiliser sans avoir besoin d’en tenir compte. Voici un exemple de code illustrant comment créer un **Uri** à partir d’un littéral de chaîne étendue, à partir d’une vue de chaîne étendue et à partir d’un **std::wstring**.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

L’accesseur de propriété [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain) est de type **hstring**.

```cppwinrt
public:
    winrt::hstring Domain();
```

Mais, là encore, vous devez prendre conscience que ce détail est optionnel grâce à l’[opérateur de conversion vers **std::wstring_view**](/uwp/api/hstring#hstringoperator-stdwstringview) de **hstring**

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

De même, [**IStringable::ToString**](https://msdn.microsoft.com/library/windows/desktop/dn302136) renvoie hstring.

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** implémente l’interface [**IStringable**](https://msdn.microsoft.com/library/windows/desktop/dn302135).

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Vous pouvez utiliser la fonction [**hstring::c_str**](/uwp/api/windows.foundation.uri#hstringcstr-function) pour obtenir une chaîne étendue standard à partir d’un **hstring** (comme à partir d’un **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Si vous avez un **hstring**, vous pouvez créer un **Uri** à partir de celui-ci.

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

Envisagez d’une méthode qui prend un **hstring**.

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

Toutes les options que vous venez de voir s’appliquent également dans de tels cas.

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** a un opérateur de conversion **std::wstring_view** membre, et la conversion est obtenue sans effort.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>Fonctions et opérateurs **winrt::hstring**
Une série de constructeurs, d’opérateurs, de fonctions et d’itérateurs sont implémentés pour [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

Un **hstring** étant une plage, vous pouvez l’utiliser avec `for` basé sur les plages, ou avec `std::for_each`. Il fournit également des opérateurs de comparaison pour effectuer des comparaisons naturelles et efficaces à ses homologues dans la bibliothèque C++ standard. Et il inclut tout ce dont vous avez besoin pour utiliser **hstring** comme clé pour les conteneurs associatifs.

Nous reconnaissons que de nombreuses bibliothèques C++ utilisent **std::string** et fonctionnent exclusivement avec du texte UTF-8. Pour votre commodité, nous fournissons des programmes d’assistance, tels que [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) et [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring), pour la conversion arrière et dans les deux sens.

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Pour obtenir plus d’informations et des exemples sur les fonctions et opérateurs **hstring**, consultez la rubrique sur les informations de référence API [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>Logique pour **winrt::hstring** et **winrt::param::hstring**
Windows Runtime est implémenté en termes de caractères **wchar_t**, mais l’interface binaire d’application (ABI) de Windows Runtime n’est pas un sous-ensemble de ce que fournit **std::wstring** ou **std::wstring_view**. L’utilisation de ceux-ci entraînerait des inefficacités importantes. Au lieu de cela, C++/WinRT fournit **winrt::hstring**, qui représente une chaîne immuable cohérente avec le [HSTRING](https://msdn.microsoft.com/library/windows/desktop/br205775) sous-jacent et implémenté derrière une interface similaire à celle de **std::wstring**. 

Vous pouvez remarquer que les paramètres d’entrée C++/WinRT qui doivent accepter logiquement **winrt::hstring** attendent en fait **winrt::param::hstring**. L’espace de nom **param** contient un ensemble de types utilisés exclusivement pour optimiser les paramètres d’entrée afin d’effectuer la liaison naturelle aux types de la bibliothèque C++ standard et d’éviter des copies et autres inefficacités. Vous ne devez pas utiliser ces types directement. Si vous voulez utiliser une optimisation pour vos propres fonctions, utilisez **std::wstring_view**.

Le résultat est que vous pouvez ignorer les spécificités de la gestion des chaînes Windows Runtime, et travailler efficacement avec ce que vous connaissez. Et c’est important étant donné l’utilisation intensive des chaînes dans Windows Runtime.

# <a name="formatting-strings"></a>Mise en forme des chaînes
Une des options de mise en forme des chaînes est **std::wstringstream**. Voici un exemple qui met en forme et affiche un message de suivi de débogage simple.

```cppwinrt
#include <sstream>
...
void OnPointerPressed(IInspectable const&, PointerEventArgs const& args)
{
    float2 const point = args.CurrentPoint().Position();
    std::wstringstream wstringstream;
    wstringstream << L"Pointer pressed at (" << point.x << L"," << point.y << L")" << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());
}
```

## <a name="important-apis"></a>API importantes
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [fonction de WinRT::to_hstring](/uwp/cpp-ref-for-winrt/to-hstring)
* [fonction de WinRT::to_string](/uwp/cpp-ref-for-winrt/to-string)
