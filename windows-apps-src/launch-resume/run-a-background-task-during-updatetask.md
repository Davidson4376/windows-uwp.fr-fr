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
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2810884"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Exécuter une tâche en arrière-plan lorsque votre application UWP est mise à jour

Découvrez comment écrire une tâche d’arrière-plan qui s’exécute après la mise à jour de votre application de banque universels Windows plateforme (UWP).

La boîte de dialogue Mettre à jour une tâche de fond est appelée par le système d’exploitation après que l’utilisateur a installé une mise à jour pour une application qui est installée sur l’appareil. Cela permet à votre application effectuer des tâches d’initialisation telles que l’initialisation d’un nouveau canal de notification push, mise à jour du schéma de base de données et ainsi de suite avant que l’utilisateur lance l’application de mises à jour.

La tâche de mise à jour est différent de lancer une tâche de fond en utilisant le déclencheur [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) car dans ce cas votre application doit s’exécuter au moins une fois avant qu’il est mis à jour afin d’enregistrer la tâche d’arrière-plan qui sera activée par le ** ServicingComplete** déclencheur.  La tâche de mise à jour n’est pas enregistrée et par conséquent, une application qui n’a jamais été exécuté, mais qui est mis à niveau, aura toujours sa tâche de mise à jour déclenchée.

## <a name="step-1-create-the-background-task-class"></a>Étape 1: Créer la classe de tâche en arrière-plan

Comme avec d’autres types de tâches en arrière-plan, vous mettre en œuvre la tâche d’arrière-plan de mise à jour comme un composant Windows Runtime. Pour créer ce composant, suivez les étapes décrites dans la section **créer la classe de tâche de fond** de [créer et enregistrer une tâche d’arrière-plan hors du processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). Les étapes sont les suivantes:

- Ajout d’un projet de composant d’exécution Windows à votre solution.
- Création d’une référence au composant à partir de votre application.
- Création d’une classe sealed publique dans le composant qui implémente [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- L’implémentation de la méthode [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , qui est le point d’entrée requis qui est appelé lorsque la tâche de mise à jour est exécutée. Si vous souhaitez effectuer des appels asynchrones à partir de votre tâche en arrière-plan, [créer et enregistrer une tâche d’arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explique comment utiliser un report dans votre méthode **d’exécution** .

Vous n’avez pas besoin d’enregistrer cette tâche de fond (section «Inscrire pour exécuter la tâche d’arrière-plan» dans la rubrique **créer et enregistrer une tâche d’arrière-plan hors du processus** ) pour utiliser la tâche de mise à jour. Il s’agit principalement d’utiliser une tâche de mise à jour, car vous n’avez pas besoin d’ajouter du code à votre application pour enregistrer la tâche et l’application ne doit pas exécuter au moins une fois avant la mise à jour pour enregistrer la tâche en arrière-plan.

L’exemple de code suivant montre un point de départ de base pour une classe de tâche de tâche de mise à jour en arrière-plan en c#. La classe de tâche de fond lui-même - et toutes les autres classes dans le projet de la tâche en arrière-plan - doivent être **public** et **sealed**. Votre classe de la tâche en arrière-plan doit dériver de **IBackgroundTask** et ont une méthode **Run()** publique avec la signature ci-dessous:

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

Dans l’Explorateur de solutions Visual Studio, avec le bouton droit **Package.appxmanifest** et cliquez sur **Afficher le Code** pour afficher le manifeste du package. Ajoutez le code suivant `<Extensions>` XML pour déclarer votre tâche de mise à jour:

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

Dans le code XML ci-dessus, assurez-vous que le `EntryPoint` attribut est défini sur le nom de l’espace de noms.classe de votre classe de tâche de mise à jour. Le nom respecte la casse.

## <a name="step-3-debugtest-your-update-task"></a>Étape 3: Debug/test votre tâche de mise à jour

Vérifiez que vous avez déployé votre application sur votre ordinateur pour qu’il y a quelque chose à mettre à jour.

Définissez un point d’arrêt dans la méthode Run() de votre tâche en arrière-plan.

![point d’arrêt défini](images/run-func-breakpoint.png)

Ensuite, dans l’Explorateur de solutions, avec le bouton droit projet (pas l’arrière-plan tâches) de votre application, puis sur **Propriétés**. Dans la fenêtre Propriétés de l’application, cliquez sur **débogage** sur la gauche, puis sélectionnez **déboguer mon code lorsqu’il démarre, mais ne lancez pas**:

![définir les paramètres de débogage](images/do-not-launch-but-debug.png)

Ensuite, pour vous assurer que le UpdateTask est déclenché, augmentez le numéro de version du package. Dans l’Explorateur de solutions, double-cliquez sur le fichier de **Package.appxmanifest** de votre application pour ouvrir le Concepteur de package, puis mettre à jour le numéro de **Build** :

![mise à jour de la version](images/bump-version.png)

Maintenant, dans Visual Studio 2017 lorsque vous appuyez sur F5, votre application sera mis à jour et le système activera votre composant UpdateTask en arrière-plan. Le débogueur s’attache automatiquement pour le processus d’arrière-plan. Obtenir atteignez votre point d’arrêt et vous pouvez parcourir votre logique du code de mise à jour.

Une fois la tâche d’arrière-plan terminée, vous pouvez lancer l’application de premier plan dans le menu Démarrer de Windows dans la même session de débogage. Le débogueur s’attache automatiquement, cette fois, de votre processus de premier plan, et vous pouvez parcourir la logique de votre application.

> [!NOTE]
> Les utilisateurs de Visual Studio 2015: les étapes ci-dessus s’appliquent à Visual Studio 2017. Si vous utilisez Visual Studio 2015, vous pouvez utiliser les mêmes techniques pour déclencher et test du UpdateTask, à l’exception de Visual Studio n’est pas attachée à celui-ci. Procédure alternative dans VS 2015 consiste à configurer un [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) qui définit la UpdateTask en tant que Point d’entrée et déclenche l’exécution directement à partir de l’application de premier plan.

## <a name="see-also"></a>Articles associés

[Créer et inscrire une tâche en arrière-plan hors processus](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
