---
description: C++/ WinRT simplifie le passage de paramètres à la frontière ABI en fournissant des conversions automatiques pour les cas courants.
title: Passage de paramètres à la frontière ABI
ms.date: 07/10/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, passer, paramètres, ABI
ms.localizationpriority: medium
ms.openlocfilehash: c1e172fc4dbd5b865add1828a98dc1a030d5dc6f
ms.sourcegitcommit: 8b4c1fdfef21925d372287901ab33441068e1a80
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844351"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>Passage de paramètres à la frontière ABI

Étant donné que les types se trouvent dans l’espace de noms **winrt::param**, C++/ WinRT simplifie le passage de paramètres à la frontière ABI en fournissant des conversions automatiques pour les cas courants. Pour obtenir plus de détails et des exemples de code, consultez [Gestion des chaînes](/windows/uwp/cpp-and-winrt-apis/strings) et [Types de données C++ standard et C++/WinRT](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types).

> [!IMPORTANT]
> Vous ne devez pas utiliser vous-même les types de l’espace de noms **winrt::param**. Ils sont au profit de la projection.

De nombreux types sont disponibles dans les versions synchrone et asynchrone. C++/WinRT utilise la version synchrone quand vous passez un paramètre à une méthode synchrone. Il utilise la version asynchrone quand vous passez un paramètre à une méthode asynchrone. La version asynchrone prend des mesures supplémentaires afin qu’il soit plus difficile pour l’appelant de muter la collection avant la fin de l’opération. Notez cependant qu’aucune des variantes ne protège contre la mutation de la collection à partir d’un autre thread. Il est de votre responsabilité d’empêcher cela.

## <a name="string-parameters"></a>Paramètres de chaîne

**winrt::param::hstring** simplifie le passage de paramètres aux API acceptant **HSTRING**.

|Types que vous pouvez passer|Remarques|
|-|-|
|`{}`|Passe une chaîne vide.|
|**winrt::hstring**||
|**std::wstring_view**|Pour les littéraux, vous pouvez écrire `L"Name"sv`. L’affichage doit avoir un terminateur null à la fin.|
|**std::wstring**|-|
|**wchar_t const\***|Chaîne se terminant par une valeur null.|

`nullptr` n’est pas autorisé. Utilisez `{}` à la place.

Le compilateur sait comment évaluer `wcslen` sur les littéraux de chaîne au moment de la compilation. Pour les littéraux, `L"Name"sv` et `L"Name"` sont donc équivalents.

Notez que les objets **std::wstring_view** ne se terminent pas par une valeur null. Toutefois, C++/WinRT nécessite que le caractère situé après la chaîne soit une valeur null. Si vous passez un **std::wstring_view** qui ne se termine pas par une valeur null, le processus s’arrête.

## <a name="iterable-parameters"></a>Paramètres pouvant être itérés

**winrt::param::iterable\<T\>** et **winrt::param::async_iterable\<T\>** simplifient le passage de paramètres aux API qui acceptent **IIterable\<T\>** .

Les collections Windows Runtime prennent déjà en charge **IIterable**.

|Types que vous pouvez passer|Sync|Async|Remarques|
|-|-|-|-|
| `nullptr` | Oui | Oui | Vous devez vérifier que la méthode sous-jacente prend en charge `nullptr`.|
| **IIterable\<T\>** | Oui | Oui | Ou tout élément convertible.|
| **std::vector\<T\> const&** | Oui | Non ||
| **std::vector\<T\>&&** | Oui | Oui | Le contenu est déplacé dans l’itérateur pour éviter la mutation.|
| **std::initializer_list\<T\>** | Oui | Oui | La version asynchrone copie les éléments.|
| **std::initializer_list\<U\>** | Oui | Non | **U** doit être convertible en **T**.|
| `{ ForwardIt begin, ForwardIt end }` | Oui | Non | `*begin` doit être convertible en **T**.|

