---
description: Identifie de manière unique les éléments qui sont créés et référencés en tant que ressources, et qui existent au sein d’une classe ResourceDictionary.
title: Attribut x:Key
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cb42fcb17cfcad76989732b1a1482d9fbc85be5e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372323"
---
# <a name="xkey-attribute"></a>Attribut x:Key


Identifie de manière unique les éléments qui sont créés et référencés en tant que ressources, et qui existent au sein d’une classe [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary).

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>Utilisation des attributs XAML (**ResourceDictionary** implicite)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| objet | Tout objet partageable. Voir [Références aux ressources ResourceDictionary et XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| stringKeyValue | Chaîne vraie utilisée en tant que clé, qui doit être conforme à la grammaire _XamlName_&gt;. Voir « Grammaire XamlName » ci-dessous. | 

##  <a name="xamlname-grammar"></a>Grammaire XamlName

Les points suivants représentent la grammaire normative qui régit une chaîne servant de clé dans l’implémentation XAML de plateforme Windows universelle (UWP) :

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Caractères sont limitées à la plage ASCII inférieure et plus spécifiquement à l’alphabet romain en majuscules et minuscules, des chiffres et le trait de soulignement (\_) caractères.
-   La plage de caractères Unicode n’est pas prise en charge.
-   Un nom ne peut pas commencer par un chiffre.

## <a name="remarks"></a>Notes

Les éléments enfants d’un [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) incluent généralement un attribut **x:Key** qui spécifie une valeur de clé unique au sein de ce dictionnaire. L’unicité de la clé est appliquée au moment du chargement par le processeur XAML. Les valeurs **x:Key** non uniques engendrent des exceptions d’analyse XAML. Sur demande de l’[extension de balisage {StaticResource}](staticresource-markup-extension.md), toute clé non résolue engendre également des exceptions d’analyse XAML.

**x:Key** et [x:Name](x-name-attribute.md) ne sont pas des concepts identiques. **x:Key** est exclusivement utilisé dans les dictionnaires de ressources. x:Name est utilisé pour toutes les zones de code XAML. Un appel [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) utilisant une valeur de clé ne permet pas de récupérer une ressource à clé. Les objets définis dans un dictionnaire de ressources peuvent avoir un **x : Key**, un **x : Name** ou les deux. La clé et le nom ne doivent pas nécessairement correspondre.

Notez que dans la syntaxe implicite présentée, l’objet [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) est implicite dans la manière dont le processeur XAML produit un nouvel objet pour renseigner une collection [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources).

Le code qui équivaut à spécifier **x:Key** est une opération qui utilise une clé avec la classe [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) sous-jacente. Par exemple, un **x:Key** appliqué dans un balisage pour une ressource équivaut à la valeur du paramètre *key* de **Insert** lorsque vous ajoutez la ressource à une classe **ResourceDictionary**.

Un élément d’un dictionnaire de ressource peut ignorer une valeur pour **x:Key** s’il s’agit d’un [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) ou [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) ciblé ; dans chacun des cas, la clé implicite de la ressource est la valeur **TargetType** interprétée comme une chaîne. Pour plus d’informations, voir [Démarrage rapide : application de styles aux contrôles](https://docs.microsoft.com/previous-versions/windows/apps/hh465498(v=win.10)) et [Références aux ressources ResourceDictionary et XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

