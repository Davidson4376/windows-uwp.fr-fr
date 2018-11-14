---
author: muhsinking
Description: Use ListView and GridView controls to display and manipulate sets of data, such as a gallery of images or a set of email messages.
title: Affichage Liste et affichage Grille
label: List view and grid view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/20/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1ee00a9af23be945ad27ab4b39eec127ec397894
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6261524"
---
# <a name="list-view-and-grid-view"></a>Affichage Liste et affichage Grille

La plupart des applications manipulent et affichent des jeux de données, par exemple, une galerie d’image ou un ensemble d’e-mails. L’infrastructure IU XAML fournit les contrôles ListView et GridView qui facilitent l’affichage et la manipulation des données dans votre application.  

> **API importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), [propriété ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx), [propriété Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)

Les contrôles ListView et GridView proviennent de la classe ListViewBase; ils possèdent donc les mêmes fonctionnalités mais affichent les données différemment. Dans cet article, lorsque nous évoquons ListView, sauf indication contraire, les informations s’appliquent aux contrôles ListView et GridView. Nous pouvons faire référence aux classes telles que ListView ou ListViewItem, mais le préfixe «List» peut être remplacé par «Grid» pour l’équivalent Grid correspondant (GridView ou GridViewItem). 

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Le contrôle ListView affiche les données en les empilant dans une seule colonne. Il est souvent utilisé pour afficher une liste ordonnée d’éléments, telle qu’une liste d’e-mails ou de résultats de recherche. 

![Un affichage Liste avec des données regroupées](images/simple-list-view-phone.png)

GridView présente une collection d’éléments en lignes et en colonnes qui peut défiler sur le plan vertical. Les données sont empilées horizontalement jusqu’à ce qu’elles remplissent les colonnes, puis se poursuivent avec la ligne suivante. Il est souvent utilisé pour mettre en valeur chaque élément sur davantage d’espace, comme dans le cas d’une galerie de photos. 

![Exemple de bibliothèque de contenu](images/controls_list_contentlibrary.png)