Notez que **IIterable\<U\>** et **std::vector\<U\>** ne sont pas autorisés, même si **U** est convertible en **T**. Pour **std::vector\<U\>** , vous pouvez utiliser la version à double itérateur (plus de détails ci-dessous).

Dans certains cas, l’objet que vous avez peut implémenter l’**IIterable** que vous souhaitez. Par exemple, l’**IVectorView\<StorageFile\>** produit par [**FileOpenPicker.PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) implémente **IIterable<StorageFile>** . Mais il implémente également **IIterable<IStorageItem>**  ; il vous suffit de le demander explicitement.

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

Dans d’autres cas, vous pouvez utiliser la version à double itérateur.

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

Le double-itérateur fonctionne généralement dans les cas où vous avez une collection qui ne correspond à aucun des scénarios ci-dessus, à condition toutefois que vous puissiez l’itérer et produire des éléments pouvant être convertis en **T**. Nous l’avons utilisé ci-dessus pour itérer un vecteur de types dérivés. Ici, nous l’utilisons pour itérer un non-vecteur de types dérivés.

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

L’implémentation d’[**IIterator\<T\>.GetMany(T\[\])** ](/uwp/api/windows.foundation.collections.iiterator-1.getmany) est plus efficace si l’itérateur est un `RandomAcessIt`. Sinon, il effectue plusieurs passes sur la plage.

|Types que vous pouvez passer|Sync|Async|Remarques|
|-|-|-|-|
| `nullptr` | Oui | Oui | Vous devez vérifier que la méthode sous-jacente prend en charge `nullptr`.|
| **IIterable\<IKeyValuePair\<K, V\>\>** | Oui | Oui | Ou tout élément convertible.|
| **std::map\<K, V\> const&** | Oui | Non ||
| **std::map\<K, V\>&&** | Oui | Oui | Le contenu est déplacé dans l’itérateur pour éviter la mutation.|
| **std::unordered_map\<K, V\> const&** | Oui | Non ||
| **std::unordered_map\<K, V\>&&** | Oui | Oui | Le contenu est déplacé dans l’itérateur pour éviter la mutation.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Oui | Oui | Les types **K** et **V** doivent correspondre exactement. Les clés ne peuvent pas être dupliquées. La version asynchrone copie les éléments.|
| `{ ForwardIt begin, ForwardIt end }` | Oui | Non | `begin->first` et `begin->second` doivent être convertibles en **K** et **V**, respectivement.|

## <a name="vector-view-parameters"></a>Paramètres d’affichage de vecteur

**winrt::param::vector_view\<T\>** et **winrt::param::async_vector_view\<T\>** simplifient le passage de paramètres aux API qui acceptent **IVectorView\<T\>** .

Vous pouvez utiliser [**IVector\<T\>.GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) pour obtenir un **IVectorView** à partir d’un **IVector**.

|Types que vous pouvez passer|Sync|Async|Remarques|
|-|-|-|-|
| `nullptr` | Oui | Oui | Vous devez vérifier que la méthode sous-jacente prend en charge `nullptr`.|
| **IVectorView\<T\>** | Oui | Oui | Ou tout élément convertible.|
| **std::vector\<T\>const&** | Oui | Non ||
| **std::vector\<T\>&&** | Oui | Oui | Le contenu est déplacé dans l’affichage pour éviter la mutation.|
| **std::initializer_list\<T\>** | Oui | Oui | Le type doit correspondre exactement. La version asynchrone copie les éléments.|
| `{ ForwardIt begin, ForwardIt end }` | Oui | Non | `*begin` doit être convertible en **T**.|

La version à double itérateur peut être utilisée pour créer des affichages de vecteur à partir d’éléments ne répondant pas aux critères requis pour être passés directement. Toutefois, étant donné que les vecteurs prennent en charge l’accès aléatoire, nous vous recommandons de passer un `RandomAcessIter`.

## <a name="map-view-parameters"></a>Paramètres d’affichage de carte

**winrt::param::map_view\<T\>** et **winrt::param::async_map_view\<T\>** simplifient le passage de paramètres aux API qui acceptent **IMapView\<T\>** .

Vous pouvez utiliser **IMap::GetView** pour obtenir un **IMapView** à partir d’un **IMap**.

|Types que vous pouvez passer|Sync|Async|Remarques|
|-|-|-|-|
| `nullptr` | Oui | Oui | Vous devez vérifier que la méthode sous-jacente prend en charge `nullptr`.|
| **IMapView\<K, V\>** | Oui | Oui | Ou tout élément convertible.|
| **std::map\<K, V\> const&** | Oui | Non ||
| **std::map\<K, V\>&&** | Oui | Oui | Le contenu est déplacé dans l’affichage pour éviter la mutation.|
| **std::unordered_map\<K, V\> const&**  | Oui | Non ||
| **std::unordered_map\<K, V\>&&** | Oui | Oui | Le contenu est déplacé dans l’affichage pour éviter la mutation.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Oui | Oui | Les versions synchrones et asynchrones copient les éléments. Vous ne pouvez pas dupliquer des clés.|

## <a name="vector-parameters"></a>Paramètres de vecteur

**winrt::param::vector\<T\>** simplifie le passage de paramètres aux API acceptant **IVector\<T\>** .

|Types que vous pouvez passer|Remarques|
|-|-|
| `nullptr` | Vous devez vérifier que la méthode sous-jacente prend en charge `nullptr`.|
| **IVector\<T\>** | Ou tout élément convertible.|
| **std::vector\<T\>&&** | Le contenu est déplacé dans le paramètre pour éviter la mutation. Les résultats ne sont pas redéplacés.|
| **std::initializer_list\<T\>** | Le contenu est copié dans le paramètre pour éviter la mutation.|

Si la méthode mute le vecteur, la seule façon d’observer la mutation consiste à passer un **IVector** directement. Si vous passez un **std::vector**, la méthode mute la copie et non l’original.

## <a name="map-parameters"></a>Paramètre de carte

**winrt::param::map\<T\>** simplifie le passage de paramètres aux API acceptant **IMap\<T\>** .

|Types que vous pouvez passer|Remarques|
|-|-|
| `nullptr` | Vous devez vérifier que la méthode sous-jacente prend en charge `nullptr`.|
| **IMap\<T\>** | Ou tout élément convertible.|
| **std::map\<K, V\>&&** | Le contenu est déplacé dans le paramètre pour éviter la mutation. Les résultats ne sont pas redéplacés.|
| **std::unordered_map\<K, V\>&&** | Le contenu est déplacé dans le paramètre pour éviter la mutation. Les résultats ne sont pas redéplacés.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Le contenu est copié dans le paramètre pour éviter la mutation.|

Si la méthode mute la carte, la seule façon d’observer la mutation consiste à passer un **IVector** directement. Si vous passez **std::map** ou **std::unordered_map**, la méthode mute la copie et non l’original.

## <a name="array-parameters"></a>Paramètres de tableau

**winrt::array_view\<T\>** n’est pas dans l’espace de noms **winrt::param**, mais il est utilisé pour les paramètres qui sont des tableaux de style C (également appelés *tableaux conformes*).

|Types que vous pouvez passer|Remarques|
|-|-|
| `{}` | Tableau vide.|
| **array** | Tableau conforme de C (autrement dit, `C array[N];`), où **C** est convertible en **T** et `sizeof(C) == sizeof(T)`. |
| **std::array<C, N>** | **std::array** C++ de **C**, où **C** est convertible en **T** et `sizeof(C) == sizeof(T)`. |
| **std::vector<C>** | **std::vector** C++ de **C**, où **C** est convertible en **T** et `sizeof(C) == sizeof(T)`. |
| `{ T*, T* }` | Paire de pointeurs représentent la plage [début, fin).|
| **std::initializer_list\<T\>** ||