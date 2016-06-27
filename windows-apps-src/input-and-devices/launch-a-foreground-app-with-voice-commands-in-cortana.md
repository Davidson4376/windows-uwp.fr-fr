---
author: Karl-Bridge-Microsoft
Description: "Les commandes sont dédiées aux fonctionnalités système, mais vous pouvez lancer une application au premier plan, ou spécifier une action ou une commande."
title: Lancer une application au premier plan avec les commandes de Cortana
ms.assetid: 8D3D1F66-7D17-4DD1-B426-DCCBD534EF00
label: Cortana-Launch a foreground app
template: detail.hbs
ms.sourcegitcommit: 7cbea3c4e784fe024aef953e3ea757dad6c5e3b8
ms.openlocfilehash: aa4d71525d4a41382b8bbe123ca1fa830a4fc720

---

# Activer une application au premier plan avec les commandes vocales via Cortana

En plus d’utiliser les commandes vocales de **Cortana** pour accéder aux fonctionnalités du système, vous pouvez également étendre les fonctionnalités et fonctions de **Cortana** à partir de votre application. À l’aide des commandes vocales, vous pouvez activer votre application au premier plan et exécuter une action ou une commande au sein de l’application. 

**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Lorsqu’une application gère une commande vocale au premier plan, elle prend le focus et Cortana se ferme. Si vous préférez, vous pouvez activer votre application et exécuter une commande en tant que tâche en arrière-plan. Dans ce cas, Cortana conserve le focus et votre application renvoie tous les commentaires et les résultats via le canevas de **Cortana** et la voix de **Cortana**.

Les commandes vocales qui requièrent un contexte supplémentaire ou une entrée utilisateur (comme l’envoi d’un message à un contact spécifique) sont mieux gérées dans une application au premier plan, tandis que les commandes de base (comme la création de la liste des prochains voyages) peuvent être gérées dans **Cortana** par le biais d’une application en arrière-plan.

Si vous souhaitez activer une application en arrière-plan à l’aide des commandes vocales, voir [Activer une application en arrière-plan avec des commandes vocales via Cortana](launch-a-background-app-with-voice-commands-in-cortana.md).

> **Remarque**  
> Une commande vocale est un énoncé unique avec une intention spécifique, défini dans un fichier VCD (Voice Command Definition), qui est dirigé vers une application installée par le biais de **Cortana**.

> Un fichier VCD définit une ou plusieurs commandes vocales, chacune avec une intention unique.

> La définition de la commande vocale peut varier en complexité. Elle peut prendre en charge un énoncé unique et limité ou une collection d’énoncés plus naturels et flexibles, indiquant tous la même intention.

