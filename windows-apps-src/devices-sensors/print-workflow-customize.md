---
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: Personnaliser le flux de travail d'impression
description: Créez des expériences de flux de travail d’impression personnalisées pour répondre aux besoins de votre organisation.
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, uwp, l’impression
ms.localizationpriority: medium
ms.openlocfilehash: 96e308793e60c0367c712fb93a5d25a056397568
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7966009"
---
# <a name="customize-the-print-workflow"></a>Personnaliser le flux de travail d'impression

## <a name="overview"></a>Vue d'ensemble
Les développeurs peuvent personnaliser l’expérience de flux de travail d’impression à l’aide d’une application de flux de travail d’impression. Les applications de flux de travail d’impression sont des applications UWP qui développent les fonctionnalités des [applications de périphériques MicrosoftStore (WSDA)](https://docs.microsoft.com/windows-hardware/drivers/devapps/). Il est donc utile d’être familiarisé avec les WSDA avant de poursuivre. 

Comme dans le cas des WSDA, lorsque l’utilisateur d’une application source choisit d'imprimer quelque chose et navigue dans la boîte de dialogue d’impression, le système vérifie si une application de flux de travail est associée à cette imprimante. Si c'est le cas, l’application de flux de travail d’impression se lance (principalement en tant que tâche en arrière-plan. Plus de détails sur ce point ci-dessous). Une application de flux de travail peut modifier à la fois le ticket d’impression (le document XML qui configure les paramètres du périphérique d’impression pour la tâche d’impression en cours) et le contenu XPS réel à imprimer. Elle peut éventuellement exposer cette fonctionnalité à l’utilisateur en lançant une interface utilisateur au milieu du processus. Après avoir effectué son travail, elle transmet le contenu d’impression et le ticket d’impression au pilote d’impression.

Dans la mesure où elle implique des composants en arrière-plan et au premier plan, et parce qu'elle est fonctionnellement couplée avec d’autres applications, une application de flux de travail d’impression peut être plus complexe à implémenter que les autres catégories d’applications UWP. Il est recommandé d'examiner l'[exemple d’application de flux de travail](https://github.com/Microsoft/print-oem-samples) lors de la lecture de ce guide pour mieux comprendre comment les différentes fonctionnalités peuvent être implémentées. Certaines fonctionnalités, telles que les différents contrôles d’erreur et la gestion de l’interface utilisateur, sont absentes de ce guide par souci de simplicité.

## <a name="getting-started"></a>Prise en main

L’application de flux de travail doit indiquer son point d’entrée au système d’impression afin qu’il puisse se lancer au moment approprié. Pour ce faire, vous devez insérer la déclaration suivante dans l'élément `Application/Extensions` du fichier *package.appxmanifest* du projet UWP. 

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT] 
> Il existe plusieurs scénarios dans lesquels la personnalisation d’impression ne nécessite pas d’entrée de l’utilisateur. Pour cette raison, les applications de flux de travail d’impression s’exécutent en tant que tâches en arrière-plan par défaut.

Si une application de flux de travail est associée à l’application source qui a démarré la tâche d’impression (voir la section ultérieure pour des instructions sur ce point), le système d’impression examine ses fichiers manifestes pour rechercher un point d’entrée de tâche en arrière-plan.

## <a name="do-background-work-on-the-print-ticket"></a>Exécuter une tâche en arrière-plan sur le ticket d’impression

La première chose que le système d’impression fait avec l’application de flux de travail est d’activer sa tâche en arrière-plan (dans ce cas, la classe `WfBackgroundTask` dans l'espace de noms `WFBackgroundTasks`). Dans la méthode `Run` de la tâche en arrière-plan, vous devez effectuer une conversion de type (transtypage) des détails du déclencheur de la tâche en tant qu’instance **[PrintWorkflowTriggerDetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)**. Cela fournit la fonctionnalité spéciale d'une tâche en arrière-plan de flux de travail d’impression. Cela expose la propriété **[PrintWorkflowSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails.PrintWorkflowSession)** qui est une instance de **[PrintWorkFlowBackgroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)**. Des classes de session de flux de travail d’impression (des variétés à la fois au premier plan et arrière-plan) contrôleront les étapes séquentielles de l’application de flux de travail d’impression. 

Inscrivez ensuite des méthodes de gestionnaire pour les deux événements que cette classe de session déclenche. Vous définirez ces méthodes ultérieurement.

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

Lorsque la méthode `Start` est appelée, le gestionnaire de session déclenche d'abord l'événement **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.SetupRequested)**. Cet événement expose des informations générales sur la tâche d’impression, ainsi que le ticket d’impression. À ce stade, le ticket d’impression peut être modifié en arrière-plan. 

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

