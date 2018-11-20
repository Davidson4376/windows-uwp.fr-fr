---
author: jwmsft
ms.assetid: 26DF15E8-2C05-4174-A714-7DF2E8273D32
title: Optimisation des options d’interface ListView et GridView
description: Optimisez ListView/GridView et le temps de démarrage via la virtualisation de l’interface, la réduction des éléments et la mise à jour progressive des éléments.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25eeea58e1e03eedfca3aaafda1cee13cac1f3c4
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7300983"
---
# <a name="listview-and-gridview-ui-optimization"></a>Optimisation des options d’interface ListView et GridView


**Remarque**  pour plus d’informations, voir la session //build/ [Accroître considérablement les performances lorsque les utilisateurs interagissent avec de grandes quantités de données dans GridView et ListView](https://channel9.msdn.com/events/build/2013/3-158).

Améliorez les performances des contrôles [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) et [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) et le temps de démarrage à travers la virtualisation de l’interface utilisateur, la réduction des éléments et la mise à jour progressive des éléments. Pour découvrir les techniques de la virtualisation des données, voir [la virtualisation des données ListView et GridView](listview-and-gridview-data-optimization.md).

## <a name="two-key-factors-in-collection-performance"></a>Deux facteurs de performances clés pour les collections

La manipulation des collections est un scénario courant. Une visionneuse de photos présente des collections de photos, un lecteur des collections d’articles/ouvrages/histoires et une application d’achat des collections de produits. Cette rubrique vous explique comment faire pour que votre application manipule efficacement les collections.

Il existe deux facteurs de performances clés en matière de collections : le temps que met le thread d’interface utilisateur à créer les éléments et la mémoire utilisée par le jeu de données brutes et les éléments d’interface utilisateur pour le rendu de ces données.

Pour garantir des panoramiques/défilements fluides, il est essentiel que le thread d’interface utilisateur effectue l’instanciation, la liaison de données et la disposition des éléments de manière efficace et intelligente.

## <a name="ui-virtualization"></a>Virtualisation de l’interface utilisateur

La virtualisation de l’interface utilisateur est le principal axe d’amélioration. Elle implique la création à la demande des éléments d’interface utilisateur. Pour un contrôle d’éléments lié à une collection de 1 000 éléments, créer l’interface utilisateur simultanément pour tous les éléments constituerait un gaspillage de ressources, car il n’est pas possible d’afficher tous les éléments en même temps. Les contrôles [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) et [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) (et d’autres contrôles standard dérivés de [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803)) se chargent de la virtualisation de l’interface utilisateur à votre place. Lorsque des éléments vont bientôt défiler dans l’affichage (dans quelques pages), l’infrastructure génère l’interface utilisateur pour ces éléments et les met en cache. Lorsqu’il est peu probable que les éléments soient de nouveau affichés, l’infrastructure récupère la mémoire qui leur était allouée.

Si vous fournissez un modèle de panneau d’éléments personnalisé (voir [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)), veillez à utiliser un volet de virtualisation tel que [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) ou [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795). Si vous utilisez [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227651), [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227717) ou [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635), vous ne pourrez pas bénéficier de la virtualisation. Par ailleurs, les événements [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) suivants sont déclenchés uniquement lors de l’utilisation d’un [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) ou d’un [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795): [**ChoosingGroupHeaderContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer), [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) et [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging).

La fenêtre d’affichage est un concept essentiel de la virtualisation de l’interface utilisateur, car l’infrastructure doit créer les éléments qui sont susceptibles d’être affichés. La fenêtre d’affichage d’un contrôle [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) est généralement l’extension du contrôle logique. Par exemple, la fenêtre d’affichage d’un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) correspond à la largeur et à la hauteur de l’élément **ListView**. Certains volets offrent un espace illimité aux éléments enfants (par exemple, [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) et [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)) avec un dimensionnement automatique des lignes ou des colonnes. Lorsqu’un contrôle **ItemsControl** virtualisé est placé dans un volet de ce type, il occupe suffisamment d’espace pour afficher tous ses éléments, ce qui va à l’encontre de la virtualisation. Restaurez la virtualisation en définissant une largeur et une hauteur pour **ItemsControl**.

## <a name="element-reduction-per-item"></a>Réduction des éléments par élément

