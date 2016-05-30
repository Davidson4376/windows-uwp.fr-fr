---
author: Karl-Bridge-Microsoft
Description: Découvrez comment un utilisateur peut interagir avec une application en arrière-plan via les fonctions vocales et le canevas de Cortana pendant l’exécution d’une commande vocale.
title: Interagir avec une application en arrière-plan
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
label: Interact with a background app
template: detail.hbs
---

# Interagir avec une application en arrière-plan dans Cortana

Permettez l’interaction de l’utilisateur avec une application en arrière-plan, par le biais de la saisie vocale et de texte dans le canevas de **Cortana**, pendant l’exécution d’une commande vocale.



**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Cortana prend en charge un flux de travail complet étape par étape avec votre application. Ce flux de travail est défini par votre application et peut prendre en charge des fonctionnalités telles que les suivantes : 

-   Réussite
-   Relais
-   Progression
-   Confirmation
-   Levée d’ambiguïté
-   Erreur

**Éléments requis :**

Cette rubrique s’appuie sur l’article [Lancer une application en arrière-plan à l’aide des commandes vocales de Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). Nous continuons ici à illustrer ces fonctionnalités avec une application de planification et de gestion de voyages nommée **Adventure Works**.

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Recommandations en matière d’expérience utilisateur :  **

