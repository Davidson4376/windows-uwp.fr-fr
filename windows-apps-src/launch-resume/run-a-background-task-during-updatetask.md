---
author: TylerMSFT
title: Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour
description: Découvrez comment créer une tâche en arrière-plan qui s’exécute lorsque votre application du Windows Store de la plateforme Windows universelle (UWP) est mise à jour.
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, mise à jour, tâche en arrière-plan, updatetask, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4263144"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour

Découvrez comment écrire une tâche en arrière-plan qui s’exécute une fois que votre application du Windows store de plateforme Windows universelle (UWP) est mis à jour.

La tâche de mettre à jour la tâche en arrière-plan est appelé par le système d’exploitation une fois que l’utilisateur installe une mise à jour vers une application qui est installée sur l’appareil. Cela permet à votre application effectuer des tâches d’initialisation telles que l’initialisation d’un nouveau canal de notification push, mise à jour de schéma de base de données et ainsi de suite, avant que l’utilisateur lance votre application mise à jour.

La tâche de mise à jour est différent de lancement d’une tâche en arrière-plan à l’aide de la gâchette [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) , car dans ce cas votre application doit s’exécuter au moins une fois avant qu’il est mis à jour afin d’inscrire la tâche en arrière-plan qui est activée par le ** ServicingComplete** déclencheur.  La tâche de mise à jour n’est pas inscrite et par conséquent, une application qui n’a jamais été exécutée, mais qui est mis à niveau, auront toujours sa tâche de mise à jour déclenchée.

## <a name="step-1-create-the-background-task-class"></a>Étape 1: Créer la classe de tâche en arrière-plan

Comme avec d’autres types de tâches en arrière-plan, vous mettre en œuvre la tâche de mise à jour la tâche en arrière-plan en tant qu’un composant Windows Runtime. Pour créer ce composant, suivez les étapes décrites dans la section de **créer la classe de tâche en arrière-plan** de [créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Les étapes sont les suivantes:

- Ajout d’un projet de composant Windows Runtime à votre solution.
- Création d’une référence au composant à partir de votre application.
- Création d’une classe publique, sealed dans le composant qui implémente [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implémentez la méthode [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , qui est le point d’entrée obligatoire qui est appelé lorsque la tâche de mise à jour est exécutée. Si vous souhaitez effectuer des appels asynchrones à partir de votre tâche en arrière-plan, [créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explique comment utiliser un report dans votre méthode **Run** .

Vous n’avez pas besoin d’enregistrer cette tâche en arrière-plan (la section «Inscrire la tâche en arrière-plan s’exécute» dans la rubrique **créer et inscrire une tâche en arrière-plan hors processus** ) pour utiliser la tâche de mise à jour. Il s’agit de la principale raison d’utiliser une tâche de mise à jour, car vous n’avez pas besoin d’ajouter du code à votre application pour inscrire la tâche et l’application ne doit pas s’exécuter au moins une fois avant mis à jour pour inscrire la tâche en arrière-plan.

L’exemple de code suivant montre un point de départ de base pour une classe de tâche en arrière-plan tâches de mise à jour en c#. La classe de tâche en arrière-plan elle-même - et toutes les autres classes dans le projet de tâche en arrière-plan - doivent être **publics** et **sealed**. Votre classe de tâche en arrière-plan doit dériver de **IBackgroundTask** et avoir une méthode **Run()** publique avec la signature illustrée ci-dessous:

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Étape 2: Déclarer votre tâche en arrière-plan dans le manifeste du package

Dans l’Explorateur de solutions Visual Studio, cliquez sur **Package.appxmanifest** , cliquez sur **Afficher le Code** pour afficher le manifeste du package. Ajoutez le code suivant `<Extensions>` XML pour déclarer votre tâche de mise à jour:

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

Dans le code XML ci-dessus, vérifiez que le `EntryPoint` attribut a pour valeur le nom namespace.class de votre classe de tâche de mise à jour. Le nom respecte la casse.

## <a name="step-3-debugtest-your-update-task"></a>Étape 3: Débogage/test votre tâche de mise à jour

Assurez-vous que vous avez déployé votre application sur votre ordinateur afin qu’il existe un élément pour mettre à jour.

Définissez un point d’arrêt dans la méthode Run() de votre tâche en arrière-plan.

![point d’arrêt défini](images/run-func-breakpoint.png)

Ensuite, dans l’Explorateur de solutions, cliquez sur le projet de votre application (et non le projet de tâche en arrière-plan), puis sur **Propriétés**. Dans la fenêtre Propriétés de l’application, cliquez sur **Déboguer** de gauche, puis sélectionnez **ne pas lancer, mais déboguer mon code au démarrage**:

![définir les paramètres de débogage](images/do-not-launch-but-debug.png)

Ensuite, pour vous assurer que le UpdateTask est déclenchée, augmentez le numéro de version du package. Dans l’Explorateur de solutions, double-cliquez sur le fichier **Package.appxmanifest** de votre application pour ouvrir le Concepteur de packages et ensuite mettre à jour le numéro de **Build** :

![mise à jour de la version](images/bump-version.png)

Maintenant, dans Visual Studio 2017 lorsque vous appuyez sur F5, mise à jour votre application et le système s’activent votre composant UpdateTask en arrière-plan. Le débogueur s’attache automatiquement pour le processus en arrière-plan. Obtenir rencontrer votre point d’arrêt et vous pouvez parcourir votre logique de code de mise à jour.

Lorsque la tâche en arrière-plan est terminée, vous pouvez lancer l’application au premier plan du menu Démarrer de Windows au sein de la même session de débogage. Le débogueur s’attache à nouveau automatiquement, cette fois à votre processus de premier plan, et vous pouvez parcourir la logique de votre application.

> [!NOTE]
> Les utilisateurs de Visual Studio 2015: les étapes ci-dessus s’appliquent à Visual Studio 2017. Si vous utilisez Visual Studio 2015, vous pouvez utiliser les mêmes techniques à déclencheur et testez le UpdateTask, à l’exception de Visual Studio ne sera pas attachée à celui-ci. Une autre possibilité dans Visual Studio 2015 consiste à configurer un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) qui définit l’UpdateTask en tant que son Point d’entrée et déclenche l’exécution directement à partir de l’application au premier plan.

## <a name="see-also"></a>Articles associés

[Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
