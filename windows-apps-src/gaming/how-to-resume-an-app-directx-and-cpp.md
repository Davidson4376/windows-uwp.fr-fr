---
author: mtoepke
title: Comment relancer une application (DirectX et C++)
description: Cet article indique comment restaurer des données d’application importantes lorsque le système reprend l’exécution de votre application DirectX de plateforme Windows universelle (UWP).
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, reprise, directx
ms.localizationpriority: medium
ms.openlocfilehash: 1149bebfd837e3d4051b5e0fca10aac248d909c5
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5594262"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>Comment relancer une application (DirectX et C++)



Cette rubrique montre comment restaurer des données d’application importantes lorsque le système reprend l’exécution de votre application DirectX de plateforme Windows universelle.

## <a name="register-the-resuming-event-handler"></a>Enregistrer le gestionnaire d’événements de reprise


Enregistrez-vous pour traiter l’événement [**CoreApplication::Resuming**](https://msdn.microsoft.com/library/windows/apps/br205859), qui indique que l’utilisateur revient vers votre application après s’en être éloigné.

Ajoutez le code suivant à votre implémentation de la méthode [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) de votre fournisseur d’affichage :

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## <a name="refresh-displayed-content-after-suspension"></a>Actualiser le contenu affiché après la suspension


Lorsque votre application gère l’événement de reprise, elle a la possibilité d’actualiser son contenu à l’écran. Restaurez les applications que vous avez enregistrées avec votre gestionnaire pour [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860), puis redémarrez le traitement. Développeurs de jeux : si vous avez suspendu votre moteur audio, il est temps de le redémarrer.

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

Ce rappel a lieu en tant que message d’événement traité par l’objet [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) pour l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de l’application. Ce rappel n’est pas effectué si vous n’appelez pas [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) à partir de la boucle principale de votre application (mise en œuvre dans la méthode [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) de votre fournisseur d’affichage).

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

## <a name="remarks"></a>Remarques


Le système suspend votre application chaque fois que l’utilisateur bascule vers une autre application ou vers le Bureau. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan. Cependant, il se peut que l’application ait été suspendue pendant une durée significative. Elle doit dans ce cas actualiser le contenu affiché susceptible d’avoir changé pendant l’inactivité et redémarrer les threads de traitement audio ou de rendu. Si vous avez enregistré des données d’état de jeu durant un événement de suspension précédent, restaurez-les maintenant.

## <a name="related-topics"></a>Rubriques connexes

* [Comment suspendre une application (DirectX et C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Comment activer une application (DirectX et C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 




