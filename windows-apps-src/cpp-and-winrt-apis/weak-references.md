---
author: stevewhims
description: La prise en charge des références faibles C++/WinRT est de type «payer pour lire», dans la mesure où elle ne vous coûte rien, sauf si votre objet est interrogé pour IWeakReferenceSource.
title: Références faibles en C++/WinRT
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection, faible, référence
ms.localizationpriority: medium
ms.openlocfilehash: 63ffad19c0ae8a52737ae13a54e5657df875d0b5
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832603"
---
# <a name="weak-references-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Références faibles en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Vous devriez pouvoir, le plus souvent, concevoir vos propres API C++/WinRT de manière à éviter le besoin de références cycliques et de références faibles. Toutefois, lorsqu’il s’agit de l’implémentation native de la structure d’interface utilisateur basée sur XAML&mdash;en raison de la conception historique de l’infrastructure&mdash;le mécanisme de références faibles en C++/WinRT est nécessaire pour gérer les références cycliques. En dehors de XAML, il est peu probable que vous devrez utiliser des références faibles (bien qu’elles ne soient pas, en théorie, spécifiques à XAML).

Pour n’importe quel type donné que vous déclarez, il n’est pas immédiatement évident pour C++/WinRT de déterminer si des références faibles sont nécessaires. Par conséquent, C++/WinRT fournit la prise en charge des références faibles automatiquement sur le modèle de structure [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), à partir duquel vos propres types C++/WinRT dérivent directement ou indirectement. Il s’agit d’une prise en charge «payer pour lire», dans la mesure où elle ne vous coûte rien, sauf si votre objet est réellement interrogé pour [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609). Et vous pouvez choisir explicitement de [refuser cette prise en charge](#opting-out-of-weak-reference-support).

## <a name="code-examples"></a>Exemples de code
Le modèle de structure [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) est une option permettant d’obtenir une référence faible à une instance de classe.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```
Ou, vous pouvez utiliser la fonction d’assistance [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

La création d’une référence faible n’affecte pas le nombre de références sur l’objet lui-même; elle entraîne simplement l’allocation d’un bloc de contrôle. Ce bloc de contrôle s’occupe de l’implémentation de la sémantique des références faibles. Vous pouvez essayer de promouvoir la référence faible pour une référence forte et, en cas de réussite, l’utiliser.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

À condition qu’une autre référence forte existe toujours, l’appel [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) incrémente le nombre de références et retourne la référence forte à l’appelant.

## <a name="a-weak-reference-to-the-this-pointer"></a>Une référence faible au pointeur *this*
Un objet C++/WinRT dérive directement ou indirectement du modèle de structure [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). La fonction membre protégée [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) renvoie une référence faible vers le pointeur *this* d’un objet C++/WinRT. [**Implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) obtient une référence forte.

## <a name="opting-out-of-weak-reference-support"></a>Refus de la prise en charge des références faibles
La prise en charge des références faibles est automatique. Mais vous pouvez choisir explicitement de refuser cette prise en charge en transmettant la structure de marqueur [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) en tant qu’argument de modèle à votre classe de base.

Si vous dérivez directement de **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Si vous créez une classe runtime.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

Peu importe où la structure de marqueur apparaît dans le pack de paramètres variadiques. Si vous demandez une référence faible pour un type refusé, le compilateur vous aidera en affichant une erreur «*This is only for weak ref support*» (ceci est réservé à la prise en charge des références faibles).

## <a name="important-apis"></a>API importantes
* [Fonction implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Modèle de fonction winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [Structure de marqueur winrt::no_weak_ref](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Modèle de structure winrt::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)
