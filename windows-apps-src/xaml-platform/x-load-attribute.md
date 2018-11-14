---
author: jwmsft
title: attribut de xLoad
description: xLoad permet la création dynamique et la destruction d’un élément et ses enfants, réduction de l’utilisation de temps et de mémoire au démarrage. 
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d9659d183c020c579aa0a21fe179a69c1d9997c5
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6656251"
---
# <a name="xload-attribute"></a>Attribut x:Load

Vous pouvez utiliser **x: Load** pour optimiser le démarrage, création de l’arborescence d’éléments visuels et d’utilisation de mémoire de votre application XAML. À l’aide de **x: Load** a un effet visuel similaire à la **visibilité**, mais lorsque l’élément n’est pas chargée, sa mémoire est publiée et en interne un espace réservé petit est utilisé pour marquer sa place dans l’arborescence visuelle.

L’élément d’interface utilisateur que l’attribut x: Load peut être chargé et déchargé par le biais du code, ou à l’aide d’une expression [x: Bind](x-bind-markup-extension.md) . Cela permet de réduire les coûts des éléments qui sont affichés rarement ou dans certaines conditions. Lorsque vous utilisez x: Load sur un conteneur comme grille ou StackPanel, le conteneur et tous ses enfants leur chargement ou déchargement en tant que groupe.

Le suivi des éléments différés par l’infrastructure XAML ajoute environ 600 octets à l’utilisation de la mémoire pour chaque élément de l’attribut x: Load, pour prendre en compte l’espace réservé. Par conséquent, il est possible d’abusif de cet attribut dans la mesure où les performances de votre diminuent réellement. Nous vous recommandons d’utiliser uniquement sur les éléments qui doivent être masqués. Si vous utilisez x: Load sur un conteneur, la surcharge est payée uniquement pour l’élément avec l’attribut x: Load.

> [!IMPORTANT]
> L’attribut x: Load est disponible à partir de Windows 10, version 1703 (Creators Update). La version minimale ciblée par votre projet Visual Studio doit être *Windows10Creators Update (10.0, build15063)* pour pouvoir utiliser x:Load.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Chargement d’éléments

Il existe différentes méthodes pour charger les éléments:

- Utiliser une expression [x: Bind](x-bind-markup-extension.md) pour spécifier l’état de chargement. L’expression doit retourner **true** pour charger et **false** pour décharger l’élément.
- Appelez [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) avec le nom que vous avez défini sur l’élément.
- Appelez [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) avec le nom que vous avez défini sur l’élément.
- Dans un [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), utilisez une animation [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou de **table de montage séquentiel** qui cible l’élément de x: Load.
- Cibler l’élément déchargée dans n’importe quel **table de montage séquentiel**.

> NOTE : une fois que l’instanciation d’un élément a démarré, celui-ci est créé sur le thread de l’interface utilisateur, au risque que celle-ci s’interrompe si ce qui est créé en une fois est trop important.

Une fois qu’un élément différé est créé par une des méthodes répertoriées précédemment, plusieurs choses se passent:

- L’événement [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) sur l’élément est déclenché.
- Le champ pour x: Name est défini.
- Toutes les liaisons x: Bind sur l’élément sont évaluées.
- Si vous vous êtes inscrit pour recevoir des notifications de modification de propriété sur la propriété contenant les éléments différés, la notification est déclenchée.

## <a name="unloading-elements"></a>Éléments de déchargement

Décharger un élément:

- Utiliser une expression x: Bind pour spécifier l’état de chargement. L’expression doit retourner **true** pour charger et **false** pour décharger l’élément.
- Dans une Page ou d’un UserControl, appelez **UnloadObject** et transmettez la référence d’objet
- Appelez **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** et passez la référence d’objet

Lorsqu’un objet est déchargé, il est remplacé dans l’arborescence avec un espace réservé. L’instance d’objet reste dans la mémoire jusqu'à ce que toutes les références ont été publiées. L’API UnloadObject sur un Page/UserControl est conçu pour libérer les références détenus par codegen pour x: Name et x: Bind. Si vous maintenez des références supplémentaires dans le code d’application qu'auront également besoin d’être libéré.

Lorsqu’un élément est déchargé, état associé à l’élément sera ignoré, par conséquent, si vous utilisez x: Load comme une version optimisée de visibilité, puis vérifiez tout état est appliqué par le biais des liaisons, ou est nouveau appliqué par du code lors de l’événement Loaded est déclenché.

## <a name="restrictions"></a>Restrictions

Les restrictions pour l’utilisation de **x: Load** sont les suivantes:

- Vous devez définir un [x: Name](x-name-attribute.md)pour l’élément, en tant que cet emplacement doit être un moyen de trouver l’élément ultérieurement.
- Vous pouvez uniquement utiliser x: Load sur les types qui dérivent de [**l’élément UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) ou [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
- Vous ne pouvez pas utiliser x: Load sur les éléments racines dans une [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), un [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)ou un [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
- Vous ne pouvez pas utiliser x: Load sur les éléments dans un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
- Vous ne pouvez pas utiliser x: Load sur un XAML libre chargé avec [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
- Le déplacement d’un élément parent efface tous les éléments qui n’ont pas été chargés.

## <a name="remarks"></a>Remarques

Vous pouvez utiliser x: Load sur les éléments imbriqués, mais ils doivent être réalisés à partir de l’élément le plus éloigné dans. Si vous tentez de réaliser un élément enfant avant que le parent soit réalisé, une exception est levée.

En règle générale, nous vous conseillons de différer les éléments qui ne sont pas visibles dans la première image.Une bonne indication pour identifier les éléments à différer consiste à rechercher des éléments créés avec une [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) réduite. De même, une interface utilisateur déclenchée par l’utilisateur est également un bon endroit où rechercher des éléments à différer.

Soyez prudent avant de différer des éléments dans un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), car si le temps de démarrage s’en trouve réduit, cela peut également réduire les performances des mouvements panoramiques, selon ce que vous créez. Si vous cherchez à accroître les performances des mouvements panoramiques, consultez la documentation sur [l’extension de balisage {x:Bind}](x-bind-markup-extension.md) et [l’attribut x:Phase](x-phase-attribute.md).

Si l' [attribut x: Phase](x-phase-attribute.md) est utilisé conjointement avec **x: Load** ensuite, lors de la réalisation d’un élément ou une arborescence d’éléments, les liaisons sont appliquées jusqu'à la phase actuelle. La phase spécifiée pour **x: Phase** affectent ou contrôler l’état de chargement de l’élément. Lorsqu’un élément de liste est recyclé dans le cadre d’un mouvement panoramique, réalisés éléments se comportent de la même manière que sur d’autres éléments actifs, et les liaisons compilées (liaisons **{x: Bind}** ) sont traitées à l’aide des mêmes règles, y compris l’exécution par phases.

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

