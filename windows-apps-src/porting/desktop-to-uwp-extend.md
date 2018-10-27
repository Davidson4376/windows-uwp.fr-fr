---
author: normesta
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: Étendre votre application de bureau avec des interfaces utilisateur et des composants Windows
ms.author: normesta
ms.date: 06/08/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1806a24d2f84b5d3e1eeff6c5b3f7900360de3e4
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5683490"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Étendre votre application de bureau avec des composants UWP modernes

Certaines expériences Windows10 (par exemple: une page d'interface utilisateur tactile) doivent s'exécuter à l'intérieur d'un conteneur d'application moderne. Si vous souhaitez ajouter ces expériences, étendez votre application de bureau avec des composant de projets UWP et Windows Runtime.

Dans de nombreux cas, vous pouvez appeler APIs Windows Runtime directement à partir de votre application de bureau, par conséquent, avant de consulter ce guide, consultez [optimisation pour Windows 10](desktop-to-uwp-enhance.md).

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

### <a name="configure-the-desktop-application"></a>Configurer l’application de bureau

Assurez-vous que votre application de bureau comporte des références aux fichiers dont vous avez besoin d’appeler APIs Windows Runtime.

Pour ce faire, consultez la section [tout d’abord, configurez votre projet](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project) de la rubrique [améliorer votre application de bureau pour Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project).

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

### <a name="build-your-solution"></a>Générez votre solution

Générez votre solution pour vous assurer qu’aucune erreur n’apparaît. Si vous recevez des erreurs, ouvrez le **Gestionnaire de Configuration** et vous assurer que vos projets ciblent la même plate-forme.

![Gestionnaire de configuration](images/desktop-to-uwp/config-manager.png)

Examinons quelques-unes des possibilités concernant vos projets UWP et les composants d'exécution.

## <a name="show-a-modern-xaml-ui"></a>Afficher une interface utilisateur XAML moderne

Dans le cadre du flux de votre application, vous pouvez incorporer des interfaces utilisateur XAML modernes dans votre application de bureau. Ces interfaces utilisateur s'adaptent naturellement à différentes résolutions et tailles d’écran et prennent en charge des modèles interactifs modernes tels que la fonctionnalité tactile et l’entrée manuscrite.

Par exemple, avec une petite quantité de balisage XAML, vous pouvez offrir aux utilisateurs des fonctionnalités puissantes de visualisation cartographique.

Cette image montre une application Windows Forms qui ouvre une interface utilisateur XAML moderne contenant un contrôle de carte.

![adaptive-design](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>Cet exemple montre une UI XAML en ajoutant un projet UWP à la solution. Qui est l’approche pris en charge stable affichant des interfaces utilisateur XAML dans une application de bureau. L’alternative à cette approche consiste à ajouter des contrôles UWP XAML directement à votre application de bureau à l’aide d’une île XAML. Îles XAML sont actuellement disponibles sous forme d’un version préliminaire pour développeurs. Bien que nous vous encourageons à les tester dans votre propre code prototype maintenant, nous ne recommandons pas que vous les utiliser dans le code de production pour l’instant. Ces API et les contrôles continuera à mûrir et stabiliser dans les futures versions de Windows. Pour en savoir plus sur XAML (îles), voir [les contrôles UWP dans les applications de bureau](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

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

Dans le code-behind de votre page XAML, remplacez le ``OnNavigatedTo`` méthode pour utiliser les paramètres transmis à la page. Dans ce cas, nous allons utiliser la latitude et la longitude qui ont été transmises à cette page pour afficher un emplacement dans une carte.

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

## <a name="making-your-desktop-application-a-share-target"></a>Faire de votre application de bureau une cible de partage

Vous pouvez faire de votre application de bureau une cible de partage afin que les utilisateurs puissent facilement partager des données (par exemple, des images) à partir d’autres applications qui prennent en charge le partage.

Par exemple, les utilisateurs peuvent choisir votre application pour partager des photos à partir de Microsoft Edge, l’application Photos. Voici un exemple d’application WPF disposant de cette fonctionnalité.

![cible de partage](images/desktop-to-uwp/share-target.png).

Voir l’exemple complet [ici](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>Modèle de conception

Pour faire de votre application une cible de partage, procédez comme suit:

:one: [Ajouter une extension de cible de partage](#share-extension)

: two: [Remplacer le Gestionnaire d’événements OnShareTargetActivated](#override)

: trois: [Ajouter les extensions de bureau au projet UWP](#desktop-extensions)

: quatre: [Ajouter l’extension de processus de confiance totale](#full-trust)

: cinq: [Modifier l’application de bureau pour obtenir le fichier partagé](#modify-desktop)

<a id="share-extension" />

Les étapes suivantes  

### <a name="add-a-share-target-extension"></a>Ajouter une extension de cible de partage

Dans l' **Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet de création de packages dans votre solution et ajoutez l’extension de cible de partage.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Indiquez le nom de l'exécutable généré par le projet UWP et le nom de la classe de point d’entrée. Ce balisage suppose que le nom du fichier exécutable de votre application UWP est `ShareTarget.exe`.

Vous devez également spécifier les types de fichiers qui peuvent être partagés avec votre application. Dans cet exemple, nous apportons l’application de bureau [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) une cible de partage pour les images bitmap afin de spécifier `Bitmap` pour le type de fichier pris en charge.

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>Remplacer le Gestionnaire d’événements OnShareTargetActivated

Remplacer le Gestionnaire d’événements **OnShareTargetActivated** dans la classe **d’application** de votre projet UWP.

Ce gestionnaire d’événements est appelé lorsque les utilisateurs choisissent votre application pour partager leurs fichiers.

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```
Dans ce code, nous enregistrons l’image qui est partagé par l’utilisateur dans un dossier de stockage local des applications. Plus tard, nous allons modifier l’application de bureau pour extraire des images à partir de ce dossier. L’application de bureau pour ce faire, car il est inclus dans le même package en tant que l’application UWP.

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Ajouter des extensions de bureau au projet UWP

Ajoutez l’extension **d’Extensions de bureau Windows pour UWP** au projet d’application UWP.

![extension de bureau](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>Ajoutez l’extension de processus de confiance totale

Dans l' **Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet de création de packages dans votre solution et puis ajouter l’extension du processus de confiance totale en regard de l’extension de cible de partage que vous avez ajouté précédemment ce fichier.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Cette extension activera l’application UWP démarrer l’application de bureau pour laquelle vous souhaitez que le partage un fichier. Dans l’exemple, nous faisons référence au fichier exécutable de l’application de bureau [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) .

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Modifier l’application de bureau pour obtenir le fichier partagé

Modifier votre application de bureau pour rechercher et traiter le fichier partagé. Dans cet exemple, l’application UWP stocké le fichier partagé dans le dossier de données d’application locale. Par conséquent, nous serait modifier l’application de bureau [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) à tirer photos à partir de ce dossier.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```
Pour les instances de l’application de bureau qui sont déjà en cours par l’utilisateur, nous pouvons également gérer l’événement [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher?view=netframework-4.7.2) et transmettre le chemin d’accès à l’emplacement du fichier. De cette façon toutes les instances ouvertes de l’application de bureau affiche la photo partagée.

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>Créer une tâche en arrière-plan

Vous ajouter une tâche en arrière-plan pour exécuter le code, même lorsque l'application est suspendue. Les tâches en arrière-plan sont idéales pour les petites tâches qui ne nécessitent pas d'interaction d'utilisateur. Par exemple, votre tâche peut télécharger des e-mails, afficher une notification toast à propos d'un message de chat entrant, ou réagir à un changement dans une condition du système.

Voici un exemple d’application WPF qui inscrit une tâche en arrière-plan.

![tâche en arrière-plan](images/desktop-to-uwp/sample-background-task.png)

La tâche réalise une requête HTTP et mesure le délai nécessaire à l'obtention d'une réponse. Vos tâches seront probablement plus intéressantes, mais cet exemple est idéal pour apprendre les mécanismes de base d'une tâche en arrière-plan.

Voir l’exemple complet [ici](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

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
