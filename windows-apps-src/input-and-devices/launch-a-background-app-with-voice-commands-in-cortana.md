---
Description: Vous pouvez utiliser des commandes vocales dans Cortana pour accéder aux fonctionnalités système. Vous pouvez également étendre Cortana avec les fonctionnalités d’une application en arrière-plan à l’aide des commandes vocales qui spécifient une action ou une commande à exécuter au sein de l’application.
title: Lancer une application en arrière-plan à l’aide des commandes vocales de Cortana
ms.assetid: DF5B530C-57DD-4CA5-B3BE-1A0B3695C9C6
label: Launch a background app
template: detail.hbs
---

# Activer une application en arrière-plan avec les commandes vocales de Cortana


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Vous pouvez utiliser des commandes vocales dans **Cortana** pour accéder aux fonctionnalités système. Vous pouvez également étendre **Cortana** avec les fonctionnalités de votre application (sous la forme d’une tâche en arrière-plan) à l’aide des commandes vocales qui spécifient une action ou une commande à exécuter. Lorsqu’une application gère une commande vocale en arrière-plan, elle ne prend pas le focus. À la place, elle renvoie tous les commentaires et résultats par le biais du canevas de **Cortana** et de la voix de **Cortana**.

Selon la complexité de l’interaction, les applications peuvent être activées au premier plan (elle prennent alors le focus) ou en arrière-plan (**Cortana** conserve le focus). Par exemple, les commandes vocales qui requièrent un contexte supplémentaire ou une entrée utilisateur (comme l’envoi d’un message à un contact spécifique) sont mieux gérées dans une application au premier plan, tandis que les commandes de base (comme la création de la liste des prochains voyages) peuvent être gérées dans **Cortana** par le biais d’une application en arrière-plan.

Si vous souhaitez activer une application au premier plan à l’aide des commandes vocales, voir [Activer une application au premier plan avec les commandes vocales via Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md).

> **Remarque**  
> Une commande vocale est un énoncé unique avec une intention spécifique, défini dans un fichier VCD (Voice Command Definition), qui est dirigé vers une application installée par le biais de **Cortana**.

> Un fichier VCD définit une ou plusieurs commandes vocales, chacune avec une intention unique.

> La définition de la commande vocale peut varier en complexité. Elle peut prendre en charge un énoncé unique et limité ou une collection d’énoncés plus naturels et flexibles, indiquant tous la même intention.

Pour faire la démonstration des fonctionnalités de l’application en arrière-plan, nous allons utiliser une application de planification et de gestion de voyages, nommée **Adventure Works**, extraite de l’[exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899).

Voici une vue d’ensemble de l’application **Adventure Works** intégrée dans le canevas de **Cortana**.

![vue d’ensemble du canevas de Cortana ](images/cortana-overview.png)

Pour afficher un voyage **Adventure Works** sans **Cortana**, un utilisateur devrait lancer l’application et accéder à la page **Prochains voyages**.

En ayant recours aux commandes vocales de **Cortana** pour lancer votre application en arrière-plan, l’utilisateur peut dire simplement « Adventure Works, quand a lieu mon prochain voyage pour Las Vegas ? ». Votre application traite la commande, et **Cortana** affiche les résultats avec l’icône de l’application et d’autres informations sur l’application, le cas échéant. Voici un exemple de requête de voyage de base et l’écran de résultats de **Cortana** qui affiche et dit « Votre prochain voyage pour Las Vegas aura lieu le 1er août ».

![requête de base et écran de résultats utilisant l’application Adventure Works en arrière-plan](images/cortana-backgroundapp-result.png)

Voici les étapes de base permettant d’ajouter des commandes vocales et d’étendre **Cortana** avec les fonctionnalités en arrière-plan de votre application à l’aide de fonctions vocales ou d’entrée au clavier :

1.  Créez un service d’application (voir [**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731)) que **Cortana** appelle en arrière-plan.
2.  Créez un fichier VCD. Il s’agit d’un document XML qui définit toutes les commandes vocales que l’utilisateur peut prononcer pour lancer des actions ou appeler des commandes au moment de l’activation de votre application. Voir [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).
3.  Enregistrez les jeux de commandes dans le fichier VCD lorsque l’application est lancée.
4.  Gérez l’activation en arrière-plan du service d’application ainsi que l’exécution de la commande vocale.
5.  Affichez et prononcez les commentaires appropriés pour la commande vocale dans **Cortana**.

