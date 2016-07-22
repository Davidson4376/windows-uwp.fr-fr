---
author: mcleblanc
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Optimiser votre balisage XAML
description: "L’analyse du balisage XAML pour la construction d’objets en mémoire est chronophage pour une interface utilisateur complexe. Voici quelques astuces pour améliorer l’analyse du balisage XAML, ainsi que l’efficacité du temps de chargement et de la mémoire de votre application."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c2131b084d8bb989f1f7767f54db697e1cdd8dcf

---
# Optimiser votre balisage XAML

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

L’analyse du balisage XAML pour la construction d’objets en mémoire est chronophage pour une interface utilisateur complexe. Voici quelques astuces pour améliorer l’analyse du balisage XAML ainsi que l’efficacité du temps de chargement et de la mémoire de votre application.

Au démarrage, limitez le balisage XAML qui se charge uniquement à ce dont vous avez besoin pour votre interface utilisateur initiale. Examinez le balisage de votre page initiale et vérifiez qu’il ne contient aucun élément superflu. Si une page fait référence à une commande utilisateur ou à une ressource définie dans un autre fichier, l’infrastructure analyse alors également ce fichier.

Dans cet exemple, étant donné que InitialPage.xaml utilise une ressource provenant de ExampleResourceDictionary.xaml, l'ensemble de ExampleResourceDictionary.xaml doit être analysé au démarrage.

**InitialPage.xaml.**

```xml
<Page x:Class="ExampleNamespace.InitialPage" ...> 
    <UserControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </UserControl.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
    used in the app, but not during startup.-->
</ResourceDictionary>
```

Si vous utilisez une ressource sur plusieurs pages au sein de votre application, l’enregistrer dans App.xaml constitue une bonne pratique qui permet d’éviter les doublons. Mais App.xaml est analysé lors du démarrage de l’application afin que toutes les ressources qui ne sont utilisées que dans une seule page (à moins qu’il ne s’agisse de la page d’accueil) soient placées dans les ressources locales de la page. Ce contre-exemple montre App.xaml contenant des ressources qui ne sont utilisées que par une seule page (qui n’est pas la page d’accueil). Cela augmente inutilement le temps de démarrage de l’application.

**InitialPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->   
    <StackPanel>  
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**SecondPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel> 
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" /> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**App.xaml**

```xml
<Application ...> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
     <Application.Resources>  
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/> 
    </Application.Resources> 
</Application> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

Afin de rendre le contre-exemple présenté ci-dessus plus efficace, il convient de déplacer `SecondPageTextBrush` dans SecondPage.xaml et `ThirdPageTextBrush` dans ThirdPage.xaml. `InitialPageTextBrush` peut rester dans App.xaml car les ressources de l’application doivent, dans tous les cas, être analysées au démarrage de l’application.

## Limiter le nombre d’éléments

Bien que la plateforme XAML soit capable d’afficher un grand nombre d’éléments, vous pouvez accélérer la disposition et le rendu de votre application en utilisant le moins d’éléments possible pour obtenir les effets visuels recherchés.

-   Les panneaux de disposition ont une propriété [**Background**](https://msdn.microsoft.com/library/windows/apps/BR227512), il n’est donc pas nécessaire de placer un [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) devant un panneau dans le but de le colorier.

**Inefficace.**

```xml
<Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Rectangle Fill="Black"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficace.**

```xml
<Grid Background="Black"/>
```

-   Si vous réutilisez suffisamment souvent le même élément vectoriel, il est alors plus efficace d’utiliser à la place un élément [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752). Les éléments vectoriel peuvent être plus onéreux car l’unité centrale doit créer chaque élément séparément. Le fichier image ne doit être décodé qu’une seule fois.

## Consolider plusieurs pinceaux ayant la même apparence dans une même ressource

La plateforme XAML essaie de mettre en cache les objets couramment utilisés afin qu’ils puissent l’être aussi souvent que possible. Toutefois, le code XAML ne peut pas facilement identifier si un pinceau déclaré dans un balisage est le même qu’un pinceau déclaré dans un balisage différent. L’exemple ci-dessous utilise [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), mais c’est encore plus probable et important avec [**GradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210068).

**Inefficace.**

```xml
<Page ... > <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
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
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

Recherchez également les pinceaux utilisant des couleurs prédéfinies : `"Orange"` et `"#FFFFA500"` sont de la même couleur. Pour éviter les doublons, définissez le pinceau en tant que ressource. Si des contrôles figurant dans d’autres pages utilisent le même pinceau, déplacez-le dans App.xaml.

**Efficace.**

```xml
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

## Limiter le surdessin

Le surdessin désigne le fait de dessiner plusieurs objets dans les mêmes pixels d’un écran. Il est parfois nécessaire de trouver un compromis entre ces instructions et la volonté de réduire le nombre d’éléments.

-   Si un élément est invisible, car il est transparent ou masqué derrière d’autres éléments, et qu’il n’est pas utilisé pour la disposition, alors supprimez-le. Si l’élément n’est pas visible dans l’état visuel initial mais qu’il apparaît dans d’autres états visuels, alors définissez [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) sur **Collapsed** sur l’élément lui-même et modifiez la valeur sur **Visible** dans les états appropriés. Il y a toutefois des exceptions: en règle générale, la valeur d’une propriété dans la plupart des états visuels est mieux définie localement sur l’élément.
-   Utilisez un élément composite au lieu de disposer en couches les différents éléments pour créer un effet. Dans cet exemple, le résultat est une forme bicolore dans laquelle la moitié supérieure est noire (depuis l’arrière-plan de la [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)) et la moitié inférieure est grise (depuis le [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) blanc semi-transparent fusionné à l’aide du canal alpha sur l’arrière-plan noir de la **Grid**). Ici, 150% des pixels nécessaires pour obtenir le résultat sont remplis.

