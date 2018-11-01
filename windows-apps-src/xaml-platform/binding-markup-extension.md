---
author: jwmsft
description: L’extension de balisage Binding est convertie au moment du chargement XAML en une instance de la classe Binding.
title: Extension de balisage Binding
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 02c856fd697bef958eb45a0f0f133e06f63a7f51
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5878148"
---
# <a name="binding-markup-extension"></a>Extension de balisage {Binding}


**Remarque**un nouveau mécanisme de liaison est disponible pour Windows 10, qui est optimisé pour la productivité des développeurs et les performances. Voir [extension de balisage {x:Bind}](x-bind-markup-extension.md).

**Remarque**pour des informations générales sur l’utilisation des données de liaison dans votre application avec **{Binding}** (et pour une comparaison entre **{x: Bind}** et **{Binding}**), consultez [liaison de données en profondeur](https://msdn.microsoft.com/library/windows/apps/mt210946).

L’extension de balisage **{Binding}** est utilisé pour les propriétés de liaison de données sur les contrôles à des valeurs provenant d’une source de données, comme du code. L’extension de balisage **{Binding}** est convertie au moment du chargement XAML en une instance de la classe [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Cet objet de liaison obtient une valeur d’une propriété sur une source de données et la transmet à la propriété sur le contrôle. L’objet de liaison peut éventuellement être configuré pour observer les modifications de la valeur de la propriété de source de données, et se mettre à jour en fonction de ces modifications. Il peut également être configuré pour renvoyer les modifications de la valeur de contrôle à la propriété source. La propriété qui est la cible de la liaison de données doit être une propriété de dépendance. Pour plus d’informations, voir [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md).

**{Binding}** a la même propriété de dépendance qu’une valeur locale, et définir une valeur locale en code impératif supprime l’effet de tout **{Binding}** défini dans le balisage.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| Terme | Description |
|------|-------------|
| *propertyPath* | Chaîne qui spécifie le chemin de propriété pour la liaison. Pour plus d’informations, voir la section [Chemin de propriété](#property-path) ci-dessous. |
| *bindingProperties* | *propName*=*value*\[, *propName*=*value*\]*<br/>Une ou plusieurs propriétés de liaison spécifiées à l’aide d’une syntaxe constituée d’une ou plusieurs paires nom/valeur. |
| *propName* | Nom de chaîne de la propriété à définir sur l’objet [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Par exemple, «Convertisseur». |
| *value* | Valeur à attribuer à la propriété. La syntaxe de l’argument dépend de la propriété de la section [Propriétés de la classe de liaison pouvant être définies avec {Binding}](#properties-of-the-binding-class-that-can-be-set-with-binding) ci-dessous. |

## <a name="property-path"></a>Chemin de propriété

[**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) décrit la propriété à laquelle vous liez (propriété source). Path est un paramètre positionnel, ce qui signifie que vous pouvez utiliser le nom du paramètre explicitement (`{Binding Path=EmployeeID}`) ou que vous pouvez le spécifier comme premier paramètre sans nom (`{Binding EmployeeID}`).

Le type de [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) est un chemin de propriété, qui est une chaîne correspondant à une propriété ou sous-propriété de votre type personnalisé ou d’un type d’infrastructure. Le type peut être, mais n’est pas nécessairement, un [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Les étapes dans un chemin de propriété sont délimitées par des points (.), et vous pouvez inclure plusieurs délimiteurs pour parcourir des sous-propriétés successives. Utilisez le point délimiteur quel que soit le langage de programmation utilisé pour implémenter l’objet cible de la liaison.

Par exemple, pour lier une interface utilisateur à la propriété Prénom d’un objet employé, votre chemin de propriété pourrait être «Employee.FirstName». Si vous liez un contrôle d’éléments à une propriété contenant des dépendances d’un employé, votre chemin de propriété pourrait être «Employee.Dependents», et le modèle d’élément du contrôle d’éléments se chargerait de l’affichage des éléments dans «Dependents».

Si la source de données est une collection, un chemin de propriété peut spécifier les éléments de la collection selon leur position ou index. Par exemple, «Teams\[0\].Players», où le littéral «\[\]» encadre le «0» qui spécifie le premier élément d’une collection.

Lorsque vous utilisez une liaison [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) vers un objet [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) existant, vous pouvez utiliser des propriétés jointes comme parties intégrantes du chemin de propriété. Pour lever toute ambiguïté concernant une propriété jointe et éviter que le point intermédiaire figurant dans le nom de la propriété ne soit considéré comme une étape dans un chemin de propriété, mettez le nom de propriété jointe qualifié par le propriétaire entre parenthèses ; par exemple, `(AutomationProperties.Name)`.

Un objet intermédiaire de chemin de propriété est stocké en tant qu’objet [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) dans une représentation d’exécution, mais la plupart des cas ne nécessitent pas d’interaction avec un objet **PropertyPath** dans le code. Vous pouvez généralement spécifier les informations de liaison dont vous avez besoin avec XAML.

Pour plus d’informations sur la syntaxe de chaîne d’un chemin de propriété, des chemins de propriété dans les zones de fonctionnalités d’animation et la construction d’un objet [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259), voir [Syntaxe de PropertyPath](property-path-syntax.md).

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>Propriétés de la classe de liaison pouvant être définies avec {Binding}


**{Binding}** est illustrée avec la syntaxe de l’espace réservé *bindingProperties*, car plusieurs propriétés en lecture/écriture d’un [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) peuvent être définies dans l’extension de balisage. Les propriétés peuvent être définies dans n’importe quel ordre à l’aide de paires *propName*=*value* séparées par des virgules. Certaines propriétés requièrent des types sans conversion de type. Elles nécessitent donc leurs propres extensions de balisage imbriquées dans la **{Binding}**.

| Propriété | Description |
|----------|-------------|
| [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) | Voir la section [Chemin de propriété](#property-path) ci-dessus. |
| [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) | Spécifie un objet convertisseur appelé par le moteur de liaison. Le convertisseur peut être défini dans le balisage à l’aide de l’[extension de balisage {StaticResource}](staticresource-markup-extension.md) pour faire référence à cet objet dans un dictionnaire de ressources. |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | Spécifie la culture que doit utiliser le convertisseur. (Si vous définissez [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826).) La culture est définie comme un identificateur basé sur des normes. Pour plus d’informations, voir [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | Spécifie un paramètre de convertisseur qui peut être utilisé dans la logique du convertisseur. (Si vous définissez [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826).) La plupart des convertisseurs utilisent une logique simple qui obtient toutes les informations de la valeur transmise à convertir, et ne nécessitent aucune valeur **ConverterParameter**. Le paramètre **ConverterParameter** est destiné aux implémentations de convertisseur plus complexes ayant une logique conditionnelle basée sur ce qui est transmis dans **ConverterParameter**. Vous pouvez écrire un convertisseur qui utilise des valeurs qui ne sont pas des chaînes, mais il s’agit d’un scénario peu courant. Pour plus d’informations, voir Remarques dans **ConverterParameter**. |
| [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | Spécifie une source de données en faisant référence à un autre élément dans la même construction XAML qui possède une propriété **Name** ou un attribut [x:Name](x-name-attribute.md). Cette propriété est souvent utilisée pour partager des valeurs associées ou utiliser des sous-propriétés d’un élément d’interface utilisateur pour fournir une valeur spécifique pour un autre élément, par exemple dans un modèle de contrôle XAML. |
| [**FallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | Spécifie une valeur à afficher quand la source ou le chemin ne peuvent pas être résolus. |
| [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) | Spécifie le mode de liaison, qui peut avoir l’une des valeurs suivantes: «OneTime», «OneWay» ou «TwoWay». Ces valeurs correspondent aux noms de constantes de l'énumération [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822). La valeur par défaut est «OneWay». Notez qu’elle diffère de la valeur par défaut de **{x:Bind}** qui est « OneTime ». | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | Spécifie une source de données en décrivant la position de la source de liaison par rapport à la position de la cible de liaison. Ceci est le plus souvent utilisé dans les liaisons au sein de modèles de contrôles XAML. Définition de l’[extension de balisage {RelativeSource}](relativesource-markup-extension.md). |
| [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) | Spécifie la source de données de l’objet. Dans l’extension de balisage **Binding**, la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) nécessite une référence d’objet comme par exemple une référence de [l’extension de balisage {StaticResource}](staticresource-markup-extension.md). Si cette propriété n’est pas spécifiée, le contexte de données actif spécifie la source. Il est plus normal de ne pas spécifier une valeur Source dans les liaisons individuelles et de compter plutôt sur l’élément **DataContext** partagé pour plusieurs liaisons. Pour plus d’informations, voir [**DataContext**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.datacontext.aspx) ou [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946). |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | Spécifie une valeur à afficher quand la valeur de la source est résolue, mais est explicitement **null**. |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | Spécifie le minutage des mises à jour de la source de liaison. Si cette valeur n’est pas spécifiée, la valeur par défaut est **Default**. |

**Remarque**si vous convertissez un balisage à partir de **{x: Bind}** **{** Binding}, tenez compte des différences des valeurs par défaut pour la propriété **Mode** .

[**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) et **ConverterLanguage** sont tous liés au scénario de conversion d’une valeur ou d’un type de la source de liaison en type ou valeur compatible avec la propriété cible de liaison. Pour obtenir plus d’informations et des exemples, voir la section « Conversions de données » de [Présentation détaillée de la liaison de données](https://msdn.microsoft.com/library/windows/apps/mt210946).

> [!NOTE]
> Depuis Windows10, version1607, l’infrastructure XAML fournit un convertisseur intégré permettant de convertir un booléen en Visibility. Le convertisseur mappe **true** à la valeur d’énumération **Visible**et **false** à la valeur d’énumération **Collapsed**. Vous pouvez ainsi lier une propriété Visibility à un booléen sans avoir à créer un convertisseur. Pour utiliser le convertisseur intégré, la version du SDK cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas l’utiliser si votre application cible des versions antérieures de Windows10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

[**Source**](https://msdn.microsoft.com/library/windows/apps/br209832), [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) et [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) spécifient une source de liaison, ils s’excluent donc mutuellement.

**Conseil**si vous avez besoin de spécifier une accolade simple pour une valeur, comme dans le [**chemin d’accès**](https://msdn.microsoft.com/library/windows/apps/br209830) ou [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), faites-la précéder d’une barre oblique inverse: `\{`. Vous pouvez également placer l’ensemble de la chaîne qui contient les accolades à échapper dans une paire de guillemets secondaire. Par exemple : `ConverterParameter='{Mix}'`.

## <a name="examples"></a>Exemples

```XML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/>
</Page>
```

Ce deuxième exemple définit quatre propriétés de [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) différentes : [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830), [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209829) et [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826). Dans le cas présent, **Path** est explicitement nommée comme étant une propriété de **Binding**. La propriété **Path** est évaluée à une source de liaison de données qui est un autre objet de la même arborescence d’objets d’exécution, un [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) nommé `sliderValueConverter`.

Notez comment la valeur de propriété [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) utilise une autre extension de balisage, l’[extension de balisage {StaticResource}](staticresource-markup-extension.md). Il y a donc deux utilisations d’extensions de balisage imbriquées ici. L’utilisation la plus imbriquée est évaluée en premier. Ainsi, une fois la ressource obtenue, il y a une classe [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) (classe personnalisée qui est instanciée par l’élément `local:S2Formatter` dans les ressources) que la liaison peut utiliser.

## <a name="tools-support"></a>Prise en charge des outils

Microsoft IntelliSense dans Microsoft Visual Studio affiche les propriétés du contexte de données lors de l’écriture de **{Binding}** dans l’éditeur de balisage XAML. Dès que vous tapez « {Binding », les propriétés de contexte de données appropriées pour [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) sont affichées dans la liste déroulante. IntelliSense est également utile avec les autres propriétés de [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Pour ce faire, vous devez disposer soit du contexte de données, soit du contexte de données au moment de la conception défini dans la page de balisage. **Atteindre la définition** (F12) fonctionne également avec **{Binding}**. Vous pouvez aussi utiliser la boîte de dialogue de liaison de données.

 
