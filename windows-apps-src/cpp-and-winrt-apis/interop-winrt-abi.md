---
description: Cette rubrique montre comment effectuer des conversions entre des objets de l’interface binaire d’application (ABI) et C++/WinRT.
title: Interopérabilité entre C++/WinRT et ABI
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrer, interopérabilité, ABI
ms.localizationpriority: medium
ms.openlocfilehash: d1def649772f94a03d5a1f352dcec1d32c7b0868
ms.sourcegitcommit: 5d71c97b6129a4267fd8334ba2bfe9ac736394cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800576"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interopérabilité entre C++/WinRT et ABI

Cette rubrique montre comment effectuer des conversions entre des objets de l’interface binaire d’application (ABI) du SDK et [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Vous pouvez utiliser ces techniques pour permettre l’interopérabilité entre le code qui utilise ces deux méthodes de programmation avec Windows Runtime, ou vous pouvez les utiliser à mesure que vous transférez votre code depuis ABI vers C++/WinRT.

En règle générale, C++/WinRT expose les types ABI en tant que **void\*** , de sorte que vous n’avez pas besoin d’inclure des fichiers d’en-tête de plateforme.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>Qu'est-ce que l’ABI Windows Runtime et quels sont les types ABI ?
Une classe Windows Runtime (classe runtime) est en réalité une abstraction. Cette abstraction définit une interface binaire (interface binaire d’application, ou ABI) qui permet à plusieurs langages de programmation d'interagir avec un objet. Quel que soit le langage de programmation, l'interaction du code client avec un objet Windows Runtime se produit au niveau le plus bas, avec des constructions de langage client traduites en appels dans ABI de l’objet.

Les en-têtes du SDK Windows dans le dossier « % WindowsSdkDir%Include\10.0.17134.0\winrt » (ajustez le numéro de version du SDK pour votre cas, si nécessaire), sont les fichiers d’en-tête ABI Windows Runtime. Ils ont été générés par le compilateur MIDL. Voici un exemple d’ajout d’un de ces en-têtes.

```cpp
#include <windows.foundation.h>
```

Et voici un exemple simplifié de l’un des types ABI que vous trouverez dans cet en-tête SDK particulier. Notez que l'espace de noms **ABI**, **Windows::Foundation** et tous les autres espaces de noms Windows, sont déclarés par les en-têtes SDK au sein de l'espace de noms **ABI**.

```cpp
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

**IUriRuntimeClass** est une interface COM. Mais en plus &mdash; comme sa base est **IInspectable**&mdash;**IUriRuntimeClass** est une interface Windows Runtime. Notez le type de retour **HRESULT**, plutôt que le déclenchement d’exceptions. Et l’utilisation d’artefacts tels que le handle **HSTRING** (il est recommandé de redéfinir ce handle sur `nullptr` lorsque vous avez terminé avec celui-ci). Cela donne un aperçu de ce que à quoi Windows Runtime ressemble au niveau binaire de l’application ; en d’autres termes, au niveau de programmation COM.

Windows Runtime est basé sur les API COM (Component Object Model). Vous pouvez accéder à Windows Runtime de cette façon, ou vous pouvez y accéder par le biais de *projections de langage*. Une projection masque les détails COM et fournit une expérience de programmation plus naturelle pour un langage donné.

Par exemple, si vous recherchez dans le dossier « % WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt » (là encore, ajustez le numéro de version du SDK pour votre cas, si nécessaire), vous y trouverez les en-têtes de projection de langage C++/WinRT. Il existe un en-tête pour chaque espace de noms Windows, tout comme il existe un en-tête ABI par espace de noms Windows. Voici un exemple d’ajout d’un de ces en-têtes C++/WinRT.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

Et, à partir de cet en-tête, voici l’équivalent C++/WinRT (simplifié) de ce type ABI que nous venons de voir.

```cppwinrt
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

L’interface ici est en C++ moderne standard. Elle rend superflus les **HRESULT** (C++/WinRT génère des exceptions si nécessaire). Et la fonction accesseur retourne un objet chaîne simple, qui est nettoyé à la fin de son étendue.

Cette rubrique est utile si vous souhaitez permettre l'interopérabilité avec, ou porter, du code qui fonctionne au niveau de la couche d'interface binaire d’application (ABI).

## <a name="converting-to-and-from-abi-types-in-code"></a>Conversion vers et depuis les types ABI dans le code
Par soucis de sécurité et de simplicité, pour les conversions bidirectionnelles vous pouvez simplement utiliser [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) et [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function). Voici un exemple de code (basé sur le modèle de projet **Console App**), qui montre également comment vous pouvez utiliser des alias d’espace de noms pour les différents îlots pour éviter les collisions potentielles d’espace de noms entre la projection C++/WinRT et ABI.

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

Les implémentations des fonctions **as** appellent [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Si vous souhaitez des conversions de niveau inférieur qui appellent uniquement [**AddRef**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref), vous pouvez utiliser les fonctions d’assistance [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) et [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi). L’exemple de code suivant ajoute ces conversions de niveau inférieur à l’exemple de code ci-dessus.

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uri, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uri, ptr.get());
    ptr = nullptr;
}
```

Voici d’autres techniques de conversions de bas niveau similaires, mais cette fois en utilisant des pointeurs bruts vers les types d’interface ABI (ceux définis par les en-têtes du SDK Windows).

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

Pour les conversions des niveaux les plus bas, qui copient uniquement les adresses, vous pouvez utiliser les fonctions d’assistance [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) et [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi).

`WINRT_ASSERT` est une définition de macro qui est développée en [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uri)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>Fonction convert_from_abi
Cette fonction d’assistance convertit un pointeur d’interface ABI brut en un objet équivalent C++/WinRT, avec une surcharge minimale.

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

La fonction appelle simplement [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) pour rechercher l’interface par défaut du type C++/WinRT demandé.

Comme nous l’avons vu, une fonction d’assistance n’est pas nécessaire pour convertir à partir d’un objet C++/WinRT vers le pointeur d’interface ABI équivalent. Utilisez simplement la fonction de membre [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) pour rechercher l’interface demandée. Les fonctions **as** et **try_as** renvoient un objet [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) encapsulant le type ABI demandé.

## <a name="code-example-using-convertfromabi"></a>Exemple de code utilisant convert_from_abi
Voici un exemple de code illustrant cette fonction d’assistance dans la pratique.

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr };

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="interoperating-with-abi-com-interface-pointers"></a>Interopérabilité avec les pointeurs d’interface COM ABI

Le modèle de fonction helper ci-dessous montre comment copier un pointeur d’interface COM ABI d’un type donné en son type de pointeur intelligent projeté C++/WinRT équivalent.

```cppwinrt
template<typename To, typename From>
To to_winrt(From* ptr)
{
    To result{ nullptr };
    winrt::check_hresult(ptr->QueryInterface(winrt::guid_of<To>(), winrt::put_abi(result)));
    return result;
}
...
ID2D1Factory1* com_ptr{ ... };
auto cppwinrt_ptr {to_winrt<winrt::com_ptr<ID2D1Factory1>>(com_ptr)};
```

Ce modèle de fonction helper suivant est équivalent, à ceci près qu’il copie à partir du type de pointeur intelligent provenant des [bibliothèques WIL (Windows Implementation Libraries)](https://github.com/Microsoft/wil).

```cppwinrt
template<typename To, typename From, typename ErrorPolicy>
To to_winrt(wil::com_ptr_t<From, ErrorPolicy> const& ptr)
{
    To result{ nullptr };
    if constexpr (std::is_same_v<typename ErrorPolicy::result, void>)
    {
        ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result));
    }
    else
    {
        winrt::check_result(ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result)));
    }
    return result;
}
```

Voir aussi [Consommer des composants COM avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-com).

### <a name="unsafe-interop-with-abi-com-interface-pointers"></a>Interopérabilité non sécurisée avec les pointeurs d’interface COM ABI

Le tableau qui suit montre (en plus d’autres opérations) des conversions non sécurisées entre un pointeur d’interface COM ABI d’un type donné et son type de pointeur intelligent projeté C++/WinRT équivalent. Pour le code dans le tableau, supposez ces déclarations.

```cppwinrt
winrt::Sample s;
ISample* p;

