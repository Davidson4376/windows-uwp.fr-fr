---
author: stevewhims
description: Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée «collection *observable*». Cette rubrique montre comment implémenter et utiliser une collection observable, et comment y lier un contrôle d’éléments XAML.
title: Contrôles d’éléments XAML; liaison à une collection C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, liaison, collection
ms.localizationpriority: medium
ms.openlocfilehash: 9ba935b1a5316c2d7af9c7681705595efea7ca08
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4149078"
---
# <a name="xaml-items-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-collection"></a>Contrôles d’éléments XAML; liaison à une collection [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée «collection *observable*». Ce concept est basé sur le modèle de conception logicielle appelé «*modèle observateur*». Cette rubrique montre comment implémenter des collections observables en C++/WinRT et y lier des contrôles d’éléments XAML.

Cette procédure s’appuie sur le projet créé dans [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md) et explicite les concepts décrits dans cette rubrique.

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>Que signifie «*observable*» pour une collection?
Si une classe runtime qui représente une collection choisit de déclencher l’événement [**IObservableVector&lt;T&gt;:: VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) chaque fois qu’un élément lui est ajouté ou retiré, alors la classe runtime est une collection observable. Un contrôle d’éléments XAML peut lier et gérer ces événements en récupérant la collection mise à jour et en se mettant à jour lui-même pour afficher les éléments actuels.

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="implement-singlethreadedobservablevectorlttgt"></a>Implémenter **single_threaded_observable_vector&lt;T&gt;**
Il vous sera utile de disposer d’un modèle de vecteur observable pour servir d’implémentation utile à usage général de [**IObservableVector&lt;T&gt;**](/uwp/api/windows.foundation.collections.iobservablevector_t_). Vous trouverez ci-dessous un listing d’une classe appelée **single_threaded_observable_vector\<T\>**.

> [!NOTE]
> Si vous avez installé la [Windows 10 SDK version d’évaluation 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)ou une version ultérieure, puis vous pouvez simplement utiliser directement la fonction d’usine **WinRT:: single_threaded_observable_vector\ < T\ >** au lieu du listing de code ci-dessous (nous allons montrer le code exact ultérieurement dans cette rubrique). Si vous n’êtes pas déjà sur cette version du SDK, il sera facile de basculer de l’utilisation de la version du listing de code à la fonction **winrt** lorsque vous êtes.

```cppwinrt
// single_threaded_observable_vector.h
#pragma once

namespace winrt::Bookstore::implementation
{
    using namespace Windows::Foundation::Collections;

    template <typename T>
    struct single_threaded_observable_vector : implements<single_threaded_observable_vector<T>,
        IObservableVector<T>,
        IVector<T>,
        IVectorView<T>,
        IIterable<T>>
    {
        event_token VectorChanged(VectorChangedEventHandler<T> const& handler)
        {
            return m_changed.add(handler);
        }

        void VectorChanged(event_token const cookie)
        {
            m_changed.remove(cookie);
        }

        T GetAt(uint32_t const index) const
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            return m_values[index];
        }

        uint32_t Size() const noexcept
        {
            return static_cast<uint32_t>(m_values.size());
        }

        IVectorView<T> GetView()
        {
            return *this;
        }

        bool IndexOf(T const& value, uint32_t& index) const noexcept
        {
            index = static_cast<uint32_t>(std::find(m_values.begin(), m_values.end(), value) - m_values.begin());
            return index < m_values.size();
        }

        void SetAt(uint32_t const index, T const& value)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values[index] = value;
            m_changed(*this, make<args>(CollectionChange::ItemChanged, index));
        }

        void InsertAt(uint32_t const index, T const& value)
        {
            if (index > m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.insert(m_values.begin() + index, value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, index));
        }

        void RemoveAt(uint32_t const index)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.erase(m_values.begin() + index);
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, index));
        }

        void Append(T const& value)
        {
            ++m_version;
            m_values.push_back(value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
        }

        void RemoveAtEnd()
        {
            if (m_values.empty())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.pop_back();
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, Size()));
        }

        void Clear() noexcept
        {
            ++m_version;
            m_values.clear();
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        uint32_t GetMany(uint32_t const startIndex, array_view<T> values) const
        {
            if (startIndex >= m_values.size())
            {
                return 0;
            }

            uint32_t actual = static_cast<uint32_t>(m_values.size() - startIndex);

            if (actual > values.size())
            {
                actual = values.size();
            }

            std::copy_n(m_values.begin() + startIndex, actual, values.begin());
            return actual;
        }

        void ReplaceAll(array_view<T const> value)
        {
            ++m_version;
            m_values.assign(value.begin(), value.end());
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        IIterator<T> First()
        {
            return make<iterator>(this);
        }

    private:

        std::vector<T> m_values;
        event<VectorChangedEventHandler<T>> m_changed;
        uint32_t m_version{};

        struct args : implements<args, IVectorChangedEventArgs>
        {
            args(CollectionChange const change, uint32_t const index) :
                m_change(change),
                m_index(index)
            {
            }

            CollectionChange CollectionChange() const
            {
                return m_change;
            }

            uint32_t Index() const
            {
                return m_index;
            }

        private:

            Windows::Foundation::Collections::CollectionChange const m_change{};
            uint32_t const m_index{};
        };

        struct iterator : implements<iterator, IIterator<T>>
        {
            explicit iterator(single_threaded_observable_vector<T>* owner) noexcept :
            m_version(owner->m_version),
                m_current(owner->m_values.begin()),
                m_end(owner->m_values.end())
            {
                m_owner.copy_from(owner);
            }

            void abi_enter() const
            {
                if (m_version != m_owner->m_version)
                {
                    throw hresult_changed_state();
                }
            }

            T Current() const
            {
                if (m_current == m_end)
                {
                    throw hresult_out_of_bounds();
                }

                return*m_current;
            }

            bool HasCurrent() const noexcept
            {
                return m_current != m_end;
            }

            bool MoveNext() noexcept
            {
                if (m_current != m_end)
                {
                    ++m_current;
                }

                return HasCurrent();
            }

            uint32_t GetMany(array_view<T> values)
            {
                uint32_t actual = static_cast<uint32_t>(std::distance(m_current, m_end));

                if (actual > values.size())
                {
                    actual = values.size();
                }

                std::copy_n(m_current, actual, values.begin());
                std::advance(m_current, actual);
                return actual;
            }

        private:

            com_ptr<single_threaded_observable_vector<T>> m_owner;
            uint32_t const m_version;
            typename std::vector<T>::const_iterator m_current;
            typename std::vector<T>::const_iterator const m_end;
        };
    };
}
```

