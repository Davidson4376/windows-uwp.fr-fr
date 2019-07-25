---
Description: Utilisez le contrôle « Tirer pour actualiser » afin d’importer le contenu nouveau dans une liste.
title: Tirer pour actualiser
label: Pull-to-refresh
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8fdde696a0bc1dc7706f89ede5d525194e5d2830
ms.sourcegitcommit: f0e539359b9766db0339ddbae3f7ccf0069011e8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67885661"
---
# <a name="pull-to-refresh"></a>Tirer pour actualiser

La commande Tirer pour actualiser permet à l’utilisateur de dérouler une liste de données à l’aide de la fonction tactile, afin de récupérer des données supplémentaires. Cette commande est largement utilisée sur les appareils dotés d’un écran tactile. Vous pouvez utiliser les API indiquées ici afin d’implémenter le modèle Tirer pour actualiser dans votre application.

> **API importantes** : [RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer), [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![Gif Tirer pour actualiser](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez la commande Tirer pour actualiser lorsque vous avez une liste ou une grille de données que l’utilisateur est susceptible de vouloir actualiser régulièrement et si votre application est susceptible de s’exécuter sur des appareils à écran tactile.

Vous pouvez également utiliser [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer) pour créer une expérience cohérente d’actualisation appelée d’autres manières, par exemple par un bouton Actualiser.

## <a name="refresh-controls"></a>Contrôles d’actualisation

La commande Tirer pour actualiser est activée par deux contrôles.

- **RefreshContainer** - ContentControl qui fournit un wrapper pour l’expérience Tirer pour actualiser. Il gère les interactions tactiles et l’état de son visualiseur d’actualisation interne.
- **RefreshVisualizer** - Encapsule la visualisation d’actualisation expliquée dans la section suivante.

Le contrôle principal est **RefreshContainer**, que vous placez comme wrapper autour du contenu que l’utilisateur tire (pull) pour déclencher une actualisation. RefreshContainer fonctionne uniquement avec une interaction tactile. Nous vous recommandons donc de mettre également un bouton Actualiser à disposition des utilisateurs qui n’ont pas d’interface tactile. Vous pouvez placer le bouton Actualiser dans un emplacement approprié de l’application, soit dans une barre de commandes, soit dans un emplacement proche de la zone en cours d’actualisation.

## <a name="refresh-visualization"></a>Visualisation d’actualisation

La visualisation d’actualisation par défaut est un compteur de progression circulaire utilisé pour indiquer quand une actualisation se produit, ainsi que sa progression une fois qu’elle est lancée. Le visualiseur d’actualisation peut avoir 5 états.

 La distance dont l’utilisateur a besoin pour tirer (pull) une liste vers le bas en vue de lancer une actualisation s’appelle le _seuil_. L’[état](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State) du visualiseur est déterminé par l’état du tirage (pull) par rapport à ce seuil. Les valeurs possibles sont contenues dans l’énumération [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate).

### <a name="idle"></a>Idle

L’état par défaut du visualiseur est **Inactif**. L’utilisateur n’interagit pas avec le RefreshContainer via l’interaction tactile, et aucune actualisation n’est en cours.

Visuellement, rien n’indique la présence d’un visualiseur d’actualisation.

### <a name="interacting"></a>Interaction

Lorsque l’utilisateur tire la liste dans la direction spécifiée par la propriété PullDirection, et avant que le seuil ne soit atteint, le visualiseur se trouve dans l’état **Interaction**.

- Si l’utilisateur relâche le contrôle dans cet état, le contrôle retourne à l’état **Inactif**.

    ![Seuil préalable de Tirer pour actualiser](images/ptr-prethreshold.png)

    Visuellement, l’icône apparaît désactivée (opacité de 60 %). En outre, l’icône effectue une rotation complète avec l’action de défilement.

- Si l’utilisateur tire (pull) la liste au-delà du seuil, le visualiseur passe de l’état **Interaction** à l’état **En attente**.

    ![Tirer pour actualiser au niveau du seuil](images/ptr-atthreshold.png)

    Visuellement, l’icône passe à 100 % d’opacité et augmente en taille par impulsions jusqu’à 150 %, puis revient à une taille de 100 % lors de la transition.

### <a name="pending"></a>Pending

Lorsque l’utilisateur a tiré la liste au-delà du seuil, le visualiseur n’est plus dans l’état **En attente**.

- Si l’utilisateur redéplace la liste au-dessus du seuil sans la relâcher, le visualiseur retourne à l’état **Interaction**.
- Si l’utilisateur relâche la liste, une requête d’actualisation est envoyée, et l’état passe à **Actualisation**.

![Seuil postérieur de Tirer pour actualiser](images/ptr-postthreshold.png)

Visuellement, l’icône est à 100 % en taille et en opacité. Dans cet état, l’icône continue à se déplacer vers le bas avec l’action de défilement, mais elle ne tourne plus.

### <a name="refreshing"></a>Actualisation

Lorsque l’utilisateur relâche le visualiseur au-delà du seuil, le visualiseur est à l’état **Actualisation**.

Lorsque cet état est entré, l’événement **RefreshRequested** est déclenché. Il s’agit du signal pour démarrer l’actualisation du contenu de l’application. Les arguments d’événement ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contiennent un objet [Deferral](/uwp/api/windows.foundation.deferral), auquel vous devez prendre un handle dans le gestionnaire d’événements. Ensuite, vous devez marquer le report comme terminé une fois que l’exécution de votre code d’actualisation est terminée.

Lorsque l’actualisation est terminée, le visualiseur retourne à l’état **Inactif**.

Visuellement, l’icône revient se fixer à l’emplacement du seuil et tourne pendant la durée de l’actualisation. Cette rotation est utilisée pour montrer la progression de l’actualisation, et est remplacée par l’animation du contenu entrant.

### <a name="peeking"></a>Aperçu

Lorsque l’utilisateur tire (pull) dans la direction de l’actualisation à partir d’une position de départ où une actualisation n’est pas autorisée, le visualiseur passe à l’état **Aperçu**. Cela se produit généralement lorsque le ScrollViewer n’est pas en position 0 quand l’utilisateur commence à tirer.

- Si l’utilisateur relâche le contrôle dans cet état, le contrôle retourne à l’état **Inactif**.

## <a name="pull-direction"></a>Sens du tirage

Par défaut, l’utilisateur tire (pull) une liste de haut en bas pour lancer une actualisation. Si vous disposez d’une liste ou d’une grille avec une orientation différente, vous devez modifier le sens du tirage du conteneur d’actualisation pour les faire correspondre.

La propriété [PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) prend l’une des valeurs [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection) suivantes : **BottomToTop**, **TopToBottom**, **RightToLeft** ou **LeftToRight**.

Lorsque vous modifiez le sens du tirage, la position de départ du compteur de progression du visualiseur tourne automatiquement de sorte que la flèche commence à la position appropriée pour le sens du tirage. Si nécessaire, vous pouvez modifier la propriété [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation) pour remplacer le comportement automatique. Dans la plupart des cas, nous recommandons de laisser la valeur par défaut, c’est-à-dire, **Automatique**.

## <a name="implement-pull-to-refresh"></a>Implémenter Tirer pour actualiser

Seules quelques étapes sont nécessaires pour ajouter la fonctionnalité Tirer pour actualiser à une liste.

1. Wrappez votre liste dans un contrôle **RefreshContainer**.
1. Gérez l’événement **RefreshRequested** pour actualiser votre contenu.
1. Si vous le souhaitez, lancez une actualisation en appelant **RequestRefresh** (par exemple, à partir d’un clic sur un bouton).

> [!NOTE]
> Vous pouvez instancier un RefreshVisualizer seul. Toutefois, nous vous recommandons de wrapper votre contenu dans un RefreshContainer et d’utiliser le RefreshVisualizer fourni par la propriété RefreshContainer.Visualizer, même pour des scénarios non tactiles. Dans cet article, nous partons du principe que le visualiseur s’obtient toujours à partir du conteneur d’actualisation.

> En outre, pour des raisons pratiques, utilisez les membres RequestRefresh et RefreshRequested du conteneur d’actualisation. `refreshContainer.RequestRefresh()` équivaut à `refreshContainer.Visualizer.RequestRefresh()` et déclenche à la fois l’événement RefreshContainer.RefreshRequested et les événements RefreshVisualizer.RefreshRequested.

### <a name="request-a-refresh"></a>Demander une actualisation

Le conteneur d’actualisation gère les interactions tactiles pour permettre à un utilisateur d’actualiser le contenu via l’interaction tactile. Nous vous recommandons de fournir d’autres affordances pour les interfaces non tactiles, comme un bouton d’actualisation ou un contrôle vocal.

Pour lancer une actualisation, appelez la méthode [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh).

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

Lorsque vous appelez RequestRefresh, l’état du visualiseur passe directement de **Inactif** à **Actualisation**.

### <a name="handle-a-refresh-request"></a>Gérer une requête d’actualisation

Pour obtenir un contenu actualisé si nécessaire, gérez l’événement RefreshRequested. Dans le gestionnaire d’événement, vous aurez besoin d’un code spécifique à votre application pour obtenir le contenu actualisé.

Les arguments d’événement ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contiennent un objet [Deferral](/uwp/api/windows.foundation.deferral). Obtenez un handle pour le report dans le gestionnaire d’événements. Ensuite, une fois que l’exécution du code permettant d’effectuer l’actualisation est terminée, marquez le report comme terminé.

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

### <a name="respond-to-state-changes"></a>Répondre aux changements d’état

Vous pouvez répondre aux changements d’état du visualiseur, si nécessaire. Par exemple, pour éviter plusieurs requêtes d’actualisation, vous pouvez désactiver le bouton d’actualisation pendant l’actualisation du visualiseur.

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
> [!NOTE]
> Le contenu d’un RefreshContainer doit être un contrôle avec défilement, par exemple ScrollViewer, GridView, ListView, etc. Définir le contenu sur un contrôle de type Grille provoquera un comportement non défini.

Cet exemple montre comment utiliser la commande Tirer pour actualiser avec une visionneuse à défilement.

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>Ajout de la commande Tirer pour actualiser à un affichage Liste

Cet exemple montre comment utiliser la commande Tirer pour actualiser avec un affichage Liste.

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

## <a name="related-articles"></a>Articles connexes

- [Interactions tactiles](../input/touch-interactions.md)
- [Vue Liste et vue Grille](listview-and-gridview.md)
- [Modèles et conteneurs d’éléments](item-containers-templates.md)
- [Animations par expressions](../../composition/composition-animation.md)
