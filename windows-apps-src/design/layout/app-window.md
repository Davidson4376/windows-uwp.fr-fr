---
Description: Utilisez la classe AppWindow pour afficher les différentes parties de votre application dans des fenêtres distinctes.
title: Utiliser la classe AppWindow pour afficher les fenêtres secondaires pour une application
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f9b247a4a8eb69fa390a9e250e0f88deff9311bf
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68730517"
---
# <a name="show-multiple-views-with-appwindow"></a>Afficher plusieurs vues avec AppWindow

[AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) et ses API associées simplifient la création d’applications à plusieurs fenêtres en vous permettant d’afficher le contenu de votre application dans des fenêtres secondaires tout en continuant à travailler sur le même thread d’interface utilisateur dans chaque fenêtre.

> [!NOTE]
> AppWindow est actuellement en version préliminaire. Cela signifie que vous pouvez envoyer des applications qui utilisent AppWindow vers le Windows Store, mais certains composants de la plateforme et de l’infrastructure sont connus pour ne pas fonctionner avec AppWindow (voir [limitations](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).

Ici, nous montrons certains scénarios pour plusieurs fenêtres avec un exemple d' `HelloAppWindow`application appelé. L’exemple d’application illustre les fonctionnalités suivantes:

- Annulez l’ancrage d’un contrôle de la page principale et ouvrez-le dans une nouvelle fenêtre.
- Ouvrir de nouvelles instances d’une page dans de nouvelles fenêtres.
- Dimensionnez et placez par programme les nouvelles fenêtres dans l’application.
- Associez un ContentDialog à la fenêtre appropriée dans l’application.

![Exemple d’application avec une seule fenêtre](images/hello-app-window-single.png)
  
> _Exemple d’application avec une seule fenêtre_

![Exemple d’application avec un sélecteur de couleurs non ancré et une fenêtre secondaire](images/hello-app-window-multi.png)

> _Exemple d’application avec un sélecteur de couleurs non ancré et une fenêtre secondaire_

> **API importantes** : [Espace de noms Windows. UI. windowmanagement](/uwp/api/windows.ui.windowmanagement), [classe AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

## <a name="api-overview"></a>Vue d’ensemble des API

La classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) et les autres API de l’espace de noms [windowmanagement](/uwp/api/windows.ui.windowmanagement) sont disponibles à partir de Windows 10, version 1903 (SDK 18362). Si votre application cible des versions antérieures de Windows 10, vous devez [utiliser ApplicationView pour créer des fenêtres secondaires](application-view.md). Les API WindowManagement sont toujours en cours de développement et présentent des [limitations](/uwp/api/windows.ui.windowmanagement.appwindow#limitations) , comme décrit dans les documents de référence sur les API.

Voici quelques-unes des API importantes que vous utilisez pour afficher le contenu dans un AppWindow.

### <a name="appwindow"></a>AppWindow

La classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) peut être utilisée pour afficher une partie d’une application Windows Runtime dans une fenêtre secondaire. Ce concept est similaire à un [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview), mais pas au même comportement et à la durée de vie. L’une des principales fonctionnalités de AppWindow est que chaque instance partage le même thread de traitement de l’interface utilisateur (y compris le répartiteur d’événements) à partir duquel elle a été créée, ce qui simplifie les applications à plusieurs fenêtres.

Vous pouvez uniquement connecter le contenu XAML à votre AppWindow. il n’existe aucune prise en charge du contenu natif DirectX ou holographique. Toutefois, vous pouvez afficher un [SWAPCHAINPANEL](/uwp/api/windows.ui.xaml.controls.swapchainpanel) XAML qui héberge du contenu DirectX.

### <a name="windowingenvironment"></a>WindowingEnvironment

L’API [WindowingEnvironment](/uwp/api/windows.ui.windowmanagement.windowingenvironment) vous permet de connaître l’environnement dans lequel votre application est présentée afin que vous puissiez adapter votre application en fonction des besoins. Il décrit le type de fenêtre pris en charge par l’environnement; par exemple, `Overlapped` si l’application s’exécute sur un PC, ou `Tiled` si l’application s’exécute sur une Xbox. Il fournit également un ensemble d’objets DisplayRegion qui décrivent les zones dans lesquelles une application peut être affichée sur un affichage logique.

### <a name="displayregion"></a>DisplayRegion

L’API [DisplayRegion](/uwp/api/windows.ui.windowmanagement.displayregion) décrit la région dans laquelle une vue peut être affichée à un utilisateur sur un affichage logique. par exemple, sur un PC de bureau, il s’agit de l’affichage complet moins la zone de la barre des tâches. Il ne s’agit pas nécessairement d’un mappage de 1:1 avec la zone d’affichage physique du moniteur de stockage. Il peut y avoir plusieurs régions d’affichage dans le même moniteur, ou un DisplayRegion peut être configuré pour s’étendre sur plusieurs analyses si ces moniteurs sont homogènes dans tous les aspects.

### <a name="appwindowpresenter"></a>AppWindowPresenter

L’API [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) vous permet de basculer facilement des fenêtres dans des configurations prédéfinies comme `FullScreen` ou. `CompactOverlay` Ces configurations offrent à l’utilisateur une expérience cohérente sur tous les appareils qui prennent en charge la configuration.

### <a name="uicontext"></a>UIContext

[UIContext](/uwp/api/windows.ui.uicontext) est un identificateur unique pour une fenêtre ou une vue d’application. Il est créé automatiquement et vous pouvez utiliser la propriété [UIElement. UIContext](/uwp/api/windows.ui.xaml.uielement.uicontext) pour récupérer UIContext. Chaque UIElement de l’arborescence XAML a le même UIContext.

 UIContext est important, car les API telles que [Window. Current](/uwp/api/Windows.UI.Xaml.Window.Current) et le modèle s’appuient sur l' `GetForCurrentView` utilisation d’un ApplicationView/CoreWindow unique avec une seule arborescence XAML par thread à utiliser. Ce n’est pas le cas lorsque vous utilisez un AppWindow, donc vous utilisez UIContext pour identifier une fenêtre particulière à la place.

### <a name="xamlroot"></a>XamlRoot

La classe [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) contient une arborescence d’éléments XAML, la connecte à l’objet hôte de fenêtre (par exemple, [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) ou [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)) et fournit des informations telles que la taille et la visibilité. Vous ne créez pas d’objet XamlRoot directement. Au lieu de cela, un élément est créé lorsque vous attachez un élément XAML à un AppWindow. Vous pouvez ensuite utiliser la propriété [UIElement. XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) pour récupérer XamlRoot.

Pour plus d’informations sur UIContext et XamlRoot, consultez [rendre le code portable sur les hôtes de fenêtrage](show-multiple-views.md#make-code-portable-across-windowing-hosts).

## <a name="show-a-new-window"></a>Afficher une nouvelle fenêtre

Examinons les étapes permettant d’afficher le contenu dans un nouveau AppWindow.

**Pour afficher une nouvelle fenêtre**

1. Appelez la méthode statique [AppWindow. TryCreateAsync](/uwp/api/windows.ui.windowmanagement.appwindow.trycreateasync) pour créer un nouveau [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow).

    ```csharp
    AppWindow appWindow = await AppWindow.TryCreateAsync();
    ```

1. Créez le contenu de la fenêtre.

    En général, vous créez un [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame)XAML, puis vous parcourez le frame jusqu’à une [page](/uwp/api/Windows.UI.Xaml.Controls.Page) XAML dans laquelle vous avez défini le contenu de votre application. Pour plus d’informations sur les frames et les pages, consultez [navigation d’égal à égal entre deux pages](../basics/navigate-between-two-pages.md).

    ```csharp
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowMainPage));
    ```

    Toutefois, vous pouvez afficher tout contenu XAML dans le AppWindow, pas seulement un frame et une page. Par exemple, vous pouvez afficher un seul contrôle, comme [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker), ou vous pouvez afficher un [SwapChainPanel](/uwp/api/windows.ui.xaml.controls.swapchainpanel) qui héberge du contenu DirectX.

1. Appelez la méthode [ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent) pour attacher le contenu XAML au AppWindow.

    ```csharp
    ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);
    ```

    L’appel à cette méthode crée un objet [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) et le définit en tant que propriété [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) pour le UIElement spécifié.

    Vous ne pouvez appeler cette méthode qu’une seule fois par instance AppWindow. Une fois que le contenu a été défini, les appels supplémentaires à SetAppWindowContent pour cette instance AppWindow échouent. En outre, si vous tentez de déconnecter le contenu AppWindow en passant un objet UIElement null, l’appel échoue.

1. Appelez la méthode [AppWindow. TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) pour afficher la nouvelle fenêtre.

    ```csharp
    await appWindow.TryShowAsync();
    ```

## <a name="release-resources-when-a-window-is-closed"></a>Libérer des ressources lors de la fermeture d’une fenêtre

Vous devez toujours gérer l’événement [AppWindow. Closed](/uwp/api/windows.ui.windowmanagement.appwindow.closed) pour libérer des ressources XAML (contenu AppWindow) et des références à AppWindow.

```csharp
appWindow.Closed += delegate
{
    appWindowContentFrame.Content = null;
    appWindow = null;
};
```

## <a name="track-instances-of-appwindow"></a>Suivi des instances de AppWindow

Selon la façon dont vous utilisez plusieurs fenêtres dans votre application, vous pouvez être amené à effectuer le suivi des instances de AppWindow que vous créez. L' `HelloAppWindow` exemple illustre différentes façons d’utiliser généralement un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow). Ici, nous allons examiner la raison pour laquelle ces fenêtres doivent être suivies et comment procéder.

### <a name="simple-tracking"></a>Suivi simple

La fenêtre du sélecteur de couleurs héberge un seul contrôle XAML, et le code permettant d’interagir avec le sélecteur de couleurs se trouve dans `MainPage.xaml.cs` le fichier. La fenêtre du sélecteur de couleurs n’autorise qu’une seule instance et est essentiellement `MainWindow`une extension de. Pour garantir qu’une seule instance est créée, la fenêtre du sélecteur de couleurs est suivie avec une variable au niveau de la page. Avant de créer une nouvelle fenêtre de sélecteur de couleurs, vérifiez si une instance existe et, si tel est le cas, ignorez les étapes pour créer une nouvelle fenêtre et appelez simplement [TryShowAsync](/uwp/api/windows.ui.windowmanagement.appwindow.tryshowasync) sur la fenêtre existante.

```csharp
AppWindow colorPickerAppWindow;

// ...

private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        // ...
        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        // ...
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

### <a name="track-an-appwindow-instance-in-its-hosted-content"></a>Suivre une instance AppWindow dans son contenu hébergé

La `AppWindowPage` fenêtre héberge une page XAML complète et le code pour interagir avec la page réside dans `AppWindowPage.xaml.cs`. Elle autorise plusieurs instances, chacune d’entre elles qui fonctionne indépendamment.

Les fonctionnalités de la page vous permettent de manipuler la fenêtre, de la `FullScreen` définir `CompactOverlay`sur ou, et d’écouter également les événements [AppWindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) pour afficher des informations sur la fenêtre. Pour appeler ces API, `AppWindowPage` requiert une référence à l’instance AppWindow qui l’héberge.

Si c’est tout ce dont vous avez besoin, vous pouvez créer une `AppWindowPage` propriété dans et lui affecter l’instance [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) quand vous la créez.

**AppWindowPage.xaml.cs**

Dans `AppWindowPage`, créez une propriété pour contenir la référence AppWindow.

```csharp
public sealed partial class AppWindowPage : Page
{
    public AppWindow MyAppWindow { get; set; }

    // ...
}
```

**MainPage.xaml.cs**

Dans `MainPage`, vous pouvez obtenir une référence à l’instance de page et assigner le AppWindow nouvellement `AppWindowPage`créé à la propriété dans.

```csharp
private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
{
    // Create a new window.
    AppWindow appWindow = await AppWindow.TryCreateAsync();

    // Create a Frame and navigate to the Page you want to show in the new window.
    Frame appWindowContentFrame = new Frame();
    appWindowContentFrame.Navigate(typeof(AppWindowPage));

    // Get a reference to the page instance and assign the
    // newly created AppWindow to the MyAppWindow property.
    AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
    page.MyAppWindow = appWindow;

    // ...
}
```

### <a name="tracking-app-windows-using-uicontext"></a>Suivi des fenêtres d’application à l’aide de UIContext

Vous pouvez également avoir besoin d’accéder aux instances [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) à partir d’autres parties de votre application. Par exemple, `MainPage` peut avoir un bouton’fermer tout’qui ferme toutes les instances suivies de AppWindow.

Dans ce cas, vous devez utiliser l’identificateur unique [UIContext](/uwp/api/windows.ui.uicontext) pour effectuer le suivi des instances de fenêtre dans un [dictionnaire](/dotnet/api/system.collections.generic.dictionary-2?view=dotnet-uwp-10.0).

**MainPage.xaml.cs**

Dans `MainPage`, créez le dictionnaire en tant que propriété statique. Ensuite, ajoutez la page au dictionnaire lorsque vous le créez, puis supprimez-la lorsque la page est fermée. Vous pouvez récupérer le UIContext à partir du [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) de`appWindowContentFrame.UIContext`contenu () après avoir appelé [ElementCompositionPreview. SetAppWindowContent](/api/windows.ui.xaml.hosting.elementcompositionpreview.setappwindowcontent).

```csharp
public sealed partial class MainPage : Page
{
    // Track open app windows in a Dictionary.
    public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
        = new Dictionary<UIContext, AppWindow>();

    // ...

    private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
    {
        // Create a new window.
        AppWindow appWindow = await AppWindow.TryCreateAsync();

        // Create a Frame and navigate to the Page you want to show in the new window.
        Frame appWindowContentFrame = new Frame();
        appWindowContentFrame.Navigate(typeof(AppWindowPage));

        // Attach the XAML content to the window.
        ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

        // Add the new page to the Dictionary using the UIContext as the Key.
        AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
        appWindow.Title = "App Window " + AppWindows.Count.ToString();

        // When the window is closed, be sure to release
        // XAML resources and the reference to the window.
        appWindow.Closed += delegate
        {
            MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
            appWindowContentFrame.Content = null;
            appWindow = null;
        };

        // Show the window.
        await appWindow.TryShowAsync();
    }

    private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
    {
        while (AppWindows.Count > 0)
        {
            await AppWindows.Values.First().CloseAsync();
        }
    }
    // ...
}
```

**AppWindowPage.xaml.cs**

Pour utiliser l’instance [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) dans votre `AppWindowPage` code, utilisez la [UIContext](/uwp/api/windows.ui.uicontext) de la page pour la récupérer à partir du dictionnaire `MainPage`statique dans. Vous devez effectuer cette opération dans le gestionnaire d’événements [chargé](/uwp/api/windows.ui.xaml.frameworkelement.loaded) de la page plutôt que dans le constructeur afin que UIContext ne soit pas null. Vous pouvez récupérer le UIContext à partir de la `this.UIContext`page:.

```csharp
public sealed partial class AppWindowPage : Page
{
    AppWindow window;

    // ...
    public AppWindowPage()
    {
        this.InitializeComponent();

        Loaded += AppWindowPage_Loaded;
    }

    private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Get the reference to this AppWindow that was stored when it was created.
        window = MainPage.AppWindows[this.UIContext];

        // Set up event handlers for the window.
        window.Changed += Window_Changed;
    }
    // ...
}
```

> [!NOTE]
> L' `HelloAppWindow` exemple montre les deux façons d’effectuer le suivi `AppWindowPage`de la fenêtre dans, mais vous allez généralement utiliser l’un ou l’autre, pas les deux.

## <a name="request-window-size-and-placement"></a>Taille et positionnement de la fenêtre de requête

La classe [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) possède plusieurs méthodes que vous pouvez utiliser pour contrôler la taille et l’emplacement de la fenêtre. Comme l’impliquent les noms de méthode, le système peut ou non honorer les modifications demandées en fonction des facteurs environnementaux.

Appelez [Request](/uwp/api/windows.ui.windowmanagement.appwindow.requestsize) pour spécifier la taille de fenêtre souhaitée, comme ceci.

```csharp
colorPickerAppWindow.RequestSize(new Size(300, 428));
```

Les méthodes permettant de gérer le placement des fenêtres sont nommées _RequestMove *_ : [RequestMoveAdjacentToCurrentView](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttocurrentview), [RequestMoveAdjacentToWindow](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoveadjacenttowindow), [RequestMoveRelativeToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmoverelativetodisplayregion), [RequestMoveToDisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindow.requestmovetodisplayregion).

Dans cet exemple, ce code déplace la fenêtre en regard de la vue principale à partir de laquelle la fenêtre est générée.

```csharp
colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
```

Pour obtenir des informations sur la taille et le positionnement actuels de la fenêtre, appelez [GetPlacement](/uwp/api/windows.ui.windowmanagement.appwindow.getplacement). Cela retourne un objet [AppWindowPlacement](/uwp/api/windows.ui.windowmanagement.appwindowplacement) qui fournit le [DisplayRegion](/uwp/api/windows.ui.windowmanagement.appwindowplacement.displayregion)actuel, l' [offset](/uwp/api/windows.ui.windowmanagement.appwindowplacement.offset)et la [taille](/uwp/api/windows.ui.windowmanagement.appwindowplacement.size) de la fenêtre.

Par exemple, vous pouvez appeler ce code pour déplacer la fenêtre vers l’angle supérieur droit de l’affichage. Ce code doit être appelé après l’affichage de la fenêtre. Sinon, la taille de la fenêtre retournée par l’appel à GetPlacement sera 0 et le décalage sera incorrect.

```csharp
DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
double displayRegionWidth = displayRegion.WorkAreaSize.Width;
double windowWidth = window.GetPlacement().Size.Width;
int horizontalOffset = (int)(displayRegionWidth - windowWidth);
window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
```

## <a name="request-a-presentation-configuration"></a>Demander une configuration de présentation

La classe [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter) vous permet d’afficher un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) à l’aide d’une configuration prédéfinie adaptée à l’appareil sur lequel il est affiché. Vous pouvez utiliser une valeur [AppWindowPresentationConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration) pour placer la fenêtre en `FullScreen` mode `CompactOverlay` ou.

Cet exemple montre comment effectuer les opérations suivantes:

- Utilisez l’événement [AppWindow. Changed](/uwp/api/windows.ui.windowmanagement.appwindow.changed) pour être averti en cas de modification des présentations de fenêtres disponibles.
- Utilisez la propriété [AppWindow.](/uwp/api/windows.ui.windowmanagement.appwindow.presenter) Presenter pour accéder au [AppWindowPresenter](/uwp/api/windows.ui.windowmanagement.appwindowpresenter)actuel.
- Appelez [IsPresentationSupported](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.ispresentationsupported) pour voir si un [AppWindowPresentationKind](/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind) spécifique est pris en charge.
- Appelez [GetConfiguration](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.getconfiguration) pour vérifier le type de configuration actuellement utilisé.
- Appelez [RequestPresentation](/uwp/api/windows.ui.windowmanagement.appwindowpresenter.requestpresentation) pour modifier la configuration actuelle.

```csharp
private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
{
    if (args.DidAvailableWindowPresentationsChange)
    {
        EnablePresentationButtons(sender);
    }

    if (args.DidWindowPresentationChange)
    {
        ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
    }

    if (args.DidSizeChange)
    {
        SizeText.Text = window.GetPlacement().Size.ToString();
    }
}

private void EnablePresentationButtons(AppWindow window)
{
    // Check whether the current AppWindowPresenter supports CompactOverlay.
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
    {
        // Show the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the CompactOverlay button...
        compactOverlayButton.Visibility = Visibility.Collapsed;
    }

    // Check whether the current AppWindowPresenter supports FullScreen?
    if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
    {
        // Show the FullScreen button...
        fullScreenButton.Visibility = Visibility.Visible;
    }
    else
    {
        // Hide the FullScreen button...
        fullScreenButton.Visibility = Visibility.Collapsed;
    }
}

private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
        fullScreenButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}

