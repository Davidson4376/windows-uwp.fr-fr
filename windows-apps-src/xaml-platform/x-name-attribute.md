---
description: Identifie de manière unique les éléments objet pour l’accès à l’objet instancié depuis le code-behind ou le code général.
title: Attribut xName
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ef1a6047a7c462961f40ae8913881125e2331bb
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8755225"
---
# <a name="xname-attribute"></a>Attribut x:Name


Identifie de manière unique les éléments objet pour l’accès à l’objet instancié depuis le code-behind ou le code général. Une fois appliqué à un modèle de programmation de stockage, **x:Name** peut être considéré comme équivalent à la variable qui contient une référence d’objet, telle que renvoyée par un constructeur.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| XAMLNameValue | Chaîne qui se conforme aux restrictions de la grammaire XamlName. |

##  <a name="xamlname-grammar"></a>Grammaire XamlName

Les points suivants représentent la grammaire normative qui régit une chaîne servant de clé dans cette implémentation XAML:

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
-   Un nom ne peut pas commencer par un chiffre. Certaines implémentations d’outils ajoutent un trait de soulignement (\_) au début d’une chaîne si l’utilisateur indique un chiffre comme premier caractère ou si l’outil génère automatiquement des valeurs **x:Name** sur la base d’autres valeurs contenant des chiffres.

## <a name="remarks"></a>Remarques

Le **x:Name** spécifié devient le nom d’un champ créé dans le code sous-jacent lors du traitement du code XAML, et ce champ contient une référence à l’objet. Le processus de création de ce champ est effectué par les étapes cible MSBuild, lesquelles ont également la responsabilité de joindre les classes partielles pour un fichier XAML et son code-behind. Ce comportement n’est pas nécessairement propre au langage XAML ; il s’agit de l’implémentation particulière que la programmation de plateforme Windows universelle (UWP) pour XAML applique afin d’utiliser **x:Name** dans ses modèles de programmation et d’application.

Chaque **x:Name** défini doit être unique au sein d’un namescope XAML. En général, un namescope XAML est défini au niveau de l’élément racine d’une page chargée et contient tous les éléments sous cet élément dans une seule page XAML. D’autres namescopes XAML sont définis par n’importe quel modèle de contrôle ou modèle de données défini sur cette page. Au moment de l’exécution, un autre namescope XAML est créé pour la racine de l’arborescence d’objets créée à partir d’un modèle de contrôle appliqué, ainsi que par les arborescences d’objets créées à partir d’un appel à la méthode [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048). Pour plus d’informations, voir [Namescopes XAML](xaml-namescopes.md).

Les outils de conception génèrent souvent automatiquement des valeurs **x:Name** pour les éléments lorsqu’ils sont introduits dans l’aire de conception. Le schéma de génération automatique varie selon le concepteur que vous utilisez, mais le schéma habituel consiste à générer une chaîne qui commence par le nom de la classe qui stocke l’élément, suivi d’un entier progressif. Par exemple, si vous introduisez le premier élément [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) dans le concepteur, vous pouvez constater que dans le code XAML, la valeur d’attribut **x:Name** de cet élément est « Button1 ».

Il n’est pas possible de définir **x:Name** dans la syntaxe de l’élément de propriété XAML, ou dans le code à l’aide de [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361). **x:Name** peut uniquement être défini à l’aide de la syntaxe de l’attribut XAML sur les éléments.

**Remarque**spécifiquement pour C++ / CX applications, le champ de sauvegarde d’une référence de **x: Name** n’est pas créé pour l’élément racine d’un fichier XAML ou une page. Si vous devez référencer l’objet racine à partir d’un fichier code-behind C++, utilisez d’autres API ou une traversée d’arborescence. Par exemple, vous pouvez appeler [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) pour un élément enfant nommé connu avant d’appeler [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739).

### <a name="xname-and-other-name-properties"></a>x:Name et autres propriétés Name

Certains types utilisés en XAML UWP ont également une propriété nommée **Name**. Par exemple, [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) et [**TextElement.Name**](https://msdn.microsoft.com/library/windows/apps/hh702125).

Si **Name** est disponible en tant que propriété définissable sur un élément, **Name** et **x:Name** peuvent être utilisés de façon interchangeable en XAML, mais une erreur est générée si les deux attributs sont spécifiés sur le même élément. Il existe également certains cas où il y a une propriété **Name** mais en lecture seule (comme [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031)). Si tel est le cas, vous devez toujours utiliser **x:Name** pour nommer cet élément dans le code XAML, et l’objet **Name** en lecture seule existe pour certains scénarios de code moins courant.

**Remarque** En règle générale, [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) ne doit pas être utilisé comme un moyen de modifier des valeurs initialement définies par **x: Name**, même s’il existe certains scénarios faisant exception à cette règle générale. Dans les scénarios standard, la création et la définition de namescopes XAML sont une opération du processeur XAML. La modification de l’objet **FrameworkElement.Name** au moment de l’exécution peut engendrer un mauvais alignement de l’attribution des noms du namescope XAML/champ privé, ce dont il est difficile d’effectuer le suivi dans votre code-behind.

### <a name="xname-and-xkey"></a>x:Name et x:Key

**x:Name** peut être appliqué en tant qu’attribut à des éléments dans un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) pour agir en tant que substitut de l’[attribut x:Key](x-key-attribute.md). (C’est une règle que tous les éléments d’un **ResourceDictionary** doivent avoir un attribut x:Key.) Cela est courant dans le cas des [Animations dans une table de montage séquentiel](https://msdn.microsoft.com/library/windows/apps/mt187354). Pour plus d’informations, voir la section correspondante de [Références aux ressources ResourceDictionary et XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