Limitez les éléments d’interface utilisateur utilisés pour afficher vos éléments à un nombre raisonnable.

Lorsqu’un contrôle d’éléments est affiché pour la première fois, tous les éléments nécessaires au rendu d’une fenêtre d’affichage remplie d’éléments sont créés. De plus, à mesure que les éléments approchent de la fenêtre d’affichage, l’infrastructure met à jour les éléments d’interface utilisateur dans les modèles d’éléments mis en cache avec les objets de données liées. Réduire la complexité du balisage à l’intérieur des modèles est payant en termes de mémoire utilisée et de temps passé sur le thread d’interface utilisateur, avec une amélioration de la réactivité particulièrement notable lors des panoramiques/défilements. Les modèles en question sont le modèle d’éléments (voir [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)) et le modèle de contrôle d’un élément [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) ou [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) (le modèle de contrôle d’éléments, ou [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)). L’avantage d’une réduction même minime du nombre d’éléments est multiplié par le nombre d’éléments affichés.

Pour obtenir des exemples de réduction des éléments, voir[Optimiser votre balisage XAML](optimize-xaml-loading.md).

Les modèles de contrôle par défaut pour [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) et [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) contiennent un élément [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/Dn298500). Ce présentateur constitue un élément optimisé unique qui affiche des effets visuels complexes pour le focus, la sélection et d’autres états visuels. Si vous disposez déjà de modèles de contrôle d’éléments personnalisés ([**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)) ou si vous modifiez par la suite une copie d’un modèle de contrôle d’éléments, nous vous recommandons d’utiliser un présentateur **ListViewItemPresenter** car cet élément vous offrira dans la plupart des cas un équilibre optimal entre performances et possibilités de personnalisation. Ce présentateur peut être personnalisé en définissant des propriétés pour lui. Par exemple, voici un balisage qui supprime la coche qui s’affiche par défaut quand un élément est sélectionné, puis remplace la couleur d’arrière-plan de l’élément sélectionné par la couleur orange.

```xml
...
<ListView>
 ...
 <ListView.ItemContainerStyle>
 <Style TargetType="ListViewItem">
 <Setter Property="Template">
 <Setter.Value>
 <ControlTemplate TargetType="ListViewItem">
 <ListViewItemPresenter SelectionCheckMarkVisualEnabled="False" SelectedBackground="Orange"/>
 </ControlTemplate>
 </Setter.Value>
 </Setter>
 </Style>
 </ListView.ItemContainerStyle>
</ListView>
<!-- ... -->
```

Il y a environ 25propriétés portant des noms autodescriptifs similaires à [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled.aspx) et [**SelectedBackground**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedbackground.aspx). Si les types de présentateur se révèlent ne pas être suffisamment personnalisables pour votre cas d’utilisation, vous pouvez modifier une copie du modèle de contrôle `ListViewItemExpanded` ou `GridViewItemExpanded` à la place. Ces modèles se trouvent dans `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`. Notez que l’utilisation de ces modèles implique un compromis en termes de performances en échange de l’augmentation des possibilités de personnalisation.

<span id="update-items-incrementally"/>

## <a name="update-listview-and-gridview-items-progressively"></a>Mettre à jour les éléments ListView et GridView de façon progressive

Si vous utilisez la virtualisation des données, vous pouvez maintenir la réactivité de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) et [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) élevée en configurant le contrôle de façon à afficher des éléments d’interface utilisateur temporaires à la place des éléments encore en cours de (télé)chargement. Les éléments temporaires sont ensuite progressivement remplacés par l’interface utilisateur réelle à mesure que les données sont chargées.

De plus, quelle que soit votre source de chargement des données (disque local, réseau ou cloud), il se peut qu’un utilisateur parcoure un contrôle [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) ou [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) si rapidement qu’il est impossible d’afficher chaque élément avec une fidélité totale tout en préservant la fluidité des panoramiques/défilements. Pour préserver la fluidité des panoramiques/défilements, vous pouvez choisir d’afficher un élément par phases en plus d’utiliser des espaces réservés.

