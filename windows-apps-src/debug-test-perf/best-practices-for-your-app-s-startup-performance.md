---
author: jwmsft
ms.assetid: 00ECF6C7-0970-4D5F-8055-47EA49F92C12
title: Meilleures pratiques en matière de performances lors du démarrage de votre application
description: Créez des applications de plateforme Windows universelle (UWP) dont le temps de démarrage est optimal en améliorant la gestion du lancement et de l’activation.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25ddcc6c9ceaecd858733a0222c22c18682041b8
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6152408"
---
# <a name="best-practices-for-your-apps-startup-performance"></a>Meilleures pratiques en matière de performances lors du démarrage de votre application


Créez des applications de plateforme Windows universelle (UWP) dont le temps de démarrage est optimal en améliorant la gestion du lancement et de l’activation.

## <a name="best-practices-for-your-apps-startup-performance"></a>Meilleures pratiques en matière de performances lors du démarrage de votre application

Les utilisateurs jugent les performances d’une application en partie sur le temps nécessaire à son démarrage. Pour les besoins de cette rubrique, le démarrage d’une application commence lorsque l’utilisateur démarre l’application et il se termine lorsque l’utilisateur peut commencer à interagir véritablement avec l’application. Cette section fournit des suggestions pour améliorer les performances de votre application au démarrage.

### <a name="measuring-your-apps-startup-time"></a>Évaluation du temps nécessaire au démarrage de votre application

Démarrez l’application plusieurs fois avant de pouvoir évaluer son temps de démarrage. Cela vous servira de référence afin d’estimer un temps de démarrage aussi court que possible, tout en restant dans la mesure du raisonnable.

