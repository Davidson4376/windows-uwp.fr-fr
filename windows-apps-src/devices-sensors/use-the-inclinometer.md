---
ms.assetid: 16AD53CA-1252-456C-8567-2263D3EC95F3
title: Utiliser l’inclinomètre
description: Découvrez comment utiliser l’inclinomètre pour déterminer le tangage, le roulis et le lacet.
ms.date: 06/06/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bfd617c3c08cdcb7815010648c6036a5f39ee3ab
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7703839"
---
# <a name="use-the-inclinometer"></a>Utiliser l’inclinomètre


**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766)

**Exemple**

-   Pour une implémentation plus complète, consultez l’[exemple d'inclinomètre](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer).

Découvrez comment utiliser l’inclinomètre pour déterminer le tangage, le roulis et le lacet.

Certaines jeux3D requièrent un inclinomètre en tant que périphérique d’entrée. Le simulateur de vol en est un exemple courant, qui mappe les troisaxes de l’inclinomètre (x, y et z) aux entrées de la gouverne de profondeur, de l’aileron et du volet de dérive.

 ## <a name="prerequisites"></a>Prérequis

Il se peut que vous devez être familiarisé avec XAML Extensible Application Markup Language (), Microsoft Visual c# et événements.

L’appareil ou émulateur que vous utilisez doit prendre en charge un inclinomètre.

 ## <a name="create-a-simple-inclinometer-app"></a>Créer une application simple d’inclinomètre

Cette section se divise en deuxsous-sections. La première sous-section vous permet d’accéder aux étapes nécessaires pour créer de bout en bout une application simple d’inclinomètre. La sous-section suivante décrit l’application que vous venez de créer.

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Inclinometer _inclinometer;

            // This event handler writes the current inclinometer reading to
            // the three text blocks on the app' s main page.

            private async void ReadingChanged(object sender, InclinometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    InclinometerReading reading = e.Reading;
                    txtPitch.Text = String.Format("{0,5:0.00}", reading.PitchDegrees);
                    txtRoll.Text = String.Format("{0,5:0.00}", reading.RollDegrees);
                    txtYaw.Text = String.Format("{0,5:0.00}", reading.YawDegrees);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _inclinometer = Inclinometer.GetDefault();


                if (_inclinometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _inclinometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _inclinometer.ReportInterval = reportInterval;

                    // Establish the event handler
                    _inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer, InclinometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

Vous devez remplacer le nom de l’espace de noms dans l’extrait de code précédent par le nom que vous avez donné à votre projet. Par exemple, si vous avez créé un projet nommé **InclinometerCS**, vous devez remplacer `namespace App1` par `namespace InclinometerCS`.

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
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
            <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
            <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
            <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>

        </Grid>
    </Page>
```

Vous devez remplacer la première partie du nom de la classe dans l’extrait de code précédent par l’espace de noms de votre application. Par exemple, si vous avez créé un projet nommé **InclinometerCS**, vous devez remplacer `x:Class="App1.MainPage"` par `x:Class="InclinometerCS.MainPage"`. Vous devez aussi remplacer `xmlns:local="using:App1"` par `xmlns:local="using:InclinometerCS"`.

-   Appuyez sur F5 ou sélectionnez **Déboguer** > **Démarrer le débogage** pour générer, déployer et exécuter l’application.

Une fois l’application en cours d’exécution, vous pouvez modifier les valeurs de l’inclinomètre en déplaçant l’appareil ou à l’aide des outils de l’émulateur.

-   Pour arrêter l’application, retournez dans Visual Studio et appuyez sur Maj+ 5, ou sélectionnez **Déboguer** > **Arrêter le débogage**.

###  <a name="explanation"></a>Explication

L’exemple précédent démontre la faible quantité de code que vous devrez écrire afin d’intégrer l’entrée de l’inclinomètre dans votre application.

L’application établit une connexion avec l’inclinomètre par défaut dans la méthode **MainPage**.

```csharp
_inclinometer = Inclinometer.GetDefault();
```

L’application établit l’intervalle de rapport dans la méthode **MainPage**. Le code suivant récupère l’intervalle minimal pris en charge par l’appareil et le compare à un intervalle demandé de 16millisecondes (ce qui représente une fréquence de rafraîchissement de 60Hz). Si l’intervalle pris en charge minimum est supérieur à l’intervalle demandé, le code définit la valeur sur l’intervalle minimum. Sinon, il définit la valeur sur l’intervalle demandé.

```csharp
uint minReportInterval = _inclinometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_inclinometer.ReportInterval = reportInterval;
```

Les nouvelles données de l’inclinomètre sont capturées dans la méthode **ReadingChanged**. Chaque fois que le pilote du capteur reçoit de nouvelles données du capteur, il transmet les valeurs à votre application à l’aide de ce gestionnaire d’événements. L’application inscrit ce gestionnaire d’événements sur la ligne suivante.

```csharp
_inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer,
InclinometerReadingChangedEventArgs>(ReadingChanged);
```

Ces nouvelles valeurs sont écrites dans les TextBlocks identifiés dans le codeXAML du projet.

```xml
<TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
 <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
 <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
 <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>
```

