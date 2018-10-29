---
author: muhsinking
ms.assetid: F90686F5-641A-42D9-BC44-EC6CA11B8A42
title: Utiliser l’accéléromètre
description: Découvrez comment utiliser l’accéléromètre pour répondre aux mouvements de l’utilisateur.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 482092e43acd6999361640e598a44391ac3f11a5
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5744674"
---
# <a name="use-the-accelerometer"></a>Utiliser l’accéléromètre


**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687)

**Exemple**

-   Pour une implémentation plus complète, consultez l’[exemple d'accéléromètre](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer).

Découvrez comment utiliser l’accéléromètre pour répondre aux mouvements de l’utilisateur.

Une application de jeu simple repose sur un capteur unique, l’accéléromètre, comme périphérique d’entrée. Ces applications utilisent généralement un ou deux axes pour l’entrée, mais elles peuvent aussi utiliser l’événement poignée comme autre source d’entrée.

## <a name="prerequisites"></a>Prérequis

Vous devez être familiarisé avec XAML Extensible Application Markup Language (), Microsoft Visual c# et événements.

L’appareil ou émulateur que vous utilisez doit prendre en charge un accéléromètre.

## <a name="create-a-simple-accelerometer-app"></a>Créer une application simple d’accéléromètre

Cette section se divise en deuxsous-sections. La première sous-section vous permet d’accéder aux étapes nécessaires pour créer de bout en bout une application simple d’accéléromètre. La sous-section suivante décrit l’application que vous venez de créer.

### <a name="instructions"></a>Instructions

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

    // Required to support the core dispatcher and the accelerometer

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    namespace App1
    {

        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private Accelerometer _accelerometer;

            // This event handler writes the current accelerometer reading to
            // the three acceleration text blocks on the app' s main page.

            private async void ReadingChanged(object sender, AccelerometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    AccelerometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationZ);

                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _accelerometer = Accelerometer.GetDefault();

                if (_accelerometer != null)
                {
                    // Establish the report interval
                    uint minReportInterval = _accelerometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _accelerometer.ReportInterval = reportInterval;

                    // Assign an event handler for the reading-changed event
                    _accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer, AccelerometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

Vous devez remplacer le nom de l’espace de noms dans l’extrait de code précédent par le nom que vous avez donné à votre projet. Par exemple, si vous avez créé un projet nommé **AccelerometerCS**, vous devez remplacer `namespace App1` par `namespace AccelerometerCS`.

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
            <TextBlock HorizontalAlignment="Left" Height="25" Margin="8,20,0,0" TextWrapping="Wrap" Text="X-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFEDE6E6"/>
            <TextBlock HorizontalAlignment="Left" Height="27" Margin="8,49,0,0" TextWrapping="Wrap" Text="Y-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF5F2F2"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,80,0,0" TextWrapping="Wrap" Text="Z-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF6F0F0"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>

        </Grid>
    </Page>
```

Vous devez remplacer la première partie du nom de la classe dans l’extrait de code précédent par l’espace de noms de votre application. Par exemple, si vous avez créé un projet nommé **AccelerometerCS**, vous devez remplacer `x:Class="App1.MainPage"` par `x:Class="AccelerometerCS.MainPage"`. Vous devez aussi remplacer `xmlns:local="using:App1"` par `xmlns:local="using:AccelerometerCS"`.

-   Appuyez sur F5 ou sélectionnez **Déboguer** &gt; **Démarrer le débogage** pour générer, déployer et exécuter l’application.

Une fois l’application en cours d’exécution, vous pouvez modifier les valeurs de l’accéléromètre en déplaçant l’appareil ou à l’aide des outils de l’émulateur.

-   Pour arrêter l’application, retournez dans Visual Studio et appuyez sur Maj+F5, ou sélectionnez **Déboguer** &gt; **Arrêter le débogage**.

### <a name="explanation"></a>Explication

L’exemple précédent démontre la faible quantité de code que vous devrez écrire afin d’intégrer l’entrée de l’accéléromètre dans votre application.

L’application établit une connexion avec l’accéléromètre par défaut dans la méthode **MainPage**.

```csharp
_accelerometer = Accelerometer.GetDefault();
```

L’application établit l’intervalle de rapport dans la méthode **MainPage**. Le code suivant récupère l’intervalle minimal pris en charge par l’appareil et le compare à un intervalle demandé de 16millisecondes (ce qui représente une fréquence de rafraîchissement de 60Hz). Si l’intervalle pris en charge minimum est supérieur à l’intervalle demandé, le code définit la valeur sur l’intervalle minimum. Sinon, il définit la valeur sur l’intervalle demandé.

```csharp
uint minReportInterval = _accelerometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_accelerometer.ReportInterval = reportInterval;
```

Les nouvelles données de l’accéléromètre sont capturées dans la méthode **ReadingChanged**. Chaque fois que le pilote du capteur reçoit de nouvelles données du capteur, il transmet les valeurs à votre application à l’aide de ce gestionnaire d’événements. L’application inscrit ce gestionnaire d’événements sur la ligne suivante.

```csharp
_accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer,
AccelerometerReadingChangedEventArgs>(ReadingChanged);
```

Ces nouvelles valeurs sont écrites dans les TextBlocks identifiés dans le codeXAML du projet.

```xml
<TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
 <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
 <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>
```