Plus important encore, c'est dans la gestion de l'événement **SetupRequested** que l’application détermine si vous souhaitez lancer un composant au premier plan. Cela peut dépendre d’un paramètre qui a été précédemment enregistré sur le stockage local ou d'un événement qui s’est produit lors de la modification du ticket d’impression, ou encore, il peut s'agir d'un paramètre statique de votre application particulière.

```csharp
    // ...
    
    if (UIrequested) {
        printTaskSetupArgs.SetRequiresUI();

        // Any data that is to be passed to the foreground task must be stored the app's local storage.
        // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
        // it can be identified as pertaining to this workflow app session.
    }

    // Complete the deferral taken out at the start of OnSetupRequested
    setupRequestedDeferral.Complete();
}
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>Exécuter la tâche au premier plan sur le travail d’impression (facultatif)

Si la méthode **[SetRequiresUI](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs.SetRequiresUI)** a été appelée, le système d’impression examine le fichier manifeste pour rechercher le point d’entrée de l’application au premier plan. L’élément `Application/Extensions` de votre fichier *package.appxmanifest* doit avoir les lignes suivantes. Remplacez la valeur `EntryPoint` par le nom de l'application au premier plan.

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" /> 
```

Ensuite, le système d’impression appelle la méthode **OnActivated** pour le point d’entrée de l'application donnée. Dans la méthode **OnActivated** de son fichier _App.xaml.cs_, l’application de flux de travail doit vérifier le type d’activation pour vérifier qu’il s’agit de l’activation d’un flux de travail. Si c'est le cas, l’application de flux de travail peut effectuer une conversion de type (transtypage) des arguments d’activation pour un objet **[PrintWorkflowUIActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)**, qui expose un objet **[PrintWorkflowForegroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** en tant que propriété. Cet objet, comme son équivalent en arrière-plan dans la section précédente, contient les événements qui sont déclenchés par le système d’impression, et vous pouvez leur assigner des gestionnaires. Dans ce cas, la fonctionnalité de gestion des événements sera implémentée dans une classe séparée appelée `WorkflowPage`.

D'abord, ouvrez le fichier _App.xaml.cs_:

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when 
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

Une fois que l’interface utilisateur a joint des gestionnaires d’événements et que la méthode **OnActivated** s’est arrêtée, le système d’impression déclenche l'événement **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.SetupRequested)** à gérer par l’interface utilisateur. Cet événement fournit les mêmes données que l'événement fourni de configuration de la tâche en arrière-plan, notamment les informations sur le travail d’impression et le document de ticket d’impression, mais sans la possibilité de demander le lancement d'une interface utilisateur supplémentaire. Dans le fichier _WorkflowPage.xaml.cs_:

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);
    
    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

Ensuite, le système d’impression déclenche l'événement **[XpsDataAvailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession.XpsDataAvailable)** pour l’interface utilisateur. Dans le gestionnaire de cet événement, l’application de flux de travail peut accéder à toutes les données disponibles pour l’événement de configuration et peut en plus lire les données XPS directement, soit sous la forme d’un flux d’octets bruts ou sous forme d'un modèle d’objet. L'accès aux données XPS permet à l’interface utilisateur de fournir des services d’aperçu avant impression et des informations supplémentaires à l’utilisateur sur les opérations que l’application de flux de travail va exécuter sur les données. 

Dans le cadre de ce gestionnaire d’événements, l’application de flux de travail doit acquérir un objet de report si elle continue d’interagir avec l’utilisateur. Sans report, le système d’impression considère la tâche de l’interface utilisateur terminée quand le gestionnaire d’événements **XpsDataAvailable** se ferme ou lorsqu’il appelle une méthode asynchrone. Lorsque l’application a recueilli toutes les informations nécessaires à partir de l’interaction utilisateur avec l’interface utilisateur, elle doit terminer le report afin que le système d’impression puisse ensuite avancer.

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent(); 
 
    IInputStream inputStream = xpsStream.GetInputSpoolStream(); 
 
    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream)) 
    { 
        // Read the XPS data from input stream 
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength]; 
        while (inputReader.UnconsumedBufferLength > 0) 
        { 
            inputReader.ReadBytes(xpsData); 
            // Do something with the XPS data, e.g. preview 
            // ...                  
        } 
    } 

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

En outre, l'instance **[PrintWorkflowSubmittedOperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** exposée par les arguments d’événement offre la possibilité d’annuler le travail d’impression ou d'indiquer que la tâche a réussi, mais qu'aucune sortie de tâche d’impression n'est nécessaire. Pour ce faire, vous devez appeler la méthode **[Complete](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation.Complete)** avec une valeur **[PrintWorkflowSubmittedStatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)**.