private void FullScreenButton_Click(object sender, RoutedEventArgs e)
{
    if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
        compactOverlayButton.IsChecked = false;
    }
    else
    {
        window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
    }
}
```

## <a name="reuse-xaml-elements"></a>Réutiliser des éléments XAML

Un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) vous permet d’avoir plusieurs arborescences XAML avec le même thread d’interface utilisateur. Toutefois, un élément XAML ne peut être ajouté qu’une seule fois à une arborescence XAML. Si vous souhaitez déplacer une partie de votre interface utilisateur d’une fenêtre à une autre, vous devez gérer son emplacement dans l’arborescence XAML.

Cet exemple montre comment réutiliser un contrôle [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) tout en le déplaçant entre la fenêtre principale et une fenêtre secondaire.

Le sélecteur de couleurs est déclaré dans le XAML `MainPage`pour, ce qui le place `MainPage` dans l’arborescence XAML.

```xaml
<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
    <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
</StackPanel>
```

Quand le sélecteur de couleurs est détaché pour être placé dans un nouveau AppWindow, vous devez d’abord le supprimer de l' `MainPage` arborescence XAML en le supprimant de son conteneur parent. Bien que cela ne soit pas obligatoire, cet exemple masque également le conteneur parent.

```csharp
colorPickerContainer.Children.Remove(colorPicker);
colorPickerContainer.Visibility = Visibility.Collapsed;
```

Vous pouvez ensuite l’ajouter à la nouvelle arborescence XAML. Ici, vous créez d’abord une [grille](/uwp/api/windows.ui.xaml.controls.grid) qui sera le conteneur parent du composant ColorPicker, puis vous ajoutez le composant ColorPicker en tant qu’enfant de la grille. (Cela vous permet de supprimer facilement le composant ColorPicker de cette arborescence XAML ultérieurement.) Vous définissez ensuite la grille en tant que racine de l’arborescence XAML dans la nouvelle fenêtre.

```csharp
Grid appWindowRootGrid = new Grid();
appWindowRootGrid.Children.Add(colorPicker);

