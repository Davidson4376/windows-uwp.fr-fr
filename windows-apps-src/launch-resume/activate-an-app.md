---
title: Gérer l’activation d’une application
description: Découvrez comment gérer l’activation d’une application en remplaçant la méthode OnLaunched.
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: a75136f26aa6cfa330e4118e6709b0b4d4be4054
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929350"
---
# <a name="handle-app-activation"></a>Gérer l’activation d’une application

Découvrez comment gérer l’activation d’application en remplaçant la méthode [**Application.OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched) .

## <a name="override-the-launch-handler"></a>Remplacer le gestionnaire de lancement

Lorsqu’une application est activée, pour une raison quelconque, le système envoie l’événement [**CoreApplicationView.Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) . Pour obtenir la liste des types d’activation, voir l’énumération [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

La classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) définit des méthodes que vous pouvez remplacer pour traiter les différents types d’activation. Plusieurs types d’activation ont une méthode spécifique que vous pouvez remplacer. Pour les autres types d’activation, remplacez la méthode [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

Définissez la classe pour votre application.

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

Remplacez la méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). Cette méthode est appelée chaque fois que l’utilisateur lance l’application. Le paramètre [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contient l’état précédent de votre application et les arguments d’activation.

> [!NOTE]
> Sur Windows, le lancement d’une application suspendue à partir de l’écran de démarrage par vignette ou application de la liste n’a pas d’appeler cette méthode.

```csharp
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

```cppwinrt
...
#include "MainPage.h"
#include "winrt/Windows.ApplicationModel.Activation.h"
#include "winrt/Windows.UI.Xaml.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
...
using namespace winrt;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    /// <summary>
    /// Invoked when the application is launched normally by the end user.  Other entry points
    /// will be used such as when the application is launched to open a specific file.
    /// </summary>
    /// <param name="e">Details about the launch request and process.</param>
    void OnLaunched(LaunchActivatedEventArgs const& e)
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
        }

        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active
        if (rootFrame == nullptr)
        {
            // Create a Frame to act as the navigation context and associate it with
            // a SuspensionManager key
            rootFrame = Frame();

            rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

            if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
            {
                // Restore the saved session state only when appropriate, scheduling the
                // final launch steps after the restore is complete
            }

            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Place the frame in the current Window
                Window::Current().Content(rootFrame);
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
        else
        {
            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
    }
};
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

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>Restaurer les données d’application en cas de suspension puis d’arrêt de l’application

Lorsque l’utilisateur bascule vers votre application arrêtée, le système envoie l’événement [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018), avec [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) défini sur **Launch** et [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) sur **Terminated** ou **ClosedByUser**. L’application doit charger ses données d’application enregistrées et actualiser son contenu à l’écran.

```csharp
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

```cppwinrt
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs const& e)
{
    if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated ||
        e.PreviousExecutionState() == ApplicationExecutionState::ClosedByUser)
    {
        // Populate the UI with the previously saved application data.
    }
    else
    {
        // Populate the UI with defaults.
    }
    ...
}
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

## <a name="remarks"></a>Notes

> [!NOTE]
> Les applications peuvent ignorer l’initialisation si un contenu est déjà défini sur la fenêtre active. Vous pouvez vérifier la propriété [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) pour déterminer si l’application a été lancée à partir d’une vignette principale ou secondaire et, en fonction de ces informations, décidez si vous devez présenter une nouvelle ou reprendre l’expérience de l’application.

## <a name="important-apis"></a>API importantes
* [Windows.ApplicationModel.Activation](https://msdn.microsoft.com/library/windows/apps/br224766)
* [Windows.UI.Xaml.Application](https://msdn.microsoft.com/library/windows/apps/br242324)

## <a name="related-topics"></a>Rubriques connexes
* [Gérer la suspension d’une application](suspend-an-app.md)
* [Gérer la reprise d’une application](resume-an-app.md)
* [Recommandations pour la suspension et la reprise d’une application](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Cycle de vie de l’application](app-lifecycle.md)
