---
In addition to using voice commands within Cortana to access system features, you can also extend Cortana with features and functionality from a background app using voice commands that specify an action or command to execute within the app.
Launch a background app with voice commands in Cortana
ms.assetid: DF5B530C-57DD-4CA5-B3BE-1A0B3695C9C6
Launch a background app
template: detail.hbs
---

# Launch a background app with voice commands in Cortana


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**Important APIs**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

In addition to using voice commands within **Cortana** to access system features, you can also extend **Cortana** with features and functionality from a background app using voice commands that specify an action or command to execute within the app. When an app handles a voice command in the background, it can display feedback on the **Cortana** canvas and communicate with the user using the **Cortana** voice.

**Note**  
A voice command is a single utterance with a specific intent, defined in a Voice Command Definition (VCD) file, directed at an installed app through **Cortana**.

A voice command definition can vary in complexity. It can support anything from a single, constrained utterance to a collection of more flexible, natural language utterances, all denoting the same intent.

A VCD file defines one or more voice commands, each with a unique intent.

The target app can be launched in the foreground (the app takes focus) or activated in the background (**Cortana** retains focus but provides results from the app), depending on the complexity of the interaction. For example, voice commands that require additional context or user input (such as sending a message to a specific contact) are best handled in a foreground app, while basic commands can be handled in **Cortana** through a background app.

 