Pour plus d’informations sur l’intégration de votre application à **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233). Pour obtenir de précieux conseils sur la conception d’une application dotée de fonctions vocales à la fois utile et conviviale, voir [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Chaînes de commentaires

Composez les chaînes de commentaires qui sont affichées et dictées par **Cortana**.

L’article [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) fournit des conseils sur la composition de chaînes pour **Cortana**.

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>Chaînes de commentaires

Les cartes de contenu peuvent fournir à l’utilisateur un contexte supplémentaire et vous aider à conserver des chaînes de commentaires concises.

**Cortana** prend en charge les modèles de carte de contenu suivants (un seul modèle peut être utilisé dans l’écran d’achèvement) :

    -   Title only
    -   Title with up to three lines of text
    -   Title with image
    -   Title with image and up to three lines of text

L’image peut être :

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

Vous pouvez également permettre aux utilisateurs de lancer votre application au premier plan en cliquant sur une carte ou sur le lien de texte de votre application.

## <span id="Completion_screen"></span><span id="completion_screen"></span><span id="COMPLETION_SCREEN"></span>Écran d’achèvement

Un écran d’achèvement fournit à l’utilisateur des informations sur la tâche de commande vocale terminée.

Les tâches auxquelles l’application peut répondre en moins de 500 millisecondes et qui ne nécessitent pas d’informations supplémentaires de la part de l’utilisateur peuvent être effectuées sans aucune autre interaction avec **Cortana**. Cortana affiche simplement l’écran d’achèvement.

Ici, nous utilisons l’application **Adventure Works** pour afficher l’écran d’achèvement d’une commande vocale pour afficher les prochains voyages à Londres. 

![Écran d’achèvement de l’application en arrière-plan de Cortana](images/cortana-completion-screen-upcomingtrip-small.png)

La commande vocale est définie dans AdventureWorksCommands.xml :
```
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs contient la méthode de message d’achèvement :

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

## <span id="Hand-off_screen"></span><span id="hand-off_screen"></span><span id="HAND-OFF_SCREEN"></span>Écran relais

Lorsqu’une commande vocale est reconnue, **Cortana** doit appeler la méthode ReportSuccessAsync et afficher le commentaire dans un délai approximatif de 500 ms. Si le service d’application ne peut pas exécuter l’action spécifiée par la commande vocale pendant ce laps de temps, **Cortana** affiche un écran relais jusqu’à ce que votre application appelle la méthode ReportSuccessAsync ou pendant 5 secondes au plus.

Si le service d’application n’appelle pas ReportSuccessAsync ni aucune des méthodes VoiceCommandServiceConnection, l’utilisateur reçoit un message d’erreur et l’appel du service d’application est annulé.

Voici un exemple d’écran relais pour l’application **Adventure Works**. Dans cet exemple, un utilisateur a interrogé **Cortana** pour obtenir les prochains voyages. L’écran relais inclut un message personnalisé avec le nom du service d’application, une icône et la chaîne **Commentaires** déclarée dans le fichier VCD.

![écran relais de l’application en arrière-plan de cortana](images/cortana-backgroundapp-progress-result.png)


## <span id="Progress_screen"></span><span id="progress_screen"></span><span id="PROGRESS_SCREEN"></span>Écran de progression


Si le service d’application appelle ReportSuccessAsync après le délai de 500 ms, **Cortana** affiche un écran de progression à l’attention de l’utilisateur. L’icône de l’application s’affiche. Vous devez fournir les chaînes de progression de l’interface graphique utilisateur et de TTS permettant d’indiquer que la tâche est en cours de gestion.

**Cortana** affiche un écran de progression pendant 5 secondes au plus. Au terme de ces 5 secondes, **Cortana** présente à l’utilisateur un message d’erreur, et le service d’application est fermé. Si le service d’application a besoin de plus de 5 secondes pour effectuer l’action, il peut continuer à mettre à jour **Cortana** avec des écrans de progression.

Voici un exemple d’écran de progression pour l’application **Adventure Works**. Dans cet exemple, un utilisateur a annulé un voyage à Las Vegas. L’écran de progression inclut un message personnalisé pour l’action, une icône et une vignette de contenu avec des informations sur le voyage annulé.

![Écran de progression de l’application en arrière-plan de Cortana ](images/cortana-progress-screen.png)

AdventureWorksVoiceCommandService.cs contient la méthode de message de progression suivant, qui appelle [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) pour afficher l’écran de progression dans **Cortana**.


```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <span id="Confirmation_screen"></span><span id="confirmation_screen"></span><span id="CONFIRMATION_SCREEN"></span>Écran de confirmation


Quand une action spécifiée par une commande vocale est irréversible, a un impact important ou bien encore lorsque la fiabilité de la reconnaissance est faible, un service d’application peut demander confirmation.

Voici un exemple d’écran de confirmation pour l’application **Adventure Works**. Dans cet exemple, l’utilisateur a demandé au service d’application d’annuler un voyage à Las Vegas via **Cortana**. Le service d’application fournit à **Cortana** un écran de confirmation demandant à l’utilisateur une réponse positive ou négative avant d’annuler le voyage.

Si l’utilisateur dit autre chose que Oui ou Non, **Cortana** ne peut pas déterminer la réponse à la question. Le cas échéant, **Cortana** invite l’utilisateur à répondre à une question similaire fournie par le service d’application.

À la deuxième invite, si l’utilisateur ne répond toujours pas par Oui ou Non, **Cortana** l’interroge une troisième fois en posant la même question précédée d’une excuse. Si l’utilisateur ne répond encore une fois ni par Oui ni par Non, **Cortana** cesse d’écouter l’entrée vocale et demande à l’utilisateur d’appuyer plutôt sur l’un des boutons.

L’écran de confirmation inclut un message personnalisé pour l’action, une icône et une vignette de contenu avec des informations sur le voyage annulé.

![Écran de confirmation de l’application en arrière-plan de Cortana](images/cortana-confirmation-screen.png)

AdventureWorksVoiceCommandService.cs contient la méthode d’annulation de voyage suivante, qui appelle [**RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582) pour afficher un écran de confirmation dans **Cortana**.

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <span id="Disambiguation_screen"></span><span id="disambiguation_screen"></span><span id="DISAMBIGUATION_SCREEN"></span>Écran de levée d’ambiguïté


Lorsqu’une action spécifiée par une commande vocale peut avoir plusieurs résultats, un service d’application peut demander des informations supplémentaires à l’utilisateur.

Voici un exemple d’écran de levée d’ambiguïté pour l’application **Adventure Works**. Dans cet exemple, l’utilisateur a demandé au service d’application d’annuler un voyage à Las Vegas via **Cortana**. Or, l’utilisateur a prévu deux voyages à Las Vegas à des dates différentes. Le service d’application ne peut pas effectuer l’action tant que l’utilisateur n’a pas sélectionné le voyage en question.

Le service d’application fournit à **Cortana** un écran de levée d’ambiguïté qui invite l’utilisateur à effectuer une sélection dans la liste des voyages correspondants avant d’en annuler un.

Le cas échéant, **Cortana** invite l’utilisateur à répondre à une question similaire fournie par le service d’application.

À la deuxième invite, si la réponse de l’utilisateur ne permet toujours pas d’identifier la sélection, **Cortana** interroge l’utilisateur une troisième fois en posant la même question précédée d’une excuse. Si là encore, la réponse de l’utilisateur ne permet pas d’identifier la sélection, **Cortana** cesse d’écouter l’entrée vocale et demande à l’utilisateur d’appuyer plutôt sur l’un des boutons.

L’écran de levée d’ambiguïté inclut un message personnalisé pour l’action, une icône et une vignette de contenu avec des informations sur le voyage annulé.

![Écran de levée d’ambiguïté de l’application en arrière-plan de Cortana ](images/cortana-disambiguation-screen.png)

AdventureWorksVoiceCommandService.cs contient la méthode d’annulation de voyage suivante, qui appelle [**RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583) pour afficher l’écran de levée d’ambiguïté dans **Cortana**.

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <span id="Error_screen"></span><span id="error_screen"></span><span id="ERROR_SCREEN"></span>Écran de notification d’erreur


Si une action spécifiée par une commande vocale ne peut pas être effectuée, un service d’application peut fournir un écran de notification d’erreur.

Voici un exemple d’écran de notification d’erreur pour l’application **Adventure Works**. Dans cet exemple, l’utilisateur a demandé au service d’application d’annuler un voyage à Las Vegas via **Cortana**. Toutefois, l’utilisateur n’a planifié aucun voyage à Las Vegas.

Le service d’application fournit à **Cortana** un écran de notification d’erreur qui inclut un message personnalisé pour l’action, une icône et le message d’erreur spécifique.

Appelez [**ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578) pour afficher l’écran de notification d’erreur dans **Cortana**.

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <span id="related_topics"></span>Articles connexes


**Développeurs**
* [Interactions avec Cortana](cortana-interactions.md)
* [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Concepteurs**
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


