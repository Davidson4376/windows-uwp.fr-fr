---
author: jwmsft
title: Attribut xDeferLoadStrategy
description: "xDeferLoadStrategy retarde la création d’un élément et ses enfants. Cela réduit le temps de démarrage, mais augmente légèrement l’utilisation de la mémoire. Chaque élément affecté ajoute environ 600 octets à l’utilisation de la mémoire."
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: b989a31439444f06dacb86adb186f853d1637f6c

---

# Attribut x&#58;DeferLoadStrategy

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**x:DeferLoadStrategy="Lazy"** retarde la création d’un élément et ses enfants. Cela réduit le temps de démarrage, mais augmente légèrement l’utilisation de la mémoire. Chaque élément affecté ajoute environ 600octets à l’utilisation de la mémoire. Plus l’arborescence d’éléments que vous différez est importante, plus vous gagnez du temps de démarrage, mais au prix d’un encombrement mémoire supérieur. Par conséquent, un usage abusif de cet attribut peut entraîner une diminution des performances.

## Utilisation des attributs XAML

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## Remarques

Les restrictions pour l’utilisation de **x:DeferLoadStrategy** sont les suivantes :

-   Nécessite un [x: Name](x-name-attribute.md) défini, car il doit être possible de trouver l’élément ultérieurement.
-   Seul un [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) peut être marqué comme différé, à l’exception des types dérivés de [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249).
-   Des éléments racines ne peuvent pas être différés dans [**Page**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page), [**UserControls**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.usercontrol) ou [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348).
-   Les éléments d’un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) ne peuvent pas être différés.
-   Ne fonctionne pas avec un XAML libre chargé avec [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048).
-   Le déplacement d’un élément parent efface tous les éléments qui n’ont pas été réalisés.

Il existe plusieurs manières de réaliser les éléments différés :

-   Appeler [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) avec le nom défini sur l’élément.
-   Appeler [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416) avec le nom défini sur l’élément.
-   Dans un [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007), utiliser une animation [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou **Storyboard** qui cible l’élément différé.
-   Cibler l’élément différé dans un **Storyboard**.
-   Utiliser une liaison qui cible l’élément différé.
-   NOTE : une fois que l’instanciation d’un élément a démarré, celui-ci est créé sur le thread de l’interface utilisateur, au risque que celle-ci s’interrompe si ce qui est créé en une fois est trop important.

Une fois qu’un élément différé est créé par une des méthodes répertoriées ci-dessus, plusieurs choses se passent :

-   L’événement [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) sur l’élément est déclenché.
-   Toutes les liaisons sur l’élément sont évaluées.
-   Si l’application est inscrite pour recevoir des notifications de modification de propriété sur la propriété contenant les éléments différés, la notification est déclenchée.

Vous pouvez imbriquer des éléments différés, mais ils doivent être réalisés à partir de l’élément le plus éloigné.  Si vous tentez de réaliser un élément enfant avant que le parent soit réalisé, une exception est levée.

En règle générale, la recommandation est de différer ce qui n’est pas visible dans le premier cadre.  Une bonne indication pour identifier les éléments à différer consiste à rechercher des éléments créés avec une [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) réduite.  De même, une interface utilisateur accessoire (c’est-à-dire déclenchée par l’utilisateur) est également un bon endroit où rechercher des éléments à différer.  

Soyez prudent avant de différer des éléments dans le cas d’un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878), car si le temps de démarrage s’en trouve réduit, cela peut également réduire les performances des mouvements panoramiques, selon ce que vous créez.  Si vous cherchez à accroître les performances des mouvements panoramiques, reportez-vous à la documentation sur [l’extension de balisage {x:Bind}](x-bind-markup-extension.md) et [l’attribut x:Phase](x-phase-attribute.md).

Si l’[attribut x:Phase](x-phase-attribute.md) est utilisé en association avec **x:DeferLoadStrategy**, lors de la réalisation d’un élément ou d’une arborescence d’éléments, les liaisons sont appliquées jusqu’à la phase actuelle, celle-ci étant elle-même incluse. La phase spécifiée pour **x:Phase** n’affecte pas et ne contrôle pas le report de l’élément. Lorsqu’un élément de liste est recyclé dans le cadre d’un mouvement panoramique, les éléments réalisés se comportent de la même manière que les autres éléments actifs, et les liaisons compilées (liaisons **{x:Bind}**) sont traitées à l’aide des mêmes règles, y compris l’exécution par phases.

Il est généralement conseillé de mesurer votre application avant et après afin d’être sûr d’obtenir les performances souhaitées.

## Exemple

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
    this.FindName("DeferredGrid"); // This will realize the deferred grid
}
```




<!--HONumber=Jun16_HO4-->


