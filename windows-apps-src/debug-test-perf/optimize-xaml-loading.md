---
author: jwmsft
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Optimiser votre balisage XAML
description: L’analyse du balisage XAML pour la construction d’objets en mémoire est chronophage pour une interface utilisateur complexe. Voici quelques astuces pour améliorer l’analyse du balisage XAML ainsi que l’efficacité du temps de chargement et de la mémoire de votre application.
ms.author: jimwalk
ms.date: 08/10/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 884825f2e9639f620d8db4e6110791fddf2d7e77
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5830159"
---
# <a name="optimize-your-xaml-markup"></a>Optimiser votre balisage XAML


L’analyse du balisage XAML pour la construction d’objets en mémoire est chronophage pour une interface utilisateur complexe. Voici quelques astuces pour améliorer l’analyse et le temps de chargement de votre balisage XAML, ainsi que l'efficacité de la mémoire de votre application.

Au démarrage, limitez le balisage XAML qui se charge uniquement à ce dont vous avez besoin pour votre interface utilisateur initiale. Examinez le balisage dans votre page initiale (y compris les ressources de page) et vérifiez que vous ne chargez pas des éléments supplémentaires qui ne sont pas immédiatement nécessaires. Ces éléments peuvent provenir de diverses sources, telles que des dictionnaires de ressources, des éléments initialement réduits ainsi que des éléments dessinés sur d’autres éléments.

L’optimisation de votre code XAML pour plus d’efficacité nécessite des compromis; il n’existe pas toujours une solution unique pour chaque situation. Nous allons examiner certains problèmes courants et vous fournir quelques conseils qui vous aideront à faire les meilleurs choix possibles pour votre application.

## <a name="minimize-element-count"></a>Limiter le nombre d’éléments

Bien que la plateforme XAML soit capable d’afficher un grand nombre d’éléments, vous pouvez accélérer la disposition et le rendu de votre application en utilisant le moins d’éléments possible pour obtenir les effets visuels recherchés.

Les choix que vous effectuez pour disposer vos contrôles d’interface utilisateur affectent le nombre d’éléments d’interface utilisateur qui sont créés au démarrage de votre application. Pour plus d’informations sur l'optimisation de la disposition, voir [Optimiser votre disposition XAML](optimize-your-xaml-layout.md).

Le nombre d’éléments est extrêmement important dans les modèles de données dans la mesure où chaque élément est créé à nouveau pour chaque élément de données. Pour plus d’informations sur la réduction du nombre d’éléments dans une liste ou une grille, voir *Réduction des éléments par élément* dans l'article [Optimisation des options d’interface ListView et GridView](optimize-gridview-and-listview.md).

Nous allons examiner ici d'autres façons de réduire le nombre d’éléments que votre application doit charger au démarrage.

### <a name="defer-item-creation"></a>Différer la création d’éléments

Si votre balisage XAML contient des éléments que vous n'affichez pas immédiatement, vous pouvez différer le chargement de ces derniers jusqu'à leur affichage. Par exemple, vous pouvez différer la création d'un contenu non visible, tel qu’un onglet secondaire dans une interface utilisateur à onglets. Ou, vous pouvez afficher les éléments dans une grille par défaut, en offrant à l'utilisateur la possibilité d'afficher les données dans une liste. Vous pouvez ainsi retarder le chargement de la liste jusqu'à ce qu’elle soit nécessaire.

Utilisez l'[attribut x:Load](../xaml-platform/x-load-attribute.md) au lieu de la propriété [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.Visibility) pour contrôler les cas dans lesquels un élément s’affiche. Si la visibilité d’un élément est définie sur **Collapsed**, l'élément sera ignoré lors de la passe de rendu, mais les coûts de l’instance d’objet en mémoire vous seront malgré tout facturés. Si vous utilisez x:Load à la place, l'infrastructure ne créera pas l’instance d’objet tant que celle-ci n'est pas nécessaire, ce qui contribuera à réduire les coûts mémoire. Le seul inconvénient est que vous payez une petite surcharge de mémoire (de 600octets environ) lorsque l’interface utilisateur n’est pas chargée.