Ce type de techniques est souvent utilisé dans les applications de visionnage de photos : même si toutes les images n’ont pas été chargées et affichées, l’utilisateur peut toujours les parcourir à l’aide de panoramiques/défilements et interagir avec la collection. Ou, dans le cas d’un élément « film », vous pouvez afficher le titre à la première phase, la note à la deuxième et une image de l’affiche à la troisième. L’utilisateur a accès aux données les plus importantes sur chaque élément le plus tôt possible, ce qui signifie qu’il est en mesure d’effectuer une action immédiatement. Les informations moins importantes sont ensuite renseignées au fur et à mesure. Voici les fonctionnalités de plateforme que vous pouvez utiliser pour implémenter ces techniques.

### <a name="placeholders"></a>Espaces réservés

La fonctionnalité d’espaces réservés visuels temporaires est activée par défaut et est contrôlée par la propriété [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders). Pendant les panoramiques/défilements rapides, cette fonctionnalité indique de façon visuelle à l’utilisateur que certains éléments ne sont pas encore complètement affichés, tout en préservant la fluidité de l’expérience. Si vous utilisez une des techniques ci-dessous, vous pouvez définir **ShowsScrollingPlaceholders** sur False si vous ne souhaitez pas que le système restitue les espaces réservés.

**Mises à jour de modèles de données progressives avec x:Phase**

