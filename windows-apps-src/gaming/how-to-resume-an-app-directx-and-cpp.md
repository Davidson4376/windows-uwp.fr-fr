---
title: Comment relancer une application (DirectX et C++)
description: Cette rubrique montre comment restaurer des données d’application importantes lorsque le système reprend l’exécution de votre application DirectX de plateforme Windows universelle.
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
---

# Comment relancer une application (DirectX et C++)


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cette rubrique montre comment restaurer des données d’application importantes lorsque le système reprend l’exécution de votre application DirectX de plateforme Windows universelle.

## Enregistrer le gestionnaire d’événements de reprise


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

## Actualiser le contenu affiché après la suspension


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

## Remarques


Le système suspend votre application chaque fois que l’utilisateur bascule vers une autre application ou vers le Bureau. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan. Cependant, il se peut que l’application ait été suspendue pendant une durée significative. Elle doit dans ce cas actualiser le contenu affiché susceptible d’avoir changé pendant l’inactivité et redémarrer les threads de traitement audio ou de rendu. Si vous avez enregistré des données d’état de jeu durant un événement de suspension précédent, restaurez-les maintenant.

## Rubriques connexes

* [Comment suspendre une application (DirectX et C++)](how-to-suspend-an-app-directx-and-cpp.md)
* [Comment activer une application (DirectX et C++)](how-to-activate-an-app-directx-and-cpp.md)

 

 






<!--HONumber=Mar16_HO1-->