Pour faire la démonstration des fonctionnalités de l’application en arrière-plan, nous allons utiliser une application de planification et de gestion de voyages, nommée **Adventure Works**, extraite de l’[exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Pour créer un voyage **Adventure Works** sans **Cortana**, un utilisateur doit lancer l’application et accéder à la page **Nouveaux voyages**. Pour afficher un voyage existant, l’utilisateur doit lancer l’application et accéder à la page **Prochains voyages**, puis sélectionner le voyage.

À l’aide des commandes vocales de **Cortana**, l’utilisateur peut également énoncer à haute voix une commande simple, comme « Adventure Works ajouter un voyage » ou « Ajouter un voyage sur Adventure Works », pour lancer l’application et accéder à la page **Nouveaux voyages**. Ensuite, en prononçant « Adventure Works, afficher mon voyage pour Londres », l’utilisateur lance l’application et accède à la page de détails **Voyage**, illustrée ici.

![lancement d’une application au premier plan avec Cortana](images/cortana-foreground-with-adventureworks.png)

Les étapes de base pour l’ajout de commandes vocales et l’intégration de Cortana à votre application en utilisant les fonctions vocale ou l’entrée au clavier sont les suivantes :

1.  Créez un fichier VCD. Il s’agit d’un document XML qui définit toutes les commandes vocales que l’utilisateur peut prononcer pour lancer des actions ou appeler des commandes au moment de l’activation de votre application. Voir [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
2.  Enregistrez les jeux de commandes dans le fichier VCD lorsque l’application est lancée.
3.  Gérez l’activation par commande vocale, la navigation dans l’application et l’exécution de la commande.

**Éléments requis :  **

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Recommandations en matière d’expérience utilisateur :  **

Pour des informations sur la manière d’intégrer votre application à **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233). Pour obtenir de précieux conseils sur la conception d’une application dotée de fonctions vocales à la fois utile et conviviale, voir [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <span id="Create_a_new_solution_with_project_in_Visual_Studio"></span><span id="create_a_new_solution_with__project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH__PROJECT_IN_VISUAL_STUDIO"></span>Créer une solution avec un projet dans Visual Studio


1.  Lancez Microsoft Visual Studio 2015.

    La page de démarrage de Visual Studio 2015 apparaît.

2.  Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.

    La boîte de dialogue **Nouveau projet** s’affiche. Le volet gauche de la boîte de dialogue vous permet de sélectionner le type de modèle à afficher.

3.  Dans le volet gauche, développez **Installé &gt; Modèles &gt; Visual C\# &gt; Windows**, puis sélectionnez le groupe de modèles **Universel**. Le volet central de la boîte de dialogue affiche une liste de modèles de projets pour les applications de plateforme Windows universelle (UWP).
4.  Dans le volet central, sélectionnez le modèle **Application vide (Windows universelle)**.

    Le modèle **Application vide** crée une application UWP dépouillée qui peut être compilée et exécutée, mais qui ne contient aucun contrôle d’interface utilisateur ni aucune donnée. Au cours de ce didacticiel, vous allez ajouter des contrôles à l’application.

5.  Dans la zone de texte **Nom**, tapez le nom de votre projet. Pour cet exemple, nous utilisons « AdventureWorks ».
6.  Cliquez sur **OK** pour créer le projet.

    Microsoft Visual Studio crée votre projet et l’affiche dans l’**Explorateur de solutions**.

## <span id="Add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>Ajouter des ressources d’image au projet et les spécifier dans le manifeste de l’application
      
Les applications UWP peuvent sélectionner automatiquement les images les plus appropriées en fonction de paramètres spécifiques et des fonctionnalités de l’appareil (contraste élevé, pixels effectifs, paramètres régionaux, etc.). Il vous suffit de fournir les images et de vérifier que vous utilisez la convention d’affectation de noms et l’organisation de dossiers appropriées dans le projet d’application pour les différentes versions de ressources. Si vous ne fournissez les versions de ressources recommandées, l’accessibilité, la localisation et la qualité des images peuvent être affectées, en fonction des préférences de l’utilisateur, de ses aptitudes, du type d’appareil utilisé et de l’emplacement.

Pour plus d’informations sur les ressources d’image pour les facteurs de contraste élevé et d’échelle, voir [Recommandations en matière de ressources de vignette et d’icône](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Les ressources sont nommées à l’aide de qualificateurs. Les qualificateurs de ressources sont des modificateurs de noms de fichiers et de dossiers qui identifient le contexte dans lequel une version particulière d’une ressource doit être utilisée.

La convention d’affectation de noms standard est `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Par exemple, `images/en-US/logo.scale-100_contrast-white.png`, est identifié dans le code par le dossier racine et le nom de fichier : `images/logo.png`. Voir [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx).

Nous vous conseillons de marquer la langue par défaut dans les fichiers de ressources de type chaîne (par exemple, `en-US\resources.resw`) et le facteur d’échelle par défaut dans les images (par exemple,`logo.scale-100.png`), même si dans l’immédiat, vous ne pensez pas localiser ces ressources, ni les proposer avec plusieurs résolutions. Toutefois, nous vous recommandons de fournir au minimum des ressources pour les facteurs d’échelle 100, 200 et 400.

> *Important

> L’icône de l’application utilisée dans la zone de titre du canevas de **Cortana** est l’icône Square44x44Logo spécifiée dans le fichier « Package.appxmanifest ». 
    
## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>Créer un fichier VCD

1. Dans Visual Studio, cliquez avec le bouton droit sur le nom du projet principal, sélectionnez **Ajouter &gt; Nouvel élément**. Ajoutez un **fichier XML**.
2. Tapez le nom du fichier [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) (dans cet exemple, AdventureWorksCommands.xml), puis cliquez sur Ajouter. 
3. Dans l’**Explorateur de solutions**, sélectionnez le fichier [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593).
4.  Dans la fenêtre **Propriétés**, affectez à **Action de génération** la valeur **Contenu**, puis définissez **Copier dans le répertoire de sortie** sur **Copier si plus récent**.

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>Modifier le fichier VCD


Ajoutez un élément **VoiceCommands** comportant un attribut **xmlns** pointant sur `http://schemas.microsoft.com/voicecommands/1.2`.

2. Pour chaque langue prise en charge par votre application, créez un élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) contenant les commandes vocales prises en charge par votre application.

  Vous pouvez déclarer plusieurs éléments [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) en leur affectant un attribut [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) différent selon le marché dans lequel vous souhaitez rendre votre application disponible. Par exemple, une application destinée au marché américain peut inclure un élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) pour l’anglais et un autre élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) pour l’espagnol.

  >  **Attention**  
  Pour activer une application et lancer une action à l’aide d’une commande vocale, l’application doit inscrire un fichier VCD contenant un élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) avec une langue qui correspond à la langue vocale sélectionnée par l’utilisateur sur son appareil. La langue vocale se trouve dans **Paramètres &gt; Système &gt; Voix &gt; Langue vocale**.

