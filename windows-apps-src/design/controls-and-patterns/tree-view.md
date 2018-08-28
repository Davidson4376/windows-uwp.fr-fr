---
author: Jwmsft
description: Vous pouvez créer une arborescence à liaison ItemsSource à une source de données hiérarchiques, ou vous pouvez créer et gérer les objets TreeViewNode.
title: Arborescence
label: Tree view
template: detail.hbs
ms.author: jimwalk
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.openlocfilehash: 20de58d13c4ace6b71ec952dc88cd59d1ab6114f
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2885253"
---
# <a name="treeview"></a>TreeView

> [!IMPORTANT]
> Cet article décrit les fonctionnalités qui n’ont pas encore été publiées et sont susceptibles d’être considérablement modifiées d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici. Fonctionnalités nécessitent [Windows 10 initiés Preview build et SDK les plus récents](https://insider.windows.com/for-developers/) ou la [Bibliothèque de l’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Le contrôle XAML TreeView active une liste hiérarchique comportant des nœuds de développement et de réduction qui contiennent des éléments imbriqués. Il peut être utilisé pour illustrer une structure de dossiers ou des relations imbriquées dans votre interface utilisateur.

Les API TreeView prennent en charge les fonctionnalités suivantes:

- Imbrication de niveau N
- Sélection d’un ou plusieurs nœuds
- (Preview) Liaison de données à la propriété ItemsSource TreeView et TreeViewItem
- (Preview) TreeViewItem comme la racine du modèle d’élément TreeView
- (Preview) Arbitraires types de contenu dans un TreeViewItem
- (Preview) Glisser -déplacer entre les vues de l’arborescence

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans le cadre de la bibliothèque de l’interface utilisateur Windows, un package NuGet qui contient les nouveaux contrôles et les fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez la [vue d’ensemble de la bibliothèque de l’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de la plateforme** | **API de bibliothèque de l’interface utilisateur Windows** |
| - | - |
| [Classe TreeView](/uwp/api/windows.ui.xaml.controls.treeview), [classe TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode), [propriété TreeView.ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) | [Classe TreeView](/uwp/api/microsoft.ui.xaml.controls.treeview), [classe TreeViewNode](/uwp/api/microsoft.ui.xaml.controls.treeviewnode), [propriété TreeView.ItemsSource](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

- Utilisez un contrôle TreeView lorsque les éléments comportent des éléments de liste imbriqués et s’il est important d’illustrer la relation hiérarchique des éléments par rapport à leurs homologues et leurs nœuds.

- Évitez d’utiliser TreeView si la mise en évidence de la relation imbriquée d’un élément n’est pas une priorité. Pour la plupart des scénarios d’exploration, un affichage sous forme de liste normal est approprié

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application de la <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/TreeView">Ouvrir l’application et de voir le contrôle TreeView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>Interface utilisateur TreeView

L'arborescence utilise une combinaison de retraits et d’icônes pour représenter la relation imbriquée existant entre les nœuds parent/de dossier et les nœuds enfant/autre que des dossiers. Les nœuds réduits utilisent un chevron pointant vers la droite, et les nœuds développés un chevron pointant vers le bas.

![L'icône de chevron dans TreeView](images/treeview-simple.png)

Vous pouvez inclure une icône dans le modèle de données d'élément de l'arborescence pour représenter les nœuds. Si vous le faites, vous devez utiliser une icône de dossier uniquement pour les nœuds qui représentent des dossiers littéraux, tels que la structure de dossiers sur un disque.

![Les icônes Chevron et Dossier dans un TreeView](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>Créer une arborescence

Vous pouvez créer une arborescence par liaison [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) à une source de données hiérarchiques, ou vous pouvez créer et gérer les objets TreeViewNode.

Pour créer une arborescence, vous utilisez un contrôle [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) et une hiérarchie d'objets [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode). Vous créez la hiérarchie de nœud en ajoutant un ou plusieurs nœuds racine à la collection de [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) du contrôle TreeView. Chaque TreeViewNode peut alors disposer de plus de nœuds ajoutés à la collection Children. Vous pouvez imbriquer des nœuds de l’arborescence à la profondeur que vous souhaitez.

À partir de l’aperçu d’initiés Windows, vous pouvez lier une source de données hiérarchique à la propriété [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) à fournir le contenu de la vue arborescence, comme vous le feriez avec ItemsSource de liste. De même, utilisez [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (et facultatif [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)) pour fournir un DataTemplate qui affiche l’élément.

> [!IMPORTANT]
> ItemsSource est un autre mécanisme pour TreeView.RootNodes pour placer le contenu dans le contrôle TreeView. Vous ne pouvez pas définir à la fois ItemsSource et RootNodes en même temps. Lorsque vous utilisez ItemsSource, nœuds créée pour vous, et vous pouvez y accéder à partir de la propriété TreeView.RootNodes.

Voici un exemple d'arborescence simple déclarée en XAML. Vous ajoutez généralement les nœuds dans le code, mais nous affichons ici la hiérarchie XAML, parce qu'il peut être utile de visualiser la façon dont la hiérarchie de nœuds est créée.

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

Dans la plupart des cas, votre arborescence affiche les données d’une source de données, vous donc généralement déclarez la racine du contrôle TreeView dans XAML, mais ajoutez les objets TreeViewNode dans le code ou à l’aide de la liaison de données.

### <a name="bind-to-a-hierarchical-data-source"></a>Lier à une source de données hiérarchiques

Pour créer une arborescence à l’aide de la liaison de données, définissez une collection hiérarchique à la propriété TreeView.ItemsSource. Puis dans la zone ItemTemplate, définissez l’enfant collection items à la propriété TreeViewItem.ItemsSource.

```xaml
<TreeView ItemsSource="{x:Bind DataSource}">
    <TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <TreeViewItem ItemsSource="{x:Bind Children}"
                          Content="{x:Bind Name}"/>
        </DataTemplate>
    </TreeView.ItemTemplate>
</TreeView>
```

Voir _arborescence à l’aide de la liaison de données_ de la section exemples pour le code complet.

#### <a name="items-and-item-containers"></a>Éléments et conteneurs d’élément

Si vous utilisez TreeView.ItemsSource, ces API est disponibles pour obtenir l’élément de données ou le nœud du conteneur et vice versa.

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | Obtient l’élément de données pour le conteneur TreeViewItem spécifié. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | Obtient le conteneur TreeViewItem pour l’élément de données spécifié. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | Obtient le TreeViewNode pour le conteneur TreeViewItem spécifié. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | Obtient le conteneur TreeViewItem pour le TreeViewNode spécifié. |

### <a name="manage-tree-view-nodes"></a>Gérer les nœuds de l’arborescence

Cette arborescence est identique à celle créée précédemment en XAML, mais les nœuds sont créés dans le code à la place.

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New TreeViewNode With {.Content = "Vanilla"})
        .Add(New TreeViewNode With {.Content = "Strawberry"})
        .Add(New TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Ces API sont disponibles pour la gestion de la hiérarchie de données de votre arborescence.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Une arborescence peut avoir un ou plusieurs nœuds racine. Ajoutez un objet TreeViewNode à la collection RootNodes pour créer un nœud racine. Le **Parent** d’un nœud racine est toujours **null**. La **Profondeur** d’un nœud racine est0. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Ajoutez des objets TreeViewNode à la collection Children d’un nœud parent pour créer votre hiérarchie de nœuds. Un nœud est le **Parent** de tous les nœuds de sa collection **Children**. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | **true** si le nœud a réalisé des enfants. **false** indique un dossier ou un élément vide. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Utilisez cette propriété si vous remplissez des nœuds lorsqu'ils sont développés. Voir _Remplir un nœud lorsqu’il est développé_ plus loin dans cet article. |
| [Profondeur](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Indique la distance entre le nœud racine et un nœud enfant. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Obtient le TreeViewNode qui possède la collection **Children** dont ce nœud fait partie. |

L'arborescence utilise les propriétés **HasChildren** et **HasUnrealizedChildren** pour déterminer si l’icône développer/réduire s'affiche. Si l'une de ces propriétés est **true**, l’icône s’affiche; autrement, elle ne s'affiche pas.

## <a name="tree-view-node-content"></a>Contenu d’un nœud d'arborescence

Vous pouvez stocker l’élément de données qu'un nœud d’arborescence représente dans sa propriété [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

Dans les exemples précédents, le contenu a une valeur de chaîne simple. Ici, un nœud d’arborescence représente le dossier Images de l’utilisateur, de sorte que la bibliothèque d’images [StorageFolder](/uwp/api/windows.storage.storagefolder) est attribuée à la propriété Content du nœud.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New TreeViewNode With {.Content = picturesFolder}
```

Vous pouvez fournir un [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) pour spécifier comment l'élément de données s'affiche dans l'arborescence.

> [!NOTE]
> Dans Windows10, version1803, vous devez redéfinir le modèle du contrôle TreeView et spécifier un ItemTemplate personnalisé si votre contenu n’est pas une chaîne. Pour plus d’informations, voir l’exemple complet à la fin de cet article. Dans les versions ultérieures, définissez la propriété [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) .

### <a name="item-container-style"></a>Style d’élément de conteneur

Si vous utilisez ItemsSource ou RootNodes, les éléments réelles utilisées pour afficher chaque nœud – appelé le «conteneur», est un objet [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) . Vous pouvez définir le style du conteneur à l’aide du contrôle TreeView propriétés ItemContainerStyle ou ItemContainerStyleSelector.

### <a name="item-template-selectors"></a>Sélecteurs de modèle d’élément

Vous pouvez choisir définir un DataTemplate différent pour l’arborescence afficher les éléments en fonction du type d’élément. Par exemple, dans une application de l’Explorateur de fichiers, vous pouvez utiliser un modèle de données de dossiers et un autre pour les fichiers.

![Dossiers et fichiers à l’aide de divers modèles de données](images/treeview-icons.png)

Voici un exemple illustrant comment créer et utiliser un sélecteur de modèle d’élément.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <TreeView ItemsSource="{x:Bind DataSource}"
              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

## <a name="interacting-with-a-tree-view"></a>Interaction avec une arborescence

Vous pouvez configurer une arborescence pour permettre à un utilisateur d’interagir avec celle-ci de différentes manières:

- développer ou réduire des nœuds
- éléments de sélection multiple ou unique
- cliquez pour appeler un élément

### <a name="expandcollapse"></a>développer/réduire

N’importe quel nœud d’arborescence ayant des enfants peut toujours être développé ou réduit en cliquant sur le glyphe développer/réduire. Vous pouvez également développer ou réduire un nœud par programme et répondre lorsqu’un nœud change d’état.

#### <a name="expandcollapse-a-node-programmatically"></a>Développer/réduire un nœud par programme

Il existe 2façons de développer ou de réduire un nœud d’arborescence dans votre code.

- La classe [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) possède les méthodes [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) et [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand). Lorsque vous appelez ces méthodes, vous passez dans le TreeViewNode que vous voulez développer ou réduire.

- Chaque [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) a la propriété [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded). Vous pouvez utiliser cette propriété pour vérifier l’état d’un nœud ou le configurer pour modifier l’état. Vous pouvez également définir cette propriété dans le code XAML pour définir l’état initial d’un nœud.

### <a name="fill-a-node-when-its-expanding"></a>Remplir un nœud lorsqu’il se développe

Vous devrez peut-être afficher un grand nombre de nœuds dans votre arborescence, ou vous ne saurez peut-être pas à l'avance combien de nœuds elle contiendra. Le contrôle TreeView n’est pas virtualisé, donc vous pouvez gérer les ressources en remplissant chaque nœud lorsqu'il est développé et en supprimant les nœuds enfants lorsqu'il est réduit.

Gérez l'événement [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) et utilisez la propriété [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) pour ajouter des enfants à un nœud lorsqu’il est développé. La propriété HasUnrealizedChildren indique si le nœud doit être rempli ou si sa collection Children a déjà été remplie. Il est important de garder à l’esprit que le TreeViewNode ne définit pas cette valeur, vous devez le gérer dans le code de votre application.

Voici un exemple d'utilisation de ces API. Voir l'exemple de code complet à la fin de cet article pour connaître le contexte, notamment l'implémentation du «FillTreeNode».

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

Ce n’est pas obligatoire, mais vous pouvez également gérer l'événement [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) et supprimer les nœuds enfants à la fermeture du nœud parent. Cela peut être important si votre arborescence a un grand nombre de nœuds, ou si les données de nœud utilisent beaucoup de ressources. Vous devez prendre en compte l’impact qu'a le remplissage d’un nœud à chaque ouverture sur les performances par rapport au fait de laisser les enfants sur un nœud fermé. La meilleure option dépend de votre application.

Voici un exemple de gestionnaire de l’événement Collapsed.

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Appel d’un élément

Un utilisateur peut appeler une action (en traitant l’élément comme un bouton) au lieu de sélectionner l’élément. Vous gérez l'événement [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) pour répondre à cette interaction utilisateur.

> [!NOTE]
> Contrairement au contrôle ListView, qui a la propriété [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled), appeler un élément est toujours activé sur l’arborescence. Vous pouvez toujours choisir si vous souhaitez gérer l’événement ou non.

**Classe [TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)**

Les arguments d’événement ItemInvoked vous permettent d’accéder à l’élément appelé. La propriété [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) a le nœud qui a été appelé. Vous pouvez le convertir en TreeViewNode et obtenir l’élément de données à partir de la propriété TreeViewNode.Content.

Voici un exemple de gestionnaire d’événements ItemInvoked. L’élément de données est un [IStorageItem](/uwp/api/windows.storage.istorageitem), et cet exemple affiche uniquement des informations sur le fichier et l’arborescence. En outre, si le nœud est un nœud de dossier, il développe ou réduit le nœud en même temps. Autrement, le nœud se développe ou se réduit uniquement lorsque vous cliquez sur le chevron.

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>Sélection d'élément

Le contrôle TreeView prend en charge la sélection unique et la sélection multiple. Par défaut, la sélection de nœuds est désactivée, mais vous pouvez définir la propriété [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) pour permettre la sélection de nœuds. Les valeurs [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) sont **None**, **Single** et **Multiple**.

Lorsque la sélection est activée, une case à cocher s’affiche en regard de chaque nœud d’arborescence, et les éléments sélectionnés sont mis en surbrillance. Un utilisateur peut sélectionner ou désélectionner un élément à l’aide de la case à cocher. Le fait de cliquer sur l’élément permet toujours de l'appeler.

Sélectionner ou désactiver un nœud parent sera sélectionner ou désélectionner tous les enfants de ce nœud. Si certains, mais pas des enfants sous un nœud parent sont sélectionnés, la case à cocher pour le nœud parent est indiqué comme indéterminé (rempli avec une zone de noir).

![Sélection multiple dans un affichage de l’arborescence](images/treeview-selection.png)

Les nœuds sélectionnés sont ajoutés à la collection [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) de l’arborescence. Vous pouvez appeler la méthode [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) pour sélectionner tous les nœuds d'une arborescence.

> [!NOTE]
> Si vous appelez **SelectAll**, tous les nœuds réalisés sont sélectionnés, quel que soit le SelectionMode. Pour fournir une expérience utilisateur cohérente, vous ne devez appeler SelectAll que si SelectionMode est défini sur **Multiple**.

#### <a name="selection-and-realizedunrealized-nodes"></a>Sélection et nœuds réalisés/non réalisés

Si votre arborescence a des nœuds non réalisés, ils ne sont pas pris en compte pour la sélection. Voici quelques aspects à prendre en compte concernant la sélection avec des nœuds non réalisés.

- Si un utilisateur sélectionne un nœud parent, tous les enfants réalisés sous ce parent sont également sélectionnés. De même, si tous les nœuds enfants sont sélectionnés, le nœud parent est également sélectionné.
- La méthode SelectAll ajoute uniquement des nœuds réalisés à la collection SelectedNodes.
- Si un nœud parent avec des enfants non réalisés est sélectionné, les enfants sont sélectionnés lorsqu'ils sont réalisés.

## <a name="code-examples"></a>Exemples de code

### <a name="tree-view-using-xaml"></a>Affichage de l’arborescence à l’aide de XAML

Cet exemple montre comment créer une structure d'arborescence simple en XAML. L’arborescence affiche des parfums de glace et des garnitures que l’utilisateur peut choisir, organisés par catégories. La sélection multiple est activée et lorsque l’utilisateur clique sur un bouton, les SelectedItems s'affichent dans l’interface utilisateur principale de l’application.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
        </SplitView.Pane>

        <StackPanel Grid.Column="1" Margin="12,0">
            <Button Content="Select all" Click="SelectAllButton_Click"/>
            <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
            <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
            <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="ToppingList"/>
        </StackPanel>
    </SplitView>
</Grid>
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
    {
        if (node.Parent.Content?.ToString() == "Flavors")
        {
            FlavorList.Text += node.Content + "; ";
        }
        else if (node.HasChildren == false)
        {
            ToppingList.Text += node.Content + "; ";
        }
    }
}

private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>Affichage de l’arborescence à l’aide de la liaison de données

Cet exemple montre comment créer l’arborescence de même que l’exemple précédent. Toutefois, au lieu de créer la hiérarchie de données dans XAML, les données sont créées dans le code et liées à la propriété ItemsSource de l’arborescence. (Les gestionnaires d’événements de bouton indiqués dans l’exemple précédent s’appliquent à cet exemple également.)

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView Name="DessertTree"
                      SelectionMode="Multiple"
                      ItemsSource="{x:Bind DataSource}">
                <TreeView.ItemTemplate>
                    <DataTemplate x:DataType="local:Item">
                        <TreeViewItem ItemsSource="{x:Bind Children}"
                                      Content="{x:Bind Name}"/>
                    </DataTemplate>
                </TreeView.ItemTemplate>
            </TreeView>
        </SplitView.Pane>

        <StackPanel Grid.Column="1" Margin="12,0">
            <Button Content="Select all" Click="SelectAllButton_Click"/>
            <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
            <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
            <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="ToppingList"/>
        </StackPanel>
    </SplitView>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

    public MainPage()
    {
        this.InitializeComponent();
        DataSource = GetDessertData();
    }

    private ObservableCollection<Item> GetDessertData()
    {
        var list = new ObservableCollection<Item>();
        Item flavorsCategory = new Item()
        {
            Name = "Flavors",
            Children =
            {
                new Item() { Name = "Vanilla" },
                new Item() { Name = "Strawberry" },
                new Item() { Name = "Chocolate" }
            }
        };
        Item toppingsCategory = new Item()
        {
            Name = "Toppings",
            Children =
            {
                new Item()
                {
                    Name = "Candy",
                    Children =
                    {
                        new Item() { Name = "Chocolate" },
                        new Item() { Name = "Mint" },
                        new Item() { Name = "Sprinkles" }
                    }
                },
                new Item()
                {
                    Name = "Fruits",
                    Children =
                    {
                        new Item() { Name = "Mango" },
                        new Item() { Name = "Peach" },
                        new Item() { Name = "Kiwi" }
                    }
                },
                new Item()
                {
                    Name = "Berries",
                    Children =
                    {
                        new Item() { Name = "Strawberry" },
                        new Item() { Name = "Blueberry" },
                        new Item() { Name = "Blackberry" }
                    }
                }
            }
        };

        list.Add(flavorsCategory);
        list.Add(toppingsCategory);
        return list;
    }

    // Button event handlers...
}

public class Item
{
    public string Name { get; set; }
    public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

    public override string ToString()
    {
        return Name;
    }
}
```

### <a name="pictures-and-music-library-tree-view"></a>Arborescence de bibliothèque d'images et de musique

Cet exemple montre comment créer une arborescence montrant le contenu et la structure des bibliothèques d'images et de musique des utilisateurs. Le nombre d’éléments ne peut pas être connu à l'avance, donc chaque nœud est rempli quand il est développé et vidé lorsqu’il est réduit.

Un modèle d’élément personnalisé est utilisé pour afficher les éléments de données, qui sont de type [IStorageItem](/uwp/api/windows.storage.istorageitem).

> [!IMPORTANT]
> Le code dans cet exemple requiert les fonctions picturesLibrary et musicLibrary. Pour plus d’informations sur l’accès aux fichiers, voir [Autorisations d’accès aux fichiers](../../files/file-access-permissions.md), [Énumérer et interroger des fichiers et dossiers](../../files/quickstart-listing-files-and-folders.md) et [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();
    InitializeTreeView();
}

private void InitializeTreeView()
{
    // A TreeView can have more than 1 root node. The Pictures library
    // and the Music library will each be a root node in the tree.
    // Get Pictures library.
    StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
{
    // Get the contents of the folder represented by the current tree node.
    // Add each item as a new child node of the node that's being expanded.

    // Only process the node if it's a folder and has unrealized children.
    StorageFolder folder = null;
    if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
    {
        folder = node.Content as StorageFolder;
    }
    else
    {
        // The node isn't a folder, or it's already been filled.
        return;
    }

    IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

    if (itemsList.Count == 0)
    {
        // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        // that the chevron appears, but don't try to process children that aren't there.
        return;
    }

    foreach (var item in itemsList)
    {
        var newNode = new TreeViewNode();
        newNode.Content = item;

        if (item is StorageFolder)
        {
            // If the item is a folder, set HasUnrealizedChildren to true. 
            // This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = true;
        }
        else
        {
            // Item is StorageFile. No processing needed for this scenario.
        }
        node.Children.Add(newNode);
    }
    // Children were just added to this node, so set HasUnrealizedChildren to false.
    node.HasUnrealizedChildren = false;
}

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}

private void RefreshButton_Click(object sender, RoutedEventArgs e)
{
    sampleTreeView.RootNodes.Clear();
    InitializeTreeView();
}
```

```vb
Public Sub New()
    InitializeComponent()
    InitializeTreeView()
End Sub

Private Sub InitializeTreeView()
    ' A TreeView can have more than 1 root node. The Pictures library
    ' and the Music library will each be a root node in the tree.
    ' Get Pictures library.
    Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
    Dim pictureNode As New TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(pictureNode)
    FillTreeNode(pictureNode)

    ' Get Music library.
    Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
    Dim musicNode As New TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(musicNode)
    FillTreeNode(musicNode)
End Sub

Private Async Sub FillTreeNode(node As TreeViewNode)
    ' Get the contents of the folder represented by the current tree node.
    ' Add each item as a new child node of the node that's being expanded.

    ' Only process the node if it's a folder and has unrealized children.
    Dim folder As StorageFolder = Nothing
    If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
        folder = TryCast(node.Content, StorageFolder)
    Else
        ' The node isn't a folder, or it's already been filled.
        Return
    End If

    Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
    If itemsList.Count = 0 Then
        ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        ' that the chevron appears, but don't try to process children that aren't there.
        Return
    End If

    For Each item In itemsList
        Dim newNode As New TreeViewNode With {
            .Content = item
        }
        If TypeOf item Is StorageFolder Then
            ' If the item is a folder, set HasUnrealizedChildren to True.
            ' This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = True
        Else
            ' Item is StorageFile. No processing needed for this scenario.
        End If
        node.Children.Add(newNode)
    Next

    ' Children were just added to this node, so set HasUnrealizedChildren to False.
    node.HasUnrealizedChildren = False
End Sub

Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub

Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub

Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub

Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
    sampleTreeView.RootNodes.Clear()
    InitializeTreeView()
End Sub
```

## <a name="related-articles"></a>Articles associés

- [Classe TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [Classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView et GridView](listview-and-gridview.md)