> [!NOTE]
> Vous pouvez différer le chargement d’éléments en utilisant l'attribut [x:Load](../xaml-platform/x-load-attribute.md) ou [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md). L’attribut x:Lad est disponible à partir de Windows10CreatorUpdate (version1703, SDKbail15063). La version minimale ciblée par votre projet Visual Studio doit être *Windows10Creators Update (10.0, build15063)* pour pouvoir utiliser x:Load. Pour cibler des versions antérieures, utilisez x:DeferLoadStrategy.

Les exemples suivants illustrent la différence de nombre d’éléments et de mémoire utilisée quand différentes techniques sont utilisées pour masquer les éléments d’interface utilisateur. Les contrôles ListView et GridView contenant des éléments identiques sont placés dans l'élément Grid racine d’une page. ListView n’est pas visible, contrairement à GridView qui est affiché. Dans chacun de ces exemples, le code XAML génère la même interface utilisateur sur l’écran. Nous utilisons les [outils de profilage et de performances](tools-for-profiling-and-performance.md) de VisualStudio pour vérifier le nombre d'éléments et la mémoire utilisée.

#### <a name="option-1---inefficient"></a>Option 1 - Inefficace

Ici, ListView est chargé, mais il n’est pas visible car sa largeur est nulle. ListView et chacun de ses éléments enfants sont créés dans l’arborescence visuelle et chargés en mémoire.

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <ListView x:Name="List1" Width="0">
        <ListViewItem>Item 1</ListViewItem>
        <ListViewItem>Item 2</ListViewItem>
        <ListViewItem>Item 3</ListViewItem>
        <ListViewItem>Item 4</ListViewItem>
        <ListViewItem>Item 5</ListViewItem>
        <ListViewItem>Item 6</ListViewItem>
        <ListViewItem>Item 7</ListViewItem>
        <ListViewItem>Item 8</ListViewItem>
        <ListViewItem>Item 9</ListViewItem>
        <ListViewItem>Item 10</ListViewItem>
    </ListView>

    <GridView x:Name="Grid1">
        <GridViewItem>Item 1</GridViewItem>
        <GridViewItem>Item 2</GridViewItem>
        <GridViewItem>Item 3</GridViewItem>
        <GridViewItem>Item 4</GridViewItem>
        <GridViewItem>Item 5</GridViewItem>
        <GridViewItem>Item 6</GridViewItem>
        <GridViewItem>Item 7</GridViewItem>
        <GridViewItem>Item 8</GridViewItem>
        <GridViewItem>Item 9</GridViewItem>
        <GridViewItem>Item 10</GridViewItem>
    </GridView>
</Grid>
```

Arborescence visuelle dynamique avec ListView chargé. Le nombre total d’éléments de la page est 89.

![Arborescence visuelle avec un affichage liste](images/visual-tree-1.png)

ListView et ses éléments enfants sont chargés en mémoire.

![Arborescence visuelle avec un affichage liste](images/memory-use-1.png)

#### <a name="option-2---better"></a>Option 2: Meilleure

Ici, la propriété Visibility de ListView est définie sur Collapsed (l'autre code XAML est identique à l’original). ListView est créé dans l’arborescence visuelle, mais pas ses éléments enfants. Toutefois, ces derniers sont chargés en mémoire, de sorte que la mémoire utilisée est identique à celle de l’exemple précédent.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

Arborescence visuelle dynamique avec ListView défini sur Collapsed. Le nombre total d’éléments de la page est 46.

![Arborescence visuelle avec un affichage liste réduit](images/visual-tree-2.png)

ListView et ses éléments enfants sont chargés en mémoire.

![Arborescence visuelle avec un affichage liste](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>Option 3 - Efficacité optimale

Ici, l’attribut x:Load de ListView est défini sur **False** (l’autre code XAML est identique à l’original). ListView n’est pas créé dans l’arborescence visuelle ni chargé en mémoire au démarrage.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

Arborescence visuelle dynamique avec ListView non chargé. Le nombre total d’éléments de la page est 45.

![Arborescence visuelle avec un affichage liste non chargé.](images/visual-tree-3.png)

ListView et ses éléments enfants ne sont pas chargés en mémoire.

![Arborescence visuelle avec un affichage liste](images/memory-use-3.png)

> [!NOTE]
> Le nombre d’éléments et la mémoire utilisée dans ces exemples sont très petits et servent uniquement à illustrer le concept. Dans ces exemples, la surcharge liée à l’utilisation de x:Load est supérieure aux gains de mémoire réalisés, de sorte que l'application n'en tire aucun bénéfice. Vous devez utiliser les outils de profilage sur votre application pour déterminer si celle-ci bénéficiera d'un chargement différé.

### <a name="use-layout-panel-properties"></a>Utiliser les propriétés des panneaux de disposition

Les panneaux de disposition ont une propriété [Background](https://msdn.microsoft.com/library/windows/apps/BR227512), il n’est donc pas nécessaire de placer un [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) devant un panneau dans le but de le colorier.

**Inefficace**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid>
    <Rectangle Fill="Black"/>
</Grid>
```