3. Ajoutez un élément **Command** pour chaque commande à prendre en charge.

  Chaque **Command** déclarée dans un fichier [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) doit inclure les informations suivantes :

  - Un attribut **Name** que votre application utilise pour identifier la commande vocale lors de l’exécution. 
  - Un élément **Example** contenant une expression qui explique comment un utilisateur peut appeler la commande. **Cortana** affiche cet exemple lorsque l’utilisateur dit « Qu’est-ce que je dis ? » ou « Aide », ou qu’il appuie sur **Plus**.    
  -   Un élément **ListenFor** contenant les mots ou expressions que l’application doit reconnaître en tant que commande. Chaque élément **ListenFor** peut contenir des références à un ou plusieurs éléments **PhraseList** contenant des mots pertinents pour la commande.
  > **Remarque**  
Les éléments   **ListenFor** ne peuvent pas être modifiés par programme. Cependant, les éléments **PhraseList** associés à des éléments **ListenFor** peuvent être modifiés par programme. Les applications doivent modifier le contenu de l’élément **PhraseList** lors de l’exécution, en fonction de l’ensemble des données généré lorsque l’application est utilisée. Voir [Modifier de manière dynamique les listes d’expressions de définition des commandes vocales (VCD)](dynamically-modify-voice-command-definition--vcd--phrase-lists.md).

  -   Un élément **Feedback** contenant le texte que **Cortana** affichera et prononcera lors du lancement de l’application.

Un élément **Navigate** indique que la commande vocale active l’application au premier plan. Dans cet exemple, la commande ```showTripToDestination``` est une tâche au premier plan.

Un élément **VoiceCommandService** indique que la commande vocale active l’application au premier plan. La valeur de l’attribut **Target** de cet élément doit correspondre à la valeur de l’attribut **Name** de l’élément [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) dans le fichier package.appxmanifest. Dans cet exemple, les commandes ```whenIsTripToDestination``` et ```cancelTripToDestination``` sont des tâches en arrière-plan qui spécifient le nom du service d’application en tant que « AdventureWorksVoiceCommandService ».

Pour des détails complémentaires, voir les informations de référence [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).

Voici une partie du fichier [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) qui définit les commandes vocales en-us pour l’application **Adventure Works**.

```xml
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
```