Au moment où votre application UWP arrive sur les ordinateurs de vos clients, votre application a été compilée à l’aide de la chaîne d’outils .NET Native. .NET Native est une technologie de compilation d’avant-garde qui convertit le MSIL en code machine exécutable en mode natif. Les applications .NET Native démarrent plus vite, utilisent moins de mémoire et consomment moins de batterie que leurs équivalents MSIL. Les applications générées avec .NET Native se lient de manière statistique dans le cadre d’une exécution personnalisée et dans le nouveau .NET Core convergé pouvant s’exécuter sur tous les appareils, afin qu’elles ne dépendent pas de l’implémentation de .NET fournie. Sur l’ordinateur de développement, votre application utilise .NET Native par défaut si vous la créez en mode « Publication », et CoreCLR si vous la créez en mode « Débogage ». Vous pouvez configurer cette option dans Visual Studio à partir de la page de génération dans « Propriétés » (C#) ou Compiler -&gt; Avancé dans « Mon projet » (VB). Recherchez une case à cocher indiquant « Compiler avec la chaîne d’outils .NET Native ».

Bien entendu, vous devez obtenir des évaluations représentatives de ce que l’utilisateur final constatera. Par conséquent, si vous n’êtes pas certain de compiler votre application en code natif sur l’ordinateur de développement, vous pouvez exécuter l’outil Native Image Generator (Ngen.exe) pour précompiler votre application avant d’évaluer son temps de démarrage.

La procédure suivante décrit comment exécuter Ngen.exe pour précompiler votre application.

**Pour exécuter Ngen.exe**

1.  Exécutez votre application au moins une fois pour vérifier que Ngen.exe la détecte.
2.  Ouvrez le **Planificateur de tâches** en effectuant l’une des opérations suivantes :
    -   Recherchez « Planificateur de tâches » dans l’écran d’accueil.
    -   Exécutez « taskschd.msc ».
3.  Dans le volet de gauche du **Planificateur de tâches**, développez **Bibliothèque du Planificateur de tâches**.
4.  Développez **Microsoft.**
5.  Développez **Windows.**
6.  Sélectionnez **.NET Framework**.
7.  Sélectionnez **.NET Framework NGEN 4.x** dans la liste des tâches.

    Si vous utilisez un ordinateur 64 bits, **.NET Framework NGEN v4.x 64** est également disponible. Si vous créez une application 64 bits, sélectionnez .**NET Framework NGEN v4.x 64**.

8.  Dans le menu **Action**, cliquez sur **Exécuter**.

Ngen.exe précompile toutes les applications de l’ordinateur qui ont été utilisées et qui n’ont pas d’images natives. Si de nombreuses applications doivent être précompilées, cette opération peut demander du temps, mais les précompilations suivantes seront plus rapides.

Lorsque vous recompilez votre application, l’image native n’est plus utilisée. L’application est alors compilée juste-à-temps, c’est-à-dire qu’elle est compilée en cours d’exécution. Vous devez réexécuter Ngen.exe pour obtenir une nouvelle image native.

### <a name="defer-work-as-long-as-possible"></a>Différer le travail aussi longtemps que possible

Pour réduire le temps de démarrage de votre application, n’effectuez que le travail qui est absolument nécessaire pour permettre à l’utilisateur de commencer à interagir avec l’application. Cela peut être particulièrement utile si vous pouvez retarder le chargement d’assemblys supplémentaires. Le Common Language Runtime charge un assembly la première fois qu’il est utilisé. Si vous pouvez réduire le nombre d’assemblys qui sont chargés, vous pourrez sans doute améliorer le temps nécessaire au démarrage de votre application et sa consommation de mémoire.

### <a name="do-long-running-work-independently"></a>Effectuer le travail demandant du temps séparément

Votre application peut être interactive même si certaines de ses parties ne sont pas totalement fonctionnelles. Par exemple, si votre application affiche des données qui sont longues à récupérer, vous pouvez faire en sorte que le code chargé de récupérer ces données s’exécute indépendamment du code de démarrage de l’application en récupérant les données de façon asynchrone. Lorsque les données sont disponibles, fournissez-les à l’interface utilisateur de l’application.

De nombreuses API de plateforme Windows universelle (UWP) qui récupèrent les données sont asynchrones. Pour cette raison, la récupération des données s’effectuera probablement de façon asynchrone. Pour en savoir plus sur les API asynchrones, voir [Appel d’API asynchrones en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). Si vous effectuez un travail qui ne nécessite pas l’utilisation d’API asynchrones, vous pouvez utiliser la classe Task pour effectuer les tâches devant s’exécuter sur le long terme, afin de ne pas empêcher l’utilisateur d’interagir avec l’application. L’application continuera à répondre aux actions de l’utilisateur pendant que les données sont chargées.

Si le chargement d’une partie de l’interface utilisateur de l’application est particulièrement long, nous vous conseillons d’ajouter une ligne à cet égard, en indiquant quelque chose comme « Obtention des dernières données en cours » afin que les utilisateurs sachent que l’application est toujours en cours de traitement.

## <a name="minimize-startup-time"></a>Réduire le temps de démarrage

Hormis les plus simples d’entre elles, les applications nécessitent un laps de temps, perceptible par l’utilisateur, pour charger les ressources, analyser le code XAML, configurer les structures de données et exécuter la logique au moment de l’activation. Dans cette rubrique, nous allons analyser le processus d’activation en le décomposant en trois phases. Nous vous donnerons également des conseils pour réduire la durée de chaque phase, ainsi que quelques techniques pour rendre chaque phase du démarrage de votre application plus agréable pour l’utilisateur.

La période d’activation désigne le laps de temps s’écoulant entre le démarrage de l’application par l’utilisateur et le moment où l’application est opérationnelle. Ce laps de temps a une grande importance, car il conditionne la première impression qu’un utilisateur se fait de votre application. Les utilisateurs s’attendent à pouvoir interagir instantanément et sans interruption avec le système et leurs applications. Ils jugeront que le système et les applications ne sont pas fiables ou qu’ils sont mal conçus si les applications ne démarrent pas assez vite. Pire encore, si une application met trop de temps à s’activer, le Gestionnaire de durée de vie d’un processus (PLM) risque de l’arrêter ou l’utilisateur de la désinstaller.

### <a name="introduction-to-the-stages-of-startup"></a>Présentation des étapes de démarrage

Le démarrage implique un certain nombre d’éléments mobiles devant être correctement coordonnés pour optimiser l’expérience utilisateur. Les étapes suivantes se déroulent entre le moment où l’utilisateur clique sur la vignette de votre application et celui où le contenu de l’application s’affiche.

-   L’environnement Windows démarre le processus et Main est appelé.
-   L’objet Application est créé.
    -   (Project template) Constructor appelle InitializeComponent qui entraîne l’analyse de App.xaml et la création des objets.
-   L’événement Application.OnLaunched est déclenché.
    -   Le code (ProjectTemplate) App crée un Frame et navigue vers MainPage.
    -   Le constructeur (ProjectTemplate) Mainpage appelle InitializeComponent qui entraîne l’analyse de MainPage.xam et la création des objets.
    -   ProjectTemplate) Window.Current.Activate() est appelé.
