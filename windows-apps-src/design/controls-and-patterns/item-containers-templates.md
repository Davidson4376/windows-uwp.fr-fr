---
Description: Utiliser des modèles pour modifier l’apparence des éléments dans les contrôles ListView ou GridView.
title: Modèles et des conteneurs d’éléments
label: Item containers and templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: d8eb818d-b62e-4314-a612-f29142dbd93f
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 5b628c4d473c2a74eb63a17c12b17ade43c11964
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244415"
---
# <a name="item-containers-and-templates"></a>Modèles et des conteneurs d’éléments

 

Les contrôles **ListView** et **GridView** gèrent la disposition de leurs éléments (horizontale, verticale, renvoi à la ligne, etc.) et l’interaction de l’utilisateur avec les éléments, mais pas l’affichage de chaque élément à l’écran. La visualisation de l’élément est gérée par les conteneurs d’éléments. Lorsque vous ajoutez des éléments à un affichage de liste, ils sont placés automatiquement dans un conteneur. Le conteneur d’éléments par défaut pour le contrôle ListView est [ListViewItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listviewitem.aspx) ; pour le contrôle GridView, il s’agit de [GridViewItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridviewitem.aspx).

> **API importantes** : [Classe de ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), [propriété ItemTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx), [ItemContainerStyle propriété](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx)


> [!NOTE]
> Les contrôles ListView et GridView proviennent de la classe [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx) ; ils possèdent donc les mêmes fonctionnalités mais affichent les données différemment. Dans cet article, lorsque nous parlons d’affichage de liste, sauf indication contraire, les informations s’appliquent aux contrôles ListView et GridView. Nous pouvons faire référence aux classes telles que ListView ou ListViewItem, mais le préfixe *List* peut être remplacé par *Grid* pour l’équivalent de grille correspondant (GridView ou GridViewItem). 

Ces contrôles de conteneur comprennent deux parties importantes qui se combinent pour créer des visuels finaux affichés pour un élément : le *modèle de données* et le *modèle de contrôle*.

- **Modèle de données** : vous assignez un [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx) à la propriété [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) de l’affichage de liste pour spécifier la façon dont chaque élément de données est affiché.
- **Modèle de contrôle** : le modèle de contrôle fournit la partie de la visualisation d’élément dont l’infrastructure est responsable, comme les états visuels. Vous pouvez utiliser la propriété [ItemContainerStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) pour modifier le modèle de contrôle. En règle générale, vous procédez ainsi pour modifier les couleurs de l’affichage de liste, afin qu’elles correspondent à votre personnalisation ou pour changer l’affichage des éléments sélectionnés.

Cette image montre comment le modèle de contrôle et le modèle de données se combinent pour créer le visuel final d’un élément.

![Modèles de données et de contrôle d’affichage de liste](images/listview-visual-parts.png)

Voici le code XAML qui crée cet élément. Nous donnerons plus de détails sur les modèles plus tard.

```xaml
<ListView Width="220" SelectionMode="Multiple">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid Background="Yellow">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="54"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="44" Height="44"
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Black"
                           FontSize="14" Grid.Column="1"
                           VerticalAlignment="Center"
                           Padding="0,0,54,0"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Background" Value="LightGreen"/>
        </Style>
    </ListView.ItemContainerStyle>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```
 
## <a name="prerequisites"></a>Prérequis

- Nous partons du principe que vous savez comment utiliser un contrôle d’affichage de liste. Pour plus d’informations, consultez l’article [ListView et GridView](listview-and-gridview.md).
- Nous supposons également que vous connaissez les modèles et les styles de contrôle, notamment l’utilisation d’un style inline ou en tant que ressource. Pour plus d’informations, consultez [Application de styles aux contrôles](xaml-styles.md) et [Modèles de contrôle](control-templates.md).

## <a name="the-data"></a>Les données

Avant d’aborder plus en détail comment afficher des éléments de données dans un affichage de liste, nous devons comprendre les données à afficher. Dans cet exemple, nous allons créer un type de données appelé `NamedColor`. Il associe un nom de couleur, une valeur de couleur et un **SolidColorBrush** pour la couleur, qui sont exposés en tant que 3 propriétés : `Name`, `Color` et `Brush`.
 
Nous allons ensuite remplir une **List** avec un objet `NamedColor` pour chaque couleur nommée dans la classe [Colors](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors.aspx). La liste est définie comme la [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) pour l’affichage de liste.

Voici le code pour définir la classe et remplir la liste `NamedColors`.

**C#**
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

namespace ColorsListApp
{
    public sealed partial class MainPage : Page
    {
        // The list of colors won't change after it's populated, so we use List<T>. 
        // If the data can change, we should use an ObservableCollection<T> intead.
        List<NamedColor> NamedColors = new List<NamedColor>();

        public MainPage()
        {
            this.InitializeComponent();

            // Use reflection to get all the properties of the Colors class.
            IEnumerable<PropertyInfo> propertyInfos = typeof(Colors).GetRuntimeProperties();

            // For each property, create a NamedColor with the property name (color name),
            // and property value (color value). Add it the NamedColors list.
            for (int i = 0; i < propertyInfos.Count(); i++)
            {
                NamedColors.Add(new NamedColor(propertyInfos.ElementAt(i).Name,
                                    (Color)propertyInfos.ElementAt(i).GetValue(null)));
            }

            colorsListView.ItemsSource = NamedColors;
        }
    }

    class NamedColor
    {
        public NamedColor(string colorName, Color colorValue)
        {
            Name = colorName;
            Color = colorValue;
        }

        public string Name { get; set; }

        public Color Color { get; set; }

        public SolidColorBrush Brush
        {
            get { return new SolidColorBrush(Color); }
        }
    }
}
```

## <a name="data-template"></a>Modèle de données

Vous spécifiez un modèle de données pour indiquer à l’affichage de liste comment votre élément de données doit être affiché. 

Par défaut, un élément de données est affiché dans l’affichage Liste en tant que représentation de chaîne de l’objet de données auquel il est lié. Si vous affichez les données « NamedColors » dans un affichage de liste sans indiquer à l’affichage de liste leur apparence, il affiche simplement ce que la méthode **ToString** renvoie, comme suit.

**XAML**
```xaml
<ListView x:Name="colorsListView"/>
```

![affichage de liste montrant la représentation de chaîne des éléments](images/listview-no-template.png)

Vous pouvez afficher la représentation de chaîne d’une propriété spécifique de l’élément de données en définissant la propriété [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) sur cette propriété. Ici, vous définissez DisplayMemberPath sur la propriété `Name` de l’élément `NamedColor`.

**XAML**
```xaml
<ListView x:Name="colorsListView" DisplayMemberPath="Name" />
```

L’affichage de liste montre maintenant les éléments par nom, comme illustré ici. Cela est plus pratique, mais pas très intéressant et laisse de nombreuses informations masquées.

![Affichage de liste montrant la représentation de chaîne d’une propriété d’élément](images/listview-display-member-path.png)

En général, on souhaite afficher une représentation enrichie des données. Pour définir précisément la façon dont les éléments sont affichés dans l’affichage Liste, vous devez créer un objet [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx). Le code XAML dans l’objet DataTemplate définit la disposition et l’apparence des contrôles qui permettent d’afficher un élément spécifique. Les contrôles dans la disposition peuvent être liés aux propriétés d’un objet de données ou leur contenu statique peut être défini inline. L’objet DataTemplate est affecté à la propriété [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) de la liste de contrôle.

> [!IMPORTANT]
> Vous ne pouvez pas utiliser **ItemTemplate** et **DisplayMemberPath** simultanément. Si les deux propriétés sont définies, une exception se produit.

Ici, vous définissez un DataTemplate qui montre un [Rectangle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.aspx) de la couleur de l’élément, ainsi que le nom de couleur et les valeurs RVB. 

> [!NOTE]
> Si vous utilisez l’[extension de balisage x:Bind](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) dans un DataTemplate, vous devez spécifier le DataType (`x:DataType`) sur le DataTemplate.

**XAML**
```XAML
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:NamedColor">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition MinWidth="54"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Rectangle Width="44" Height="44" Fill="{x:Bind Brush}" Grid.RowSpan="2"/>
                <TextBlock Text="{x:Bind Name}" Grid.Column="1" Grid.ColumnSpan="4"/>
                <TextBlock Text="{x:Bind Color.R}" Grid.Column="1" Grid.Row="1" Foreground="Red"/>
                <TextBlock Text="{x:Bind Color.G}" Grid.Column="2" Grid.Row="1" Foreground="Green"/>
                <TextBlock Text="{x:Bind Color.B}" Grid.Column="3" Grid.Row="1" Foreground="Blue"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Voici ce à quoi ressembleront les éléments de données affichés avec ce modèle de données.

![Éléments de l’affichage Liste avec un modèle de données](images/listview-data-template-0.png)

Vous souhaiterez peut-être afficher les données dans un contrôle GridView. Voici un autre modèle de données qui affiche les données d’une façon plus appropriée pour une disposition en grille. Cette fois, le modèle de données est défini en tant que ressource plutôt qu’inline avec le code XAML pour le contrôle GridView.


**XAML**
```xaml
<Page.Resources>
    <DataTemplate x:Key="namedColorItemGridTemplate" x:DataType="local:NamedColor">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="96"/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
    
            <Rectangle Width="96" Height="96" Fill="{x:Bind Brush}" Grid.ColumnSpan="3" />
            <!-- Name -->
            <Border Background="#AAFFFFFF" Grid.ColumnSpan="3" Height="40" VerticalAlignment="Top">
                <TextBlock Text="{x:Bind Name}" TextWrapping="Wrap" Margin="4,0,0,0"/>
            </Border>
            <!-- RGB -->
            <Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
            <TextBlock Text="{x:Bind Color.R}" Foreground="Red"
                   Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.G}" Foreground="Green"
                   Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
                   Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
            <!-- HEX -->
            <Border Background="Gray" Grid.Row="2" Grid.ColumnSpan="3">
                <TextBlock Text="{x:Bind Color}" Foreground="White" Margin="4,0,0,0"/>
            </Border>
        </Grid>
    </DataTemplate>
</Page.Resources>

...

<GridView x:Name="colorsGridView" 
          ItemTemplate="{StaticResource namedColorItemGridTemplate}"/>
```

Lorsque les données sont affichées dans une grille à l’aide de ce modèle de données, le résultat est le suivant.

![Éléments d’affichage de grille avec un modèle de données](images/gridview-data-template.png)

### <a name="performance-considerations"></a>Considérations relatives aux performances

Les modèles de données sont le principal moyen de définir l’aspect de votre affichage Liste. Ils peuvent également avoir un impact significatif sur les performances si votre liste affiche un grand nombre d’éléments. 

