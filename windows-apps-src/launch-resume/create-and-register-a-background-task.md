---
author: TylerMSFT
title: "Créer et inscrire une tâche en arrière-plan hors processus"
description: "Créez une classe de tâche en arrière-plan hors processus et inscrivez-la pour permettre son exécution lorsque votre application ne se trouve pas au premier plan."
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ba9c5c1a59452295a07efd371ccfd632f290c837
ms.lasthandoff: 02/07/2017

---

# <a name="create-and-register-an-out-of-process-background-task"></a>Créer et inscrire une tâche en arrière-plan hors processus

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Créez une classe de tâche en arrière-plan et inscrivez-la pour permettre son exécution lorsque votre application ne se trouve pas au premier plan. Cette rubrique explique comment créer et inscrire une tâche en arrière-plan qui s’exécute dans un processus distinct du processus de votre application. Pour exécuter une tâche en arrière-plan directement dans l’application au premier plan, consultez [Créer et inscrire une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).

> [!Note]
> Si vous utilisez une tâche en arrière-plan pour lire du contenu multimédia en arrière-plan, consultez [Lire du contenu multimédia en arrière-plan](https://msdn.microsoft.com/en-us/windows/uwp/audio-video-camera/background-audio). Les informations fournies sur les améliorations apportées dans Windows 10 version 1607 vous simplifieront la vie.

## <a name="create-the-background-task-class"></a>Créer la classe de tâche en arrière-plan

Vous pouvez exécuter du code en arrière-plan en écrivant des classes qui implémentent l’interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Ce code s’exécute lorsqu’un événement spécifique est déclenché à l’aide, par exemple, de [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) ou de [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517).

Les étapes suivantes vous montrent comment écrire une nouvelle classe qui implémente l’interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Avant de commencer, créez un projet dans votre solution pour les tâches en arrière-plan. Ajoutez une nouvelle classe vide pour votre tâche en arrière-plan et importez l’espace de noms [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847).

1.  Créez un projet pour les tâches en arrière-plan et ajoutez-le à votre solution. Pour ce faire, cliquez avec le bouton droit sur le nœud de votre solution dans l’**Explorateur de solutions**, puis sélectionnez Ajouter-&gt;Nouveau projet. Sélectionnez ensuite le type de projet **Composant Windows Runtime (Windows universel)**, nommez le projet, puis cliquez sur OK.
2.  Référencez le projet des tâches en arrière-plan à partir de votre projet d’application de plateforme Windows universelle (UWP).

    Pour une application C++, cliquez avec le bouton droit sur votre projet d’application et sélectionnez **Propriétés**. Accédez ensuite à **Propriétés communes** et cliquez sur **Ajouter une nouvelle référence**, cochez la case en regard de votre projet de tâches en arrière-plan, et cliquez sur **OK** dans les deux boîtes de dialogue.

    Pour une application C#, dans votre projet d’application, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une nouvelle référence**. Sous **Solution**, sélectionnez **Projets** et le nom de votre projet de tâches en arrière-plan, puis cliquez sur **OK**.

3.  Créez une classe qui implémente l’interface [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). La méthode [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) est un point d’entrée obligatoire qui est appelé quand l’événement spécifié est déclenché ; cette méthode est nécessaire dans chaque tâche en arrière-plan.

    > [!NOTE]
    > La classe de tâche en arrière-plan elle-même, ainsi que toutes les autres classes au sein du projet de tâche en arrière-plan, doivent être des classes **public** qui sont **sealed**.

    L’exemple de code suivant présente un point de départ très élémentaire pour une classe de tâche en arrière-plan :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     //
    >     // ExampleBackgroundTask.cs
    >     //
    >
    >     using Windows.ApplicationModel.Background;
    >
    >     namespace Tasks
    >     {
    >         public sealed class ExampleBackgroundTask : IBackgroundTask
    >         {
    >             public void Run(IBackgroundTaskInstance taskInstance)
    >             {
    >                 
    >             }        
    >         }
    >     }
    > ```
    > ```cpp
    >     //
    >     // ExampleBackgroundTask.h
    >     //
    >
    >     #pragma once
    >
    >     using namespace Windows::ApplicationModel::Background;
    >
    >     namespace RuntimeComponent1
    >     {
    >         public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    >         {
    >
    >         public:
    >             ExampleBackgroundTask();
    >
    >             virtual void Run(IBackgroundTaskInstance^ taskInstance);
    >             void OnCompleted(
    >                     BackgroundTaskRegistration^ task,
    >                     BackgroundTaskCompletedEventArgs^ args
    >                     );
    >         };
    >     }
    >
    >     //
    >     // ExampleBackgroundTask.cpp
    >     //
    >
    >     #include "ExampleBackgroundTask.h"
    >
    >     using namespace Tasks;
    >
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >
    >     }
    >  ```

4.  Si vous exécutez du code asynchrone dans votre tâche en arrière-plan, celle-ci doit alors utiliser un report, sans quoi le processus de la tâche en arrière-plan risque de prendre fin de manière inattendue si la méthode Run se termine avant la fin de votre appel de méthode asynchrone.

    Demandez le report dans la méthode Run avant d’appeler la méthode asynchrone. Enregistrez le report dans une variable globale pour que la méthode asynchrone puisse y accéder. Déclarez le report terminé après que l’exécution du code asynchrone a abouti.

    L’exemple de code suivant obtient le report, l’enregistre et le libère à l’issue de l’exécution du code asynchrone :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskDeferral _deferral; // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     public async void Run(IBackgroundTaskInstance taskInstance)
    >     {
    >         _deferral = taskInstance.GetDeferral()
    >         //
    >         // TODO: Insert code to start one or more asynchronous methods using the
    >         //       await keyword, for example:
    >         //
    >         // await ExampleMethodAsync();
    >         //
    >         
    >         _deferral.Complete();
    >     }
    > ```
    > ```cpp
    >     BackgroundTaskDeferral^ deferral = taskInstance->GetDeferral(); // Note: defined at class scope so we can mark it complete inside the OnCancel() callback if we choose to support cancellation
    >     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
    >     {
    >         //
    >         // TODO: Modify the following line of code to call a real async function.
    >         //       Note that the task<void> return type applies only to async
    >         //       actions. If you need to call an async operation instead, replace
    >         //       task<void> with the correct return type.
    >         //
    >         task<void> myTask(ExampleFunctionAsync());
    >         
    >         myTask.then([=] () {
    >             deferral->Complete();
    >         });
    >     }
    > ```

