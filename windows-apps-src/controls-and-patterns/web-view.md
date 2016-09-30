---
author: Jwmsft
Description: "Un contrôle d’affichage web incorpore dans votre application une vue qui affiche le contenu web à l’aide du moteur de rendu Microsoft Edge. Des liens hypertexte peuvent également apparaître et fonctionner dans un contrôle d’affichage web."
title: Affichage Web
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: dd947d0b55dad56fdd6c684ae236f1c31ac8da86

---

# Affichage web



Un contrôle d’affichage web incorpore dans votre application une vue qui affiche le contenu web à l’aide du moteur de rendu Microsoft Edge. Des liens hypertexte peuvent également apparaître et fonctionner dans un contrôle d’affichage web.

**API importantes**

-   [**Classe WebView**](https://msdn.microsoft.com/library/windows/apps/br227702)

## Est-ce le contrôle approprié?

Utilisez un contrôle d’affichage web pour afficher du contenu HTML à mise en forme enrichie à partir d’un serveur web distant, du code généré de manière dynamique ou des fichiers de contenu dans votre package d’application. Le contenu enrichi peut aussi contenir du code de script et communiquer entre le script et le code de votre application.

## Créer un affichage web

**Modifier l’apparence d’un affichage web**

[**WebView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) n’étant pas une sous-classe [**Control**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx), aucun modèle de contrôle ne lui est associé. Toutefois, il est possible de définir différentes propriétés pour contrôler certains aspects visuels de l’affichage web.
- Pour limiter la zone d’affichage, définissez les propriétés [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) et [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx). 
- Pour translater, mettre à l’échelle, incliner et faire pivoter un affichage web, utilisez la propriété [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransform.aspx).
- Pour contrôler l’opacité de l’affichage web, définissez la propriété [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx).
- Pour spécifier une couleur à utiliser en arrière-plan de la page web quand le contenu HTML ne spécifie aucune couleur, définissez la propriété [**DefaultBackgroundColor**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultbackgroundcolor.aspx). 

**Obtenir le titre de la page web**

Vous pouvez obtenir le titre du document HTML actuellement affiché dans l’affichage web à l’aide de la propriété [**DocumentTitle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.documenttitle.aspx). 

**Événements d’entrée et ordre de tabulation**

Bien que le contrôle WebView ne soit pas une sous-classe Control, il reçoit le focus d’entrée du clavier et fait partie intégrante de la séquence de tabulation. Il fournit une méthode [**Focus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.focus.aspx), ainsi que des événements [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.gotfocus.aspx) et [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.lostfocus.aspx), mais il ne possède aucune propriété en rapport avec la tabulation. Sa position dans la séquence de tabulation est identique à sa position dans l’ordre du document XAML. La séquence de tabulation inclut tous les éléments du contenu de l’affichage web qui peuvent recevoir le focus d’entrée. 

Comme indiqué dans le tableau Événements de la page consacrée à la classe [**WebView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx), l’affichage web ne prend pas en charge la plupart des événements d’entrée utilisateur hérités de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), tels que [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keydown.aspx), [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keyup.aspx) et [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx). À la place, vous pouvez faire appel à [**InvokeScriptAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) avec la fonction JavaScript **eval** pour utiliser les gestionnaires d’événements HTML, et aussi pour utiliser **window.external.notify** à partir du gestionnaire d’événements HTML afin de notifier l’application à l’aide de [**WebView.ScriptNotify**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx).

### Accès au contenu

L’affichage web offre plusieurs API pour la navigation de base : [**GoBack**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goback.aspx), [**GoForward**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goforward.aspx), [**Stop**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.stop.aspx), [**Refresh**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.refresh.aspx), [**CanGoBack**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoback.aspx) et [**CanGoForward**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoforward.aspx). Ces API vous permettent d’ajouter des fonctionnalités de navigation web standard à votre application. 

Pour définir le contenu initial de l’affichage web, définissez la propriété [**Source**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.source.aspx) en XAML. L’analyseur XAML convertit automatiquement la chaîne en [**Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.uri.aspx). 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

