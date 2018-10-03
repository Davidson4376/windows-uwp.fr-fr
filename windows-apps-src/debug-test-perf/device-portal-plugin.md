---
author: PatrickFarley
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Écrire un plug-in personnalisé pour DevicePortal
description: Découvrez comment écrire une application UWP qui utilise WindowsDevicePortal pour héberger une page web et fournir des informations de diagnostic.
ms.author: pafarley
ms.date: 03/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portail d’appareil
ms.localizationpriority: medium
ms.openlocfilehash: 6bf511a910a266877d54a014f087c1748ff25838
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4258400"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Écrire un plug-in personnalisé pour DevicePortal

Découvrez comment écrire une application UWP qui utilise WindowsDevicePortal pour héberger une page web et fournir des informations de diagnostic.

À partir de la version CreatorsUpdate, vous pouvez utiliser DevicePortal pour héberger les interfaces de diagnostic de votre application. Cet article décrit les trois tâches requises pour la création d’une fonctionnalité DevicePortalProvider pour votre application: les modifications du fichier appxmanifest, la configuration de la connexion de votre application au service DevicePortal et la gestion d’une requête entrante. Un exemple d’application vous est également fourni pour simplifier la prise en main (bientôt disponible). 

## <a name="create-a-new-uwp-app-project"></a>Créer un projet d’application UWP
Dans ce guide, nous allons créer tous les éléments dans une seule solution par souci de simplicité.

Dans Microsoft VisualStudio2017, créez un projet d’application UWP. Accédez à Fichier> Nouveau projet, puis sélectionnez Modèles> VisualC#> Windowsuniversel> Application vide (Windowsuniversel). Nommez votre application «DevicePortalProvider». Il s’agira de l’application qui contient le service d’application. Veillez à choisir le Kit de développement logiciel (SDK) CreatorsUpdate à des fins de prise en charge.  Vous devrez peut-être mettre à jour VisualStudio ou installer le nouveau Kit de développement logiciel (SDK); pour plus d’informations, consultez [ce billet de blog](https://blogs.windows.com/buildingapps/2017/04/05/updating-tooling-windows-10-creators-update/). 

## <a name="add-the-deviceportalprovider-extension-to-your-packageappxmanifest-file"></a>Ajouter l’extension devicePortalProvider à votre fichier package.appxmanifest
Vous devrez ajouter du code à votre fichier *package.appxmanifest* pour faire fonctionner votre application en tant que plug-in DevicePortal. Commencez par ajouter les définitions d’espace de noms ci-après au début du fichier. Ajoutez également ces définitions à l’attribut `IgnorableNamespaces`.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

Pour déclarer votre application en tant que fournisseur DevicePortal, vous devez créer un service d’application, ainsi qu’une extension de fournisseur DevicePortal utilisant ce service. Ajoutez à la fois l’extension windows.appService et l’extension windows.devicePortalProvider dans l’élément `Extensions` sous `Application`. Vérifiez que les attributs `AppServiceName` correspondent dans les deux extensions. Cette opération indique au service DevicePortal que ce service d’application peut être lancé pour gérer les requêtes sur l’espace de noms du gestionnaire. 

```xml
...   
<Application 
    Id="App" 
    Executable="$targetnametoken$.exe"
    EntryPoint="DevicePortalProvider.App">
    ...
    <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MySampleProvider.SampleProvider">
            <uap:AppService Name="com.sampleProvider.wdp" />
        </uap:Extension>
        <uap4:Extension Category="windows.devicePortalProvider">
            <uap4:DevicePortalProvider 
                DisplayName="My Device Portal Provider Sample App" 
                AppServiceName="com.sampleProvider.wdp" 
                HandlerRoute="/MyNamespace/api/" />
        </uap4:Extension>
    </Extensions>
</Application>
...
```

L’attribut `HandlerRoute` référence l’espace de noms REST revendiqué par votre application. Toutes les requêtes HTTP sur cet espace de noms (implicitement suivi d’un caractère générique) qui sont reçues par le service DevicePortal seront envoyées à votre application pour être gérées. Dans ce cas, toutes les requêtes HTTP correctement authentifiées à destination de `<ip_address>/MyNamespace/api/*` seront envoyées à votre application. Les conflits entre les itinéraires de gestionnaire sont réglés par le biais d’une vérification du type «priorité à l’itinéraire le plus long»: autrement dit, l’itinéraire qui correspond le mieux aux requêtes est sélectionné. Par exemple, une requête adressée à «/MyNamespace/api/foo» sera mise en correspondance avec un fournisseur contenant «/MyNamespace/api» plutôt qu’avec un fournisseur indiquant simplement «/MyNamespace».  

Cette approche requiert deux nouvelles fonctionnalités, que vous devrez également ajouter à votre fichier *package.appxmanifest*.

```xml
...
<Capabilities>
    ...
    <Capability Name="privateNetworkClientServer" />
    <rescap:Capability Name="devicePortalProvider" />
</Capabilities>
...
```