## <span id="Install_the_VCD_commands"></span><span id="install_the_vcd_commands"></span><span id="INSTALL_THE_VCD_COMMANDS"></span>Installer les commandes VCD


Votre application doit s’exécuter une fois pour installer les commandes VCD. 

>  **Remarque**  
Les données des commandes vocales ne sont pas conservées entre les installations de l’application. Pour vous assurer que les données des commandes vocales de votre application restent intactes, il est conseillé d’initialiser votre fichier VCD chaque fois que votre application est démarrée ou activée. Vous pouvez aussi conserver un paramètre qui indique si le fichier VCD est actuellement installé.

Ouvrez le fichier « app.xaml.cs » :

1. Ajoutez la directive using suivante :  
```csharp
using Windows.Storage;
```
2. Marquez la méthode « OnLaunched » avec le modificateur async.  
```csharp
protected async override void OnLaunched(LaunchActivatedEventArgs e)
```
3. Appelez [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) dans le gestionnaire [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) pour inscrire les commandes vocales que le système doit reconnaître.

  Dans l’exemple Adventure Works, nous définissons tout d’abord un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). 

  Ensuite, nous appelons [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) pour l’initialiser avec notre fichier AdventureWorksCommands.xml.

  Cet objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) est ensuite transmis à [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205).    
```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
    await Package.Current.InstalledLocation.GetFileAsync(
      @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
    InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Gérer l’activation des commandes vocales et les exécuter

Spécifiez la manière dont votre application répond aux activations par commande vocale ultérieures (après au moins un lancement et une fois que les jeux de commande vocale ont été installés).

1.  Vérifiez que votre application a été activée par une commande vocale.

    Remplacez l’événement [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) et vérifiez si [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) est [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Identifier le nom de la commande et le texte prononcé.

    Obtenir une référence à un objet [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) à partir de [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) et interroger la propriété [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) d’un objet [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432).

    Pour déterminer ce que l’utilisateur a dit, vérifiez la valeur de l’élément [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) ou les propriétés sémantiques de l’expression reconnue dans le dictionnaire [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

3.  Effectuez l’action requise dans votre application, par exemple en accédant à la page appropriée.

Pour les besoins de cet exemple, nous nous reportons au fichier VCD mentionné à l’étape 3 : Modifier le fichier VCD.

Le résultat de la reconnaissance vocale pour la commande vocale permet d’obtenir le nom de la commande dans la première valeur du tableau [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438). Comme le fichier VCD définit plusieurs commandes vocales possibles, nous devons comparer la valeur par rapport aux noms de commande de ce fichier et agir en conséquence.

L’action la plus courante qu’une application peut exécuter consiste à naviguer vers une page dont le contenu est pertinent par rapport au contexte de la commande vocale. Pour les besoins de cet exemple, nous allons accéder à une page **TripPage** et transmettre la valeur de la commande vocale, ainsi que le mode d’entrée de la commande et la phrase de destination reconnue (le cas échéant). L’application peut également envoyer un paramètre de navigation à l’élément [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) lorsque vous accédez à la page.

Vous pouvez déterminer si la commande vocale qui a lancé votre application a été prononcée ou si elle a été tapée sous forme de texte. Pour cela, utilisez la clé **commandMode** dans le dictionnaire [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445). La valeur de cette clé est voice ou text. Si la valeur de la clé est voice, envisagez d’utiliser la synthèse vocale ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) dans votre application afin de fournir des commentaires à l’oral.

Utilisez l’élément [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) pour rechercher le contenu prononcé dans les contraintes **PhraseList** ou **PhraseTopic** d’un élément **ListenFor**. La clé de dictionnaire est la valeur de l’attribut **Label** de l’élément **PhraseList** ou **PhraseTopic**. Voici comment accéder à la valeur de l’expression **{destination}**.

``` csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <span id="related_topics"></span>Articles connexes


**Développeurs**
* [Interactions avec Cortana](cortana-interactions.md)
* [Définir des contraintes de reconnaissance vocale personnalisées](define-custom-recognition-constraints.md)
* [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Concepteurs**
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


