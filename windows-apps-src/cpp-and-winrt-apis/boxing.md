---
description: Une valeur scalaire doit être encapsulée dans un objet de classe de référence avant d’être transmise à une fonction qui attend **IInspectable**. Ce processus d’encapsulation est appelé « *conversion boxing* » de la valeur.
title: Conversions boxing et unboxing de valeurs scalaires vers IInspectable avec C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, contrôle, conversion boxing, scalaire, valeur
ms.localizationpriority: medium
ms.openlocfilehash: 1bd69e07f2285b6c05581a60ddbaf4b394d0b6d0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361201"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Conversions boxing et unboxing de valeurs scalaires vers IInspectable avec C++/WinRT
 
L’[**interface IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) est l’interface racine de chaque classe runtime dans Windows Runtime (WinRT). Il s’agit d’un concept analogue à [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) se trouvant à la racine de chaque interface et classe COM et de **System.Object** se trouvant à la racine de chaque classe [Common Type System](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system).

En d’autres termes, une fonction qui attend **IInspectable** peut se voir transmettre une instance de n’importe quelle classe runtime. Mais vous ne pouvez pas transmettre directement une valeur scalaire, comme une valeur numérique ou une valeur de texte, vers une telle fonction. Au lieu de cela, une valeur scalaire doit être encapsulée dans un objet de classe de référence. Ce processus d’encapsulation est appelé « *conversion boxing* » de la valeur.

[C++/ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) fournit le [ **winrt::box_value** ](/uwp/cpp-ref-for-winrt/box-value) (fonction), qui prend une valeur scalaire et retourne la valeur convertie en un **IInspectable**. Pour annuler la conversion d’un **IInspectable** et revenir à une valeur scalaire, il existe les fonctions [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) et [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Exemples de conversion boxing d’une valeur
La fonction accesseur [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) renvoie un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), qui est une valeur scalaire. Nous pouvons convertir cette valeur **hstring** et la transmettre à une fonction qui attend **IInspectable** comme suit.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Pour définir la propriété de contenu d’un élément [**Button**](/uwp/api/windows.ui.xaml.controls.button) XAML, appelez la fonction mutateur [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?). Pour définir la propriété de contenu sur une valeur de chaîne, vous pouvez utiliser ce code.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Tout d’abord, le constructeur de conversion [**hstring**](/uwp/cpp-ref-for-winrt/hstring) convertit la chaîne littérale en un **hstring**. Puis la surcharge de **winrt::box_value** qui prend un **hstring** est appelée.

## <a name="examples-of-unboxing-an-iinspectable"></a>Exemples de conversion unboxing d’un IInspectable
Dans vos propres fonctions qui attendent **IInspectable**, vous pouvez utiliser [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) pour effectuer une conversion unboxing, et vous pouvez utiliser [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) pour effectuer une conversion unboxing avec une valeur par défaut.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Déterminer le type d’une valeur convertie
Si vous recevez une valeur convertie et que vous n'êtes pas certain du type qu’elle contient (vous devez connaître son type pour lui appliquer une conversion unboxing), vous pouvez interroger la valeur convertie pour rechercher son interface [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue), puis appeler **Type** sur elle. Voici un exemple de code.

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>API importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [modèle de fonction WinRT::box_value](/uwp/cpp-ref-for-winrt/box-value)
* [WinRT::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [modèle de fonction WinRT::unbox_value](/uwp/cpp-ref-for-winrt/unbox-value)
* [modèle de fonction WinRT::unbox_value_or](/uwp/cpp-ref-for-winrt/unbox-value-or)
