---
description: C++ / c++ / WinRT fournit des fonctions et classes de base qui vous faire gagner beaucoup de temps et d’efforts lorsque vous souhaitez implémenter et/ou de passer des collections.
title: Collections avec C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, collection
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a50ab5f70faa0c8f8b73eada38444bcafd444d8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635434"
---
# <a name="collections-with-cwinrt"></a>Collections avec C++/WinRT

En interne, une collection de Windows Runtime comporte un grand nombre d’éléments mobiles compliqués. Mais lorsque vous souhaitez passer un objet de collection à une fonction Windows Runtime, ou pour implémenter vos propres propriétés de collection et les types de collection, il existe des fonctions et classes de base dans [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) pour prendre en charge de vous. Ces fonctionnalités simplifient la vos mains et vous faire gagner beaucoup de surcharge de temps et d’efforts.

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_) est l’interface Windows Runtime implémentée par n’importe quel regroupement d’accès aléatoire d’éléments. Si vous deviez implémenter **IVector** vous-même, vous devez également implémenter [ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [ **IVectorView** ](/uwp/api/windows.foundation.collections.ivectorview_t_), et [ **IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Même si vous *devez* un type de collection personnalisé, qui est beaucoup de travail. Mais si vous avez des données un **std::vector** (ou un **std::map**, ou un **std::unordered_map**) et vous souhaitez simplement est passe à une API de Runtime Windows, alors que vous souhaitez éviter cette opération Ce niveau de travail, si possible. Et de les éviter *est* possible, étant donné que C++ / c++ / WinRT vous aide à créer des regroupements efficacement et sans effort.

Consultez également [XAML des éléments des contrôles ; lier C + c++ / WinRT collection](binding-collection.md).

> [!NOTE]
> Si vous n’avez pas installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809) ou version ultérieure, vous n’aurez accès aux fonctions et classes de base qui sont documentées dans cette rubrique. Au lieu de cela, consultez [si vous disposez d’une version antérieure du SDK Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) pour obtenir la liste d’un modèle de vecteur observable que vous pouvez utiliser à la place.

## <a name="helper-functions-for-collections"></a>Fonctions d’assistance pour les collections

### <a name="general-purpose-collection-empty"></a>Collection à usage général, vide

Cette section décrit le scénario dans lequel vous souhaitez créer une collection qui est initialement vide. puis le remplit *après* la création.

Pour récupérer un nouvel objet d’un type qui implémente une collection à usage général, vous pouvez appeler la [ **winrt::single_threaded_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-vector) modèle de fonction. L’objet est retourné comme un [ **IVector**](/uwp/api/windows.foundation.collections.ivector_t_), et qui est l’interface via laquelle vous appelez des fonctions et les propriétés de l’objet retourné.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

Comme vous pouvez le voir dans l’exemple de code ci-dessus, après avoir créé la collection vous pouvez ajouter des éléments, effectuer une itération sur les et généralement traiter l’objet comme vous le feriez pour n’importe quel objet de collection de Windows Runtime que vous avez peut-être reçu à partir d’une API. Si vous avez besoin d’une vue immuable sur la collection, vous pouvez appeler [ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), comme indiqué. Le modèle ci-dessus&mdash;de création et utilisation d’une collection&mdash;est appropriée pour les scénarios simples où vous souhaitez passer des données, ou obtenir des données en dehors d’une API. Vous pouvez passer un **IVector**, ou un **IVectorView**, n’importe où un [ **IIterable** ](/uwp/api/windows.foundation.collections.iiterable_t_) est attendu.

Dans l’exemple de code ci-dessus, l’appel à **winrt::init_apartment** initialise COM ; par défaut, dans un multithread cloisonné.

### <a name="general-purpose-collection-primed-from-data"></a>Collection à usage général, amorcée à partir des données

Cette section décrit le scénario dans lequel vous souhaitez créer une collection et le remplir en même temps.

Vous pouvez éviter la surcharge des appels à **Append** dans l’exemple de code précédent. Vous disposez peut-être déjà la source de données, ou vous pouvez remplir les données source avant de créer la collection d’objets Windows Runtime. Voici comment procéder.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Vous pouvez passer un objet temporaire contenant vos données à **winrt::single_threaded_vector**, comme avec `coll1`, ci-dessus. Ou vous pouvez déplacer un **std::vector** (en supposant que vous n’y accéder à nouveau) dans la fonction. Dans les deux cas, vous transmettez un *rvalue* dans la fonction. Qui permet au compilateur d’être efficaces et pour éviter de copier les données. Si vous souhaitez en savoir plus sur *rvalues*, consultez [catégories de valeur et des références pour les](cpp-value-categories.md).

Si vous souhaitez lier un contrôle d’éléments XAML à votre collection, vous pouvez ensuite. Sachez toutefois que pour définir correctement la [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) propriété, vous devez lui attribuer une valeur de type **IVector** de **IInspectable** (ou d’un type d’interopérabilité comme [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Voici un exemple de code qui produit une collection d’un type pouvant être lié et y ajoute un élément.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Vous pouvez créer une collection de Windows Runtime à partir des données et obtenir une vue dessus prêt à passer à une API, tout cela sans copie quoi que ce soit.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

Dans les exemples ci-dessus, la collection que nous créons *pouvez* être lié à un XAML éléments contrôle ; mais n’est pas de la collection observable.

### <a name="observable-collection"></a>Collection observable

Pour récupérer un nouvel objet d’un type qui implémente un *observable* collection, appelez le [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) modèle de fonction avec n’importe quel type d’élément. Mais pour rendre un ensemble observable pouvant être lié à un contrôle d’éléments XAML, utilisez **IInspectable** en tant que le type d’élément.

L’objet est retourné comme un [ **IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), et qui est l’interface via laquelle vous (ou le contrôle auquel il est lié) appeler les fonctions et les propriétés de l’objet retourné.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Pour plus d’informations et d’exemples de code, à propos de la liaison de votre utilisateur (IU) contrôles d’interface à une collection observable, consultez [XAML des éléments des contrôles ; lier C + c++ / WinRT collection](binding-collection.md).

### <a name="associative-collection-map"></a>Collection associative (map)

Il existe des versions de collection associative des deux fonctions que nous avons examiné.

- Le [ **winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map) modèle de fonction retourne une collection associative non observable comme un [ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- Le [ **winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) modèle de fonction retourne une collection associative observable comme un [ **IObservableMap** ](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Vous pouvez éventuellement prime ces collections avec des données en passant à la fonction un *rvalue* de type **std::map** ou **std::unordered_map**.

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>Monothread

Le « single-threaded » dans les noms de ces fonctions indique qu’ils ne fournissent pas toutes d’accès concurrentiel&mdash;en d’autres termes, ils ne sont pas thread-safe. La mention de threads n’est pas liée à cloisonnés (STA), étant donné que les objets retournés par ces fonctions sont toutes agiles (voir [objets Agile en C / c++ / WinRT](agile-objects.md)). Il est simplement que les objets sont à thread unique. Et c’est tout à fait approprié si vous souhaitez simplement passer un moyen de données ou l’autre à travers l’interface binaire d’application (ABI).

## <a name="base-classes-for-collections"></a>Classes de base pour les collections

Si, pour une flexibilité complète, que vous souhaitez implémenter votre propre collection personnalisée, vous souhaiterez afin d’éviter toute qui dépens. Par exemple, c’est quoi ressemblerait une vue personnalisée de vecteur *sans l’aide de C / c++ / classes de base de WinRT*.

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

Au lieu de cela, il est beaucoup plus facile de dériver votre vue personnalisée de vecteur dans le [ **winrt::vector_view_base** ](/uwp/cpp-ref-for-winrt/vector-view-base) modèle struct et implémenter simplement le **get_container** à fonction exposer le conteneur contenant vos données.

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

Le conteneur retourné par **get_container** doit fournir le **commencer** et **fin** interface qui **winrt::vector_view_base** attend. Comme indiqué dans l’exemple ci-dessus, **std::vector** qui fournit. Mais vous pouvez revenir à n’importe quel conteneur qui remplit le même contrat, y compris votre propre conteneur personnalisé.

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

Il s’agit de la base de classes que C++ / c++ / WinRT fournit pour vous aider à implémenter des collections personnalisées.

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Consultez les exemples de code ci-dessus.

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>API importantes
* [Propriété de ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [Interface de IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interface IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base struct template](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base struct template](/uwp/cpp-ref-for-winrt/map-view-base)
* [modèle de struct WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [modèle de struct WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [modèle de fonction WinRT::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [modèle de fonction WinRT::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [modèle de fonction WinRT::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [modèle de fonction WinRT::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base struct template](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base struct template](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Rubriques connexes
* [Catégories de valeur et les références à ceux-ci](cpp-value-categories.md)
* [XAML des éléments des contrôles ; lier à C++ / c++ / WinRT collection](binding-collection.md)