void GetSample(_Out_ ISample** pp);
```

Supposez aussi que **ISample** est l’interface par défaut pour **Sample**.

Vous pouvez déclarer cela au moment de la compilation avec ce code.

```cppwinrt
static_assert(std::is_same_v<winrt::default_interface<winrt::Sample>, winrt::ISample>);
```

| Opération | Comment procéder | Remarques |
|-|-|-|
| Extraire **ISample\***  de **winrt::Sample** | `p = reinterpret_cast<ISample*>(get_abi(s));` | *s* est toujours propriétaire de l’objet. |
| Détacher **ISample\***  de **winrt::Sample** | `p = reinterpret_cast<ISample*>(detach_abi(s));` | *s* n’est plus propriétaire de l’objet. |
| Transférer **ISample\***  vers un nouveau **winrt::Sample** | `winrt::Sample s{ p, winrt::take_ownership_from_abi };` | *s* prend la propriété de l’objet. |
| Définir **ISample\*** dans **winrt::Sample** | `*put_abi(s) = p;` | *s* prend la propriété de l’objet. Tout objet précédemment propriété de *s* subit une fuite (déclaration dans le débogage). |
| Recevoir **ISample\*** dans **winrt::Sample** | `GetSample(reinterpret_cast<ISample**>(put_abi(s)));` | *s* prend la propriété de l’objet. Tout objet précédemment propriété de *s* subit une fuite (déclaration dans le débogage). |
| Remplacer **ISample\*** dans **winrt::Sample** | `attach_abi(s, p);` | *s* prend la propriété de l’objet. L’objet précédemment propriété de *s* est libéré. |
| Copier **ISample\*** dans **winrt::Sample** | `copy_from_abi(s, p);` | *s* fait une nouvelle référence à l’objet. L’objet précédemment propriété de *s* est libéré. |
| Copier **winrt::Sample** dans **ISample\*** | `copy_to_abi(s, reinterpret_cast<void*&>(p));` | *p* reçoit une copie de l’objet. Tout objet précédemment propriété de *s* subit une fuite. |

## <a name="interoperating-with-the-abis-guid-struct"></a>Interopération avec le struct GUID d’ABI

[**GUID**](/previous-versions/aa373931(v%3Dvs.80)) est projeté en tant que **winrt::guid**. Pour les API que vous implémentez, vous devez utiliser **winrt::guid** pour les paramètres GUID. Sinon, il existe des conversions automatiques entre **winrt::guid** et **GUID** dès lors que vous incluez `unknwn.h` (implicitement inclus par <windows.h> et de nombreux autres fichiers d’en-tête) avant d’inclure les en-têtes C++/WinRT.

Si vous ne faites pas ça, vous pouvez faire un `reinterpret_cast` en dur entre eux. Pour le tableau qui suit, supposez ces déclarations.

```cppwinrt
winrt::guid winrtguid;
GUID abiguid;
```

| Conversion | Avec `#include <unknwn.h>` | Sans `#include <unknwn.h>` |
|-|-|-|
| De **winrt::guid** en **GUID** | `abiguid = winrtguid;` | `abiguid = reinterpret_cast<GUID&>(winrtguid);` |
| De **GUID** en **winrt::guid** | `winrtguid = abiguid;` | `winrtguid = reinterpret_cast<winrt::guid&>(abiguid);` |

