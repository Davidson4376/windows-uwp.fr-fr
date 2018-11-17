---
author: TylerMSFT
ms.assetid: 3a3ea86e-fa47-46ee-9e2e-f59644c0d1db
description: Cet article vous explique comment libérer de la mémoire lorsque votre application bascule en arrière-plan.
title: Libérer de la mémoire lorsque votre application passe à l’arrière-plan
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eef1edc4e5c725756cdef788bf555f706621741d
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7165385"
---
# <a name="free-memory-when-your-app-moves-to-the-background"></a>Libérer de la mémoire quand l’application bascule en arrière-plan

Cet article vous explique comment réduire la quantité de mémoire utilisée par votre application lorsqu’elle bascule en arrière-plan, afin qu’elle ne soit pas suspendue, voire arrêtée.

## <a name="new-background-events"></a>Nouveaux événements en arrière-plan

La version1607 de Windows10 inclut deux nouveaux évéments de cycle de vie de l’application, [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) et [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground). Ces événements indiquent à votre application à quel moment elle passe en arrière-plan et à quel moment elle revient au premier plan.

Lorsque votre application passe en arrière-plan, les contraintes de mémoire appliquées par le système peuvent changer. Utilisez ces événements pour vérifier votre consommation de mémoire actuelle et libérer des ressources afin de rester sous la limite autorisée et éviter que votre application ne soit suspendue, voire arrêtée, alors qu’elle se trouve en arrière-plan.

### <a name="events-for-controlling-your-apps-memory-usage"></a>Événements permettant de contrôler l’utilisation de la mémoire de votre application

L’événement [MemoryManager.AppMemoryUsageLimitChanging](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelimitchanging.aspx) se déclenche juste avant que la limite de mémoire totale pouvant être utilisée par l’application soit modifiée. Par exemple, lorsque l’application passe en arrière-plan et sur la Xbox, la limite de mémoire passe de 1024Mo à 128Mo.  
Il s’agit du principal événement à gérer pour éviter que la plateforme suspende ou arrête l’application.

L’événement [MemoryManager.AppMemoryUsageIncreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusageincreased.aspx) se déclenche lorsque le niveau de consommation de mémoire de l’application augmente dans l’énumération [AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.appmemoryusagelevel.aspx). Par exemple, si la consommation passe de **Low** à **Medium**. Facultative, la gestion de cet événement est toutefois recommandée, car l’application doit toujours rester sous la limite autorisée.

L’événement [MemoryManager.AppMemoryUsageDecreased](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagedecreased.aspx) se déclenche lorsque le niveau de consommation de mémoire de l’application baisse dans l’énumération **AppMemoryUsageLevel**. Par exemple, si la consommation passe de **High** à **Low**. La gestion de cet événement est facultative mais indique que l’application est capable d’allouer davantage de mémoire si nécessaire.

## <a name="handle-the-transition-between-foreground-and-background"></a>Gérer la transition entre le premier plan et l’arrière-plan

Lorsque votre application passe du premier plan à l’arrière-plan, l’événement [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) est déclenché. Quand votre application revient au premier plan, l’événement [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) est déclenché. Vous pouvez enregistrer des gestionnaires pour ces événements au moment de la création de votre application. Dans le modèle de projet par défaut, l’enregistrement s’effectue au niveau du constructeur de la classe **App**, dans App.xaml.cs.

