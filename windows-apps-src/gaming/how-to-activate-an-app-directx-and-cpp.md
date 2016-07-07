---
author: mtoepke
title: Comment activer une application (DirectX et C++)
description: "Cette rubrique explique comment définir l’expérience d’activation d’une application DirectX de plateforme Windows universelle (UWP)."
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 14859d03c7af45a17772c76f8c79b3c1bc56272c

---

# Comment activer une application (DirectX et C++)


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette rubrique explique comment définir l’expérience d’activation d’une application DirectX de plateforme Windows universelle (UWP).

## Enregistrer le gestionnaire d’événements d’activation d’application


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

## Activer l’instance CoreWindow pour l’application


Au démarrage de votre application, vous devez obtenir une référence à l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de votre application. 
            **CoreWindow** contient le répartiteur de message d’événement de fenêtre utilisé par votre application pour traiter les événements de fenêtre. Obtenez cette référence dans votre rappel pour l’événement d’activation d’application en appelant [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589). Après avoir obtenu cette référence, activez la fenêtre principale de l’application en appelant [**CoreWindow::Activate**](https://msdn.microsoft.com/library/windows/apps/br208254).

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## Commencer le traitement du message d’événement pour la fenêtre principale de l’application


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

## Articles connexes


* [Comment suspendre une application (DirectX et C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Comment relancer une application (DirectX et C++)](how-to-resume-an-app-directx-and-cpp.md)

 

 







<!--HONumber=Jun16_HO4-->


