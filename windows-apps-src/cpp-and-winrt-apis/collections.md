---
description: C++/WinRT fournit des fonctions et classes de base qui vous permettent d’économiser du temps et des efforts quand vous souhaitez implémenter et/ou transmettre des collections.
title: Collections avec C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, collection
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4f1b15ec377b030a467dded634abe3fdde717896
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270154"
---
# <a name="collections-with-cwinrt"></a>Collections avec C++/WinRT

En interne, une collection Windows Runtime comporte un grand nombre d’éléments mobiles compliqués. Mais si vous souhaitez passer un objet de collection à une fonction Windows Runtime ou implémenter vos propres propriétés et types de collection, il existe des fonctions et classes de base [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) pour vous aider. Ces fonctionnalités vous simplifient la tâche et vous font gagner économiser beaucoup de temps et d’efforts.

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_) est l’interface Windows Runtime implémentée par n’importe quelle collection d’éléments à accès aléatoire. Si vous deviez implémenter **IVector** vous-même, vous devriez également implémenter [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_) et [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Même si vous *avez besoin* d’un type de collection personnalisé, cela représente beaucoup de travail. Mais si vos données sont stockées dans un **std::vector** (un **std::map** ou un **std::unordered_map**) et que vous souhaitez simplement passer ces données à une API Windows Runtime, vous devez éviter si possible une charge de travail aussi importante. Et cela *est* possible car C++/WinRT vous aide à créer des collections efficacement et sans effort.

Consultez également [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](binding-collection.md).

## <a name="helper-functions-for-collections"></a>Fonctions d’assistance pour les collections

### <a name="general-purpose-collection-empty"></a>Collection à usage général, vide

Cette section décrit le scénario dans lequel vous souhaitez créer une collection initialement vide, puis la remplir *après* sa création.

Pour récupérer un nouvel objet d’un type qui implémente une collection à usage général, vous pouvez appeler le modèle de fonction [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector). L’objet est retourné comme un [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_), et il s’agit de l’interface via laquelle vous appelez les fonctions et les propriétés de l’objet retourné.

Si vous souhaitez copier-coller les exemples de code suivants directement dans le fichier de code source principal d’un projet **Application console Windows (C++/WinRT)** , définissez d’abord **Sans utiliser les en-têtes précompilés** dans les propriétés du projet.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

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

Comme vous pouvez le voir dans l’exemple de code ci-dessus, après avoir créé la collection, vous pouvez ajouter des éléments, effectuer une itération sur ces éléments, et généralement traiter l’objet comme vous le feriez pour n’importe quel objet de collection Windows Runtime reçu à partir d’une API. Si vous avez besoin d’une vue immuable sur la collection, vous pouvez appeler [ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), comme indiqué. Le modèle &mdash;de création et d’utilisation d’une collection&mdash; ci-dessus est convient aux scénarios simples où vous souhaitez passer des données à une API, ou obtenir des données d’une API. Vous pouvez passer un **IVector** ou un **IVectorView** n’importe où un [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_) est attendu.

Dans l’exemple de code ci-dessus, l’appel à **winrt::init_apartment** initialise le thread dans Windows Runtime, notamment dans un multithread cloisonné par défaut. L’appel initialise également COM.

### <a name="general-purpose-collection-primed-from-data"></a>Collection à usage général, amorcée à partir des données

Cette section décrit le scénario dans lequel vous souhaitez créer une collection et le remplir en même temps.

Vous pouvez éviter une surcharge des appels à **Append** dans l’exemple de code précédent. Vous disposez peut-être déjà des données source, ou vous préférez remplir les données source avant de créer l’objet de collection Windows Runtime. Voici comment procéder.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Vous pouvez passer un objet temporaire contenant vos données à **winrt::single_threaded_vector**, comme avec `coll1`, ci-dessus. Ou vous pouvez déplacer un **std::vector** (en supposant que vous n’y accéderez plus) dans la fonction. Dans les deux cas, vous passez un *rvalue* dans la fonction. Cela garantit l’efficacité du compilateur et éviter la copie des données. Pour en savoir plus sur *rvalues*, consultez [Catégories de valeurs et références](cpp-value-categories.md).

Si vous le souhaitez, vous pouvez lier un contrôle d’éléments XAML à votre collection. Sachez toutefois que pour définir correctement la [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), vous devez lui attribuer une valeur de type **IVector** de **IInspectable** (ou d’un type d’interopérabilité comme [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)).

Voici un exemple de code qui produit une collection d’un type pouvant être lié et y ajoute un élément. Vous trouverez le contexte de cet exemple de code dans [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](binding-collection.md).

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Vous pouvez créer une collection Windows Runtime à partir de données et obtenir une vue pour la passer à une API. Tout cela, sans copie quoi que ce soit.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

Dans les exemples ci-dessus, la collection que nous créons *peut* être liée à un contrôle d’éléments XAML, mais la collection n’est pas observable.

### <a name="observable-collection"></a>Collection observable

Pour récupérer un nouvel objet d’un type qui implémente une collection *observable*, appelez le modèle de fonction [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) avec n’importe quel type d’élément. Mais pour pouvoir lier une collection observable à un contrôle d’éléments XAML, utilisez **IInspectable** en tant que type d’élément.

L’objet est retourné comme un [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), et il s’agit de l’interface (ou le contrôle auquel il est lié) vous permettant d’appeler les fonctions et les propriétés de l’objet retourné.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Pour obtenir des exemples de code et plus d’informations sur la liaison de vos contrôles d’interface utilisateur à une collection observable, consultez [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](binding-collection.md).

### <a name="associative-collection-map"></a>Collection associative (carte)

Il existe des versions de collections associatives des deux fonctions que nous avons examinées.

- Le modèle de fonction [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) retourne une collection associative non observable en tant que [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- Le modèle de fonction [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) retourne une collection associative observable en tant que [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Vous pouvez éventuellement amorcer ces collections avec des données en passant à la fonction un *rvalue* de type **std::map** ou **std::unordered_map**.

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

### <a name="single-threaded"></a>Single-threaded

La mention « single-threaded » dans les noms de ces fonctions indique qu’elles ne fournissent aucun accès concurrentiel&mdash;en d’autres termes, elles ne sont pas thread-safe. La mention des threads n’est pas liée aux cloisonnements car les objets retournés par ces fonctions sont tous agiles (voir [Objets agiles en C++/WinRT](agile-objects.md)). Ces objets sont simplement de type single-threaded. Et cela ne pose aucun problème si vous souhaitez simplement passer des données d’une façon ou d’une autre via l’interface binaire de l’application (ABI).

## <a name="base-classes-for-collections"></a>Classes de base pour les collections

Si, pour une flexibilité complète, vous souhaitez implémenter votre propre collection personnalisée, vous devez procéder de la meilleure façon. Par exemple, voici à quoi ressemblerait une vue de vecteur personnalisée *sans classes de base C++/WinRT*.

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

Il est beaucoup plus facile de dériver votre vue de vecteur personnalisée du modèle de structure [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base), puis d’implémenter simplement la fonction **get_container** pour exposer le conteneur contenant vos données.

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

Le conteneur retourné par **get_container** doit fournir l’interface de **début** et de **fin** attendue par **winrt::vector_view_base**. Comme indiqué dans l’exemple ci-dessus, **std::vector** fournit ces éléments. Mais vous pouvez retourner n’importe quel conteneur qui remplit la même fonction, y compris votre propre conteneur personnalisé.

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

Telles sont les classes de base fournies par C++/WinRT pour vous aider à implémenter des collections personnalisées.

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
* [Propriété ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [Interface IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interface IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [Modèle de structure winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [Modèle de structure winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [Modèle de structure winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [Modèle de structure winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [Modèle de fonction winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [Modèle de fonction winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [Modèle de fonction winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [Modèle de fonction winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [Modèle de structure winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [Modèle de structure winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Rubriques connexes
* [Catégories de valeurs et références à celles-ci](cpp-value-categories.md)
* [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](binding-collection.md)