L’exécution en arrière-plan implique une réduction des ressources de mémoire que votre application est autorisée à conserver. Vous devez donc également enregistrer les événements [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) et [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging), que vous pouvez utiliser pour vérifier l’utilisation actuelle de la mémoire par l’application et la limite actuelle autorisée. Les gestionnaires de ces événements sont affichés dans les exemples suivants. Pour plus d’informations sur le cycle de vie des applicationsUWP, consultez la rubrique [Cycle de vie de l’application](..//launch-resume/app-lifecycle.md).

[!code-cs[RegisterEvents](./code/ReduceMemory/cs/App.xaml.cs#SnippetRegisterEvents)]

Lorsque l’événement [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) est déclenché, définissez la variable de détection afin d’indiquer que l’exécution se déroule actuellement dans l’arrière-plan. Ce procédé est utile lorsque vous écrivez le code permettant de réduire l’utilisation de la mémoire.

[!code-cs[EnteredBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetEnteredBackground)]

Lorsque votre application passe en arrière-plan, le système réduit la limite de mémoire pour l’application afin de garantir que l’application qui s’exécute actuellement au premier plan dispose des ressources suffisantes pour fournir une expérience utilisateur réactive.

Le gestionnaire d’événements [**AppMemoryUsageLimitChanging**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageLimitChanging) indique à votre application que sa limite de mémoire a été réduite et fournit la nouvelle limite dans les arguments d’événement qui lui ont été transmis. Comparez la propriété [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsage), qui fait état de l’utilisation actuelle de votre application, à la propriété [**NewLimit**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLimitChangingEventArgs.NewLimit) des arguments d’événement qui définit la nouvelle limite. Si votre utilisation de la mémoire dépasse la limite, vous devez la réduire.

Dans cet exemple, cette opération s’effectue dans la méthode d’assistance **ReduceMemoryUsage**, définie plus loin dans cet article.

[!code-cs[MemoryUsageLimitChanging](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageLimitChanging)]

> [!NOTE]
> Certaines configurations d’appareil permettent à une application de continuer à s’exécuter au-delà de la nouvelle limite de mémoire jusqu’à ce que le système manque de ressources, mais ce n’est pas toujours le cas. Sur Xbox en particulier, les applications sont interrompues ou arrêtées si elles ne réduisent pas leur utilisation de mémoire en deçà de la nouvelle limite dans un délai de 2secondes. Cela signifie que vous pouvez fournir la meilleure expérience sur un large éventail d’appareils en utilisant cet événement pour réduire l’utilisation des ressources en deçà de la limite dans les 2secondes suivant le déclenchement de l’événement.

Même si l’utilisation de la mémoire de votre application est en dessous de la limite autorisée pour les applications en arrière-plan au moment où l’application passe en arrière-plan, il est possible que l’application augmente sa consommation de mémoire au fil du temps pour s’approcher de la limite autorisée. Le gestionnaire de l’événement [**AppMemoryUsageIncreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageIncreased) vous permet de savoir quand votre utilisation augmente, et de libérer de la mémoire le cas échéant.

Vérifiez si [**AppMemoryUsageLevel**](https://msdn.microsoft.com/library/windows/apps/Windows.System.AppMemoryUsageLevel) présente l’état **High** ou **OverLimit**. Le cas échéant, réduisez votre utilisation de la mémoire. Dans cet exemple, le processus est géré via la méthode d’assistance, **ReduceMemoryUsage**. Vous pouvez également vous abonner à l’événement [**AppMemoryUsageDecreased**](https://msdn.microsoft.com/library/windows/apps/Windows.System.MemoryManager.AppMemoryUsageDecreased) pour vérifier si votre application se situe en deçà de la limite autorisée et allouer des ressources supplémentaires en cas de besoin.

[!code-cs[MemoryUsageIncreased](./code/ReduceMemory/cs/App.xaml.cs#SnippetMemoryUsageIncreased)]

**ReduceMemoryUsage** est une méthode d’assistance que vous pouvez implémenter pour libérer de la mémoire lorsque votre application qui s’exécute en arrière-plan dépasse la limite d’utilisation autorisée. La manière dont vous libérez de la mémoire dépend des spécificités de votre application, mais nous vous recommandons entre autres de vous libérer de votre interface utilisateur et d’autres ressources associées à l’affichage de votre application. Pour ce faire, assurez-vous que l’application s’exécute bien en arrière-plan, définissez la propriété [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) de la fenêtre de votre application sur `null`, désactivez vos gestionnaires d’événements de l’interface utilisateur et supprimez toute autre référence associée à la page. Sans cela, vous ne pourrez pas libérer les ressources de la page. Appelez ensuite **GC.Collect** pour récupérer immédiatement la mémoire libérée. En général, vous ne forcez pas un nettoyage de la mémoire, car le système s’en charge pour vous. Dans ce cas précis, nous diminuons la quantité de mémoire chargée pour cette application lorsqu'elle passe en arrière-plan afin de réduire la probabilité que le système détermine qu’il doit fermer l’application pour récupérer de la mémoire.

[!code-cs[UnloadViewContent](./code/ReduceMemory/cs/App.xaml.cs#SnippetUnloadViewContent)]

Lorsque le contenu de la fenêtre est collecté, chaque élément Frame démarre son processus de déconnexion. Si l’arborescence des objets visuels sous le contenu de la fenêtre comporte des éléments Pages, ces derniers commenceront à déclencher leur événement Unloaded. Les Pages ne peuvent pas être complètement effacées de la mémoire, sauf si l’ensemble des références associées sont supprimées. Dans le rappel Unloaded, procédez comme suit afin de garantir la libération rapide de la mémoire:
* Effacez les grandes structures de données de votre Page, et définissez-les sur `null`.
* Annulez l’enregistrement de l’ensemble des gestionnaires d’événements comprenant des méthodes de rappel dans la Page. Veillez à inscrire ces rappels durant le gestionnaire d’événement Loaded associé à la Page. L’événement Loaded est déclenché quand l’interface utilisateur a été reconstituée et que la Page a été ajoutée à l’arborescence des objets visuels.
* Appelez `GC.Collect` à la fin du rappel Unloaded afin de procéder rapidement à un nettoyage de la mémoire des grandes structures de données que vous venez de définir sur `null`. Une fois de plus, vous ne forcez généralement pas un nettoyage de la mémoire, car le système s’en charge pour vous. Dans ce cas précis, nous diminuons la quantité de mémoire chargée pour cette application lorsqu'elle passe en arrière-plan afin de réduire la probabilité que le système détermine qu’il doit fermer l’application pour récupérer de la mémoire.

[!code-cs[MainPageUnloaded](./code/ReduceMemory/cs/App.xaml.cs#SnippetMainPageUnloaded)]

Dans le gestionnaire d’événement [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground), définissez la variable de détection (`isInBackgroundMode`) pour indiquer que votre application ne s’exécute plus en arrière-plan. Ensuite, vérifiez si l’élément [**Content**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Window.Content) de la fenêtre actuelle est défini sur `null`, ce qui sera le cas si vous avez supprimé les vues de l’application afin de nettoyer la mémoire pendant l’exécution en arrière-plan. Si le contenu de la fenêtre est défini sur `null`, régénérez l’affichage de votre application. Dans cet exemple, le contenu de la fenêtre est créé dans la méthode d’assistance **CreateRootFrame**.

[!code-cs[LeavingBackground](./code/ReduceMemory/cs/App.xaml.cs#SnippetLeavingBackground)]

La méthode d’assistance **CreateRootFrame** régénère le contenu d’affichage de votre application. Le code de cette méthode est presque identique au code du gestionnaire [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) fourni dans le modèle de projet par défaut. La seule différence est que le gestionnaire **Launching** détermine l’état d’exécution précédent à partir de la propriété [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs.PreviousExecutionState) de la classe [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs), alors que la méthode **CreateRootFrame** récupère simplement l’état précédent d’exécution transmis en tant qu’argument. Pour réduire le code dédupliqué, vous pouvez refactoriser le code du gestionnaire d’événement **Launching** par défaut afin d’appeler **CreateRootFrame**.

[!code-cs[CreateRootFrame](./code/ReduceMemory/cs/App.xaml.cs#SnippetCreateRootFrame)]

## <a name="guidelines"></a>Recommandations

### <a name="moving-from-the-foreground-to-the-background"></a>Passage du premier plan à l’arrière-plan

Lorsqu’une application passe du premier plan à l’arrière-plan, le système se charge de libérer les ressources dont l’application n’a pas besoin en arrière-plan. Par exemple, les infrastructures d’interface utilisateur vident les textures mises en cache et le sous-système vidéo libère la mémoire allouée pour le compte de l’application. Toutefois, une application devra toujours surveiller attentivement son utilisation de la mémoire pour ne pas être suspendue ou arrêtée par le système.

Lorsqu’une application passe du premier plan à l’arrière-plan, elle reçoit d’abord un événement **EnteredBackground**, puis un événement **AppMemoryUsageLimitChanging**.

- **Utilisez** l’événement **EnteredBackground** pour libérer les ressources d’interface utilisateur dont votre application n’a pas besoin pour s’exécuter en arrière-plan. Vous pouvez par exemple supprimer l’image de pochette d’une chanson.
- **Utilisez** l’événement **AppMemoryUsageLimitChanging** pour vous assurer que votre application utilise moins de mémoire que la nouvelle limite autorisée en arrière-plan. Si ce n’est pas le cas, assurez-vous de libérer des ressources. Si vous ne le faites pas, votre application peut être suspendue ou arrêtée selon la stratégie définie pour l’appareil.
- **Appelez** manuellement le récupérateur de mémoire si votre application dépasse la nouvelle limite de mémoire lorsque l’événement **AppMemoryUsageLimitChanging** est déclenché.
- **Utilisez** l’événement **AppMemoryUsageIncreased** pour continuer à surveiller l’utilisation de la mémoire de votre application lors de son exécution en arrière-plan si vous pensez qu’elle peut évoluer. Si le niveau **AppMemoryUsageLevel** est défini sur **High** ou **OverLimit**, veillez à libérer des ressources.
- Pour optimiser les performances, **libérez** les ressources d’interface utilisateur dans le gestionnaire d’événement **AppMemoryUsageLimitChanging** plutôt que dans le gestionnaire **EnteredBackground**. Utilisez une valeur booléenne définie dans les gestionnaires d’événements **EnteredBackground/LeavingBackground** pour savoir si l’application s’exécute en arrière-plan ou au premier plan. Puis, dans le gestionnaire d’événement **AppMemoryUsageLimitChanging**, si la valeur **AppMemoryUsage** est au-dessus de la limite et que l’application s’exécute en arrière-plan (tel que défini par la valeur booléenne), vous pouvez libérer des ressources d’interface utilisateur.
- **N’effectuez pas** d’opérations longues dans l’événement **EnteredBackground** afin de fournir à l’utilisateur une expérience de transition fluide entre les applications.

### <a name="moving-from-the-background-to-the-foreground"></a>Passage de l’arrière-plan au premier plan

Lorsqu’une application passe de l’arrière-plan au premier plan, elle reçoit d’abord un événement **AppMemoryUsageLimitChanging**, puis un événement **LeavingBackground**.

- **Utilisez** l’événement **LeavingBackground** pour recréer les ressources d’interface utilisateur que votre application a supprimées lors de son passage en arrière-plan.

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de lecture de contenu multimédia en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=800141) - explique comment libérer de la mémoire lorsque votre application passe en arrière-plan.
* [Outils de diagnostic](https://blogs.msdn.microsoft.com/visualstudioalm/2015/01/16/diagnostic-tools-debugger-window-in-visual-studio-2015/) - utilisez les outils de diagnostic pour analyser les événements de nettoyage de la mémoire et vérifier que votre application libère de la mémoire de manière optimale.
