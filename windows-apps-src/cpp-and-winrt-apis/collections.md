---
author: stevewhims
description: C++ / WinRT fournit des fonctions et des classes de base qui vous faire gagner beaucoup de temps et d’efforts lorsque vous souhaitez implémenter ou passer des collections.
title: Collections avec C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, collection
ms.localizationpriority: medium
ms.openlocfilehash: e6a0cf8c2798adc59ffcf84381d6bbf64f2ce80e
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4416609"
---
# <a name="collections-with-cwinrt"></a>Collections avec C++/WinRT

En interne, une collection Windows Runtime comporte un grand nombre d’éléments mobiles compliquées. Toutefois, lorsque vous souhaitez transmettre une collection d’objets à une fonction Windows Runtime, ou pour implémenter vos propres propriétés de collection et les types de collection, il existe des fonctions et des classes de base dans [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) pour prendre en charge vous. Ces fonctionnalités prennent la complexité en dehors de vos mains et vous faire gagner un grand nombre de la surcharge de temps et d’efforts.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) est l’interface Windows Runtime implémentée par n’importe quelle collection à accès aléatoire des éléments. Si vous étiez à implémenter **IVector** vous-même, vous devez également implémenter [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)et [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Même si vous *avez besoin* une collection personnalisée de type, qui est un grand nombre de travail. Toutefois, si vous avez des données dans un **std::vector** (ou un **std::map**ou un **std::unordered_map**) et tout ce que vous voulez faire est transmettre à une API Windows Runtime, puis vous pouvez éviter d’exécuter ce niveau de travail, si possible. Et en évitant qu’il *est* possible, étant donné que C++ / WinRT vous aide à créer des collections efficacement et avec peu d’effort.

Consultez également [contrôles d’éléments XAML; liaison à C++ / WinRT collection](binding-collection.md).

> [!NOTE]
> Si vous n’avez pas encore installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809) ou une version ultérieure, puis vous n’avez pas accès à des fonctions et des classes de base qui sont documentés dans cette rubrique. Voir à la place, [Si vous disposez d’une version antérieure du SDK Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) pour obtenir la liste d’un modèle de vecteur observable que vous pouvez utiliser à la place.

## <a name="helper-functions-for-collections"></a>Fonctions d’assistance pour les collections

### <a name="general-purpose-collection-empty"></a>Collection à usage général, vide

Cette section décrit le scénario dans lequel vous souhaitez créer une collection qui est initialement vide; et ensuite remplir *après* sa création.

Pour récupérer un nouvel objet d’un type qui implémente une collection à usage général, vous pouvez appeler le modèle de fonction [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) . L’objet est retourné comme une [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_), et c’est l’interface par lequel vous appelez des fonctions et des propriétés de l’objet retourné.

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

Comme vous pouvez le voir dans l’exemple de code ci-dessus, après avoir créé la collection vous pouvez ajouter des éléments, itérer sur les et traitent généralement l’objet comme vous le feriez pour n’importe quel objet de collection Windows Runtime que vous avez peut-être reçu à partir d’une API. Si vous avez besoin d’une vue immuable au-dessus de la collection, vous pouvez appeler [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), comme indiqué. Le modèle présenté ci-dessus&mdash;de création et l’utilisation d’une collection&mdash;est appropriée pour les scénarios simples où vous souhaitez passer des données dans ou à recevoir des données en dehors d’une API. Vous pouvez transmettre un **IVector**ou un **IVectorView**, n’importe où un [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_) est attendu.

Dans l’exemple de code ci-dessus, l’appel à **winrt::init_apartment** initialise COM; par défaut, un multithread cloisonné.

### <a name="general-purpose-collection-primed-from-data"></a>Collection à usage général, amorcée à partir des données

Cette section décrit le scénario dans lequel vous souhaitez créer une collection et à le remplir en même temps.

Vous pouvez éviter la surcharge liée à des appels à **Ajouter** dans l’exemple de code précédent. Vous disposez peut-être déjà la source de données, ou vous pouvez préférer remplir la source de données avant la création de l’objet de collection Windows Runtime. Voici comment procéder.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Vous pouvez transmettre un objet temporaire contenant vos données à **winrt::single_threaded_vector**, comme avec `coll1`ci-dessus. Ou vous pouvez déplacer un **std::vector** (en supposant que vous n’y accéder à nouveau) dans la fonction. Dans les deux cas, vous êtes en passant une *rvalue* dans la fonction. Qui permet au compilateur être efficace et éviter la copie des données. Si vous souhaitez en savoir plus sur *rvalues*, consultez [catégories de valeur et références pour eux](cpp-value-categories.md).