**Efficace**

```xaml
<Grid Background="Black"/>
```

Les panneaux de disposition intègrent également des propriétés de bordure, il n'est donc pas nécessaire de placer un élément [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) autour d'un tel panneau. Voir [Optimiser votre disposition XAML](optimize-your-xaml-layout.md) pour obtenir des informations et des exemples supplémentaires.

### <a name="use-images-in-place-of-vector-based-elements"></a>Utiliser des images à la place des éléments vectoriels

Si vous réutilisez suffisamment souvent le même élément vectoriel, il est alors plus efficace d’utiliser à la place un élément [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image). Les éléments vectoriels peuvent être plus onéreux car l’unité centrale doit créer chaque élément séparément. Le fichier image ne doit être décodé qu’une seule fois.

## <a name="optimize-resources-and-resource-dictionaries"></a>Optimiser les ressources et les dictionnaires de ressources

Vous utilisez généralement des [dictionnaires de ressources](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md) pour stocker dans plusieurs endroits de votre application, à un niveau global, les ressources que vous souhaitez référencer. Il s’agit par exemple, des styles, des pinceaux, des modèles et ainsi de suite.

D'une façon générale, nous avons optimisé [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) pour qu’il n’instancie pas de ressources, sauf demande contraire. Mais il existe des situations que vous devez éviter afin que les ressources ne soient pas instanciées inutilement.

### <a name="resources-with-xname"></a>Ressources pourvues de x:Name

Utilisez l'[attribut x:Key](../xaml-platform/x-key-attribute.md) pour référencer vos ressources. Les ressources pourvues de l'[attribut x:Name](../xaml-platform/x-name-attribute.md) ne bénéficient pas de l’optimisation de la plateforme; elles sont instanciées dès que le ResourceDictionary est créé. En effet, x:Name indique à la plateforme que votre application doit accéder à cette ressource. La plateforme doit donc créer un élément à référencer.

### <a name="resourcedictionary-in-a-usercontrol"></a>ResourceDictionary dans un UserControl

Un ResourceDictionary défini dans un [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) entraîne une pénalité. La plateforme crée une copie d’un tel ResourceDictionary pour chaque instance du UserControl. Si le UserControl est énormément utilisé, déplacez le ResourceDictionary en dehors du UserControl et placez-le au niveau de la page.

### <a name="resource-and-resourcedictionary-scope"></a>Étendue Ressource et ResourceDictionary

Si une page référence un contrôle utilisateur ou une ressource définis dans un autre fichier, alors l’infrastructure analyse également ce fichier.

Dans cet exemple, étant donné que le fichier _InitialPage.xaml_ utilise une ressource provenant du fichier _ExampleResourceDictionary.xaml_, la totalité du fichier _ExampleResourceDictionary.xaml_ doit être analysée au démarrage.

**InitialPage.xaml.**

```xaml
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
        are used in the app, but are not needed during startup.-->
</ResourceDictionary>
```

Si vous utilisez une ressource sur plusieurs pages au sein de votre application, l’enregistrer dans _App.xaml_ constitue une bonne pratique qui permet d’éviter les doublons. Mais _App.xaml_ est analysé lors du démarrage de l’application afin que toutes les ressources qui ne sont utilisées que dans une seule page (à moins qu’il ne s’agisse de la page d’accueil) soient placées dans les ressources locales de la page. Cet exemple montre _App.xaml_ contenant des ressources qui ne sont utilisées que par une seule page (qui n’est pas la page d’accueil). Cela augmente inutilement le temps de démarrage de l’application.