// Create a new window
colorPickerAppWindow = await AppWindow.TryCreateAsync();

// Attach the XAML content to our window
ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);
```

Lorsque le [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow) est fermé, vous inversez le processus. Tout d’abord, supprimez le [composant ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) de la [grille](/uwp/api/windows.ui.xaml.controls.grid), puis ajoutez-le en tant `MainPage`qu’enfant de l' [StackPanel](/uwp/api/windows.ui.xaml.controls.stackpanel) dans.

```csharp
// When the window is closed, be sure to release XAML resources
// and the reference to the window.
colorPickerAppWindow.Closed += delegate
{
    appWindowRootGrid.Children.Remove(colorPicker);
    appWindowRootGrid = null;
    colorPickerAppWindow = null;

    colorPickerContainer.Children.Add(colorPicker);
    colorPickerContainer.Visibility = Visibility.Visible;
};
```

```csharp
private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
{
    ColorPickerContainer.Visibility = Visibility.Collapsed;

    // Create the color picker window.
    if (colorPickerAppWindow == null)
    {
        ColorPickerContainer.Children.Remove(colorPicker);

        Grid appWindowRootGrid = new Grid();
        appWindowRootGrid.Children.Add(colorPicker);

        // Create a new window
        colorPickerAppWindow = await AppWindow.TryCreateAsync();
        colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
        colorPickerAppWindow.RequestSize(new Size(300, 428));
        colorPickerAppWindow.Title = "Color picker";

        // Attach the XAML content to our window
        ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

        // When the window is closed, be sure to release XAML resources
        // and the reference to the window.
        colorPickerAppWindow.Closed += delegate
        {
            appWindowRootGrid.Children.Remove(colorPicker);
            appWindowRootGrid = null;
            colorPickerAppWindow = null;

            ColorPickerContainer.Children.Add(colorPicker);
            ColorPickerContainer.Visibility = Visibility.Visible;
        };
    }
    // Show the window.
    await colorPickerAppWindow.TryShowAsync();
}
```

## <a name="show-a-dialog-box"></a>Afficher une boîte de dialogue

Par défaut, le contenu de boîtes de dialogue s’affiche de manière modale, en fonction de la racine [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview). Quand vous utilisez un [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) à l’intérieur d’un [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow), vous devez définir manuellement le XamlRoot dans la boîte de dialogue à la racine de l’hôte XAML.

Pour ce faire, définissez la propriété [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) de ContentDialog sur le même [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) qu’un élément déjà présent dans le AppWindow. Ici, ce code se trouve à l’intérieur du gestionnaire d’événements [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) d’un bouton, ce qui vous permet d’utiliser l' _expéditeur_ (le bouton sur lequel vous avez cliqué) pour obtenir le XamlRoot.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
{
    simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
}
```

