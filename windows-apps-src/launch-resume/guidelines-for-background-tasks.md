---
author: TylerMSFT
title: "Recommandations pour les tâches en arrière-plan"
description: "Assurez-vous que votre application répond aux exigences relatives à l’exécution de tâches en arrière-plan."
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 3fb6884a968afb87e8de303bbd17feba4993c597

---

# Recommandations pour les tâches en arrière-plan


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Assurez-vous que votre application répond aux exigences relatives à l’exécution de tâches en arrière-plan.

## Recommandations en matière de tâches en arrière-plan


Tenez compte des recommandations suivantes au moment de développer une tâche en arrière-plan et avant de publier votre application.

**Quotas de processeur:**Les tâches en arrière-plan sont limitées par la quantité de temps d’utilisation de l’horloge en fonction de leur type de déclencheur. La plupart des déclencheurs sont limités à 30secondes d’utilisation de l’horloge, mais certains ont une capacité d’exécution pouvant atteindre 10minutes afin d’effectuer des tâches intensives. Les tâches en arrière-plan doivent être légères pour préserver l’autonomie de la batterie et assurer une meilleure expérience utilisateur pour les applications de premier plan. Pour plus d’informations sur les contraintes de ressource appliquées à la plupart des tâches en arrière-plan, voir [Prendre en charge votre application avec des tâches en arrière-plan](support-your-app-with-background-tasks.md).

**Gérer les tâches en arrière-plan:**Votre application doit obtenir la liste des tâches en arrière-plan inscrites, s’inscrire aux gestionnaires de progression et d’achèvement et gérer ces événements de manière appropriée. Vos classes de tâche en arrière-plan doivent signaler la progression, l’annulation et l’exécution à terme des tâches. Pour plus d’informations, voir [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md) et [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md).

**Utilisez [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499):**si votre classe de tâche en arrière-plan exécute du code asynchrone, veillez à utiliser des reports. À défaut, votre tâche en arrière-plan risque de se terminer prématurément lorsque la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) aboutira. Pour plus d’informations, voir [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md).

L’autre solution consiste à demander un report et à utiliser **async/await** pour exécuter des appels de méthode asynchrone. Fermez le report après les appels de méthode **await**.

**Mettre à jour le manifeste de l’application:**déclarez chaque tâche en arrière-plan dans le manifeste de l’application, de même que le type de déclencheur avec lequel elle est utilisée. À défaut, votre application ne pourra pas inscrire la tâche en arrière-plan au moment de l’exécution. Pour plus d’informations, voir [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md).

**Préparez les mises à jour de l’application:**Si votre application doit être mise à jour, créez et inscrivez une tâche en arrière-plan **ServicingComplete** (voir [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)) pour faciliter l’exécution des mises à jour de l’application pouvant être nécessaires en dehors du contexte d’exécution au premier plan.

**Demander l’exécution des tâches en arrière-plan:  **

> **Important** Depuis Windows 10, les applications n’ont plus besoin de figurer dans l’écran de verrouillage pour exécuter des tâches en arrière-plan.

Les applications de plateforme Windows universelle (UWP) peuvent exécuter tous les types de tâche pris en charge sans être épinglées à l’écran de verrouillage. Toutefois, les applications doivent appeler [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire tout type de tâche en arrière-plan. Cette méthode retourne [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439) si l’utilisateur a explicitement refusé des autorisations de tâche en arrière-plan pour votre application dans les paramètres de l’appareil.
## Liste de contrôle relative aux tâches en arrière-plan


La liste de vérification suivante s’applique à toutes les tâches en arrière-plan.

-   Créez votre tâche en arrière-plan dans un composant WindowsRuntime.
-   Associez votre tâche en arrière-plan au déclencheur approprié.
-   Ajoutez des conditions pour assurer une exécution aboutie de votre tâche en arrière-plan.
-   Gérez la progression, l’achèvement et l’annulation de la tâche en arrière-plan.
-   N’affichez pas l’interface utilisateur en dehors des toasts, des vignettes et des mises à jour de badge à partir de la tâche en arrière-plan.
-   Dans la méthode [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), demandez des reports pour chaque appel de méthode asynchrone, puis fermez-les lorsque la méthode est terminée. Vous pouvez également utiliser un report avec **async/await**.
-   Utilisez un dispositif de stockage persistant pour partager des données entre la tâche en arrière-plan et l’application.
-   Déclarez chaque tâche en arrière-plan dans le manifeste de l’application, de même que le type de déclencheur avec lequel elle est utilisée. Assurez-vous que le point d’entrée et les types de déclencheur sont corrects.
-   Écrivez des tâches en arrière-plan de courte durée. Les tâches en arrière-plan sont limitées à 30 secondes de l’utilisation de l’horloge.
-   Ne comptez pas sur l’interaction utilisateur dans les tâches en arrière-plan.
-   Réinscrivez vos tâches en arrière-plan pendant le lancement de l’application. De cette façon, elles seront inscrites au premier lancement de l’application. Cela permet également de détecter si l’utilisateur a désactivé les fonctionnalités d’exécution en arrière-plan de votre application (en cas d’échec de l’inscription).
-   Ne spécifiez aucun élément Executable dans le manifeste sauf si vous utilisez un déclencheur qui doit être exécuté dans le même contexte que l’application (par exemple le [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)).
-   Recherchez les erreurs d’inscription de la tâche en arrière-plan. Le cas échéant, tentez d’inscrire la tâche en arrière-plan une nouvelle fois avec d’autres valeurs de paramètres.
-   Pour toutes les familles d’appareils, à l’exception des ordinateurs de bureau, les tâches en arrière-plan peuvent être arrêtées en cas de mémoire insuffisante de l’appareil. Si aucune exception de mémoire insuffisante n’est exposée ou si l’application ne la gère pas, la tâche en arrière-plan est alors arrêtée sans avertissement ni déclenchement de l’événement OnCanceled. Cela permet de garantir l’expérience utilisateur de l’application au premier plan. Votre tâche en arrière-plan doit être conçue de manière à gérer ce scénario.

## Windows : Liste de contrôle des tâches en arrière-plan pour les applications compatibles avec l’écran de verrouillage


Suivez ces recommandations lorsque vous développez des tâches en arrière-plan pour des applications pouvant figurer dans l’écran de verrouillage. Suivez les recommandations de la rubrique [Recommandations et liste de vérification sur les vignettes d’écran de verrouillage](https://msdn.microsoft.com/library/windows/apps/hh465403).

-   Assurez-vous que votre application a besoin de figurer dans l’écran de verrouillage avant de la développer en tant qu’application compatible avec l’écran de verrouillage. Pour plus d’informations, voir [Vue d’ensemble des écrans de verrouillage](https://msdn.microsoft.com/library/windows/apps/hh779720).

-   Vérifiez que votre application fonctionne toujours lorsqu’elle ne figure pas dans l’écran de verrouillage.

-   Incluez une tâche en arrière-plan inscrite avec [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) ou [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) et déclarez-la dans le manifeste de l’application. Assurez-vous que le point d’entrée et les types de déclencheur sont corrects. Il s’agit d’une condition de certification, et cela permet à l’utilisateur de placer l’application dans l’écran de verrouillage.

**Remarque**  
Cet article s’adresse aux développeurs de Windows10 qui développent des applications de la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