**App.xaml**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Application ...>
     <Application.Resources>
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/>
    </Application.Resources>
</Application>
```

**InitialPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <StackPanel>
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/>
    </StackPanel>
</Page>
```

**SecondPage.xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.SecondPage" ...>
    <StackPanel>
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" />
    </StackPanel>
</Page>
```

Afin de rendre cet exemple plus efficace, déplacez `SecondPageTextBrush` dans _SecondPage.xaml_ et `ThirdPageTextBrush` dans _ThirdPage.xaml_. `InitialPageTextBrush` peut rester dans _App.xaml_ car les ressources de l’application doivent, dans tous les cas, être analysées au démarrage de l’application.

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>Consolider plusieurs pinceaux ayant la même apparence dans une même ressource

La plateforme XAML essaie de mettre en cache les objets couramment utilisés afin qu’ils puissent l’être aussi souvent que possible. Toutefois, le code XAML ne peut pas facilement identifier si un pinceau déclaré dans un balisage est le même qu’un pinceau déclaré dans un balisage différent. L’exemple ci-dessous utilise [SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962), mais c’est encore plus probable et important avec [GradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210068). Recherchez également les pinceaux utilisant des couleurs prédéfinies, par exemple: `"Orange"` et `"#FFFFA500"` sont de la même couleur.

**Inefficace.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBox>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

Pour éviter les doublons, définissez le pinceau en tant que ressource. Si des contrôles figurant dans d’autres pages utilisent le même pinceau, déplacez-le dans _App.xaml_.

**Efficace.**

```xaml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>

    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## <a name="minimize-overdrawing"></a>Limiter le surdessin

Le surdessin désigne le fait de dessiner plusieurs objets dans les mêmes pixels d’un écran. Il est parfois nécessaire de trouver un compromis entre ces instructions et la volonté de réduire le nombre d’éléments.

Utilisez [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) pour effectuer un diagnostic visuel. Vous verrez peut-être apparaître dans la scène des objets dont vous ne soupçonniez pas l’existence.

### <a name="transparent-or-hidden-elements"></a>Éléments transparents ou masqués