Si vous avez un ou plusieurs AppWindows ouverts en plus de la fenêtre principale (ApplicationView), chaque fenêtre peut tenter d’ouvrir une boîte de dialogue, car la boîte de dialogue modale bloque uniquement la fenêtre dans laquelle elle est enracinée. Toutefois, il ne peut y avoir qu’un seul [ContentDialog](/uwp/api/windows.ui.xaml.controls.contentdialog) ouvert par thread à la fois. Toute tentative d’ouverture de deux ContentDialog lève une exception, même si la tentative d’ouverture se fait dans des AppWindows distinctes.

Pour gérer cela, vous devez au moins ouvrir la boîte de dialogue dans `try/catch` un bloc pour intercepter l’exception en cas d’une autre boîte de dialogue déjà ouverte.

```csharp
try
{
    ContentDialogResult result = await simpleDialog.ShowAsync();
}
catch (Exception)
{
    // The dialog didn't open, probably because another dialog is already open.
}
```

Une autre façon de gérer les boîtes de dialogue consiste à suivre la boîte de dialogue actuellement ouverte et à la fermer avant d’essayer d’ouvrir une nouvelle boîte de dialogue. Ici, vous créez une propriété statique dans `MainPage` appelée `CurrentDialog` à cet effet.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

   // ...
}
```

Ensuite, vérifiez s’il existe une boîte de dialogue actuellement ouverte et, si tel est le cas, appelez la méthode [Hide](/uwp/api/windows.ui.xaml.controls.contentdialog.hide) pour la fermer. Enfin, assignez la nouvelle `CurrentDialog`boîte de dialogue à et essayez de l’afficher.

```csharp
private async void DialogButton_Click(object sender, RoutedEventArgs e)
{
    ContentDialog simpleDialog = new ContentDialog
    {
        Title = "Content dialog",
        Content = "Dialog box for " + window.Title,
        CloseButtonText = "Ok"
    };

    if (MainPage.CurrentDialog != null)
    {
        MainPage.CurrentDialog.Hide();
    }
    MainPage.CurrentDialog = simpleDialog;

    // Use this code to associate the dialog to the appropriate AppWindow by setting
    // the dialog's XamlRoot to the same XamlRoot as an element that is already
    // present in the AppWindow.
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
    {
        simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
    }

    try
    {
        ContentDialogResult result = await simpleDialog.ShowAsync();
    }
    catch (Exception)
    {
        // The dialog didn't open, probably because another dialog is already open.
    }
}
```

S’il n’est pas souhaitable de fermer une boîte de dialogue par programmation, ne l' `CurrentDialog`attribuez pas comme. Ici, `MainPage` affiche une boîte de dialogue importante qui ne doit être fermée que lorsque `Ok`vous cliquez sur. Étant donné qu’elle n’est pas `CurrentDialog`affectée en tant que, aucune tentative n’est faite pour la fermer par programmation.

```csharp
public sealed partial class MainPage : Page
{
    // Track the last opened dialog so you can close it if another dialog tries to open.
    public static ContentDialog CurrentDialog { get; set; } = null;