## <a name="interoperating-with-the-abis-hstring"></a>Interopération avec HSTRING d’ABI

Le tableau suivant montre des conversions entre **winrt::hstring** et [**HSTRING**](/windows/win32/winrt/hstring), et d’autres opérations. Pour le code dans le tableau, supposez ces déclarations.

```cppwinrt
winrt::hstring s;
HSTRING h;

void GetString(_Out_ HSTRING* value);
```

| Opération | Comment procéder | Remarques |
|-|-|-|
| Extraire **HSTRING** de **hstring** | `h = static_cast<HSTRING>(get_abi(s));` | *s* est toujours propriétaire de la chaîne. |
| Détacher **HSTRING** de **hstring** | `h = reinterpret_cast<HSTRING>(detach_abi(s));` | *s* n’est plus propriétaire de la chaîne. |
| Définir **HSTRING** dans **hstring** | `*put_abi(s) = h;` | *s* prend la propriété de la chaîne. Toute chaîne précédemment propriété de *s* subit une fuite (déclaration dans le débogage). |
| Recevoir **HSTRING** dans **hstring** | `GetString(reinterpret_cast<HSTRING*>(put_abi(s)));` | *s* prend la propriété de la chaîne. Toute chaîne précédemment propriété de *s* subit une fuite (déclaration dans le débogage). |
| Remplacer **HSTRING** dans **hstring** | `attach_abi(s, h);` | *s* prend la propriété de la chaîne. La chaîne précédemment propriété de *s* est libérée. |
| Copier **HSTRING** dans **hstring** | `copy_from_abi(s, h);` | *s* fait une copie privée de la chaîne. La chaîne précédemment propriété de *s* est libérée. |
| Copier **hstring** dans **HSTRING** | `copy_to_abi(s, reinterpret_cast<void*&>(h));` | *h* reçoit une copie de la chaîne. Toute chaîne précédemment propriété de *h* est libérée. |

## <a name="important-apis"></a>API importantes
* [Fonction AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [Fonction QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Fonction winrt::attach_abi](/uwp/cpp-ref-for-winrt/attach-abi)
* [Modèle de structure winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Fonction winrt::copy_from_abi](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [Fonction winrt::copy_to_abi](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [Fonction winrt::detach_abi](/uwp/cpp-ref-for-winrt/detach-abi)
* [Fonction winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Fonction de membre winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Fonction de membre winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
