---
author: mcleblanc
title: Définir des conditions pour exécuter une tâche en arrière-plan
description: Découvrez comment définir des conditions qui contrôlent le moment auquel votre tâche en arrière-plan s’exécutera.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
---

# Définir des conditions pour exécuter une tâche en arrière-plan


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Découvrez comment définir des conditions qui contrôlent le moment auquel votre tâche en arrière-plan s’exécutera.

Parfois, outre l’événement chargé de déclencher la tâche, une tâche en arrière-plan doit remplir certaines conditions pour pouvoir aboutir. Vous pouvez spécifier une ou plusieurs des conditions spécifiées par [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) au moment d’inscrire votre tâche en arrière-plan. La condition est vérifiée après que le déclencheur est activé ; la tâche en arrière-plan est mise en file d’attente et ne s’exécute qu’à partir du moment où toutes les conditions requises sont satisfaites.

L’affectation de conditions aux tâches en arrière-plan permet d’économiser l’autonomie de la batterie et le temps d’exécution processeur en empêchant toute exécution inutile des tâches. Par exemple, si votre tâche en arrière-plan est exécutée sur un minuteur et nécessite une connectivité Internet, ajoutez la condition **InternetAvailable** au [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) avant d’inscrire la tâche. Cela empêchera ainsi la tâche de faire inutilement appel aux ressources système et à l’autonomie de la batterie. Elle s’exécutera une fois que le minuteur sera arrivé à expiration et qu’Internet sera accessible.

**Remarque** Il est possible de combiner plusieurs conditions en appelant AddCondition plusieurs fois sur le même [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Veillez à ne pas ajouter de conditions conflictuelles, telles que **UserPresent** et **UserNotPresent**.

 

## Créer un objet SystemCondition


Cette rubrique suppose qu’une tâche en arrière-plan est déjà associée à votre application et que cette dernière comporte déjà du code qui crée un objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) nommé **taskBuilder**.

Avant d’ajouter la condition, créez un objet [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) représentant la condition qui doit être effective pour qu’une tâche en arrière-plan soit exécutée. Dans le constructeur, spécifiez la condition qui doit être remplie en fournissant une valeur d’énumération [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

Le code suivant crée un objet [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) qui spécifie l’accessibilité à Internet comme étant une condition essentielle :

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## Ajouter l’objet SystemCondition à votre tâche en arrière-plan


Pour ajouter la condition, appelez la méthode [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) sur l’objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) et transmettez-lui l’objet [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834).

Le code suivant inscrit la condition de tâche en arrière-plan InternetAvailable avec TaskBuilder :

> [!div class="tabbedCodeSnippets"]
> ```cs
> taskBuilder.AddCondition(internetCondition);
> ```
> ```cpp
> taskBuilder->AddCondition(internetCondition);
> ```

## Inscrire votre tâche en arrière-plan


Vous pouvez à présent inscrire votre tâche en arrière-plan à l’aide de la méthode [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) ; la tâche ne démarrera pas tant que la condition spécifiée n’aura pas été satisfaite.

Le code suivant inscrit la tâche et stocke l’objet BackgroundTaskRegistration obtenu :

> [!div class="tabbedCodeSnippets"]
> ```cs
> BackgroundTaskRegistration task = taskBuilder.Register();
> ```
> ```cpp
> BackgroundTaskRegistration ^ task = taskBuilder->Register();
> ```

> **Remarque** Les applications Windows universelles doivent appeler [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire tout type de déclencheur en arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, vous devez appeler [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471), puis [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) lorsque votre application est lancée après avoir été mise à jour. Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

> **Remarque** Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de façon fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

## Placer plusieurs conditions dans la tâche en arrière-plan

Pour ajouter plusieurs conditions, votre application effectue plusieurs appels à la méthode [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769). Pour être effectifs, ces appels doivent intervenir avant l’inscription de la tâche.

> **Remarque** Veillez à ne pas ajouter de conditions conflictuelles à une tâche en arrière-plan.
 

L’extrait de code suivant présente plusieurs conditions dans un contexte de création et d’inscription d’une tâche en arrière-plan :

> [!div class="tabbedCodeSnippets"]
```cs
> //
> // Set up the background task.
> //
>
> TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
>
> var recurringTaskBuilder = new BackgroundTaskBuilder();
>
> recurringTaskBuilder.Name           = "Hourly background task";
> recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder.SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
>
> recurringTaskBuilder.AddCondition(userCondition);
> recurringTaskBuilder.AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```
```cpp
> //
> // Set up the background task.
> //
>
> TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
>
> auto recurringTaskBuilder = ref new BackgroundTaskBuilder();
>
> recurringTaskBuilder->Name           = "Hourly background task";
> recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder->SetTrigger(hourlyTrigger);
>
> //
> // Begin adding conditions.
> //
>
> SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
>
> recurringTaskBuilder->AddCondition(userCondition);
> recurringTaskBuilder->AddCondition(internetCondition);
>
> //
> // Done adding conditions, now register the background task.
> //
>
> BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## Remarques


> **Remarque** Choisissez les conditions appropriées pour votre tâche en arrière-plan afin qu’elle s’exécute uniquement lorsque cela est nécessaire, et non à un moment où elle sera inopérante. Voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) pour obtenir une description des différentes conditions de tâche en arrière-plan.

> **Remarque** Cet article s’adresse aux développeurs Windows 10 qui créent des applications de plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Rubriques connexes


****

* [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Mettre à jour une vignette dynamique à partir d’une tâche en arrière-plan](update-a-live-tile-from-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md)

****

* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 


<!--HONumber=May16_HO2-->