> [!NOTE]
> Si l’application de flux de travail annule le travail d’impression, il est vivement recommandé qu'elle fournisse une notification toast indiquant pourquoi la tâche a été annulée. 


## <a name="do-final-background-work-on-the-print-content"></a>Exécuter une tâche en arrière-plan finale sur le contenu d’impression

Une fois que l’interface utilisateur a terminé le report dans l'événement **PrintTaskXpsDataAvailable** (ou si l’étape de l’interface utilisateur a été ignorée), le système d’impression déclenche l'événement **[Submitted](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession.Submitted)** pour la tâche en arrière-plan. Dans le gestionnaire de cet événement, l’application de flux de travail peut accéder aux mêmes données que celles fournies par l'événement **XpsDataAvailable**. Cependant, contrairement à tous les événements précédents, **Submitted** fournit également un accès *en écriture* au contenu de la tâche d’impression finale par le biais d’une instance **[PrintWorkflowTarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)**. 

L’objet utilisé pour mettre en attente les données d'impression finale dépend de si la source de données est accessible sous forme d’un flux d’octets bruts ou sous forme d'un modèle d’objet XPS. Lorsque l’application de flux de travail accède à la source de données par le biais d’un flux d’octets, un flux d’octets de sortie est fourni pour y écrire les données de la tâche finale. Lorsque l’application de flux de travail accède à la source de données par le biais d’un modèle d'objet, un Document Writer est fourni pour écrire des objets dans la tâche de sortie. Dans les deux cas, l’application de flux de travail doit lire toutes les sources de données, modifier les données requises et écrire les données modifiées dans la cible de sortie.

Lorsque la tâche en arrière-plan a terminé d’écrire les données, elle doit appeler **Complete** sur l'objet **PrintWorkflowSubmittedOperation** correspondant. Une fois que l’application de flux de travail a terminé cette étape et que le gestionnaire d’événements **Submitted** se ferme, la session de flux de travail est fermée et l’utilisateur peut surveiller l’état de la tâche d’impression finale via les boîtes de dialogue d’impression standard.

## <a name="final-steps"></a>Étapes finales

### <a name="register-the-print-workflow-app-to-the-printer"></a>Inscrire l’application de flux de travail d’impression sur l’imprimante

Votre application de flux de travail est associé à une imprimante à l’aide du même type de soumission de fichier de métadonnées que pour les WSDA. En fait, une seule application UWP peut servir à la fois d'application de flux de travail et de WSDA qui fournit des fonctionnalités de paramètres de tâche d’impression. Suivez les étapes du [WSDA correspondant pour créer l’association de métadonnées](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata).

La différence est qu'alors que les WSDA sont automatiquement activés pour l’utilisateur (l’application s’ouvre toujours lorsque cet utilisateur imprime sur le périphérique associé), les applications de flux de travail, elles, ne le sont pas. Elles ont une stratégie distincte qui doit être définie.

### <a name="set-the-workflow-apps-policy"></a>Définir la stratégie de l’application de flux de travail
La stratégie d’application de flux de travail est définie par des commandes Powershell sur l’appareil qui doit exécuter l’application de flux de travail. Les commandes Set-Printer, Add-Printer (port existant) et Add-Printer (nouveau port WSD) seront modifiées pour pouvoir définir les règles du flux de travail. 
* `Disabled`: les applications de flux de travail ne seront pas activées.
* `Uninitialized`: les applications de flux de travail seront activées si le flux de travail DCA est installé dans le système. Si l’application n’est pas installée, l’impression continue. 
* `Enabled`: le contrat de flux de travail sera activé si le flux de travail DCA est installé dans le système. Si l’application n’est pas installée, l’impression échoue. 

La commande suivante rend l’application de flux de travail nécessaire sur l’imprimante spécifiée.
```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy On
```

Un utilisateur local peut exécuter cette stratégie sur une imprimante locale, ou dans le cas d'une implémentation en entreprise, l’administrateur de l’imprimante peut exécuter cette stratégie sur le serveur d’impression. La stratégie sera alors synchronisée sur toutes les connexions clientes. L’administrateur de l’imprimante peut utiliser cette stratégie chaque fois qu’une nouvelle imprimante est ajoutée.

## <a name="see-also"></a>Articles associés

[Exemple d’application de flux de travail](https://github.com/Microsoft/print-oem-samples)

[Espace de noms Windows.Graphics.Printing.Workflow](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)