    // ...
    private async void DialogButton_Click(object sender, RoutedEventArgs e)
    {
        ContentDialog importantDialog = new ContentDialog
        {
            Title = "Important dialog",
            Content = "This dialog can only be dismissed by clicking Ok.",
            CloseButtonText = "Ok"
        };

        if (MainPage.CurrentDialog != null)
        {
            MainPage.CurrentDialog.Hide();
        }
        // Do not track this dialog as the MainPage.CurrentDialog.
        // It should only be closed by clicking the Ok button.
        MainPage.CurrentDialog = null;

        try
        {
            ContentDialogResult result = await importantDialog.ShowAsync();
        }
        catch (Exception)
        {
            // The dialog didn't open, probably because another dialog is already open.
        }
    }
    // ...
}
```

## <a name="complete-code"></a>Code complet

### <a name="mainpagexaml"></a>MainPage.xaml

```xaml
<Page
    x:Class="HelloAppWindow.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button x:Name="NewWindowButton" Content="Open new window" 
                    Click="ShowNewWindowButton_Click" Margin="0,12"/>
            <Button Content="Open dialog" Click="DialogButton_Click" 
                    HorizontalAlignment="Stretch"/>
            <Button Content="Close all" Click="CloseAllButton_Click" 
                    Margin="0,12" HorizontalAlignment="Stretch"/>
        </StackPanel>