La fonction **Append** illustre comment déclencher l’événement [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged).

```cppwinrt
m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
```

Les arguments d’événement indiquent à la fois qu’un élément a été inséré, et également quel est son index (le dernier élément, dans le cas présent). Ces arguments activent un contrôle d’éléments XAML pour répondre à l’événement et s’actualiser de façon optimale.

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Ajouter une collection **BookSkus** à **BookstoreViewModel**
Dans [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md), nous avons ajouté une propriété de type **BookSku** à notre modèle d’affichage principal. Dans cette étape, nous allons utiliser **single_threaded_observable_vector&lt;T&gt;** pour nous aider à implémenter une collection observable de **BookSku** sur le même modèle d’affichage.

Déclarez une nouvelle propriété dans `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> Dans la liste de MIDL 3.0 ci-dessus, notez que le type de la propriété **BookSkus** est [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821). Dans la section suivante de cette rubrique, nous allons lie la source des éléments d’une [**zone de liste**](/uwp/api/windows.ui.xaml.controls.listbox) à **BookSkus**. Une zone de liste est un contrôle d’éléments, et pour définir correctement la propriété [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , vous devez lui affecter une valeur de type **IVector** de **IInspectable**, ou d’un type d’interopérabilité tels que [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

Enregistrez et lancez la génération. Copiez les stubs accesseur de `BookstoreViewModel.h` et `BookstoreViewModel.cpp` dans le dossier `Generated Files`, et implémentez-les.

```cppwinrt
// BookstoreViewModel.h
...
#include "single_threaded_observable_vector.h"
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="if-you-have-a-windows-10-sdk-preview-build"></a>Si vous disposez d’une version d’évaluation Windows 10 SDK
Si vous avez installé la [Windows 10 SDK version d’évaluation 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK), ou une version ultérieure, puis remplacez la ligne de code

```cppwinrt
m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
```

Cela.

```cppwinrt
m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
```

Au lieu d’appeler [**winrt::make**](https://docs.microsoft.com/en-us/uwp/cpp-ref-for-winrt/make), vous créez l’objet de collection appropriée en appelant la fonction d’usine **WinRT:: single_threaded_observable_vector\ < T\ >** .

## <a name="bind-a-listbox-to-the-bookskus-property"></a>Lier un élément ListBox à la propriété **BookSkus**
Ouvrez `MainPage.xaml`, qui contient le balisage XAML pour notre page d’interface utilisateur principale. Ajoutez le balisage suivant dans le même **StackPanel** que **Button**.

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

Dans `MainPage.cpp`, ajoutez une ligne de code au gestionnaire d’événements **Click** pour ajouter un livre à la collection.

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

Lancez à présent le processus de génération et exécutez le projet. Cliquez sur le bouton pour exécuter le gestionnaire d’événements **Click**. Nous avons vu que l’implémentation de **Append** déclenche un événement pour informer l’interface utilisateur que la collection a changé; l'’élément **ListBox** interroge de nouveau la collection pour mettre à jour sa propre valeur **Items**. Exactement comme précédemment, le titre de l’un des livres change; et ce changement de titre est reflété à la fois sur le bouton et dans la zone de liste.

## <a name="important-apis"></a>API importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Rubriquesassociées
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Créer des API avec C++/WinRT](author-apis.md)