Une instance de chaque élément XAML dans un modèle de données est créée pour chaque élément dans l’affichage de liste. Par exemple, le modèle de grille dans l’exemple précédent compte 10 éléments XAML (1 Grid, 1 Rectangle, 3 Borders, 5 TextBlocks). Un contrôle GridView qui montre les 20 éléments sur l’écran à l’aide de ce modèle de données crée au moins 200 éléments (20*10 = 200). Réduire le nombre d’éléments dans un modèle de données peut réduire de manière considérable le nombre total d’éléments créés pour votre affichage de liste. Pour plus d’informations, consultez [ListView et GridView UI optimisation : Réduction du nombre élément par élément](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview#element-reduction-per-item).

 Tenez compte de cette section du modèle de données de grille. Examinons quelques points permettant de réduire le nombre d’éléments.

**XAML**
```xaml
<!-- RGB -->
<Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
<TextBlock Text="{x:Bind Color.R}" Foreground="Red"
           Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.G}" Foreground="Green"
           Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
           Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
```

 - Tout d’abord, la disposition utilise un seul élément Grid. Vous pourriez avoir un élément Grid à une seule colonne et placer ces 3 TextBlocks dans un élément StackPanel, mais dans un modèle de données qui est créé à de nombreuses reprises, vous devez rechercher des moyens permettant d’éviter l’incorporation des panneaux de disposition au sein d’autres panneaux de disposition.
 - Deuxièmement, vous pouvez utiliser un contrôle Border pour générer un arrière-plan sans réellement les éléments dans l’élément Border. Un élément Border ne peut avoir qu’un seul élément enfant. Vous allez donc devoir ajouter un panneau de disposition supplémentaire pour héberger les 3 éléments TextBlock au sein de l’élément Border en XAML. En ne définissant pas les éléments TextBlock comme enfants de l’élément Border, vous n’avez plus besoin de panneau pour contenir les éléments TextBlock.
 - Enfin, vous pouvez placer les éléments TextBlock dans un StackPanel et définir les propriétés de bordure de l’élément StackPanel plutôt que d’utiliser un élément Border explicite. Toutefois, l’élément Border est un contrôle beaucoup plus léger que StackPanel et a donc moins d’impact sur les performances lorsqu’il est généré à plusieurs reprises.

## <a name="control-template"></a>Modèle de contrôle
Un modèle de contrôle d’un élément contient les visuels qui affichent l’état, tels que la sélection, le pointage et le focus. Ces visuels sont générés au-dessus ou en dessous du modèle de données. Certains des visuels par défaut les plus courants dessinés par le modèle de contrôle ListView sont affichés ici.

- Pointage : un rectangle gris clair dessiné sous le modèle de données.  
- Sélection : un rectangle bleu clair dessiné sous le modèle de données. 
- Focus clavier : une bordure en pointillés noir et blanc dessinée au-dessus du modèle d’élément. 

![Visuels d’état de l’affichage de liste](images/listview-state-visuals.png)

L’affichage de liste combine les éléments à partir du modèle de données et du modèle de contrôle pour créer les visuels finaux affichés sur l’écran. Ici, les visuels d’état sont affichés dans le contexte d’un affichage de liste.

![Affichage de liste avec des éléments dans différents états](images/listview-states.png)

### <a name="listviewitempresenter"></a>ListViewItemPresenter

Comme nous l’avons remarqué précédemment pour les modèles de données, le nombre d’éléments XAML créés pour chaque élément peut avoir un impact considérable sur les performances d’un affichage de liste. Étant donné que le modèle de données et le modèle de contrôle sont combinés pour afficher chaque élément, le nombre réel d’éléments nécessaires pour afficher un élément comprend les éléments des deux modèles.

Les contrôles ListView et GridView sont optimisés pour réduire le nombre d’éléments XAML créés par élément. Les visuels **ListViewItem** sont créés par le [ListViewItemPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx), qui est un élément XAML spécial affichant des visuels complexes pour le focus, la sélection et d’autres états visuels, sans la surcharge de nombreux éléments UIElement.
 
> [!NOTE]
> Dans les applications UWP pour Windows 10, **ListViewItem** et **GridViewItem** utilisent **ListViewItemPresenter** ; GridViewItemPresenter est déconseillé, et vous ne devez pas l’utiliser. ListViewItem et GridViewItem définissent des valeurs de propriété différentes sur ListViewItemPresenter afin d’obtenir des apparences par défaut différentes.

Pour modifier l’apparence du conteneur d’éléments, utilisez la propriété [ItemContainerStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx) et fournissez un [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.aspx) avec son [TargetType](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.targettype.aspx) défini sur **ListViewItem** ou **GridViewItem**.

Dans cet exemple, vous ajoutez un espacement à ListViewItem pour créer de l’espace entre les éléments de la liste.

```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <!-- DataTemplate XAML shown in previous ListView example -->
    </ListView.ItemTemplate>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Padding" Value="0,4"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

L’affichage de liste se présente maintenant comme suit avec un espacement entre les éléments.

![Éléments d’affichage de liste avec espacement appliqué](images/listview-data-template-1.png)

Dans le style par défaut ListViewItem, la propriété **ContentMargin** de ListViewItemPresenter a un [TemplateBinding](https://msdn.microsoft.com/windows/uwp/xaml-platform/templatebinding-markup-extension) pour la propriété **Padding** de ListViewItem (`<ListViewItemPresenter ContentMargin="{TemplateBinding Padding}"/>`). Lorsque nous définissons la propriété Padding, cette valeur est vraiment transmise à la propriété ContentMargin de ListViewItemPresenter.

Pour modifier les autres propriétés de ListViewItemPresenter qui ne sont pas liées par modèle aux propriétés de ListViewItems, vous devez redéfinir le ListViewItem avec un nouveau ListViewItemPresenter sur lequel vous pouvez modifier les propriétés. 

> [!NOTE]
> Les styles par défaut de ListViewItem et GridViewItem définissent un grand nombre de propriétés sur ListViewItemPresenter. Vous devez toujours commencer avec une copie du style par défaut et modifiez uniquement les propriétés nécessaires. Dans le cas contraire, les visuels ne s’afficheront probablement pas comme vous le souhaitez, car certaines propriétés n’auront pas été définies correctement.

**Pour faire une copie du modèle par défaut dans Visual Studio**
 
1. Ouvrir le volet Structure du document (**Affichage &gt; Autres fenêtres &gt; Structure du document**).
2. Sélectionnez l’élément de liste ou de grille à modifier. Dans cet exemple, vous allez modifier l’élément `colorsGridView`.
3. Cliquez avec le bouton droit et sélectionnez **Modifier les modèles supplémentaires &gt; Modifier le conteneur d’éléments généré (ItemContainerStyle) &gt; Modifier une copie**.
    ![Éditeur Visual Studio](images/listview-itemcontainerstyle-vs.png)
4. Dans la boîte de dialogue Créer la ressource Style, entrez un nom pour le style. Dans cet exemple, vous utilisez `colorsGridViewItemStyle`.
    ![Boîte de dialogue Créer la ressource Style Visual Studio(images/listview-style-resource-vs.png)

Une copie du style par défaut est ajoutée à votre application en tant que ressource et la propriété **GridView.ItemContainerStyle** est définie sur cette ressource, comme le montre ce code XAML. 

```xaml
<Style x:Key="colorsGridViewItemStyle" TargetType="GridViewItem">
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="Foreground" Value="{ThemeResource SystemControlForegroundBaseHighBrush}"/>
    <Setter Property="TabNavigation" Value="Local"/>
    <Setter Property="IsHoldingEnabled" Value="True"/>
    <Setter Property="HorizontalContentAlignment" Value="Center"/>
    <Setter Property="VerticalContentAlignment" Value="Center"/>
    <Setter Property="Margin" Value="0,0,4,4"/>
    <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
    <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="GridViewItem">
                <ListViewItemPresenter 
                    CheckBrush="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                    ContentMargin="{TemplateBinding Padding}" 
                    CheckMode="Overlay" 
                    ContentTransitions="{TemplateBinding ContentTransitions}" 
                    CheckBoxBrush="{ThemeResource SystemControlBackgroundChromeMediumBrush}" 
                    DragForeground="{ThemeResource ListViewItemDragForegroundThemeBrush}" 
                    DragOpacity="{ThemeResource ListViewItemDragThemeOpacity}" 
                    DragBackground="{ThemeResource ListViewItemDragBackgroundThemeBrush}" 
                    DisabledOpacity="{ThemeResource ListViewItemDisabledThemeOpacity}" 
                    FocusBorderBrush="{ThemeResource SystemControlForegroundAltHighBrush}" 
                    FocusSecondaryBorderBrush="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}" 
                    PointerOverForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    PressedBackground="{ThemeResource SystemControlHighlightListMediumBrush}" 
                    PlaceholderBackground="{ThemeResource ListViewItemPlaceholderBackgroundThemeBrush}" 
                    PointerOverBackground="{ThemeResource SystemControlHighlightListLowBrush}" 
                    ReorderHintOffset="{ThemeResource GridViewItemReorderHintThemeOffset}" 
                    SelectedPressedBackground="{ThemeResource SystemControlHighlightListAccentHighBrush}" 
                    SelectionCheckMarkVisualEnabled="True" 
                    SelectedForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    SelectedPointerOverBackground="{ThemeResource SystemControlHighlightListAccentMediumBrush}" 
                    SelectedBackground="{ThemeResource SystemControlHighlightAccentBrush}" 
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>

...

<GridView x:Name="colorsGridView" ItemContainerStyle="{StaticResource colorsGridViewItemStyle}"/>
```

Vous pouvez maintenant modifier les propriétés de ListViewItemPresenter pour contrôler la case de sélection, le positionnement de l’élément et les couleurs de pinceau pour les états visuels. 

#### <a name="inline-and-overlay-selection-visuals"></a>Visuels de sélection Inline et Overlay

ListView et GridView indiquent les éléments sélectionnés de différentes façons, selon le contrôle et le [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx). Pour plus d’informations sur la sélection d’affichage de liste, consultez [ListView et GridView](listview-and-gridview.md). 

Quand **SelectionMode** est défini sur **Multiple**, une case à cocher de sélection s’affiche dans le modèle de contrôle de l’élément. Vous pouvez utiliser la propriété [SelectionCheckMarkVisualEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled.aspx) pour désactiver la case de sélection en mode de sélection Multiple. Toutefois, cette propriété est ignorée dans les autres modes de sélection, afin que vous ne puissiez pas activer la case à cocher en mode de sélection Extended ou Single.

Vous pouvez définir la propriété [CheckMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode.aspx) pour spécifier si la case à cocher s’affiche avec le style Inline ou le style Overlay.

- **Inline**: Ce style montre la case à cocher à gauche du contenu et l’arrière-plan du conteneur d’élément pour indiquer la sélection des couleurs. Il s’agit du style par défaut pour ListView.
- **Superposition**: Ce style montre la case à cocher sur le contenu et seule la bordure du conteneur d’élément pour indiquer la sélection des couleurs. Il s’agit du style par défaut pour GridView.

Le tableau suivant montre les visuels par défaut utilisés pour indiquer la sélection.

SelectionMode :&nbsp;&nbsp; | Single/Extended | Plusieurs
---------------|-----------------|---------
Inline | ![Inline, sélection Extended ou Single](images/listview-single-selection.png) | ![Inline, sélection Multiple](images/listview-multi-selection.png)
Superposition | ![Overlay, sélection Extended ou Single](images/gridview-single-selection.png) | ![Overlay, sélection Multiple](images/gridview-multi-selection.png)

> [!NOTE]
> Dans cet exemple et les suivants, les éléments de données de chaîne simple sont présentés sans modèle de données pour mettre en évidence les visuels fournis par le modèle de contrôle.

Il existe également plusieurs propriétés de pinceau pour modifier les couleurs de la case à cocher. Nous examinerons celles-ci et d’autres propriétés de pinceau plus tard.

#### <a name="brushes"></a>Pinceaux 

De nombreuses propriétés spécifient les pinceaux utilisés pour les différents états visuels. Vous souhaiterez peut-être les modifier pour qu’elles correspondent à la couleur de votre marque. 

Ce tableau indique les états visuels Common et Selection pour ListViewItem et les pinceaux utilisés pour le rendu des visuels pour chaque état. Les images illustrent les effets des pinceaux sur les styles visuels de sélection Inline et Overlay.

> [!NOTE]
> Dans ce tableau, les valeurs de couleur modifiées pour les pinceaux sont des couleurs nommées codées en dur, et les couleurs sont sélectionnées pour les rendre plus visibles lorsqu’elles sont appliquées dans le modèle. Il ne s’agit pas des couleurs par défaut pour les états visuels. Si vous modifiez les couleurs par défaut dans votre application, vous devez utiliser les ressources de pinceau pour modifier les valeurs de couleur, comme illustré dans le modèle par défaut.

Nom de pinceau/d’état | Style Inline | Style Overlay
------------|--------------|--------------
<b>Normal</b><ul><li><b>CheckBoxBrush="Red"</b></li></ul> | ![Inline, sélection d’élément Normal](images/listview-item-normal.png) | ![Overlay, sélection d’élément Normal](images/gridview-item-normal.png)
<b>PointerOver</b><ul><li><b>PointerOverForeground="DarkOrange"</b></li><li><b>PointerOverBackground="MistyRose"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Inline, sélection d’élément PointerOver](images/listview-item-pointerover.png) | ![Overlay, sélection d’élément PointerOver](images/gridview-item-pointerover.png)
<b>Pressed</b><ul><li><b>PressedBackground="LightCyan"</b></li><li>PointerOverForeground="DarkOrange"</li><li>CheckBoxBrush="Red"</li></ul> | ![Inline, sélection d’élément Pressed](images/listview-item-pressed.png) | ![Overlay, sélection d’élément Pressed](images/gridview-item-pressed.png)
<b>Selected</b><ul><li><b>SelectedForeground="Navy"</b></li><li><b>SelectedBackground="Khaki"</b></li><li><b>CheckBrush="Green"</b></li><li>CheckBoxBrush="Red" (Inline uniquement)</li></ul> | ![Inline, sélection d’élément Selected](images/listview-item-selected.png) | ![Overlay, sélection d’élément Selected](images/gridview-item-selected.png)
<b>PointerOverSelected</b><ul><li><b>SelectedPointerOverBackground="Lavender"</b></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (Overlay uniquement)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (Inline uniquement)</li></ul> | ![Inline, sélection d’élément PointerOverSelected](images/listview-item-pointeroverselected.png) | ![Overlay, sélection d’élément PointerOverSelected](images/gridview-item-pointeroverselected.png)
<b>PressedSelected</b><ul><li><b>SelectedPressedBackground="MediumTurquoise"</b></li></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (Overlay uniquement)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (Inline uniquement)</li></ul> | ![Inline, sélection d’élément PressedSelected](images/listview-item-pressedselected.png) | ![Overlay, sélection d’élément PressedSelected](images/gridview-item-pressedselected.png)
<b>Focused</b><ul><li><b>FocusBorderBrush="Crimson"</b></li><li><b>FocusSecondaryBorderBrush="Gold"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Inline, sélection d’élément Focused](images/listview-item-focused.png) | ![Overlay, sélection d’élément Focused](images/gridview-item-focused.png)

ListViewItemPresenter a d’autres propriétés de pinceau pour les espaces réservés de données et les états de glissement. Si vous utilisez le chargement incrémentiel ou le glisser-déposer dans votre affichage de liste, vous devez vous demander si ces propriétés de pinceaux supplémentaires doivent être modifiées. Consultez la classe ListViewItemPresenter pour obtenir la liste complète des propriétés que vous pouvez modifier. 

### <a name="expanded-xaml-item-templates"></a>Modèles d’élément XAML développés

Si vous devez apporter plus de modifications que ce qui est autorisé par les propriétés **ListViewItemPresenter** (par exemple, si vous avez besoin de changer la position de la case à cocher), vous pouvez utiliser les modèles *ListViewItemExpanded* ou *GridViewItemExpanded*. Ces modèles sont inclus avec les styles par défaut dans generic.xaml. Ils suivent le modèle XAML standard de création de tous les visuels à partir des éléments UIElements individuels.

Comme mentionné précédemment, le nombre d’éléments UIElements dans un modèle d’élément a un impact significatif sur les performances de votre affichage de liste. Remplacer ListViewItemPresenter par les modèles XAML développés augmente considérablement le nombre d’éléments et cela est déconseillé lorsque votre affichage de liste montre un grand nombre d’éléments ou lorsque les performances sont une priorité.

> [!NOTE]
> **ListViewItemPresenter** est pris en charge uniquement lorsque [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) de l’affichage de liste est de type [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx) ou [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx). Si vous modifiez l’élément ItemsPanel pour utiliser [VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.variablesizedwrapgrid.aspx), [WrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.wrapgrid.aspx) ou [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx), alors le modèle d’élément est remplacé automatiquement par le modèle XAML développé. Pour plus d’informations, consultez [Optimisation des options d’interface ListView et GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)

Pour personnaliser un modèle XAML développé, vous devez faire une copie de ce dernier dans votre application et définir la propriété **ItemContainerStyle** sur votre copie.

**Pour copier le modèle développé**
1. Définissez la propriété ItemContainerStyle comme indiqué ici pour votre contrôle ListView ou GridView.
    ```xaml
    <ListView ItemContainerStyle="{StaticResource ListViewItemExpanded}"/>
    <GridView ItemContainerStyle="{StaticResource GridViewItemExpanded}"/>
    ```
2. Dans le volet Propriétés de Visual Studio, développez la section Divers et recherchez la propriété ItemContainerStyle. (Assurez-vous que le contrôle ListView ou GridView est sélectionné).
3. Cliquez sur le marqueur de propriété pour la propriété ItemContainerStyle. (Il s’agit de la petite case en regard de l’élément TextBox. Ses coloreed vert pour montrer qu’il est défini sur StaticResource.) Le menu des propriétés s’ouvre.
4. Dans le menu des propriétés, cliquez sur **Convertir en nouvelle ressource...** 
    
    ![Menu des propriétés Visual Studio](images/listview-convert-resource-vs.png)
5. Dans la boîte de dialogue Créer la ressource Style, entrez un nom pour la ressource et cliquez sur OK.

Une copie du modèle développé de generic.xaml est créée dans votre application, copie que vous pouvez modifier selon les besoins.


## <a name="related-articles"></a>Articles connexes

- [Listes](lists.md)
- [ListView et GridView](listview-and-gridview.md)

