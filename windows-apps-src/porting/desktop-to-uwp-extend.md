---
author: normesta
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: Étendre votre application de bureau avec des interfaces utilisateur et des composants Windows
ms.author: normesta
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bed06d5f9f43acd5aa4ec5ff7b2b7139ad0dd26f
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4465893"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Étendre votre application de bureau avec des composants UWP modernes

Certaines expériences Windows10 (par exemple: une page d'interface utilisateur tactile) doivent s'exécuter à l'intérieur d'un conteneur d'application moderne. Si vous souhaitez ajouter ces expériences, étendez votre application de bureau avec des composant de projets UWP et Windows Runtime.

Dans de nombreux cas, vous pouvez appeler des API UWP directement à partir de votre application de bureau. Par conséquent, avant de consulter ce guide, consultez [Optimisation pour Windows10](desktop-to-uwp-enhance.md).

>[!NOTE]
>Ce guide suppose que vous avez créé un package d’application Windows pour votre application de bureau. Si vous n’avez pas encore fait, consultez [les applications de bureau de Package](desktop-to-uwp-root.md).

Si vous êtes prêt, commençons.

<a id="setup" />

## <a name="first-setup-your-solution"></a>Tout d’abord, configurez votre Solution

Ajoutez un ou plusieurs projets UWP et composants d'exécution à votre solution.

Commencez avec une solution qui contient un **Projet de création de packages d’Application Windows** avec une référence à votre application de bureau.

Cette image illustre un exemple de solution.

![Démarrage étendu d'un projet](images/desktop-to-uwp/extend-start-project.png)

Si votre solution ne contient pas un projet de création de packages, consultez le [Package de votre application de bureau à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md).

### <a name="add-a-uwp-project"></a>Ajouter un projet UWP

Ajoutez une **Application vide (Windows universelle)** à votre solution.

C'est ici que vous générez une interface utilisateur XAML moderne, ou que vous utilisez vos API uniquement exécutées dans un processus UWP.

![Projet UWP](images/desktop-to-uwp/add-uwp-project-to-solution.png)

Dans votre projet de mise en package, cliquez avec le bouton droit sur le nœud **Applications**, puis cliquez sur **Ajouter une référence**.

![Référencer un projet UWP](images/desktop-to-uwp/add-uwp-project-reference.png)

Puis, ajoutez une référence au projet UWP.

![Référencer un projet UWP](images/desktop-to-uwp/choose-uwp-project.png)

Votre solution doit ressembler à ceci:

![Solution avec un projet UWP](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Facultatif) Ajoutez un composant Windows Runtime

Pour accomplir certains scénarios, vous devez ajouter un code à un composant WindowsRuntime.

![service d’application composant d'exécution](images/desktop-to-uwp/add-runtime-component.png)

Ensuite, à partir de votre projet UWP, ajoutez une référence au composant d'exécution. Votre solution doit ressembler à ceci:

![Référence de composant d'exécution](images/desktop-to-uwp/runtime-component-reference.png)

Examinons quelques-unes des possibilités concernant vos projets UWP et les composants d'exécution.

## <a name="show-a-modern-xaml-ui"></a>Afficher une interface utilisateur XAML moderne

Dans le cadre du flux de votre application, vous pouvez incorporer des interfaces utilisateur XAML modernes dans votre application de bureau. Ces interfaces utilisateur s'adaptent naturellement à différentes résolutions et tailles d’écran et prennent en charge des modèles interactifs modernes tels que la fonctionnalité tactile et l’entrée manuscrite.

Par exemple, avec une petite quantité de balisage XAML, vous pouvez offrir aux utilisateurs des fonctionnalités puissantes de visualisation cartographique.

Cette image montre une application Windows Forms qui ouvre une interface utilisateur XAML moderne contenant un contrôle de carte.

![adaptive-design](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>Cet exemple montre une UI XAML en ajoutant un projet UWP à la solution. C’est l’approche stable pris en charge à l’affichage des interfaces utilisateur XAML dans une application de bureau. L’alternative à cette approche consiste à ajouter des contrôles XAML UWP directement à votre application de bureau à l’aide d’une île XAML. Îles XAML sont actuellement disponibles sous forme d’un version préliminaire pour développeurs. Bien que nous vous encourageons à les tester dans votre propre code prototype maintenant, nous ne recommandons pas que vous les utiliser dans le code de production pour l’instant. Ces API et les contrôles continuera à mûrir et stabiliser dans les futures versions de Windows. Pour en savoir plus sur XAML (îles), voir [les contrôles UWP dans les applications de bureau](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="the-design-pattern"></a>Modèle de conception

Pour afficher une interface utilisateur XAML, procédez comme suit:

:one: [Configurez votre solution](#solution-setup)

:two: [Créez une interface utilisateur avec XAML](#xaml-UI)

:three: [Ajoutez une extension de protocole au projet UWP](#protocol)

:four: [Démarrez l’application UWP à partir de votre application de bureau](#start)

:five: [Dans le projet UWP, affichez la page que vous voulez](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>Configurez votre solution

Pour obtenir des instructions générales sur la façon de configurer votre solution, consultez la section [Tout d’abord, configurez votre solution](#setup) au début de ce guide.

Votre solution ressemblera à ceci:

![Solution d'interface utilisateur XAML](images/desktop-to-uwp/xaml-ui-solution.png)

Dans cet exemple, le projet Windows Forms est nommé **Landmarks** et le projet UWP qui contient l'interface utilisateur XAML est nommé **MapUI**.

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>Créez une interface utilisateur avec XAML

Ajoutez une interface utilisateur XAML à votre projet UWP Voici le code XAML d'une carte de base.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"    
                     HorizontalAlignment="Left"               
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>Ajoutez une extension de protocole

Dans l' **Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet de création de packages dans votre solution et ajoutez cette extension.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>    
```

Nommez le protocole, indiquez le nom de l’exécutable généré par le projet UWP et le nom de la classe de point d’entrée.

Vous pouvez également ouvrir le **package.appxmanifest** dans le concepteur, choisir l'onglet **Déclarations**, puis ajouter l’extension ici.

![declarations-tab](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Les contrôles de carte téléchargent des données à partir d’internet, donc, si vous en utilisez un, vous devez également ajouter la fonctionnalité «client internet» à votre manifeste.

<a id="start" />

### <a name="start-the-uwp-app"></a>Démarrez l'application UWP.

Tout d’abord, à partir de votre application de bureau créez un [Uri](https://msdn.microsoft.com/library/system.uri.aspx) qui inclut le nom du protocole et les paramètres que vous souhaitez passer à l’application UWP. Appelez ensuite la méthode [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync).

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>Analysez les paramètres et affichez une page

Dans la classe **Application** de votre projet UWP, remplacez le gestionnaire d’événements **OnActivated**. Si l’application est activée par votre protocole, analysez les paramètres, puis ouvrez la page que vous voulez.

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

Remplacez la méthode ``OnNavigatedTo`` pour utiliser les paramètres transmis à la page. Dans ce cas, nous allons utiliser la latitude et la longitude qui ont été transmises à cette page pour afficher un emplacement dans une carte.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

### <a name="similar-samples"></a>Exemples similaires

[Ajout d’une expérience utilisateur XAML UWP aux applications VB6](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

[Exemple Northwind: exemple de bout en bout pour une interface utilisateur UWA et du code hérité Win32](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Exemple Northwind: application UWP se connectant à SQLServer](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>Fournir des services à d’autres applications

Vous ajoutez un service que d'autres applications peuvent utiliser. Par exemple, vous pouvez ajouter un service qui offre à d’autres applications un accès contrôlé à la base de données derrière votre application. En implémentant une tâche en arrière-plan, les applications peuvent atteindre le service même si votre application de bureau n’est pas en cours d’exécution.

Voici un exemple qui permet de le faire.

![adaptive-design](images/desktop-to-uwp/winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>Examinons de près cette application

:heavy_check_mark: [Obtenir l’application](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [Parcourir le code](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>Modèle de conception

Pour fournir un service, procédez comme suit:

:one: [Implémenter le service d’application](#appservice)

:two: [Ajoutez une extension de service d’application](#extension)

:three: [Tester le service d’application](#test)

<a id="appservice" />

### <a name="implement-the-app-service"></a>Implémenter le service d’application

C'est ici que vous allez valider et traiter les demandes des autres applications. Ajoutez ce code à un composant Windows Runtime dans votre solution.

```csharp
public sealed class AppServiceTask : IBackgroundTask
{
    private BackgroundTaskDeferral backgroundTaskDeferral;
 
    public void Run(IBackgroundTaskInstance taskInstance)
    {
        this.backgroundTaskDeferral = taskInstance.GetDeferral();
        taskInstance.Canceled += OnTaskCanceled;
        var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        details.AppServiceConnection.RequestReceived += OnRequestReceived;
    }
 
    private async void OnRequestReceived(AppServiceConnection sender,
                                         AppServiceRequestReceivedEventArgs args)
    {
        var messageDeferral = args.GetDeferral();
        ValueSet message = args.Request.Message;
        string id = message["ID"] as string;
        ValueSet returnData = DataBase.GetData(id);
        await args.Request.SendResponseAsync(returnData);
        messageDeferral.Complete();
    }
 
 
    private void OnTaskCanceled(IBackgroundTaskInstance sender,
                                BackgroundTaskCancellationReason reason)
    {
        if (this.backgroundTaskDeferral != null)
        {
            this.backgroundTaskDeferral.Complete();
        }
    }
}
```

<a id="extension" />

### <a name="add-an-app-service-extension-to-the-packaging-project"></a>Ajoutez une extension de service d’application au projet de création de packages

Ouvrez le fichier **package.appxmanifest** du projet de création de package, puis ajoutez une extension de service d’application à la ``<Application>`` élément.

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="AppServiceComponent.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```
Nommez le service d’application et indiquez le nom de la classe de point d’entrée. Il s'agit de la classe dans laquelle vous avez implémenté le service.

<a id="test" />

### <a name="test-the-app-service"></a>Tester le service d’application

Testez votre service en l'appelant à partir d’une autre application. Ce code peut être une application de bureau par exemple, une application Windows forms ou une autre application UWP.

> [!NOTE]
> Ce code fonctionne uniquement si vous avez correctement défini la propriété ``PackageFamilyName`` de la classe ``AppServiceConnection``. Vous pouvez obtenir ce nom en appelant ``Windows.ApplicationModel.Package.Current.Id.FamilyName`` dans le contexte du projet UWP. Consultez [Créer et utiliser un service d’application](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

```csharp
private async void button_Click(object sender, RoutedEventArgs e)
{
    AppServiceConnection dataService = new AppServiceConnection();
    dataService.AppServiceName = "com.microsoft.samples.winforms";
    dataService.PackageFamilyName = "Microsoft.SDKSamples.WinformWithAppService";
 
    var status = await dataService.OpenAsync();
    if (status == AppServiceConnectionStatus.Success)
    {
        string id = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("ID", id);
        AppServiceResponse response = await dataService.SendMessageAsync(message);
 
        if (response.Status == AppServiceResponseStatus.Success)
        {
            if (response.Message["Status"] as string == "OK")
            {
                DisplayResult(response.Message["Result"]);
            }
        }
    }
}
```

En savoir plus sur les services d’application: [Créer et utiliser un service d’application](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

### <a name="similar-samples"></a>Exemples similaires

[Exemple de pont de service d’application](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[Exemple de pont de service d’application avec une application Win32 C++](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[Application MFC qui reçoit des notifications Push](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>Faire de votre application de bureau une cible de partage

Vous pouvez faire de votre application de bureau une cible de partage afin que les utilisateurs puissent facilement partager des données (par exemple, des images) à partir d’autres applications qui prennent en charge le partage.

Par exemple, les utilisateurs peuvent choisir votre application pour partager des photos à partir de Microsoft Edge, l’application Photos. Voici un exemple d’application WPF disposant de cette fonctionnalité.

![cible de partage](images/desktop-to-uwp/share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>Examinons de près cette application

:heavy_check_mark: [Obtenir l’application](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [Parcourir le code](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>Modèle de conception

Pour faire de votre application une cible de partage, procédez comme suit:

:one: [Ajouter une extension de cible de partage](#share-extension)

:two: [Remplacer le gestionnaire d’événements OnNavigatedTo](#override)

<a id="share-extension" />

### <a name="add-a-share-target-extension"></a>Ajouter une extension de cible de partage

Dans l' **Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet de création de packages dans votre solution et ajoutez l’extension.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="ShareTarget.App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Indiquez le nom de l'exécutable généré par le projet UWP et le nom de la classe de point d’entrée. Vous devez également spécifier les types de fichiers qui peuvent être partagés avec votre application.

<a id="override" />

### <a name="override-the-onnavigatedto-event-handler"></a>Remplacer le gestionnaire d’événements OnNavigatedTo

Remplacez le gestionnaire d’événements **OnNavigatedTo** dans la classe **Application** de votre projet UWP.

Ce gestionnaire d’événements est appelé lorsque les utilisateurs choisissent votre application pour partager leurs fichiers.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
  this.shareOperation = (ShareOperation)e.Parameter;
  if (this.shareOperation.Data.Contains(StandardDataFormats.StorageItems))
  {
      this.sharedStorageItems =
        await this.shareOperation.Data.GetStorageItemsAsync();
       
      foreach (StorageFile item in this.sharedStorageItems)
      {
          ProcessSharedFile(item);
      }
  }
}
```

## <a name="create-a-background-task"></a>Créer une tâche en arrière-plan

Vous ajouter une tâche en arrière-plan pour exécuter le code, même lorsque l'application est suspendue. Les tâches en arrière-plan sont idéales pour les petites tâches qui ne nécessitent pas d'interaction d'utilisateur. Par exemple, votre tâche peut télécharger des e-mails, afficher une notification toast à propos d'un message de chat entrant, ou réagir à un changement dans une condition du système.

Voici un exemple d’application WPF qui inscrit une tâche en arrière-plan.

![tâche en arrière-plan](images/desktop-to-uwp/sample-background-task.png)

La tâche réalise une requête HTTP et mesure le délai nécessaire à l'obtention d'une réponse. Vos tâches seront probablement plus intéressantes, mais cet exemple est idéal pour apprendre les mécanismes de base d'une tâche en arrière-plan.

### <a name="have-a-closer-look-at-this-app"></a>Examinons de près cette application

:heavy_check_mark: [Parcourir le code](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)

### <a name="the-design-pattern"></a>Le modèle de conception

Pour créer un service en arrière-plan, procédez comme suit:

:one: [Implémenter la tâche en arrière-plan](#implement-task)

:two: [Configurer la tâche en arrière-plan](#configure-background-task)

:three: [Enregistrer la tâche en arrière-plan](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>Implémenter la tâche en arrière-plan

Implémentez la tâche en arrière-plan en ajoutant le code au projet de composant WindowsRuntime.

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>Configurer la tâche en arrière-plan

Dans le Concepteur de manifeste, ouvrez le fichier **package.appxmanifest** du projet de création de packages dans votre solution.

Dans l'onglet **Déclarations**, ajoutez une déclaration **Tâches en arrière-plan**.

![Option de tâche en arrière-plan](images/desktop-to-uwp/background-task-option.png)

Ensuite, choisissez les propriétés souhaitées. Notre exemple utilise la propriété **Minuteur**.

![Propriété Minuteur](images/desktop-to-uwp/timer-property.png)

Fournissez le nom complet de la classe dans votre composant WindowsRuntime qui implémente la tâche en arrière-plan.

![Propriété Minuteur](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

Ajoutez le code à votre projet d'application de bureau qui inscrit la tâche en arrière-plan.

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```
## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
