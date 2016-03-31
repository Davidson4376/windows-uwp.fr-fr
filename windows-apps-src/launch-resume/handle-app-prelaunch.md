---
title: Gérer le prélancement d’une application
description: Découvrez comment gérer le prélancement d’une application en remplaçant la méthode OnLaunched.
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
---

# Gérer le prélancement d’une application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x articles, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Découvrez comment gérer le prélancement d’une application en remplaçant la méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## Introduction


Lorsque les ressources système disponibles le permettent, les performances de démarrage des applications du Windows Store sont améliorées en lançant, de manière proactive, les applications les plus fréquemment utilisées en arrière-plan. Une application prélancée est placée à l’état suspendu dans les secondes suivant son lancement. Lorsque l’utilisateur appelle l’application, celle-ci reprend l’exécution à partir de l’état suspendu, ce qui est plus rapide que le lancement de l’application à froid.

Avant Windows 10, les applications ne bénéficiaient pas automatiquement du prélancement. À partir de Windows 10, toutes les applications de plateforme Windows universelle (UWP, Universal Windows Platform) en bénéficient automatiquement.

La plupart des applications doivent fonctionner avec prélancement sans qu’aucune modification soit nécessaire. Toutefois, le comportement au démarrage de certains types d’applications doit éventuellement être modifié pour fonctionner correctement avec le prélancement. Par exemple, une application de messagerie qui change la visibilité en ligne de l’utilisateur lors du démarrage ou un jeu qui suppose que l’utilisateur est présent et affiche des éléments visuels élaborés au démarrage de l’application.

## Prélancement et cycle de vie de l’application


Une fois qu’une application est prélancée, elle passe très vite à l’état suspendu. (Voir [Gérer la suspension d’une application](suspend-an-app.md).)

## Détecter et gérer le prélancement


Les applications reçoivent l’indicateur [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) pendant l’activation. Cet indicateur permet de déterminer s’il convient d’exécuter des opérations qui doivent être effectuées uniquement lorsque l’utilisateur lance explicitement l’application, comme indiqué dans l’extrait suivant de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a 
            // what&#39;s new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn&#39;t restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Conseil** Si vous souhaitez refuser le prélancement, cochez l’indicateur [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740). Si celui-ci est défini, revenez à OnLaunched() avant de créer une image ou d’activer la fenêtre.

 

## Utiliser l’événement VisibilityChanged


Les applications activées par prélancement ne sont pas visibles pour l’utilisateur. Elles le deviennent lorsque l’utilisateur bascule vers celles-ci. Vous souhaitez peut-être retarder certaines opérations jusqu’à ce que la fenêtre principale de votre application soit visible. Par exemple, si votre application affiche une liste des nouveaux éléments d’un flux, vous pouvez mettre à jour cette liste pendant l’événement [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) plutôt que de vous appuyer sur une liste qui a été générée lors du prélancement de l’application et qui peut être obsolète au moment où l’utilisateur active l’application. Le code suivant gère le l’événement **VisibilityChanged** pour **MainPage** :

```cs
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than 
        // when it is prelaunched, such as building a what&#39;s new feed 
    }
}
```

## Indications


-   Les applications ne doivent pas effectuer d’opérations de longue durée pendant le prélancement, puisque l’application s’arrêtera si elle ne peut pas être suspendue rapidement.
-   Les applications ne doivent pas initier la lecture audio à partir de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) lorsque l’application est prélancée, car l’application n’est pas visible et ne sera pas apparente lors de la lecture audio.
-   Les applications ne doivent pas effectuer d’opérations lors du lancement, qui suppose que l’application est visible par l’utilisateur ou qu’elle a été lancée explicitement par l’utilisateur. Dans la mesure où une application peut maintenant être lancée en arrière-plan sans action explicite de l’utilisateur, les développeurs doivent prendre en compte la confidentialité, l’incidence sur les performances et l’expérience utilisateur.
    -   Le moment où une application sociale doit définir l’état de l’utilisateur sur « en ligne » est un exemple de considération relative à la confidentialité. Elle doit attendre que l’utilisateur bascule vers l’application au lieu de modifier l’état de celui-ci lors du prélancement de l’application.
    -   Exemple de considération relative à l’expérience utilisateur : vous possédez une application, telle qu’un jeu, qui affiche une séquence d’introduction lors de son lancement, et aimeriez avoir la possibilité de retarder la séquence d’introduction jusqu’à ce que l’utilisateur bascule vers l’application.
    -   En ce qui concerne l’incidence sur les performances, il pourrait s’agir d’attendre que l’utilisateur bascule vers l’application pour récupérer les informations météorologiques actuelles au lieu de les charger lors du prélancement de l’application, puis de les charger une nouvelle fois lorsque l’application devient visible pour vous assurer que les informations sont à jour.
-   Si votre application efface sa vignette dynamique lors de son lancement, reportez cette opération pour qu’elle ait lieu après l’événement VisibilityChanged.
-   La télémétrie de votre application doit faire la distinction entre les activations de vignette standard et les activations de prélancement, afin que vous puissiez identifier le scénario dans lequel les problèmes se produisent.
-   Si vous possédez Microsoft Visual Studio 2015 Update 1 et Windows 10 version 1511, vous pouvez simuler le prélancement de votre application dans Visual Studio 2015 en choisissant **Déboguer** &gt; **Autres cibles de débogage** &gt; **Déboguer le prélancement de l’application Windows universelle**.

## Rubriques connexes

* [Cycle de vie de l’application](app-lifecycle.md)

 

 



<!--HONumber=Mar16_HO1-->