Pour une comparaison et des recommandations plus détaillées sur le contrôle à utiliser, consultez [Lists](lists.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l'objet <a href="xamlcontrolsgallery:/item/ListView">ListView</a> ou <a href="xamlcontrolsgallery:/item/GridView">GridView</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-list-view"></a>Créer un affichage Liste

L’affichage Liste est un élément [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx). Il peut donc contenir une collection d’éléments de n’importe quel type. Des éléments doivent figurer dans sa collection [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) avant de pouvoir afficher quoi que ce soit à l’écran. Pour remplir l’affichage, vous pouvez ajouter des éléments directement à la collection [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) ou définir la propriété [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) sur une source de données. 

**Important**&nbsp;&nbsp;Vous pouvez utiliser Items ou ItemsSource pour remplir la liste, mais vous ne pouvez pas utiliser les deux en même temps. Si vous définissez la propriété ItemsSource et que vous ajoutez un élément en XAML, l’élément ajouté est ignoré. Si vous définissez la propriété ItemsSource et que vous ajoutez un élément à la collection Items dans le code, une exception est levée.

> **Remarque**&nbsp;&nbsp;Par souci de simplicité, de nombreux exemples de cet article remplissent directement la collection **Items**. Toutefois, il est plus courant que les éléments d’une liste proviennent d’une source dynamique, par exemple, une liste de livres d’une base de données en ligne. Vous utilisez la propriété **ItemsSource** dans ce but. 

### <a name="add-items-to-the-items-collection"></a>Ajouter des éléments à la collection Items

Vous pouvez ajouter des éléments à la collection [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) en utilisant le langage XAML ou du code. Vous ajoutez généralement des éléments de cette façon si vous avez un petit nombre d’éléments qui ne sont pas modifiés et sont facilement définis en XAML, ou si vous générez les éléments dans le code lors de l’exécution. 

Voici un affichage Liste avec des éléments définis inline en XAML. Lorsque vous définissez les éléments en XAML, ceux-ci sont automatiquement ajoutés à la collection Items.

**XAML**
```xaml
<ListView x:Name="listView1"> 
   <x:String>Item 1</x:String> 
   <x:String>Item 2</x:String> 
   <x:String>Item 3</x:String> 
   <x:String>Item 4</x:String> 
   <x:String>Item 5</x:String> 
</ListView>  
```

Voici l’affichage Liste créé dans le code. La liste résultante est identique à celle créée précédemment dans le code XAML.

**C#**
```csharp
// Create a new ListView and add content. 
ListView listView1 = new ListView(); 
listView1.Items.Add("Item 1"); 
listView1.Items.Add("Item 2"); 
listView1.Items.Add("Item 3"); 
listView1.Items.Add("Item 4"); 
listView1.Items.Add("Item 5");
 
// Add the ListView to a parent container in the visual tree. 
stackPanel1.Children.Add(listView1); 
```

ListView se présente comme suit.

![Un affichage Liste simple](images/listview-simple.png)

### <a name="set-the-items-source"></a>Définir la source des éléments

On utilise en général un affichage Liste pour afficher des données d’une source telle qu’une base de données ou Internet. Pour renseigner un affichage Liste à partir d’une source de données, vous affectez à sa propriété [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) une collection d’éléments de données.

Ici, la propriété ItemsSource de l’affichage Liste prend la valeur de l’instance d’une collection directement dans le code.

**C#**
```csharp 
// Instead of hard coded items, the data could be pulled 
// asynchronously from a database or the internet.
ObservableCollection<string> listItems = new ObservableCollection<string>();
listItems.Add("Item 1");
listItems.Add("Item 2");
listItems.Add("Item 3");
listItems.Add("Item 4");
listItems.Add("Item 5");

// Create a new list view, add content, 
ListView itemListView = new ListView();
itemListView.ItemsSource = listItems;

// Add the list view to a parent container in the visual tree.
stackPanel1.Children.Add(itemListView);
```

Vous pouvez également lier la propriété ItemsSource à une collection en XAML. Pour plus d’informations sur la liaison de données, voir [Vue d’ensemble de la liaison de données](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).

Ici, ItemsSource est lié à une propriété publique nommée `Items` qui expose la collection de données privées de la page.

**XAML**
```xaml
<ListView x:Name="itemListView" ItemsSource="{x:Bind Items}"/>
```

**C#**
```csharp
private ObservableCollection<string> _items = new ObservableCollection<string>();

public ObservableCollection<string> Items
{
    get { return this._items; }
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Items.Add("Item 1");
    Items.Add("Item 2");
    Items.Add("Item 3");
    Items.Add("Item 4");
    Items.Add("Item 5");
}
```

Si vous avez besoin d’afficher des données groupées dans votre affichage Liste, vous devez lier un élément [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx). CollectionViewSource agit en tant que proxy pour la classe de collection dans XAML et active la prise en charge du regroupement. Pour plus d’informations, consultez [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

## <a name="data-template"></a>Modèle de données

Un modèle de données d’un élément définit la manière dont les données sont visualisées. Par défaut, un élément de données est affiché dans l’affichage Liste en tant que représentation de chaîne de l’objet de données auquel il est lié. Vous pouvez afficher la représentation de chaîne d’une propriété spécifique de l’élément de données en définissant la propriété [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) sur cette propriété.

Toutefois, en général, on souhaite afficher une représentation enrichie des données. Pour définir précisément la façon dont les éléments sont affichés dans l’affichage Liste, vous devez créer un objet [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx). Le code XAML dans l’objet DataTemplate définit la disposition et l’apparence des contrôles qui permettent d’afficher un élément spécifique. Les contrôles dans la disposition peuvent être liés aux propriétés d’un objet de données ou leur contenu statique peut être défini inline. L’objet DataTemplate est affecté à la propriété [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) de la liste de contrôle.

Dans cet exemple, l’élément de données est une chaîne simple. Vous utilisez un DataTemplate pour ajouter une image à gauche de la chaîne et afficher la chaîne en bleu canard.

> **Remarque**&nbsp;&nbsp;Quand vous utilisez l’[extension de balisage x:Bind](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) dans un DataTemplate, vous devez spécifier le DataType (`x:DataType`) sur le DataTemplate.

**XAML**
```XAML
<ListView x:Name="listView1">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="47"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="32" Height="32" 
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Teal" 
                           FontSize="15" Grid.Column="1"/>
            </Grid> 
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

Voici ce à quoi ressembleront les éléments de données affichés avec ce modèle de données.

![Éléments de l’affichage Liste avec un modèle de données](images/listview-itemstemplate.png)

Les modèles de données sont le principal moyen de définir l’aspect de votre affichage Liste. Ils peuvent également avoir un impact significatif sur les performances si votre liste affiche un grand nombre d’éléments. Dans cet article, nous utilisons des données de chaîne simple pour la plupart des exemples et nous ne spécifions pas de modèle de données. Pour plus d’informations et pour obtenir des exemples d’utilisation de modèles de données et de conteneurs d’éléments afin de définir l’apparence des éléments dans votre liste ou grille, voir [Modèles et conteneurs d'éléments](item-containers-templates.md). 

## <a name="change-the-layout-of-items"></a>Modifier la disposition des éléments

Lorsque vous ajoutez des éléments à un affichage Liste ou Grille, le contrôle encapsule automatiquement chaque élément dans un conteneur d’éléments, puis dispose tous les conteneurs d’éléments. La manière dont ces conteneurs d’éléments sont disposés dépend de l’élément [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) du contrôle.  
- Par défaut, **ListView** utilise un élément [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx), ce qui donne une liste verticale, comme ceci.

![Un affichage Liste simple](images/listview-simple.png)

- **GridView** utilise un élément [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx), ce qui ajoute les éléments horizontalement, enveloppe et défile verticalement, comme ceci.

![Un affichage Grille simple](images/gridview-simple.png)

Vous pouvez modifier la disposition des éléments en ajustant les propriétés sur le panneau d’éléments, ou remplacer le panneau par défaut par un autre panneau.

> Remarque&nbsp;&nbsp;Veillez à ne pas désactiver la virtualisation si vous modifiez ItemsPanel. **ItemsStackPanel** et **ItemsWrapGrid** prennent en charge la virtualisation, leur utilisation est donc sécurisée. Si vous utilisez tout autre panneau, vous pourriez désactiver la virtualisation et ralentir les performances de l’affichage Liste. Pour plus d’informations, voir les articles relatifs à l’affichage Liste sous [Performances](https://msdn.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui). 

Cet exemple montre comment créer une disposition **ListView** avec des conteneurs d’éléments dans une liste horizontale en modifiant la propriété [Orientation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.orientation.aspx) de **ItemsStackPanel**.
Étant donné que l’affichage Liste défile verticalement par défaut, vous devez également ajuster certaines propriétés sur l’élément interne [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) de l’affichage Liste pour le faire défiler horizontalement.
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx) sur **Activé** ou **Auto**
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx) sur **Auto** 
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx) sur **Désactivé** 
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) sur **Masqué** 

> **Remarque**&nbsp;&nbsp;Ces exemples sont affichés avec une largeur non limitée de l’affichage Liste, les barres de défilement horizontales ne sont donc pas visibles. Si vous exécutez ce code, vous pouvez définir `Width="180"` sur ListView pour afficher les barres de défilement.

**XAML**
```xaml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

La liste résultante ressemble à ce qui suit.

![Un affichage Liste horizontal](images/listview-horizontal.png)

 Dans l’exemple suivant, **ListView** dispose les éléments dans une liste d’habillage verticale en utilisant **ItemsWrapGrid** au lieu d’**ItemsStackPanel**. 
 
> **Remarque**&nbsp;&nbsp;La hauteur de l’affichage Liste doit être limitée pour forcer le contrôle à encapsuler les conteneurs.

**XAML**
```xaml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

La liste résultante ressemble à ce qui suit.

![Un affichage Liste avec disposition de grille](images/listview-itemswrapgrid.png)

Si vous affichez des données groupées dans votre affichage Liste, ItemsPanel détermine la manière dont les groupes d’éléments (et non les éléments individuels) sont disposés. Par exemple, si l’élément ItemsStackPanel horizontal présenté précédemment est utilisé pour afficher les données groupées, les groupes sont organisés horizontalement, mais les éléments dans chaque groupe sont toujours empilés verticalement, comme illustré ici.

![Un affichage Liste horizontal groupé](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>Sélection d’éléments et interaction

Vous pouvez choisir différentes façons de permettre à un utilisateur d’interagir avec un affichage Liste. Par défaut, un utilisateur peut sélectionner un seul élément. Vous pouvez modifier la propriété [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) afin d’autoriser la sélection multiple ou de désactiver la sélection. Vous pouvez définir la propriété [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) pour qu’un utilisateur clique sur un élément pour appeler une action (par exemple, un bouton) au lieu de sélectionner l’élément.

> **Remarque**&nbsp;&nbsp;ListView et GridView utilisent l’énumération [ListViewSelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewselectionmode.aspx) pour leur propriété SelectionMode. IsItemClickEnabled est défini sur **False** par défaut, vous devez le définir uniquement pour activer le mode clic.

Le tableau suivant montre les moyens dont un utilisateur dispose pour interagir avec un affichage Liste, et comment vous pouvez répondre à l’interaction.

Pour activer cette interaction: | Utilisez ces paramètres: | Gérez cet événement: | Utilisez cette propriété pour obtenir l’élément sélectionné:
----------------------------|---------------------|--------------------|--------------------------------------------
Aucune interaction | [SelectionMode ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) = **Aucun**, [IsItemClickEnabledFalse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) = **False** | Non applicable | Non applicable 
Sélection unique | SelectionMode = **Simple**, IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx), [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx)  
Sélection multiple | SelectionMode = **Multiple**, IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
Sélection étendue | SelectionMode = **Étendu**, IsItemClickEnabled = **False** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
Clic | SelectionMode = **Aucun**, IsItemClickEnabled = **True** | [ItemClick](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.itemclick.aspx) | Non applicable 

> **Remarque**&nbsp;&nbsp;À compter de Windows10, vous pouvez activer IsItemClickEnabled pour déclencher un événement ItemClick pendant que SelectionMode est également défini sur Simple, Multiple ou Étendu. De cette façon, l’événement ItemClick est déclenché en premier suivi de l’événement SelectionChanged. Dans certains cas, par exemple, si vous accédez à une autre page dans le gestionnaire d’événements ItemClick, l’événement SelectionChanged n’est pas déclenché et l’élément n’est pas sélectionné.

Vous pouvez définir ces propriétés dans XAML ou dans le code, comme illustré ici.

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>Lecture seule

Vous pouvez définir la propriété SelectionMode sur **ListViewSelectionMode.None** pour désactiver la sélection d’éléments. Cela place le contrôle en mode lecture seule: vous pouvez l’utiliser pour afficher des données, mais pas pour interagir avec celles-ci. Le contrôle lui-même n’est pas désactivé, seule la sélection d’éléments est désactivée.

### <a name="single-selection"></a>Sélection unique

Le tableau suivant décrit le clavier, la souris et les interactions tactiles, lorsque SelectionMode est défini sur **Simple**.

Touche de modification | Interaction
-------------|------------
Aucun | <li>Un utilisateur peut sélectionner un seul élément à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile.</li>
Ctrl | <li>Un utilisateur peut désélectionner un seul élément à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile.</li><li>À l’aide des touches de direction, un utilisateur peut déplacer le focus indépendamment de la sélection.</li>

Lorsque SelectionMode est défini sur **Simple**, vous pouvez obtenir l’élément de données sélectionné à partir de la propriété [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx). Vous pouvez obtenir l’index dans la collection de l’élément sélectionné en utilisant la propriété [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx). Si aucun élément n’est sélectionné, SelectedItem est défini sur **null**, et SelectedIndex est -1. 
 
Si vous essayez de définir un élément qui n’est pas dans la collection **Items** en tant que **SelectedItem**, l’opération est ignorée et SelectedItem est **null**. Toutefois, si vous tentez de définir **SelectedIndex** sur un index en dehors de la plage **Items** dans la liste, une exception **System.ArgumentException** se produit. 

### <a name="multiple-selection"></a>Sélection multiple

Le tableau suivant décrit le clavier, la souris et les interactions tactiles, lorsque SelectionMode est défini sur **Multiple**.

Touche de modification | Interaction
-------------|------------
Aucun | <li>Un utilisateur peut sélectionner plusieurs éléments à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile pour activer/désactiver la sélection sur l’élément sélectionné.</li><li>À l’aide des touches de direction, un utilisateur peut déplacer le focus indépendamment de la sélection.</li>
Maj | <li>Un utilisateur peut sélectionner plusieurs éléments contigus en cliquant ou en appuyant sur le premier élément de la sélection, puis sur le dernier.</li><li>À l’aide des touches de direction, un utilisateur peut créer une sélection contiguë à partir de l’élément sélectionné lorsque la touche MAJ est enfoncée.</li>

### <a name="extended-selection"></a>Sélection étendue

Le tableau suivant décrit le clavier, la souris et les interactions tactiles, lorsque SelectionMode est défini sur **Étendu**.

Touche de modification | Interaction
-------------|------------
Aucun | <li>Le comportement est identique à celui de la sélection **Simple**.</li>
Ctrl | <li>Un utilisateur peut sélectionner plusieurs éléments à l’aide de la barre d’espace, du clic de souris ou de l’appui tactile pour activer/désactiver la sélection sur l’élément sélectionné.</li><li>À l’aide des touches de direction, un utilisateur peut déplacer le focus indépendamment de la sélection.</li>
Maj | <li>Un utilisateur peut sélectionner plusieurs éléments contigus en cliquant ou en appuyant sur le premier élément de la sélection, puis sur le dernier.</li><li>À l’aide des touches de direction, un utilisateur peut créer une sélection contiguë à partir de l’élément sélectionné lorsque la touche MAJ est enfoncée.</li>

Lorsque SelectionMode est défini sur **Multiple** ou **Étendu**, vous pouvez obtenir les éléments de données sélectionnés à partir de la propriété [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx). 

Les propriétés **SelectedIndex**, **SelectedItem** et **SelectedItems** sont synchronisées. Par exemple, si vous définissez SelectedIndex sur -1, SelectedItem est défini sur **null** et SelectedItems est vide; si vous définissez SelectedItem sur **null**, SelectedIndex est défini sur -1 et SelectedItems est vide.

En mode de sélection multiple, **SelectedItem** contient l’élément qui a été sélectionné en premier, et **Selectedindex** contient l’index de l’élément qui a été sélectionné en premier. 

### <a name="respond-to-selection-changes"></a>Répondre aux modifications de sélection

En réaction aux modifications de sélection dans un affichage Liste, gérez l’événement [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx). Dans le code du gestionnaire d’événements, vous pouvez obtenir la liste des éléments sélectionnés auprès de la propriété [SelectionChangedEventArgs.AddedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.addeditems.aspx). Vous pouvez obtenir tous les éléments qui ont été désélectionnés à partir de la propriété [SelectionChangedEventArgs.RemovedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.removeditems.aspx). Les collections AddedItems et RemovedItems contiennent au maximum 1élément, sauf si l’utilisateur sélectionne une plage d’éléments en maintenant la touche MAJ enfoncée.

Cet exemple montre comment gérer l’événement **SelectionChanged** et accéder à des collections d’éléments différents.

**XAML**
```xaml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>Mode clic

Vous pouvez modifier un affichage Liste afin qu’un utilisateur clique sur des éléments tels que des boutons au lieu de les sélectionner. Cela est par exemple utile lorsque l’utilisateur de votre application accède à une nouvelle page en cliquant sur un élément dans une liste ou dans une grille. Pour activer ce comportement:
- Définissez **SelectionMode** sur **Aucun**.
- Définissez **IsItemClickEnabled** sur **true**.
- Gérez l’événement **ItemClick** pour effectuer une action lorsque l’utilisateur clique sur un élément.

Voici un affichage Liste constitué d’éléments pouvant être activés par un clic. Le code dans le gestionnaire d’événement ItemClick accède à une nouvelle page.

**XAML**
```xaml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>Sélectionner une plage d’éléments par programme

Parfois, vous avez besoin de manipuler la sélection d’éléments d’un affichage Liste par programme. Par exemple, vous pourriez avoir un bouton **Sélectionner tout** pour permettre à un utilisateur de sélectionner tous les éléments dans une liste. Dans ce cas, il n’est généralement pas très efficace d’ajouter et de supprimer des éléments de la collection SelectedItems un par un. Chaque modification d’élément déclenche un événement SelectionChanged. Lorsque vous travaillez avec des éléments directement au lieu de fonctionner avec des valeurs d’index, les éléments sont dévirtualisés.

Les méthodes [SelectAll](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectall.aspx), [SelectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectrange.aspx), et [DeselectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.deselectrange.aspx) fournissent un moyen plus efficace de modifier la sélection que l’utilisation de la propriété SelectedItems. Ces méthodes sélectionnent ou désélectionnent à l’aide de plages d’index d’élément. Les éléments qui sont virtualisés le restent, car seul l’index est utilisé. Tous les éléments dans la plage spécifiée sont sélectionnés (ou désélectionnés), quel que soit leur état de sélection d’origine. L’événement SelectionChanged ne se produit qu’une seule fois pour chaque appel à ces méthodes.

> **Important**&nbsp;&nbsp;Vous devez appeler ces méthodes uniquement quand la propriété SelectionMode est définie sur Multiple ou Étendu. Si vous appelez SelectRange quand SelectionMode est défini sur Simple, ou Aucun, une exception est levée.

Lorsque vous sélectionnez des éléments utilisant des plages d’index, utilisez la propriété [SelectedRanges](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectedranges.aspx) pour obtenir toutes les plages sélectionnées dans la liste.

Si ItemsSource implémente [IItemsRangeInfo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.iitemsrangeinfo.aspx), et que vous utilisez ces méthodes pour modifier la sélection, les propriétés **AddedItems** et **RemovedItems** ne sont pas définies dans SelectionChangedEventArgs. La définition de ces propriétés nécessite la dévirtualisation de l’objet d’élément. Utilisez la propriété **SelectedRanges** pour obtenir les éléments à la place.

Vous pouvez sélectionner tous les éléments dans une collection en appelant la méthode SelectAll. Toutefois, il n’existe aucune méthode correspondante pour désélectionner tous les éléments. Vous pouvez désélectionner tous les éléments en appelant DeselectRange et en transmettant [ItemIndexRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.aspx) avec une valeur [FirstIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.firstindex.aspx) de 0 et une valeur [Longueur](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.length.aspx) égale au nombre d’éléments dans la collection. 

**XAML**
```xaml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

Pour plus d’informations sur la modification de l’aspect des éléments sélectionnés, voir [Modèles et conteneurs d'éléments](item-containers-templates.md).

### <a name="drag-and-drop"></a>Glisser-déplacer

Les contrôles ListView et GridView prennent en charge le glisser-déplacer des éléments à l’intérieur d’eux-mêmes et entre eux-mêmes et d’autres contrôles ListView et GridView. Pour plus d’informations sur l’implémentation du modèle glisser-déplacer, voir [Glisser-déplacer](https://msdn.microsoft.com/windows/uwp/design/input/drag-and-drop). 

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de ListView et GridView XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) - Illustre les contrôles ListView et GridView.
- [Exemple de fonctionnalité glisser-déposer XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) - Illustre la fonctionnalité glisser-déposer avec le contrôle ListView.
- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Listes](lists.md)
- [Modèles et conteneurs d’éléments](item-containers-templates.md)
- [Glisser-déposer](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
