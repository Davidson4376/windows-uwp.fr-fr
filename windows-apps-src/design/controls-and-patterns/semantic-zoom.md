---
author: muhsinking
Description: A semantic zoom control allows the user to zoom between two different semantic views of the same data set.
title: Zoom sémantique
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 17391b48b7b75209321f35ffd20610e17e3935c4
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7436343"
---
# <a name="semantic-zoom"></a>Zoom sémantique

 

Le zoom sémantique permet à l’utilisateur de basculer entre deux affichages du même contenu pour pouvoir rapidement parcourir un jeu de données volumineux.
 
- La vue avec zoom avant est la vue principale du contenu. Il s’agit de la vue principale dans laquelle vous affichez des éléments de données individuels. 
- La vue avec zoom arrière est une vue plus générale du même contenu. En règle générale, vous affichez les en-têtes de groupe d’un jeu de données regroupées dans cette vue. 

Par exemple, s’il consulte un carnet d’adresses, l’utilisateur peut effectuer un zoom arrière pour accéder rapidement à la lettre «W» et effectuer un zoom avant sur cette lettre pour afficher les noms associés. 

> **API importantes**: [classe SemanticZoom](https://msdn.microsoft.com/library/windows/apps/hh702601), [classe ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx)

**Fonctionnalités**:

-   La taille de la vue avec zoom arrière est restreinte par les limites du contrôle de zoom sémantique.
-   Appuyer sur un en-tête de groupe permet de basculer entre les vues. Il est possible d’activer le pincement pour basculer entre les vues.
-   Les en-têtes actifs basculent entre les vues.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un contrôle **SemanticZoom** quand vous avez besoin d’afficher un jeu de données regroupées trop grand pour être affiché sur une ou deux pages.

Ne confondez pas le zoom sémantique avec le zoom optique. Ces deux types de zoom interagissent et se comportent fondamentalement de la même façon (en affichant des données plus ou moins détaillées selon le facteur de zoom choisi), mais le zoom optique sert à ajuster le grossissement d’une zone de contenu ou d’un objet, tel qu’une photographie. Pour plus d’informations sur le contrôle qui effectue un zoom optique, voir le contrôle [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/SemanticZoom">ouvrir l’application et voir l'objet SemanticZoom en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Application Photos**

Zoom sémantique utilisé dans l’application Photos. Les photos sont regroupées par mois. La sélection d’un en-tête de mois dans l’affichage Grille par défaut effectue un zoom arrière sur l’affichage Liste du mois pour une navigation plus rapide.

![Zoom sémantique utilisé dans l’application Photos](images/control-examples/semantic-zoom-photos.png)

**Carnet d’adresses**

Un carnet d’adresses est un autre exemple de jeu de données dans lequel la navigation peut être considérablement simplifiée à l’aide du zoom sémantique. Vous pouvez utiliser la vue avec zoom arrière pour accéder rapidement à la lettre dont vous avez besoin (image de gauche), tandis que la vue avec zoom avant affiche les éléments de données individuels (image de droite).

![Exemple de zoom sémantique utilisé dans une liste de contacts](images/semanticzoom-win10.png)

## <a name="create-a-semantic-zoom"></a>Créer un zoom sémantique

Le contrôle **SemanticZoom** n’a pas de représentation visuelle propre. Il s’agit d’un contrôle hôte qui gère la transition entre 2autres contrôles qui fournissent les vues de votre contenu, généralement les contrôles **ListView** ou **GridView**.  Vous définissez les contrôles d’affichage sur les propriétés [ZoomedInView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedinview.aspx) et [ZoomedOutView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.zoomedoutview.aspx) de SemanticZoom.

Les 3éléments dont vous avez besoin pour un zoom sémantique sont les suivants:
- Une source de données regroupées
- Une vue avec zoom avant qui affiche les données au niveau de l’élément.
- Une vue avec zoom arrière qui affiche les données au niveau du groupe.

Avant d’utiliser un zoom sémantique, vous devez comprendre comment utiliser un affichage Liste avec des données regroupées. Pour plus d’informations, voir [Affichage Liste et affichage Grille](listview-and-gridview.md) et [Regroupement d’éléments dans une liste](). 

> **Remarque**&nbsp;&nbsp;Pour définir la vue avec zoom avant et la vue avec zoom arrière du contrôle SemanticZoom, vous pouvez utiliser l’un ou l’autre des deux contrôles qui implémentent l’interface [ISemanticZoomInformation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.isemanticzoominformation.aspx). L’infrastructure XAML fournit 3contrôles qui implémentent cette interface: ListView, GridView et Hub.
 
 Ce code XAML montre la structure du contrôle SemanticZoom. Vous attribuez d’autres contrôles aux propriétés ZoomedInView et ZoomedOutView.
 
 ```xaml
<SemanticZoom>
    <SemanticZoom.ZoomedInView>
        <!-- Put the GridView for the zoomed in view here. -->   
    </SemanticZoom.ZoomedInView>

    <SemanticZoom.ZoomedOutView>
        <!-- Put the ListView for the zoomed out view here. -->       
    </SemanticZoom.ZoomedOutView>
</SemanticZoom>
 ```
 
Ces exemples sont tirés de la page SemanticZoom de l’[exemple d’éléments de base d’une interface utilisateur XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992). Vous pouvez télécharger l’exemple pour voir le code complet, y compris la source de données. Le zoom sémantique utilise un contrôle GridView pour fournir la vue avec zoom avant et un contrôle ListView pour la vue avec zoom arrière.
  
**Définir la vue avec zoom avant**

Voici le contrôle GridView pour la vue avec zoom avant. La vue avec zoom avant doit afficher les éléments de données individuels dans des groupes. L’exemple suivant indique comment afficher les éléments dans une grille avec une image et du texte. 

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

```xaml
<DataTemplate x:Key="ZoomedInGroupHeaderTemplate" x:DataType="data:ControlInfoDataGroup">
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

```xaml
<SemanticZoom.ZoomedOutView>
    <ListView ItemsSource="{x:Bind cvsGroups.View.CollectionGroups}" 
              SelectionMode="None" 
              ItemTemplate="{StaticResource ZoomedOutTemplate}" />
</SemanticZoom.ZoomedOutView>
```

 L’apparence est définie dans la ressource `ZoomedOutTemplate`.
 
```xaml    
<DataTemplate x:Key="ZoomedOutTemplate" x:DataType="wuxdata:ICollectionViewGroup">
    <TextBlock Text="{x:Bind Group.(data:ControlInfoDataGroup.Title)}" 
               Style="{StaticResource SubtitleTextBlockStyle}" TextWrapping="Wrap"/>
</DataTemplate>
```

**Synchroniser les vues**

Les vues avec zoom avant et zoom arrière doivent être synchronisées de sorte que si un utilisateur sélectionne un groupe dans la vue avec zoom arrière, les détails de ce même groupe apparaissent dans la vue avec zoom avant. Vous pouvez utiliser un [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) ou ajouter du code pour synchroniser les vues.

Tous les contrôles que vous liez au même CollectionViewSource ont toujours le même élément actif. Si les deux vues utilisent le même CollectionViewSource comme source de données, le CollectionViewSource synchronise automatiquement les vues. Pour plus d’informations, voir [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

Si vous n’utilisez pas de CollectionViewSource pour synchroniser les vues, vous devez gérer l’événement [ViewChangeStarted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.viewchangestarted.aspx) et synchroniser les éléments dans le gestionnaire d’événements de la manière suivante.

```xaml
<SemanticZoom x:Name="semanticZoom" ViewChangeStarted="SemanticZoom_ViewChangeStarted">
```

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

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.


## <a name="related-articles"></a>Articles associés

- [Informations de base relatives à la conception de la navigation](../basics/navigation-basics.md)
- [Affichage Liste et affichage Grille](listview-and-gridview.md)
- [Modèles et conteneurs d’éléments](item-containers-templates.md)