**Inefficace.**
    
```xml
    <Grid Background="Black"> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Grid.RowDefinitions> 
            <RowDefinition Height="*"/> 
            <RowDefinition Height="*"/> 
        </Grid.RowDefinitions> 
        <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficace.**

```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Rectangle Fill="Black"/>
        <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
    </Grid>
```

-   Un panneau de disposition peut servir à deux choses : colorier une zone et disposer les éléments enfants. Si un élément plus éloigné dans l’ordre Z colore déjà une zone, alors un panneau de disposition situé au premier plan n’a pas besoin de la colorer également. À la place, il peut simplement se concentrer sur la disposition de ses enfants. Voici un exemple.

**Inefficace.**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid Background="Blue"/> 
            </DataTemplate> 
        </GridView.ItemTemplate> 
    </GridView> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficace.**

```xml
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid/> 
            </DataTemplate>
        </GridView.ItemTemplate>
    </GridView> 
```

Si la [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) doit faire l’objet d’un test d’atteinte, définissez alors une valeur d’arrière-plan de transparent.

-   Utilisez un élément [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209253) pour dessiner une bordure autour d’un objet. Dans cet exemple, une [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) est utilisée comme bordure autour d’une [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683). Mais tous les pixels de la cellule centrale sont surdessinés.

**Inefficace.**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
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
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Efficace.**

```xml
    <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
        <TextBox/>
    </Border>
```

-   Tenez compte des marges. Deux éléments voisins risquent de se chevaucher si des marges négatives débordent les limites de rendu et provoquent un surdessin.

Utilisez [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) pour effectuer un diagnostic visuel. Vous verrez peut-être apparaître dans la scène des objets dont vous ne soupçonniez pas l’existence.

## Contenu statique du cache

Une forme constituée de nombreux éléments qui se chevauchent peut également occasionner un surdessin. Si vous configurez [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084) sur **BitmapCache** sur l’[**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) contenant la forme composite, la plateforme affiche alors l’élément dans une image bitmap une seule fois, puis utilise cette image bitmap dans chaque image au lieu d’avoir recours au surdessin.

**Inefficace.**

```xml
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

```xml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Notez l’utilisation du [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084). N’utilisez pas cette technique si l’une des formes secondaires est animée, car le cache d’images bitmap devra probablement être régénéré à chaque image, ce qui irait à l’encontre de l’intention souhaitée.

## ResourceDictionaries

Les ResourceDictionaries sont généralement utilisés pour stocker vos ressources à un niveau global. Les ressources que votre application souhaite référencer sont stockées dans plusieurs endroits. Il s’agit par exemple, des styles, des pinceaux, des modèles et ainsi de suite. En règle générale, nous optimisons les ResourceDictionaries pour ne pas instancier de ressources sauf demande contraire. Cependant, vous devez être prudent dans quelques emplacements.

**Ressource pourvue de x:Name**. Les ressources pourvues de x:Name ne bénéficient pas de l’optimisation de la plateforme, mais elles sont instanciées dès que le ResourceDictionary est créé. Cela se produit, car x:Name indique à la plateforme que votre application doit accéder à cette ressource. La plateforme doit donc créer un élément pour lequel une référence doit être créée.

**ResourceDictionaries dans un UserControl**. Les ResourceDictionaries définis dans un UserControl entraînent une pénalité. La plateforme crée une copie d’un tel ResourceDictionary pour chaque instance du UserControl. Si le UserControl est énormément utilisé, déplacez le ResourceDictionary en dehors du UserControl et placez-le au niveau de la page.

## Utiliser XBF2

XBF2 est une représentation binaire du balisage XAML, qui permet d’éviter les coûts liés à l’analyse du texte lors de l’exécution. Elle optimise également votre binaire pour la création de charge et d’arborescence, et permet des types XAML « fast-path » pour améliorer les coûts de création de tas et d’objet, par exemple VSM, ResourceDictionary, Styles, etc. Elle est complètement mappée en mémoire. Il n’existe donc pas d’encombrement du tas pour le chargement et la lecture d’une page XAML. En outre, elle permet de réduire l’encombrement disque des pages XAML stockées dans un appx. XBF2 est une représentation plus compacte, et elle peut réduire l’encombrement disque des fichiers XAML/XBF1 comparatifs jusqu’à 50 %. Par exemple, l’application Photos intégrée présente une réduction d’environ 60 % après la conversion en XBF2, en passant d’1 Mo de ressources XBF1 à environ 400 Ko de ressources XBF2. Nous constatons également que les applications passent de 15 à 20 % dans le processeur, et de 10 à 15 % dans le tas Win32.

Les contrôles et dictionnaires intégrés dans XAML, qui sont fournis par l’infrastructure sont déjà entièrement compatibles XBF2. Pour votre propre application, vérifiez que votre fichier projet déclare TargetPlatformVersion 8.2 ou ultérieur.

Pour vérifier si vous possédez XBF2, ouvrez votre application dans un éditeur binaire ; les 12e et 13e octets correspondent à 00 02 si vous possédez XBF2.




<!--HONumber=Jun16_HO4-->


