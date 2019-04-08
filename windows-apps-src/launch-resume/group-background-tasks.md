---
title: Regrouper une inscription de la tâche en arrière-plan.
description: Inscrivez/désinscrivez des tâches en arrière-plan dans un groupe, afin d’isoler ces inscriptions.
ms.date: 04/05/2017
ms.topic: article
keywords: Windows 10, tâche en arrière-plan
ms.openlocfilehash: a70c814e5e35359746076c5418d1f1d973e61773
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623854"
---
# <a name="group-background-task-registration"></a>Regrouper une inscription de la tâche en arrière-plan.

**API importantes**

[Classe de BackgroundTaskRegistrationGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

Les tâches en arrière-plan peuvent désormais être inscrites dans un groupe, que vous pouvez considérer comme un espace de noms logique. Cette isolation contribue à garantir que les différents composants d’une application, ou les différentes bibliothèques, n’interfèrent pas avec les inscriptions de tâche en arrière-plan des autres.

Lorsqu’une application et l’infrastructure (ou bibliothèque) qu’elle utilise inscrit une tâche en arrière-plan qui porte le même nom, l’application peut par inadvertance supprimer les inscriptions des tâches en arrière-plan de l’infrastructure. Les auteurs d’application peuvent également supprimer accidentellement des inscriptions de tâches en arrière-plan d’infrastructure et de bibliothèque car ils peuvent désinscrire toutes les tâches en arrière-plan inscrites à l’aide de [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks).  Les groupes vous permettent d’isoler vos inscriptions de tâche en arrière-plan pour que cela ne se produise pas.

## <a name="features-of-groups"></a>Caractéristiques des groupes

* Les groupes peuvent être identifiés de façon unique à l’aide d’un GUID. Ils peuvent également être associés à une chaîne de nom conviviale qui simplifie la lecture pendant le débogage.
* Plusieurs tâches en arrière-plan peuvent être inscrites dans un groupe.
* Les tâches en arrière-plan inscrites dans un groupe ne s’affichent pas dans [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks). Par conséquent, les applications qui utilisent actuellement **BackgroundTaskRegistration.AllTasks** pour désinscrire leurs tâches ne vont pas désinscrire par inadvertance leurs tâches en arrière-plan inscrites dans un groupe. Consultez [annuler l’inscription de tâches en arrière-plan dans un groupe](#unregister-background-tasks-in-a-group) ci-dessous pour voir comment annuler l’inscription de tous les déclencheurs d’arrière-plan qui ont été enregistrés en tant que partie d’un groupe.
* Chaque inscription de tâche en arrière-plan est dotée d’une propriété Group permettant de déterminer le groupe auquel elle est associée.
* Tâches en arrière-plan de l’inscription d’In-Process avec un groupe entraîne l’activation à passer par [BackgroundTaskRegistrationGroup.BackgroundActivated](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) événements au lieu de [Application.OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

## <a name="register-a-background-task-in-a-group"></a>Inscrire une tâche en arrière-plan dans un groupe

L’exemple suivant montre comment inscrire une tâche en arrière-plan (déclenchée par un changement de fuseau horaire, dans cet exemple) comme élément d’un groupe.

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>Désinscrire des tâches en arrière-plan d’un groupe

Voici comment désinscrire des tâches en arrière-plan qui ont été inscrites comme éléments d’un groupe.
Comme les tâches en arrière-plan inscrites dans un groupe n’apparaissent pas dans [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks), vous devez parcourir les groupes, trouver les tâches en arrière-plan inscrites dans chaque groupe et les désinscrire.

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>Inscrire des événements persistants

Lorsque vous inscrivez des tâches en arrière-plan dans des groupes pour des tâches en arrière-plan in-process, les activations en arrière-plan sont dirigées vers l’événement du groupe et non vers celui de l’objet Application ou CoreApplication. Cela permet à plusieurs composants de votre application de gérer l’activation, plutôt que de placer tous les chemins de code d’activation dans l’objet Application. L’exemple suivant montre comment s’inscrire à l’événement d’arrière-plan activé du groupe. Vérifiez d’abord [BackgroundTaskRegistration.GetTaskGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) pour déterminer si le groupe a déjà été inscrit. Si ce n’est pas le cas, créez un groupe avec votre identifiant et un nom convivial. Ensuite, inscrivez un gestionnaire d’événements à l’événement BackgroundActivated sur le groupe.

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```
