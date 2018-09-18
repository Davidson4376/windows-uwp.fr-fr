---
author: stevewhims
description: Cette rubrique montre comment effectuer des conversions entre des objets de l’interface binaire d’application (ABI) et C++/WinRT.
title: Interopérabilité entre C++/WinRT et ABI
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, port, migrer, interopérabilité, ABI
ms.localizationpriority: medium
ms.openlocfilehash: b641591e7be23226edc354e02513d723fbe8afba
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "4019489"
---
# <a name="interop-between-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-and-the-abi"></a>Interopérabilité entre [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) et ABI
Cette rubrique montre comment effectuer des conversions entre des objets de l’interface binaire d’application (ABI) du SDK et C++/WinRT. Vous pouvez utiliser ces techniques pour permettre l’interopérabilité entre le code qui utilise ces deux méthodes de programmation avec Windows Runtime, ou vous pouvez les utiliser à mesure que vous transférez votre code depuis ABI vers C++/WinRT.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>Qu'est-ce que l’ABI Windows Runtime et quels sont les types ABI?
Une classe Windows Runtime (classe runtime) est en réalité une abstraction. Cette abstraction définit une interface binaire (interface binaire d’application, ou ABI) qui permet à plusieurs langages de programmation d'interagir avec un objet. Quel que soit le langage de programmation, l'interaction du code client avec un objet Windows Runtime se produit au niveau le plus bas, avec des constructions de langage client traduites en appels dans ABI de l’objet.

Les en-têtes du SDK Windows dans le dossier «% WindowsSdkDir%Include\10.0.17134.0\winrt» (ajustez le numéro de version du SDK pour votre cas, si nécessaire), sont les fichiers d’en-tête ABI WindowsRuntime. Ils ont été générés par le compilateur MIDL. Voici un exemple d’ajout d’un de ces en-têtes.

```
#include <windows.foundation.h>
```

Et voici un exemple simplifié de l’un des types ABI que vous trouverez dans cet en-tête SDK particulier. Notez que l'espace de noms **ABI**, **Windows::Foundation** et tous les autres espaces de noms Windows, sont déclarés par les en-têtes SDK au sein de l'espace de noms **ABI**.

```
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

**IUriRuntimeClass** est une interface COM. Mais en plus &mdash;comme sa base est **IInspectable**&mdash; **IUriRuntimeClass** est une interface Windows Runtime. Notez le type de retour **HRESULT**, plutôt que le déclenchement d’exceptions. Et l’utilisation d’artefacts tels que le handle **HSTRING** (il est recommandé de redéfinir ce handle sur `nullptr` lorsque vous avez terminé avec celui-ci). Cela donne un aperçu de ce que à quoi Windows Runtime ressemble au niveau binaire de l’application; en d’autres termes, au niveau de programmation COM.

Windows Runtime est basé sur les API COM (Component Object Model). Vous pouvez accéder à Windows Runtime de cette façon, ou vous pouvez y accéder par le biais de *projections de langage*. Une projection masque les détails COM et fournit une expérience de programmation plus naturelle pour un langage donné.

Par exemple, si vous recherchez dans le dossier «% WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt» (là encore, ajustez le numéro de version du SDK pour votre cas, si nécessaire), vous y trouverez les en-têtes de projection de langage C++/WinRT. Il existe un en-tête pour chaque espace de noms Windows, tout comme il existe un en-tête ABI par espace de noms Windows. Voici un exemple d’ajout d’un de ces en-têtes C++/WinRT.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

Et, à partir de cet en-tête, voici l’équivalent C++/WinRT (simplifié) de ce type ABI que nous venons de voir.

```
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
Par soucis de sécurité et de simplicité, pour les conversions bidirectionnelles vous pouvez simplement utiliser [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) et [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function). Voici un exemple de code (basé sur le modèle de projet **Console App**), qui montre également comment vous pouvez utiliser des alias d’espace de noms pour les différents îlots pour éviter les collisions potentielles d’espace de noms entre la projection C++/WinRT et ABI.

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>

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
    winrt::com_ptr<abi::IStringable> ptr = uri.as<abi::IStringable>();

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable = ptr.as<winrt::IStringable>();
}
```

Les implémentations des fonctions **as** appellent [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Si vous souhaitez des conversions de niveau inférieur qui appellent uniquement [**AddRef**](https://msdn.microsoft.com/library/windows/desktop/ms691379), vous pouvez utiliser les fonctions d’assistance [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) et [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi). L’exemple de code suivant ajoute ces conversions de niveau inférieur à l’exemple de code ci-dessus.

```cppwinrt
int main()
{
    ...

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
    ...

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning = nullptr;
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

Pour les conversions des niveaux les plus bas, qui copient uniquement les adresses, vous pouvez utiliser les fonctions d’assistance [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) et [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi).

```cppwinrt
    ...

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning = static_cast<abi::IStringable*>(winrt::get_abi(uri));
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

La fonction appelle simplement [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) pour rechercher l’interface par défaut du type C++/WinRT demandé.

Comme nous l’avons vu, une fonction d’assistance n’est pas nécessaire pour convertir à partir d’un objet C++/WinRT vers le pointeur d’interface ABI équivalent. Utilisez simplement la fonction de membre [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) pour rechercher l’interface demandée. Les fonctions **as** et **try_as** renvoient un objet [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) encapsulant le type ABI demandé.

## <a name="code-example-using-convertfromabi"></a>Exemple de code utilisant convert_from_abi
Voici un exemple de code illustrant cette fonction d’assistance dans la pratique.

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>
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

template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="important-apis"></a>API importantes
* [Fonction AddRef](https://msdn.microsoft.com/library/windows/desktop/ms691379)
* [Fonction QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [fonction de WinRT::attach_abi](/uwp/cpp-ref-for-winrt/attach-abi)
* [Modèle de structure winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [fonction de WinRT::copy_from_abi](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [fonction de WinRT::copy_to_abi](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [fonction de WinRT::detach_abi](/uwp/cpp-ref-for-winrt/detach-abi)
* [Fonction winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Fonction de membre winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Fonction de membre winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)