<StackPanel x:Name="colorPickerContainer" Grid.Column="1" Background="WhiteSmoke">
    <Button Click="DetachColorPickerButton_Click" HorizontalAlignment="Right">
        <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE2B4;" />
    </Button>
            <ColorPicker x:Name="colorPicker" Margin="12" Width="288"
                 IsColorChannelTextInputVisible="False"
                 ColorChanged="ColorPicker_ColorChanged"/>
        </StackPanel>
    </Grid>
</Page>

```

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Windows.Foundation;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=402352&clcid=0x409

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        AppWindow colorPickerAppWindow;

        // Track open app windows in a Dictionary.
        public static Dictionary<UIContext, AppWindow> AppWindows { get; set; }
            = new Dictionary<UIContext, AppWindow>();

        // Track the last opened dialog so you can close it if another dialog tries to open.
        public static ContentDialog CurrentDialog { get; set; } = null;

        public MainPage()
        {
            this.InitializeComponent();
        }

        private async void ShowNewWindowButton_Click(object sender, RoutedEventArgs e)
        {
            // Create a new window.
            AppWindow appWindow = await AppWindow.TryCreateAsync();

            // Create a Frame and navigate to the Page you want to show in the new window.
            Frame appWindowContentFrame = new Frame();
            appWindowContentFrame.Navigate(typeof(AppWindowPage));

            // Get a reference to the page instance and assign the
            // newly created AppWindow to the MyAppWindow property.
            AppWindowPage page = (AppWindowPage)appWindowContentFrame.Content;
            page.MyAppWindow = appWindow;
            page.TextColorBrush = new SolidColorBrush(colorPicker.Color);

            // Attach the XAML content to the window.
            ElementCompositionPreview.SetAppWindowContent(appWindow, appWindowContentFrame);

            // Add the new page to the Dictionary using the UIContext as the Key.
            AppWindows.Add(appWindowContentFrame.UIContext, appWindow);
            appWindow.Title = "App Window " + AppWindows.Count.ToString();

            // When the window is closed, be sure to release XAML resources
            // and the reference to the window.
            appWindow.Closed += delegate
            {
                MainPage.AppWindows.Remove(appWindowContentFrame.UIContext);
                appWindowContentFrame.Content = null;
                appWindow = null;
            };

            // Show the window.
            await appWindow.TryShowAsync();
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog importantDialog = new ContentDialog
            {
                Title = "Important dialog",
                Content = "This dialog can only be dismissed by clicking Ok.",
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            // Do not track this dialog as the MainPage.CurrentDialog.
            // It should only be closed by clicking the Ok button.
            MainPage.CurrentDialog = null;

            try
            {
                ContentDialogResult result = await importantDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private async void DetachColorPickerButton_Click(object sender, RoutedEventArgs e)
        {
            // Create the color picker window.
            if (colorPickerAppWindow == null)
            {
                colorPickerContainer.Children.Remove(colorPicker);
                colorPickerContainer.Visibility = Visibility.Collapsed;

                Grid appWindowRootGrid = new Grid();
                appWindowRootGrid.Children.Add(colorPicker);

                // Create a new window
                colorPickerAppWindow = await AppWindow.TryCreateAsync();
                colorPickerAppWindow.RequestMoveAdjacentToCurrentView();
                colorPickerAppWindow.RequestSize(new Size(300, 428));
                colorPickerAppWindow.Title = "Color picker";

                // Attach the XAML content to our window
                ElementCompositionPreview.SetAppWindowContent(colorPickerAppWindow, appWindowRootGrid);

                // Make sure to release the reference to this window, 
                // and release XAML resources, when it's closed
                colorPickerAppWindow.Closed += delegate
                {
                    appWindowRootGrid.Children.Remove(colorPicker);
                    appWindowRootGrid = null;
                    colorPickerAppWindow = null;

                    colorPickerContainer.Children.Add(colorPicker);
                    colorPickerContainer.Visibility = Visibility.Visible;
                };
            }
            // Show the window.
            await colorPickerAppWindow.TryShowAsync();
        }

        private void ColorPicker_ColorChanged(ColorPicker sender, ColorChangedEventArgs args)
        {
            NewWindowButton.Background = new SolidColorBrush(args.NewColor);
        }

        private async void CloseAllButton_Click(object sender, RoutedEventArgs e)
        {
            while (AppWindows.Count > 0)
            {
                await AppWindows.Values.First().CloseAsync();
            }
        }
    }
}

```

