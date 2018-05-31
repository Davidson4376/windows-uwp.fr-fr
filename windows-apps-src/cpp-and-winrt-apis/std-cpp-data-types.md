---
author: stevewhims
description: Avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de données C++ standard.
title: Types de données C++ standard et C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, données, types
ms.localizationpriority: medium
ms.openlocfilehash: ccf79b1ec21688d9573e62777def8f15295c3fca
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832073"
---
# <a name="standard-c-data-types-and-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Types de données C++ standard et [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de données C++ standard, y compris certains types de données de la bibliothèque C++ standard.

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
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to an array_view before being passed to WriteBytes.
}
```

Il existe deux éléments impliqués dans cette tâche. Tout d’abord, la méthode **DataWriter::WriteBytes** prend un paramètre de type [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(array_view<uint8_t const> value) const
```

 **array_view** est un type personnalisé C++/WinRT qui représente en toute sécurité une série contiguë de valeurs (définie dans la bibliothèque de base C++/WinRT, à savoir `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

Deuxièmement, **array_view** possède un constructeur initialiseur-liste.

```cppwinrt
template <typename T> array_view(std::initializer_list<T> value) noexcept
```

Dans de nombreux cas, vous pouvez choisir ou non de prendre en compte **array_view** dans votre programmation. Si vous choisissez de ne *pas* le prendre en compte, vous ne devrez changer aucun code si un type équivalent s’affiche dans la bibliothèque C++ standard.

Vous pouvez transmettre une liste d’initialiseurs à une API Windows Runtime qui attend un paramètre de collection. Prenons **StorageItemContentProperties::RetrievePropertiesAsync** comme exemple.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Vous pouvez appeler cette API avec une liste d’initialiseurs comme suit.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" });
}
```

Deux facteurs sont impliqués ici. Tout d’abord, l’appelé construit un **std::vector** à partir de la liste d’initialiseurs (cet appelé est asynchrone, donc il peut être-et doit être-le propriétaire de cet objet). Deuxièmement, C++/WinRT lie de manière transparente (et sans créer de copies) **std::vector** comme un paramètre de collection Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Vecteurs et tableaux standard
**array_view** a également des constructeurs de conversion à partir de **std::vector** et **std::array**.

```cppwinrt
template <typename C, size_type N> array_view(std::array<C, N>& value) noexcept
template <typename C> array_view(std::vector<C>& vectorValue) noexcept
```

Par conséquent, vous pouvez appeler à la place **DataWriter::WriteBytes** avec un **std::vector**.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to an array_view before being passed to WriteBytes.
```

Ou avec un **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to an array_view before being passed to WriteBytes.
```

C++/WinRT lie **std::vector** comme un paramètre de collection Windows Runtime. Par conséquent, vous pouvez transmettre un **std::vector&lt;winrt::hstring&gt;**, et il sera converti dans la collection Windows Runtime appropriée de **winrt::hstring**. Si l’appelé est asynchrone, vous devez copier ou déplacer le vecteur. Dans l’exemple de code ci-dessous, nous déplaçons la propriété du vecteur vers l’appelé asynchrone.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<winrt::hstring> const& vecH)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH));
}
```

Mais vous ne pouvez pas transmettre un **std::vector&lt;std::wstring&gt;** là où une collection Windows Runtime est attendue. Cela est dû au fait que, la collection Windows Runtime appropriée de **std::wstring** ayant été convertie, le langage C++ ne forcera pas le ou les paramètres de type de cette collection. Par conséquent, l’exemple de code suivant ne sera pas compilé.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)); // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Tableaux bruts et plages de pointeurs
En gardant en tête qu’un type équivalent peut exister dans l’avenir dans la bibliothèque C++ standard, vous pouvez également travailler directement avec **array_view**, par choix ou par nécessité.

**array_view** a des constructeurs de conversion à partir d’un tableau brut et d’une plage de **T*** (pointeurs vers le type d’élément).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Opérateurs et fonctions winrt::array_view
Une série de constructeurs, d’opérateurs, de fonctions et d’itérateurs sont implémentés pour **array_view**. Un **array_view** étant une plage, vous pouvez l’utiliser avec `for` basé sur les plages ou avec **std::for_each**.

Pour plus d’exemples et d’informations, consultez la rubrique sur les informations de référence sur les API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="important-apis"></a>API importantes
* [Modèle de structure winrt::array_view](/uwp/cpp-ref-for-winrt/array-view)
