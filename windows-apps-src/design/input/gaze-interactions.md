---
author: Karl-Bridge-Microsoft
ms.author: kbridge
title: Interactions avec le pointage du regard
Description: Learn how to design and optimize your UWP apps to provide the best experience possible for users who rely on gaze input from eye and head trackers.
label: Gaze interactions
template: detail.hbs
keywords: pointer du regard, suivi oculaire, suivi de la tête, endroit pointé par le regard, entrée, interaction utilisateur, accessibilité, facilité d’utilisation
ms.date: 05/01/2018
ms.topic: article
pm-contact: Jake Cohen
dev-contact: Austin Hodges
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 758678a7fb65e32d4d3d411956aadfdab515df43
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7166288"
---
# <a name="gaze-interactions-and-eye-tracking-in-uwp-apps"></a>Interactions avec le pointage du regard et le suivi oculaire dans les applications UWP

![Suivi oculaire Hero](images/gaze/eyecontrolbanner1.png)

Fournissez la prise en charge du suivi du pointage du regard, de l'attention et de la présence de l'utilisateur en fonction de l’emplacement et des mouvements de ses yeux.

> [!NOTE]
> Pour l’entrée par pointage du regard dans [WindowsMixedReality](https://docs.microsoft.com/windows/mixed-reality/), voir [Pointer du regard](https://docs.microsoft.com/windows/mixed-reality/gaze).

**API importantes**: [Windows.Devices.Input.Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview), [GazeDevicePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicepreview), [GazePointPreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview), [GazeInputSourcePreview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview)

## <a name="overview"></a>Présentation

Entrée avec le pointage du regard est un moyen puissant d'interagir et d'utiliser des applications UWP et Windows, particulièrement utile comme technologie d’assistance pour les utilisateurs souffrant de maladies neuro-musculaires (comme la SLA) ou d’autres handicaps impliquant des troubles des fonctions musculaires ou neurologiques.

En outre, l'entrée par le pointage du regard offre également de remarquables opportunités pour les jeux (notamment pour l'acquisition de cible et le suivi), les applications de productivité traditionnelles, les bornes et autres scénarios interactifs dans lesquels les périphériques classiques (clavier, souris entrée tactile) ne sont pas disponibles ou il peut être utile de libérer des mains de l’utilisateur pour d’autres tâches (par exemple, pour tenir des sacs).

