---
author: QuinnRadich
Description: Use layout panels to arrange and group UI elements in your app.
title: Panneaux de disposition des applications de plateforme Windows universelle (UWP)
ms.author: quradic
ms.date: 04/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a839379150ecd38fc1925c81d4e11d588018011f
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4565022"
---
# <a name="layout-panels"></a>Panneaux de disposition

Les panneaux de disposition sont des conteneurs qui vous permettent d’organiser et de regrouper des éléments d’interface utilisateur dans votre application. Les panneaux de disposition XAML intégrés incluent [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx), [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) et [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx). Nous décrivons ici chaque panneau et la manière de les utiliser pour disposer des éléments d'interface utilisateur XAML.

Plusieurs éléments sont à prendre en considération lors du choix d’un panneau de disposition :
- la manière dont le panneau positionne ses éléments enfants ;
- la manière dont le panneau dimensionne ses éléments enfants ;
- la manière dont les éléments enfants superposés s’empilent les uns sur les autres (ordre de plan) ;
- le nombre et la complexité des éléments de panneau imbriqués nécessaires pour créer la disposition souhaitée.

## <a name="panel-properties"></a>Propriétés des panneaux

Avant d’aborder les panneaux individuels, étudions certaines propriétés communes à tous les panneaux. 

### <a name="panel-attached-properties"></a>Propriétés jointes du panneau

La plupart des panneaux de disposition XAML utilisent des propriétés jointes pour permettre à leurs éléments enfants d’informer le panneau parent sur la manière dont ils doivent être placés dans l’interface utilisateur. Les propriétés jointes utilisent la syntaxe *AttachedPropertyProvider.PropertyName*. Si des panneaux sont imbriqués dans d’autres panneaux, les propriétés jointes des éléments d’interface utilisateur spécifiant les caractéristiques de disposition à un parent sont interprétées par le panneau parent le plus proche uniquement.

Voici un exemple de la façon dont vous pouvez définir la propriété jointe [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) sur un contrôle Button en XAML. Cela informe l’élément Canvas parent que le contrôle Button doit être positionné à 50 pixels effectifs du bord gauche de l’élément Canvas.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

Pour plus d’informations sur les propriétés jointes, voir [Vue d’ensemble des propriétés jointes](../../xaml-platform/attached-properties-overview.md).

### <a name="panel-borders"></a>Bordures des panneaux

Les panneaux RelativePanel, StackPanel et Grid définissent les propriétés de bordure qui vous permettent de dessiner une bordure autour du panneau sans les encapsuler dans un autre élément Border. Les propriétés de bordure sont **BorderBrush**, **BorderThickness**, **CornerRadius** et **Padding**.

Voici un exemple illustrant comment définir les propriétés de bordure sur un élément Grid.

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![Un élément Grid avec des bordures](images/layout-panel-grid-border.png)

