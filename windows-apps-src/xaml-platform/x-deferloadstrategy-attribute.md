---
title: Attribut xDeferLoadStrategy
description: xDeferLoadStrategy retarde la création d’un élément et ses enfants. Cela réduit le temps de démarrage, mais augmente légèrement l’utilisation de la mémoire. Chaque élément affecté ajoute environ 600 octets à l’utilisation de la mémoire.
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f445b186c42095bfdf6d10fa7960b78691ced792
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371091"
---
# <a name="xdeferloadstrategy-attribute"></a>attribut x: DeferLoadStrategy

> [!IMPORTANT]
> À compter de Windows 10, version 1703 (Creators Update), **x:DeferLoadStrategy** est remplacé par l’[**attribut x:Load**](x-load-attribute.md). Utiliser `x:Load="False"` équivaut à utiliser `x:DeferLoadStrategy="Lazy"`, mais permet de décharger l’interface utilisateur si nécessaire. Voir l’[attribut x:Load](x-load-attribute.md) pour plus d’informations.

Vous pouvez utiliser **x:DeferLoadStrategy="Lazy"** pour optimiser le démarrage ou les performances de création d’arborescence de votre application XAML. Lorsque vous utilisez **x:DeferLoadStrategy="Lazy"** , la création d’un élément et de ses enfants est retardée, ce qui réduit le temps de démarrage et les coûts mémoire. Cela permet de réduire les coûts des éléments qui sont affichés rarement ou dans certaines conditions. L’élément est réalisé lorsqu’il est référencé dans le code ou dans VisualStateManager.

Toutefois, le suivi des éléments différés par l’infrastructure XAML ajoute environ 600 octets à l’utilisation de la mémoire pour chaque élément affecté. Plus l’arborescence d’éléments que vous reportez est importante, plus vous économisez de temps de démarrage, mais au prix d’une plus grande empreinte mémoire. Par conséquent, un usage abusif de cet attribut peut entraîner une diminution des performances.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>Notes

Les restrictions pour l’utilisation de **x:DeferLoadStrategy** sont les suivantes :

- Vous devez définir un [x : Name](x-name-attribute.md) pour l’élément, en tant que cet emplacement doit être un moyen pour rechercher l’élément plus tard.
- Vous pouvez différer uniquement les types qui dérivent de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) ou de [**FlyoutBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- Vous ne pouvez pas différer d’éléments racines dans un objet [**Page**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page), [**UserControls**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) ou [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate).
- Vous ne pouvez pas différer d’éléments dans un [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- Vous ne pouvez pas différer un XAML libre chargé avec [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Le déplacement d’un élément parent efface tous les éléments qui n’ont pas été réalisés.

Il existe plusieurs manières de réaliser les éléments différés :

- Appelez [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) avec le nom que vous avez défini sur l’élément.
- Appelez [**GetTemplateChild**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) avec le nom que vous avez défini sur l’élément.
- Dans un [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState), utilisez une animation [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) ou **Storyboard** qui cible l’élément différé.
- Cibler l’élément différé dans un **Storyboard**.
- Utilisez une liaison qui cible l’élément différé.

> REMARQUE : Une fois que l’instanciation d’un élément a démarré, il est créé sur le thread d’interface utilisateur, il risquerait de provoquer l’interface utilisateur pour être perturbée si trop que grande partie est créée en même temps.

Une fois qu’un élément différé est créé par une des méthodes répertoriées précédemment, plusieurs choses se passent :

- L’événement [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) sur l’élément est déclenché.
- Toutes les liaisons sur l’élément sont évaluées.
- Si vous vous êtes inscrit pour recevoir des notifications de modification de propriété sur la propriété contenant les éléments différés, la notification est déclenchée.

Vous pouvez imbriquer des éléments différés, mais ils doivent être réalisés à partir de l’élément le plus éloigné.  Si vous tentez de réaliser un élément enfant avant que le parent soit réalisé, une exception est levée.

En règle générale, nous vous conseillons de différer les éléments qui ne sont pas visibles dans la première image. Une bonne indication pour identifier les éléments à différer consiste à rechercher des éléments créés avec une [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) réduite. De même, une interface utilisateur déclenchée par l’utilisateur est également un bon endroit où rechercher des éléments à différer.

Soyez prudent avant de différer des éléments dans un contrôle [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), car si le temps de démarrage s’en trouve réduit, cela peut également réduire les performances des mouvements panoramiques, selon ce que vous créez. Si vous cherchez à accroître les performances des mouvements panoramiques, consultez la documentation sur [l’extension de balisage {x:Bind}](x-bind-markup-extension.md) et [l’attribut x:Phase](x-phase-attribute.md).

Si l’[attribut x:Phase](x-phase-attribute.md) est utilisé en association avec **x:DeferLoadStrategy**, lors de la réalisation d’un élément ou d’une arborescence d’éléments, les liaisons sont appliquées jusqu’à la phase actuelle, celle-ci étant elle-même incluse. La phase spécifiée pour **x:Phase** n’affecte pas et ne contrôle pas le report de l’élément. Lorsqu’un élément de liste est recyclé dans le cadre d’un mouvement panoramique, les éléments réalisés se comportent de la même manière que les autres éléments actifs, et les liaisons compilées (liaisons **{x:Bind}** ) sont traitées à l’aide des mêmes règles, y compris l’exécution par phases.

Il est généralement conseillé de mesurer les performances de votre application avant et après afin de vous assurer d’obtenir les performances souhaitées.

## <a name="example"></a>Exemple

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```
