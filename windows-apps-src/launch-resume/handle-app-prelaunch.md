---
author: TylerMSFT
title: "Gérer le prélancement d’une application"
description: "Découvrez comment gérer le prélancement d’une application en remplaçant la méthode OnLaunched et en appelant CoreApplication.EnablePrelaunch(true)."
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: b8200f3dd345d2da63a9fa127db53201afefc92d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="handle-app-prelaunch"></a>Gérer le prélancement d’une application

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Découvrez comment gérer le prélancement d’une application en remplaçant la méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## <a name="introduction"></a>Introduction

Lorsque les ressources système disponibles le permettent, les performances de démarrage des applications du Windows Store sur les appareils de bureau sont améliorées en lançant, de manière proactive, les applications les plus fréquemment utilisées en arrière-plan. Une application prélancée est placée à l’état suspendu dans les secondes suivant son lancement. Ainsi, lorsque l’utilisateur appelle l’application, celle-ci reprend l’exécution à partir de l’état suspendu, ce qui est plus rapide que le lancement de l’application à froid. L’utilisateur peut donc lancer son application très rapidement.

Avant Windows10, les applications ne bénéficiaient pas automatiquement du prélancement. Dans Windows10, version1511, toutes les applications de plateforme Windows universelle (UWP) étaient candidates au prélancement. Dans Windows10, version1607, vous devez autoriser le comportement de prélancement en appelant [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx). Placez cet appel dans `OnLaunched()`, à proximité de l’emplacement où la vérification `if (e.PrelaunchActivated == false)` est effectuée.

Le prélancement d’une application dépend des ressources système. Si le système manque de ressources, les applications ne sont pas prélancées.

Le comportement au démarrage de certains types d’applications doit éventuellement être modifié pour prendre en charge le prélancement. Par exemple, une application qui diffuse de la musique au démarrage, un jeu qui part du principe que l’utilisateur est présent et qui affiche des éléments visuels sophistiqués au démarrage, une application de messagerie qui modifie le statut de visibilité en ligne de l’utilisateur au moment du démarrage, peuvent savoir si l’application a été prélancée et modifier ainsi leur comportement au démarrage comme décrit dans les sections ci-dessous.

Les modèles par défaut pour les projets XAML (C#, VB, C++) et WinJS prennent en charge le prélancement dans Visual Studio 2015 Update3.

## <a name="prelaunch-and-the-app-lifecycle"></a>Prélancement et cycle de vie de l’application

Une fois qu’une application est prélancée, elle passe à l’état suspendu. (Voir [Gérer la suspension d’une application](suspend-an-app.md).)

## <a name="detect-and-handle-prelaunch"></a>Détecter et gérer le prélancement

Les applications reçoivent l’indicateur [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) pendant l’activation. Cet indicateur permet d’exécuter du code qui doit être exécuté uniquement lorsque l’utilisateur lance explicitement l’application, comme indiqué dans l’extrait suivant de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

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
            // what's new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Conseil**  Si vous utilisez une version de Windows10 antérieure à la version1607 et que vous souhaitez refuser le prélancement, cochez l’indicateur [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740). Si celui-ci est défini, revenez à OnLaunched() avant de créer une image ou d’activer la fenêtre.

## <a name="use-the-visibilitychanged-event"></a>Utiliser l’événement VisibilityChanged

Les applications activées par prélancement ne sont pas visibles pour l’utilisateur. Elles le deviennent lorsque l’utilisateur bascule vers celles-ci. Vous souhaitez peut-être retarder certaines opérations jusqu’à ce que la fenêtre principale de votre application soit visible. Par exemple, si votre application affiche une liste des nouveaux éléments d’un flux, vous pouvez mettre à jour cette liste pendant l’événement [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) plutôt que d’utiliser une liste qui a été générée lors du prélancement de l’application et qui peut être obsolète au moment où l’utilisateur active l’application. Le code suivant gère l’événement **VisibilityChanged** pour **MainPage**:

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
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## <a name="directx-games-guidance"></a>Recommandations en matière de jeux DirectX

Il n’est généralement pas conseillé d’activer le prélancement pour les jeux DirectX car de nombreux jeux DirectX s’initialisent avant que le prélancement puisse être détecté. À partir de Windows version1607, édition anniversaire, le prélancement de votre jeu sera désactivé par défaut.  Si vous souhaitez activer le prélancement pour votre jeu, appelez l’élément [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx).

Si votre jeu utilise une version antérieure de Windows10, vous pouvez gérer la condition de prélancement pour quitter l’application:

```cs
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## <a name="winjs-app-guidance"></a>Recommandations en matière d’applications WinJS

Si votre application WinJS utilise une version antérieure de Windows10, vous pouvez gérer la condition de prélancement dans votre gestionnaire [onactivated](https://msdn.microsoft.com/library/windows/apps/br212679.aspx):

```js
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## <a name="general-guidance"></a>Recommandations d’ordre général

-   Les applications ne doivent pas effectuer d’opérations de longue durée pendant le prélancement, puisque l’application s’arrêtera si elle ne peut pas être suspendue rapidement.
-   Les applications ne doivent pas initier la lecture audio à partir de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) lorsque l’application est prélancée, car l’application n’est pas visible et ne sera pas apparente lors de la lecture audio.
-   Les applications ne doivent pas effectuer d’opérations lors du lancement, qui suppose que l’application est visible par l’utilisateur ou qu’elle a été lancée explicitement par l’utilisateur. Dans la mesure où une application peut maintenant être lancée en arrière-plan sans action explicite de l’utilisateur, les développeurs doivent prendre en compte la confidentialité, l’incidence sur les performances et l’expérience utilisateur.
    -   Le moment où une application sociale doit définir l’état de l’utilisateur sur « en ligne » est un exemple de considération relative à la confidentialité. Elle doit attendre que l’utilisateur bascule vers l’application au lieu de modifier l’état de celui-ci lors du prélancement de l’application.
    -   Exemple de considération relative à l’expérience utilisateur : vous possédez une application, telle qu’un jeu, qui affiche une séquence d’introduction lors de son lancement, et aimeriez avoir la possibilité de retarder la séquence d’introduction jusqu’à ce que l’utilisateur bascule vers l’application.
    -   En ce qui concerne l’incidence sur les performances, vous devrez peut-être attendre que l’utilisateur bascule vers l’application pour récupérer les informations météorologiques actuelles au lieu de les charger lors du prélancement de l’application, puis les charger une nouvelle fois lorsque l’application devient visible pour vous assurer que les informations sont à jour.
-   Si votre application efface sa vignette dynamique lors de son lancement, reportez cette opération pour qu’elle ait lieu après l’événement VisibilityChanged.
-   La télémétrie de votre application doit faire la distinction entre les activations de vignette standard et les activations de prélancement, afin que vous puissiez identifier plus facilement le scénario correspondant en cas de problème.
-   Si vous possédez Microsoft Visual Studio2015 Update1 et Windows10 version1511, vous pouvez simuler le prélancement de votre application dans Visual Studio2015 en choisissant **Déboguer** &gt; **Autres cibles de débogage** &gt; **Déboguer le prélancement de l’application Windows universelle**.

## <a name="related-topics"></a>Rubriques connexes

* [Cycle de vie de l’application](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)
