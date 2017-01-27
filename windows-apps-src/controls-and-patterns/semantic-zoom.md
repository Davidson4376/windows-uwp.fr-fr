---
author: Jwmsft
Description: "Un contrôle de zoom sémantique permet à l’utilisateur d’effectuer un zoom entre deux affichages sémantiques différents du même jeu de données."
title: "Zoom sémantique"
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: b3ca48b678fb4e1ddb4b26ad7add723474527591

---
# <a name="semantic-zoom"></a>Zoom sémantique

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Le zoom sémantique permet à l’utilisateur de basculer entre deux affichages du même contenu pour pouvoir rapidement parcourir un jeu de données volumineux.
 
- La vue avec zoom avant est la vue principale du contenu. Il s’agit de la vue principale dans laquelle vous affichez des éléments de données individuels. 
- La vue avec zoom arrière est une vue plus générale du même contenu. En règle générale, vous affichez les en-têtes de groupe d’un jeu de données regroupées dans cette vue. 

Par exemple, s’il consulte un carnet d’adresses, l’utilisateur peut effectuer un zoom arrière pour accéder rapidement à la lettre « W » et effectuer un zoom avant sur cette lettre pour afficher les noms associés. 

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Classe SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)</li>
<li>[**Classe ListView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx)</li>
<li>[**Classe GridView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx)</li>
</ul>
</div>

**Fonctionnalités** :

-   La taille de la vue avec zoom arrière est restreinte par les limites du contrôle de zoom sémantique.
-   Appuyer sur un en-tête de groupe permet de basculer entre les vues. Il est possible d’activer le pincement pour basculer entre les vues.
-   Les en-têtes actifs basculent entre les vues.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un contrôle **SemanticZoom** quand vous avez besoin d’afficher un jeu de données regroupées trop grand pour être affiché sur une ou deux pages.

Ne confondez pas le zoom sémantique avec le zoom optique. Ces deux types de zoom interagissent et se comportent fondamentalement de la même façon (en affichant des données plus ou moins détaillées selon le facteur de zoom choisi), mais le zoom optique sert à ajuster le grossissement d’une zone de contenu ou d’un objet, tel qu’une photographie. Pour plus d’informations sur le contrôle qui effectue un zoom optique, voir le contrôle [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx).

## <a name="examples"></a>Exemples

**Application Photos**

Zoom sémantique utilisé dans l’application Photos. Les photos sont regroupées par mois. La sélection d’un en-tête de mois dans l’affichage Grille par défaut effectue un zoom arrière sur l’affichage Liste du mois pour une navigation plus rapide.

![Zoom sémantique utilisé dans l’application Photos](images/control-examples/semantic-zoom-photos.png)

**Carnet d’adresses**

Un carnet d’adresses est un autre exemple de jeu de données dans lequel la navigation peut être considérablement simplifiée à l’aide du zoom sémantique. Vous pouvez utiliser la vue avec zoom arrière pour accéder rapidement à la lettre dont vous avez besoin (image de gauche), tandis que la vue avec zoom avant affiche les éléments de données individuels (image de droite).

![Exemple de zoom sémantique utilisé dans une liste de contacts](images/semanticzoom-win10.png)

## <a name="create-a-semantic-zoom"></a>Créer un zoom sémantique

Le contrôle **SemanticZoom** n’a pas de représentation visuelle propre. Il s’agit d’un contrôle hôte qui gère la transition entre 2 autres contrôles qui fournissent les vues de votre contenu, généralement les contrôles **ListView** ou **GridView**.  Vous définissez les contrôles d’affichage sur les propriétés [**ZoomedInView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedinview.aspx) et [**ZoomedOutView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedoutview.aspx) de SemanticZoom.

Les 3 éléments dont vous avez besoin pour un zoom sémantique sont les suivants :
- Une source de données regroupées
- Une vue avec zoom avant qui affiche les données au niveau de l’élément.
- Une vue avec zoom arrière qui affiche les données au niveau du groupe.

Avant d’utiliser un zoom sémantique, vous devez comprendre comment utiliser un affichage Liste avec des données regroupées. Pour plus d’informations, voir [Affichage Liste et affichage Grille](listview-and-gridview.md) et [Regroupement d’éléments dans une liste](). 

> **Remarque**&nbsp;&nbsp;Pour définir la vue avec zoom avant et la vue avec zoom arrière du contrôle SemanticZoom, vous pouvez utiliser l’un ou l’autre des deux contrôles qui implémentent l’interface [**ISemanticZoomInformation**](). L’infrastructure XAML fournit 3 contrôles qui implémentent cette interface : ListView, GridView et Hub.
 
 Ce code XAML montre la structure du contrôle SemanticZoom. Vous attribuez d’autres contrôles aux propriétés ZoomedInView et ZoomedOutView.
 
 **XAML**
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed out view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed in view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
Ces exemples sont tirés de la page SemanticZoom de l’[Exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992). Vous pouvez télécharger l’exemple pour voir le code complet, y compris la source de données. Le zoom sémantique utilise un contrôle GridView pour fournir la vue avec zoom avant et un contrôle ListView pour la vue avec zoom arrière.
  