> [!NOTE]
> En C#, les méthodes asynchrones de vos tâches en arrière-plan peuvent être appelées à l’aide des mots clés **async/await**. En C++, un résultat similaire peut être obtenu à l’aide d’une chaîne de tâches.

Pour plus d’informations sur les modèles asynchrones, voir [Programmation asynchrone](https://msdn.microsoft.com/library/windows/apps/mt187335). Pour obtenir des exemples supplémentaires sur l’utilisation de reports en vue d’empêcher l’arrêt prématuré d’une tâche en arrière-plan, voir l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666).

Les étapes qui suivent sont à effectuer dans l’une de vos classes d’application (par exemple, MainPage.xaml.cs).

> [!NOTE]
> Vous pouvez aussi créer une fonction consacrée à l’inscription des tâches en arrière-plan (voir [Inscrire une tâche en arrière-plan](register-a-background-task.md)). Dans ce cas, plutôt que de suivre les trois étapes suivantes, vous pouvez simplement construire le déclencheur et le communiquer à la fonction d’inscription avec le nom de la tâche, son point d’entrée et une condition (facultatif).

## <a name="register-the-background-task-to-run"></a>Inscrire la tâche en arrière-plan à des fins d’exécution

1.  Découvrez si la tâche en arrière-plan est déjà inscrite en parcourant la propriété [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787). Cette étape est primordiale ; si votre application ne vérifie pas la présence d’inscriptions de tâches en arrière-plan existantes, elle peut aisément procéder plusieurs fois à l’inscription de la tâche, ce qui risque de poser des problèmes de performance et d’épuiser le temps processeur disponible pour la tâche avant que le travail ne soit effectué.

    L’exemple qui suit parcourt la propriété AllTasks et définit une variable d’indicateur sur True si la tâche est déjà inscrite :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var taskRegistered = false;
    >     var exampleTaskName = "ExampleBackgroundTask";
    >
    >     foreach (var task in BackgroundTaskRegistration.AllTasks)
    >     {
    >         if (task.Value.Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >     }
    > ```
    > ```cpp
    >     boolean taskRegistered = false;
    >     Platform::String^ exampleTaskName = "ExampleBackgroundTask";
    >
    >     auto iter = BackgroundTaskRegistration::AllTasks->First();
    >     auto hascur = iter->HasCurrent;
    >
    >     while (hascur)
    >     {
    >         auto cur = iter->Current->Value;
    >
    >         if(cur->Name == exampleTaskName)
    >         {
    >             taskRegistered = true;
    >             break;
    >         }
    >
    >         hascur = iter->MoveNext();
    >     }
    > ```

2.  Si la tâche en arrière-plan n’est pas déjà inscrite, utilisez [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) pour créer une instance de votre tâche en arrière-plan. Le point d’entrée de la tâche doit correspondre au nom de votre classe de tâche en arrière-plan précédé de l’espace de noms.

    Le déclencheur de tâche en arrière-plan contrôle à quel moment la tâche en arrière-plan. Pour obtenir la liste des déclencheurs possibles, voir [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

    Par exemple, ce code crée une tâche en arrière-plan et la configure pour l’exécuter au moment où le déclencheur **TimeZoneChanged** est activé :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     var builder = new BackgroundTaskBuilder();
    >
    >     builder.Name = exampleTaskName;
    >     builder.TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
    > ```
    > ```cpp
    >     auto builder = ref new BackgroundTaskBuilder();
    >
    >     builder->Name = exampleTaskName;
    >     builder->TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
    >     builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
    > ```

3.  Vous pouvez ajouter une condition afin de contrôler à quel moment votre tâche sera exécutée après que l’événement de déclencheur est survenu (facultatif). Par exemple, si vous ne souhaitez pas que la tâche s’exécute tant que l’utilisateur n’est pas présent, appliquez la condition **UserPresent**. Pour obtenir la liste des conditions possibles, voir [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    L’exemple de code suivant affecte une condition qui exige la présence de l’utilisateur :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
    > ```
    > ```cpp
    >     builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
    > ```

4.  Inscrivez la tâche en arrière-plan en appelant la méthode Register sur l’objet [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Stockez le résultat [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) pour pouvoir l’utiliser à l’étape suivante.

    Le code qui suit inscrit la tâche en arrière-plan et stocke le résultat :

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     BackgroundTaskRegistration task = builder.Register();
    >     ```
    > ```cpp
    >     BackgroundTaskRegistration^ task = builder->Register();
    > ```

> [!NOTE]
> Les applications Windows universelles doivent appeler l’élément [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) avant d’inscrire n’importe quel type de déclencheur en arrière-plan.

Pour vous assurer que votre application Windows universelle continue de s’exécuter correctement après la publication d’une mise à jour, utilisez le déclencheur **ServicingComplete** (voir [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) pour effectuer les modifications de configuration requises après la mise à jour, telles que la migration de la base de données de l’application et l’inscription des tâches en arrière-plan. Pour l’instant, il est recommandé d’annuler l’inscription des tâches en arrière-plan associées à la version précédente de l’application (voir [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)) et d’inscrire des tâches en arrière-plan pour la nouvelle version de l’application (voir [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)).

Pour plus d’informations, voir [Recommandations en matière de tâches en arrière-plan](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Gérer l’achèvement des tâches en arrière-plan à l’aide de gestionnaires d’événements

Vous devez inscrire une méthode avec le [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) afin que votre application puisse obtenir les résultats de la tâche en arrière-plan. Lors du lancement ou de la reprise de l’application, la méthode mark est appelée si la tâche en arrière-plan s’est terminée depuis la dernière fois où l’application s’est trouvée au premier plan. (La méthode OnCompleted est appelée immédiatement si la tâche en arrière-plan se termine pendant que votre application est au premier plan.)

1.  Écrivez une méthode OnCompleted pour gérer l’achèvement des tâches en arrière-plan. Par exemple, le résultat des tâches en arrière-plan peut entraîner une mise à jour de l’interface utilisateur. L’empreinte de la méthode présentée ici est requise pour la méthode de gestionnaire d’événements OnCompleted, même si cet exemple n’utilise pas le paramètre *args*.

    L’exemple de code suivant reconnaît l’achèvement des tâches en arrière-plan et appelle un exemple de méthode de mise à jour de l’interface utilisateur qui prend une chaîne de message.

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    >         var key = task.TaskId.ToString();
    >         var message = settings.Values[key].ToString();
    >         UpdateUI(message);
    >     }
    > ```
    > ```cpp
    >     void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {
    >         auto settings = ApplicationData::Current->LocalSettings->Values;
    >         auto key = task->TaskId.ToString();
    >         auto message = dynamic_cast<String^>(settings->Lookup(key));
    >         UpdateUI(message);
    >     }
    > ```

    > [!NOTE]
    > Les mises à jour de l’interface utilisateur doivent être effectuées de manière asynchrone pour éviter de retarder le thread d’interface utilisateur. Pour obtenir un exemple, voir la méthode UpdateUI dans l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666).


2.  Revenez à l’endroit où vous avez inscrit la tâche en arrière-plan. Après cette ligne de code, ajoutez un nouvel objet [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781). Fournissez votre méthode OnCompleted comme paramètre du constructeur **BackgroundTaskCompletedEventHandler**.

    L’exemple de code suivant ajoute un objet [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) à [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) :

     > [!div class="tabbedCodeSnippets"]
    > ```cs
    >     task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    > ```
    > ```cpp
    >     task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
    > ```

## <a name="declare-that-your-app-uses-background-tasks-in-the-app-manifest"></a>Déclarer que votre application utilise des tâches en arrière-plan dans le manifeste de l’application

Pour que votre application puisse exécuter des tâches en arrière-plan, vous devez déclarer chaque tâche en arrière-plan dans le manifeste de l’application. Si votre application tente d’inscrire une tâche en arrière-plan avec un déclencheur qui n’est pas répertorié dans le manifeste, l’inscription échoue.

1.  Ouvrez le concepteur de manifeste du package en accédant au fichier nommé Package.appxmanifest.
2.  Ouvrez l’onglet **Déclarations**.
3.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Tâches en arrière-plan**, puis cliquez sur **Ajouter**.
4.  Cochez la case **Événement système**.
5.  Dans la zone de texte **Point d’entrée**, entrez l’espace de noms et le nom de votre classe en arrière-plan qui, pour cet exemple, est RuntimeComponent1.ExampleBackgroundTask.
6.  Fermez le concepteur de manifeste.

    L’élément Extensions suivant est ajouté à votre fichier Package.appxmanifest pour inscrire la tâche en arrière-plan :

    ```xml
    <Extensions>
      <Extension Category="windows.backgroundTasks" EntryPoint="RuntimeComponent1.ExampleBackgroundTask">
        <BackgroundTasks>
          <Task Type="systemEvent" />
        </BackgroundTasks>
      </Extension>
    </Extensions>
    ```

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Vous devez à présent être en mesure d’écrire une classe de tâche en arrière-plan, d’inscrire la tâche en arrière-plan dans votre application et de permettre à votre application de reconnaître à quel moment la tâche en arrière-plan est achevée. Vous devez également savoir comment mettre à jour le manifeste de l’application de sorte que votre application peut inscrire correctement la tâche en arrière-plan.

> [!NOTE]
> Téléchargez l’[exemple de tâche en arrière-plan](http://go.microsoft.com/fwlink/p/?LinkId=618666) pour examiner des exemples de code similaires dans le contexte d’une application UWP aboutie et robuste ayant recours à des tâches en arrière-plan.

Consultez les rubriques connexes suivantes pour obtenir des informations de référence sur les API, des recommandations conceptuelles pour les tâches en arrière-plan, ainsi que des instructions plus détaillées pour écrire des applications qui utilisent des tâches en arrière-plan.

> [!NOTE]
> Cet article s’adresse aux développeurs de Windows 10 qui écrivent des applications de plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

## <a name="related-topics"></a>Rubriques connexes

**Rubriques d’instructions détaillées sur les tâches en arrière-plan**

* [Répondre aux événements système avec des tâches en arrière-plan](respond-to-system-events-with-background-tasks.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Définir des conditions pour exécuter une tâche en arrière-plan](set-conditions-for-running-a-background-task.md)
* [Utiliser un déclencheur de maintenance](use-a-maintenance-trigger.md)
* [Gérer une tâche en arrière-plan annulée](handle-a-cancelled-background-task.md)
* [Surveiller la progression et l’achèvement des tâches en arrière-plan](monitor-background-task-progress-and-completion.md)
* [Exécuter une tâche en arrière-plan en fonction d’un minuteur](run-a-background-task-on-a-timer-.md)
* [Créez et inscrivez une tâche en arrière-plan in-process](create-and-register-an-inproc-background-task.md).
* [Convertir une tâche en arrière-plan hors processus en une tâche en arrière-plan in-process](convert-out-of-process-background-task.md)  

**Recommandations en matière de tâches en arrière-plan**

* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Déboguer une tâche en arrière-plan](debug-a-background-task.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store (lors du débogage)](http://go.microsoft.com/fwlink/p/?linkid=254345)

**Informations de référence d’API de tâche en arrière-plan**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)