-   La plateforme XAML exécute la passe de disposition, notamment Mesurer et disposer.
    -   ApplyTemplate entraîne la création du contenu du modèle de contrôle pour chaque contrôle, qui représente généralement la majeure partie du temps de disposition au démarrage.
-   Render est appelé pour créer des effets visuels pour tous les contenus de fenêtre.
-   Frame est présenté au gestionnaire de fenêtrage (DWM).

### <a name="do-less-in-your-startup-path"></a>Effectuer un moins grand nombre d’opérations dans votre chemin de démarrage

Retirez de votre code de démarrage tout ce qui n’est pas nécessaire à votre première image.

-   Si vous disposez de DLL utilisateur contenant des contrôles qui ne sont pas nécessaires pour la première image, envisagez de retarder leur chargement.
-   Si une partie de votre interface utilisateur dépend de données issues du cloud, fractionnez-la. Commencez par afficher l’interface utilisateur qui n’est pas dépendante des données du cloud et affichez ensuite l’interface utilisateur dépendante du cloud de manière asynchrone. Vous devez également envisager la mise en cache locale des données pour que l’application fonctionne en mode hors connexion ou ne soit pas affectée par un ralentissement de la connexion réseau.
-   Afficher la progression de l’interface utilisateur si votre interface utilisateur est en attente de données.
-   Soyez vigilant à l’égard des conceptions d’applications impliquant une quantité importante d’analyses de fichiers de configuration ou une interface utilisateur générée de manière dynamique par du code.

### <a name="reduce-element-count"></a>Réduire le nombre d’éléments

Les performances de démarrage d’une application XAML sont en corrélation directe avec le nombre d’éléments créés lors du démarrage. Moins vous créez d’éléments et plus le démarrage de votre application est rapide. À titre de référence, la création de chaque élément doit prendre environ 1 ms.

-   Les modèles utilisés dans les contrôles d’éléments peuvent avoir un impact majeur, dans la mesure où ils sont répétés plusieurs fois. Voir [Optimisation des options d’interface ListView et GridView](optimize-gridview-and-listview.md)
-   UserControls et les modèles de contrôle sont étendus et doivent donc également être pris en compte.
-   Si vous créez du code XAML qui ne figure pas sur l’écran, vous devez indiquer pourquoi ces parties de code doivent être créées lors du démarrage.

