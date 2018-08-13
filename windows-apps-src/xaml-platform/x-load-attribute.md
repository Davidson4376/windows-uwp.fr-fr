---
author: jwmsft
title: attribut xLoad
description: xLoad permet la création dynamique et la destruction d’un élément et ses enfants, une diminution de l’utilisation des temps et de mémoire de démarrage.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f34ea43b981de65c81e9cce2b8a896c3577e595
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "610788"
---
# <a name="xload-attribute"></a>Attribut x:Load

Vous pouvez utiliser **X: charge** afin d’optimiser le démarrage, création de l’arborescence visual et utilisation de la mémoire de votre application XAML. À l’aide de **X: charge** a un effet visuel similaire à la **visibilité**, sauf que lorsque l’élément n’est pas chargé, sa mémoire est publiée et un espace réservé de petite taille est utilisé en interne pour marquer sa place dans l’arborescence visuelle.

L’élément d’interface utilisateur attribué avec x: charge peut être chargé et déchargé via le code, ou à l’aide d’une expression [: Lier x](x-bind-markup-extension.md) . Cela permet de réduire les coûts des éléments qui sont affichés rarement ou dans certaines conditions. Lorsque vous utilisez un conteneur comme grille ou StackPanel x: charge, le conteneur et tous ses enfants sont chargés ou non chargé en tant que groupe.

Le suivi des éléments différés par framework XAML ajoute environ 600 octets à l’utilisation de la mémoire pour chaque élément de l’attribut x: charge, pour prendre en compte pour l’espace réservé. Par conséquent, il est possible de sur-utilisation de cet attribut dans la mesure où vos performances diminue réellement. Nous vous recommandons d’utiliser uniquement sur les éléments qui doivent être masqué. Si vous utilisez x: charge sur un conteneur, la charge est payée uniquement pour l’élément avec l’attribut x: charge.

> [!IMPORTANT]
> L’attribut x: charge est disponible à partir de Windows 10, version 1703 (créateurs de mise à jour). La version minimale ciblée par votre projet Visual Studio doit être *Windows10Creators Update (10.0, build15063)* pour pouvoir utiliser x:Load.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>Chargement des éléments

Il existe différentes façons de charger les éléments:

- Expression [: Lier x](x-bind-markup-extension.md) permet de définir l’état de chargement. L’expression doit renvoyer **la valeur true** pour charger et **false** pour décharger l’élément.
- Appelez [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) avec le nom que vous avez défini sur l’élément.
- Appelez [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) avec le nom que vous avez défini sur l’élément.
- Dans un [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), utilisez une animation [**Set**](https://msdn.microsoft.com/library/windows/apps/br208817) ou **Storyboard** qui traite l’élément x: charge.
- L’élément déchargé dans n’importe quel **Storyboard**cible.

> NOTE : une fois que l’instanciation d’un élément a démarré, celui-ci est créé sur le thread de l’interface utilisateur, au risque que celle-ci s’interrompe si ce qui est créé en une fois est trop important.

Une fois qu’un élément différé est créé par une des méthodes répertoriées précédemment, plusieurs choses se passent:

- L’événement [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) sur l’élément est déclenché.
- Le champ pour x: Name est défini.
- Toutes les liaisons: lier x sur l’élément sont évaluées.
- Si vous vous êtes inscrit pour recevoir des notifications de modification de propriété sur la propriété contenant les éléments différés, la notification est déclenchée.

## <a name="unloading-elements"></a>Déchargement d’éléments

Pour décharger un élément:

- Expression: lier x permet de définir l’état de chargement. L’expression doit renvoyer **la valeur true** pour charger et **false** pour décharger l’élément.
- Dans une Page ou UserControl, appelez **UnloadObject** et passez dans la référence d’objet
- Appelez **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** et passez dans la référence d’objet

Lorsqu’un objet est déchargé, il sera remplacé avec un espace réservé dans l’arborescence. L’instance de l’objet reste en mémoire jusqu'à ce que toutes les références ont été publiées. L’API UnloadObject sur un Page/UserControl est conçu pour libérer les références de codegen pour x: Name et x: Bind. Si vous maintenez références supplémentaires dans le code de l’application qu'ils devront également être libéré.

Lorsqu’un élément est déchargé, état associé à l’élément sera ignoré, donc si vous utilisez x: charge comme une version optimisée de visibilité, puis vérifiez tous les états temporaires est appliqué par le biais de liaisons, ou est nouveau appliquée par le code lorsque l’événement Loaded est déclenché.

## <a name="restrictions"></a>Restrictions

Les restrictions d’utilisation de **X: charge** sont les suivants:

- Vous devez définir un [x:Name](x-name-attribute.md) pour l’élément, afin de pouvoir retrouver l’élément ultérieurement.
- Vous pouvez uniquement utiliser x: charge sur les types qui dérivent de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) ou [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
- Vous ne pouvez pas utiliser x: charge sur les éléments racine dans une [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page), un [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)ou un [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
- Vous ne pouvez pas utiliser x: charge sur les éléments d’un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).
- Vous ne pouvez pas utiliser x: charge dans XAML libre chargé avec [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
- Déplacement d’un élément parent efface tous les éléments qui n’ont pas été chargés.

## <a name="remarks"></a>Remarques

Vous pouvez utiliser x: charge sur les éléments imbriqués, mais elles doivent être réalisées à partir de l’élément le plus extérieur dans.  Si vous tentez de réaliser un élément enfant avant que le parent soit réalisé, une exception est levée.

En règle générale, nous vous conseillons de différer les éléments qui ne sont pas visibles dans la première image. Une bonne indication pour identifier les éléments à différer consiste à rechercher des éléments créés avec une [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) réduite. De même, une interface utilisateur déclenchée par l’utilisateur est également un bon endroit où rechercher des éléments à différer.

Soyez prudent avant de différer des éléments dans un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), car si le temps de démarrage s’en trouve réduit, cela peut également réduire les performances des mouvements panoramiques, selon ce que vous créez. Si vous cherchez à accroître les performances des mouvements panoramiques, consultez la documentation sur [l’extension de balisage {x:Bind}](x-bind-markup-extension.md) et [l’attribut x:Phase](x-phase-attribute.md).

Si l' [attribut x: Phase](x-phase-attribute.md) est utilisé conjointement avec **X: charge** puis, lorsqu’un élément ou une arborescence d’éléments est réalisée, les liaisons sont appliquées jusqu'à et y compris la phase actuelle. La phase spécifiée pour **x: Phase** affectent ou contrôler l’état de chargement de l’élément. Lorsqu’un élément de liste est recyclé dans le cadre de panoramique, réalisés éléments se comportent de la même manière que d’autres éléments actives et compilées bindings (liaisons **{x: Bind}** ) sont traités en utilisant les mêmes règles, y compris progressive.

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