> [!NOTE]
> La prise en charge du matériel de suivi oculaire a été introduite dans **Windows10 Fall Creators Update** avec le [contrôle visuel](https://support.microsoft.com/en-us/help/4043921/windows-10-get-started-eye-control), une fonctionnalité intégrée qui vous permet d’utiliser vos yeux pour contrôler le pointeur à l'écran, taper avec le clavier visuel et communiquer avec des personnes à l’aide de la synthèse vocale. Un ensemble d'[API UWP]([Windows.Devices.Input.Preview](https://docs.microsoft.com/uwp/api/windows.devices.input.preview)) pour générer des applications capables d'interagir avec le matériel de suivi oculaire est disponible avec la **Mise à jour d'avril2018 de Windows10 (version1803, build17134)** et les versions ultérieures.

## <a name="privacy"></a>Confidentialité

En raison des données personnelles potentiellement sensibles collectées par les périphériques de suivi oculaire, vous devez déclarer la fonctionnalité `gazeInput` dans le manifeste de votre application UWP (voir la section suivante **Configuration**). Lorsqu'elle est déclarée, Windows invite automatiquement les utilisateurs via une boîte de dialogue de consentement (lors de la première exécution de l’application), dans laquelle l’utilisateur doit donner à l’application l'autorisation de communiquer avec l’appareil de suivi oculaire et d'accéder à ces données.

En outre, si votre application collecte, stocke ou transfère des données de suivi oculaire, vous devez le décrire dans la déclaration de confidentialité de votre application et suivre toutes les autres conditions requises concernant les **informations personnelles** dans le [Contrat du développeur d'application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) et les [Politiques du MicrosoftStore](https://docs.microsoft.com/legal/windows/agreements/store-policies).

## <a name="setup"></a>Configuration

Pour utiliser les API d'entrée avec le pointage du regard dans votre application UWP, vous devez: 

- Spécifiez les fonctionnalités `gazeInput` dans le manifeste de l’application.

    Ouvrez le fichier **Package.appxmanifest** avec le concepteur de manifeste Visual Studio ou ajoutez la fonctionnalité manuellement en sélectionnant **Afficher le code** et en insérant les éléments suivants `DeviceCapability` dans le nœud `Capabilities`:

    ```xaml
    <Capabilities>
       <DeviceCapability Name="gazeInput" />
    </Capabilities>
    ```

- Un appareil de suivi oculaire compatible avec Windows connecté à votre système (intégré ou périphérique) et activé.

    Voir [Prise en main du contrôle visuel dans Windows10](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control#supported-devices ) pour obtenir la liste des périphériques pris en charge de suivi oculaire.

## <a name="basic-eye-tracking"></a>Suivi oculaire de base

Dans cet exemple, nous montrons comment effectuer le suivi du regard de l’utilisateur au sein d’une application UWP et utiliser une fonction de minuteur avec un test de positionnement de base pour indiquer la manière dont il peut maintenir le focus du regard sur un élément spécifique.

Une petite ellipse est utilisée pour déterminer l'endroit où pointe le regard dans la fenêtre d’affichage de l’application, tandis qu’une [RadialProgressBar](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/controls/radialprogressbar) du [WindowsCommunityToolkit](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/) est placée sur le canevas de manière aléatoire. Lorsque le focus du regard est détecté sur la barre de progression, un minuteur se lance et la barre de progression est déplacée au hasard sur le canevas lorsque la barre de progression atteint 100%.

![Exemple de suivi du regard avec un minuteur](images/gaze/gaze-input-timed2.gif)

*Exemple de suivi du regard avec un minuteur*

**Télécharger cet exemple depuis [Exemple d’entrée avec le pointage du regard (de base)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)**

1. Tout d’abord, nous configurons l’interface utilisateur (MainPage.xaml).

    ```xaml
    <Page
        x:Class="gazeinput.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:gazeinput"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"    
        mc:Ignorable="d">
    
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Grid x:Name="containerGrid">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <StackPanel x:Name="HeaderPanel" 
                        Orientation="Horizontal" 
                        Grid.Row="0">
                    <StackPanel.Transitions>
                        <TransitionCollection>
                            <AddDeleteThemeTransition/>
                        </TransitionCollection>
                    </StackPanel.Transitions>
                    <TextBlock x:Name="Header" 
                           Text="Gaze tracking sample" 
                           Style="{ThemeResource HeaderTextBlockStyle}" 
                           Margin="10,0,0,0" />
                    <TextBlock x:Name="TrackerCounterLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="Number of trackers: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerCounter"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="0" 
                           Margin="10,0,0,0"/>
                    <TextBlock x:Name="TrackerStateLabel"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="State: " 
                           Margin="50,0,0,0"/>
                    <TextBlock x:Name="TrackerState"
                           VerticalAlignment="Center"                 
                           Style="{ThemeResource BodyTextBlockStyle}"
                           Text="n/a" 
                           Margin="10,0,0,0"/>
                </StackPanel>
                <Canvas x:Name="gazePositionCanvas" Grid.Row="1">
                    <controls:RadialProgressBar
                        x:Name="GazeRadialProgressBar" 
                        Value="0"
                        Foreground="Blue" 
                        Background="White"
                        Thickness="4"
                        Minimum="0"
                        Maximum="100"
                        Width="100"
                        Height="100"
                        Outline="Gray"
                        Visibility="Collapsed"/>
                    <Ellipse 
                        x:Name="eyeGazePositionEllipse"
                        Width="20" Height="20"
                        Fill="Blue" 
                        Opacity="0.5" 
                        Visibility="Collapsed">
                    </Ellipse>
                </Canvas>
            </Grid>
        </Grid>
    </Page>
    ```

2. Ensuite, nous initialisons notre application.

    Dans cet extrait de code, nous déclarons nos objets globaux et remplaçons l'événement de page [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) pour démarrer notre [Observateur d’appareils de pointage du regard](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview) et l'événement de page [OnNavigatedFrom](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) pour arrêter notre [Observateur d’appareils de pointage du regard](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview).

    ```csharp
    using System;
    using Windows.Devices.Input.Preview;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml;
    using Windows.Foundation;
    using System.Collections.Generic;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;
    
    namespace gazeinput
    {
        public sealed partial class MainPage : Page
        {
            /// <summary>
            /// Reference to the user's eyes and head as detected
            /// by the eye-tracking device.
            /// </summary>
            private GazeInputSourcePreview gazeInputSource;
    
            /// <summary>
            /// Dynamic store of eye-tracking devices.
            /// </summary>
            /// <remarks>
            /// Receives event notifications when a device is added, removed, 
            /// or updated after the initial enumeration.
            /// </remarks>
            private GazeDeviceWatcherPreview gazeDeviceWatcher;
    
            /// <summary>
            /// Eye-tracking device counter.
            /// </summary>
            private int deviceCounter = 0;
    
            /// <summary>
            /// Timer for gaze focus on RadialProgressBar.
            /// </summary>
            DispatcherTimer timerGaze = new DispatcherTimer();
    
            /// <summary>
            /// Tracker used to prevent gaze timer restarts.
            /// </summary>
            bool timerStarted = false;
    
            /// <summary>
            /// Initialize the app.
            /// </summary>
            public MainPage()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Override of OnNavigatedTo page event starts GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedTo event</param>
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                // Start listening for device events on navigation to eye-tracking page.
                StartGazeDeviceWatcher();
            }
    
            /// <summary>
            /// Override of OnNavigatedFrom page event stops GazeDeviceWatcher.
            /// </summary>
            /// <param name="e">Event args for the NavigatedFrom event</param>
            protected override void OnNavigatedFrom(NavigationEventArgs e)
            {
                // Stop listening for device events on navigation from eye-tracking page.
                StopGazeDeviceWatcher();
            }
        }
    }
    ```

3. Ensuite, nous ajoutons nos méthodes de l’observateur d’appareils de pointage du regard. 
    
    Dans `StartGazeDeviceWatcher`, nous appelons [CreateWatcher](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.createwatcher) et déclarons les écouteurs d’événements de l’appareil de suivi oculaire ([DeviceAdded](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.added), [DeviceUpdated](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.updated) et [DeviceRemoved](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazedevicewatcherpreview.removed)).

    Dans `DeviceAdded`, nous vérifions l’état de l’appareil de suivi oculaire. Si l'appareil est fiable, nous incrémentons le compteur de notre appareil et activons le suivi du regard. Voir l’étape suivante pour plus d’informations.

    Dans `DeviceUpdated`, nous activons également le suivi du regard car cet événement est déclenché si un appareil est réétalonné.

    Dans `DeviceRemoved`, nous décrémentons le compteur de notre appareil et supprimons les gestionnaires d’événements du périphérique.

    Dans `StopGazeDeviceWatcher`, nous arrêtons l’observateur d’appareils de suivi du regard. 

```csharp
    /// <summary>
    /// Start gaze watcher and declare watcher event handlers.
    /// </summary>
    private void StartGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher == null)
        {
            gazeDeviceWatcher = GazeInputSourcePreview.CreateWatcher();
            gazeDeviceWatcher.Added += this.DeviceAdded;
            gazeDeviceWatcher.Updated += this.DeviceUpdated;
            gazeDeviceWatcher.Removed += this.DeviceRemoved;
            gazeDeviceWatcher.Start();
        }
    }

    /// <summary>
    /// Shut down gaze watcher and stop listening for events.
    /// </summary>
    private void StopGazeDeviceWatcher()
    {
        if (gazeDeviceWatcher != null)
        {
            gazeDeviceWatcher.Stop();
            gazeDeviceWatcher.Added -= this.DeviceAdded;
            gazeDeviceWatcher.Updated -= this.DeviceUpdated;
            gazeDeviceWatcher.Removed -= this.DeviceRemoved;
            gazeDeviceWatcher = null;
        }
    }

    /// <summary>
    /// Eye-tracking device connected (added, or available when watcher is initialized).
    /// </summary>
    /// <param name="sender">Source of the device added event</param>
    /// <param name="e">Event args for the device added event</param>
    private void DeviceAdded(GazeDeviceWatcherPreview source, 
        GazeDeviceWatcherAddedPreviewEventArgs args)
    {
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter++;
            TrackerCounter.Text = deviceCounter.ToString();
        }
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Initial device state might be uncalibrated, 
    /// but device was subsequently calibrated.
    /// </summary>
    /// <param name="sender">Source of the device updated event</param>
    /// <param name="e">Event args for the device updated event</param>
    private void DeviceUpdated(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherUpdatedPreviewEventArgs args)
    {
        // Set up gaze tracking.
        TryEnableGazeTrackingAsync(args.Device);
    }

    /// <summary>
    /// Handles disconnection of eye-tracking devices.
    /// </summary>
    /// <param name="sender">Source of the device removed event</param>
    /// <param name="e">Event args for the device removed event</param>
    private void DeviceRemoved(GazeDeviceWatcherPreview source,
        GazeDeviceWatcherRemovedPreviewEventArgs args)
    {
        // Decrement gaze device counter and remove event handlers.
        if (IsSupportedDevice(args.Device))
        {
            deviceCounter--;
            TrackerCounter.Text = deviceCounter.ToString();

            if (deviceCounter == 0)
            {
                gazeInputSource.GazeEntered -= this.GazeEntered;
                gazeInputSource.GazeMoved -= this.GazeMoved;
                gazeInputSource.GazeExited -= this.GazeExited;
            }
        }
    }
```

4. Ici, nous vérifions si l’appareil est fiable dans `IsSupportedDevice` et, si tel est le cas, nous essayons d’activer le suivi du regard dans `TryEnableGazeTrackingAsync`.

    Dans `TryEnableGazeTrackingAsync`, nous déclarons le gestionnaires d’événements de pointage du regard et appelons [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) pour obtenir une référence à la source d’entrée (celle-ci doit être appelée sur le thread de l'interface utilisateur, voir [Assurer la réactivité du thread de l’interface utilisateur](https://docs.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)).

    > [!NOTE]
    > Vous devez appeler [GazeInputSourcePreview.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeinputsourcepreview.getforcurrentview) uniquement quand un appareil de suivi oculaire compatible est connecté et requis par votre application. Dans le cas contraire, la boîte de dialogue de consentement est inutile.

```csharp
    /// <summary>
    /// Initialize gaze tracking.
    /// </summary>
    /// <param name="gazeDevice"></param>
    private async void TryEnableGazeTrackingAsync(GazeDevicePreview gazeDevice)
    {
        // If eye-tracking device is ready, declare event handlers and start tracking.
        if (IsSupportedDevice(gazeDevice))
        {
            timerGaze.Interval = new TimeSpan(0, 0, 0, 0, 20);
            timerGaze.Tick += TimerGaze_Tick;

            SetGazeTargetLocation();

            // This must be called from the UI thread.
            gazeInputSource = GazeInputSourcePreview.GetForCurrentView();

            gazeInputSource.GazeEntered += GazeEntered;
            gazeInputSource.GazeMoved += GazeMoved;
            gazeInputSource.GazeExited += GazeExited;
        }
        // Notify if device calibration required.
        else if (gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.UserCalibrationNeeded ||
                    gazeDevice.ConfigurationState ==
                    GazeDeviceConfigurationStatePreview.ScreenSetupNeeded)
        {
            // Device isn't calibrated, so invoke the calibration handler.
            System.Diagnostics.Debug.WriteLine(
                "Your device needs to calibrate. Please wait for it to finish.");
            await gazeDevice.RequestCalibrationAsync();
        }
        // Notify if device calibration underway.
        else if (gazeDevice.ConfigurationState == 
            GazeDeviceConfigurationStatePreview.Configuring)
        {
            // Device is currently undergoing calibration.  
            // A device update is sent when calibration complete.
            System.Diagnostics.Debug.WriteLine(
                "Your device is being configured. Please wait for it to finish"); 
        }
        // Device is not viable.
        else if (gazeDevice.ConfigurationState == GazeDeviceConfigurationStatePreview.Unknown)
        {
            // Notify if device is in unknown state.  
            // Reconfigure/recalbirate the device.  
            System.Diagnostics.Debug.WriteLine(
                "Your device is not ready. Please set up your device or reconfigure it."); 
        }
    }

    /// <summary>
    /// Check if eye-tracking device is viable.
    /// </summary>
    /// <param name="gazeDevice">Reference to eye-tracking device.</param>
    /// <returns>True, if device is viable; otherwise, false.</returns>
    private bool IsSupportedDevice(GazeDevicePreview gazeDevice)
    {
        TrackerState.Text = gazeDevice.ConfigurationState.ToString();
        return (gazeDevice.CanTrackEyes &&
                    gazeDevice.ConfigurationState == 
                    GazeDeviceConfigurationStatePreview.Ready);
    }
```

5. Ensuite, nous définissons nos gestionnaires d’événements de pointage du regard.

    Nous affichons et masquons l'ellipse de suivi du regard dans `GazeEntered` et `GazeExited`, respectivement.

    Dans `GazeMoved`, nous déplaçons notre ellipse de suivi du regard en fonction de la [EyeGazePosition](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazepointpreview.eyegazeposition) fournie par le [CurrentPoint](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs.currentpoint) de [GazeEnteredPreviewEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.input.preview.gazeenteredprevieweventargs). Nous gérons également le minuteur de focus du regard sur la [RadialProgressBar](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/controls/radialprogressbar), ce qui déclenche le repositionnement de la barre de progression. Voir l’étape suivante pour plus d’informations.

    ```csharp
    /// <summary>
    /// GazeEntered handler.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void GazeEntered(
        GazeInputSourcePreview sender, 
        GazeEnteredPreviewEventArgs args)
    {
        // Show ellipse representing gaze point.
        eyeGazePositionEllipse.Visibility = Visibility.Visible;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeExited handler.
    /// Call DisplayRequest.RequestRelease to conclude the 
    /// RequestActive called in GazeEntered.
    /// </summary>
    /// <param name="sender">Source of the gaze exited event</param>
    /// <param name="e">Event args for the gaze exited event</param>
    private void GazeExited(
        GazeInputSourcePreview sender, 
        GazeExitedPreviewEventArgs args)
    {
        // Hide gaze tracking ellipse.
        eyeGazePositionEllipse.Visibility = Visibility.Collapsed;

        // Mark the event handled.
        args.Handled = true;
    }

    /// <summary>
    /// GazeMoved handler translates the ellipse on the canvas to reflect gaze point.
    /// </summary>
    /// <param name="sender">Source of the gaze moved event</param>
    /// <param name="e">Event args for the gaze moved event</param>
    private void GazeMoved(GazeInputSourcePreview sender, GazeMovedPreviewEventArgs args)
    {
        // Update the position of the ellipse corresponding to gaze point.
        if (args.CurrentPoint.EyeGazePosition != null)
        {
            double gazePointX = args.CurrentPoint.EyeGazePosition.Value.X;
            double gazePointY = args.CurrentPoint.EyeGazePosition.Value.Y;

            double ellipseLeft = 
                gazePointX - 
                (eyeGazePositionEllipse.Width / 2.0f);
            double ellipseTop = 
                gazePointY - 
                (eyeGazePositionEllipse.Height / 2.0f) - 
                (int)Header.ActualHeight;

            // Translate transform for moving gaze ellipse.
            TranslateTransform translateEllipse = new TranslateTransform
            {
                X = ellipseLeft,
                Y = ellipseTop
            };

            eyeGazePositionEllipse.RenderTransform = translateEllipse;

            // The gaze point screen location.
            Point gazePoint = new Point(gazePointX, gazePointY);

            // Basic hit test to determine if gaze point is on progress bar.
            bool hitRadialProgressBar = 
                DoesElementContainPoint(
                    gazePoint, 
                    GazeRadialProgressBar.Name, 
                    GazeRadialProgressBar); 

            // Use progress bar thickness for visual feedback.
            if (hitRadialProgressBar)
            {
                GazeRadialProgressBar.Thickness = 10;
            }
            else
            {
                GazeRadialProgressBar.Thickness = 4;
            }

            // Mark the event handled.
            args.Handled = true;
        }
    }
    ```
6. Enfin, voici les méthodes utilisées pour gérer le minuteur de focus du regard pour cette application.

    `DoesElementContainPoint` Vérifie si le pointeur de regard se trouve sur la barre de progression. Dans ce cas, il démarre le minuteur de pointage du regard et incrémente la barre de progression sur chaque cycle du minuteur de pointage du regard.

    `SetGazeTargetLocation` Définit l’emplacement initial de la barre de progression et, si la barre de progression est terminée (selon le minuteur de focus du regard), la barre de progression se déplace vers un emplacement aléatoire.

    ```csharp
    /// <summary>
    /// Return whether the gaze point is over the progress bar.
    /// </summary>
    /// <param name="gazePoint">The gaze point screen location</param>
    /// <param name="elementName">The progress bar name</param>
    /// <param name="uiElement">The progress bar UI element</param>
    /// <returns></returns>
    private bool DoesElementContainPoint(
        Point gazePoint, string elementName, UIElement uiElement)
    {
        // Use entire visual tree of progress bar.
        IEnumerable<UIElement> elementStack = 
            VisualTreeHelper.FindElementsInHostCoordinates(gazePoint, uiElement, true);
        foreach (UIElement item in elementStack)
        {
            //Cast to FrameworkElement and get element name.
            if (item is FrameworkElement feItem)
            {
                if (feItem.Name.Equals(elementName))
                {
                    if (!timerStarted)
                    {
                        // Start gaze timer if gaze over element.
                        timerGaze.Start();
                        timerStarted = true;
                    }
                    return true;
                }
            }
        }

        // Stop gaze timer and reset progress bar if gaze leaves element.
        timerGaze.Stop();
        GazeRadialProgressBar.Value = 0;
        timerStarted = false;
        return false;
    }

    /// <summary>
    /// Tick handler for gaze focus timer.
    /// </summary>
    /// <param name="sender">Source of the gaze entered event</param>
    /// <param name="e">Event args for the gaze entered event</param>
    private void TimerGaze_Tick(object sender, object e)
    {
        // Increment progress bar.
        GazeRadialProgressBar.Value += 1;

        // If progress bar reaches maximum value, reset and relocate.
        if (GazeRadialProgressBar.Value == 100)
        {
            SetGazeTargetLocation();
        }
    }

    /// <summary>
    /// Set/reset the screen location of the progress bar.
    /// </summary>
    private void SetGazeTargetLocation()
    {
        // Ensure the gaze timer restarts on new progress bar location.
        timerGaze.Stop();
        timerStarted = false;

        // Get the bounding rectangle of the app window.
        Rect appBounds = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().VisibleBounds;

        // Translate transform for moving progress bar.
        TranslateTransform translateTarget = new TranslateTransform();

        // Calculate random location within gaze canvas.
            Random random = new Random();
            int randomX = 
                random.Next(
                    0, 
                    (int)appBounds.Width - (int)GazeRadialProgressBar.Width);
            int randomY = 
                random.Next(
                    0, 
                    (int)appBounds.Height - (int)GazeRadialProgressBar.Height - (int)Header.ActualHeight);

        translateTarget.X = randomX;
        translateTarget.Y = randomY;

        GazeRadialProgressBar.RenderTransform = translateTarget;

        // Show progress bar.
        GazeRadialProgressBar.Visibility = Visibility.Visible;
        GazeRadialProgressBar.Value = 0;
    }
    ```

## <a name="see-also"></a>Articles associés

### <a name="resources"></a>Ressources

- [Bibliothèque de pointage du regard du WindowsCommunity Toolkit](https://docs.microsoft.com/en-us/windows/uwpcommunitytoolkit/gaze/gazeinteractionlibrary)

### <a name="topic-samples"></a>Exemples de la rubrique

- [Exemple de pointage du regard (de base) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-gazeinput-basic.zip)
