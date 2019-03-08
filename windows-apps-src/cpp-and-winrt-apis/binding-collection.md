---
description: Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée « collection *observable* ». Cette rubrique montre comment implémenter et utiliser une collection observable, et comment y lier un contrôle d’éléments XAML.
title: Contrôles d’éléments XAML ; liaison avec une collection C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, liaison, collection
ms.localizationpriority: medium
ms.openlocfilehash: c3551ebcc59ebfe426b0be8d5bd20f7578517a25
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649204"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>Contrôles d’éléments XAML ; liaison avec une collection C++/WinRT

Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée « collection *observable* ». Ce concept est basé sur le modèle de conception logicielle appelé « *modèle observateur* ». Cette rubrique montre comment implémenter des collections observables [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), et comment XAML de lier des éléments des contrôles leur.

Cette procédure s’appuie sur le projet créé dans [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md) et explicite les concepts décrits dans cette rubrique.

> [!IMPORTANT]
> Pour obtenir les principaux concepts et termes facilitant votre compréhension pour utiliser et créer des classes runtime avec C++/WinRT, voir [Utiliser des API avec C++/WinRT](consume-apis.md) et [Créer des API avec C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>Que signifie « *observable* » pour une collection ?
Si une classe runtime qui représente une collection choisit de déclencher l’événement [**IObservableVector&lt;T&gt;: : VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) chaque fois qu’un élément lui est ajouté ou retiré, alors la classe runtime est une collection observable. Un contrôle d’éléments XAML peut lier et gérer ces événements en récupérant la collection mise à jour et en se mettant à jour lui-même pour afficher les éléments actuels.

> [!NOTE]
> Pour plus d’informations sur l’installation et à l’aide de C++ / c++ / WinRT Extension Visual Studio (VSIX) (qui fournit la prise en charge des modèles de projet) consultez [prise en charge de Visual Studio pour C / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Ajouter une collection **BookSkus** à **BookstoreViewModel**

Dans [Contrôles XAML ; liaison à une propriété C++/WinRT](binding-property.md), nous avons ajouté une propriété de type **BookSku** à notre modèle d’affichage principal. Dans cette étape, nous allons utiliser le [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) modèle de fonction de fabrique pour nous aider à implémenter une collection observable de **BookSku** sur le modèle de la même vue.

> [!NOTE]
> Si vous n’avez pas installé le SDK Windows version 10.0.17763.0 (Windows 10, version 1809) ou version ultérieure, puis consultez [si vous disposez d’une version antérieure du SDK Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) pour obtenir la liste d’un modèle de vecteur observable que vous pouvez utiliser au lieu de **winrt::single_threaded_observable_vector**.

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
> Dans la liste MIDL 3.0 ci-dessus, notez que le type de la **BookSkus** propriété est [ **IObservableVector** ](/uwp/api/windows.foundation.collections.ivector_t_) de [ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Dans la section suivante de cette rubrique, nous allons liaison la source des éléments d’un [ **ListBox** ](/uwp/api/windows.ui.xaml.controls.listbox) à **BookSkus**. Une zone de liste est un contrôle d’éléments et pour définir correctement la [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) propriété, vous devez lui attribuer une valeur de type **IObservableVector** (ou **IVector**) de **IInspectable**, ou d’un type d’interopérabilité comme [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

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

Lancez à présent le processus de génération et exécutez le projet. Cliquez sur le bouton pour exécuter le gestionnaire d’événements **Click**. Nous avons vu que l’implémentation de **Append** déclenche un événement pour informer l’interface utilisateur que la collection a changé ; l'’élément **ListBox** interroge de nouveau la collection pour mettre à jour sa propre valeur **Items**. Exactement comme précédemment, le titre de l’un des livres change ; et ce changement de titre est reflété à la fois sur le bouton et dans la zone de liste.

## <a name="important-apis"></a>API importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [modèle de fonction WinRT::Make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Rubriques connexes
* [Consommer des API avec C / c++ / WinRT](consume-apis.md)
* [Créer des API avec C / c++ / WinRT](author-apis.md)
