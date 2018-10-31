---
author: jwmsft
description: Identifie de manière unique les éléments qui sont créés et référencés en tant que ressources, et qui existent au sein d’une classe ResourceDictionary.
title: Attribut x:Key
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8d48ccb93a411e92b57059192de38366f27353a3
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5864389"
---
# <a name="xkey-attribute"></a>Attribut x:Key


Identifie de manière unique les éléments qui sont créés et référencés en tant que ressources, et qui existent au sein d’une classe [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).

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
| object | Tout objet partageable. Voir [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273). |
| stringKeyValue | Chaîne vraie utilisée en tant que clé, qui doit être conforme à la grammaire _XamlName_&gt;. Voir «Grammaire XamlName» ci-dessous. | 

##  <a name="xamlname-grammar"></a>Grammaire XamlName

Les points suivants représentent la grammaire normative qui régit une chaîne servant de clé dans l’implémentation XAML de plateforme Windows universelle (UWP):

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Les caractères se limitent à la plage ASCII inférieure, plus précisément aux lettres majuscules et minuscules de l’alphabet romain, aux chiffres et au trait de soulignement (\_).
-   La plage de caractères Unicode n’est pas prise en charge.
-   Un nom ne peut pas commencer par un chiffre.

## <a name="remarks"></a>Remarques

Les éléments enfants d’un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) incluent généralement un attribut **x:Key** qui spécifie une valeur de clé unique au sein de ce dictionnaire. L’unicité de la clé est appliquée au moment du chargement par le processeur XAML. Les valeurs **x:Key** non uniques engendrent des exceptions d’analyse XAML. Sur demande de l’[extension de balisage {StaticResource}](staticresource-markup-extension.md), toute clé non résolue engendre également des exceptions d’analyse XAML.

**x:Key** et [x:Name](x-name-attribute.md) ne sont pas des concepts identiques. **x:Key** est exclusivement utilisé dans les dictionnaires de ressources. x:Name est utilisé pour toutes les zones de code XAML. Un appel [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) utilisant une valeur de clé ne permet pas de récupérer une ressource à clé. Les objets définis dans un dictionnaire de ressources peuvent avoir un **x: Key**, un **x: Name** ou les deux. La clé et le nom ne doivent pas nécessairement correspondre.

Notez que dans la syntaxe implicite présentée, l’objet [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) est implicite dans la manière dont le processeur XAML produit un nouvel objet pour renseigner une collection [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740).

Le code qui équivaut à spécifier **x:Key** est une opération qui utilise une clé avec la classe [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) sous-jacente. Par exemple, un **x:Key** appliqué dans un balisage pour une ressource équivaut à la valeur du paramètre *key* de **Insert** lorsque vous ajoutez la ressource à une classe **ResourceDictionary**.

Un élément d’un dictionnaire de ressource peut ignorer une valeur pour **x:Key** s’il s’agit d’un [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) ou [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) ciblé ; dans chacun des cas, la clé implicite de la ressource est la valeur **TargetType** interprétée comme une chaîne. Pour plus d’informations, voir [Démarrage rapide : application de styles aux contrôles](https://msdn.microsoft.com/library/windows/apps/hh465498) et [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