### <a name="appwindowpagexaml"></a>AppWindowPage. Xaml

```xaml
<Page
    x:Class="HelloAppWindow.AppWindowPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloAppWindow"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid>
        <TextBlock x:Name="TitleTextBlock" Text="Hello AppWindow!" FontSize="24" HorizontalAlignment="Center" Margin="24"/>

        <StackPanel VerticalAlignment="Center" HorizontalAlignment="Center">
            <Button Content="Open dialog" Click="DialogButton_Click"
                    Width="200" Margin="0,4"/>
            <Button Content="Move window" Click="MoveWindowButton_Click"
                    Width="200" Margin="0,4"/>
            <ToggleButton Content="Compact Overlay" x:Name="compactOverlayButton" Click="CompactOverlayButton_Click"
                          Width="200" Margin="0,4"/>
            <ToggleButton Content="Full Screen" x:Name="fullScreenButton" Click="FullScreenButton_Click"
                          Width="200" Margin="0,4"/>
            <Grid>
                <TextBlock Text="Size:"/>
                <TextBlock x:Name="SizeText" HorizontalAlignment="Right"/>
            </Grid>
            <Grid>
                <TextBlock Text="Presentation:"/>
                <TextBlock x:Name="ConfigText" HorizontalAlignment="Right"/>
            </Grid>
        </StackPanel>
    </Grid>
</Page>
```