We demonstrate these features here with a trip planning and management app named **Adventure Works** from the [Cortana voice command sample](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Here's an overview of the **Adventure Works** app integrated with the **Cortana** canvas.

![cortana canvas overview ](images/cortana-overview.png)

To view an **Adventure Works** trip without **Cortana**, a user would launch the app and navigate to the **Upcoming trips** page.

Using voice commands through **Cortana** to launch your app in the background, the user can instead just say, "Adventure Works, when is my trip to Las Vegas?". Your app handles the command and **Cortana** displays results along with your app icon and other app info, if provided. Here's an example of a basic trip query and **Cortana** result screen that both shows and speaks "Your next trip to Las Vegas is on August 1st".

![a basic query and result screen using the adventure works app in the background](images/cortana-backgroundapp-result.png)

These are the basic steps to add voice-command functionality and extend **Cortana** with background functionality from your app using speech or keyboard input:

1.  Create an app service (see [**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731)) that **Cortana** invokes in the background.
2.  Create a VCD file. This is an XML document that defines all the spoken commands that the user can say to initiate actions or invoke commands when activating your app. See [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
3.  Register the command sets in the VCD file when the app is launched.
4.  Handle the background activation of the app service and the execution of the voice command.
5.  Display and speak the appropriate feedback to the voice command within **Cortana**.

**Prerequisites:  **

If you're new to developing Universal Windows Platform (UWP) apps, have a look through these topics to get familiar with the technologies discussed here.

-   [Create your first app](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Learn about events with [Events and routed events overview](https://msdn.microsoft.com/library/windows/apps/mt185584)

**User experience guidelines:  **

See [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233) for info about how to integrate your app with **Cortana** and [Speech design guidelines](https://msdn.microsoft.com/library/windows/apps/dn596121) for helpful tips on designing a useful and engaging speech-enabled app.

## <span id="Create_a_new_solution_with_a_primary_project_in_Visual_Studio"></span><span id="create_a_new_solution_with_a_primary_project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH_A_PRIMARY_PROJECT_IN_VISUAL_STUDIO"></span>Create a new solution with a primary project in Visual Studio


1.  Launch Microsoft Visual Studio 2015.

    The Visual Studio 2015 Start page appears.

2.  On the **File** menu, select **New** &gt; **Project**.

    The **New Project** dialog appears. The left pane of the dialog lets you select the type of templates to display.

3.  In the left pane, expand **Installed &gt; Templates &gt; Visual C\# &gt; Windows**, then pick the **Universal** template group. The dialog's center pane displays a list of project templates for Universal Windows Platform (UWP) apps.
4.  In the center pane, select the **Blank App (Universal Windows)** template.

    The **Blank App** template creates a minimal UWP app that compiles and runs, but contains no user-interface controls or data. You add controls to the app over the course of this tutorial.

5.  In the **Name** text box, type your project name.
6.  Click **OK** to create the project.

    Microsoft Visual Studio creates your project and displays it in the **Solution Explorer**.

## <span id="Create_an_app_service_project"></span><span id="create_an_app_service_project"></span><span id="CREATE_AN_APP_SERVICE_PROJECT"></span>Create an app service project


1.  Right-click your Solution name, select Add-&gt;New Project, and then select **Windows Runtime Component**. This is the component that will implement the app service (see [**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731)).
2.  Type a name for the project (for example, "VoiceCommandService") and click **OK**.
3.  In **Solution Explorer**, select the "VoiceCommandService" project and rename the "Class1.cs" file generated by Visual Studio. For example, "YourAppVoiceCommandService.cs".
4.  In the "YourAppVoiceCommandService.cs" file, add the following using directive.
```    CSharp
using Windows.ApplicationModel.Background;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

5.  Create a new class that implements the [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) interface. The [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) method of this class is the required entry point, called when **Cortana** recognizes the voice command.

    **Note**  The background task class itself, and all other classes in the background task project, need to be sealed public classes.

     

    Here's a basic background task class for the **Adventure Works** app.

    Note that we've renamed the default namespace assigned by Visual Studio.

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
using Windows.ApplicationModel.Background;

    namespace AdventureWorks.VoiceCommands
    {
      public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
      {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
          BackgroundTaskDeferral _deferral = taskInstance.GetDeferral();
        
          //
          // TODO: Insert code 
          //
          //
        
          _deferral.Complete();
        }        
      }
    }
```

6.  Declare your background task as an **AppService** in the app manifest.

    1.  In **Solution Explorer**, right click the "Package.appxmanifest" file and select **View Code**.
    2.  Find the [****Application****](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
    3.  Add an [****Extensions****](https://msdn.microsoft.com/library/windows/apps/dn934720) element to the [****Application****](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
    4.  Add a [****uap:Extension****](https://msdn.microsoft.com/library/windows/apps/dn986788) element to the [****Extensions****](https://msdn.microsoft.com/library/windows/apps/dn934720) element.
    5.  Add a **Category** attribute to the **uap:Extension** element and set the value of the **Category** attribute to "windows.appService".
    6.  Add an **EntryPoint** attribute to the **uap:Extension** element and set the value of the **EntryPoint** attribute to the name of the class that implements [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794), in this case "AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService".
    7.  Add a [****uap:AppService****](https://msdn.microsoft.com/library/windows/apps/dn934779) element to the **uap:Extension** element.
    8.  Add a **Name** attribute to the [****uap:AppService****](https://msdn.microsoft.com/library/windows/apps/dn934779) element and set the value of the **Name** attribute to a name for the app service, in this case "AdventureWorksVoiceCommandService".
```        XML
<Package>
          <Applications>
            <Application>

              <Extensions>
                <uap:Extension Category="windows.appService" 
                  EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
                  <uap:AppService Name="AdventureWorksVoiceCommandService"/>
                </uap:Extension>
                <uap:Extension Category="windows.personalAssistantLaunch"/>
              </Extensions>

            <Application>
          <Applications>
        </Package>
```

## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Create a VCD file


1.  In Visual Studio, right-click your primary project name, select Add-&gt;New Item, and then select **Text File**.
2.  Type a name for the [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) file and be sure to append the ".xml" file extension. For example, "AdventureWorksCommands.xml". Click **Add**.
3.  In **Solution Explorer**, select the [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) file.
4.  In the **Properties** window, set **Build action** to **Content**, and then set **Copy to output directory** to **Copy if newer**.

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Edit the VCD file


For each language supported by your app, create a **CommandSet** of voice commands that your app can handle.

Each **Command** declared in a [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) file must include this information:

-   A command Name used by the application to identify the voice command at runtime
-   An **Example** element that contains a phrase describing how a user can invoke the command. **Cortana** shows this example when the user says "What can I say?", "Help", or they tap **See more**.

    see

-   A **ListenFor** element that contains the words or phrases that your app recognizes to initiate a command. Each command needs to have at least one **ListenFor** element.
-   A **Feedback** element that contains the text for **Cortana** to display and speak as the application is launched.
-   A **VoiceCommandService** element to indicate the voice command launches the app in the background.

For more detail, see the [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593) reference.

You can specify multiple language versions for the commands used to activate your app and execute a command. You can create multiple [****CommandSet****](https://msdn.microsoft.com/library/windows/apps/dn722331) elements, each with a different [****xml:lang****](https://msdn.microsoft.com/library/windows/apps/dn722331) attribute to allow your app to be used in different markets. For example, an app for the United States might have a [****CommandSet****](https://msdn.microsoft.com/library/windows/apps/dn722331) for English and a [****CommandSet****](https://msdn.microsoft.com/library/windows/apps/dn722331) for Spanish.

**Caution**  
To activate an app and initiate an action using a voice command, the app must register a VCD file that contains a [****CommandSet****](https://msdn.microsoft.com/library/windows/apps/dn722331) with a language that matches the speech language that the user selected on their device. This language is set by the user on the device Settings &gt; System &gt; Speech &gt; Speech Language screen.

 

Here's a [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) file that defines a voice command for the **Adventure Works** app.

For this example, **CommandPrefix** is set to "Adventure Works", **Command** is set to "whenIsTripToDestination", **ListenFor** specifies the text that can be recognized (with a reference to a **PhraseList** element that constrains the recognized destinations), **VoiceCommandService** indicates that the voice command is handled by an app service in the background, and **Feedback** specifies what the user will hear when **Cortana** launches the app service.

The value of the **Target** attribute of the **VoiceCommandService** element needs to be the name of the app service as specified in the [****uap:AppService****](https://msdn.microsoft.com/library/windows/apps/dn934779) element of the package.appxmanifest. In this example, the name of the app service is "AdventureWorksVoiceCommandService".

The "whenIsTripToDestination" command has a **ListenFor** element with a reference to a **PhraseList** for a constrained set of destinations.

**ListenFor** elements cannot be programmatically modified. However, **PhraseList** elements associated with **ListenFor** elements can be programmatically modified. Applications should modify the content of the **PhraseList** at runtime based on the data set generated as the user uses the app. See [Dynamically modify Voice Command Definition (VCD) phrase lists](dynamically-modify-voice-command-definition--vcd--phrase-lists.md).

```XML
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
    <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
</VoiceCommands>
```

## <span id="Install_the_VCD_commands"></span><span id="install_the_vcd_commands"></span><span id="INSTALL_THE_VCD_COMMANDS"></span>Install the VCD commands


Your app must run once to install the command sets in the VCD.

When your app is activated, call [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) in the [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) handler to register the commands that the system should listen for.

**Note**  If a device backup occurs and your app reinstalls automatically, voice command data is not preserved. To ensure the voice command data for your app stays intact, consider initializing your VCD file each time your app launches or activates, or store a setting that indicates if the VCD is currently installed and check the setting each time your app launches or activates.

 

In this example, we first define a [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) object.

We then call [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) to initialize it with our "AdventureWorksCommands.xml" file.

This [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) object is then passed to [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205).

```CSharp
try
{
    // Install the VCD. 
    StorageFile vcdStorageFile = 
        await Package.Current.InstalledLocation.GetFileAsync(
        @"AdventureWorksCommands.xml");

    await 
        Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
        InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);
}
catch (Exception ex)
{
    System.Diagnostics.Debug.WriteLine(
      "Installing Voice Commands Failed: " + ex.ToString());
}
```

## <span id="Handle_the_voice_command_in_the_app_service"></span><span id="handle_the_voice_command_in_the_app_service"></span><span id="HANDLE_THE_VOICE_COMMAND_IN_THE_APP_SERVICE"></span>Handle the voice command in the app service


Specify in the app service how your app responds to voice-command activations after it has launched and the voice command sets have been installed.

1.  Take a service deferral so your app service is not terminated while handling the voice command.
2.  Confirm that your background task is running as an app service activated by a voice command.

    1.  Cast the [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) to [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](https://msdn.microsoft.com/library/windows/apps/dn921727).
    2.  Check that [**IBackgroundTaskInstance.TriggerDetails.Name**](https://msdn.microsoft.com/library/windows/apps/br224807) is the name of the app service in the "Package.appxmanifest" file.

3.  Use [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) to create a [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) to **Cortana** to retrieve the voice command.
4.  Register an event handler for [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).[**VoiceCommandCompleted**](https://msdn.microsoft.com/library/windows/apps/dn706584) to receive notification when the app service is closed due to a user cancellation.
5.  Register an event handler for the [**IBackgroundTaskInstance.Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) to receive notification when the app service is closed due to an unexpected failure.
6.  Determine the name of the command and what was spoken.

    1.  Use the [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/dn974162).[**CommandName**](https://msdn.microsoft.com/library/windows/apps/dn706589) property to determine the name of the voice command.
    2.  To determine what the user said, check the value of [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) or the semantic properties of the recognized phrase in the [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) dictionary.

7.  Take the appropriate action in your app service.
8.  Display and speak the feedback to the voice command with **Cortana**.

    1.  Determine the strings that you want **Cortana** to display and speak to the user in response to the voice command and create a [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) object. For guidance on how to select the feedback strings that **Cortana** shows and speaks, see [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233).
    2.  Use the [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) instance to report progress or completion to **Cortana** by calling [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) or [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) with the **VoiceCommandServiceConnection** object.

    For this example, we refer back to the VCD in Step 3: Edit the VCD file.

```    CSharp
public sealed class VoiceCommandService : IBackgroundTask
    {
      private BackgroundTaskDeferral serviceDeferral;
      VoiceCommandServiceConnection voiceServiceConnection;

      public async void Run(IBackgroundTaskInstance taskInstance)
      {
      //Take a service deferral so the service isn&#39;t terminated.
        this.serviceDeferral = taskInstance.GetDeferral();

        taskInstance.Canceled += OnTaskCanceled;

        var triggerDetails = 
          taskInstance.TriggerDetails as AppServiceTriggerDetails;

        if (triggerDetails != null &amp;&amp; 
          triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint")
        {
          try
          {
            voiceServiceConnection = 
              VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
                triggerDetails);

            voiceServiceConnection.VoiceCommandCompleted += 
              VoiceCommandCompleted;

            VoiceCommand voiceCommand = await
            voiceServiceConnection.GetVoiceCommandAsync();

            switch (voiceCommand.CommandName)
            {
              case "whenIsTripToDestination":
              {
                var destination = 
                  voiceCommand.Properties["destination"][0];
                SendCompletionMessageForDestination(destination);
                break;
              }

              // As a last resort, launch the app in the foreground.
              default:
                LaunchAppInForeground();
                break;
            }
          }
          finally
          {
            if (this.serviceDeferral != null)
            {
              // Complete the service deferral.
              this.serviceDeferral.Complete();
            }
          }
        }
      }

      private void VoiceCommandCompleted(
        VoiceCommandServiceConnection sender, 
        VoiceCommandCompletedEventArgs args)
      {
        if (this.serviceDeferral != null)
        {
          // Insert your code here.
          // Complete the service deferral.
          this.serviceDeferral.Complete();
        }
      }

      private async void SendCompletionMessageForDestination(
        string destination)
      {
        // Take action and determine when the next trip to destination
        // Insert code here.
        
        // Replace the hardcoded strings used here with strings 
        // appropriate for your application.

        // First, create the VoiceCommandUserMessage with the strings 
        // that Cortana will show and speak.
        var userMessage = new VoiceCommandUserMessage();
        userMessage.DisplayMessage = "Here’s your trip.";
        userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";

        // Optionally, present visual information about the answer.
        // For this example, create a VoiceCommandContentTile with an 
        // icon and a string.
        var destinationsContentTiles = new List<VoiceCommandContentTile>();

        var destinationTile = new VoiceCommandContentTile();
        destinationTile.ContentTileType = 
          VoiceCommandContentTileType.TitleWith68x68IconAndText;
        // The user can tap on the visual content to launch the app. 
        // Pass in a launch argument to enable the app to deep link to a 
        // page relevant to the item displayed on the content tile.
        destinationTile.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        destinationTile.Title = "Las Vegas";
        destinationTile.TextLine1 = "August 3rd 2015";
        destinationsContentTiles.Add(destinationTile);

        // Create the VoiceCommandResponse from the userMessage and list    
        // of content tiles.
        var response = 
          VoiceCommandResponse.CreateResponse(
            userMessage, destinationsContentTiles);

        // Cortana will present a “Go to app_name” link that the user 
        // can tap to launch the app. 
        // Pass in a launch to enable the app to deep link to a page 
        // relevant to the voice command.
        response.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        
        // Ask Cortana to display the user message and content tile and 
        // also speak the user message.
        await voiceServiceConnection.ReportSuccessAsync(response);
      }

      private async void LaunchAppInForeground()
      {
        var userMessage = new VoiceCommandUserMessage();
        userMessage.SpokenMessage = "Launching Adventure Works";

        var response = VoiceCommandResponse.CreateResponse(userMessage);

        // When launching the app in the foreground, pass an app 
        // specific launch parameter to indicate what page to show.
        response.AppLaunchArgument = "showAllTrips=true";

        await voiceServiceConnection.RequestAppLaunchAsync(response);
      }
    }
```

Once launched, the app service has .5 seconds to call [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580). **Cortana** uses the data provided by the app to show and say the feedback specified in the VCD file. If the app takes longer than .5 seconds to make the call, **Cortana** inserts a hand-off screen, as shown here. **Cortana** displays the hand-off screen until the application calls **ReportSuccessAsync**, or for up to 5 seconds. If the app service doesn’t call **ReportSuccessAsync**, or any of the [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) methods that provide **Cortana** with information, the user receives an error message and the app service is cancelled.

![a basic query with progress and result screens using the adventure works app in the background](images/cortana-backgroundapp-progress-result.png)

## <span id="Image_resources_and_scaling"></span><span id="image_resources_and_scaling"></span><span id="IMAGE_RESOURCES_AND_SCALING"></span>Image resources and scaling


UWP apps can automatically select the most appropriate image based on specific settings and device capabilities (high contrast, effective pixels, locale, and so on). All you need to do is provide the images and ensure you use the appropriate naming convention and folder organization within the app project for the different resource versions. If you don't provide the recommended resource versions, accessibility, localization, and image quality can suffer, depending on the user's preferences, abilities, device type, and location.

For more detail on image resources for high contrast and scale factors, see [Guidelines for tile and icon assets](https://msdn.microsoft.com/library/windows/apps/mt412102).

You name resources using qualifiers. Resource qualifiers are folder and filename modifiers that identify the context in which a particular version of a resource should be used.

The standard naming convention is "foldername/qualifiername-value\[\_qualifiername-value\]/filename.qualifiername-value\[\_qualifiername-value\].ext". For example: images/en-US/logo.scale-100\_contrast-white.png is simply referred to in code using the root folder and the filename: images/logo.png. See [Guidelines for files, data, and globalization](https://msdn.microsoft.com/library/windows/apps/dn611859) and [How to name resources using qualifiers](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

We recommend that you mark the default language on string resource files (such as "en-US\\resources.resw") and the default scale factor on images (such as "logo.scale-100.png"), even if you do not currently plan to provide localized or multiple resolution resources. However, you should provide assets for 100, 200, and 400 scale factors.

**Important**  
Valid icon sizes for the **Cortana** content tile are:

-   68w x 68h
-   68w x 92h
-   280w x 140h

The content tile is not validated until a [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) is passed to the [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204). If you pass a **VoiceCommandResponse** object to **Cortana** that contains a content tile with an image that does not adhere to these size ratios, an exception might occur.

 

In this example from the **Adventure Works** app (VoiceCommandService\\AdventureWorksVoiceCommandService.cs), we specify a simple grey square ("GreyTile.png") as the app logo on the [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) using the [**TitleWith68x68IconAndText**](https://msdn.microsoft.com/library/windows/apps/dn974169) tile template. The logo variants are located in VoiceCommandService\\Images, and are retrieved using the [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) method.

```CSharp
var destinationTile = new VoiceCommandContentTile();

destinationTile.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = 
  await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
```

## <span id="related_topics"></span>Related articles


**Developers**
* [Cortana interactions](cortana-interactions.md)
* [Define custom recognition constraints](define-custom-recognition-constraints.md)
* [Interact with a background app in Cortana](interact-with-a-background-app-in-cortana.md)
* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
* [Quickstart: Using file or image resources](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)
* [How to name resources using qualifiers](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
**Designers**
* [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Speech design guidelines](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Responsive design 101 for UWP apps](https://msdn.microsoft.com/library/windows/apps/dn958435)
* [Guidelines for tile and icon assets](https://msdn.microsoft.com/library/windows/apps/mt412102)
**Samples**
* [Cortana voice command sample](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 




<!--HONumber=Mar16_HO1-->
