---
author: stevewhims
description: C + / WinRT fournit des fonctions et des classes de base qui vous gagner du temps et d’efforts lorsque vous souhaitez implémenter et/ou de passer des collections.
title: Collections avec C + / WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c ++, RPC, winrt, projection, collection
ms.localizationpriority: medium
ms.openlocfilehash: dacfe4135402b85bac68b63c06f99f97001fa5b9
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2889326"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Collections avec [C + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

En interne, une collection Windows Runtime a un grand nombre d’éléments complexes. Mais lorsque vous souhaitez transmettre un objet de collection pour une fonction Windows Runtime, ou pour implémenter votre propre, collection-propriétés et les types de collection, il existe des fonctions et des classes de base dans C + / WinRT pour prendre en charge vous. Ces fonctionnalités Simplifiez vos mains et vous enregistrement une surcharge importante de temps et des efforts.

> [!IMPORTANT]
> Les fonctionnalités décrites dans cette rubrique sont disponibles que si vous avez installé le [Windows 10 SDK Preview Build 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)ou version ultérieure.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) est l’interface de Windows Runtime implémentée par n’importe quelle collection accès aléatoire des éléments. Si vous deviez implémenter **IVector** vous-même, vous devez également implémenter [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)et [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Même si vous *avez besoin* une collection personnalisée de type, qui est beaucoup de travail. Mais si vous avez données **std::vector** (ou **std::map**ou à une **std::unordered_map**) et vous souhaitez faire passer à une API d’exécution Windows, vous pouvez éviter d’effectuer ce niveau de travail, si possible. Et éviter qu’il *est* possible, étant donné que C + / WinRT vous aide à créer des collections efficacement, avec peu d’efforts.

## <a name="helper-functions-for-collections"></a>Fonctions d’assistance pour les collections

### <a name="general-purpose-collection-empty"></a>Collection d’ordre général, vide

Pour récupérer un objet d’un type qui implémente une collection d’ordre général, vous pouvez appeler le modèle de fonction [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) . L’objet est renvoyé sous la forme d’un [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_), et qui est l’interface par le biais de laquelle vous appelez les fonctions et les propriétés de l’objet renvoyé.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    init_apartment();

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

Comme vous pouvez le constater dans l’exemple ci-dessus, après avoir créé la collection vous pouvez ajouter des éléments, parcourir et traitent généralement l’objet comme vous le feriez pour n’importe quel objet collection Windows Runtime que vous avez peut-être reçu à partir d’une API. Si vous avez besoin d’une vue immuable sur la collection, vous pouvez appeler [IVector::GetView](/uwp/api/windows.foundation.collections.ivector-1.getview), comme indiqué. Le modèle illustré ci-dessus&mdash;de création et utilisation d’une collection&mdash;est appropriée pour les scénarios simples où vous souhaitez passer des données dans ou obtenir des données en dehors d’une API.

### <a name="general-purpose-collection-primed-from-data"></a>Collection d’ordre général, amorcée à partir des données

Vous pouvez également éviter la surcharge des appels sur **Append** que vous pouvez voir dans l’exemple de code ci-dessus. Est peut-être déjà la source de données, ou vous pouvez remplir avant la création de l’objet de la collection Windows Runtime. Voici comment procéder.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Vous pouvez transmettre un objet temporaire contenant vos données à **winrt::single_threaded_vector**, comme avec `coll1`, ci-dessus. Ou vous pouvez déplacer un **std::vector** (en supposant que vous n’y accéder à nouveau) dans la fonction. Dans les deux cas, vous transmettez une *valeur rvalue* dans la fonction. Ainsi, le compilateur pour gagner en efficacité et pour éviter la copie des données. Si vous souhaitez en savoir plus sur *rvalues*, voir [catégories de valeur et les références correspondantes](cpp-value-categories.md).

Si vous souhaitez lier un contrôle d’éléments XAML à votre collection, vous pouvez. Mais sachez que pour définir correctement la propriété [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , vous devez lui attribuer une valeur de type **IVector** de **IInspectable** (ou d’un type d’interopérabilité tels que [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Voici un exemple de code qui produit un type approprié pour la liaison à une collection et ajoute un élément à celui-ci.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

La collection ci-dessus *peut* être lié à un contrôle d’éléments XAML; mais n’est pas la collection peuvent être observés.

### <a name="observable-collection"></a>Collection visible

Pour récupérer un objet d’un type qui implémente une collection *peuvent être observés* , appelez le modèle de fonction [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) avec n’importe quel type d’élément. Mais pour transformer une collection visible pouvant être lié à un contrôle des éléments XAML, utilisez **IInspectable** comme type d’élément.

L’objet est renvoyé sous la forme d’un [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), et qui est l’interface par le biais de laquelle vous (ou le contrôle à laquelle elle est liée) appeler les fonctions et les propriétés de l’objet renvoyé.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Pour plus d’informations et les exemples de code, sur la liaison d’utilisateur de votre interface utilisateur de contrôles à une collection visible, voir [contrôles d’éléments XAML; lier C + / WinRT collection](binding-collection.md).

### <a name="associative-collection-map"></a>Collection associative (map)

Il existe des versions de collection associatif des deux fonctions que nous avons présenté.

- Le modèle de la fonction [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) retourne la collection non-peuvent être observés associative comme un [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- Le modèle de la fonction [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) renvoie une collection associative visible comme un [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Vous pouvez éventuellement prime ces collections avec les données en passant à la fonction une *rvalue* de type **std::map** ou **std::unordered_map**.

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

### <a name="single-threaded"></a>Un seul thread

Le «thread» dans les noms de ces fonctions indique qu’ils ne fournissent un accès concurrentiel&mdash;en d’autres termes, ils ne sont pas thread-safe. La mention de threads n’est pas liée à compartiments, étant donné que les objets retournés par ces fonctions sont toutes agiles (voir [objets Agile dans C + / WinRT](agile-objects.md)). Il est simplement que les objets sont le seul thread. Et qui est entièrement appropriée si vous voulez simplement transmettre des données ou l’autre sur l’interface binaire d’application (ABI).

## <a name="base-classes-for-collections"></a>Classes de base pour les collections

Si, pour une flexibilité complète, que vous souhaitez implémenter votre propre collection personnalisée, vous souhaiterez afin d’éviter toute que la méthode difficile. Par exemple, il s’agit d’une vue personnalisée qui se présente comme *sans l’aide de C + / classes de base du WinRT*.

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

Au lieu de cela, il est beaucoup plus facile de dériver votre vue personnalisée à partir du modèle de struct [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) et simplement implémenter la fonction **get_container** pour exposer le conteneur de vos données.

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

Le conteneur renvoyé par **get_container** doit fournir l’interface **begin** et **end** que **winrt::vector_view_base** attend. Comme indiqué dans l’exemple ci-dessus, **std::vector** prévoit que. Mais vous pouvez retourner n’importe quel conteneur qui remplit le même contrat, y compris votre propre conteneur personnalisé.

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

Il s’agit de la base de classes que C + / WinRT fournit pour vous aider à implémenter des collections personnalisées.

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
* [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [WinRT::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [WinRT::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [WinRT::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [WinRT::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Rubriques associées
* [Catégories de valeur et les références correspondantes](cpp-value-categories.md)
* [Contrôles d’éléments XAML; liaison à une collection C++/WinRT](binding-collection.md)
