---
title: Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour
description: Découvrez comment créer une tâche en arrière-plan qui s’exécute lorsque votre application du Windows Store de la plateforme Windows universelle (UWP) est mise à jour.
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10, uwp, mise à jour, tâche en arrière-plan, updatetask, tâche en arrière-plan
ms.localizationpriority: medium
ms.openlocfilehash: 3683595926f20fdd9f9af5929db65396b0001bcc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371486"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour

Découvrez comment écrire une tâche en arrière-plan qui s’exécute après la mise à jour de votre application du Windows Store de la plateforme Windows universelle (UWP).

La tâche en arrière-plan de mise à jour est appelée par le système d’exploitation une fois que l’utilisateur a installé une mise à jour d’une application installée sur l’appareil. Cela permet à votre application d’effectuer des tâches d’initialisation telles que l’initialisation d’un nouveau canal de notification Push, la mise à jour d’un schéma de base de données et ainsi de suite, avant que l’utilisateur ne lance votre application mise à jour.

La tâche de mise à jour diffère du lancement d’une tâche en arrière-plan à l’aide du déclencheur [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType), car, dans ce cas, votre application doit s’exécuter au moins une fois avant d’être mise à jour, de manière à inscrire la tâche en arrière-plan qui sera activée par le déclencheur **ServicingComplete**.  Comme la tâche de mise à jour n’est pas inscrite, une application qui n’a jamais été exécutée, mais qui est mise à niveau, verra tout de même sa tâche de mise à jour déclenchée.

## <a name="step-1-create-the-background-task-class"></a>Étape 1 : Créer la classe de tâche en arrière-plan

Comme pour les autres types de tâches en arrière-plan, l’implémentation de la tâche en arrière-plan de mise à jour est réalisée sous forme de composant Windows Runtime. Pour créer ce composant, suivez les étapes de la section **Créer la classe de tâche en arrière-plan** de la rubrique [Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Ces étapes sont les suivantes :

- Ajout d’un projet de composant Windows Runtime à votre solution.
- Création d’une référence entre votre application et le composant.
- Création d’une classe public sealed dans le composant qui implémente [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).
- Implémentation de la méthode [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.), qui correspond au point d’entrée requis appelé lors de l’exécution de la tâche de mise à jour. Si vous envisagez de passer des appels asynchrones à partir de votre tâche en arrière-plan, la rubrique [Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explique comment utiliser un report dans votre méthode **Run**.

Vous n’avez pas besoin d’inscrire cette tâche en arrière-plan (la section « Inscrire la tâche en arrière-plan à des fins d’exécution » dans la rubrique **Créer et inscrire une tâche en arrière-plan hors processus**) pour utiliser la tâche de mise à jour. Il s’agit du principal motif d’utilisation de la tâche de mise à jour car vous n’avez pas besoin d’ajouter du code à votre application pour inscrire la tâche et l’application n’a pas besoin de s’exécuter au moins une fois avant d’être mise à jour pour inscrire la tâche en arrière-plan.

L’exemple de code suivant présente un point de départ élémentaire pour une classe de tâche en arrière-plan de mise à jour en C#. La classe de tâche en arrière-plan elle-même, ainsi que toutes les autres classes au sein du projet de tâche en arrière-plan, doivent être des classes **public** et **sealed**. Votre classe de tâche en arrière-plan doit dériver de **IBackgroundTask** et présenter une méthode **Run()** publique avec la signature présentée ci-dessous :

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Étape 2 : Déclarez votre tâche en arrière-plan dans le manifeste du package

Dans l’explorateur de solutions Visual Studio, cliquez avec le bouton droit sur **Package.appxmanifest** et cliquez sur **Afficher le code** pour afficher le manifeste du package. Ajoutez les `<Extensions>` XML suivantes pour déclarer votre tâche de mise à jour :

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

Dans le code XML ci-dessus, vérifiez que l’attribut `EntryPoint` est défini sur le nom namespace.class correspondant à la classe de votre tâche de mise à jour. Le nom est sensible à la casse.

## <a name="step-3-debugtest-your-update-task"></a>Étape 3 : Votre tâche de mise à jour de débogage et de test

Assurez-vous d’avoir déployé votre application sur votre machine pour qu’il existe un élément à mettre à jour.

Définissez un point d’arrêt dans la méthode Run() de votre tâche en arrière-plan.

![définition du point d’arrêt](images/run-func-breakpoint.png)

Puis, dans l’explorateur de solutions, cliquez avec le bouton droit sur le projet de votre application (pas le projet de tâche en arrière-plan) et sur **Propriétés**. Dans la fenêtre Propriétés de l’application, cliquez sur **Déboguer** sur la gauche, puis sélectionnez **Ne pas lancer, mais déboguer mon code au démarrage** :

![définition des paramètres de débogage](images/do-not-launch-but-debug.png)

Ensuite, pour vous assurer que la tâche UpdateTask est déclenchée, augmentez le numéro de version du package. Dans l’explorateur de solutions, double-cliquez sur le fichier **Package.appxmanifest** de votre application pour ouvrir le concepteur de packages, puis mettez à jour le numéro de **Build** :

![mise à jour de la version](images/bump-version.png)

Désormais, dans Visual Studio 2017 lorsque vous appuyez sur F5, votre application sera mise à jour et le système activera votre composant UpdateTask en arrière-plan. Le débogueur se connectera automatiquement au processus en arrière-plan. Votre point d’arrêt sera atteint et vous pourrez exécuter la logique de votre code de mise à jour.

Lorsque la tâche en arrière-plan est terminée, vous pouvez lancer l’application au premier plan depuis le menu Démarrer de Windows, dans la même session de débogage. Le débogueur se connecte à nouveau automatiquement, cette fois avec votre processus au premier plan, et vous pouvez exécuter la logique de votre application.

> [!NOTE]
> Utilisateurs de Visual Studio 2015 : Les étapes ci-dessus s’appliquent à Visual Studio 2017. Si vous utilisez Visual Studio 2015, vous pouvez utiliser les mêmes techniques pour déclencher et tester UpdateTask, sauf que Visual Studio ne s’y connectera pas. Sinon, VS 2015 vous permet de configurer un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) qui définit la tâche UpdateTask comme son point d’entrée et déclenche l’exécution directement depuis l’application au premier plan.

## <a name="see-also"></a>Voir aussi

[Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