Voici comment utiliser l’[attribut x:Phase](https://msdn.microsoft.com/library/windows/apps/Mt204790) avec les liaisons [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) pour implémenter des mises à jour de modèles de données progressives.

1.  Voici à quoi ressemble la source de liaison (il s’agit de la source de données que nous allons lier):

    ```csharp
    namespace LotsOfItems
    {
        public class ExampleItem
        {
            public string Title { get; set; }
            public string Subtitle { get; set; }
            public string Description { get; set; }
        }

        public class ExampleItemViewModel
        {
            private ObservableCollection<ExampleItem> exampleItems = new ObservableCollection<ExampleItem>();
            public ObservableCollection<ExampleItem> ExampleItems { get { return this.exampleItems; } }

            public ExampleItemViewModel()
            {
                for (int i = 1; i < 150000; i++)
                {
                    this.exampleItems.Add(new ExampleItem(){
                        Title = "Title: " + i.ToString(),
                        Subtitle = "Sub: " + i.ToString(),
                        Description = "Desc: " + i.ToString()
                    });
                }
            }
        }
    }
    ```
2.  Voici le balisage que contient le fichier `DeferMainPage.xaml`. L’affichage grille contient un modèle d’élément avec des éléments liés aux propriétés **Title**, **Subtitle** et **Description** de la classe **MyItem**. Notez que **x:Phase** a pour valeur par défaut0. Ici, les éléments sont affichés initialement avec seulement le titre visible. L’élément sous-titre est ensuite lié aux données et affiché pour tous les éléments, et ainsi de suite jusqu’à ce que toutes les phases aient été traitées.
    ```xml
    <Page
        x:Class="LotsOfItems.DeferMainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Text="{x:Bind Subtitle}" x:Phase="1"/>
                            <TextBlock Text="{x:Bind Description}" x:Phase="2"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```

3.  Si vous exécutez l’application maintenant et que vous parcourez rapidement l’affichage grille à l’aide de panoramiques/défilements, vous remarquerez que lorsque chaque nouvel élément apparaît à l’écran, il est tout d’abord affiché sous forme de rectangle gris foncé (grâce à la propriété [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) par défaut **true**), puis le titre s’affiche, suivi du sous-titre, puis d’une description.

**Mises à jour de modèles de données progressives avec ContainerContentChanging**

La stratégie générale pour l’événement [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) consiste à utiliser **Opacity** pour masquer les éléments qui n’ont pas besoin d’être immédiatement visibles. Lorsque les éléments sont recyclés, ils conservent leurs anciennes valeurs. C’est pourquoi nous souhaitons masquer ces éléments jusqu’à ce que nous ayons mis à jour ces valeurs à partir du nouvel élément de données. Nous utilisons la propriété **Phase** sur les arguments d’événement pour déterminer les éléments à mettre à jour et à afficher. Si des phases supplémentaires sont nécessaires, nous inscrivons un rappel.

1.  Nous utilisons la même source de liaison que pour **x:Phase**.

2.  Voici le balisage que contient le fichier `MainPage.xaml`. L’affichage grille déclare un gestionnaire à son événement [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) et contient un modèle d’élément avec des éléments utilisés pour afficher les propriétés **Title**, **Subtitle** et **Description** de la classe **MyItem**. Pour exploiter pleinement les avantages de **ContainerContentChanging** en termes de performances, nous n’utilisons pas de liaisons dans le balisage, mais nous attribuons à la place des valeurs par programme. L’exception ici est l’élément affichant le titre, que nous considérons dans la phase0.
    ```xml
    <Page
        x:Class="LotsOfItems.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}" ContainerContentChanging="GridView_ContainerContentChanging">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Opacity="0"/>
                            <TextBlock Opacity="0"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```
3.  Enfin, voici l’implémentation du gestionnaire d’événements [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging). Ce code montre également comment nous ajoutons une propriété de type **RecordingViewModel** à **MainPage** pour exposer la classe de source de liaison à partir de la classe qui représente notre page de balisage. À condition que votre modèle de données ne comporte aucune liaison [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), marquez l’objet arguments d’événement comme géré dans la première phase du gestionnaire pour indiquer à l’élément qu’il n’a pas besoin de définir un contexte de données.
    ```csharp
    namespace LotsOfItems
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                this.ViewModel = new ExampleItemViewModel();
            }

            public ExampleItemViewModel ViewModel { get; set; }

            // Display each item incrementally to improve performance.
            private void GridView_ContainerContentChanging(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 0)
                {
                    throw new System.Exception("We should be in phase 0, but we are not.");
                }

                // It's phase 0, so this item's title will already be bound and displayed.

                args.RegisterUpdateCallback(this.ShowSubtitle);

                args.Handled = true;
            }

            private void ShowSubtitle(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 1)
                {
                    throw new System.Exception("We should be in phase 1, but we are not.");
                }

                // It's phase 1, so show this item's subtitle.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[1] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Subtitle;
                textBlock.Opacity = 1;

                args.RegisterUpdateCallback(this.ShowDescription);
            }

            private void ShowDescription(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 2)
                {
                    throw new System.Exception("We should be in phase 2, but we are not.");
                }

                // It's phase 2, so show this item's description.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[2] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Description;
                textBlock.Opacity = 1;
            }
        }
    }
    ```

4.  Si vous exécutez l’application maintenant et que vous parcourez rapidement l’affichage grille à l’aide de panoramiques/défilements, vous observerez le même comportement que pour **x:Phase**.

## <a name="container-recycling-with-heterogeneous-collections"></a>Recyclage de conteneurs avec des collections hétérogènes

Dans certaines applications, vous devez avoir différentes interfaces utilisateur pour différents types d’élément au sein d’une collection. Cela peut créer une situation dans laquelle il est impossible pour les volets de virtualisation de réutiliser/recycler les éléments visuels utilisés pour afficher les éléments. La recréation des éléments visuels d’un élément pendant le mouvement panoramique annule bon nombre des gains de performances offerts par la virtualisation. Cependant, un minimum de planification peut permettre aux volets de virtualisation de réutiliser les éléments. Les développeurs ont deux options en fonction de leur scénario: l’événement [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) ou un sélecteur de modèles d’éléments. L’approche **ChoosingItemContainer** offre de meilleures performances.

**Événement ChoosingItemContainer**

[**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) est un événement qui vous permet de fournir un élément (**ListViewItem**/**GridViewItem**) à la [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)/[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) chaque fois qu’un nouvel élément est nécessaire au cours de démarrage ou du recyclage. Vous pouvez créer un conteneur basé sur le type d’élément de données affiché par le conteneur (illustré dans l’exemple ci-dessous). **ChoosingItemContainer** est la méthode la plus performante pour utiliser différents modèles de données pour différents éléments. La mise en cache du conteneur peut être obtenue avec **ChoosingItemContainer**. Par exemple, si vous avez cinq modèles différents, avec un modèle présentant un ordre de grandeur plus fréquent que les autres, alors ChoosingItemContainer permet non seulement de créer des éléments dans les rapports nécessaires, mais également de conserver un nombre approprié d’éléments mis en cache et disponibles pour le recyclage. [**ChoosingGroupHeaderContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer) fournit les mêmes fonctionnalités pour les en-têtes de groupe.

```csharp
// Example shows how to use ChoosingItemContainer to return the correct
// DataTemplate when one is available. This example shows how to return different 
// data templates based on the type of FileItem. Available ListViewItems are kept
// in two separate lists based on the type of DataTemplate needed.
private void ListView_ChoosingItemContainer
    (ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    // Determines type of FileItem from the item passed in.
    bool special = args.Item is DifferentFileItem;

    // Uses the Tag property to keep track of whether a particular ListViewItem's 
    // datatemplate should be a simple or a special one.
    string tag = special ? "specialFiles" : "simpleFiles";

    // Based on the type of datatemplate needed return the correct list of 
    // ListViewItems, this could have also been handled with a hash table. These 
    // two lists are being used to keep track of ItemContainers that can be reused.
    List<UIElement> relevantStorage = special ? specialFileItemTrees : simpleFileItemTrees;

    // args.ItemContainer is used to indicate whether the ListView is proposing an 
    // ItemContainer (ListViewItem) to use. If args.Itemcontainer, then there was a 
    // recycled ItemContainer available to be reused.
    if (args.ItemContainer != null)
    {
        // The Tag is being used to determine whether this is a special file or 
        // a simple file.
        if (args.ItemContainer.Tag.Equals(tag))
        {
            // Great: the system suggested a container that is actually going to 
            // work well.
        }
        else
        {
            // the ItemContainer's datatemplate does not match the needed 
            // datatemplate.
            args.ItemContainer = null;
        }
    }

    if (args.ItemContainer == null)
    {
        // see if we can fetch from the correct list.
        if (relevantStorage.Count > 0)
        {
            args.ItemContainer = relevantStorage[0] as SelectorItem;
        }
        else
        {
            // there aren't any (recycled) ItemContainers available. So a new one 
            // needs to be created.
            ListViewItem item = new ListViewItem();
            item.ContentTemplate = this.Resources[tag] as DataTemplate;
            item.Tag = tag;
            args.ItemContainer = item;
        }
    }
}
```

**Sélecteur de modèles d’éléments**

Un sélecteur de modèles d’éléments ([**DataTemplateSelector**](https://msdn.microsoft.com/library/windows/apps/BR209469)) permet à une application de renvoyer un modèle d’élément différent lors de l’exécution en fonction du type de l’élément de données qui sera affiché. Cela rend le développement plus productif, mais la virtualisation de l’interface utilisateur plus difficile, car chaque modèle d’élément ne peut pas être réutilisé pour chaque élément de données.

Lors du recyclage d’un élément (**ListViewItem**/**GridViewItem**), l’infrastructure doit décider si les éléments qui sont disponibles pour utilisation dans la file d’attente de recyclage (la file d’attente de recyclage est un cache d’éléments qui ne sont pas utilisés pour afficher des données) disposent d’un modèle d’élément qui correspond à celui souhaité par l’élément de données en cours. S’il n’y a aucun élément dans la file d’attente de recyclage avec le modèle d’élément approprié, alors un nouvel élément est créé, et le modèle d’élément approprié est instancié pour celui-ci. Si, d’autre part, la file d’attente de recyclage contient un élément avec le modèle d’élément approprié, alors cet élément est supprimé de la file d’attente de recyclage et est utilisé pour l’élément de données actuel. Un sélecteur de modèles d’éléments est adapté lorsque seul un petit nombre de modèles d’éléments est utilisé et qu’il existe une répartition homogène dans la collection d’éléments utilisant différents modèles d’éléments.

Lorsque la répartition des éléments utilisant différents modèles d’éléments est inégale, alors les modèles d’éléments devront probablement être créés lors du mouvement panoramique, annulant ainsi nombre des avantages offerts par la virtualisation. En outre, un sélecteur de modèles d’éléments ne considère que cinq candidats possibles lorsqu’il s’agit d’évaluer si un conteneur donné peut être réutilisé pour l’élément de données actuel. Par conséquent, il convient d’examiner attentivement vos données pour déterminer si elles sont appropriées pour une utilisation avec un sélecteur de modèles d’éléments avant d’en utiliser un dans votre application. Si votre collection est en grande partie homogène, le sélecteur renvoie le même type la plupart du temps (voire tout le temps). Soyez conscient du prix que vous payez pour les exceptions à cette homogénéité et demandez-vous s’il est préférable d’utiliser [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) (ou deux contrôles d’éléments).

 

