---
description: Avec C++/WinRT, vous pouvez appeler les API Windows Runtime à l’aide de types de données C++ standard.
title: Types de données C++ et C++/WinRT standard
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, données, types
ms.localizationpriority: medium
ms.openlocfilehash: 44de7b61264f8e0e04d1de6d2b1101844656f28b
ms.sourcegitcommit: 99271798fe53d9768fc52b21366de05268cadcb0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58221455"
---
# <a name="standard-c-data-types-and-cwinrt"></a>Types de données C++ et C++/WinRT standard

Avec [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), vous pouvez appeler Windows Runtime APIs à l’aide des types de données C++ Standard, y compris certains types de données de bibliothèque C++ Standard. Vous pouvez passer des chaînes standards aux API (consultez [des chaînes en C++ / c++ / WinRT](strings.md)), et vous pouvez transmettre initialiseur de listes et des conteneurs standards pour les API qui attendent une collection sémantiquement équivalente.

## <a name="standard-initializer-lists"></a>Listes d’initialiseurs standard
Une liste d’initialiseurs (**std::initializer_list**) est une construction de la bibliothèque C++ standard. Vous pouvez utiliser des listes d’initialiseurs lorsque vous appelez certains constructeurs et méthodes Windows Runtime. Par exemple, vous pouvez appeler [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes) avec une telle liste.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

Il existe deux éléments impliqués dans cette tâche. Tout d’abord, la méthode **DataWriter::WriteBytes** prend un paramètre de type [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**WinRT::array_view** personnalisé C++/WinRT type qui représente en toute sécurité une série contiguë de valeurs (il est défini dans le C++/WinRT base bibliothèque, `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

Ensuite, **winrt::array_view** a une liste d’initialiseurs de constructeur.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

Dans de nombreux cas, vous pouvez choisir s’il faut être conscient de **winrt::array_view** dans votre programmation. Si vous choisissez de ne *pas* le prendre en compte, vous ne devrez changer aucun code si un type équivalent s’affiche dans la bibliothèque C++ standard.

Vous pouvez transmettre une liste d’initialiseurs à une API Windows Runtime qui attend un paramètre de collection. Prenons **StorageItemContentProperties::RetrievePropertiesAsync** comme exemple.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Vous pouvez appeler cette API avec une liste d’initialiseurs comme suit.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

Deux facteurs sont impliqués ici. Tout d’abord, l’appelé construit un **std::vector** à partir de la liste d’initialiseurs (cette appelé est asynchrone, donc il est en mesure de propriétaire de cet objet, il doit). Deuxièmement, C++/WinRT lie de manière transparente (et sans créer de copies) **std::vector** comme un paramètre de collection Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Vecteurs et tableaux standard
[**WinRT::array_view** ](/uwp/cpp-ref-for-winrt/array-view) a également des constructeurs de conversion à partir de **std::vector** et **std::array**.

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

Par conséquent, vous pouvez appeler à la place **DataWriter::WriteBytes** avec un **std::vector**.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

Ou avec un **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

C++/WinRT lie **std::vector** comme un paramètre de collection Windows Runtime. Par conséquent, vous pouvez transmettre un **std::vector&lt;winrt::hstring&gt;**, et il sera converti dans la collection Windows Runtime appropriée de **winrt::hstring**. Il existe un détail supplémentaire à prendre en compte si l’appelé est asynchrone. En raison des détails d’implémentation de ce cas, vous devez fournir une rvalue, donc vous devez fournir une copie ou un déplacement du vecteur. Dans l’exemple de code ci-dessous, nous passons la propriété du vecteur à l’objet du type de paramètre accepté par l’appelé async (et nous sommes veiller à ne pas accéder `vecH` à nouveau après l’avoir déplacé). Si vous souhaitez en savoir plus sur les rvalues, consultez [catégories de valeur et des références pour les](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Mais vous ne pouvez pas transmettre un **std::vector&lt;std::wstring&gt;** là où une collection Windows Runtime est attendue. Cela est dû au fait que, la collection Windows Runtime appropriée de **std::wstring** ayant été convertie, le langage C++ ne forcera pas le ou les paramètres de type de cette collection. Par conséquent, l’exemple de code suivant n’est pas compilé (et la solution consiste à passer un **std::vector&lt;winrt::hstring&gt;**  au lieu de cela, comme indiqué ci-dessus).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Tableaux bruts et plages de pointeurs
Compte tenu de l’inconvénient qu’un type équivalent peut-être exister dans l’avenir dans le C++ bibliothèque Standard, vous pouvez également travailler directement avec **winrt::array_view** si vous souhaitez ou devez.

**WinRT::array_view** a des constructeurs de conversion à partir d’un tableau brut et à partir d’une plage de **T&ast;**  (pointeurs vers le type d’élément).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Opérateurs et fonctions winrt::array_view
Un hôte de constructeurs, les opérateurs, les fonctions et les itérateurs sont implémentées pour **winrt::array_view**. Un **winrt::array_view** est une plage, afin de pouvoir l’utiliser avec basé sur une plage `for`, ou avec **std::for_each**.

Pour plus d’exemples et d’informations, consultez la rubrique sur les informations de référence sur les API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;**  et constructions d’itération standard
[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) est un exemple d’une API de Runtime Windows qui retourne une collection de type [ **IVector&lt;T&gt;**  ](/uwp/api/windows.foundation.collections.ivector_t_) (projetés C + C++ / WinRT en tant que **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;**). Vous pouvez utiliser ce type avec des constructions d’itération standard, tels que sur une plage `for`.

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>Coroutines C++ avec Windows Runtime APIs asynchrones
Vous pouvez continuer à utiliser le [bibliothèque de modèles parallèles (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) lors de l’appel asynchrone Windows Runtime APIs. Toutefois, dans de nombreux cas, les coroutines C++ fournissent un idiome efficace et plus facilement codées pour interagir avec les objets asynchrones. Pour plus d’informations et d’exemples de code, consultez [concurrence et des opérations asynchrones avec C / c++ / WinRT](concurrency.md).

## <a name="important-apis"></a>API importantes
* [IVector&lt;T&gt; interface](/uwp/api/windows.foundation.collections.ivector_t_)
* [modèle de struct WinRT::array_view](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Rubriques connexes
* [Chaîne gère en C / c++ / WinRT](strings.md)
