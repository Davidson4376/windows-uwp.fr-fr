---
description: Avec C++/WinRT, vous pouvez appeler les API Windows Runtime à l’aide de types de données C++ standard.
title: Types de données C++ standard et C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, données, types
ms.localizationpriority: medium
ms.openlocfilehash: a87ba48a0853058ba1259e079c586b97af551656
ms.sourcegitcommit: 8b4c1fdfef21925d372287901ab33441068e1a80
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844332"
---
# <a name="standard-c-data-types-and-cwinrt"></a>Types de données C++ standard et C++/WinRT

Avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), vous pouvez appeler des API Windows Runtime à l’aide de types de données C++ standard, y compris certains types de données de la bibliothèque C++ standard. Vous pouvez passer des chaînes standard aux API (consultez [Gestion des chaînes en C++/WinRT](strings.md)), et passer des listes d’initialiseurs et des conteneurs standard aux API qui attendent une collection sémantiquement équivalente.

Consultez également [Passage de paramètres à la frontière ABI](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi).

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

**winrt::array_view** est un type personnalisé C++/WinRT qui représente en toute sécurité une série contiguë de valeurs (définie dans la bibliothèque de base C++/WinRT, à savoir `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

Deuxièmement, **winrt::array_view** possède un constructeur initialiseur-liste.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

Dans de nombreux cas, vous pouvez choisir ou non de prendre en compte **winrt::array_view** dans votre programmation. Si vous choisissez de ne *pas* le prendre en compte, vous ne devrez changer aucun code si un type équivalent s’affiche dans la bibliothèque C++ standard.

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

Deux facteurs sont impliqués ici. Tout d’abord, l’appelé construit un **std::vector** à partir de la liste d’initialiseurs (cet appelé est asynchrone, donc il peut être - et doit être - le propriétaire de cet objet). Deuxièmement, C++/WinRT lie de manière transparente (et sans créer de copies) **std::vector** comme un paramètre de collection Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Vecteurs et tableaux standard
[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) a également des constructeurs de conversion à partir de **std::vector** et **std::array**.

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

C++/WinRT lie **std::vector** comme un paramètre de collection Windows Runtime. Par conséquent, vous pouvez transmettre un **std::vector&lt;winrt::hstring&gt;** , et il sera converti dans la collection Windows Runtime appropriée de **winrt::hstring**. Il existe des informations supplémentaires à prendre en compte si l’appelé est asynchrone. En raison des détails d’implémentation de ce cas, vous devez fournir un rvalue et donc une copie ou un déplacement du vecteur. Dans l’exemple de code ci-dessous, nous passons la propriété du vecteur à l’objet du type de paramètre accepté par l’appelé asynchrone (en veillant à ne pas accéder à nouveau à `vecH` après l’avoir déplacé). Pour en savoir plus sur rvalues, consultez [Catégories de valeurs et références](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Mais vous ne pouvez pas transmettre un **std::vector&lt;std::wstring&gt;** là où une collection Windows Runtime est attendue. Cela est dû au fait que, la collection Windows Runtime appropriée de **std::wstring** ayant été convertie, le langage C++ ne forcera pas le ou les paramètres de type de cette collection. Par conséquent, l’exemple de code suivant ne sera pas compilé (et la solution consiste à passer un **std::vector&lt;winrt::hstring&gt;** à la place, comme indiqué ci-dessus).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Tableaux bruts et plages de pointeurs
En gardant en tête qu’un type équivalent peut exister dans l’avenir dans la bibliothèque C++ standard, vous pouvez également travailler directement avec **winrt::array_view**, par choix ou par nécessité.

**winrt::array_view** a des constructeurs de conversion à partir d’un tableau brut et d’une plage de **T&ast;** (pointeurs vers le type d’élément).

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
Une série de constructeurs, d’opérateurs, de fonctions et d’itérateurs sont implémentés pour **winrt::array_view**. Un **winrt::array_view** étant une plage, vous pouvez l’utiliser avec `for` basé sur les plages ou avec **std::for_each**.

Pour plus d’exemples et d’informations, consultez la rubrique sur les informations de référence sur les API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** et constructions d’itération standard
[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) est un exemple d’API Windows Runtime qui retourne une collection de type [**IVector&lt;T&gt;** ](/uwp/api/windows.foundation.collections.ivector_t_) (projeté dans C++/WinRT comme **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;** ). Vous pouvez utiliser ce type avec des constructions d’itération standard, par exemple des `for` basés sur les plages.

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>Coroutines C++ avec des API Windows Runtime asynchrones
Vous pouvez continuer à utiliser la [Bibliothèque de modèles parallèles (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) lors de l’appel aux API Windows Runtime asynchrones. Toutefois, dans de nombreux cas, les coroutines C++ fournissent un idiome efficace et plus facilement codé pour interagir avec les objets asynchrones. Pour plus d’informations et des exemples de code, voir [Opérations concurrentes et asynchrones avec C++/WinRT](concurrency.md).

## <a name="important-apis"></a>API importantes
* [Interface IVector&lt;T&gt;](/uwp/api/windows.foundation.collections.ivector_t_)
* [Modèle de structure winrt::array_view](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Rubriques connexes
* [Gestion des chaînes en C++/WinRT](strings.md)
