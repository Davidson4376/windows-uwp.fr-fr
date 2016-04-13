---
Description: Indiquez des liens ciblés à partir du service d’application en arrière-plan dans Cortana pour lancer l’application au premier plan dans un état ou un contexte spécifique.
title: Lien ciblé de Cortana vers une application en arrière-plan
ms.assetid: BE811A87-8821-476A-90E4-2E20D37E4043
label: Deep link to a background app
template: detail.hbs
---

# Lien ciblé de Cortana vers une application en arrière-plan


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Indiquez des liens ciblés à partir d’une application en arrière-plan dans **Cortana** qui lance l’application au premier plan dans un état ou un contexte spécifique.

> **Remarque**  
**Cortana** et le service d’application en arrière-plan se ferment lors du lancement de l’application au premier plan.

Un lien ciblé est affiché par défaut sur l’écran d’achèvement de **Cortana** (ici affiché sous la forme « Go to AdventureWorks »), mais vous pouvez afficher des liens ciblés sur divers autres écrans. 

![Écran d’achèvement de l’application en arrière-plan de Cortana](images/cortana-completion-screen-upcomingtrip-small.png)

**Prérequis : **

Cette rubrique s’appuie sur la page [Interaction avec une application en arrière-plan dans Cortana](interact-with-a-background-app-in-cortana.md). Nous continuons à utiliser une application de planification et de gestion de voyages nommée **Adventure Works** pour illustrer diverses fonctionnalités **Cortana**.

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Recommandations en matière d’expérience utilisateur : **

Pour des informations sur la manière d’intégrer votre application à **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233). Pour obtenir de précieux conseils sur la conception d’une application, dotée de fonctions vocales, à la fois utile et conviviale, voir [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <span id="Overview"> </span> <span id="overview"> </span> <span id="OVERVIEW"> </span>Vue d’ensemble


Les utilisateurs peuvent accéder à votre application via **Cortana** en :

-   l’activant au premier plan (voir [Activer une application au premier plan avec les commandes vocales de Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)) ;
-   proposant des fonctionnalités spécifiques comme service d’application en arrière-plan (voir [Activer une application en arrière-plan avec des commandes vocales dans Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)) ;
-   créant un lien ciblé vers des pages, du contenu, et un état ou du contexte spécifiques.

Nous décrivons ici la création de liens profonds.

La création de liens ciblés est utile lorsque Cortana et le service de votre application servent de passerelle vers l’application complète (au lieu d’obliger l’utilisateur à lancer votre application via le menu Démarrer), ou pour fournir un accès dans votre application à des détails et des fonctionnalités non disponibles via Cortana. La création de liens ciblés constitue un autre moyen de faciliter l’utilisation et la promotion de votre application.

Il existe trois manières de fournir des liens ciblés :

-   Un lien « Atteindre &lt;application&gt; » sur différents écrans **Cortana**.
-   Un lien incorporé dans une vignette de contenu sur différents écrans **Cortana**.
-   Le lancement par programmation de l’application au premier plan à partir du service d’application en arrière-plan.

## <span id="Go_to__app__deep_link"> </span> <span id="go_to__app__deep_link"> </span> <span id="GO_TO__APP__DEEP_LINK"> </span>Lien ciblé « Atteindre &lt;application&gt; »


**Cortana** affiche un lien ciblé « Atteindre &lt;application&gt; » sous la carte de contenu sur la plupart des écrans.

![Écran d’achèvement de l’application en arrière-plan de Cortana](images/cortana-completion-screen.png)

Vous pouvez fournir un argument de lancement pour ce lien qui ouvre votre application avec un contexte similaire au service d’application. Si vous ne fournissez pas d’argument de lancement, l’application est lancée sur l’écran principal.

Dans cet exemple du fichier AdventureWorksVoiceCommandService.cs de l’exemple **AdventureWorks**, nous transmettons la destination spécifiée à la méthode SendCompletionMessageForDestination, qui récupère tous les voyages correspondants et fournit un lien ciblé vers l’application.

Tout d’abord, nous créons un message [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```), qui est prononcé par **Cortana** et affiché sur le canevas de **Cortana**. Un objet de liste [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) est ensuite créé pour afficher la collection de cartes de résultat sur le canevas. 

Ces deux objets sont ensuite transmis à la méthode [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) de l’objet [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) (```response```). Nous définissons ensuite la valeur de propriété [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) sur la valeur de la destination dans la commande vocale.

Enfin, nous appelons la méthode [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) de la classe [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```


## <span id="Content_tile_deep_link"> </span> <span id="content_tile_deep_link"> </span> <span id="CONTENT_TILE_DEEP_LINK"> </span>Lien ciblé vers une vignette de contenu


Vous pouvez ajouter des liens ciblés à des cartes de contenu sur différents écrans **Cortana**.

![Écran relais de l’application en arrière-plan de Cortana ](images/cortana-backgroundapp-progress-result.png)

À l’instar des liens « Atteindre &lt;application&gt; », vous pouvez fournir un argument de lancement pour ouvrir votre application avec un contexte similaire au service d’application. Si vous ne fournissez pas d’argument de lancement, la vignette de contenu n’est pas liée à votre application.

Dans cet exemple du fichier AdventureWorksVoiceCommandService.cs de l’exemple **AdventureWorks**, nous transmettons la destination spécifiée à la méthode SendCompletionMessageForDestination, qui récupère tous les voyages correspondants et fournit aux cartes de contenu un lien ciblé vers l’application.

Tout d’abord, nous créons un message [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```), qui est prononcé par **Cortana** et affiché sur le canevas de **Cortana**. Un objet de liste [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) est ensuite créé pour afficher la collection de cartes de résultat sur le canevas. 

Ces deux objets sont ensuite transmis à la méthode [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) de l’objet [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) (```response```). Nous définissons ensuite la valeur de propriété [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) sur la valeur de la destination dans la commande vocale.

Enfin, nous appelons la méthode [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) de la classe [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).
Ici, nous ajoutons deux vignettes de contenu avec différentes valeurs de paramètre [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) à une liste [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) utilisée dans l’appel [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) de l’objet [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
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
* [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Concepteurs**
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


