---
author: Jwmsft
Description: "Utilisez l’exemple de code d’arborescence pour créer une arborescence extensible."
title: Arborescence
label: Tree view
template: detail.hbs
ms.openlocfilehash: c7ad99d20fe30ea4b94ad62de45b3832aae3805e
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2017
---
# <a name="hierarchical-layout-with-treeview"></a>Disposition hiérarchique avec TreeView
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

TreeView est un modèle de liste hiérarchique comportant des nœuds de développement et de réduction qui contiennent des éléments imbriqués. Ces derniers peuvent être des nœuds supplémentaires ou des éléments de liste standard. Vous pouvez utiliser un élément [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) pour générer une arborescence afin d’illustrer une structure de dossiers ou des relations imbriquées dans votre interface utilisateur.

L’[exemple TreeView](http://go.microsoft.com/fwlink/?LinkId=785018) est une implémentation de référence générée à l’aide de **ListView**. Il ne s’agit pas d’un contrôle autonome. Le contrôle TreeView rencontré dans le volet Favoris du navigateur Microsoft Edge utilise cette implémentation de référence.

L’exemple prend en charge les éléments suivants:
- Imbrication de niveau N
- Extension/Réduction des nœuds
- Glissement - déplacement des nœuds dans le contrôle TreeView
- Accessibilité intégrée

![Arborescence dans l’exemple de référence](images/tree-view-sample.png) | ![Arborescence dans le navigateur Edge](images/tree-view-edge.png)
-- | --
Exemple de référence TreeView | TreeView dans le navigateur Edge

## <a name="is-this-the-right-pattern"></a>Est-ce le modèle approprié?

- Utilisez un contrôle TreeView lorsque les éléments comportent des éléments de liste imbriqués et s’il est important d’illustrer la relation hiérarchique des éléments par rapport à leurs homologues et leurs nœuds.

- Évitez d’utiliser TreeView si la mise en évidence de la relation imbriquée d’un élément n’est pas une priorité. Pour la plupart des scénarios d’exploration, un affichage sous forme de liste normal est approprié

## <a name="treeview-ui-structure"></a>Structure de l’interface utilisateur de TreeView

Vous pouvez utiliser des icônes pour représenter les nœuds dans un modèle TreeView. Une combinaison de retraits et d’icônes peut être utilisée pour représenter la relation imbriquée existant entre les nœuds parent/de dossier et les nœuds enfant/autre que des dossiers. Voici comment procéder.

### <a name="icons"></a>Icônes

Utilisez des icônes pour indiquer qu’un élément est un nœud, ainsi que son état (développé ou réduit).

#### <a name="chevron"></a>Chevron

Par souci de cohérence, les nœuds réduits doivent utiliser un chevron pointant vers la droite, et les nœuds développés un chevron pointant vers le bas.

![Utilisation de l’icône Chevron dans TreeView](images/treeview_chevron.png)

#### <a name="folder"></a>Dossier

Utilisez une icône de dossier uniquement pour les représentations littérales des dossiers.

![Utilisation de l’icône Dossier dans TreeView](images/treeview_folder.png)

#### <a name="chevron-and-folder"></a>Chevron et Dossier

La combinaison chevron/dossier doit être utilisée uniquement si les éléments de liste autres que des nœuds dans TreeView possèdent également des icônes.

![Utilisation combinée des icônes Chevron et Dossier dans un modèle TreeView](images/treeview_chevron_folder.png)

#### <a name="redlines-for-indentation-of-folders-and-non-folder-nodes"></a>Traits rouges pour la mise en retrait des nœuds de dossier et d’autres types

Utilisez les traits rouges dans la capture d’écran ci-dessous pour la mise en retrait des nœuds de dossier et d’autres types

![Traits rouges pour la mise en retrait des nœuds de dossier et d’autres types](images/treeview_chevron_folder_indent_rl.png)

## <a name="building-a-treeview"></a>Création d’un modèle TreeView

TreeView comporte les classes principales suivantes. Elles sont toutes définies et incluses dans l’implémentation de référence.

> **Remarque**&nbsp;&nbsp;TreeView est implémenté sous forme de [composant Windows Runtime](https://msdn.microsoft.com/windows/uwp/winrt-components/index) écrit en C++. Par conséquent, une application UWP peut y faire référence dans n’importe quelle langue. Dans l’exemple, le code TreeView se trouve dans le dossier *cpp/Control*. Il n’existe aucun dossier *cs/Control* correspondant pour C#.

- La classe `TreeNode` implémente la disposition hiérarchique pour TreeView. Elle conserve également les données qui seront associées dans le modèle d’éléments.
- La classe `TreeView` implémente des événements pour ItemClick, développe/réduit des dossiers, et lance l’opération glisser.
- La classe `TreeViewItem` implémente les événements pour l’opération déplacer.
- La classe `ViewModel` aplatit la liste des TreeViewItems afin que les opérations, telles que la navigation au clavier et les opérations de glisser-déplacer puissent être héritées de ListView.

## <a name="create-a-data-template-for-your-treeviewitem"></a>Créer un modèle de données pour votre TreeViewItem

Voici la partie du code XAML qui configure le modèle de données pour les éléments de type dossier et autre.
- Pour spécifier un ListViewItem comme dossier, vous devez définir explicitement la propriété [AllowDrop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.allowdrop.aspx) sur **true** sur ce ListViewItem. Ce code XAML montre une manière d’y parvenir.
- Pour définir un ListViewItem comme n’étant pas un dossier, il est inutile de spécifier de propriété sur l’élément ListViewItem proprement dit. Il suffit de définir la propriété AllowDrop sur True sur l’élément ListView.
- Vous pouvez utiliser les icônes de dossier de développement/réduction ou des chevrons pour indiquer visuellement si un dossier est développé ou réduit.
- Vous pouvez utiliser des convertisseurs permettant de choisir les différentes icônes nécessaires correspondant à l’état développé/réduit comme illustré dans cet exemple.

```xaml
<!-- MainPage.xaml -->
<DataTemplate x:Key="TreeViewItemDataTemplate">
    <StackPanel Orientation="Horizontal" Height="40" Margin="{Binding Depth, Converter={StaticResource IntToIndConverter}}" AllowDrop="{Binding Data.IsFolder}">
        <FontIcon x:Name="expandCollapseChevron"
                  Glyph="{Binding IsExpanded, Converter={StaticResource expandCollapseGlyphConverter}}"
                  Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"                           
                  FontSize="12"
                  Margin="12,8,12,8"
                  FontFamily="Segoe MDL2 Assets"                          
                  />
        <Grid>
            <FontIcon x:Name ="expandCollapseFolder"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderGlyphConverter}}"
                      Foreground="#FFFFE793"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="nonFolderIcon"
                      Glyph="&#xE160;"
                      Foreground="{ThemeResource SystemControlForegroundBaseLowBrush}"
                      FontSize="12"
                      Margin="20,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource inverseBooleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="expandCollapseFolderOutline"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderOutlineGlyphConverter}}"
                      Foreground="#FFECC849"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"/>
        </Grid>

        <TextBlock Text="{Binding Data.Name}"
                   HorizontalAlignment="Stretch"
                   VerticalAlignment="Center"  
                   FontWeight="Medium"
                   FontFamily="Segoe MDL2 Assests"                           
                   Style="{ThemeResource BodyTextBlockStyle}"/>
    </StackPanel>
</DataTemplate>
```

## <a name="set-up-the-data-in-your-treeview"></a>Configurer les données dans votre TreeView

Voici le code qui configure les données dans l’exemple TreeView.

```csharp
 public MainPage()
 {
     this.InitializeComponent();

     TreeNode workFolder = CreateFolderNode("Work Documents");
     workFolder.Add(CreateFileNode("Feature Functional Spec"));
     workFolder.Add(CreateFileNode("Feature Schedule"));
     workFolder.Add(CreateFileNode("Overall Project Plan"));
     workFolder.Add(CreateFileNode("Feature Resource allocation"));
     sampleTreeView.RootNode.Add(workFolder);

     TreeNode remodelFolder = CreateFolderNode("Home Remodel");
     remodelFolder.IsExpanded = true;
     remodelFolder.Add(CreateFileNode("Contactor Contact Information"));
     remodelFolder.Add(CreateFileNode("Paint Color Scheme"));
     remodelFolder.Add(CreateFileNode("Flooring woodgrain types"));
     remodelFolder.Add(CreateFileNode("Kitchen cabinet styles"));

     TreeNode personalFolder = CreateFolderNode("Personal Documents");
     personalFolder.IsExpanded = true;
     personalFolder.Add(remodelFolder);

     sampleTreeView.RootNode.Add(personalFolder);
 }

 private static TreeNode CreateFileNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) };
 }

 private static TreeNode CreateFolderNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) { IsFolder = true } };
 }
```

Une fois que vous avez terminé les étapes ci-dessus, vous aurez une disposition TreeView/hiérarchique entièrement remplie avec imbrication de niveau n pour le développement/la réduction des dossiers, les opérations de glisser-déplacer entre les dossiers et l’accessibilité intégrée.

Pour fournir à l’utilisateur la possibilité d’ajouter/supprimer des éléments à partir de TreeView, nous vous recommandons d’ajouter un menu contextuel pour exposer ces options à l’utilisateur.


## <a name="related-articles"></a>Articles connexes

- [Exemple TreeView](http://go.microsoft.com/fwlink/?LinkId=785018)
- [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView et GridView](listview-and-gridview.md)
