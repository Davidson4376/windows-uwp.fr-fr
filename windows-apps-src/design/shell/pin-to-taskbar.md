---
author: mijacobs
Description: You can programmatically pin your app to the taskbar,  bnd you can check if it's currently pinned.
title: Épingler votre application à la barre des tâches
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, la barre des tâches, gestionnaire de la barre des tâches, épingler à la barre des tâches, vignette principale
ms.localizationpriority: medium
ms.openlocfilehash: 47fcd1f9d090c49ecbd49e05696b33f789973160
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6458474"
---
# <a name="pin-your-app-to-the-taskbar"></a>Épingler votre application à la barre des tâches

Vous pouvez épingler par programme votre propre application à la barre des tâches, comme vous pouvez [épingler votre application au menu Démarrer](tiles-and-notifications/primary-tile-apis.md). Vous pouvez également vérifier si votre application est actuellement épinglée, et si la barre des tâches permet l’épinglage. 

![Barre des tâches](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **Nécessite Fall Creators Update**: vous devez cibler le Kit de développement logiciel (SDK)16299 et exécuter la Build16299 ou une version plus récente pour utiliser les API de la barre des tâches.

> **API importantes**: [classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>Quand demander à l’utilisateur d’épingler votre application à la barre des tâches? 

La [classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) permet de demander à l’utilisateur d’épingler votre application à la barre des tâches. L’utilisateur doit approuver la demande. Vous vous êtes efforcé de créer une application exceptionnelle, et maintenant, vous avez la possibilité de demander à l’utilisateur de l’épingler à la barre des tâches. Néanmoins, avant de nous pencher sur le code, voici quelques éléments à garder à l’esprit lorsque vous concevez votre expérience:

* **Concevez** une expérience utilisateur non perturbatrice et facilement révocable dans votre application avec un appel à l’action «Épingler à la barre des tâches» clair. Évitez d’utiliser des boîtes de dialogue et des menus volants à cet effet. 
* **Expliquez** clairement la valeur de votre application avant de demander à l’utilisateur de l’épingler.
* **Ne demandez pas** à un utilisateur d’épingler votre application si la vignette est déjà épinglée ou si l’appareil ne la prend pas en charge. (Cet article explique comment déterminer si l’épinglage est pris en charge.)
* **Ne demandez pas** plusieurs fois à l’utilisateur d’épingler votre application (cela risque de l’agacer).
* **N’appelez pas** l’API Pin sans interaction explicite de l’utilisateur ou lorsque votre application est réduite ou fermée.


## <a name="1-check-whether-the-required-apis-exist"></a>1. Vérifiez si les API nécessaires existent

Si votre application prend en charge des versions antérieures de Windows10, vous devez vérifier si la classe TaskbarManager est disponible. Vous pouvez utiliser la [méthode ApiInformation.IsTypePresent](https://docs.microsoft.com/en-us/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) pour effectuer cette vérification. Si la classe TaskbarManager n’est pas disponible, évitez d’exécuter des appels vers les API.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. Vérifiez si la barre des tâches est présente et permet l’épinglage

Les applications UWP peuvent s’exécuter sur un large éventail d’appareils, qui ne prennent pas tous en charge la barre des tâches. Actuellement, seuls les appareils de bureau prennent en charge la barre des tâches. 

Même si la barre des tâches est disponible, une stratégie de groupe sur l’ordinateur de l’utilisateur peut désactiver l’épinglage à la barre des tâches. Par conséquent, avant de tenter d’épingler votre application, vous devez vérifier si l’épinglage à la barre des tâches est pris en charge. La propriété [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) renvoie la valeur true si la barre des tâches est présente et permet l’épinglage. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> Si vous ne voulez pas épingler votre application à la barre des tâches et souhaitez simplement savoir si la barre des tâches est disponible, utilisez la propriété [TaskbarManager.IsSupported](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported).


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. Vérifiez si votre application est actuellement épinglée à la barre des tâches

Bien évidemment, il est inutile de demander à l’utilisateur d’autoriser l’épinglage de l’application à la barre des tâches s’il y est déjà épinglé. Vous pouvez utiliser la méthode [TaskbarManager.IsCurrentAppPinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync) pour vérifier si l’application est déjà épinglée avant de poser la question à l’utilisateur.

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4. Épinglez votre application

Si la barre des tâches est présente, l’épinglage autorisé et si votre application n’est pas actuellement épinglée, vous souhaiterez peut-être afficher une info-bulle pour informer les utilisateurs qu’ils peuvent épingler votre application. Par exemple, vous pouvez afficher une icône représentant une épingle dans votre interface utilisateur sur laquelle l’utilisateur peut cliquer. 

Si l’utilisateur clique sur l’interface utilisateur de suggestion d’épinglage, vous appelez ensuite la [méthode TaskbarManager.RequestPinCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync). Cette méthode affiche une boîte de dialogue qui demande à l’utilisateur de confirmer qu’il souhaite épingler votre application à la barre des tâches.

> [!IMPORTANT]
> Elle doit être appelée à partir d’un thread d’interface utilisateur de premier plan, sans quoi une exception est levée.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![Boîte de dialogue d’épinglage](images/taskbar/pin-dialog.png)

Cette méthode renvoie une valeur booléenne qui indique si votre application est désormais épinglée à la barre des tâches. Si votre application a déjà été épinglée, la méthode retourne immédiatement la valeur true sans afficher la boîte de dialogue à l’utilisateur. Si l’utilisateur clique sur «non» dans la boîte de dialogue ou si l’épinglage à la barre des tâches n’est pas autorisé, la méthode retourne la valeur false. Dans le cas contraire, l’utilisateur clique sur oui et l’application est épinglée, et l’API retourne la valeur true.


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [Classe TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Épingler une application au menu Démarrer](tiles-and-notifications/primary-tile-apis.md)