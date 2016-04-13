---
title: Gérer l’activation d’une application
description: Découvrez comment gérer l’activation d’une application en remplaçant la méthode OnLaunched.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
---

# Gérer l’activation d’une application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Découvrez comment gérer l’activation d’une application en remplaçant la méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## Remplacer le gestionnaire de lancement

Lorsqu’une application est activée, pour quelque raison que ce soit, le système envoie l’événement [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018). Pour obtenir la liste des types d’activation, voir l’énumération [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

La classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) définit des méthodes que vous pouvez remplacer pour traiter les différents types d’activation. Plusieurs types d’activation ont une méthode spécifique que vous pouvez remplacer. Pour les autres types d’activation, remplacez la méthode [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

Définissez la classe pour votre application.

```xaml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
             x:Class="AppName.App" >
```

Remplacez la méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). Cette méthode est appelée chaque fois que l’utilisateur lance l’application. Le paramètre [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contient l’état précédent de votre application et les arguments d’activation.

**Remarque** Dans les applications du Windows Phone Store, cette méthode est appelée chaque fois que l’utilisateur lance l’application à partir de la vignette d’accueil ou de la liste d’applications, même lorsque l’application est actuellement suspendue en mémoire. Sous Windows, le lancement d’une application suspendue à partir de la vignette d’accueil ou de la liste d’applications n’appelle pas cette méthode.

> [!div class="tabbedCodeSnippets"]
```cs
using System;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace AppName
{
   public partial class App
   {
      async protected override void OnLaunched(LaunchActivatedEventArgs args)
      {
         EnsurePageCreatedAndActivate();
      }
      
      // Creates the MainPage if it isn't already created.  Also activates
      // the window so it takes foreground and input focus.
      private MainPage EnsurePageCreatedAndActivate()
      {
         if (Window.Current.Content == null)
         {
             Window.Current.Content = new MainPage();
         }
         
         Window.Current.Activate();
         return Window.Current.Content as MainPage;
      }
   }
}
```
```vb
Class App
   Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
      Window.Current.Content = New MainPage()
      Window.Current.Activate()
   End Sub
End Class
```
```cpp
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;
void App::OnLaunched(LaunchActivatedEventArgs^ args)
{
   EnsurePageCreatedAndActivate();
}

// Creates the MainPage if it isn't already created.  Also activates
// the window so it takes foreground and input focus.
void App::EnsurePageCreatedAndActivate()
{
    if (_mainPage == nullptr)
    {
        // Save the MainPage for use if we get activated later
        _mainPage = ref new MainPage();
    }
    Window::Current->Content = _mainPage;
    Window::Current->Activate();
}
```

## Restaurer les données d’application en cas de suspension puis d’arrêt de l’application


Lorsque l’utilisateur bascule vers votre application arrêtée, le système envoie l’événement [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018), avec [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) défini sur **Launch** et [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) sur **Terminated** ou **ClosedByUser**. L’application doit charger ses données d’application enregistrées et actualiser son contenu à l’écran.

> [!div class="tabbedCodeSnippets"]
```cs
async protected override void OnLaunched(LaunchActivatedEventArgs args)
{
   if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
       args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }
   
   EnsurePageCreatedAndActivate();
}
```
```vb
Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
   Dim restoreState As Boolean = False

   Select Case args.PreviousExecutionState
      Case ApplicationExecutionState.Terminated
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case ApplicationExecutionState.ClosedByUser
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case Else
         ' TODO: Populate the UI with defaults
   End Select

   Window.Current.Content = New MainPage(restoreState)
   Window.Current.Activate()
End Sub
```
```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
   if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
       args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

Si la valeur de [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) est **NotRunning**, l’application n’a pas réussi à enregistrer ses données d’application et doit redémarrer de zéro.

## Notes

> **Remarque** Dans les applications du Windows Phone Store, l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) est toujours suivi de l’événement [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), même lorsque votre application est suspendue et que l’utilisateur relance votre application à partir d’une vignette principale ou d’une liste d’applications. Les applications peuvent ignorer l’initialisation si un contenu est déjà défini sur la fenêtre active. Vous pouvez vérifier la propriété [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) pour déterminer si l’application a été lancée à partir d’une vignette principale ou secondaire et, en fonction de l’information obtenue, décider si vous devez présenter une expérience de nouvelle exécution ou de reprise d’exécution de l’application.

## Rubriques connexes

* [Gérer la suspension d’une application](suspend-an-app.md)
* [Gérer la reprise d’une application](resume-an-app.md)
* [Recommandations pour la suspension et la reprise d’une application](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Cycle de vie de l’application](app-lifecycle.md)

**Référence**

* [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)

 

 





<!--HONumber=Mar16_HO1-->