Si un élément est invisible, car il est transparent ou masqué derrière d’autres éléments, et qu’il n’est pas utilisé pour la disposition, alors supprimez-le. Si l’élément n’est pas visible dans l’état visuel initial, mais qu’il apparaît dans d’autres états visuels, utilisez x:Load pour contrôler son état ou définissez [Visibility](https://msdn.microsoft.com/library/windows/apps/BR208992) sur **Collapsed** au niveau de l’élément proprement dit et remplacez la valeur par **Visible** dans les états appropriés. Il y a toutefois des exceptions: en règle générale, la valeur d’une propriété dans la plupart des états visuels est mieux définie localement sur l’élément.

### <a name="composite-elements"></a>Éléments composites

Utilisez un élément composite au lieu de disposer en couches les différents éléments pour créer un effet. Dans cet exemple, le résultat est une forme bicolore dans laquelle la moitié supérieure est noire (depuis l’arrière-plan de la [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704)) et la moitié inférieure est grise (depuis le [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) blanc semi-transparent fusionné à l’aide du canal alpha sur l’arrière-plan noir de la **Grid**). Ici, 150% des pixels nécessaires pour obtenir le résultat sont remplis.

**Inefficace.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Black">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/>
</Grid>
```

**Efficace.**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Fill="Black"/>
    <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
</Grid>
```

### <a name="layout-panels"></a>Panneaux de disposition

Un panneau de disposition peut servir à deux choses : colorier une zone et disposer les éléments enfants. Si un élément plus éloigné dans l’ordre Z colore déjà une zone, alors un panneau de disposition situé au premier plan n’a pas besoin de la colorer également. À la place, il peut simplement se concentrer sur la disposition de ses enfants. Voici un exemple.

**Inefficace.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid Background="Blue"/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

**Efficace.**

```xaml
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

Si la [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) doit faire l’objet d’un test d’atteinte, définissez alors une valeur d’arrière-plan de transparent.

### <a name="borders"></a>Bordures

Utilisez un élément [Border](https://msdn.microsoft.com/library/windows/apps/BR209253) pour dessiner une bordure autour d’un objet. Dans cet exemple, une [Grid](https://msdn.microsoft.com/library/windows/apps/BR242704) est utilisée comme bordure autour d’une [TextBox](https://msdn.microsoft.com/library/windows/apps/BR209683). Mais tous les pixels de la cellule centrale sont surdessinés.

**Inefficace.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Blue" Width="300" Height="45">
    <Grid.RowDefinitions>
        <RowDefinition Height="5"/>
        <RowDefinition/>
        <RowDefinition Height="5"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="5"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="5"/>
    </Grid.ColumnDefinitions>
    <TextBox Grid.Row="1" Grid.Column="1"></TextBox>
</Grid>
```

**Efficace.**

```xaml
 <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
     <TextBox/>
</Border>
```

### <a name="margins"></a>Marges

Tenez compte des marges. Deux éléments voisins risquent de se chevaucher si des marges négatives débordent les limites de rendu et provoquent un surdessin.

### <a name="cache-static-content"></a>Contenu statique du cache

Une forme constituée de nombreux éléments qui se chevauchent peut également occasionner un surdessin. Si vous configurez [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084) sur **BitmapCache** sur l’[UIElement](https://msdn.microsoft.com/library/windows/apps/BR208911) contenant la forme composite, la plateforme affiche alors l’élément dans une image bitmap une seule fois, puis utilise cette image bitmap dans chaque image au lieu d’avoir recours au surdessin.

**Inefficace.**

```xaml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![Diagramme de Venn à trois cercles unis](images/solidvenn.png)

L'image ci-dessus présente le résultat, mais voici une carte indiquant les zones surdessinées. Le rouge foncé indique l’excès de surdessin.

![Diagramme de Venn illustrant les zones de superposition](images/translucentvenn.png)

**Efficace.**

```xaml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Notez l’utilisation du [CacheMode](https://msdn.microsoft.com/library/windows/apps/BR228084). N’utilisez pas cette technique si l’une des formes secondaires est animée, car le cache d’images bitmap devra probablement être régénéré à chaque image, ce qui irait à l’encontre de l’intention souhaitée.

## <a name="use-xbf2"></a>Utiliser XBF2

XBF2 est une représentation binaire du balisage XAML, qui permet d’éviter les coûts liés à l’analyse du texte lors de l’exécution. Elle optimise également votre binaire pour la création de charge et d’arborescence, et permet des types XAML « fast-path » pour améliorer les coûts de création de tas et d’objet, par exemple VSM, ResourceDictionary, Styles, etc. Elle est complètement mappée en mémoire. Il n’existe donc pas d’encombrement du tas pour le chargement et la lecture d’une page XAML. En outre, elle permet de réduire l’encombrement disque des pages XAML stockées dans un appx. XBF2 est une représentation plus compacte, et elle peut réduire l’encombrement disque des fichiers XAML/XBF1 comparatifs jusqu’à 50 %. Par exemple, l’application Photos intégrée présente une réduction d’environ 60 % après la conversion en XBF2, en passant d’1 Mo de ressources XBF1 à environ 400 Ko de ressources XBF2. Nous constatons également que les applications passent de 15 à 20 % dans le processeur, et de 10 à 15 % dans le tas Win32.

Les contrôles et dictionnaires intégrés dans XAML, qui sont fournis par l’infrastructure sont déjà entièrement compatibles XBF2. Pour votre propre application, vérifiez que votre fichier projet déclare TargetPlatformVersion 8.2 ou ultérieur.

Pour vérifier si vous possédez XBF2, ouvrez votre application dans un éditeur binaire ; les 12e et 13e octets correspondent à 00 02 si vous possédez XBF2.

## <a name="related-articles"></a>Articles connexes

- [Meilleures pratiques en matière de performances de démarrage de votre application](best-practices-for-your-app-s-startup-performance.md)
- [Optimiser votre disposition XAML](optimize-your-xaml-layout.md)
- [Optimisation des options d’interface ListView et GridView](optimize-gridview-and-listview.md)
- [Outils de profilage et de performances](tools-for-profiling-and-performance.md)
