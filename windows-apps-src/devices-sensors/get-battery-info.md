---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
Obtenir des informations sur la batterie
Découvrez comment obtenir des informations détaillées sur la batterie à l’aide des API dans l’espace de noms Windows.Devices.Power.
---
# Obtenir des informations sur la batterie

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

** API importantes **

-   [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017)
-   [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432)

Découvrez comment obtenir des informations détaillées sur la batterie à l’aide des API de l’espace de noms [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017). Un *rapport sur la batterie*([**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)) décrit la charge, la capacité et l’état d’une batterie ou d’un groupe de batteries. Cette rubrique montre comment votre application peut obtenir des rapports sur la batterie et être avertie des modifications. Les exemples de code sont tirés de l’application de batterie de base mentionnée à la fin de cette rubrique.

## Obtenir un rapport sur un groupe de batteries


Certains appareils utilisent plusieurs batteries et il n’est pas toujours évident de déterminer de quelle manière chacune d’elles contribue à la capacité globale d’alimentation de l’appareil. C’est là que la classe [**AggregateBattery**](https://msdn.microsoft.com/library/windows/apps/Dn895011) entre en jeu. Le *groupe de batteries* représente tous les contrôleurs de batterie connectés à un appareil et peut fournir un objet [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) global.

**Remarque** Une classe [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) correspond en réalité à un contrôleur de batterie. Le contrôleur est tantôt relié à la batterie physique de l’appareil, tantôt à son boîtier. Par conséquent, il est possible de créer un objet batterie, même en l’absence de toute batterie. Dans certains cas, l’objet batterie peut être **null**.

Lorsque vous disposez d’un groupe de batteries, appelez la méthode [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) pour obtenir le rapport [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) correspondant.

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## Obtenir des rapports sur des batteries individuelles

Vous pouvez également créer un objet [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) pour des batteries individuelles. Utilisez la méthode [**GetDeviceSelector**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.battery.getdeviceselector.aspx) avec la méthode [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432) pour obtenir une collection d’objets [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) représentant tous les contrôleurs de batterie connectés à l’appareil. Ensuite, à l’aide de la propriété **Id** de l’objet **DeviceInformation** souhaité, créez une classe [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) correspondante avec la méthode [**FromIdAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.battery.fromidasync.aspx). Enfin, appelez la méthode [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) pour obtenir le rapport sur la batterie individuelle.

Cet exemple montre comment créer un rapport sur la batterie pour chacune des batteries connectées à l’appareil.

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## Accéder aux détails du rapport

L’objet [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) fournit de nombreuses informations sur la batterie. Pour plus d’informations, reportez-vous à la référence de l’API pour connaître ses propriétés : **Status** (énumération [**BatteryStatus**](https://msdn.microsoft.com/library/windows/apps/Dn818458)), [**ChargeRateInMilliwatts**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.chargerateinmilliwatts.aspx), [**DesignCapacityInMilliwattHours**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.designcapacityinmilliwatthours.aspx), [**FullChargeCapacityInMilliwattHours**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours.aspx) et [**RemainingCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours). Cet exemple présente certaines des propriétés du rapport sur la batterie utilisées par l’application de batterie de base fournie plus loin dans cette rubrique.

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## Demander des mises à jour de rapport

L’objet [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) déclenche l’événement [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) quand la charge, la capacité ou l’état de la batterie changent. Cela se produit généralement immédiatement pour les changements d’état, et périodiquement pour tous les autres changements. Cet exemple montre comment s’inscrire pour les mises à jour de rapport sur la batterie.

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## Gérer les mises à jour de rapport

En cas de mise à jour de la batterie, l’événement [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) transmet l’objet [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) correspondant à la méthode du gestionnaire d’événements. Cependant, ce gestionnaire d’événements n’est pas appelé à partir du thread d’interface utilisateur. Vous devez utiliser l’objet [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) pour appeler toute modification de l’interface utilisateur, comme illustré dans cet exemple.

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## Exemple : application de batterie de base

Testez ces API en créant l’application suivante de batterie de base dans Microsoft Visual Studio. Dans la page d’accueil de Visual Studio, cliquez sur **Nouveau projet**, puis, sous les modèles **Visual C# &gt; Windows &gt; universel**, créez une application à l’aide du modèle **Application vide**.

Ensuite, ouvrez le fichier **MainPage.xaml** et copiez le code XML suivant dans ce fichier (en remplaçant son contenu d’origine).

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

Si votre application n’est pas nommée **App1**, vous devez remplacer la première partie du nom de la classe dans l’extrait de code précédent par l’espace de noms de votre application. Par exemple, si vous avez créé un projet nommé **BasicBatteryApp**, vous devez remplacer `x:Class="App1.MainPage"` par `x:Class="BasicBatteryApp.MainPage"`. Vous devez aussi remplacer `xmlns:local="using:App1"` par `xmlns:local="using:BasicBatteryApp"`.

Ensuite, ouvrez le fichier **MainPage.xaml.cs** de votre projet et remplacez le code existant par ce qui suit.

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar &amp; labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

Si votre application n’est pas nommée **App1**, vous devez renommer l’espace de noms dans l’exemple précédent en lui attribuant le nom que vous avez donné à votre projet. Par exemple, si vous avez créé un projet nommé **BasicBatteryApp**, vous devez remplacer l’espace de noms `App1` par l’espace de noms `BasicBatteryApp`.

Enfin, pour exécuter cette application de batterie de base, dans le menu **Déboguer**, cliquez sur **Démarrer le débogage** afin de tester la solution.

**Conseil** Pour recevoir des valeurs numériques de l’objet [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005), déboguez votre application sur l’**ordinateur local** ou un **appareil** externe (par exemple, un téléphone Windows Phone). Lors du débogage sur un émulateur d’appareil, l’objet **BatteryReport** renvoie **null** aux propriétés de capacité et de vitesse.

 



<!--HONumber=Mar16_HO1-->


