---
author: muhsinking
ms.assetid: 454953E1-DD8F-44B7-A614-7BAD8C683536
title: Utiliser le gyromètre
description: Découvrez comment utiliser le gyromètre pour détecter les changements de mouvements de l’utilisateur.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 84c27e35e7e9cc3763a4d1d86348da14f3a18910
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5666495"
---
# <a name="use-the-gyrometer"></a>Utiliser le gyromètre


**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718)

**Exemple**

-   Pour une implémentation plus complète, consultez l’[exemple de gyromètre](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/gyrometer).

Découvrez comment utiliser le gyromètre pour détecter les changements de mouvements de l’utilisateur.

Les gyromètres viennent compléter les accéléromètres en tant que contrôleurs de jeu. Tandis que l’accéléromètre peut mesurer le déplacement linéaire, le gyromètre mesure la vitesse angulaire ou déplacement rotatoire.

## <a name="prerequisites"></a>Prérequis

Vous devez être familiarisé avec XAML Extensible Application Markup Language (), Microsoft Visual c# et événements.

L’appareil ou émulateur que vous utilisez doit prendre en charge un gyromètre.

## <a name="create-a-simple-gyrometer-app"></a>Créer une application simple de gyromètre

Cette section se divise en deuxsous-sections. La première sous-section vous permet d’accéder aux étapes nécessaires pour créer de bout en bout une application simple de gyromètre. La sous-section suivante décrit l’application que vous venez de créer.

###  <a name="instructions"></a>Instructions

-   Créez un projet en choisissant une **Application vide (Windows universel)** dans les modèles de projet **Visual C#**.

-   Ouvrez le fichier MainPage.xaml.cs de votre projet et remplacez le code existant par ce qui suit.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the gyrometer


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Gyrometer _gyrometer; // Our app' s gyrometer object

            // This event handler writes the current gyrometer reading to
            // the three textblocks on the app' s main page.

            private async void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    GyrometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityZ);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object

                if (_gyrometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _gyrometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _gyrometer.ReportInterval = reportInterval;

                    // Assign an event handler for the gyrometer reading-changed event
                    _gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer, GyrometerReadingChangedEventArgs>(ReadingChanged);
                }

            }
        }
    }
```

Vous devez remplacer le nom de l’espace de noms dans l’extrait de code précédent par le nom que vous avez donné à votre projet. Par exemple, si vous avez créé un projet nommé **GyrometerCS**, vous devez remplacer `namespace App1` par `namespace GyrometerCS`.

-   Ouvrez le fichier MainPage.xaml et remplacez le contenu d’origine par le code XML suivant.

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
            <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>

        </Grid>
    </Page>
```

Vous devez remplacer la première partie du nom de la classe dans l’extrait de code précédent par l’espace de noms de votre application. Par exemple, si vous avez créé un projet nommé **GyrometerCS**, vous devez remplacer `x:Class="App1.MainPage"` par `x:Class="GyrometerCS.MainPage"`. Vous devez aussi remplacer `xmlns:local="using:App1"` par `xmlns:local="using:GyrometerCS"`.

-   Appuyez sur F5 ou sélectionnez **Déboguer** > **Démarrer le débogage** pour générer, déployer et exécuter l’application.

Une fois l’application en cours d’exécution, vous pouvez modifier les valeurs du gyromètre en déplaçant l’appareil ou à l’aide des outils de l’émulateur.

-   Pour arrêter l’application, retournez dans Visual Studio et appuyez sur Maj+ 5, ou sélectionnez **Déboguer** > **Arrêter le débogage**.

###  <a name="explanation"></a>Explication

L’exemple précédent démontre la faible quantité de code que vous devrez écrire afin d’intégrer l’entrée du gyromètre dans votre application.

L’application établit une connexion avec le gyromètre par défaut dans la méthode **MainPage**.

```csharp
_gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object
```

L’application établit l’intervalle de rapport dans la méthode **MainPage**. Le code suivant récupère l’intervalle minimal pris en charge par l’appareil et le compare à un intervalle demandé de 16millisecondes (ce qui représente une fréquence de rafraîchissement de 60Hz). Si l’intervalle pris en charge minimum est supérieur à l’intervalle demandé, le code définit la valeur sur l’intervalle minimum. Sinon, il définit la valeur sur l’intervalle demandé.

```csharp
uint minReportInterval = _gyrometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_gyrometer.ReportInterval = reportInterval;
```

Les nouvelles données du gyromètre sont capturées dans la méthode **ReadingChanged**. Chaque fois que le pilote du capteur reçoit de nouvelles données du capteur, il transmet les valeurs à votre application à l’aide de ce gestionnaire d’événements. L’application inscrit ce gestionnaire d’événements sur la ligne suivante.

```csharp
_gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer,
GyrometerReadingChangedEventArgs>(ReadingChanged);
```

Ces nouvelles valeurs sont écrites dans les TextBlocks identifiés dans le code XAML du projet.

```xml
        <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
        <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
        <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
        <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
        <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
        <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>
```

 ## <a name="related-topics"></a>Rubriques connexes

* [Exemple de gyromètre](http://go.microsoft.com/fwlink/p/?linkid=241379)
