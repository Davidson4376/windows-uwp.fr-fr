---
description: Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée collection *observable*. Cette rubrique montre comment implémenter et utiliser une collection observable, et comment y lier un contrôle d’éléments XAML.
title: Contrôles d’éléments XAML ; liaison à une collection C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, liaison, collection
ms.localizationpriority: medium
ms.openlocfilehash: 7669c6536f28d5f979567f5b433dbf614800bec3
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65627677"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>Contrôles d’éléments XAML ; liaison à une collection C++/WinRT

Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée collection *observable*. Ce concept est basé sur le modèle de conception logicielle appelé *modèle observateur*. Cette rubrique montre comment implémenter des collections observables en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) et y lier des contrôles d’éléments XAML.

Si vous souhaitez suivre cette rubrique, nous vous recommandons de tout d’abord créer le projet décrit dans la section [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md). Cette rubrique ajoute plus de code à ce projet et complète les concepts abordés.

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>Que signifie « *observable* » pour une collection ?
Si une classe runtime qui représente une collection choisit de déclencher l’événement [**IObservableVector&lt;T&gt;: : VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) chaque fois qu’un élément lui est ajouté ou retiré, alors la classe runtime est une collection observable. Un contrôle d’éléments XAML peut lier et gérer ces événements en récupérant la collection mise à jour et en se mettant à jour lui-même pour afficher les éléments actuels.

> [!NOTE]
> Pour plus d’informations sur l’installation et l’utilisation de l’extension VSIX (Visual Studio Extension) C++/WinRT et du package NuGet (qui fournissent ensemble la prise en charge des modèles et des builds de projet), consultez [Prise en charge de Visual Studio pour C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Ajouter une collection **BookSkus** à **BookstoreViewModel**

Dans [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md), nous avons ajouté une propriété de type **BookSku** à notre modèle d’affichage principal. Dans cette étape, nous allons utiliser le modèle de fonction d’usine [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) pour implémenter une collection observable de **BookSku** sur le même modèle d’affichage.

Déclarez une nouvelle propriété dans `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> Dans le listing MIDL 3.0 ci-dessus, notez que le type de la propriété **BookSkus** est [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Dans la section suivante de cette rubrique, nous allons lier la source des éléments d’un [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) à **BookSkus**. Une zone de liste est un contrôle d’éléments, et pour définir correctement la propriété [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), vous devez lui attribuer une valeur de type **IObservableVector** (ou **IVector**) de **IInspectable**, ou d’un type d’interopérabilité comme [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

Enregistrez et lancez la génération. Copiez les stubs accesseur de `BookstoreViewModel.h` et `BookstoreViewModel.cpp` dans le dossier `\Bookstore\Bookstore\Generated Files\sources` (pour plus d’informations, consultez la rubrique précédente, [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md)). Implémentez ces stubs accesseur comme suit.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

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
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

Lancez à présent le processus de génération et exécutez le projet. Cliquez sur le bouton pour exécuter le gestionnaire d’événements **Click**. Nous avons vu que l’implémentation de **Append** déclenche un événement pour informer l’interface utilisateur que la collection a changé ; l’élément **ListBox** interroge de nouveau la collection pour mettre à jour sa propre valeur **Items**. Exactement comme précédemment, le titre de l’un des livres change ; et ce changement de titre est reflété à la fois sur le bouton et dans la zone de liste.

## <a name="important-apis"></a>API importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Rubriques connexes
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Créer des API avec C++/WinRT](author-apis.md)
