---
title: Inscrire une tâche en arrière-plan
description: Découvrez comment créer une fonction que vous pouvez réutiliser pour inscrire la plupart des tâches en arrière-plan en toute sécurité.
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
ms.date: 02/08/2017
ms.topic: article
keywords: tâche en arrière-plan Windows 10, uwp,
ms.localizationpriority: medium
ms.openlocfilehash: f940b0433c5cf7818102f92c9e61a6fe012bf4b9
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8886514"
---
# <a name="register-a-background-task"></a>Inscrire une tâche en arrière-plan


**API importantes**

-   [**Classe BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**Classe BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**Classe SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Découvrez comment créer une fonction que vous pouvez réutiliser pour inscrire la plupart des tâches en arrière-plan en toute sécurité.

Cette rubrique s’applique aussi bien aux tâches en arrière-plan in-process qu’aux tâches en arrière-plan hors processus. Cette rubrique suppose que vous disposez déjà d’une tâche en arrière-plan nécessitant une inscription. (Consultez les rubriques [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md) ou [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md) pour découvrir comment écrire une tâche en arrière-plan).

Cette rubrique décrit une fonction utilitaire chargée d’inscrire les tâches en arrière-plan. Pour éviter tout problème avec de multiples inscriptions, cette fonction utilitaire recherche d’abord des inscriptions existantes avant d’inscrire la tâche plusieurs fois. Elle peut aussi appliquer une condition système à la tâche en arrière-plan. La procédure pas à pas inclut un exemple de mise en pratique exhaustif de cette fonction utilitaire.

**Remarque**  

Les applications Windows universelles doivent appeler l’élément [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire n’importe quel type de déclencheur en arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, vous devez appeler [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

## <a name="define-the-method-signature-and-return-type"></a>Définir la signature de la méthode et le type de retour

Cette méthode contient le point d’entrée de la tâche, son nom, un déclencheur de tâche en arrière-plan créé à l’avance et un objet [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) pour la tâche en arrière-plan (facultatif). Cette méthode renvoie un objet [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).

> [!Important]
> `taskEntryPoint` - pour les tâches en arrière-plan qui s’exécutent hors processus, la chaîne doit être au format suivant: nom de l’espace de noms, «.» et nom de la classe contenant votre classe en arrière-plan. La chaîne est sensible à la casse.  Par exemple, si vous avez un espace de noms «MyBackgroundTasks» et une classe «BackgroundTask1» contenant votre code de classe en arrière-plan, la chaîne pour `taskEntryPoint` serait «MyBackgroundTasks.BackgroundTask1».
> Si votre tâche en arrière-plan s’exécute dans le même processus que votre application (c’est-à-dire dans le cas d’une tâche en arrière-plan in-process), la chaîne `taskEntryPoint` ne doit pas être définie.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```

## <a name="check-for-existing-registrations"></a>Rechercher des inscriptions existantes

Vérifiez si la tâche est déjà inscrite. Cette vérification est primordiale car, si la tâche est inscrite plusieurs fois, elle sera exécutée plusieurs fois à chaque fois qu’elle est déclenchée, ce qui peut aboutir à une utilisation excessive du processeur et entraîner un comportement inattendu.

Pour rechercher des inscriptions existantes, vous pouvez interroger la propriété [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) et examiner le résultat. Vérifiez le nom de chaque instance. S’il correspond au nom de la tâche que vous inscrivez, sortez de la boucle et définissez une variable d’indicateur afin que votre code puisse choisir un chemin différent lors de la prochaine étape.

> **Remarque**utiliser des noms de tâche en arrière-plan qui sont propres à votre application. Assurez-vous que chaque tâche en arrière-plan possède un nom unique.

Le code qui suit inscrit une tâche en arrière-plan à l’aide de l’objet [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) créé au cours de la dernière étape :

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == name)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     // We'll register the task in the next step.
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     // We'll register the task in the next step.
> }
> ```

## <a name="register-the-background-task-or-return-the-existing-registration"></a>Inscrire la tâche en arrière-plan (ou renvoyer l’inscription existante)


Vérifiez si la tâche a été trouvée dans la liste des inscriptions de tâches en arrière-plan existantes. Dans l’affirmative, renvoyez cette instance de la tâche.

Inscrivez ensuite la tâche à l’aide d’un nouvel objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Le code doit vérifier si le paramètre de condition est Null. Si cela n’est pas le cas, ajoutez la condition à l’objet d’inscription. Renvoyez l’objet [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) renvoyé par la méthode [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772).

> **Remarque**paramètres d’inscription de tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de manière fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.
> **Remarque** Si vous inscrivez une tâche en arrière-plan qui s’exécute dans le même processus que votre application, envoyez `String.Empty` ou `null` pour le paramètre `taskEntryPoint`.

L’exemple suivant renvoie la tâche existante ou bien ajoute le code chargé d’inscrire la tâche en arrière-plan (ycompris la condition système facultative si elle est fournie):

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = name;
>
>     // in-process background tasks don't set TaskEntryPoint
>     if ( taskEntryPoint != null && taskEntryPoint != String.Empty)
>     {
>         builder.TaskEntryPoint = taskEntryPoint;
>     }
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="complete-background-task-registration-utility-function"></a>Fonction utilitaire d’inscription des tâches en arrière-plan terminée

Cet exemple montre la fonction d’inscription des tâches en arrière-plan parvenue à son terme. Vous pouvez vous servir de cette fonction pour inscrire la plupart des tâches en arrière-plan, à l’exception des tâches en arrière-plan réseau.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> public static BackgroundTaskRegistration RegisterBackgroundTask(string taskEntryPoint,
>                                                                 string taskName,
>                                                                 IBackgroundTrigger trigger,
>                                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = taskName;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(Platform::String ^ taskEntryPoint,
>                                                              Platform::String ^ taskName,
>                                                              IBackgroundTrigger ^ trigger,
>                                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="related-topics"></a>Rubriquesassociées

* [Créer et inscrire une tâche en arrière-plan hors processus](create-and-register-a-background-task.md)
* [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications UWP (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)