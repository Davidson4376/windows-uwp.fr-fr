---
author: mtoepke
title: Comment activer une application (DirectX et C++)
description: Cet article explique comment définir l’expérience d’activation d’une application DirectX de plateforme Windows universelle (UWP).
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, directx, activation
ms.localizationpriority: medium
ms.openlocfilehash: b7f700ab97566ad9ec03d0595c55721dd9a9be98
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6459199"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>Activation d’une application (DirectX et C++)



Cette rubrique explique comment définir l’expérience d’activation d’une application DirectX de plateforme Windows universelle (UWP).

## <a name="register-the-app-activation-event-handler"></a>Enregistrer le gestionnaire d’événements d’activation d’application


Tout d’abord, inscrivez le gestionnaire de l’événement [**CoreApplicationView::Activated**](https://msdn.microsoft.com/library/windows/apps/br225018), lequel est déclenché au démarrage et à l’initialisation de votre application par le système d’exploitation.

Ajoutez le code suivant à votre implémentation de la méthode [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) de votre fournisseur d’affichage (nommé **MyViewProvider** dans l’exemple) :

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## <a name="activate-the-corewindow-instance-for-the-app"></a>Activer l’instance CoreWindow pour l’application


Au démarrage de votre application, vous devez obtenir une référence à l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de votre application. **CoreWindow** contient le répartiteur de message d’événement de fenêtre utilisé par votre application pour traiter les événements de fenêtre. Obtenez cette référence dans votre rappel pour l’événement d’activation d’application en appelant [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589). Après avoir obtenu cette référence, activez la fenêtre principale de l’application en appelant [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>Commencer le traitement du message d’événement pour la fenêtre principale de l’application


Vos rappels ont lieu en tant que messages d’événements traités par l’objet [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) pour l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de l’application. Ce rappel n’est pas effectué si vous n’appelez pas [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) à partir de la boucle principale de votre application (mise en œuvre dans la méthode [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) de votre fournisseur d’affichage).

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="related-topics"></a>Articles connexes


* [Comment suspendre une application (DirectX et C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Comment relancer une application (DirectX et C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 




