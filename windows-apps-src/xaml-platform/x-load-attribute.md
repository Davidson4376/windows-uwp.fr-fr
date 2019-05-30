---
title: Attribut xLoad
description: xLoad permet la création dynamique et la destruction d’un élément et de ses enfants, la réduction du temps de démarrage et de l’utilisation de la mémoire.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d85051aabdb7631c5bdb84e08d6d10a0f70d6ede
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372296"
---
# <a name="xload-attribute"></a>Attribut x:Load

Vous pouvez utiliser **x:Load** pour optimiser le démarrage, la création de l’arborescence d’éléments visuels et l’utilisation de la mémoire de votre application XAML. Utiliser **x:Load** a un effet similaire à **Visibility**, sauf que lorsque l’élément n’est pas chargé, sa mémoire est publiée et un petit espace réservé est utilisé en interne pour marquer son emplacement dans l’arborescence visuelle.

L’élément d’interface utilisateur ayant pour attribut x:Load peut être chargé et déchargé par le biais de code, ou à l’aide d’une expression [x:Bind](x-bind-markup-extension.md). Cela permet de réduire les coûts des éléments qui sont affichés rarement ou dans certaines conditions. Lorsque vous utilisez x:Load sur un conteneur comme Grid ou StackPanel, le conteneur et tous ses enfants sont chargés ou déchargés en tant que groupe.

Le suivi des éléments différés par l’infrastructure XAML ajoute environ 600 octets à l’utilisation de la mémoire pour chaque élément ayant pour attribut x:Load, pour prendre en compte l’espace réservé. Par conséquent, un usage abusif de cet attribut peut entraîner une diminution réelle des performances. Nous recommandons de l’utiliser uniquement sur les éléments qui doivent être masqués. Si vous utilisez x:Load sur un conteneur, la surcharge est payée uniquement pour l’élément avec l’attribut x:Load.

> [!IMPORTANT]
> L’attribut x : Load est disponible à partir de Windows 10, version 1703 (Creator Update). La version minimale ciblée par votre projet Visual Studio doit être *Windows 10 Creators Update (10.0, build 15063)* pour pouvoir utiliser x:Load.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Chargement des éléments

Il existe plusieurs manières de charger les éléments :

- Utilisez une expression [x:Bind](x-bind-markup-extension.md) pour spécifier l’état de chargement. L’expression doit renvoyer **true** pour charger l’élément et **false** pour le décharger.
- Appelez [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) avec le nom que vous avez défini sur l’élément.
- Appelez [**GetTemplateChild**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) avec le nom que vous avez défini sur l’élément.
- Dans un [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState), utilisez une animation [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) ou **Storyboard** qui cible l’élément x:Load.
- Ciblez l’élément déchargé dans un **Storyboard**.

> REMARQUE : Une fois que l’instanciation d’un élément a démarré, il est créé sur le thread d’interface utilisateur, il risquerait de provoquer l’interface utilisateur pour être perturbée si trop que grande partie est créée en même temps.

Une fois qu’un élément différé est créé par une des méthodes répertoriées précédemment, plusieurs choses se passent :

- L’événement [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) sur l’élément est déclenché.
- Le champ de x:Name est défini.
- Toutes les liaisons x:Bind sur l’élément sont évaluées.
- Si vous vous êtes inscrit pour recevoir des notifications de modification de propriété sur la propriété contenant les éléments différés, la notification est déclenchée.

## <a name="unloading-elements"></a>Déchargement des éléments

Pour décharger un élément :

- Utilisez une expression x:Bind pour spécifier l’état de chargement. L’expression doit renvoyer **true** pour charger l’élément et **false** pour le décharger.
- Dans un élément Page ou UserControl, appelez **UnloadObject** et transmettez la référence d’objet
- Appelez **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** et transmettez la référence d’objet

Lorsqu’un objet est déchargé, il sera remplacé dans l’arborescence par un espace réservé. L’instance d’objet reste dans la mémoire jusqu’à ce que toutes les références aient été publiées. L’API UnloadObject sur un élément Page/UserControl est conçue pour libérer les références détenues par codegen pour x:Name et x:Bind. Si vous maintenez des références supplémentaires dans le code d’application, elles devront également être publiées.

Lorsqu’un élément est déchargé, tous les états associés à l’élément sont ignorés, de même si vous utilisez x:Load en tant que version optimisée de Visibility, vérifiez ensuite que les états sont appliqués via des liaisons, ou sont de nouveau appliqués par le code lorsque l’événement Loaded est déclenché.

## <a name="restrictions"></a>Restrictions

Les restrictions pour l’utilisation de **x:Load** sont les suivantes :

- Vous devez définir un [x : Name](x-name-attribute.md) pour l’élément, en tant que cet emplacement doit être un moyen pour rechercher l’élément plus tard.
- Vous ne pouvez utiliser x:Load que sur les types qui dérivent de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) ou de [**FlyoutBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase).
- Vous ne pouvez pas utiliser x:Load sur les éléments racines dans un élément [**Page**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page), [**UserControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol), ou [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate).
- Vous ne pouvez pas utiliser x:Load sur les éléments dans un [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary).
- Vous ne pouvez pas utiliser x:Load sur un XAML libre chargé avec [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load).
- Le déplacement d’un élément parent efface tous les éléments qui n’ont pas été chargés.

## <a name="remarks"></a>Notes

Vous pouvez utiliser x:Load sur des éléments imbriqués, mais ils doivent être réalisés à partir de l’élément le plus éloigné.  Si vous tentez de réaliser un élément enfant avant que le parent soit réalisé, une exception est levée.

En règle générale, nous vous conseillons de différer les éléments qui ne sont pas visibles dans la première image. Une bonne indication pour identifier les éléments à différer consiste à rechercher des éléments créés avec une [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) réduite. De même, une interface utilisateur déclenchée par l’utilisateur est également un bon endroit où rechercher des éléments à différer.

Soyez prudent avant de différer des éléments dans un contrôle [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), car si le temps de démarrage s’en trouve réduit, cela peut également réduire les performances des mouvements panoramiques, selon ce que vous créez. Si vous cherchez à accroître les performances des mouvements panoramiques, consultez la documentation sur [l’extension de balisage {x:Bind}](x-bind-markup-extension.md) et [l’attribut x:Phase](x-phase-attribute.md).

Si l’[attribut x:Phase](x-phase-attribute.md) est utilisé en association avec **x:Load**, lors de la réalisation d’un élément ou d’une arborescence d’éléments, les liaisons sont appliquées jusqu’à la phase actuelle, celle-ci étant elle-même incluse. La phase spécifiée pour **x:Phase** affecte ou contrôle l’état de chargement de l’élément. Lorsqu’un élément de liste est recyclé dans le cadre d’un mouvement panoramique, les éléments réalisés se comportent de la même manière que les autres éléments actifs, et les liaisons compilées (liaisons **{x:Bind}** ) sont traitées à l’aide des mêmes règles, y compris l’exécution par phases.

Il est généralement conseillé de mesurer les performances de votre application avant et après afin de vous assurer d’obtenir les performances souhaitées.

## <a name="example"></a>Exemple

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```

