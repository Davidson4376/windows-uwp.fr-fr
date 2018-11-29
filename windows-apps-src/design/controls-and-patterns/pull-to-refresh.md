---
Description: Use the pull-to-refresh control to get new content into a list.
title: Tirer pour actualiser
label: Pull-to-refresh
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2efd091d90a856e45d76c0b1357f30417812160a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194801"
---
# <a name="pull-to-refresh"></a>Tirer pour actualiser

La commande Tirer pour actualiser permet à l’utilisateur de dérouler une liste de données à l’aide de la fonction tactile afin de récupérer des données supplémentaires. Tirer pour actualiser est largement utilisé sur les appareils avec un écran tactile. Vous pouvez utiliser les API indiquées ici afin d'implémenter le modèle Tirer pour actualiser dans votre application.

> **API importantes**: [RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer), [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![gif tirer pour actualiser](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez Tirer pour actualiser lorsque vous avez une liste ou une grille de données que l’utilisateur est susceptible de vouloir actualiser régulièrement et si votre application est susceptible de s’exécuter sur des appareils tactiles.

Vous pouvez également utiliser le [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer) pour créer une expérience cohérente d'actualisation invoquée d’autres manières, par exemple par un bouton Actualiser.

## <a name="refresh-controls"></a>Actualiser les contrôles

Tirer pour actualiser est activé par 2contrôles.

- **RefreshContainer** - Un ContentControl qui fournit un wrapper pour l’expérience Tirer pour actualiser. Il gère les interactions tactiles et l’état de son visualiseur d'actualisation interne.
- **RefreshVisualizer** - encapsule la visualisation d'actualisation expliquée dans la section suivante.

Le contrôle principal est le **RefreshContainer**, que vous placez comme wrapper autour du contenu que l’utilisateur tire pour déclencher une actualisation. RefreshContainer fonctionne uniquement avec une interaction tactile. Nous vous recommandons donc de mettre également un bouton Actualiser à disposition des utilisateurs qui ne disposent pas d'une interface tactile. Vous pouvez placer le bouton Actualiser dans un emplacement approprié de l’application, soit dans une barre de commandes, soit dans un emplacement proche de la surface en cours d’actualisation.

## <a name="refresh-visualization"></a>Visualisation d'actualisation

La visualisation d’actualisation par défaut est un compteur de progression circulaire utilisé pour indiquer quand une actualisation se produit, ainsi que sa progression une fois qu'elle est lancée. Le visualiseur d'actualisation possède 5états.

 La distance dont l’utilisateur a besoin pour tirer une liste vers le bas pour lancer une actualisation s'appelle le _seuil_. L'[État](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State) du visualiseur est déterminé par l’état de l’extraction par rapport à ce seuil. Les valeurs possibles sont contenues dans l'énumération [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate).

### <a name="idle"></a>Inactif

L’état par défaut du visualiseur est **Inactif**. L’utilisateur n’interagit pas avec le RefreshContainer via l’interaction tactile et aucune actualisation n'est en cours.

Visuellement, rien ne prouve la présence d'un visualiseur d'actualisation.

### <a name="interacting"></a>Interaction

Lorsque l’utilisateur tire la liste dans la direction spécifiée par la propriété PullDirection, et avant que le seuil ne soit atteint, le visualiseur se trouve dans l'état **Interaction**.

- Si l’utilisateur relâche le contrôle dans cet état, le contrôle retourne à l'état **Inactif**.

    ![seuil préalable de tirer pour actualiser](images/ptr-prethreshold.png)

    Visuellement, l’icône apparaît désactivée (opacité de 60%). En outre, l’icône tourne selon une rotation complète avec l’action de défilement.

- Si l’utilisateur extrait la liste au-delà du seuil, le visualiseur passe de **Interaction** à **En attente**.

    ![tirer pour actualiser au niveau du seuil](images/ptr-atthreshold.png)

    Visuellement, l’icône passe à 100% d'opacité et augmente en taille par impulsions jusqu'à 150%, puis revient à une taille de 100% lors de la transition.

### <a name="pending"></a>En attente

Lorsque l’utilisateur a tiré la liste au-delà du seuil, le visualiseur n’est plus dans l'état **En attente**.

- Si l’utilisateur redéplace la liste au-dessus du seuil sans la relâcher, il retourne à l'état **Interaction**.
- Si l’utilisateur relâche la liste, une demande d’actualisation est lancée et elle passe à l'état **Actualisation**.

![seuil postérieur de tirer pour actualiser](images/ptr-postthreshold.png)

Visuellement, l’icône est à 100% en taille et en opacité. Dans cet état, l’icône continue à se déplacer vers le bas avec l’action de défilement, mais ne tourne plus.

### <a name="refreshing"></a>Actualisation

Lorsque l’utilisateur relâche le visualiseur au-delà du seuil, il est dans l'état **Actualisation**.

Lorsque cet état est entré, l'événement **RefreshRequested** est déclenché. Il s’agit du signal pour démarrer l’actualisation du contenu de l’application. Les arguments d’événement ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contiennent un objet [Deferral](/uwp/api/windows.foundation.deferral), auquel vous devez prendre un handle dans le gestionnaire d’événements. Ensuite, vous devez marquer le report comme terminé une fois votre code pour effectuer l’actualisation terminé.

Lorsque l’actualisation est terminée, le visualiseur retourne à l'état **Inactif**.

Visuellement, l’icône revient se fixer à l’emplacement du seuil et tourne pendant la durée de l’actualisation. Cette rotation est utilisée pour montrer la progression de l’actualisation et est remplacée par l’animation du contenu entrant.

### <a name="peeking"></a>Lecture

Lorsque l’utilisateur tire dans la direction de l’actualisation à partir d’une position de départ où une actualisation n’est pas autorisée, le visualiseur entre dans l'état **Lecture**. Cela se produit généralement lorsque le ScrollViewer n’est pas en position 0 lorsque l’utilisateur commence à tirer.

- Si l’utilisateur relâche le contrôle dans cet état, le contrôle retourne à l'état **Inactif**.

## <a name="pull-direction"></a>Sens de la traction

Par défaut, l’utilisateur tire une liste de haut en bas pour lancer une actualisation. Si vous disposez d’une liste ou d'une grille avec une orientation différente, vous devez modifier le sens de traction du conteneur d’actualisation pour les faire correspondre.

La propriété [PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) prend l'une de ces valeurs de [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection): **BottomToTop**, **TopToBottom**, **RightToLeft** ou **LeftToRight**.

Lorsque vous modifiez le sens de traction, la position de départ du compteur de progression du visualiseur tourne automatiquement de sorte que la flèche commence à la position approprié pour le sens de traction. Si nécessaire, vous pouvez modifier la propriété [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation) pour remplacer le comportement automatique. Dans la plupart des cas, nous recommandons de laisser la valeur par défaut **Automatique**.

## <a name="implement-pull-to-refresh"></a>Implémenter Tirer pour actualiser

L'ajout de la fonctionnalité tirer pour actualiser à une liste nécessite seulement quelques étapes.

1. Encapsulez votre liste dans un contrôle **RefreshContainer**.
1. Gérez l'événement **RefreshRequested** pour actualiser votre contenu.
1. Si vous le souhaitez, lancez une actualisation en appelant **RequestRefresh** (par exemple, à partir d’un clic sur un bouton).

> [!NOTE]
> Vous pouvez instancier un RefreshVisualizer seul. Toutefois, nous vous recommandons d'encapsuler votre contenu dans un RefreshContainer et d'utiliser le RefreshVisualizer fourni par la propriété RefreshContainer.Visualizer, même pour des scénarios non tactiles. Dans cet article, nous partons du principe que le visualiseur s'obtient toujours à partir du conteneur d’actualisation.

> En outre, utilisez les membres RequestRefresh et RefreshRequested du conteneur d’actualisation pour des raisons pratiques. `refreshContainer.RequestRefresh()` équivaut à `refreshContainer.Visualizer.RequestRefresh()` et déclenche à la fois l'événement RefreshContainer.RefreshRequested et les événements RefreshVisualizer.RefreshRequested.

### <a name="request-a-refresh"></a>Demander une actualisation

Le conteneur d’actualisation gère les interactions tactiles pour permettre à un utilisateur d’actualiser le contenu via l’interaction tactile. Nous vous recommandons de fournir d'autres affordances pour les interfaces non tactiles, comme un bouton d'actualisation ou un contrôle vocal.

Pour lancer une actualisation, appelez la méthode [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh).

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

Lorsque vous appelez RequestRefresh, l’état du visualiseur passe directement de **Inactif** à **Actualisation**.

### <a name="handle-a-refresh-request"></a>Gérer une demande d'actualisation

Pour obtenir un contenu actualisé si nécessaire, gérez l’événement RefreshRequested. Dans le gestionnaire d'événement, vous aurez besoin d'un code spécifique à votre application pour obtenir le contenu actualisé.

Les arguments d’événement ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contiennent un objet [Deferral](/uwp/api/windows.foundation.deferral). Obtenez un handle pour le report dans le gestionnaire d’événements. Ensuite, marquez le report comme terminé une fois votre code permettant d'effectuer l’actualisation terminé.

```csharp
// See the Examples section for the full code.
private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
{
    // Respond to a request by performing a refresh and using the deferral object.
    using (var RefreshCompletionDeferral = args.GetDeferral())
    {
        // Do some async operation to refresh the content

         await FetchAndInsertItemsAsync(3);

        // The 'using' statement ensures the deferral is marked as complete.
        // Otherwise, you'd call
        // RefreshCompletionDeferral.Complete();
        // RefreshCompletionDeferral.Dispose();
    }
}
```

### <a name="respond-to-state-changes"></a>Répondre aux changements d'état

Vous pouvez répondre aux modifications d’état du visualiseur, si nécessaire. Par exemple, pour éviter plusieurs demandes d’actualisation, vous pouvez désactiver un bouton d’actualisation pendant l’actualisation du visualiseur.

```csharp
// See the Examples section for the full code.
private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
{
    // Respond to visualizer state changes.
    // Disable the refresh button if the visualizer is refreshing.
    if (args.NewState == RefreshVisualizerState.Refreshing)
    {
        RefreshButton.IsEnabled = false;
    }
    else
    {
        RefreshButton.IsEnabled = true;
    }
}
```

## <a name="examples"></a>Exemples

### <a name="using-a-scrollviewer-in-a-refreshcontainer"></a>Utilisation d’un ScrollViewer dans un RefreshContainer

Cet exemple montre comment utiliser tirer pour actualiser avec une visionneuse à défilement.

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>Ajout de tirer pour actualiser à un contrôle ListView

Cet exemple montre comment utiliser tirer pour actualiser avec un affichage Liste.

```xaml
<StackPanel Margin="0,40" Width="280">
    <CommandBar OverflowButtonVisibility="Collapsed">
        <AppBarButton x:Name="RefreshButton" Click="RefreshButtonClick"
                      Icon="Refresh" Label="Refresh"/>
        <CommandBar.Content>
            <TextBlock Text="List of items" 
                       Style="{StaticResource TitleTextBlockStyle}"
                       Margin="12,8"/>
        </CommandBar.Content>
    </CommandBar>

    <RefreshContainer x:Name="RefreshContainer">
        <ListView x:Name="ListView1" Height="400">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <Grid Height="80">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <TextBlock Text="{x:Bind Path=Header}"
                                   Style="{StaticResource SubtitleTextBlockStyle}"
                                   Grid.Row="0"/>
                        <TextBlock Text="{x:Bind Path=Date}"
                                   Style="{StaticResource CaptionTextBlockStyle}"
                                   Grid.Row="1"/>
                        <TextBlock Text="{x:Bind Path=Body}"
                                   Style="{StaticResource BodyTextBlockStyle}"
                                   Grid.Row="2"
                                   Margin="0,4,0,0" />
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RefreshContainer>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<ListItemData> Items { get; set; } 
        = new ObservableCollection<ListItemData>();

    public MainPage()
    {
        this.InitializeComponent();

        Loaded += MainPage_Loaded;
        ListView1.ItemsSource = Items;
    }

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        Loaded -= MainPage_Loaded;
        RefreshContainer.RefreshRequested += RefreshContainer_RefreshRequested;
        RefreshContainer.Visualizer.RefreshStateChanged += Visualizer_RefreshStateChanged;

        // Add some initial content to the list.
        await FetchAndInsertItemsAsync(2);
    }

    private void RefreshButtonClick(object sender, RoutedEventArgs e)
    {
        RefreshContainer.RequestRefresh();
    }

    private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
    {
        // Respond to a request by performing a refresh and using the deferral object.
        using (var RefreshCompletionDeferral = args.GetDeferral())
        {
            // Do some async operation to refresh the content

            await FetchAndInsertItemsAsync(3);

            // The 'using' statement ensures the deferral is marked as complete.
            // Otherwise, you'd call
            // RefreshCompletionDeferral.Complete();
            // RefreshCompletionDeferral.Dispose();
        }
    }

    private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
    {
        // Respond to visualizer state changes.
        // Disable the refresh button if the visualizer is refreshing.
        if (args.NewState == RefreshVisualizerState.Refreshing)
        {
            RefreshButton.IsEnabled = false;
        }
        else
        {
            RefreshButton.IsEnabled = true;
        }
    }

    // App specific code to get fresh data.
    private async Task FetchAndInsertItemsAsync(int updateCount)
    {
        for (int i = 0; i < updateCount; ++i)
        {
            // Simulate delay while we go fetch new items.
            await Task.Delay(1000);
            Items.Insert(0, GetNextItem());
        }
    }

    private ListItemData GetNextItem()
    {
        return new ListItemData()
        {
            Header = "Header " + DateTime.Now.Second.ToString(),
            Date = DateTime.Now.ToLongDateString(),
            Body = DateTime.Now.ToLongTimeString()
        };
    }
}

public class ListItemData
{
    public string Header { get; set; }
    public string Date { get; set; }
    public string Body { get; set; }
}
```

## <a name="related-articles"></a>Articles associés

- [Interactions tactiles](../input/touch-interactions.md)
- [Affichage Liste et affichage Grille](listview-and-gridview.md)
- [Modèles et conteneurs d’éléments](item-containers-templates.md)
- [Animations par expressions](../../composition/composition-animation.md)