L’utilisation des propriétés de bordure intégrées réduit le nombre d’éléments XAML, ce qui peut améliorer les performances de l’interface utilisateur de votre application. Pour plus d’informations sur les panneaux de disposition et les performances de l’interface utilisateur, voir [Optimiser votre disposition XAML](https://msdn.microsoft.com/en-us/library/windows/apps/mt404609.aspx).

## <a name="relativepanel"></a>RelativePanel

[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) vous permet de disposer des éléments d’interface utilisateur en spécifiant leur emplacement en fonction d’autres éléments et par rapport au panneau. Par défaut, un élément est positionné dans le coin supérieur gauche du panneau. Vous pouvez utiliser RelativePanel avec [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) et [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) pour réorganiser votre interface utilisateur pour différentes tailles de fenêtre.

Ce tableau indique les propriétés jointes que vous pouvez utiliser pour aligner un élément par rapport au panneau ou à d’autres éléments.

Alignement du panneau | Alignement frère | Position sœur
----------------|-------------------|-----------------
[**AlignTopWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwithpanel.aspx) | [**AlignTopWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aligntopwith.aspx) | [**Above**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.above.aspx)  
[**AlignBottomWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwithpanel.aspx) | [**AlignBottomWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignbottomwith.aspx) | [**Below**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.below.aspx)  
[**AlignLeftWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwithpanel.aspx) | [**AlignLeftWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignleftwith.aspx) | [**LeftOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.leftof.aspx)  
[**AlignRightWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwithpanel.aspx) | [**AlignRightWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignrightwith.aspx) | [**RightOf**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.rightof.aspx)  
[**AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) | [**AlignHorizontalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwith.aspx) | &nbsp;   
[**AlignVerticalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanel.aspx) | [**AlignVerticalCenterWith**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignverticalcenterwith.aspx) | &nbsp;   

 
Ce code XAML montre comment organiser des éléments dans un élément RelativePanel.

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Orange"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

Le résultat se présente ainsi: 

![Panneau relatif](images/layout-panel-relative-panel.png)

Voici quelques informations à noter concernant le dimensionnement des rectangles:
- Le rectangle rouge est fourni dans une taille explicite de 44 x 44. Il est placé dans le coin supérieur gauche du panneau, qui est la position par défaut.
- Le rectangle vert est fourni dans une hauteur explicite de 44. Le côté gauche est aligné avec le rectangle rouge, et son côté droit est aligné avec le rectangle bleu, ce qui détermine sa largeur.
- Le rectangle orange n’est pas fourni dans une taille explicite. Son côté gauche est aligné avec le rectangle bleu. Ses bords droit et inférieur sont alignés avec le bord du panneau. Sa taille est déterminée par ces alignements et elle est redimensionnées lorsque le panneau est redimensionné.

## <a name="stackpanel"></a>StackPanel

[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) organise ses éléments enfants sur une seule ligne orientable horizontalement ou verticalement. StackPanel est généralement utilisé pour organiser une petite sous-section de l’interface utilisateur sur une page.

Vous pouvez utiliser la propriété [**Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.orientation.aspx) pour préciser l’orientation des éléments enfants. L’orientation par défaut est [**Vertical**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.orientation.aspx).

Le code XAML suivant indique comment créer un empilement StackPanel vertical des éléments.

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Orange" Height="44"/>
</StackPanel>
```


Le résultat se présente ainsi:

![Panneau d’empilement](images/layout-panel-stack-panel.png)

Dans un panneau StackPanel, si la taille d’un élément enfant n’est pas définie explicitement, ce dernier s’étire pour remplir la largeur disponible (ou la hauteur si la propriété Orientation est définie sur **Horizontal**). Dans cet exemple, la largeur des rectangles n’est pas définie. Les rectangles se développent afin d’occuper toute la largeur du panneau StackPanel.

## <a name="grid"></a>Grid

Le panneau [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) prend en charge les dispositions fluides et vous permet d’organiser des contrôles dans des dispositions constituées de plusieurs lignes et colonnes. Vous pouvez spécifier les lignes et les colonnes d’un panneau Grid à l’aide des propriétés [**RowDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowdefinitions.aspx) et [**ColumnDefinitions**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columndefinitions.aspx).

Pour positionner les objets dans des cellules spécifiques du panneau Grid, utilisez les propriétés jointes [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.column.aspx) et [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.row.aspx).

Pour forcer le contenu à s’étendre sur plusieurs lignes et colonnes, utilisez les propriétés jointes [**Grid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.rowspan.aspx) et [**Grid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.columnspan.aspx).

Cet exemple XAML montre comment créer un élément Grid à deuxlignes et deuxcolonnes.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


Le résultat se présente ainsi:

![Grille](images/layout-panel-grid.png)

Dans cet exemple, le dimensionnement fonctionne comme suit. 
- La deuxième ligne a une hauteur explicite de 44 pixels effectifs. Par défaut, la hauteur de la première ligne remplit l’espace restant disponible.
- La largeur de la première colonne est définie sur **Auto**, de manière à être suffisamment large pour ses enfants. Dans ce cas, elle est de 44 pixels efficaces pour s’adapter à la largeur du rectangle rouge.
- Il n’existe aucune autre contrainte de taille sur les rectangles, de sorte que chacun d’entre eux s’étire pour remplir la cellule de grille qui le contient.

Vous pouvez répartir l’espace au sein d’une colonne ou d’une ligne en utilisant le redimensionnement **Auto** ou proportionnel. Le dimensionnement automatique permet de redimensionner les éléments d’interface utilisateur pour qu’ils s’adaptent à leur contenu ou à leur conteneur parent. Vous pouvez également utiliser le dimensionnement automatique avec les lignes et les colonnes d’une grille. Pour utiliser le dimensionnement automatique, définissez la propriété Height et/ou Width des éléments d’interface utilisateur sur **Auto**.

Le *dimensionnement proportionnel* sert à répartir l’espace disponible entre les lignes et les colonnes d’une grille par proportions pondérées. En XAML, les valeurs proportionnelles sont exprimées par \* (ou *n*\* pour le dimensionnement proportionnel pondéré). À titre d’exemple, dans une disposition à deux colonnes, pour spécifier qu’une colonne est cinq fois plus large que l’autre colonne, utilisez « 5\* » et « \* » pour les propriétés [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx) des éléments [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx).

Cet exemple combine le dimensionnement fixe, automatique et proportionnel dans un élément [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) avec 4colonnes.

&nbsp;|&nbsp;|&nbsp;
------|------|------
Colonne_1 | **Auto** | La taille de la colonne s’adaptera à son contenu.
Colonne_2 | * | Une fois les colonnes Auto calculées, la colonne conserve une partie de la largeur restante. Colonne_2 sera deux fois moins large que Colonne_4.
Colonne_3 | **44** | La colonne aura une largeur de 44 pixels.
Colonne_4 | **2**\* | Une fois les colonnes Auto calculées, la colonne conserve une partie de la largeur restante. Colonne_4 sera deux fois plus large que Colonne_2.

La largeur par défaut de la colonne est «*», de sorte que vous n’avez pas besoin de définir explicitement cette valeur pour la deuxième colonne.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

Dans le concepteur XAML de Visual Studio, le résultat se présente comme suit.

![Grille de 4 colonnes dans le concepteur Visual Studio](images/xaml-layout-grid-in-designer.png)

## <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid

[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) est un panneau de disposition de style Grid dans lequel les lignes ou les colonnes sont automatiquement renvoyées à la ligne ou dans une nouvelle colonne lorsque la valeur [**MaximumRowsOrColumns**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns.aspx) est atteinte. 

La propriété [**Orientation**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.orientation.aspx) spécifie si la grille ajoute ses éléments en lignes ou en colonnes avant leur renvoi. L’orientation par défaut est **Vertical**, ce qui signifie que la grille ajoute des éléments de haut en bas jusqu’à ce qu’une colonne soit remplie, puis passe à une nouvelle colonne. Lorsque la valeur est **Horizontal**, la grille ajoute des éléments de gauche à droite, puis passe à la ligne suivante.

Les dimensions des cellules sont spécifiées par [**ItemHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight.aspx) et [**ItemWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth.aspx). Chaque cellule a la même taille. Si les valeurs ItemHeight ou ItemWidth ne sont pas spécifiées, la première cellule est redimensionnée pour s’adapter à son contenu, et toutes les autres cellules prennent la même taille.

Vous pouvez utiliser les propriétés jointes [**VariableSizedWrapGrid.ColumnSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.columnspan.aspx) et [**VariableSizedWrapGrid.RowSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.rowspan.aspx) pour spécifier le nombre de cellules adjacentes devant être remplies par un élément enfant.

Voici comment utiliser un élément VariableSizedWrapGrid en XAML.

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


Le résultat se présente ainsi:

![Grille avec renvoi à la ligne à taille variable](images/layout-panel-variable-size-wrap-grid.png)

Dans cet exemple, le nombre maximal de lignes dans chaque colonne est 3. La première colonne contient seulement 2 éléments (les rectangles rouge et bleu), car le rectangle bleu s'étend sur 2 lignes. Le rectangle vert s’étend ensuite jusqu’en haut de la colonne suivante.

## <a name="canvas"></a>Zone de dessin

Le panneau [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) positionne ses éléments enfants à l’aide de points de coordonnées fixes et ne prend pas en charge les dispositions fluides. Vous spécifiez les points des éléments enfants individuels en définissant les propriétés jointes [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.left.aspx) et [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.top.aspx) de chaque élément. L’élément Canvas parent lit les valeurs des propriétés jointes de ses enfants pendant la transmission de disposition [Arrange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.arrange.aspx).

Dans un élément Canvas, les objets peuvent se chevaucher, auquel cas un objet est dessiné sur un autre objet. Par défaut, l’élément Canvas restitue les objets enfants dans l’ordre dans lequel ils sont déclarés, de sorte que le dernier enfant est restitué en haut (chaque élément a une valeur ZIndex par défaut de 0). Il en va de même pour les autres panneaux intégrés. Toutefois, l’élément Canvas prend également en charge la propriété jointe [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) que vous pouvez définir sur chacun des éléments enfants. Vous pouvez définir cette propriété dans le code pour changer l’ordre de dessin des éléments pendant l’exécution. L’élément avec la valeur Canvas.ZIndex la plus élevée est dessiné en dernier. Ainsi, il est dessiné au-dessus des autres éléments qui partagent le même espace ou qui se chevauchent. Notez que la valeur alpha (transparence) est respectée. Ainsi, même si des éléments se chevauchent, le contenu affiché dans les zones de chevauchement peut être fusionné si le contenu supérieur a une valeur alpha non maximale.

L’élément Canvas ne procède à aucun redimensionnement de ses enfants. Chaque élément doit spécifier sa taille.

Voici un exemple d’élément Canvas en XAML.

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

Le résultat se présente ainsi:

![Canevas](images/layout-panel-canvas.png)

Utilisez le panneau Canvas en fonction de vos besoins. Bien qu’il soit pratique de pouvoir contrôler précisément les positions des éléments de l’interface utilisateur dans certains scénarios, un panneau de disposition positionné de manière fixe rend cette zone de l’interface utilisateur moins adaptable à l’ensemble des modifications de taille de fenêtre de l’application. Le redimensionnement des fenêtres d’application peut être dû au changement d’orientation de l’appareil, au fractionnement des fenêtres d’application, au changement de moniteur, ainsi qu’à plusieurs autres scénarios utilisateur.

## <a name="panels-for-itemscontrol"></a>Panneaux pour ItemsControl

Il existe plusieurs panneaux à usage spécifique qui peuvent être utilisés uniquement comme un contrôle [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) pour afficher des éléments dans un contrôle [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx). Il s’agit de [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemsstackpanel.aspx), [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemswrapgrid.aspx), [**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.virtualizingstackpanel.aspx) et [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.wrapgrid.aspx). Vous ne pouvez pas utiliser ces panneaux pour créer une disposition générale de l’interface utilisateur.