Si vous souhaitez lier un contrôle d’éléments XAML à votre collection, vous pouvez ensuite. Mais n’oubliez pas que pour définir correctement la propriété [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , vous devez lui affecter une valeur de type **IVector** de **IInspectable** (ou d’un type d’interopérabilité tels que [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Voici un exemple de code qui produit une collection d’un type approprié pour la liaison et ajoute un élément à celui-ci.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Vous pouvez créer une collection Windows Runtime à partir des données et obtenir une vue dessus prêt à transmettre à une API, sans quoi que ce soit copie.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

Dans les exemples ci-dessus, la collection nous créons *peut* être liée à un contrôle d’éléments XAML; mais n’est pas la collection observable.

### <a name="observable-collection"></a>Collection observable

Pour récupérer un nouvel objet d’un type qui implémente une collection *observable* , appelez le modèle de fonction [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) avec n’importe quel type d’élément. Toutefois, pour rendre une collection observable approprié pour la liaison à un contrôle d’éléments XAML, utilisez **IInspectable** comme type d’élément.

L’objet est retourné comme une [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), et c’est l’interface par l’intermédiaire de laquelle vous (ou le contrôle à laquelle elle est liée) appeler fonctions et les propriétés de l’objet retourné.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Pour plus d’informations et des exemples de code, sur la liaison de votre utilisateur (UI) contrôles d’interface à une collection observable, voir [contrôles d’éléments XAML; liaison à C++ / WinRT collection](binding-collection.md).

### <a name="associative-collection-map"></a>Collection associatif (mapper)

Il existe des versions de collection associatifs des deux fonctions que nous avons examinées.

- Le modèle de fonction [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) renvoie une collection d’associatif non observables en tant qu’un [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- Le modèle de fonction [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) renvoie une collection observable associatif comme un [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Vous pouvez éventuellement prime ces collections avec des données en transmettant à la fonction une *rvalue* de type **std::map** ou **std::unordered_map**.

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

### <a name="single-threaded"></a>Thread unique

Le «single-threaded» dans les noms de ces fonctions indique qu’ils ne fournissent pas n’importe quel concurrency&mdash;en d’autres termes, ils ne sont pas thread-safe. La mention de threads n’est pas liée à compartiments, dans la mesure où les objets renvoyées à partir de ces fonctions sont tous les agiles (voir [des objets agiles en C++ / WinRT](agile-objects.md)). Il est simplement que les objets sont à thread unique. Et c’est tout à fait approprié si vous souhaitez simplement transmettre des données d’une façon ou l’autre sur l’interface binaire d’application (ABI).

## <a name="base-classes-for-collections"></a>Classes de base pour les collections

Si, pour une flexibilité complète, vous souhaitez implémenter votre propre collection personnalisée, vous devrez éviter d’exécuter qui dépens. Par exemple, voici comment se présenterait une vue personnalisée vecteur *sans l’aide de C++ / classes de base de WinRT*.

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

Au lieu de cela, il est beaucoup plus facile de dériver de votre affichage vectoriel personnalisée à partir du modèle de structure [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) et implémenter simplement la fonction **get_container** pour exposer le conteneur contenant vos données.

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

Le conteneur renvoyé par **get_container** doit fournir à l’interface de **début** et **fin** que **winrt::vector_view_base** attend. Comme indiqué dans l’exemple ci-dessus, **std::vector** prévoit que. Toutefois, vous pouvez revenir à n’importe quel conteneur qui répond le même contrat, y compris votre propre conteneur personnalisé.

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

Il s’agit de la base de classes que C++ / WinRT fournit pour vous aider à implémenter des collections personnalisées.

### [<a name="winrtvectorviewbase"></a>WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Consultez les exemples de code ci-dessus.

### [<a name="winrtvectorbase"></a>WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### [<a name="winrtobservablevectorbase"></a>WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### [<a name="winrtmapviewbase"></a>WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### [<a name="winrtmapbase"></a>WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### [<a name="winrtobservablemapbase"></a>WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [Propriété ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [Interface IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interface IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [modèle de structure WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [modèle de structure WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [modèle de structure WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [modèle de structure WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [modèle de fonction WinRT::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [modèle de fonction WinRT::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [modèle de fonction WinRT::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [modèle de fonction WinRT::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [modèle de structure WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [modèle de structure WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Rubriques associées
* [Les catégories de valeur et des références associées](cpp-value-categories.md)
* [Contrôles d’éléments XAML; liaison avec une collection C++/WinRT](binding-collection.md)
