---
author: TylerMSFT
title: Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour
description: Découvrez comment créer une tâche en arrière-plan qui s’exécute lorsque votre application du Windows Store de la plateforme Windows universelle (UWP) est mise à jour.
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10, uwp, mise à jour, tâche en arrière-plan, updatetask, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 1ef6351bcf2ef57a1900c429ddcb65e5a2a4e67b
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7438292"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour

Découvrez comment écrire une tâche en arrière-plan qui s’exécute une fois que votre application du Windows store de plateforme Windows universelle (UWP) est mis à jour.

Une fois que l’utilisateur installe une mise à jour vers une application qui est installée sur l’appareil, la tâche de mise à jour la tâche en arrière-plan est appelé par le système d’exploitation. Cela permet à votre application effectuer des tâches d’initialisation telles que l’initialisation d’un nouveau canal de notification push, mise à jour du schéma de base de données et ainsi de suite, avant que l’utilisateur lance votre application mise à jour.

La tâche de mise à jour est différente de lancement d’une tâche en arrière-plan à l’aide du déclencheur [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) car dans ce cas votre application doit s’exécuter au moins une fois avant qu’il est mis à jour afin d’inscrire la tâche en arrière-plan qui est activée par le ** ServicingComplete** déclencheur.  La tâche de mise à jour n’est pas inscrite et par conséquent, une application qui n’a jamais été exécutée, mais qui est mis à niveau, aura toujours sa tâche de mise à jour déclenchée.

## <a name="step-1-create-the-background-task-class"></a>Étape 1: Créer la classe de tâche en arrière-plan

Comme avec d’autres types de tâches en arrière-plan, vous mettre en œuvre la tâche en arrière-plan de tâche de mise à jour comme un composant Windows Runtime. Pour créer ce composant, suivez les étapes décrites dans la section de **créer la classe de tâche en arrière-plan** de [créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Les étapes sont les suivantes:

- Ajout d’un projet de composant Windows Runtime à votre solution.
- Création d’une référence au composant à partir de votre application.
- Création d’une classe publique, sealed dans le composant qui implémente [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implémentez la méthode [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , qui est le point d’entrée obligatoire qui est appelé lorsque la tâche de mise à jour est exécutée. Si vous souhaitez effectuer des appels asynchrones à partir de votre tâche en arrière-plan, [créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explique comment utiliser un report dans votre méthode **Run** .

Vous n’avez pas besoin d’enregistrer cette tâche en arrière-plan (la section «Inscrire la tâche en arrière-plan s’exécute» dans la rubrique **créer et inscrire une tâche en arrière-plan hors processus** ) pour utiliser la tâche de mise à jour. Il s’agit de la principale raison d’utiliser une tâche de mise à jour, car vous n’avez pas besoin d’ajouter du code à votre application pour inscrire la tâche et l’application ne doit pas s’exécuter au moins une fois avant la mise à jour pour inscrire la tâche en arrière-plan.

L’exemple de code suivant montre un point de départ de base pour une classe de tâche en arrière-plan des tâches de mise à jour en c#. La classe de tâche en arrière-plan elle-même - et toutes les autres classes dans le projet de tâche en arrière-plan - doivent être **publics** et **sealed**. Votre classe de tâche en arrière-plan doit dériver de **IBackgroundTask** et avoir une méthode **Run()** publique avec la signature illustrée ci-dessous:

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

Dans l’Explorateur de solutions Visual Studio, cliquez sur **Package.appxmanifest** , puis cliquez sur **Afficher le Code** pour afficher le manifeste du package. Ajoutez le code suivant `<Extensions>` XML pour déclarer votre tâche de mise à jour:

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

Ensuite, dans l’Explorateur de solutions, cliquez sur le projet de votre application (et non le projet de tâche en arrière-plan) et puis cliquez sur **Propriétés**. Dans la fenêtre de propriétés d’application, cliquez sur **Déboguer** de gauche, puis sélectionnez **ne pas lancer, mais déboguer mon code au démarrage**:

![définir les paramètres de débogage](images/do-not-launch-but-debug.png)

Ensuite, pour vous assurer que le UpdateTask est déclenchée, augmentez le numéro de version du package. Dans l’Explorateur de solutions, double-cliquez sur le fichier de votre application **Package.appxmanifest** pour ouvrir le Concepteur de packages et ensuite mettre à jour le numéro de **Build** :

![mise à jour de la version](images/bump-version.png)

Désormais, dans Visual Studio 2017 lorsque vous appuyez sur F5, mise à jour votre application et le système activera votre composant UpdateTask en arrière-plan. Le débogueur s’attache automatiquement pour le processus en arrière-plan. Obtenir atteignez votre point d’arrêt et vous pouvez parcourir votre logique de code de mise à jour.

Lorsque la tâche en arrière-plan est terminée, vous pouvez lancer l’application au premier plan du menu Démarrer de Windows au sein de la même session de débogage. Le débogueur s’attache à nouveau automatiquement, cette fois à votre processus de premier plan, et vous pouvez parcourir la logique de votre application.

> [!NOTE]
> Les utilisateurs de Visual Studio 2015: les étapes ci-dessus s’appliquent à Visual Studio 2017. Si vous utilisez Visual Studio 2015, vous pouvez utiliser les mêmes techniques de déclencheur et de test le UpdateTask, à l’exception de Visual Studio ne sera pas attacher à celui-ci. Une autre possibilité dans Visual Studio 2015 consiste à configurer un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) qui définit la UpdateTask que son Point d’entrée et déclenche l’exécution directement à partir de l’application au premier plan.

## <a name="see-also"></a>Voir aussi

[Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