> [!NOTE]
> La fonctionnalité «devicePortalProvider» est restreinte («rescap»), ce qui signifie que vous devez obtenir une approbation préalable auprès du WindowsStore avant de pouvoir publier votre application sur ce dernier. Toutefois, ceci ne vous empêche pas de tester votre application localement par le biais d’un chargement indépendant. Pour plus d’informations sur les fonctionnalités restreintes, consultez l’article [Déclarations des fonctionnalités d’application](https://msdn.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities).

## <a name="set-up-your-background-task-and-winrt-component"></a>Configurer votre tâche en arrière-plan et le composant WinRT
Pour la configuration de la connexion DevicePortal, votre application doit raccorder une connexion de service d’application à partir du service DevicePortal à l’instance DevicePortal en cours d’exécution au sein de votre application. Pour effectuer cette opération, ajoutez un nouveau composantWinRT à votre application avec une classe qui implémente [**IBackgroundTask**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtask).

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

Assurez-vous que son nom correspond à l’espace de noms et au nom de classe configurés par le point d’entrée du service d’application («MySampleProvider.SampleProvider»). Lorsque vous adressez votre première requête à votre fournisseur DevicePortal, DevicePortal remise (stash) la requête, lance la tâche en arrière-plan de votre application, appelle sa méthode **Run** et transmet une interface [**IBackgroundTaskInstance**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance). Votre application utilise ensuite cette dernière pour configurer une instance [**DevicePortalConnection**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection).

```csharp
// Implement background task handler with a DevicePortalConnection
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take a deferral to allow the background task to continue executing 
    this.taskDeferral = taskInstance.GetDeferral();
    taskInstance.Canceled += TaskInstance_Canceled;

    // Create a DevicePortal client from an AppServiceConnection 
    var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    var appServiceConnection = details.AppServiceConnection;
    this.devicePortalConnection = DevicePortalConnection.GetForAppServiceConnection(appServiceConnection);

    // Add Closed, RequestReceived handlers 
    devicePortalConnection.Closed += DevicePortalConnection_Closed;
    devicePortalConnection.RequestReceived += DevicePortalConnection_RequestReceived;
}
```

L’application doit gérer deux événements pour exécuter la boucle de gestion des requêtes: l’événement **Closed**, qui se déclenche chaque fois que le service DevicePortal s’arrête, et l’événement [**RequestReceived**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs), qui expose les requêtesHTTP entrantes et assure la principale fonctionnalité du fournisseur DevicePortal. 

## <a name="handle-the-requestreceived-event"></a>Gérer l’événement RequestReceived
L’événement **RequestReceived** se déclenche chaque fois qu’une requêteHTTP est effectuée sur l’itinéraire du gestionnaire spécifié de votre plug-in. La boucle de gestion des requêtes pour les fournisseurs DevicePortal est comparable à celle de NodeJSExpress: les objets de requête et de réponse sont fournis en même temps que l’événement, et le gestionnaire répond en remplissant l’objet de réponse. Dans les fournisseurs DevicePortal, l’événement **RequestReceived** et ses gestionnaires utilisent les objets [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httprequestmessage) et [**HttpResponseMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpresponsemessage).   

```csharp
// Sample RequestReceived echo handler: respond with an HTML page including the query and some additional process information. 
private void DevicePortalConnection_RequestReceived(DevicePortalConnection sender, DevicePortalConnectionRequestReceivedEventArgs args)
{
    var req = args.RequestMessage;
    var res = args.ResponseMessage;

    if (req.RequestUri.AbsolutePath.EndsWith("/echo"))
    {
        // construct an html response message
        string con = "<h1>" + req.RequestUri.AbsoluteUri + "</h1><br/>";
        var proc = Windows.System.Diagnostics.ProcessDiagnosticInfo.GetForCurrentProcess();
        con += String.Format("This process is consuming {0} bytes (Working Set)<br/>", proc.MemoryUsage.GetReport().WorkingSetSizeInBytes);
        con += String.Format("The process PID is {0}<br/>", proc.ProcessId);
        con += String.Format("The executable filename is {0}", proc.ExecutableFileName);
        res.Content = new HttpStringContent(con);
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
        res.StatusCode = HttpStatusCode.Ok;            
    }
    //...
}
```

Dans cet exemple de gestionnaire de requêtes, nous commençons par extraire les objets de requête et de réponse du paramètre *args*, puis nous créons une chaîne contenant l’URL de la requête et une mise en forme HTML supplémentaire. Cette chaîne est ajoutée à l’objet de réponse sous la forme d’une instance [**HttpStringContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpstringcontent). D’autres classes [**IHttpContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.ihttpcontent), telles que celles pour «String» et «Buffer», sont également autorisées.

La réponse est ensuite définie en tant que réponse HTTP et reçoit un code d’état200 (OK). Elle devrait s’afficher comme prévu dans le navigateur qui a effectué l’appel d’origine. Notez que lorsque le gestionnaire d’événements **RequestReceived** répond, le message de réponse est automatiquement renvoyé à l’agent utilisateur: aucune méthode «send» supplémentaire n’est nécessaire.

![Message de réponse DevicePortal](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>Fourniture de contenu statique
Le contenu statique peut être traité directement à partir d’un dossier de votre package, ce qui facilite sensiblement l’ajout d’une interface utilisateur à votre fournisseur.  Le moyen le plus simple de traiter du contenu statique consiste à créer dans votre projet un dossier de contenu pouvant être mappé sur une URL.

![Dossier de contenu statique DevicePortal](images/device-portal/plugin-static-content.png)
 
Ensuite, ajoutez un gestionnaire d’itinéraires à votre gestionnaire d’événements **RequestReceived** qui détecte les itinéraires de contenu statique et mappe une requête en conséquence.  

```csharp
if (req.RequestUri.LocalPath.ToLower().Contains("/www/")) {
    var filePath = req.RequestUri.AbsolutePath.Replace('/', '\\').ToLower();
    filePath = filePath.Replace("\\backgroundprovider", "")
    try {
        var fileStream = Windows.ApplicationModel.Package.Current.InstalledLocation.OpenStreamForReadAsync(filePath).GetAwaiter().GetResult();
        res.StatusCode = HttpStatusCode.Ok;
        res.Content = new HttpStreamContent(fileStream.AsInputStream());
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    } catch(FileNotFoundException e) {
        string con = String.Format("<h1>{0} - not found</h1>\r\n", filePath);
        con += "Exception: " + e.ToString();
        res.Content = new HttpStringContent(con);
        res.StatusCode = HttpStatusCode.NotFound;
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    }
}
```
Assurez-vous que tous les fichiers figurant dans le dossier de contenu sont marqués en tant que «Contenu» et sont définis sur «Copier si plus récent» ou sur «Toujours copier» dans le menu Propriétés de VisualStudio.  Cette opération garantit que les fichiers seront présents dans votre packageAppX lorsque vous le déploierez.  

![Configurer la copie des fichiers de contenu statique](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>Utilisation des ressources et des API DevicePortal existantes
Le contenu statique traité par un fournisseur DevicePortal est traité sur le même port que le service DevicePortal principal.  Cela signifie que vous pouvez référencer les éléments JS et CSS existants inclus dans DevicePortal avec de simples balises `<link>` et `<script>` dans votre code HTML. En règle générale, nous vous suggérons d’utiliser *rest.js*, qui inclut dans un wrapper la totalité des API REST DevicePortal principales dans un objet webbRest pratique, ainsi que le fichier *common.css*, qui vous permettra de styliser votre contenu pour l’adapter au reste de l’interface utilisateur de DevicePortal. La page *index.html* illustrée ci-dessous vous en fournit un exemple. Cette page utilise *rest.js* pour récupérer le nom de l’appareil et les processus en cours d’exécution à partir de DevicePortal. 

![Sortie du plug-in DevicePortal](images/device-portal/plugin-output.png)
 
Plus important encore, l’utilisation des méthodes HttpPost/DeleteExpect200 sur webbRest assurera automatiquement la [gestion des attaques de falsification de requête intersites (CSRF, Cross Site Request Forgery)](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) à votre intention, ce qui permet à votre page web d’appeler les API REST de modification d’état.  

> [!NOTE] 
> Le contenu statique fourni dans DevicePortal n’est pas protégé contre les changements importants. Bien que les API ne soient pas susceptibles de changer fréquemment, il peut arriver qu’elles le soient, notamment dans les fichiers *common.js* et *controls.js*, que votre fournisseur ne doit pas utiliser. 

## <a name="debugging-the-device-portal-connection"></a>Débogage de la connexion DevicePortal
Pour déboguer votre tâche en arrière-plan, vous devez modifier la façon dont VisualStudio exécute votre code. Pour inspecter le mode de gestion des requêtes HTTP par votre fournisseur, suivez la procédure ci-après de débogage d’une connexion de service d’application:

1.  Dans le menu Déboguer, sélectionnez Propriétés DevicePortalProvider. 
2.  Sous l’onglet Débogage, dans la section Action de démarrage, sélectionnez «Ne pas lancer, mais déboguer mon code au démarrage».  
![Passage du plug-in en mode débogage](images/device-portal/plugin-debug-mode.png)
3.  Définissez un point d’arrêt dans la fonction du gestionnaire RequestReceived.
![Point d’arrêt au niveau du gestionnaire RequestReceived](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> Assurez-vous que l’architecture de la build correspond exactement à celle de la cible. Si vous utilisez un PC 64bits, vous devez effectuer le déploiement à l’aide d’une buildAMD64. 
4.  Appuyez surF5 pour déployer votre application.
5.  Désactivez DevicePortal, puis réactivez-le pour qu’il trouve votre application (cette opération n’est requise que si vous modifiez le manifeste de votre application; dans les autres cas, il vous suffit de procéder à un nouveau déploiement et d’ignorer cette étape). 
6.  Dans votre navigateur, accédez à l’espace de noms de votre fournisseur; le point d’arrêt devrait alors être atteint.

## <a name="related-topics"></a>Articles connexes
* [Vue d’ensemble de Windows Device Portal](device-portal.md)
* [Créer et utiliser un service d’application](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