La propriété Source peut être définie dans le code, mais il est généralement préférable d’utiliser une des méthodes **Navigate** pour charger du contenu dans le code. 

Pour charger du contenu web, utilisez la méthode [**Navigate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigate.aspx) avec un **Uri** qui fait appel au schéma HTTP ou HTTPS. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

Pour accéder à un URI avec une requête POST et des en-têtes HTTP, utilisez la méthode [**NavigateWithHttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage.aspx). Cette méthode prend uniquement en charge [**HttpMethod.Post**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.post.aspx) et [**HttpMethod.Get**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.get.aspx) pour la valeur de propriété [**HttpRequestMessage.Method**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httprequestmessage.method.aspx). 

Pour charger du contenu non compressé et non chiffré à partir des magasins de données [**LocalFolder**]() ou [**TemporaryFolder**]() de votre application, utilisez la méthode **Navigate** avec un **Uri** qui fait appel au [schéma ms-appdata](). La prise en charge de l’affichage web pour ce schéma nécessite que vous placiez votre contenu dans un sous-dossier à l’intérieur du dossier local ou temporaire. Cela permet l’accès à des URI tels que ms-appdata:///local/*dossier*/*fichier*.html et ms-appdata:///temp/*dossier*/*fichier*.html. (Pour charger des fichiers compressés ou chiffrés, voir [**NavigateToLocalStreamUri**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx).) 

Chacun de ces sous-dossiers de premier niveau est isolé du contenu des autres sous-dossiers de premier niveau. Par exemple, vous pouvez accéder à ms-appdata:///temp/dossier1/fichier.html, mais ce fichier ne peut pas contenir de lien vers ms-appdata:///temp/dossier2/fichier.html. Vous pouvez toutefois ajouter des liens vers du contenu HTML dans le package d’application à l’aide du **schéma ms-appx-web** et vers du contenu web à l’aide des schémas d’URI **http** et **https**.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

Pour charger du contenu à partir de votre package d’application, utilisez la méthode **Navigate** avec un **Uri** qui fait appel au [**schéma ms-appx-web**](https://msdn.microsoft.com/library/windows/apps/xaml/jj655406.aspx#ms_appx_web). 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

Vous pouvez charger du contenu local via un résolveur personnalisé à l’aide de la méthode [**NavigateToLocalStreamUri**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx). Cela permet de mettre en œuvre des scénarios avancés tels que le téléchargement et la mise en cache de contenu web en vue d’une utilisation hors ligne ou l’extraction du contenu d’un fichier compressé.

### Réponse aux événements de navigation

Le contrôle d’affichage web fournit plusieurs événements que vous pouvez utiliser pour répondre aux états de navigation et de chargement de contenu. Les événements se produisent dans l’ordre suivant pour le contenu de l’affichage web racine : [**NavigationStarting**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationstarting.aspx), [**ContentLoading**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.contentloading.aspx), [**DOMContentLoaded**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.domcontentloaded.aspx), [**NavigationCompleted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationcompleted.aspx)


**NavigationStarting** : se produit avant que l’affichage web accède à du nouveau contenu. Vous pouvez annuler la navigation dans un gestionnaire pour cet événement en définissant la propriété WebViewNavigationStartingEventArgs.Cancel sur true. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** : se produit lorsque l’affichage web a commencé à charger du nouveau contenu. 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** : se produit lorsque l’affichage web a fini d’analyser le contenu HTML actuel. 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** : se produit lorsque l’affichage web a fini de charger le contenu actuel ou en cas d’échec de la navigation. Pour déterminer si la navigation a échoué, vérifiez les propriétés [**IsSuccess**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess.aspx) et [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus.aspx) de la classe [**WebViewNavigationCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.aspx). 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

Des événements similaires se produisent dans le même ordre pour chaque élément **iframe** du contenu de l’affichage web : 
- [**FrameNavigationStarting**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationstarting.aspx) : se produit avant qu’une trame de l’affichage web accède à du nouveau contenu. 
- [**FrameContentLoading**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framecontentloading.aspx) : se produit lorsqu’une trame de l’affichage web a commencé à charger du nouveau contenu. 
- [**FrameDOMContentLoaded**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framedomcontentloaded.aspx) : se produit lorsqu’une trame de l’affichage web a fini d’analyser son contenu HTML actuel. 
- [**FrameNavigationCompleted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationcompleted.aspx) : se produit lorsqu’une trame de l’affichage web a fini de charger son contenu. 

### Réponse aux problèmes potentiels

Vous pouvez répondre aux problèmes potentiels liés au contenu tel que les longs scripts d’exécution, le contenu que l’affichage web ne peut pas charger et les avertissements relatifs à du contenu dangereux. 

Votre application peut sembler inerte pendant l’exécution de scripts. L’événement [**LongRunningScriptDetected**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.longrunningscriptdetected.aspx) se produit régulièrement pendant que l’affichage web exécute JavaScript et donne la possibilité d’interrompre le script. Pour déterminer depuis combien de temps le script est exécuté, vérifiez la propriété [**ExecutionTime**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime.aspx) de la classe [**WebViewLongRunningScriptDetectedEventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.aspx). Pour interrompre le script, définissez la propriété [**StopPageScriptExecution**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution.aspx) des arguments d’événement sur **true**. Le script interrompu ne sera pas exécuté de nouveau, sauf s’il est rechargé au cours d’une navigation ultérieure dans l’affichage web. 

Le contrôle d’affichage web ne peut pas héberger des types de fichier arbitraires. Lorsque l’utilisateur essaie de charger du contenu que l’affichage web ne peut pas héberger, l’événement [**UnviewableContentIdentified**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unviewablecontentidentified.aspx) se produit. Vous pouvez gérer cet événement et avertir l’utilisateur ou utiliser la classe [**Launcher**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.system.launcher.aspx) pour rediriger le fichier vers un navigateur externe ou une autre application.

De même, l’événement [**UnsupportedUriSchemeIdentified**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsupportedurischemeidentified.aspx) se produit quand un schéma d’URI non pris en charge, tel que fbconnect:// ou mailto://, est appelé dans le contenu web. Vous pouvez gérer cet événement de manière à personnaliser le comportement plutôt que d’autoriser le lanceur système par défaut à lancer l’URI.

L’événement [**UnsafeContentWarningDisplayingevent**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying.aspx) se produit lorsque l’affichage web affiche une page d’avertissement pour du contenu qui a été signalé comme dangereux par le filtre SmartScreen. Si l’utilisateur choisit de poursuivre la navigation, l’avertissement ne s’affichera pas et l’événement ne se déclenchera pas la prochaine fois qu’il accédera à la page.

### Gestion des cas particuliers de contenu de l’affichage web

Vous pouvez utiliser la propriété [**ContainsFullScreenElement**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelement.aspx) et l’événement [**ContainsFullScreenElementChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelementchanged.aspx) pour détecter et activer les expériences en plein écran dans le contenu web, telles que la lecture vidéo en plein écran, et y répondre. Par exemple, vous pouvez utiliser l’événement ContainsFullScreenElementChanged pour redimensionner l’affichage web afin qu’il occupe la totalité de l’affichage de votre application, ou, comme dans l’exemple ci-après, afin de passer une application du mode fenêtre au mode plein écran quand une expérience web est souhaitée.

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

Vous pouvez utiliser l’événement [**NewWindowRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.newwindowrequested.aspx) pour gérer les cas où le contenu web hébergé demande l’affichage d’une nouvelle fenêtre, telle qu’une fenêtre contextuelle. Vous pouvez utiliser un autre contrôle WebView pour afficher le contenu de la fenêtre demandée.

Utilisez l’événement [**PermissionRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx) pour activer les fonctionnalités web qui requièrent des fonctionnalités spéciales. Ces dernières incluent actuellement la géolocalisation, le stockage IndexedDB et la prise en charge du son et de la vidéo de l’utilisateur (d’un microphone ou d’une webcam, par exemple). Si votre application accède à l’emplacement ou au média de l’utilisateur, vous devez déclarer cette fonctionnalité dans le manifeste de l’application. Par exemple, une application qui utilise la géolocalisation doit inclure au minimum les déclarations de fonctionnalités suivantes dans le fichier Package.appxmanifest:

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

En plus de la gestion par l’application de l’événement [**PermissionRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx), l’utilisateur devra approuver des boîtes de dialogue système standard pour les applications faisant appel à des fonctionnalités de géolocalisation ou multimédias afin d’activer ces dernières.

Voici un exemple de la façon dont une application activerait la géolocalisation dans une carte de Bing:

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

Si votre application nécessite une entrée utilisateur ou d’autres opérations asynchrones pour répondre à une demande d’autorisation, utilisez la méthode [**Defer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx) de [**WebViewPermissionRequest**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.aspx) afin de créer un élément [**WebViewDeferredPermissionRequest**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewdeferredpermissionrequest.aspx) qui pourra être traité à un moment ultérieur. Voir [**WebViewPermissionRequest.Defer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx). 

Si les utilisateurs doivent se déconnecter de façon sécurisée d’un site web hébergé dans un affichage web, ou dans tout autre cas où la sécurité est importante, appelez la méthode statique [**ClearTemporaryWebDataAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cleartemporarywebdataasync.aspx) pour effacer tout le contenu mis en cache localement de la session d’affichage web. Cela permet d’empêcher les utilisateurs malveillants d’accéder à des données sensibles. 

### Interaction avec le contenu de l’affichage web

Vous pouvez interagir avec le contenu de l’affichage web à l’aide de la méthode [**InvokeScriptAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) pour appeler ou injecter un script dans le contenu de l’affichage web, et à l’aide de l’événement [**ScriptNotify**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) pour récupérer des informations à partir du contenu de l’affichage web.

Pour appeler un script JavaScript à l’intérieur du contenu de l’affichage web, utilisez la méthode [**InvokeScriptAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx). Le script appelé peut retourner uniquement des valeurs de chaîne. 

Par exemple, si le contenu d’un affichage web nommé `webView1` contient une fonction nommée `setDate` qui nécessite 3 paramètres, vous pouvez l’appeler comme ceci : 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


Vous pouvez utiliser **InvokeScriptAsync** avec la fonction JavaScript **eval** pour injecter du contenu dans la page web.

Ici, le texte d’une zone de texte XAML (`nameTextBox.Text`) est écrit dans un élément div d’une page HTML hébergée dans `webView1`. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Les scripts au sein du contenu de l’affichage web peuvent utiliser **window.external.notify** avec un paramètre de chaîne pour renvoyer des informations à votre application. Pour recevoir ces messages, vous devez gérer l’événement [**ScriptNotify**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx). 

Pour permettre à une page Web externe de déclencher l’événement **ScriptNotify** durant l’appel de window.external.notify, vous devez inclure l’URI de la page dans la section **ApplicationContentUriRules** du manifeste de l’application. (Vous pouvez faire cela dans Microsoft Visual Studio sous l’onglet URI de contenu du concepteur du fichier Package.appxmanifest.) Les URI répertoriés doivent utiliser le protocole HTTPS, et peuvent contenir des caractères génériques de sous-domaine (par exemple, `https://*.microsoft.com`), mais pas de caractères génériques de domaine (par exemple, `https://*.com` et `https://*.*`). La configuration requise pour le manifeste ne s’applique pas au contenu qui provient du package d’application, qui utilise un URI ms-local-stream:// ou qui est chargé à l’aide de [**NavigateToString**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetostring.aspx). 

### Accès au composant Windows Runtime d’un affichage web

Vous pouvez utiliser la méthode [**AddWebAllowedObject**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx) pour injecter une instance d’une classe native à partir d’un composant Windows Runtime dans le contexte JavaScript de l’affichage web. Cela permet un accès complet aux méthodes natives, aux propriétés et aux événements de cet objet dans le contenu JavaScript de cet affichage web. La classe doit être décorée de l’attribut [**AllowForWeb**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.metadata.allowforwebattribute.aspx). 

Par exemple, ce code injecte une instance de `MyClass` importée à partir d’un composant Windows Runtime dans un affichage web.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

Pour plus d’informations, voir [**WebView.AddWebAllowedObject**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx). 

Par ailleurs, le contenu JavaScript approuvé d’un affichage web peut être autorisé à accéder directement aux API Windows Runtime. Cela fournit des fonctionnalités natives puissantes pour les applications web hébergées dans un affichage web. Pour activer cette fonctionnalité, l’URI du contenu approuvé doit être autorisé dans la section ApplicationContentUriRules de l’application au sein du fichier Package.appxmanifest, avec WindowsRuntimeAccess spécifiquement défini sur «all». 

Cet exemple montre une section du manifeste de l’application. Ici, un URI local est autorisé à accéder au composant Windows Runtime. 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### Options pour l’hébergement de contenu web

Vous pouvez utiliser la propriété [**WebView.Settings**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.settings.aspx) (de type [**WebViewSettings**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewsettings.aspx)) pour activer ou désactiver JavaScript et IndexedDB. Par exemple, si vous utilisez un affichage web pour afficher du contenu uniquement statique, il peut être judicieux de désactiver JavaScript pour optimiser les performances.

### Capture du contenu de l’affichage web

Pour activer le partage du contenu de l’affichage web avec d’autres applications, utilisez la méthode [**CaptureSelectedContentToDataPackageAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync.aspx), qui retourne le contenu sélectionné sous forme d’élément [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datapackage.aspx). Cette méthode étant asynchrone, vous devez utiliser un report pour empêcher le retour du gestionnaire d’événements [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx) avant la fin de l’appel asynchrone. 

Pour obtenir une image d’aperçu du contenu actuel de l’affichage web, utilisez la méthode [**CapturePreviewToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.capturepreviewtostreamasync.aspx). Cette méthode crée une image du contenu actuel et l’écrit dans le flux spécifié. 

### Comportement de threading

Par défaut, le contenu de l’affichage web est hébergé sur le thread d’interface utilisateur sur les appareils de bureau et en dehors de celui-ci sur tous les autres appareils. Vous pouvez utiliser la propriété statique [**WebView.DefaultExecutionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultexecutionmode.aspx) pour obtenir le comportement de threading par défaut du client actuel. Si nécessaire, vous pouvez utiliser le constructeur [**WebView(WebViewExecutionMode)**](https://msdn.microsoft.com/library/windows/apps/xaml/dn932036.aspx) pour remplacer ce comportement. 

> **Remarque** &nbsp;&nbsp;Des problèmes de performances peuvent se présenter lorsque du contenu est hébergé sur le thread d’interface utilisateur sur des appareils mobiles. Par conséquent, veillez à effectuer des tests sur tous les appareils cibles en cas de changement de la propriété DefaultExecutionMode.

Un affichage web qui héberge le contenu en dehors du thread d’interface utilisateur n’est pas compatible avec les contrôles parents qui nécessitent la propagation de mouvements entre le contrôle d’affichage web et le parent, tels que [**FlipView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx), [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) et les autres contrôles associés. Ces contrôles ne sont pas en mesure de recevoir des mouvements effectués dans l’affichage web hors thread. En outre, l’impression de contenu web hors thread n’est pas directement prise en charge: vous devez imprimer un élément avec le remplissage [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.aspx) à la place.

## <span id="Recommendations"></span><span id="recommendations"></span><span id="RECOMMENDATIONS"></span>Recommandations


-   Vérifiez que le site web chargé possède une mise en forme adaptée à l’appareil, et qu’il utilise des couleurs, une typographie et une navigation cohérentes avec le reste de votre application.
-   Les champs d’entrée doivent avoir une taille appropriée. Il se peut que les utilisateurs ne réalisent pas qu’ils peuvent zoomer pour entrer du texte.
-   Si un affichage web n’a pas la même apparence que le reste de votre application, utilisez d’autres contrôles ou adoptez une autre approche pour accomplir les tâches en question. Si l’apparence de votre affichage web correspond au reste de votre application, l’expérience sera beaucoup plus fluide et transparente aux yeux des utilisateurs.



## <span id="related_topics"></span>Rubriques connexes

* [**Classe WebView**](https://msdn.microsoft.com/library/windows/apps/br227702)
 

 







<!--HONumber=Jun16_HO4-->


