---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: Créer un élément de travail périodique
description: Découvrez comment créer un élément de travail qui se reproduit régulièrement.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, élément de travail périodique, threads, minuteurs
ms.localizationpriority: medium
ms.openlocfilehash: 92142bcf084b6504e4c694ca33d2dc8532f1acca
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8344151"
---
# <a name="create-a-periodic-work-item"></a>Créer un élément de travail périodique


** API importantes **

-   [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915)
-   [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587)

Découvrez comment créer un élément de travail qui se reproduit régulièrement.

## <a name="create-the-periodic-work-item"></a>Créer l’élément de travail périodique

Utilisez la méthode [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) pour créer un élément de travail périodique. Fournissez une expression lambda qui effectue la tâche, puis utilisez le paramètre *period* pour spécifier l’intervalle entre les envois. La période est spécifiée à l’aide d’une structure [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996). L’élément de travail est renvoyé chaque fois que la période est écoulée; vérifiez donc que la période est suffisamment longue pour que la tâche s’accomplisse.

[**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.createtimer.aspx) retourne un objet [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587). Stockez cet objet au cas où le minuteur devrait être annulé.

> **Remarque**Évitez de spécifier la valeur zéro (ou toute valeur inférieure à une milliseconde) pour l’intervalle. Sinon, cela amène le minuteur périodique à se comporter comme un minuteur à déclenchement unique.

> **Remarque**vous pouvez utiliser [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) pour accéder à l’interface utilisateur et afficher la progression à partir de l’élément de travail.

L’exemple suivant crée un élément de travail qui s’exécute une fois toutes les 60 secondes:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>                         
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>Gérer l’annulation de l’élément périodique (facultatif)

Si nécessaire, vous pouvez gérer l’annulation du minuteur périodique avec un objet [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926). Utilisez la surcharge [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) pour fournir une expression lambda supplémentaire qui gère l’annulation de l’élément de travail périodique.

L’exemple suivant crée un élément de travail périodique qui se reproduit toutes les 60secondes et fournit également un gestionnaire d’annulation:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>                 
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Annuler le minuteur

Si nécessaire, appelez la méthode [**Cancel**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.cancel.aspx) pour mettre fin à la répétition de l’élément de travail périodique. Si l’élément de travail est en cours d’exécution lorsque le minuteur périodique est annulé, il est autorisé à se terminer. L’objet [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926) (s’il est fourni) est appelé lorsque toutes les instances de l’élément de travail périodique sont terminées.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>Notes

Pour obtenir des informations sur les minuteurs à déclenchement unique, voir [Utiliser un minuteur pour envoyer un élément de travail](use-a-timer-to-submit-a-work-item.md).

## <a name="related-topics"></a>Rubriques connexes

* [Envoyer un élément de travail au pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Meilleures pratiques pour l’utilisation du pool de threads](best-practices-for-using-the-thread-pool.md)
* [Utiliser un minuteur pour envoyer un élément de travail](use-a-timer-to-submit-a-work-item.md)
 
