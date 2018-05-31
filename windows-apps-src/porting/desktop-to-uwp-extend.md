---
author: normesta
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: Étendre votre application de bureau avec des interfaces utilisateur et des composants Windows
ms.author: normesta
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef20366092a5f284c39f4e43d4412c69b60f12fa
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691328"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Étendre votre application de bureau avec des composants UWP modernes

Certaines expériences Windows10 (par exemple: une page d'interface utilisateur tactile) doivent s'exécuter à l'intérieur d'un conteneur d'application moderne. Si vous souhaitez ajouter ces expériences, étendez votre application de bureau avec des composant de projets UWP et Windows Runtime.

Dans de nombreux cas, vous pouvez appeler des API UWP directement à partir de votre application de bureau. Par conséquent, avant de consulter ce guide, consultez [Optimisation pour Windows10](desktop-to-uwp-enhance.md).

>[!NOTE]
>Ce guide suppose que vous avez créé un package d’application Windows pour votre application de bureau à l’aide de Pont du bureau. Si ce n'est pas encore fait, consultez [Pont du bureau](desktop-to-uwp-root.md).

Si vous êtes prêt, commençons.

## <a name="first-setup-your-solution"></a>Tout d’abord, configurez votre Solution

Ajoutez un ou plusieurs projets UWP et composants d'exécution à votre solution.

Commencez avec une solution qui contient un **Projet de création de packages d’Application Windows** avec une référence à votre application de bureau.

Cette image illustre un exemple de solution.

![Démarrage étendu d'un projet](images/desktop-to-uwp/extend-start-project.png)

Si votre solution ne contient aucun projet de mise en package, consultez [Mise en package de votre application à l’aide de VisualStudio](desktop-to-uwp-packaging-dot-net.md).

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

Cette image montre une application VB6 qui ouvre une interface utilisateur XAML moderne contenant un contrôle de carte.

![adaptive-design](images/desktop-to-uwp/extend-xaml-ui.png)

### <a name="have-a-closer-look-at-this-app"></a>Examinons de près cette application

:heavy_check_mark: [Obtenir l’application](https://www.microsoft.com/en-us/store/p/vb6-app-with-xaml-sample/9n191ncxf2f6)

:heavy_check_mark: [Parcourir le code](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

### <a name="the-design-pattern"></a>Modèle de conception

Pour afficher une interface utilisateur XAML, procédez comme suit:

:one: [Ajouter une extension de protocole à ce projet](#protocol)

:two: [Démarrer l’application UWP à partir de votre application de bureau](#start)

:three: [Dans le projet UWP, afficher la page que vous souhaitez](#parse)

<a id="protocol" />

### <a name="add-a-protocol-extension"></a>Ajouter une extension de protocole

Dans l'**Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet UWP dans votre solution et ajoutez cette extension.

```xml
<Extensions>
      <uap:Extension
          Category="windows.protocol"
          Executable="MapUI.exe"
          EntryPoint=" MapUI.App">
        <uap:Protocol Name="desktopbridgemapsample" />
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

Voici un exemple simple en C#.

```csharp

private async void showMap(double lat, double lon)
{
    string str = "desktopbridgemapsample://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

    if (success)
    {
        // URI launched
    }
    else
    {
        // URI launch failed
    }
}
```
Dans notre exemple, nous faisons quelque chose d’un peu plus indirect. Nous avons encapsulé l’appel dans une fonction d’interopérabilité qui peut être appelée par VB6, nommée ``LaunchMap``. Cette fonction est écrite en C++.

Voici le bloc VB:

```VB
Private Declare Function LaunchMap Lib "UWPWrappers.dll" _
  (ByVal lat As Double, ByVal lon As Double) As Boolean
 
Private Sub EiffelTower_Click()
    LaunchMap 48.858222, 2.2945
End Sub
```

Voici la fonction C++:

```C++

DllExport bool __stdcall LaunchMap(double lat, double lon)
{
  try
  {
    String ^str = ref new String(L"desktopbridgemapsample://");
    Uri ^uri = ref new Uri(
      str + L"location?lat=" + lat.ToString() + L"&?lon=" + lon.ToString());
 
    // now launch the UWP component
    Launcher::LaunchUriAsync(uri);
  }
  catch (Exception^ ex) { return false; }
  return true;
}

```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>Analyser les paramètres et afficher une page

Dans la classe **Application** de votre projet UWP, remplacez le gestionnaire d’événements **OnActivated**. Si l’application est activée par votre protocole, analysez les paramètres, puis ouvrez la page que vous voulez.

```C++
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ e)
{
  if (e->Kind == ActivationKind::Protocol)
  {
    ProtocolActivatedEventArgs^ protocolArgs = (ProtocolActivatedEventArgs^)e;
    Uri ^uri = protocolArgs->Uri;
    if (uri->SchemeName == "desktopbridgemapsample")
    {
      Frame ^rootFrame = ref new Frame();
      Window::Current->Content = rootFrame;
      rootFrame->Navigate(TypeName(MainPage::typeid), uri->Query);
      Window::Current->Activate();
    }
  }
}
```

### <a name="similar-samples"></a>Exemples similaires

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

### <a name="add-an-app-service-extension-to-the-uwp-project"></a>Ajouter une extension de service d’application au projet UWP

Ouvrez le fichier **package.appxmanifest** du project UWP, puis ajoutez une extension de service d'application à l'élément ``<Application>``.

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

Testez votre service en l'appelant à partir d’une autre application. Ce code peut désigner une application de bureau telle qu'une application de formulaire Windows ou toute autre application UWP.

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

Par exemple, les utilisateurs peuvent choisir votre application pour partager des photos à partir de MicrosoftEdge ou l’application Photos. Voici un exemple d’application WPF disposant de cette fonctionnalité.

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

Dans l'**Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet UWP dans votre solution et ajoutez l'extension.

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

Voici l'exemple d'une application WPF qui inscrit une tâche en arrière-plan.

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

Dans le concepteur de manifeste, ouvrez le fichier **package.appxmanifest** du projet UWP dans votre solution.

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
