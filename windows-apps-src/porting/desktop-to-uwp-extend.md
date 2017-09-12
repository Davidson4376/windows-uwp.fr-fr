---
author: normesta
Description: "Étendre votre application de bureau avec des interfaces utilisateur et des composants Windows"
Search.Product: eADQiWindows 10XVcnh
title: "Étendre votre application de bureau avec des interfaces utilisateur et des composants Windows"
ms.author: normesta
ms.date: 07/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: fce6076f07957b50e83cf80e8d12350630b99456
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2017
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Étendre votre application de bureau avec des composants UWP modernes

Certaines expériences Windows10 (par exemple: une page d'interface utilisateur tactile) doivent s'exécuter à l'intérieur d'un conteneur d'application moderne. Si vous souhaitez ajouter ces expériences, étendez votre application de bureau avec un composant UWP.

Dans de nombreux cas, vous pouvez appeler des API UWP directement à partir de votre application de bureau. Par conséquent, avant de consulter ce guide, consultez [Optimisation pour Windows10](desktop-to-uwp-enhance.md).

>[!NOTE]
>Ce guide suppose que vous avez créé un package d’application Windows pour votre application de bureau à l’aide de Pont du bureau. Si ce n'est pas encore fait, consultez [Pont du bureau](desktop-to-uwp-root.md).

Si vous êtes prêt, commençons.

## <a name="show-a-modern-xaml-ui"></a>Afficher une interface utilisateur XAML moderne

Dans le cadre du flux de votre application, vous pouvez incorporer des interfaces utilisateur XAML modernes dans votre application de bureau. Ces interfaces utilisateur s'adaptent naturellement à différentes résolutions et tailles d’écran et prennent en charge des modèles interactifs modernes tels que la fonctionnalité tactile et l’entrée manuscrite.

Par exemple, avec une petite quantité de balisage XAML, vous pouvez offrir aux utilisateurs des fonctionnalités puissantes de visualisation cartographique.

Cette image montre une application VB6 qui ouvre une interface utilisateur XAML moderne contenant un contrôle de carte.

![adaptive-design](images\desktop-to-uwp\extend-xaml-ui.png)

### <a name="have-a-closer-look-at-this-app"></a>Examinons de près cette application

:heavy_check_mark: [Regarder une vidéo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Add-a-XAML-UI-and-Toast-Notification-to-a-VB6-Application-OsJHC7WhD_8006218965)

:heavy_check_mark: [Obtenir l’application](https://www.microsoft.com/en-us/store/p/vb6-app-with-xaml-sample/9n191ncxf2f6)

:heavy_check_mark: [Parcourir le code](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

### <a name="the-design-pattern"></a>Modèle de conception

Pour afficher une interface utilisateur XAML, procédez comme suit:

:one: [Ajouter un projet UWP à votre solution](#project)

:two: [Ajoutez une extension de protocole à ce projet](#protocol)

:three: [Démarrez l’application UWP à partir de votre application de bureau](#start)

:four: [Dans le projet UWP, affichez la page que vous voulez](#parse)

<span id="project" />
### <a name="add-a-uwp-project"></a>Ajouter un projet UWP

Ajoutez un projet **Application vide (Windows universelle)** à votre solution.

<span id="protocol" />
### <a name="add-a-protocol-extension"></a>Ajouter une extension de protocole

Dans l'**Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet et ajoutez l’extension.

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

![declarations-tab](images\desktop-to-uwp\protocol-properties.png)



> [!NOTE]
> Les contrôles de carte téléchargent des données à partir d’internet, donc, si vous en utilisez un, vous devez également ajouter la fonctionnalité «client internet» à votre manifeste.

<span id="start" />
### <a name="start-the-uwp-app"></a>Démarrez l'application UWP.

Tout d’abord, créez un [Uri](https://msdn.microsoft.com/library/system.uri.aspx) qui inclut le nom du protocole et les paramètres que vous souhaitez passer à l’application UWP. Appelez ensuite la méthode [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_).

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

<span id="parse" />
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

![adaptive-design](images\desktop-to-uwp\winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>Examinons de près cette application

:heavy_check_mark: [Regarder une vidéo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Expose-an-AppService-from-a-Windows-Forms-Data-Application-GiqNS7WhD_706218965)

:heavy_check_mark: [Obtenir l’application](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [Parcourir le code](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>Modèle de conception

Pour fournir un service, procédez comme suit:

:one: [Ajoutez un composant Windows Runtime](#component)

:two: [Ajoutez une extension de service d’application](#extension)

:three: [Implémentez le service d’application](#appservice)

:four: [Testez le service d’application](#test)

<span id="component" />

### <a name="add-a-windows-runtime-component"></a>Ajouter un composant Windows Runtime

Ajoutez un projet **composant Windows Runtime (Windows universel)** à votre solution.

Référencez ensuite le projet de ce composant runtime à partir de votre projet de création de packages UWP.

<span id="extension" />
### <a name="add-an-app-service-extension"></a>Ajouter une extension de service d’application

Dans l'**Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** de votre projet de création de packages et ajoutez une extension de service d’application.

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="MyAppService.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```

Nommez le service d’application et indiquez le nom de la classe de point d’entrée. Il s’agit de la classe que vous allez utiliser pour implémenter votre service d’application.

<span id="appservice" />
### <a name="implement-the-app-service"></a>Implémenter le service d’application

C'est là que vous allez valider et traiter les demandes des autres applications.

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

<span id="test" />
### <a name="test-the-app-service"></a>Tester le service d’application

Testez votre service en l'appelant à partir d’une autre application.

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
        string result = "";
 
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

![cible de partage](images\desktop-to-uwp\share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>Examinons de près cette application

:heavy_check_mark: [Regarder une vidéo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Make-a-WPF-Application-a-Share-Target-xd6Fu6WhD_8406218965)

:heavy_check_mark: [Obtenir l’application](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [Parcourir le code](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>Modèle de conception

Pour faire de votre application une cible de partage, procédez comme suit:

:one: [Ajoutez un projet UWP à votre solution](#project2)

:two: [Ajoutez une extension de cible de partage](#share-extension)

:three: [Remplacez le gestionnaire d’événements OnNavigatedTo](#override)

<span id="project2" />
### <a name="add-a-uwp-project-to-your-solution"></a>Ajouter un projet UWP à votre solution

Ajoutez un projet **Application vide (Windows universelle)** à votre solution.

<span id="share-extension" />
### <a name="add-a-share-target-extension"></a>Ajouter une extension de cible de partage

Dans l'**Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet et ajoutez l’extension.

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

<span id="override" />
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

## <a name="support-and-feedback"></a>Support et commentaires

**Trouver des réponses aux questions spécifiques**

Notre équipe contrôle ces [balises StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Transmettre des commentaires ou suggérer des fonctionnalités**

Voir [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Envoyer vos commentaires concernant cet article**

Utilisez la section remarques ci-dessous.