La fenêtre [Arborescence visuelle dynamique de Visual Studio](http://blogs.msdn.com/b/visualstudio/archive/2015/02/24/introducing-the-ui-debugging-tools-for-xaml.aspx) indique le nombre d’éléments enfant pour chaque nœud de l’arborescence.

![Arborescence visuelle dynamique](images/live-visual-tree.png)

**Différez le chargement**. Le fait de réduire un élément ou de définir son opacité sur 0 n’empêche pas la création de l’élément. L’utilisation de x:Load ou de x:DeferLoadStrategy vous permet de différer le chargement d’un élément d’interface utilisateur et de le charger quand c’est nécessaire. Cela est très pratique pour retarder le traitement de l’interface utilisateur qui n’est pas visible sur l’écran de démarrage, afin de pouvoir la charger en cas de besoin, ou dans le cadre d’une logique différée. Pour déclencher le chargement, il vous suffit d’appeler FindName pour l’élément. Pour plus d’informations et pour obtenir un exemple d’utilisation, consultez les articles [Attribut x:Load](../xaml-platform/x-load-attribute.md) et [Attribut x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785).

**Virtualisation** Si votre interface utilisateur comporte un contenu de liste ou Repeater, il est vivement recommandé d’utiliser la virtualisation de l’interface utilisateur. Si l’interface utilisateur de liste n’est pas virtualisée, vous devez créer sur le moment tous les éléments, ce qui peut ralentir le démarrage. Voir [Optimisation des options d’interface ListView et GridView](optimize-gridview-and-listview.md)

Les performances d’une application reposent non seulement sur les performances brutes, mais également sur leur perception. La modification de l’ordre des opérations visant à afficher en priorité les aspects visuels donne l’impression à l’utilisateur que l’application est plus rapide. Les utilisateurs considèrent que l’application est chargée une fois le contenu affiché à l’écran. Le plus souvent, les applications doivent effectuer plusieurs opérations au démarrage, qui ne sont pas toutes nécessaires pour afficher l’interface utilisateur, et qui par conséquent, doivent être retardées ou reléguées au second plan.

Cette rubrique traite de la « première image » qui provient de l’animation/de la télévision, et qui est une mesure du temps que met un contenu avant d’être visible à l’utilisateur final.

### <a name="improve-startup-perception"></a>Améliorer la perception du démarrage

Prenons l’exemple d’un jeu en ligne simple pour nous aider à identifier chaque phase du démarrage et les différentes techniques garantissant à l’utilisateur une réactivité optimale pendant tout le processus. Pour cet exemple, la première phase d’activation est le temps écoulé entre le moment où l’utilisateur appuie sur la vignette du jeu et celui où le jeu commence à exécuter son code. Pendant cette phase, le système ne dispose d’aucune information à afficher pour indiquer à l’utilisateur que le jeu sélectionné a démarré. L’affichage d’un écran de démarrage permet au système de fournir ce contenu. Le jeu informe ensuite l’utilisateur que la première phase d’activation est terminée. Pour cela, il remplace l’écran de démarrage statique par sa propre interface utilisateur dès qu’il commence à exécuter le code.

La deuxième phase d’activation englobe la création et l’initialisation des structures les plus importantes du jeu. Si l’application parvient à créer rapidement son interface utilisateur initiale à partir des données issues de la première phase d’activation, la deuxième phase consiste simplement à afficher directement l’interface utilisateur. Si la création prend un peu plus de temps, il est préférable que l’application affiche une page de chargement pendant la durée de son initialisation.

Vous pouvez choisir quelles informations afficher dans la page de chargement de votre application : cela peut être tout simplement une barre ou un anneau de progression. L’essentiel est d’indiquer à l’utilisateur que l’application n’est pas réactive actuellement, car elle est en train d’effectuer certaines tâches. Dans notre exemple, l’écran initial du jeu ne peut pas être affiché, car cette interface utilisateur doit d’abord charger en mémoire des images et des sons à partir du disque. Comme ces tâches prennent quelques secondes, l’application tient l’utilisateur informé en remplaçant l’écran de démarrage par une page de chargement. Cette page affiche une animation simple liée au thème du jeu.

La troisième et dernière phase intervient une fois que le jeu dispose des informations minimales requises pour créer une interface utilisateur interactive et l’afficher à la place de la page de chargement. À ce stade, les seules informations fournies au jeu en ligne sont les données que l’application a chargées à partir du disque. Le jeu peut être fourni avec suffisamment de contenu pour pouvoir créer une interface utilisateur interactive, mais s’agissant d’un jeu en ligne, il ne devient pleinement opérationnel qu’après avoir téléchargé des informations supplémentaires sur Internet. Pendant cette phase où le jeu récupère toutes les informations dont il a besoin, l’utilisateur est en mesure d’interagir avec l’interface utilisateur. Les fonctionnalités qui ne sont pas encore disponibles parce qu’elles attendent des informations du web doivent avertir l’utilisateur qu’elles sont en train de télécharger du contenu. L’activation complète d’une application peut nécessiter du temps. C’est pourquoi il est important que toutes les fonctionnalités soient disponibles le plus rapidement possible.

Après avoir identifié les trois phases d’activation du jeu en ligne, nous allons maintenant examiner le code associé.

### <a name="phase-1"></a>Phase 1

Avant de démarrer, l’application doit indiquer au système quel contenu elle souhaite afficher dans l’écran de démarrage. Pour cela, elle transmet une image et une couleur d’arrière-plan à l’élément SplashScreen dans un manifeste de l’application, tel que décrit dans l’exemple. Windows affiche l’écran de démarrage aussitôt après que l’application a lancé le processus d’activation.

```xml
<Package ...>
  ...
  <Applications>
    <Application ...>
      <VisualElements ...>
        ...
        <SplashScreen Image="Images\splashscreen.png" BackgroundColor="#000000" />
        ...
      </VisualElements>
    </Application>
  </Applications>
</Package>
```

Pour plus d’informations, voir [Ajouter un écran de démarrage](https://msdn.microsoft.com/library/windows/apps/Mt187306).

Utilisez le constructeur de l’application uniquement pour initialiser les structures de données qui sont essentielles pour l’application. Le constructeur n’est appelé que la première fois où l’application est exécutée et pas forcément à chaque activation de l’application. Par exemple, le constructeur n’est pas appelé si l’application a déjà été exécutée, placée en arrière-plan, puis activée par le biais du contrat de recherche.

### <a name="phase-2"></a>Phase 2

Une application peut être activée pour plusieurs raisons, chacune pouvant être gérée différemment si vous le souhaitez. Vous pouvez substituer les méthodes [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/BR242330), [**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701797), [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/BR242331), [**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701799), [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701801), [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/BR242335), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/BR242336) et [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701806) pour gérer chaque raison de l’activation. Dans ces méthodes, l’application doit notamment créer une interface utilisateur, affecter cette dernière à [**Window.Content**](https://msdn.microsoft.com/library/windows/apps/BR209051), puis appelez [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046). À ce stade, l’écran de démarrage est remplacé par l’interface utilisateur que l’application a créée. Le contenu affiché peut être la page de chargement, ou l’interface utilisateur actuelle de l’application si toutes les informations nécessaires à sa création sont disponibles au moment de l’activation.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public partial class App : Application
> {
>     // A handler for regular activation.
>     async protected override void OnLaunched(LaunchActivatedEventArgs args)
>     {
>         base.OnLaunched(args);
> 
>         // Asynchronously restore state based on generic launch.
> 
>         // Create the ExtendedSplash screen which serves as a loading page while the
>         // reader downloads the section information.
>         ExtendedSplash eSplash = new ExtendedSplash();
> 
>         // Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash;
> 
>         // Notify the Window that the process of activation is completed
>         Window.Current.Activate();
>     }
> 
>     // a different handler for activation via the search contract
>     async protected override void OnSearchActivated(SearchActivatedEventArgs args)
>     {
>         base.OnSearchActivated(args);
> 
>         // Do an asynchronous restore based on Search activation
> 
>         // the rest of the code is the same as the OnLaunched method
>     }
> }
> 
> partial class ExtendedSplash : Page
> {
>     // This is the UIELement that's the game's home page.
>     private GameHomePage homePage;
> 
>     public ExtendedSplash()
>     {
>         InitializeComponent();
>         homePage = new GameHomePage();
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Public Class App
>     Inherits Application
> 
>     ' A handler for regular activation.
>     Protected Overrides Async Sub OnLaunched(ByVal args As LaunchActivatedEventArgs)
>         MyBase.OnLaunched(args)
> 
>         ' Asynchronously restore state based on generic launch.
> 
>         ' Create the ExtendedSplash screen which serves as a loading page while the
>         ' reader downloads the section information.
>         Dim eSplash As New ExtendedSplash()
> 
>         ' Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash
> 
>         ' Notify the Window that the process of activation is completed
>         Window.Current.Activate()
>     End Sub
> 
>     ' a different handler for activation via the search contract
>     Protected Overrides Async Sub OnSearchActivated(ByVal args As SearchActivatedEventArgs)
>         MyBase.OnSearchActivated(args)
> 
>         ' Do an asynchronous restore based on Search activation
> 
>         ' the rest of the code is the same as the OnLaunched method
>     End Sub
> End Class
> 
> Partial Friend Class ExtendedSplash
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' Downloading the data necessary for 
>         ' initial UI on a background thread.
>         Task.Run(Sub() DownloadData())
>     End Sub
> 
>     Private Sub DownloadData()
>         ' Download data to populate the initial UI.
> 
>         ' Create the first page. 
>         Dim firstPage As New MainPage()
> 
>         ' Add the data just downloaded to the first page
> 
>         ' Replace the loading page, which is currently 
>         ' set as the window's content, with the initial UI for the app
>         Window.Current.Content = firstPage
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class 
> ```

Les applications affichant une page de chargement dans le gestionnaire d’activation commencent à créer l’interface utilisateur en arrière-plan. Une fois que cet élément a été créé, l’événement [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723) associé est déclenché. Dans le gestionnaire d’événements, vous remplacez le contenu de la fenêtre, à savoir la page de chargement, par la nouvelle page d’accueil.

Il est essentiel de prévoir l’affichage d’une page de chargement dans toute application qui nécessite une période d’initialisation plus longue. En plus d’informer l’utilisateur sur la progression du processus d’activation, cette page empêche que le processus ne soit arrêté si [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046) n’est pas appelé dans les 15secondes suivant son lancement.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> partial class GameHomePage : Page
> {
>     public GameHomePage()
>     {
>         InitializeComponent();
> 
>         // add a handler to be called when the home page has been loaded
>         this.Loaded += ReaderHomePageLoaded;
> 
>         // load the minimal amount of image and sound data from disk necessary to create the home page.        
>     }
>     
>     void ReaderHomePageLoaded(object sender, RoutedEventArgs e)
>     {
>         // set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = this;
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Friend Class GameHomePage
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' add a handler to be called when the home page has been loaded
>         AddHandler Me.Loaded, AddressOf ReaderHomePageLoaded
> 
>         ' load the minimal amount of image and sound data from disk necessary to create the home page.        
>     End Sub
> 
>     Private Sub ReaderHomePageLoaded(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         ' set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = Me
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class
> ```

Pour obtenir un exemple d’utilisation d’un écran de démarrage étendu, voir cet [exemple d’écran de démarrage](http://go.microsoft.com/fwlink/p/?linkid=234889).

### <a name="phase-3"></a>Phase 3

Bien que l’application affiche maintenant l’interface utilisateur, elle n’est pas encore totalement opérationnelle. Dans notre exemple de jeu, l’interface utilisateur est affichée avec des espaces réservés pour les fonctionnalités ayant besoin de récupérer des données sur Internet. Le jeu va donc télécharger les données supplémentaires requises pour rendre l’application pleinement opérationnelle et activer les fonctionnalités les unes après les autres à mesure qu’elle dispose des données requises.

Parfois, une grande partie du contenu nécessaire à l’activation est déjà fournie avec l’application (dans les jeux simples, notamment), ce qui facilite le processus d’activation. En revanche, de nombreux programmes (tels que les lecteurs de News et les visionneuses de photos) doivent extraire certaines données du Web pour fonctionner correctement. Le téléchargement de ces données parfois volumineuses peut prendre beaucoup de temps. La façon dont l’application récupère ces données lors du processus d’activation peut avoir un impact important sur la perception que l’utilisateur a des performances de votre application.

Si vous affichez une page de chargement ou pire un écran de démarrage pendant toute la durée (de plusieurs minutes parfois) où l’application essaie de télécharger l’ensemble des données requises par ses fonctionnalités, lors de la phase 1 ou 2 de l’activation, l’application peut paraître bloquée voire être arrêtée par le système. Nous vous recommandons plutôt de télécharger le minimum de données nécessaire à l’application pour afficher une interface utilisateur interactive avec des espaces réservés lors de la phase 2, puis de charger progressivement les données correspondant aux espaces réservés au cours de la phase 3. Pour plus d’informations sur la manipulation des données, voir [Optimiser les contrôles ListView et GridView](optimize-gridview-and-listview.md).

C’est vous qui définissez le comportement de l’application à chaque phase du démarrage, mais gardez à l’esprit que, si vous fournissez des informations précises sur la progression du processus (par un écran de démarrage, une page de chargement ou une interface utilisateur pendant le chargement des données), l’utilisateur aura une meilleure perception de votre application, et du système en général, en termes de rapidité.

### <a name="minimize-managed-assemblies-in-the-startup-path"></a>Limiter l’utilisation des assemblys managés dans le chemin de démarrage

Le code réutilisable prend souvent la forme de modules (DLL) inclus dans un projet. Le chargement de ces modules nécessite des accès au disque, ce qui peut évidemment être coûteux en ressources. Cela peut avoir un impact sur les démarrages à chaud, même si c’est dans une moindre mesure que lors des démarrages à froid. En C# et Visual Basic, le CLR essaie le plus possible de différer ce coût en chargeant les assemblys à la demande : il ne charge un module que si celui-ci est référencé par une méthode exécutée. Par conséquent, dans le code de démarrage, référencez uniquement les assemblys nécessaires au lancement de votre application afin que le CLR ne charge pas de modules inutiles. Si le chemin de démarrage comporte des chemins de code inutilisés avec des références superflues, vous pouvez déplacer ces chemins de code vers d’autres méthodes pour éviter les chargements non nécessaires.

Pour réduire les chargements de modules, vous pouvez aussi combiner les modules de votre application. En effet, le chargement d’un assembly volumineux est généralement plus rapide que celui de deux assemblys plus petits. Notez que la combinaison des modules n’est pas toujours possible. Par ailleurs, optez pour cette solution seulement si elle n’a pas d’impact significatif sur la productivité du développeur ni sur la réutilisation du code. Utilisez des outils tels que [PerfView](http://go.microsoft.com/fwlink/p/?linkid=251609) ou l’[Analyseur de performance Windows](https://msdn.microsoft.com/library/windows/apps/xaml/ff191077.aspx) pour identifier les modules chargés au démarrage.

### <a name="make-smart-web-requests"></a>Effectuer des requêtes Web intelligentes

Vous pouvez améliorer considérablement le temps de chargement d’une application en empaquetant son contenu localement, y compris le code XAML, les images et tout autre fichier important pour l’application. Les opérations sur disque sont plus rapides que les opérations réseau. Lorsqu’une application nécessite un fichier particulier lors de l’initialisation, vous pouvez réduire le temps de démarrage global en chargeant ce fichier directement du disque au lieu de le récupérer sur un serveur distant.

## <a name="journal-and-cache-pages-efficiently"></a>Journaliser des pages et les mettre en cache efficacement

Le contrôle Frame propose des fonctionnalités de navigation. Il offre des possibilités de navigation vers une page (méthode Navigate), de journalisation de la navigation (propriétés BackStack/ForwardStack, méthode GoForward/GoBack), de mise en cache de pages (méthode Page.NavigationCacheMode) et de prise en charge de la sérialisation (méthode GetNavigationState).

Les performances à connaître associées au contrôle Frame tournent principalement autour de la journalisation et de la mise en cache de pages.

**Journalisation du contrôle Frame**. Lorsque vous naviguez vers une page avec Frame.Navigate(), un PageStackEntry pour la page actuelle est ajouté à la collection Frame.BackStack. L’élément PageStackEntry est relativement petit, mais la taille de la collection BackStack ne comporte aucune limite intégrée. Potentiellement, un utilisateur pourrait naviguer en boucle et alimenter indéfiniment cette collection.

L’élément PageStackEntry comprend également le paramètre transmis à la méthode Frame.Navigate(). Il est recommandé que ce paramètre soit de type sérialisable primitif (par exemple, un entier ou une chaîne), afin de permettre à la méthode Frame.GetNavigationState() de fonctionner. Toutefois, ce paramètre peut potentiellement faire référence à un objet qui justifie une plage de travail plus importante ou d’autres ressources, ce qui rend chaque entrée du BackStack plus onéreux. Par exemple, vous pourriez éventuellement utiliser un StorageFile en tant que paramètre pour que le BackStack maintienne l’ouverture d’un nombre indéfini de fichiers.

Par conséquent, il est recommandé de conserver un petit nombre de paramètres de navigation et de limiter la taille du BackStack. Le BackStack est un vecteur standard (IList en C#, Platform::Vector en C++/CX) qui peut, à ce titre, faire l’objet d’un découpage en supprimant des entrées.

**Mise en cache de pages**. Par défaut, lorsque vous naviguez vers une page avec la méthode Frame.Navigate, une nouvelle instance de la page est instanciée. De même, si vous revenez à la page précédente avec Frame.GoBack, une nouvelle instance de la page précédente est allouée.

Frame, cependant, propose un cache de pages facultatif permettant d’éviter les instanciations. Pour obtenir la mise en cache d’une page, utilisez la propriété Page.NavigationCacheMode. Le fait de définir ce mode sur Required entraîne la mise en cache forcée de la page, alors que le paramètre Enabled autorise sa mise en cache. Par défaut, le cache peut contenir 10 pages, toutefois, cette capacité peut être modifiée à l’aide de la propriété Frame.CacheSize. Toutes les pages Required sont mises en cache, et si leur nombre est inférieur à celui des pages CacheSize Required, les pages Enabled peuvent l’être également.

La mise en cache de pages peut améliorer les performances en évitant les instanciations, et par conséquent, la navigation. L’utilisation excessive de la mise en cache de pages peut avoir un effet néfaste sur les performances et par là même sur la plage de travail.

C’est la raison pour laquelle nous vous recommandons d’utiliser la mise en cache de pages en fonction des besoins de votre application. Supposons par exemple que vous disposez d’une application affichant une liste d’éléments d’un contrôle Frame, et que le fait de cliquer sur un élément implique la navigation dans Frame vers une page de détails correspondant à cet élément. La page de liste devrait probablement être mise en cache. Si la page de détails est identique pour tous les éléments, elle devrait probablement être mise en cache également. Toutefois, si la page de détails est plus hétérogène, il peut être préférable d’abandonner la mise en cache.
