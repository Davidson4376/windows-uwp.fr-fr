---
author: stevewhims
description: Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée «collection *observable*». Cette rubrique montre comment implémenter et utiliser une collection observable, et comment y lier un contrôle d’éléments XAML.
title: Contrôles d’éléments XAML; liaison à une collection C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, liaison, collection
ms.localizationpriority: medium
ms.openlocfilehash: f9d9d6a2aafe1b81ff331bac92662b111eb48956
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7429401"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>Contrôles d’éléments XAML; liaison à une collection C++/WinRT

Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée «collection *observable*». Ce concept est basé sur le modèle de conception logicielle appelé «*modèle observateur*». Cette rubrique montre comment implémenter des collections observables dans [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), et comment lier XAML des contrôles d’éléments.

Cette procédure s’appuie sur le projet créé dans [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md) et explicite les concepts décrits dans cette rubrique.

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>Que signifie «*observable*» pour une collection?
Si une classe runtime qui représente une collection choisit de déclencher l’événement [**IObservableVector&lt;T&gt;:: VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) chaque fois qu’un élément lui est ajouté ou retiré, alors la classe runtime est une collection observable. Un contrôle d’éléments XAML peut lier et gérer ces événements en récupérant la collection mise à jour et en se mettant à jour lui-même pour afficher les éléments actuels.

> [!NOTE]
> Pour plus d’informations sur l'installation et l'utilisation de l'extension Visual Studio (VSIX) C++/WinRT (qui fournit la prise en charge des modèles de projet, ainsi que les propriétés et cibles MSBuild C++/WinRT), voir [Prise en charge de Visual Studio de C++/WinRT et VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Ajouter une collection **BookSkus** à **BookstoreViewModel**

Dans [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md), nous avons ajouté une propriété de type **BookSku** à notre modèle d’affichage principal. Dans cette étape, nous allons utiliser le modèle de fonction factory [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) pour nous aider à implémenter une collection observable **associé à booksku** sur le même modèle d’affichage.

> [!NOTE]
> Si vous n’avez pas installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809), ou une version ultérieure, voir [Si vous disposez d’une version antérieure du SDK Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) pour obtenir la liste d’un modèle de vecteur observable que vous pouvez utiliser au lieu de **winrt::single_ threaded_observable_vector**.

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
> Dans la liste de MIDL 3.0 ci-dessus, notez que le type de la propriété **BookSkus** est [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Dans la section suivante de cette rubrique, nous allons liaison la source des éléments d’une [**zone de liste**](/uwp/api/windows.ui.xaml.controls.listbox) à **BookSkus**. Une zone de liste est un contrôle d’éléments, et définir correctement la propriété [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , vous devez définir pour une valeur de type **IObservableVector** (ou **IVector**) de **IInspectable**ou d’un type d’interopérabilité comme [** IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

Enregistrez et lancez la génération. Copiez les stubs accesseur de `BookstoreViewModel.h` et `BookstoreViewModel.cpp` dans le dossier `Generated Files`, et implémentez-les.

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

Lancez à présent le processus de génération et exécutez le projet. Cliquez sur le bouton pour exécuter le gestionnaire d’événements **Click**. Nous avons vu que l’implémentation de **Append** déclenche un événement pour informer l’interface utilisateur que la collection a changé; l'’élément **ListBox** interroge de nouveau la collection pour mettre à jour sa propre valeur **Items**. Exactement comme précédemment, le titre de l’un des livres change; et ce changement de titre est reflété à la fois sur le bouton et dans la zone de liste.

## <a name="important-apis"></a>API importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Modèle de fonction winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Rubriquesassociées
* [Utiliser des API avec C++/WinRT](consume-apis.md)
* [Créer des API avec C++/WinRT](author-apis.md)