**Prérequis : **

Si vous débutez dans le développement d’applications de plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Recommandations en matière d’expérience utilisateur : **

Pour des informations sur la manière d’intégrer votre application à **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233). Pour obtenir de précieux conseils sur la conception d’une application, dotée de fonctions vocales, à la fois utile et conviviale, voir [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <span id="Create_a_new_solution_with_a_primary_project_in_Visual_Studio"> </span> <span id="create_a_new_solution_with_a_primary_project_in_visual_studio"> </span> <span id="CREATE_A_NEW_SOLUTION_WITH_A_PRIMARY_PROJECT_IN_VISUAL_STUDIO"> </span>Créer une solution avec un projet principal dans Visual Studio


1.  Lancez Microsoft Visual Studio 2015.

    La page de démarrage de Visual Studio 2015 apparaît.

2.  Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.

    La boîte de dialogue **Nouveau projet** s’affiche. Le volet gauche de la boîte de dialogue vous permet de sélectionner le type de modèle à afficher.

3.  Dans le volet gauche, développez **Installé > Modèles > Visual C\# > Windows**, puis sélectionnez le groupe de modèles **Universel**. Le volet central de la boîte de dialogue affiche une liste de modèles de projets pour les applications de plateforme Windows universelle (UWP).
4.  Dans le volet central, sélectionnez le modèle **Application vide (Windows universelle)**.

    Le modèle **Application vide** crée une application UWP dépouillée qui peut être compilée et exécutée, mais qui ne contient aucun contrôle d’interface utilisateur ni aucune donnée. Au cours de ce didacticiel, vous allez ajouter des contrôles à l’application.

5.  Dans la zone de texte **Nom**, tapez le nom de votre projet. Pour cet exemple, nous utilisons « AdventureWorks ».
6.  Cliquez sur **OK** pour créer le projet.

    Microsoft Visual Studio crée votre projet et l’affiche dans l’**Explorateur de solutions**.


## <span id="Add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="ADD_IMAGE_ASSETS_TO_PRIMARY_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"> </span>Ajouter des ressources d’image au projet principal et les spécifier dans le manifeste de l’application
      
Les applications UWP peuvent sélectionner automatiquement les images les plus appropriées en fonction de paramètres spécifiques et des fonctionnalités de l’appareil (contraste élevé, pixels effectifs, paramètres régionaux, etc.). Il vous suffit de fournir les images et de vérifier que vous utilisez la convention d’affectation de noms et l’organisation de dossiers appropriées dans le projet d’application pour les différentes versions de ressources. Si vous ne fournissez les versions de ressources recommandées, l’accessibilité, la localisation et la qualité des images peuvent être affectées, en fonction des préférences de l’utilisateur, de ses aptitudes, du type d’appareil utilisé et de l’emplacement.

Pour plus d’informations sur les ressources d’image pour les facteurs de contraste élevé et d’échelle, voir [Recommandations en matière de ressources de vignette et d’icône](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets).

Les ressources sont nommées à l’aide de qualificateurs. Les qualificateurs de ressources sont des modificateurs de noms de fichiers et de dossiers qui identifient le contexte dans lequel une version particulière d’une ressource doit être utilisée.

La convention d’affectation de noms standard est `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`. Par exemple, `images/en-US/logo.scale-100_contrast-white.png` est simplement identifié dans le code par le dossier racine et le nom de fichier : `images/logo.png`. Voir [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh965324.aspx).

Nous vous conseillons de marquer la langue par défaut dans les fichiers de ressources de type chaîne (par exemple, `en-US\resources.resw`) et le facteur d’échelle par défaut dans les images (par exemple,`logo.scale-100.png`), même si dans l’immédiat, vous ne pensez localiser ces ressources, ni les proposer avec plusieurs résolutions. Toutefois, nous vous recommandons de fournir au minimum des ressources pour les facteurs d’échelle 100, 200 et 400.

> *Important

> L’icône de l’application utilisée dans la zone de titre du canevas de **Cortana** est l’icône Square44x44Logo spécifiée dans le fichier « Package.appxmanifest ». 

> Vous pouvez également spécifier une icône pour chaque entrée de la zone de contenu du canevas **Cortana**. Les formats d’image valides pour les icônes de résultats sont les suivants :

> - 68 x 68 
> - 68 x 92 
> - 280 x 140 

La vignette de contenu n’est pas validée tant qu’aucun élément [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) n’a été transmis à l’élément [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204). Si vous transmettez à **Cortana** un objet **VoiceCommandResponse** contenant une vignette de contenu avec une image qui n’est pas conforme à ces rapports de taille, une exception peut se produire. 

Dans l’exemple de l’application **Adventure Works** (VoiceCommandService\\AdventureWorksVoiceCommandService.cs), nous utilisons un carré gris simple (GreyTile.png) sur [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) à l’aide du modèle de vignette [**TitleWith68x68IconAndText**](https://msdn.microsoft.com/library/windows/apps/dn974169). Les variantes de logo se trouvent dans VoiceCommandService\\Images, et sont récupérées à l’aide de la méthode [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741).

```CSharp
var destinationTile = new VoiceCommandContentTile();

destinationTile.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = 
  await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
```

## <span id="Create_an_app_service_project"> </span> <span id="create_an_app_service_project"> </span> <span id="CREATE_AN_APP_SERVICE_PROJECT"> </span>Créer un projet de service d’application

<ol>
    <li>
    Right-click your Solution name, select **New > Project**.
    </li>
    <li>
    Under **Installed > Templates > Visual C# > Windows > Universal**, select **Windows Runtime Component**. This is the component that implements the app service (**[Windows.ApplicationModel.AppService](https://msdn.microsoft.com/library/windows/apps/dn921731)**).
    </li>
    <li>
    Type a name for the project (for example, "VoiceCommandService") and click **OK**.
    </li>
    <li>
    In **Solution Explorer**, select the "VoiceCommandService" project and rename the "Class1.cs" file generated by Visual Studio. For the **Adventure Works** example we use "AdventureWorksVoiceCommandService.cs".
    </li>
    <li>
    Click **Yes** when asked if you want to rename all occurrences of "Class1.cs". 
    </li>
    <li>
    In the "AdventureWorksVoiceCommandService.cs" file:
        <ol type="i">
 <li>
 Ajoutez la directive using suivante.  
 ```using Windows.ApplicationModel.Background;```
 </li>
 <li>
 Lorsque vous créez un projet, le nom du projet est utilisé comme espace de noms racine par défaut dans tous les fichiers. Renommez l’espace de noms pour incorporer le code du service d’application sous le projet principal. Exemple : `namespace AdventureWorks.VoiceCommands`. 
 </li>
 <li>
 Cliquez avec le bouton droit sur le nom du projet de service d’application dans l’Explorateur de solutions et sélectionnez **Propriétés**. 
 </li>
 <li>
 Dans l’onglet **Bibliothèque**, mettez à jour le champ **Espace de noms par défaut** avec cette même valeur (dans cet exemple, « AdventureWorks.VoiceCommands »). 
 </li>
 <li>
 Créez une classe qui implémente l’interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Cette classe nécessite une méthode [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811), qui constitue le point d’entrée lorsque Cortana reconnaît la commande vocale. 
 </li>
        </ol>
    </li>
</ol>

Voici une classe de tâche en arrière-plan de base provenant de l’application **Adventure Works**. Nous en fournirons tous les détails ultérieurement.
> **Remarque**   
> La classe de tâche en arrière-plan proprement dite, ainsi que toutes les autres classes au sein du projet de tâche en arrière-plan, doivent être des classes « public sealed ».
 
``` csharp
namespace AdventureWorks.VoiceCommands
{
    ...
    
    /// <summary>
    /// The VoiceCommandService implements the entry point for all voice commands.
    /// The individual commands supported are described in the VCD xml file. 
    /// The service entry point is defined in the appxmanifest.
    /// </summary>
    public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
    {
        ...
        
        /// <summary>
        /// The background task entrypoint. 
        /// 
        /// Background tasks must respond to activation by Cortana within 0.5 seconds, and must 
        /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
        /// input). There is no execution time limit on the background task managed by Cortana,
        /// but developers should use plmdebug (https://msdn.microsoft.com/en-us/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
        /// on the Cortana app package in order to prevent Cortana timing out the task during
        /// debugging.
        /// 
        /// The Cortana UI is dismissed if Cortana loses focus. 
        /// The background task is also dismissed even if being debugged. 
        /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
        /// Open the project properties for the app package (not the background task project), 
        /// and enable Debug -> "Do not launch, but debug my code when it starts". 
        /// Alternatively, add a long initial progress screen, and attach to the background task process while it executes.
        /// </summary>
        /// <param name="taskInstance">Connection to the hosting background service process.</param>
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        
          //
          // TODO: Insert code 
          //
          //
        
    }        
  }
}
```

<ol start="7">
    <li>
    Declare your background task as an **AppService** in the app manifest.
    <ol type="i">
        <li>
        In **Solution Explorer**, right click the "Package.appxmanifest" file and select **View Code**. 
        </li>
        <li>
        Find the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add an [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element to the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add a [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to the [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element.
        </li>
        <li>Ajoutez un attribut **Category** à l’élément **uap:Extension**, et définissez la valeur de l’attribut **Category** sur « windows.appService ».
        </li>
        <li>
        Add an **EntryPoint** attribute to the **uap:Extension** element and set the value of the **EntryPoint** attribute to the name of the class that implements [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794), in this case "AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element to the **uap:Extension** element.
        </li>
        <li>
        Add a **Name** attribute to the [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element and set the value of the **Name** attribute to a name for the app service, in this case "AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a second [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720).
        </li>
        <li>
        Add a **Category** attribute to this [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element and set the value of the **Category** attribute to "windows.personalAssistantLaunch".
        </li>
    </li> 
    </ol>
    </li>    
</ol>  

Voici le manifeste de l’application Adventure Works :
```xml
<Package>
  <Applications>
    <Application>

      <Extensions>
        <uap:Extension Category="windows.appService" 
          EntryPoint="CortanaBack1.VoiceCommands.CortanaBack1VoiceCommandService">
          <uap:AppService Name="CortanaBack1VoiceCommandService"/>
        </uap:Extension>
        <uap:Extension Category="windows.personalAssistantLaunch"/>
      </Extensions>

    <Application>
  <Applications>
</Package>
```

<ol start="8">
    <li>
    Add this app service project as a reference in the primary project. 
    <ol type="i">
        <li>
        Right click **References**. 
        </li>
        <li>
        Select **Add Reference...** 
        </li>
        <li>
        In the **Reference Manager** dialog, expand **Projects** and select the app service project. 
        </li>
        <li>
        Click OK. 
        </li>
    </ol>
    </li>
</ol>

## <span id="Create_a_VCD_file"> </span> <span id="create_a_vcd_file"> </span> <span id="CREATE_A_VCD_FILE"> </span>Créer un fichier VCD


1. Dans Visual Studio, cliquez avec le bouton droit sur le nom du projet principal et sélectionnez **Ajouter > Nouvel élément**. Ajoutez un **fichier XML**.
2. Tapez un nom pour le fichier [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) (dans cet exemple, « AdventureWorksCommands.xml »), puis cliquez sur Ajouter. 
3. Dans l’**Explorateur de solutions**, sélectionnez le fichier [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593).
4.  Dans la fenêtre **Propriétés**, affectez à **Action de génération** la valeur **Contenu**, puis affectez à l’option **Copier dans le répertoire de sortie** la valeur **Copier si plus récent**.

## <span id="Edit_the_VCD_file"> </span> <span id="edit_the_vcd_file"> </span> <span id="EDIT_THE_VCD_FILE"> </span>Modifier le fichier VCD

1. Ajoutez un élément **VoiceCommands** avec un attribut **xmlns** pointant sur « http://schemas.microsoft.com/voicecommands/1.2 ».

2. Pour chaque langue prise en charge par votre application, créez un élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) contenant les commandes vocales prises en charge par votre application.

  Vous pouvez déclarer plusieurs éléments [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) en leur affectant un attribut [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) différent selon le marché dans lequel vous souhaitez rendre votre application disponible. Par exemple, une application destinée au marché américain peut inclure un élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) pour l’anglais et un autre élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) pour l’espagnol.

  >  **Attention**  
  Pour activer une application et lancer une action à l’aide d’une commande vocale, l’application doit inscrire un fichier VCD contenant un élément [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) avec une langue qui correspond à la langue vocale sélectionnée par l’utilisateur sur son appareil. La langue vocale se trouve dans **Paramètres > Système > Voix > Langue vocale**.

3. Ajoutez un élément **Command** pour chaque commande à prendre en charge.

  Chaque élément **Command** déclaré dans un fichier [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) doit inclure les informations suivantes :

  - Un attribut **Name** que votre application utilise pour identifier la commande vocale lors de l’exécution. 
  - Un élément **Example** contenant une expression qui explique comment un utilisateur peut appeler la commande. **Cortana** affiche cet exemple lorsque l’utilisateur dit « Qu’est-ce que je dis ? » ou « Aide », ou qu’il appuie sur **Plus**.    
  -   Un élément **ListenFor** contenant les mots ou expressions que l’application doit reconnaître en tant que commande. Chaque élément **ListenFor** peut contenir des références à un ou plusieurs éléments **PhraseList** contenant des mots pertinents pour la commande.
  > **Remarque**  
Les éléments   **ListenFor** ne peuvent pas être modifiés par programme. Cependant, les éléments **PhraseList** associés à des éléments **ListenFor** peuvent être modifiés par programme. Les applications doivent modifier le contenu de l’élément **PhraseList** lors de l’exécution, en fonction de l’ensemble des données généré lorsque l’utilisateur exécute l’application. Voir [Modifier de manière dynamique les listes d’expressions de définition des commandes vocales (VCD)](dynamically-modify-voice-command-definition--vcd--phrase-lists.md).

  -   Un élément **Feedback** contenant le texte que **Cortana** affichera et prononcera lors du lancement de l’application.

Un élément **Navigate** indique que la commande vocale active l’application au premier plan. Dans cet exemple, la commande ```showTripToDestination``` est une tâche au premier plan.

Un élément **VoiceCommandService** indique que la commande vocale active l’application à l’arrière-plan. La valeur de l’attribut **Target** de cet élément doit correspondre à la valeur de l’attribut **Name** de l’élément [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) dans le fichier package.appxmanifest. Dans cet exemple, les commandes ```whenIsTripToDestination``` et ```cancelTripToDestination``` sont des tâches en arrière-plan qui spécifient le nom du service d’application en tant que « AdventureWorksVoiceCommandService ».

Pour plus de détails, voir les informations de référence [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).

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

## <span id="Install_the_VCD_commands"> </span> <span id="install_the_vcd_commands"> </span> <span id="INSTALL_THE_VCD_COMMANDS"> </span>Installer les commandes VCD

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
```CSharp
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
}```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Handle activation

Specify how your app responds to subsequent voice command activations (after it has been launched at least once and the voice command sets have been installed).

1.  Confirm that your app was activated by a voice command.

    Override the [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) event and check whether [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) is [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determine the name of the command and what was spoken.

    Get a reference to a [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) object from the [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) and query the [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) property for a [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) object.

    To determine what the user said, check the value of [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) or the semantic properties of the recognized phrase in the [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) dictionary.

3.  Take the appropriate action in your app, such as navigating to the desired page.

For this example, we refer back to the VCD in Step 3: Edit the VCD file.

Once we get the speech-recognition result for the voice command, we get the command name from the first value in the [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) array. As the VCD file defined more than one possible voice command, we need to compare the value against the command names in the VCD and take the appropriate action.

The most common action an application can take is to navigate to a page with content relevant to the context of the voice command. For this example, we navigate to a **TripPage** page and pass in the value of the voice command, how the command was input, and the recognized "destination" phrase (if applicable). Alternatively, the app could send a navigation parameter to the [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) when navigating to the page.

You can find out whether the voice command that launched your app was actually spoken, or whether it was typed in as text, from the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) dictionary using the **commandMode** key. The value of that key will be either "voice" or "text". If the value of the key is "voice", consider using speech synthesis ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) in your app to provide the user with spoken feedback.

Use the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) to find out the content spoken in the **PhraseList** or **PhraseTopic** constraints of a **ListenFor** element. The dictionary key is the value of the **Label** attribute of the **PhraseList** or **PhraseTopic** element. Here, we show how to access the value of **{destination}** phrase.

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

## <span id="Handle_the_voice_command_in_the_app_service"> </span> <span id="handle_the_voice_command_in_the_app_service"> </span> <span id="HANDLE_THE_VOICE_COMMAND_IN_THE_APP_SERVICE"> </span>Gérer la commande vocale dans le service d’application


Traitez la commande vocale dans le service d’application.


1.  Ajoutez les directives using ci-après à votre fichier de service de commande vocale, « AdventureWorksVoiceCommandService.cs » dans cet exemple.
```csharp
using Windows.ApplicationModel.VoiceCommands;
using Windows.ApplicationModel.Resources.Core;
using Windows.ApplicationModel.AppService;
```

2.  Effectuez un report de service pour que le service de votre application ne soit pas arrêté lors du traitement de la commande vocale.
3.  Vérifiez que votre tâche en arrière-plan s’exécute en tant que service d’application activé par une commande vocale.

    1.  Associez [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) à [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](https://msdn.microsoft.com/library/windows/apps/dn921727).
    2.  Vérifiez que [**IBackgroundTaskInstance.TriggerDetails.Name**](https://msdn.microsoft.com/library/windows/apps/br224807) correspond au nom du service d’application dans le fichier Package.appxmanifest.

4.  Utilisez [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) pour créer un élément [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204), afin que **Cortana** puisse récupérer la commande vocale.
5.  Inscrivez un gestionnaire d’événements pour l’élément [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).[**VoiceCommandCompleted**](https://msdn.microsoft.com/library/windows/apps/dn706584) pour recevoir une notification lorsque le service d’application ferme en raison de l’annulation de l’utilisateur.
6.  Inscrivez un gestionnaire d’événements pour l’élément [**IBackgroundTaskInstance.Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) pour recevoir une notification lorsque le service d’application ferme en raison d’un échec inattendu.
7.  Identifiez le nom de la commande et le texte prononcé par l’utilisateur.

    1.  Utilisez la propriété [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/dn974162).[**CommandName**](https://msdn.microsoft.com/library/windows/apps/dn706589) pour déterminer le nom de la commande vocale.
    2.  Pour déterminer ce que l’utilisateur a dit, vérifiez la valeur de l’élément [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) ou les propriétés sémantiques de l’expression reconnue dans le dictionnaire [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443).

7.  Effectuez l’action appropriée dans le service de votre application.
8.  Affichez et prononcez les commentaires pour la commande vocale dans **Cortana**.

    1.  Déterminez les chaînes que vous souhaitez que **Cortana** affiche et prononce pour l’utilisateur en réponse à la commande vocale, et créez un objet [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182). Pour obtenir des recommandations sur la sélection des chaînes de commentaires affichées et prononcées par **Cortana**, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233).
    2.  Utilisez l’instance [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) afin de signaler à **Cortana** l’avancement ou l’achèvement d’une fonction en appelant [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) ou [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) à l’aide de l’objet **VoiceCommandServiceConnection**.

    Pour les besoins de cet exemple, nous nous reportons au fichier VCD mentionné à l’étape 3 : Modifier le fichier VCD.

```csharp
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

Une fois activé, le service d’application a 0,5 seconde pour appeler [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580). **Cortana** utilise les données fournies par l’application pour afficher et prononcer les commentaires spécifiés dans le fichier VCD. Si l’application met plus de 5 secondes à effectuer l’appel, **Cortana** insère un écran relais, comme illustré ici. **Cortana** affiche l’écran relais tant que l’application n’a pas appelé **ReportSuccessAsync**, ou pendant 5 secondes au plus. Si le service d’application n’appelle pas **ReportSuccessAsync** ni aucune des méthodes [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) qui fournissent des informations à **Cortana**, l’utilisateur reçoit un message d’erreur et le service d’application est annulé.

![requête de base et écrans de progression et de résultats utilisant l’application Adventure Works en arrière-plan](images/cortana-backgroundapp-progress-result.png)

## <span id="related_topics"> </span>Articles connexes


**Développeurs**
* [Interactions avec Cortana](cortana-interactions.md)
* [Définir des contraintes de reconnaissance vocale personnalisées](define-custom-recognition-constraints.md)
* [Interagir avec une application en arrière-plan dans Cortana](interact-with-a-background-app-in-cortana.md)
* [**Éléments et attributs d’un fichier VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
* [Démarrage rapide : utilisation de ressources de fichiers ou d’images](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)
* [Comment nommer des ressources à l’aide de qualificateurs](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)

**Concepteurs**
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Conception réactive 101 pour les applications pour UWP](https://msdn.microsoft.com/library/windows/apps/dn958435)
* [Recommandations en matière de ressources de vignette et d’icône](https://msdn.microsoft.com/library/windows/apps/mt412102)

**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


