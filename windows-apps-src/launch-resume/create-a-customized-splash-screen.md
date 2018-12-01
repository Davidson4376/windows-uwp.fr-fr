---
title: Afficher un écran de démarrage plus longtemps
description: Affichez un écran de démarrage plus longtemps en créant et en affichant un écran de démarrage étendu pour votre application. Cet écran étendu imite l’écran de démarrage affiché quand votre application est lancée, mais il est personnalisable.
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.date: 11/08/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 142ee642806ebba41d6ddb4d49fe55217e7a0e2e
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338426"
---
# <a name="display-a-splash-screen-for-more-time"></a>Afficher un écran de démarrage plus longtemps

**API importantes**

-   [**Classe SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)
-   [**Événement Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055)
-   [**Méthode Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Affichez un écran de démarrage plus longtemps en créant un écran de démarrage étendu pour votre application. Cet écran étendu imite l’écran de démarrage affiché quand votre application est lancée, mais il est personnalisable. Que vous vouliez afficher des informations de chargement en temps réel ou simplement donner à votre application un délai supplémentaire pour préparer son interface utilisateur initiale, un écran de démarrage étendu vous permet de définir l’expérience de lancement.

> [!NOTE]
> L’expression «écran de démarrage étendu» dans cette rubrique fait référence à un écran de démarrage qui reste à l’écran pendant une période prolongée. Elle ne désigne pas une sous-classe qui dérive de la classe [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763).

Assurez-vous que votre écran de démarrage étendu imite avec précision l’écran de démarrage par défaut, en suivant ces recommandations:

-   La page de votre écran de démarrage étendu doit utiliser une image de 620x300 pixels cohérente avec l’image spécifiée pour l’écran de démarrage dans le manifeste de votre application (image de l’écran de démarrage de votre application). Dans Microsoft Studio2015 visuelle, les paramètres d’écran de démarrage sont stockés dans la section de **l’Écran de démarrage** de l’onglet **Ressources visuelles** dans votre manifeste d’application (fichier Package.appxmanifest).
-   Votre écran de démarrage étendu doit utiliser une couleur d’arrière-plan identique à celle spécifiée pour l’écran de démarrage dans votre manifeste de l’application (arrière-plan de l’écran de démarrage de votre application).
-   Votre code doit utiliser la classe [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) pour positionner l’image de l’écran de démarrage de votre application aux mêmes coordonnées d’écran que l’écran de démarrage par défaut.
-   Votre code doit répondre aux événements de redimensionnement de fenêtre (par exemple, quand l’écran pivote ou que votre application est déplacée à côté d’une autre application à l’écran) via la classe [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763), pour permettre le repositionnement des éléments sur votre écran de démarrage étendu.

Procédez comme suit pour créer un écran de démarrage étendu qui imite efficacement l’écran de démarrage par défaut.

## <a name="add-a-blank-page-item-to-your-existing-app"></a>Ajouter un élément **Page vierge** à votre application existante


Cette rubrique suppose que vous voulez ajouter un écran de démarrage étendu à un projet d’application de plateforme Windows universelle (UWP) existant en C#, Visual Basic ou C++.

-   Ouvrez votre application dans Visual Studio.
-   Appuyez sur ou ouvrez **Projet** à partir de la barre de menus, puis cliquez sur **Ajouter un nouvel élément**. Une boîte de dialogue **Ajouter un nouvel élément** s’affiche.
-   À partir de cette boîte de dialogue, ajoutez un nouvel élément **Page vierge** à votre application. Dans cette rubrique, «ExtendedSplash» est le nom de la page de l’écran de démarrage étendu.

L’ajout d’un élément **Page vierge** génère deux fichiers, l’un pour le balisage (ExtendedSplash.xaml) et l’autre pour le code (ExtendedSplash.xaml.cs).

## <a name="essential-xaml-for-an-extended-splash-screen"></a>XAML de base pour un écran de démarrage étendu


Suivez ces étapes pour ajouter une image et un contrôle de progression à votre écran de démarrage étendu.

Dans votre fichier ExtendedSplash.xaml:

-   Modifiez la propriété [**Background**](https://msdn.microsoft.com/library/windows/apps/br209396) de l’élément [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) par défaut pour correspondre à la couleur d’arrière-plan que vous avez définie pour l’écran de démarrage de votre application dans votre manifeste de l’application (dans la section **Composants visuels** de votre fichier Package.appxmanifest). La couleur par défaut de l’écran de démarrage est de type gris clair (valeur hexadécimale \#464646). Notez que cet élément **Grid** est fourni par défaut quand vous créez une **Page vierge**. Vous n’avez pas à utiliser **Grid**. Il s’agit juste d’une base de départ pour la création d’un écran de démarrage étendu.
-   Ajoutez un élément [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) à [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Vous utiliserez **Canvas** pour positionner votre image de l’écran de démarrage étendu.
-   Ajoutez un élément [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) à [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267). Utilisez la même image de 600 x 320 pixels pour votre écran de démarrage étendu que pour l’écran de démarrage par défaut.
-   (Facultatif) Ajoutez un contrôle de progression pour indiquer aux utilisateurs que votre application est en cours de chargement. Dans cette rubrique, [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) est ajouté à la place d’un [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/br227529) déterminé ou indéterminé.

L’exemple suivant montre une [**grille**](https://msdn.microsoft.com/library/windows/apps/br242704) avec ces ajouts et des modifications.

```xaml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

> [!NOTE]
> Cet exemple définit la largeur de la [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) à 20 pixels. Vous pouvez définir manuellement sa largeur en fonction d’une valeur qui convient pour votre application. Toutefois, le contrôle ne s’affiche pas pour les largeurs de moins de 20pixels.

## <a name="essential-code-for-an-extended-splash-screen-class"></a>Code de base pour une classe d’écran de démarrage étendu


Votre écran de démarrage étendu doit répondre chaque fois que la taille (Windows uniquement) ou l’orientation de la fenêtre change. La position de l’image que vous utilisez doit être mise à jour afin que votre écran de démarrage étendu s’affiche toujours correctement, quelle que soit la façon dont la fenêtre change.

Utilisez la procédure ci-dessous pour définir des méthodes permettant d’afficher correctement votre écran de démarrage étendu.

1.  **Ajouter les espaces de noms nécessaires**

    Vous devez ajouter les espaces de noms suivants à ExtendedSplash.xaml.cs pour accéder à la classe [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763), événements [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055).

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.UI.Core;
    ```

2.  **Créez une classe partielle et déclarer des variables de classe.**

    Incluez le code suivant dans ExtendedSplash.xaml.cs pour créer une classe partielle et représenter un écran de démarrage étendu.

    ```cs
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

       // Define methods and constructor
    }
    ```

    Ces variables de classe sont utilisées par plusieurs méthodes. La variable `splashImageRect` stocke les coordonnées utilisées par le système pour afficher l’image de l’écran de démarrage de l’application. La variable `splash` stocke un objet [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) et la variable `dismissed` effectue le suivi de l’écran de démarrage affiché afin de savoir si le système a fait disparaître ou non ce dernier.

3.  **Définir un constructeur pour votre classe qui positionne correctement l’image**

    Le code suivant définit un constructeur pour la classe d’écran de démarrage étendu qui écoute les événements de redimensionnement de fenêtre, positionne l’image et (facultatif) le contrôle de progression sur l’écran de démarrage étendu, crée un cadre pour la navigation, et appelle une méthode asynchrone pour restaurer l’état de session enregistré.

    ```cs
    public ExtendedSplash(SplashScreen splashscreen, bool loadState)
    {
        InitializeComponent();

        // Listen for window resize events to reposition the extended splash screen image accordingly.
        // This ensures that the extended splash screen formats properly in response to window resizing.
        Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

        splash = splashscreen;
        if (splash != null)
        {
            // Register an event handler to be executed when the splash screen has been dismissed.
            splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

            // Retrieve the window coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            PositionRing();
        }

        // Create a Frame to act as the navigation context
        rootFrame = new Frame();            
    }
    ```

    Veillez à inscrire votre gestionnaire [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) (`ExtendedSplash_OnResize` dans l’exemple) dans le constructeur de votre classe pour permettre à votre application de positionner correctement l’image dans votre écran de démarrage étendu.

4.  **Définissez une méthode de classe pour positionner l’image dans l’écran de démarrage étendu.**

    Ce code indique comment positionner l’image sur la page de l’écran de démarrage étendu avec la variable de classe `splashImageRect`.

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **(Facultatif) Définir une méthode de classe pour positionner un contrôle de progression dans votre écran de démarrage étendu**

    Si vous avez choisi d’ajouter [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) à votre écran de démarrage étendu, positionnez-le par rapport à l’image de l’écran de démarrage. Ajoutez le code suivant à ExtendedSplash.xaml.cs pour centrer **ProgressRing** 32 pixels en dessous de l’image.

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **À l’intérieur de la classe, définissez un gestionnaire pour l’événement Dismissed.**

    Dans ExtendedSplash.xaml.cs, affectez la valeur true à la variable de classe `dismissed` en réponse à l’événement [**SplashScreen.Dismissed**](https://msdn.microsoft.com/library/windows/apps/br224764). Si votre application comporte des opérations d’installation, ajoutez-les à ce gestionnaire d’événements.

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    Une fois l’installation de l’application terminée, quittez votre écran de démarrage étendu. Le code suivant définit une méthode appelée `DismissExtendedSplash`, qui accède au `MainPage` défini dans le fichier MainPage.xaml de votre application.

    ```cs
    async void DismissExtendedSplash()
      {
         await Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,() =>            {
              rootFrame = new Frame();
              rootFrame.Content = new MainPage(); Window.Current.Content = rootFrame;
            });
      }
      ```

7.  **À l’intérieur de la classe, définissez un gestionnaire pour l’événement Window.SizeChanged.**

    Préparez votre écran de démarrage étendu à repositionner ses éléments si un utilisateur redimensionne la fenêtre. Ce code répond à un événement [**Window.SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br209055) en capturant les nouvelles coordonnées et en repositionnant l’image. Si vous avez ajouté un contrôle de progression à votre écran de démarrage étendu, repositionnez-le également dans ce gestionnaire d’événements.

    ```cs
    void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
    {
        // Safely update the extended splash screen image coordinates. This function will be executed when a user resizes the window.
        if (splash != null)
        {
            // Update the coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            // PositionRing();
        }
    }
    ```

    > [!NOTE]
    > Avant d’essayer d’obtenir l’emplacement de l’image Assurez-vous que la variable de classe (`splash`) contient un objet [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) valide, tel qu’illustré dans l’exemple.

     

8.  **(Facultatif) Ajouter une méthode de classe pour restaurer un état de session enregistré**

    Le code que vous avez ajouté à la méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), à l’étape 4 : [Modifier le gestionnaire d’activation de lancement](#modify-the-launch-activation-handler), indique à votre application d’afficher un écran de démarrage étendu durant son lancement. Pour consolider toutes les méthodes liées au lancement de l’application dans votre classe d’écran de démarrage étendu, vous pouvez envisager l’ajout d’une méthode à votre fichier ExtendedSplash.xaml.cs pour restaurer l’état de l’application.

    ```cs
    void RestoreState(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    Quand vous modifiez le gestionnaire d’activation de lancement dans App.xaml.cs, vous affectez également la valeur true à `loadstate`, si le [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) précédent de votre application est **Terminated**. Dans ce cas, la méthode `RestoreState` restaure l’application dans son état antérieur. Pour une vue d’ensemble du lancement, de la suspension et de l’arrêt de l’application, voir [Cycle de vie de l’application](app-lifecycle.md).

## <a name="modify-the-launch-activation-handler"></a>Modifier le gestionnaire d’activation de lancement


Lors du lancement de votre application, le système transmet les informations de l’écran de démarrage au gestionnaire d’événements d’activation de lancement de l’application. Vous pouvez utiliser ces informations pour positionner correctement l’image sur la page de votre écran de démarrage étendu. Vous pouvez obtenir ces informations d’écran de démarrage à partir des arguments d’événements d’activation transmis au gestionnaire [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) de votre application (voir la variable `args` dans le code suivant).

Si vous n’avez pas encore remplacé le gestionnaire [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) de votre application, voir [Cycle de vie de l’application](app-lifecycle.md) pour apprendre à gérer les événements d’activation.

Dans App.xaml.cs, ajoutez le code suivant pour créer et afficher un écran de démarrage étendu.

```cs
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    if (args.PreviousExecutionState != ApplicationExecutionState.Running)
    {
        bool loadState = (args.PreviousExecutionState == ApplicationExecutionState.Terminated);
        ExtendedSplash extendedSplash = new ExtendedSplash(args.SplashScreen, loadState);
        Window.Current.Content = extendedSplash;
    }

    Window.Current.Activate();
}
```

## <a name="complete-code"></a>Code complet

Le code suivant diffère légèrement des extraits présentés dans les étapes précédentes.
-   ExtendedSplash.xaml comprend un bouton `DismissSplash`. Quand l’utilisateur clique sur ce bouton, le gestionnaire d’événements `DismissSplashButton_Click` appelle la méthode `DismissExtendedSplash`. Dans votre application, appelez `DismissExtendedSplash` quand votre application a terminé de charger les ressources ou d’initialiser son interface utilisateur.
-   Cette application utilise également un modèle de projet d’application UWP, qui utilise la navigation [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682). Ainsi, dans App.xaml.cs, le gestionnaire d’activation de lancement ([**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)) définit `rootFrame` et l’utilise pour définir le contenu de la fenêtre de l’application.

### <a name="extendedsplashxaml"></a>ExtendedSplash.xaml

Cet exemple inclut un `DismissSplash` de bouton, car il n’a pas pour charger les ressources d’application. Dans votre application, masquez l’écran de démarrage étendu automatiquement une fois que l’application a terminé le chargement des ressources ou la préparation de son interface utilisateur initiale.

```xml
<Page
    x:Class="SplashScreenExample.ExtendedSplash"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SplashScreenExample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"/>
        </Canvas>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Bottom">
            <Button x:Name="DismissSplash" Content="Dismiss extended splash screen" HorizontalAlignment="Center" Click="DismissSplashButton_Click" />
        </StackPanel>
    </Grid>
</Page>
```

### <a name="extendedsplashxamlcs"></a>ExtendedSplash.xaml.cs

Notez que la `DismissExtendedSplash` méthode est appelée à partir du Gestionnaire d’événements click pour le `DismissSplash` bouton. Dans votre application, vous n’avez pas besoin d’un bouton `DismissSplash`. À la place, appelez `DismissExtendedSplash` une fois que votre application a terminé le chargement des ressources et que vous voulez accéder à sa page principale.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

using Windows.ApplicationModel.Activation;
using SplashScreenExample.Common;
using Windows.UI.Core;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234238

namespace SplashScreenExample
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

        public ExtendedSplash(SplashScreen splashscreen, bool loadState)
        {
            InitializeComponent();

            // Listen for window resize events to reposition the extended splash screen image accordingly.
            // This is important to ensure that the extended splash screen is formatted properly in response to snapping, unsnapping, rotation, etc...
            Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

            splash = splashscreen;

            if (splash != null)
            {
                // Register an event handler to be executed when the splash screen has been dismissed.
                splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

                // Retrieve the window coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();

                // Optional: Add a progress ring to your splash screen to show users that content is loading
                PositionRing();
            }

            // Create a Frame to act as the navigation context
            rootFrame = new Frame();

            // Restore the saved session state if necessary
            RestoreState(loadState);
        }

        void RestoreState(bool loadState)
        {
            if (loadState)
            {
                // TODO: write code to load state
            }
        }

        // Position the extended splash screen image in the same location as the system splash screen image.
        void PositionImage()
        {
            extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
            extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
            extendedSplashImage.Height = splashImageRect.Height;
            extendedSplashImage.Width = splashImageRect.Width;

        }

        void PositionRing()
        {
            splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
            splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
        }

        void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
        {
            // Safely update the extended splash screen image coordinates. This function will be fired in response to snapping, unsnapping, rotation, etc...
            if (splash != null)
            {
                // Update the coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();
                PositionRing();
            }
        }

        // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
        void DismissedEventHandler(SplashScreen sender, object e)
        {
            dismissed = true;

            // Complete app setup operations here...
        }

        void DismissExtendedSplash()
        {
            // Navigate to mainpage
            rootFrame.Navigate(typeof(MainPage));
            // Place the frame in the current Window
            Window.Current.Content = rootFrame;
        }

        void DismissSplashButton_Click(object sender, RoutedEventArgs e)
        {
            DismissExtendedSplash();
        }
    }
}
```

### <a name="appxamlcs"></a>App.Xaml.cs

Ce projet a été créé à l’aide du modèle projet **Application vide (XAML)** d’application UWP dans Visual Studio. Les gestionnaires d’événements `OnNavigationFailed` et `OnSuspending` sont générés automatiquement et n’ont pas besoin d’être modifiés pour l’implémentation d’un écran de démarrage étendu. Cette rubrique modifie uniquement `OnLaunched`.

Si vous n’avez pas utilisé de modèle de projet pour votre application, voir l’étape 4 : [Modifier le gestionnaire d’activation de lancement](#modify-the-launch-activation-handler) pour obtenir un exemple de `OnLaunched` modifié qui n’utilise pas la navigation [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682).

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Application template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234227

namespace SplashScreenExample
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : Application
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Microsoft.ApplicationInsights.WindowsAppInitializer.InitializeAsync(
            Microsoft.ApplicationInsights.WindowsCollectors.Metadata |
            Microsoft.ApplicationInsights.WindowsCollectors.Session);
            this.InitializeComponent();
            this.Suspending += OnSuspending;
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="e">Details about the launch request and process.</param>
        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
#if DEBUG
            if (System.Diagnostics.Debugger.IsAttached)
            {
                this.DebugSettings.EnableFrameRateCounter = true;
            }
#endif

            Frame rootFrame = Window.Current.Content as Frame;

            // Do not repeat app initialization when the Window already has content,
            // just ensure that the window is active
            if (rootFrame == null)
            {
                // Create a Frame to act as the navigation context and navigate to the first page
                rootFrame = new Frame();
                // Set the default language
                rootFrame.Language = Windows.Globalization.ApplicationLanguages.Languages[0];

                rootFrame.NavigationFailed += OnNavigationFailed;

                //  Display an extended splash screen if app was not previously running.
                if (e.PreviousExecutionState != ApplicationExecutionState.Running)
                {
                    bool loadState = (e.PreviousExecutionState == ApplicationExecutionState.Terminated);
                    ExtendedSplash extendedSplash = new ExtendedSplash(e.SplashScreen, loadState);
                    rootFrame.Content = extendedSplash;
                    Window.Current.Content = rootFrame;
                }
            }

            if (rootFrame.Content == null)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(typeof(MainPage), e.Arguments);
            }
            // Ensure the current window is active
            Window.Current.Activate();
        }

        /// <summary>
        /// Invoked when Navigation to a certain page fails
        /// </summary>
        /// <param name="sender">The Frame which failed navigation</param>
        /// <param name="e">Details about the navigation failure</param>
        void OnNavigationFailed(object sender, NavigationFailedEventArgs e)
        {
            throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
        }

        /// <summary>
        /// Invoked when application execution is being suspended.  Application state is saved
        /// without knowing whether the application will be terminated or resumed with the contents
        /// of memory still intact.
        /// </summary>
        /// <param name="sender">The source of the suspend request.</param>
        /// <param name="e">Details about the suspend request.</param>
        private void OnSuspending(object sender, SuspendingEventArgs e)
        {
            var deferral = e.SuspendingOperation.GetDeferral();
            // TODO: Save applicaiton state and stop any background activity
            deferral.Complete();
        }
    }
}
```

## <a name="related-topics"></a>Rubriques connexes


* [Cycle de vie de l’application](app-lifecycle.md)

**Référence**

* [**Espace de noms Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Classe Windows.ApplicationModel.Activation.SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763)
* [**Propriété Windows.ApplicationModel.Activation.SplashScreen.ImageLocation**](https://msdn.microsoft.com/library/windows/apps/br224765)
* [**Événement Windows.ApplicationModel.Core.CoreApplicationView.Activated**](https://msdn.microsoft.com/library/windows/apps/br225018)

 

 
