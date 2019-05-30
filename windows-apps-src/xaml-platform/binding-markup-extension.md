---
description: L’extension de balisage Binding est convertie au moment du chargement XAML en une instance de la classe Binding.
title: Extension de balisage Binding
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c2d54e9077429c6526168492c31a9d24a43a1be8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366528"
---
# <a name="binding-markup-extension"></a>Extension de balisage {Binding}


**Remarque**  un nouveau mécanisme de liaison est disponible pour Windows 10, qui est optimisé pour la productivité des développeurs et les performances. Voir [extension de balisage {x:Bind}](x-bind-markup-extension.md).

**Remarque**  pour des informations générales sur l’utilisation de la liaison de données dans votre application avec **{Binding}** (et pour une comparaison global entre **{x : Bind}** et **{Binding}** ), consultez [liaison de données en profondeur](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

Le **{Binding}** extension de balisage est utilisée pour les propriétés de liaison de données sur les contrôles à des valeurs provenant d’une source de données tel que le code. L’extension de balisage **{Binding}** est convertie au moment du chargement XAML en une instance de la classe [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Cet objet de liaison obtient une valeur d’une propriété sur une source de données et la transmet à la propriété sur le contrôle. L’objet de liaison peut éventuellement être configuré pour observer les modifications de la valeur de la propriété de source de données, et se mettre à jour en fonction de ces modifications. Il peut également être configuré pour renvoyer les modifications de la valeur de contrôle à la propriété source. La propriété qui est la cible de la liaison de données doit être une propriété de dépendance. Pour plus d’informations, voir [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md).

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
| *propName* | Nom de chaîne de la propriété à définir sur l’objet [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Par exemple, « Convertisseur ». |
| *value* | Valeur à attribuer à la propriété. La syntaxe de l’argument dépend de la propriété de la section [Propriétés de la classe de liaison pouvant être définies avec {Binding}](#properties-of-the-binding-class-that-can-be-set-with-binding) ci-dessous. |

## <a name="property-path"></a>Chemin de propriété

[**Chemin d’accès** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) décrit la propriété que vous procédez à la liaison (la propriété source). Path est un paramètre positionnel, ce qui signifie que vous pouvez utiliser le nom du paramètre explicitement (`{Binding Path=EmployeeID}`) ou que vous pouvez le spécifier comme premier paramètre sans nom (`{Binding EmployeeID}`).

Le type de [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) est un chemin de propriété, qui est une chaîne correspondant à une propriété ou sous-propriété de votre type personnalisé ou d’un type d’infrastructure. Le type peut être, mais n’est pas nécessairement, un [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). Les étapes dans un chemin de propriété sont délimitées par des points (.), et vous pouvez inclure plusieurs délimiteurs pour parcourir des sous-propriétés successives. Utilisez le point délimiteur quel que soit le langage de programmation utilisé pour implémenter l’objet cible de la liaison.

Par exemple, pour lier une interface utilisateur à la propriété Prénom d’un objet employé, votre chemin de propriété pourrait être « Employee.FirstName ». Si vous liez un contrôle d’éléments à une propriété contenant des dépendances d’un employé, votre chemin de propriété pourrait être « Employee.Dependents », et le modèle d’élément du contrôle d’éléments se chargerait de l’affichage des éléments dans « Dependents ».

Si la source de données est une collection, un chemin de propriété peut spécifier les éléments de la collection selon leur position ou index. Par exemple, « équipes\[0\]. Lecteurs », où le littéral «\[\]» englobe le « 0 » qui spécifie le premier élément dans une collection.

Lorsque vous utilisez une liaison [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) vers un objet [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) existant, vous pouvez utiliser des propriétés jointes comme parties intégrantes du chemin de propriété. Pour lever toute ambiguïté concernant une propriété jointe et éviter que le point intermédiaire figurant dans le nom de la propriété ne soit considéré comme une étape dans un chemin de propriété, mettez le nom de propriété jointe qualifié par le propriétaire entre parenthèses ; par exemple, `(AutomationProperties.Name)`.

Un objet intermédiaire de chemin de propriété est stocké en tant qu’objet [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath) dans une représentation d’exécution, mais la plupart des cas ne nécessitent pas d’interaction avec un objet **PropertyPath** dans le code. Vous pouvez généralement spécifier les informations de liaison dont vous avez besoin avec XAML.

Pour plus d’informations sur la syntaxe de chaîne d’un chemin de propriété, des chemins de propriété dans les zones de fonctionnalités d’animation et la construction d’un objet [**PropertyPath**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyPath), voir [Syntaxe de PropertyPath](property-path-syntax.md).

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>Propriétés de la classe de liaison pouvant être définies avec {Binding}


**{Binding}** est illustrée avec la syntaxe de l’espace réservé *bindingProperties*, car plusieurs propriétés en lecture/écriture d’un [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) peuvent être définies dans l’extension de balisage. Les propriétés peuvent être définies dans n’importe quel ordre à l’aide de paires *propName*=*value* séparées par des virgules. Certaines propriétés requièrent des types sans conversion de type. Elles nécessitent donc leurs propres extensions de balisage imbriquées dans la **{Binding}** .

| Propriété | Description |
|----------|-------------|
| [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) | Voir la section [Chemin de propriété](#property-path) ci-dessus. |
| [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) | Spécifie un objet convertisseur appelé par le moteur de liaison. Le convertisseur peut être défini dans le balisage à l’aide de l’[extension de balisage {StaticResource}](staticresource-markup-extension.md) pour faire référence à cet objet dans un dictionnaire de ressources. |
| [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) | Spécifie la culture que doit utiliser le convertisseur. (Si vous définissez [ **convertisseur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter).) La culture est définie comme un identificateur basée sur des normes. Pour plus d’informations, voir [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) | Spécifie un paramètre de convertisseur qui peut être utilisé dans la logique du convertisseur. (Si vous définissez [ **convertisseur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter).) La plupart des convertisseurs utiliser une logique simple permettant d’obtenir toutes les informations dont ils ont besoin à partir de la valeur passée à convertir, et vous n’avez pas besoin un **ConverterParameter** valeur. Le paramètre **ConverterParameter** est destiné aux implémentations de convertisseur plus complexes ayant une logique conditionnelle basée sur ce qui est transmis dans **ConverterParameter**. Vous pouvez écrire un convertisseur qui utilise des valeurs qui ne sont pas des chaînes, mais il s’agit d’un scénario peu courant. Pour plus d’informations, voir Remarques dans **ConverterParameter**. |
| [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) | Spécifie une source de données en faisant référence à un autre élément dans la même construction XAML qui possède une propriété **Name** ou un attribut [x:Name](x-name-attribute.md). Cette propriété est souvent utilisée pour partager des valeurs associées ou utiliser des sous-propriétés d’un élément d’interface utilisateur pour fournir une valeur spécifique pour un autre élément, par exemple dans un modèle de contrôle XAML. |
| [**FallbackValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) | Spécifie une valeur à afficher quand la source ou le chemin ne peuvent pas être résolus. |
| [**Mode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode) | Spécifie le mode de liaison, comme l’une des valeurs suivantes : « OneTime », « OneWay » ou « TwoWay ». Ces valeurs correspondent aux noms de constantes de l’énumération [**BindingMode**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingMode). La valeur par défaut est « OneWay ». Notez qu’elle diffère de la valeur par défaut de **{x:Bind}** , qui est « OneTime ». | 
| [**RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) | Spécifie une source de données en décrivant la position de la source de liaison par rapport à la position de la cible de liaison. Ceci est le plus souvent utilisé dans les liaisons au sein de modèles de contrôles XAML. Définition de l’[extension de balisage {RelativeSource}](relativesource-markup-extension.md). |
| [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) | Spécifie la source de données de l’objet. Dans l’extension de balisage **Binding**, la propriété [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source) nécessite une référence d’objet comme par exemple une référence de [l’extension de balisage {StaticResource}](staticresource-markup-extension.md). Si cette propriété n’est pas spécifiée, le contexte de données actif spécifie la source. Il est plus normal de ne pas spécifier une valeur Source dans les liaisons individuelles et de compter plutôt sur l’élément **DataContext** partagé pour plusieurs liaisons. Pour plus d’informations, voir [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) ou [Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth). |
| [**TargetNullValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.targetnullvalue) | Spécifie une valeur à afficher quand la valeur de la source est résolue, mais est explicitement **null**. |
| [**UpdateSourceTrigger**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.updatesourcetrigger) | Spécifie le minutage des mises à jour de la source de liaison. Si cette valeur n’est pas spécifiée, la valeur par défaut est **Default**. |

**Remarque**  si vous convertissez le balisage à partir de **{x : Bind}** à **{Binding}** , puis être conscient des différences dans les valeurs par défaut pour le **Mode** propriété.

[**Convertisseur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [ **ConverterLanguage** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) et **ConverterLanguage** sont toutes associées au scénario de conversion d’une valeur ou type à partir du source de liaison dans un type ou une valeur qui est compatible avec la propriété de cible de liaison. Pour obtenir plus d’informations et des exemples, voir la section « Conversions de données » de [Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

> [!NOTE]
> Depuis Windows 10, version 1607, l’infrastructure XAML fournit un convertisseur intégré permettant de convertir un booléen en Visibility. Le convertisseur mappe **true** à la valeur d’énumération **Visible**et **false** à la valeur d’énumération **Collapsed**. Vous pouvez ainsi lier une propriété Visibility à un booléen sans avoir à créer un convertisseur. Pour utiliser le convertisseur intégré, la version du SDK cible de votre application doit être 14393 ou une version ultérieure. Vous ne pouvez pas l’utiliser si votre application cible des versions antérieures de Windows 10. Pour plus d’informations sur les versions cibles, voir [Code adaptatif de version](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

[**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.source), [ **RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource), et [ **ElementName** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) spécifier une source de liaison, afin qu’ils soient qui s’excluent mutuellement.

**Conseil**  si vous devez spécifier une seule accolade pour une valeur, comme dans [ **chemin d’accès** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) ou [ **ConverterParameter** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), puis du faire précéder d’une barre oblique inverse : `\{`. Vous pouvez également placer l’ensemble de la chaîne qui contient les accolades à échapper dans une paire de guillemets secondaire. Par exemple : `ConverterParameter='{Mix}'`.

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

Le deuxième exemple définit quatre différents [ **liaison** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) propriétés : [**ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname), [ **chemin d’accès**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path), [ **Mode** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.mode) et [ **convertisseur** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter). Dans le cas présent, **Path** est explicitement nommée comme étant une propriété de **Binding**. La propriété **Path** est évaluée à une source de liaison de données qui est un autre objet de la même arborescence d’objets d’exécution, un [**Slider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) nommé `sliderValueConverter`.

Notez comment la valeur de propriété [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) utilise une autre extension de balisage, l’[extension de balisage {StaticResource}](staticresource-markup-extension.md). Il y a donc deux utilisations d’extensions de balisage imbriquées ici. L’utilisation la plus imbriquée est évaluée en premier. Ainsi, une fois la ressource obtenue, il y a une classe [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) (classe personnalisée qui est instanciée par l’élément `local:S2Formatter` dans les ressources) que la liaison peut utiliser.

## <a name="tools-support"></a>Prise en charge des outils

Microsoft IntelliSense dans Microsoft Visual Studio affiche les propriétés du contexte de données lors de l’écriture de **{Binding}** dans l’éditeur de balisage XAML. Dès que vous tapez « {Binding », les propriétés de contexte de données appropriées pour [**Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) sont affichées dans la liste déroulante. IntelliSense est également utile avec les autres propriétés de [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding). Pour ce faire, vous devez disposer soit du contexte de données, soit du contexte de données au moment de la conception défini dans la page de balisage. **Atteindre la définition** (F12) fonctionne également avec **{Binding}** . Vous pouvez aussi utiliser la boîte de dialogue de liaison de données.

 
