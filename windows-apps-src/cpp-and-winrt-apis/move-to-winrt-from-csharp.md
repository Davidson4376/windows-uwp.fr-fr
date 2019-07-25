---
description: Cette rubrique montre comment porter du code C# vers son équivalent en C++/WinRT.
title: Passer de C# à C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, porter, migrer, C#
ms.localizationpriority: medium
ms.openlocfilehash: a63d38db613ebe6425a05ed20563405242ffd441
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328861"
---
# <a name="move-to-cwinrt-from-c"></a>Passer de C# à C++/WinRT

Cette rubrique montre comment porter le code d’un projet [C#](/visualstudio/get-started/csharp) vers son équivalent en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="register-an-event-handler"></a>Inscrire un gestionnaire d’événements

Vous pouvez inscrire un gestionnaire d’événements dans le balisage XAML.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

Dans C#, même si votre méthode **OpenButton_Click** est privée, XAML sera toujours en mesure de la connecter à l’événement [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) déclenché par *OpenButton*.

Dans C++/WinRT, votre méthode **OpenButton_Click** doit être publique dans votre [type d’implémentation](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Vous pouvez également faire en sorte que la classe d’inscription soit l’amie de votre classe d’implémentation, tandis que **OpenButton_Click** est privée.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="making-a-class-available-to-the-binding-markup-extension"></a>Mise à disposition d’une classe pour l’extension de balisage {Binding}

Si vous envisagez d’utiliser l’extension de balisage {Binding} pour lier des données à votre type de données, consultez [Objet de liaison déclaré à l’aide de {Binding}](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding).

## <a name="making-a-data-source-available-to-xaml-markup"></a>Mise à disposition d’une source de données pour le balisage XAML

Dans C++/WinRT 2.0.190530.8 et les versions ultérieures, [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) crée un vecteur observable qui prend en charge **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** et **IObservableVector\<IInspectable\>** .

Vous pouvez créer votre **fichier Midl (.idl)** de cette façon (consultez [Factorisation des classes runtime dans des fichiers Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Ensuite, vous l’implémentez comme suit.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

Pour plus d’informations, consultez [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection).

## <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Mise à disposition d’une source de données pour le balisage XAML (version antérieure à C++/WinRT 2.0.190530.8)

La liaison de données XAML exige qu’une source d’éléments implémente **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** , ainsi que l’une des combinaisons d’interfaces suivantes.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** et **INotifyCollectionChanged**
- **IBindableVector** et **IBindableObservableVector**
- **IBindableVector** uniquement (ne répond pas aux modifications)
- **IVector\<IInspectable\>**
- **IBindableIterable** (effectue une itération et enregistre les éléments dans une collection privée)

Une interface générique comme **IVector\<T\>** ne peut pas être détectée au moment de l’exécution. Chaque **IVector\<T\>** possède une fonction **T** qui correspond à un identificateur d’interface (IID) différent. Les développeurs peuvent développer l’ensemble de fonctions **T** de manière arbitraire. Le code de liaison XAML ne peut donc jamais connaître l’ensemble complet à interroger. Cette restriction n’est pas un problème pour C#, car chaque objet CLR implémentant **IEnumerable\<T\>** implémente automatiquement **IEnumerable**. Au niveau de l’ABI, cela signifie que chaque objet implémentant **IObservableVector\<T\>** implémente automatiquement **IObservableVector\<IInspectable\>** .

C++/WinRT ne propose pas cette garantie. Si une classe runtime C++/WinRT implémente **IObservableVector\<T\>** , nous ne pouvons pas partir du principe qu’une implémentation **IObservableVector\<IInspectable\>** est également fournie.

Par conséquent, voici à quoi doit ressembler l’exemple précédent.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

Quant à l’implémentation :

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

Si vous avez besoin d’accéder à des objets dans *m_bookSkus*, vous devez appliquer en retour une méthode QI à **Bookstore:: BookSku**.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

## <a name="tostring"></a>ToString()

Les types C# fournissent la méthode [Object::ToString](/dotnet/api/system.object.tostring).

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/WinRT ne fournit pas directement cette fonctionnalité, mais vous disposez d’alternatives.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT prend également en charge [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) pour certains types. Vous devez ajouter des surcharges pour tous les types supplémentaires que vous souhaitez convertir en chaîne.

| Langue | Convertir un entier en chaîne | Convertir une valeur enum en chaîne |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

Si vous souhaitez convertir une valeur enum en chaîne, vous devez fournir l’implémentation de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

Ces conversions en chaîne sont souvent utilisées implicitement par la liaison de données.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Ces liaisons effectuent la conversion **winrt::to_hstring** de la propriété liée. Pour le deuxième exemple (**StatusEnum**), vous devez fournir votre propre surcharge de **winrt::to_hstring**. Dans le cas contraire, vous recevrez une erreur relative au compilateur.

## <a name="string-building"></a>Génération de chaîne

Pour la génération de chaînes, C# possède un type [**StringBuilder**](/dotnet/api/system.text.stringbuilder) intégré.

| | C# | C++/WinRT |
|-|-|-|
| Classe de générateur | `StringBuilder builder;` | `std::wstringstream stream;` |
| Ajout de chaîne, conservation des valeurs null | `builder.Append(s);` | `stream << std::wstring_view{ s };` |
| Extraire le résultat | `s = builder.ToString();` | `ws = stream.str();` |

## <a name="boxing-and-unboxing"></a>Boxing et unboxing

C# effectue automatiquement une conversion boxing des scalaires en objets. C++/WinRT vous oblige à appeler la fonction [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) de manière explicite. Les deux langages nécessitent un unboxing explicite. Consultez [Conversions boxing et unboxing avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

Dans les tableaux suivants, nous allons utiliser ces définitions.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| Opération | C# | C++/WinRT|
|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX et C# génèrent des exceptions si vous essayez d’effectuer une conversion unboxing d’un pointeur null en un type de valeur. C++/WinRT considère qu’il s’agit d’une erreur de programmation, ce qui provoque un blocage. Dans C++/WinRT, utilisez la fonction [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) si vous souhaitez prendre en charge le cas avec l’objet qui n’est pas du type que vous pensiez.

| Scénario | C# | C++/WinRT|
|-|-|-|
| Conversion unboxing d’un entier connu |`i = (int)o;` | `i = unbox_value<int>(o);` |
| Si o est null | `System.NullReferenceException` | Se bloquer |
| Si o n’est pas un entier converti par boxing | `System.InvalidCastException` | Se bloquer |
| Conversion unboxing d’un entier, utiliser la valeur de secours si la valeur est null ; se bloquer si autre chose | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Conversion unboxing d’un entier si possible ; utiliser la valeur de secours pour toute autre chose | `var box = o as int?;`<br>`i = box != null ? box.Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Conversions boxing et unboxing d’une chaîne

Une chaîne est parfois un type de valeur et parfois un type de référence. C# et C++/WinRT traitent les chaînes différemment.

Le type ABI [**HSTRING**](/windows/win32/winrt/hstring) est un pointeur vers une chaîne avec décompte des références. Toutefois, il ne dérive pas à partir de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable). Techniquement, il ne s’agit donc pas d’un *objet*. En outre, un pointeur **HSTRING** null représente la chaîne vide. La conversion boxing d’éléments non dérivés à partir de **IInspectable** est effectuée en les encapsulant dans une interface [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_). De plus, Windows Runtime fournit une implémentation standard sous la forme de l’objet [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (les types personnalisés sont signalés sous la forme [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

C# représente une chaîne Windows Runtime sous la forme d’un type de référence, tandis que C++/WinRT projette une chaîne sous la forme d’un type de valeur. Cela signifie qu’une chaîne null convertie par boxing peut avoir des représentations différentes selon la façon dont vous l’avez obtenue.

| Opération | C# | C++/WinRT|
|-|-|-|
| Catégorie de type de chaîne | Type de référence | Type de valeur |
| **HSTRING** null projette sous la forme | `""` | `hstring{ nullptr }` |
| Est-ce que null et `""` sont identiques ? | Non | Oui |
| Validité de la valeur null | `s = null;`<br>`s.Length` génère l’exception **NullReferenceException** | `s = nullptr;`<br>`s.size() == 0` (valide) |
| Conversion boxing d’une chaîne | `o = s;` | `o = box_value(s);` |
| Si `s` est `null` | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{nullptr});`<br>`o != nullptr` |
| Si `s` est `""` | `o = "";`<br>`o != null;` | `o = box_value(hstring{L""});`<br>`o != nullptr;` |
| Conversion boxing d’une chaîne en conservant null | `o = s;` | `o = s.empty() ? nullptr : box_value(s);` |
| Conversion boxing forcée d’une chaîne | `o = PropertyValue.CreateString(s);` | `o = box_value(s);` |
| Conversion unboxing d’une chaîne connue | `s = (string)o;` | `s = unbox_value<hstring>(o);` |
| Si `o` est null | `s == null; // not equivalent to ""` | Se bloquer |
| Si `o` n’est pas une chaîne convertie par boxing | `System.InvalidCastException` | Se bloquer |
| Conversion unboxing d’une chaîne, utiliser la valeur de secours si la valeur est null ; se bloquer si autre chose | `s = o != null ? (string)o : fallback;` | `s = o ? unbox_value<hstring>(o) : fallback;` |
| Conversion unboxing d’une chaîne si possible ; utiliser la valeur de secours pour toute autre chose | `var s = o as string ?? fallback;` | `s = unbox_value_or<hstring>(o, fallback);` |

Dans les deux scénarios de *conversion unboxing avec valeur de secours* ci-dessus, il est possible qu’une chaîne null ait été convertie à l’aide d’une conversion boxing forcée. Auquel cas, la valeur de secours ne sera pas utilisée. La valeur de sortie sera une chaîne vide, car il s’agit de ce qu’il y avait à l’intérieur.

## <a name="derived-classes"></a>Classes dérivées

La classe de base doit être *composable* pour être dérivée à partir d’une classe runtime. C# ne nécessite pas de procédure spéciale pour que vos classes soient composables, contrairement à C++/WinRT. Vous utilisez le [mot clé unsealed](/uwp/midl-3/intro#base-classes) pour indiquer que vous souhaitez que votre classe soit utilisable comme classe de base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

Dans votre classe d’en-tête d’implémentation, vous devez ajouter le fichier d’en-tête de la classe de base avant d’inclure l’en-tête généré automatiquement pour la classe dérivée. Dans le cas contraire, vous obtiendrez des erreurs telles que « Utilisation non conforme de ce type comme expression ».

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="consuming-objects-from-xaml-markup"></a>Utilisation d’objets à partir du balisage XAML

Dans un projet C#, vous pouvez utiliser des éléments nommés et des membres privés à partir du balisage XAML. Toutefois, dans C++/WinRT, toutes les entités utilisées via [**l’extension de balisage {x:Bind}** ](/windows/uwp/xaml-platform/x-bind-markup-extension) XAML doivent être exposées publiquement dans IDL.

En outre, une liaison à un booléen affiche `true` ou `false` dans C#, mais affichera **Windows.Foundation.IReference`1\<Boolean\>** dans C++/WinRT.

Pour obtenir plus d’informations et d’exemples de code, consultez [Utilisation d’objets à partir du balisage](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

## <a name="important-apis"></a>API importantes
* [Modèle de fonction winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriques connexes
* [Didacticiels C#](/visualstudio/get-started/csharp)
* [Créer des API avec C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Présentation détaillée de la liaison de données](/windows/uwp/data-binding/data-binding-in-depth)
* [Contrôles d’éléments XAML ; liaison à une collection C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection)