---
Description: Indiquez des liens ciblés à partir du service d’application en arrière-plan dans Cortana pour lancer l’application au premier plan dans un état ou un contexte spécifique.
title: Lien ciblé de Cortana vers une application en arrière-plan
ms.assetid: BE811A87-8821-476A-90E4-2E20D37E4043
label: Lien ciblé vers une application en arrière-plan
template: detail.hbs
---

# Lien ciblé de Cortana vers une application en arrière-plan


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Éléments et attributs de définition des commandes vocales (VCD) v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Indiquez des liens ciblés à partir du service d’application en arrière-plan dans **Cortana** pour lancer l’application au premier plan dans un état ou un contexte spécifique.

Un lien ciblé est affiché par défaut sur l’écran d’achèvement de **Cortana**, mais vous pouvez afficher des liens ciblés sur divers autres écrans. Pour plus d’informations, voir [Interaction avec une application en arrière-plan dans Cortana](interact-with-a-background-app-in-cortana.md).

**Prérequis : **

Cette rubrique s’appuie sur la page [Interaction avec une application en arrière-plan dans Cortana](interact-with-a-background-app-in-cortana.md). Nous continuons à utiliser une application de planification et de gestion de voyages nommée **Adventure Works** pour illustrer diverses fonctionnalités **Cortana**.

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Recommandations en matière d’expérience utilisateur :**

Pour des informations sur la manière d’intégrer votre application à **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233). Pour obtenir de précieux conseils sur la conception d’une application, dotée de fonctions vocales, à la fois utile et conviviale, voir [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <span id="Overview"> </span> <span id="overview"> </span> <span id="OVERVIEW"> </span>Vue d’ensemble


Les utilisateurs peuvent accéder à votre application via **Cortana** en :

-   la lançant au premier plan (voir [Lancer une application au premier plan avec les commandes vocales de Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)) ;
-   proposant des fonctionnalités spécifiques comme service d’application en arrière-plan (voir [Lancer une application en arrière-plan avec des commandes vocales dans Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)) ;
-   créant un lien ciblé vers des pages, du contenu, et un état ou du contexte spécifiques.

Nous abordons ici ce dernier point.

La création de liens ciblés est utile lorsque Cortana et le service de votre application servent de passerelle vers l’application complète (au lieu d’obliger l’utilisateur à lancer votre application via le menu Démarrer), ou pour fournir un accès dans votre application à des détails et des fonctionnalités non disponibles via Cortana. La création de liens ciblés constitue un autre moyen de faciliter l’utilisation et l’accès à votre application.

Il existe trois manières de fournir des liens ciblés :

-   Un lien « Atteindre &lt;application&gt; » sur différents écrans **Cortana**.
-   Un lien incorporé dans une vignette de contenu sur différents écrans **Cortana**.
-   Le service d’application utilise un programme pour lancer l’application au premier plan.

Les deux **Cortana** et le service d’application en arrière-plan se ferment lors du lancement de l’application au premier plan.

## <span id="Go_to__app__deep_link"> </span> <span id="go_to__app__deep_link"> </span> <span id="GO_TO__APP__DEEP_LINK"> </span>Lien ciblé « Atteindre &lt;application&gt; »


**Cortana** peut afficher un lien ciblé « Atteindre &lt;application&gt; » sous la vignette de contenu sur différents écrans.

![écran d’achèvement de l’application en arrière-plan de Cortana](images/cortana-completion-screen.png)

Vous pouvez fournir un argument de lancement de façon à ce que ce lien ouvre votre application avec un contexte similaire au service d’application. Si vous ne fournissez pas d’argument de lancement, l’application est lancée sur l’écran principal.

Ici, nous ajoutons un paramètre [AppLaunchArgument](https://msdn.microsoft.com/library/windows/apps/dn974183) avec la valeur [Las Vegas](https://msdn.microsoft.com/library/windows/apps/dn974182) à un objet **VoiceCommandResponse** utilisé dans l’appel **ReportSuccessAsync** de l’objet [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204). objet

```CSharp
var userMessage = new VoiceCommandUserMessage();
string keepingTripToDestination = string.Format(
  cortanaResourceMap.GetValue("KeepingTripToDestination", 
    cortanaContext).ValueAsString, destination);
userMessage.DisplayMessage = userMessage.SpokenMessage = 
  keepingTripToDestination;

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await voiceServiceConnection.ReportSuccessAsync(response);
```

## <span id="Content_tile_deep_link"> </span> <span id="content_tile_deep_link"> </span> <span id="CONTENT_TILE_DEEP_LINK"> </span>Lien ciblé vers une vignette de contenu


Vous pouvez fournir des liens ciblés vers une vignette de contenu complète sur différents écrans **Cortana**.

![écran relais de l’application en arrière-plan de Cortana ](images/cortana-backgroundapp-progress-result.png)

À l’instar des liens « Atteindre &lt;application&gt; », vous pouvez fournir un argument de lancement pour ouvrir votre application avec un contexte similaire au service d’application. Si vous ne fournissez pas d’argument de lancement, la vignette de contenu n’est pas liée à votre application.

Ici, nous ajoutons deux vignettes de contenu avec différentes valeurs de paramètre [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) à une liste [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) utilisée dans l’appel [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) de l’objet [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have two trips to Vegas coming up.";

var destinationsContentTiles = new List<VoiceCommandContentTile>();

var destinationTile1 = new VoiceCommandContentTile();
destinationTile1.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile1.AppLaunchArgument = “id_Vegas_001";
destinationTile1.Title = "Las Vegas Tech Conference";
destinationTile1.TextLine1 = "May 15th 2015";
destinationsContentTiles.Add(destinationTile1);

var destinationTile2 = new VoiceCommandContentTile();
destinationTile2.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile2.AppLaunchArgument = “id_Vegas_002";
destinationTile2.Title = "Fun in Vegas";
destinationTile2.TextLine1 = "August 24th 2015";
destinationsContentTiles.Add(destinationTile2);

var response = 
  VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

response.AppLaunchArgument = “destination=Las Vegas";
    
await voiceServiceConnection.ReportSuccessAsync(response);
```

## <span id="Programmatic_deep_link"> </span> <span id="programmatic_deep_link"> </span> <span id="PROGRAMMATIC_DEEP_LINK"> </span>Lien ciblé par programme


Vous pouvez également lancer votre application par programme avec un argument de lancement permettant d’ouvrir votre application avec un contexte similaire au service d’application. Si vous ne fournissez pas d’argument de lancement, l’application est lancée sur l’écran principal.

Ici, nous ajoutons un paramètre [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) avec la valeur Las Vegas à un objet [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) utilisé dans l’appel [**RequestAppLaunchAsync**](https://msdn.microsoft.com/library/windows/apps/dn706581) de l’objet [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <span id="App_manifest"> </span> <span id="app_manifest"> </span> <span id="APP_MANIFEST"> </span>Manifeste de l’application


Pour activer un lien ciblé vers votre application, vous devez déclarer l’extension `windows.personalAssistantLaunch` dans le fichier Package.appxmanifest de votre projet d’application.

Ici, nous déclarons l’extension `windows.personalAssistantLaunch` pour l’application **Adventure Works**.

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <span id="Protocol_contract"> </span> <span id="protocol_contract"> </span> <span id="PROTOCOL_CONTRACT"> </span>Contrat de protocole


Votre application se lance au premier plan par le biais de l’activation d’un URI (Uniform Resource Identifier) à l’aide d’un contrat [**Protocol**](https://msdn.microsoft.com/library/windows/apps/br224693). Votre application doit ignorer l’événement [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) de votre application et rechercher un **ActivationKind** de **Protocol**. Pour plus d’informations, voir [Gérer l’activation des URI](https://msdn.microsoft.com/library/windows/apps/mt228339).

Ici, nous décodons l’URI fourni par [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742) pour accéder à l’argument de lancement. Pour cet exemple, l’[**Uri**](https://msdn.microsoft.com/library/windows/apps/br224746) est défini sur « windows.personalassistantlaunch:?LaunchContext=Las Vegas. »

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <span id="related_topics"> </span>Articles connexes


**Développeurs**
* [Interactions avec Cortana](cortana-interactions.md)
* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Concepteurs**
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 




<!--HONumber=Mar16_HO1-->
