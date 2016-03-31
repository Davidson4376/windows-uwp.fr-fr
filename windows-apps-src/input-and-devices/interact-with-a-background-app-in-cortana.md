---
Learn how a user can interact with a background app through the Cortana voice and canvas during the execution of a voice command.
Interact with a background app
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
Interact with a background app
template: detail.hbs
---

# Interact with a background app in Cortana


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Important APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Voice Command Definition (VCD) elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Learn how a user can interact with a background app through the **Cortana** voice and canvas during the execution of a voice command.

Voice commands with **Cortana** can include a rich user experience and interaction flow within **Cortana** that is controlled by the background app. The app can specify a number of different types of screens to support functionality that includes:

-   Successful completion
-   Hand-off
-   Progress
-   Confirmation
-   Disambiguation
-   Error

**Prerequisites:  **

This topic builds on [Launch a background app with voice commands in Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). We continue here to demonstrate features with a trip planning and management app named **Adventure Works**.

If you're new to developing Universal Windows Platform (UWP) apps, have a look through these topics to get familiar with the technologies discussed here.

-   [Create your first app](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Learn about events with [Events and routed events overview](https://msdn.microsoft.com/library/windows/apps/mt185584)

**User experience guidelines:  **

See [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for info about how to integrate your app with **Cortana** and [Speech design guidelines](https://msdn.microsoft.com/library/windows/apps/dn596121) for helpful tips on designing a useful and engaging speech-enabled app.

## <span id="Completion_screen"></span><span id="completion_screen"></span><span id="COMPLETION_SCREEN"></span>Completion screen


A completion screen provides the user with information about the completed voice command task.

Tasks that take less than 500 milliseconds for your app to respond, and require no additional information from the user, can be completed without further participation from **Cortana**, other than displaying the completion screen.

Here, we show how **Cortana** can display a cancellation result from the **Adventure Works** app for an upcoming trip to Las Vegas.

![cortana background app completion screen](images/cortana-completion-screen.png)

1.  **Choose the feedback strings to be displayed and spoken by Cortana**

    Follow the [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for recommendations on composing strings that **Cortana** shows and speaks.

2.  **Choose content tiles based on the action performed (optional)**

    Content tiles can provide additional context for the user and help keep the feedback strings concise.

    **Cortana** supports the following content tile templates (only one template can be used on the completion screen):

    -   Title only
    -   Title with up to three lines of text
    -   Title with icon
    -   Title with icon and up to three lines of text

    The icon can be:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

    You can also let users launch your app in the foreground by either tapping a tile or the text link to your app.

3.  **Show the successful completion screen**

    Here's an example of a successful completion screen with multiple content tiles.

```    CSharp
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

    response.AppLaunchArgument = “Las Vegas";
        
    await voiceServiceConnection.ReportSuccessAsync(response);
```

## <span id="Hand-off_screen"></span><span id="hand-off_screen"></span><span id="HAND-OFF_SCREEN"></span>Hand-off screen


Once a voice command is recognized, **Cortana** must present feedback within approximately 500 milliseconds. If the app service cannot complete the action specified by the voice command within 500ms, **Cortana** presents a hand-off screen that is shown for up to 5 seconds.

The app icon and name are displayed on the hand-off screen, and you must provide both GUI and text-to-speech (TTS) handoff strings to indicate that the voice command was correctly understood.

Here's an example of a hand-off screen for the **Adventure Works** app. In this example, a user has queried **Cortana** for upcoming trips. The hand-off screen includes a message customized with the app service name, an icon, and the **Feedback** string declared in the VCD file.

![cortana background app hand-off screen](images/cortana-backgroundapp-progress-result.png)

## <span id="Progress_screen"></span><span id="progress_screen"></span><span id="PROGRESS_SCREEN"></span>Progress screen


If the app service takes more than 500ms between steps, **Cortana** updates the user on what’s happening with a progress screen. The app icon is displayed, and you must provide both GUI and TTS progress strings to indicate that the task is being actively handled.

**Cortana** shows a progress screen for a maximum of 5 seconds. After 5 seconds, **Cortana** presents the user with an error message and the app service is closed. If the app service needs more than 5 seconds to complete the action, it can continue to update **Cortana** with progress screens.

Here's an example of a hand-off screen for the **Adventure Works** app. In this example, a user has canceled a trip to Las Vegas through **Cortana**. The progress screen includes a message customized for the action, an icon, and a content tile with information about the trip being canceled.

![cortana background app progress screen ](images/cortana-progress-screen.png)

1.  **Choose the feedback strings to be displayed and spoken by Cortana**

    Follow the [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for recommendations on composing strings that **Cortana** shows and speaks.

2.  **Choose content tiles based on the action performed (optional)**

    Content tiles can provide additional context for the user and help keep the feedback strings concise.

    **Cortana** supports the following content tile templates (only one template can be used on the completion screen):

    -   Title only
    -   Title with up to three lines of text
    -   Title with icon
    -   Title with icon and up to three lines of text

    The icon can be:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

    You can also let users launch your app in the foreground by either tapping a tile or the text link to your app.

3.  **Build the response**

    Call [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) to show the progress screen in **Cortana**.

4.  **Show the progress screen**

    Here's an example of a progress screen with a content tile.

```    CSharp
var userMessage = new VoiceCommandUserMessage();

    var destinationsContentTiles = new List<VoiceCommandContentTile>();

    destinationsContentTiles.Add(selectedDestination);

    var response = 
      VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    response.AppLaunchArgument = "destination=Las Vegas";
    await voiceServiceConnection.ReportProgressAsync(response);
```

## <span id="Confirmation_screen"></span><span id="confirmation_screen"></span><span id="CONFIRMATION_SCREEN"></span>Confirmation screen


When an action specified by a voice command is irreversible, has a significant impact, or the recognition confidence is not high, an app service can request confirmation.

Here's an example of a confirmation screen for the **Adventure Works** app. In this example, a user has instructed the app service to cancel a trip to Las Vegas through **Cortana**. The app service has provided **Cortana** with a confirmation screen that prompts the user for a yes or no answer before canceling the trip.

If the user says something other than "Yes" or "No", **Cortana** cannot determine the answer to the question. In this case, **Cortana** prompts the user with a similar question provided by the app service.

On the second prompt, if the user still doesn’t say "Yes" or "No", **Cortana** prompts the user a third time with the same question prefixed with an apology. If the user still doesn’t say "Yes" or "No", **Cortana** stops listening for voice input and asks the user to tap one of the buttons instead.

The confirmation screen includes a message customized for the action, an icon, and a content tile with information about the trip being canceled.

![cortana background app confirmation screen](images/cortana-confirmation-screen.png)

1.  **Choose the feedback strings to be displayed and spoken by Cortana**

    Follow the [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for recommendations on composing strings that **Cortana** shows and speaks.

2.  **Choose content tiles based on the action performed (optional)**

    Content tiles can provide additional context for the user and help keep the feedback strings concise.

    **Cortana** supports the following content tile templates (only one template can be used on the completion screen):

    -   Title only
    -   Title with up to three lines of text
    -   Title with icon
    -   Title with icon and up to three lines of text

    The icon can be:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

    You can also let users launch your app in the foreground by either tapping a tile or the text link to your app.

3.  **Build the response**

    Call [**RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582) to show the confirmation screen in **Cortana**.

4.  **Show the confirmation screen**

    Here's an example of a confirmation screen with a content tile.

```    CSharp
var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage = userPrompt.SpokenMessage = 
      "Are you sure you want to cancel the trip to Las Vegas?”;

    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage = 
      userReprompt.SpokenMessage = "Do you want to cancel this trip to Las Vegas?"; 

    userPrompt.DisplayMessage = “Cancel this trip?”;
    userPrompt.SpokenMessage ="Do you wanna cancel this trip to Vegas?”;

    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage = “Did you want to cancel this trip?”;
    userReprompt.SpokenMessage = "Did you wanna cancel this trip?"; 

    var destinationsContentTiles = new List<VoiceCommandContentTile>();

    var destinationTile = new VoiceCommandContentTile();
    destinationTile.ContentTileType = 
      VoiceCommandContentTileType.TitleWith68x68IconAndText;
    destinationTile.Title = "Vegas Tech Conference";

    destinationTile.TextLine1 = "May 15th";


    destinationsContentTiles.Add(destinationTile);

    var response = 
      VoiceCommandResponse.CreateResponseForPrompt(
        userPrompt, userReprompt, destinationsContentTiles);

    var voiceCommandConfirmation = 
      await voiceServiceConnection.RequestConfirmationAsync(response);

    if (voiceCommandConfirmation != null)
    {
       // Use the voiceCommandConfirmation.Confirmed to take action.
       // Call Cortana to present the next screen in .5 seconds 
       // and avoid a transition screen.
    }
```

## <span id="Disambiguation_screen"></span><span id="disambiguation_screen"></span><span id="DISAMBIGUATION_SCREEN"></span>Disambiguation screen


When an action specified by a voice command has more than one possible outcome, an app service can request more info from the user.

Here's an example of a disambiguation screen for the **Adventure Works** app. In this example, a user has instructed the app service to cancel a trip to Las Vegas through **Cortana**. However, the user has two trips to Las Vegas on different dates and the app service cannot complete the action without the user selecting the intended trip.

The app service provides **Cortana** with a disambiguation screen that prompts the user to make a selection from a list of matching trips, before it cancels any.

In this case, **Cortana** prompts the user with a similar question provided by the app service.

On the second prompt, if the user still doesn’t say something that can be used to identify the selection, **Cortana** prompts the user a third time with the same question prefixed with an apology. If the user still doesn’t say something that can be used to identify the selection, **Cortana** stops listening for voice input and asks the user to tap one of the buttons instead.

The disambiguation screen includes a message customized for the action, an icon, and a content tile with information about the trip being canceled.

![cortana background app disambiguation screen ](images/cortana-disambiguation-screen.png)

1.  **Choose the feedback strings to be displayed and spoken by Cortana**

    Follow the [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for recommendations on composing strings that **Cortana** shows and speaks.

2.  **Choose content tiles based on the action performed (optional)**

    Content tiles can provide additional context for the user and help keep the feedback strings concise.

    **Cortana** supports the following content tile templates (only one template can be used on the completion screen):

    -   Title only
    -   Title with up to three lines of text
    -   Title with icon
    -   Title with icon and up to three lines of text

    The icon can be:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

    You can also let users launch your app in the foreground by either tapping a tile or the text link to your app.

3.  **Build the response**

    Call [**RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583) to show the disambiguation screen in **Cortana**.

4.  **Show the disambiguation screen**

    Here's an example of a disambiguation screen with content tiles.

```    CSharp
// Create a VoiceCommandUserMessage for the initial question.  
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage = "Which one do you want to cancel?";
    userPrompt.SpokenMessage = 
      “Which Vegas trip do you wanna cancel? Vegas Tech Conference or Fun in Vegas?”;


    // Create a VoiceCommandUserMessage for the second question,
    // in case Cortana needs to reprompt. 
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage = “Which one did you want to cancel?”;
    userReprompt.SpokenMessage = "Which one did you wanna to cancel?";

    // Create the list of content tiles to show the selection items.
    var destinationsContentTiles = new List<VoiceCommandContentTile>();

    var destinationTile = new VoiceCommandContentTile();
    destinationTile.ContentTileType = 
      VoiceCommandContentTileType.TitleWith68x68IconAndText;
      
    // The AppContext is optional. 
    // Replace this value with something specific to your app. 
    destinationTile.AppContext = "id_Vegas_001";
    destinationTile.Title = "Vegas Tech Conference";

    destinationTile.TextLine1 = "May 15th";


    destinationsContentTiles.Add(destinationTile);

    var destination2 = new VoiceCommandContentTile();
    destination2.ContentTileType = 
      VoiceCommandContentTileType.TitleWith68x68IconAndText;
    // The AppContext is optional. 
    // Replace this value with something specific to your app. 
    destination2.AppContext = "id_LasVegas_002";

    destination2.Title = "Fun in Vegas";

    destination2.TextLine1 = "August 24th";

    destinationsContentTiles.Add(destination2);

    // Create the disambiguation response.
    var response = 
      VoiceCommandResponse.CreateResponseForPrompt(
        userPrompt, userReprompt, destinationsContentTiles);

    // Request that Cortana shows the Disambiguation screen.
    var voiceCommandDisambiguationResult = 
      await voiceServiceConnection.RequestDisambiguationAsync(response);

    if (voiceCommandDisambiguationResult != null)
    {
       // Use the voiceCommandDisambiguationResult.SelectedItem to take action.
       // Call Cortana to present the next screen in .5 seconds   
       // and avoid a transition screen. 
    }
```

## <span id="Error_screen"></span><span id="error_screen"></span><span id="ERROR_SCREEN"></span>Error screen


When an action specified by a voice command cannot be completed, an app service can provide an error screen.

Here's an example of an error screen for the **Adventure Works** app. In this example, a user has instructed the app service to cancel a trip to Las Vegas through **Cortana**. However, the user does not have any trips scheduled to Las Vegas.

The app service provides **Cortana** with an error screen that includes a message customized for the action, an icon, and the specific error message.

1.  **Choose the feedback strings to be displayed and spoken by Cortana**

    Follow the [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for recommendations on composing strings that **Cortana** shows and speaks.

2.  **Choose content tiles based on the action performed (optional)**

    Content tiles can provide additional context for the user and help keep the feedback strings concise.

    **Cortana** supports the following content tile templates (only one template can be used on the completion screen):

    -   Title only
    -   Title with up to three lines of text
    -   Title with icon
    -   Title with icon and up to three lines of text

    The icon can be:

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

    You can also let users launch your app in the foreground by either tapping a tile or the text link to your app.

3.  **Build the response**

    Call [**ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578) to show the error screen in **Cortana**.

4.  **Show the error screen**

    Here's an example of an error screen.

```    CSharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don&#39;t have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <span id="related_topics"></span>Related articles


**Developers**
* [Cortana interactions](cortana-interactions.md)
* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Speech design guidelines](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Samples**
* [Cortana voice command sample](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 




<!--HONumber=Mar16_HO1-->