### <a name="appwindowpagexamlcs"></a>AppWindowPage.xaml.cs

```csharp
using System;
using Windows.Foundation;
using Windows.Foundation.Metadata;
using Windows.UI;
using Windows.UI.WindowManagement;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=234238

namespace HelloAppWindow
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class AppWindowPage : Page
    {
        AppWindow window;

        public AppWindow MyAppWindow { get; set; }

        public SolidColorBrush TextColorBrush { get; set; } = new SolidColorBrush(Colors.Black);

        public AppWindowPage()
        {
            this.InitializeComponent();

            Loaded += AppWindowPage_Loaded;
        }

        private void AppWindowPage_Loaded(object sender, RoutedEventArgs e)
        {
            // Get the reference to this AppWindow that was stored when it was created.
            window = MainPage.AppWindows[this.UIContext];

            // Set up event handlers for the window.
            window.Changed += Window_Changed;

            TitleTextBlock.Foreground = TextColorBrush;
        }

        private async void DialogButton_Click(object sender, RoutedEventArgs e)
        {
            ContentDialog simpleDialog = new ContentDialog
            {
                Title = "Content dialog",
                Content = "Dialog box for " + window.Title,
                CloseButtonText = "Ok"
            };

            if (MainPage.CurrentDialog != null)
            {
                MainPage.CurrentDialog.Hide();
            }
            MainPage.CurrentDialog = simpleDialog;

            // Use this code to associate the dialog to the appropriate AppWindow by setting
            // the dialog's XamlRoot to the same XamlRoot as an element that is already 
            // present in the AppWindow.
            if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 8))
            {
                simpleDialog.XamlRoot = ((Button)sender).XamlRoot;
            }

            try
            {
                ContentDialogResult result = await simpleDialog.ShowAsync();
            }
            catch (Exception)
            {
                // The dialog didn't open, probably because another dialog is already open.
            }
        }

        private void Window_Changed(AppWindow sender, AppWindowChangedEventArgs args)
        {
            if (args.DidAvailableWindowPresentationsChange)
            {
                EnablePresentationButtons(sender);
            }

            if (args.DidWindowPresentationChange)
            {
                ConfigText.Text = window.Presenter.GetConfiguration().Kind.ToString();
            }

            if (args.DidSizeChange)
            {
                SizeText.Text = window.GetPlacement().Size.ToString();
            }
        }

        private void EnablePresentationButtons(AppWindow window)
        {
            // Check whether the current AppWindowPresenter supports CompactOverlay.
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.CompactOverlay))
            {
                // Show the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the CompactOverlay button...
                compactOverlayButton.Visibility = Visibility.Collapsed;
            }

            // Check whether the current AppWindowPresenter supports FullScreen?
            if (window.Presenter.IsPresentationSupported(AppWindowPresentationKind.FullScreen))
            {
                // Show the FullScreen button...
                fullScreenButton.Visibility = Visibility.Visible;
            }
            else
            {
                // Hide the FullScreen button...
                fullScreenButton.Visibility = Visibility.Collapsed;
            }
        }

        private void CompactOverlayButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.CompactOverlay)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.CompactOverlay);
                fullScreenButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void FullScreenButton_Click(object sender, RoutedEventArgs e)
        {
            if (window.Presenter.GetConfiguration().Kind != AppWindowPresentationKind.FullScreen)
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.FullScreen);
                compactOverlayButton.IsChecked = false;
            }
            else
            {
                window.Presenter.RequestPresentation(AppWindowPresentationKind.Default);
            }
        }

        private void MoveWindowButton_Click(object sender, RoutedEventArgs e)
        {
            DisplayRegion displayRegion = window.GetPlacement().DisplayRegion;
            double displayRegionWidth = displayRegion.WorkAreaSize.Width;
            double windowWidth = window.GetPlacement().Size.Width;
            int horizontalOffset = (int)(displayRegionWidth - windowWidth);
            window.RequestMoveRelativeToDisplayRegion(displayRegion, new Point(horizontalOffset, 0));
        }
    }
}
```

## <a name="related-topics"></a>Rubriques connexes

- [Afficher plusieurs vues](show-multiple-views.md)
- [Afficher plusieurs vues avec ApplicationView](application-view.md)