**Définir la vue avec zoom avant**

Voici le contrôle GridView pour la vue avec zoom avant. La vue avec zoom avant doit afficher les éléments de données individuels dans des groupes. L’exemple suivant indique comment afficher les éléments dans une grille avec une image et du texte. 

**XAML**
```xaml
<SemanticZoom.ZoomedInView>
    <GridView ItemsSource="{x:Bind cvsGroups.View}" 
              ScrollViewer.IsHorizontalScrollChainingEnabled="False" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedInTemplate}">
        <GridView.GroupStyle>
            <GroupStyle HeaderTemplate="{StaticResource ZoomedInGroupHeaderTemplate}"/>
        </GridView.GroupStyle>
    </GridView>
</SemanticZoom.ZoomedInView>
```
 
L’apparence des en-têtes de groupe est définie dans la ressource `ZoomedInGroupHeaderTemplate`. L’apparence des éléments est définie dans la ressource `ZoomedInTemplate`. 

**XAML**   
```xaml
<DataTemplate x:Key="" x:DataType="data:ControlInfoDataGroup">
    <TextBlock Text="{x:Bind Title}" 
               Foreground="{ThemeResource ApplicationForegroundThemeBrush}" 
               Style="{StaticResource SubtitleTextBlockStyle}"/>
</DataTemplate>

<DataTemplate x:Key="ZoomedInTemplate" x:DataType="data:ControlInfoDataItem">
    <StackPanel Orientation="Horizontal" MinWidth="200" Margin="12,6,0,6">
        <Image Source="{x:Bind ImagePath}" Height="80" Width="80"/>
        <StackPanel Margin="20,0,0,0">
            <TextBlock Text="{x:Bind Title}" 
                       Style="{StaticResource BaseTextBlockStyle}"/>
            <TextBlock Text="{x:Bind Subtitle}" 
                       TextWrapping="Wrap" HorizontalAlignment="Left" 
                       Width="300" Style="{StaticResource BodyTextBlockStyle}"/>
        </StackPanel>
    </StackPanel>
</DataTemplate>
```

**Définir la vue avec zoom arrière**

Ce code XAML définit un contrôle ListView pour la vue avec zoom arrière. Cet exemple indique comment afficher les en-têtes de groupe sous forme de texte dans une liste.

**XAML**
```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 L’apparence est définie dans la ressource `ZoomedOutTemplate`.
 
 **XAML**   
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**Synchroniser les vues**

Les vues avec zoom avant et zoom arrière doivent être synchronisées de sorte que si un utilisateur sélectionne un groupe dans la vue avec zoom arrière, les détails de ce même groupe apparaissent dans la vue avec zoom avant. Vous pouvez utiliser un [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) ou ajouter du code pour synchroniser les vues.

Tous les contrôles que vous liez au même CollectionViewSource ont toujours le même élément actif. Si les deux vues utilisent le même CollectionViewSource comme source de données, le CollectionViewSource synchronise automatiquement les vues. Pour plus d’informations, voir [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

Si vous n’utilisez pas de CollectionViewSource pour synchroniser les vues, vous devez gérer l’événement [**ViewChangeStarted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.viewchangestarted.aspx) et synchroniser les éléments dans le gestionnaire d’événements de la manière suivante.

**XAML**
```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

**C#**
```csharp
private void SemanticZoom_ViewChangeStarted(object sender, SemanticZoomViewChangedEventArgs e)
{
    if (e.IsSourceZoomedInView == false)
    {
        e.DestinationItem.Item = e.SourceItem.Item;
    }
}
```

## <a name="recommendations"></a>Recommandations

-   Lorsque vous utilisez le zoom sémantique dans votre application, veillez à ce que la disposition et le sens du mouvement panoramique de l’élément ne changent pas en fonction du niveau de zoom. Les dispositions et les interactions de mouvement panoramique doivent être cohérentes et prévisibles dans tous les niveaux de zoom.
-   Comme le zoom sémantique permet aux utilisateurs de passer rapidement d’un élément de contenu à un autre, limitez à trois le nombre de pages/d’écrans du mode d’affichage général. Un mouvement panoramique trop important réduit la fonctionnalité du zoom sémantique.
-   Évitez d’utiliser le zoom sémantique pour modifier l’étendue du contenu. Par exemple, il convient de ne pas basculer un album photo vers un affichage des dossiers dans l’Explorateur de fichiers.
-   Utilisez une structure et une sémantique qui sont essentielles à la vue.
-   Utilisez des noms de groupe pour les éléments d’une collection groupée.
-   Utilisez un ordre de tri pour une collection non groupée, mais triée, par exemple l’ordre chronologique de dates ou l’ordre alphabétique d’une liste de noms.


## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992)


## <a name="related-articles"></a>Articles connexes

- [Informations de base relatives à la conception de la navigation](../layout/navigation-basics.md)
- [Affichage Liste et affichage Grille](listview-and-gridview.md)
- [Modèles d’élément d’affichage Liste](listview-item-templates.md)








<!--HONumber=Dec16_HO2-->


