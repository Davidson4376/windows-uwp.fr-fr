---
author: drewbatgit
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: Cet article vous montre comment utiliser MediaProcessingTrigger et une tâche en arrière-plan pour traiter des fichiers multimédias en arrière-plan.
title: Traiter des fichiers multimédias en arrière-plan
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 866fedf35aa6f1f585825195b18cdd1fed4bad11
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6193280"
---
# <a name="process-media-files-in-the-background"></a>Traiter des fichiers multimédias en arrière-plan



Cet article vous montre comment utiliser [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) et une tâche en arrière-plan pour traiter des fichiers multimédias en arrière-plan.

L’exemple d’application décrit dans cet article permet à l’utilisateur de sélectionner un fichier multimédia d’entrée à transcoder et de spécifier un fichier de sortie pour le résultat de transcodage. Ensuite, une tâche en arrière-plan est lancée pour effectuer l’opération de transcodage. Outre le transcodage, [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) est destiné à prendre en charge différents scénarios de traitement multimédia, notamment le rendu de compositions multimédias sur disque et le chargement de fichiers multimédias traités une fois le traitement terminé.

Pour obtenir des informations plus détaillées sur les différentes fonctionnalités des applications Windows universelles utilisées dans cet exemple, voir :

-   [Transcoder des fichiers multimédias](transcode-media-files.md)
-   [Lancement de tâches de reprise et en arrière-plan](https://msdn.microsoft.com/library/windows/apps/mt227652)
-   [Vignettes, badges et notifications](https://msdn.microsoft.com/library/windows/apps/mt185606)

## <a name="create-a-media-processing-background-task"></a>Créer une tâche de traitement multimédia en arrière-plan

Pour ajouter une tâche en arrière-plan à votre solution existante dans Microsoft Visual Studio, entrez un nom pour votre composition.

1.  Dans le menu **Fichier**, sélectionnez **Ajouter**, puis **Nouveau projet…**.
2.  Sélectionnez le type de projet **Composant Windows Runtime (Universel Windows)**.
3.  Entrez un nom pour votre nouveau projet de composant. Cet exemple utilise le nom de projet **MediaProcessingBackgroundTask**.
4.  Cliquez sur OK.

Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur l’icône du fichier « Class1.cs » qui est créé par défaut et sélectionnez **Renommer**. Renommez le fichier «MediaProcessingTask.cs». Lorsque Visual Studio vous demande si vous souhaitez renommer toutes les références à cette classe, cliquez sur **Oui**.

Dans le fichier de classe renommé, ajoutez les directives **using** suivantes pour inclure ces espaces de noms dans votre projet.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

Mettez à jour votre déclaration de classe pour faire en sorte que votre classe hérite de [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

Ajoutez les variables membres suivantes à votre classe:

-   Un objet [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797) qui sera utilisé pour mettre à jour l’application au premier plan avec la progression de la tâche en arrière-plan.
-   Un objet [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499) qui empêche le système d’arrêter votre tâche en arrière-plan pendant que le transcodage multimédia est exécuté en mode asynchrone.
-   Un objet **CancellationTokenSource** qui peut être utilisé pour annuler l’opération de transcodage asynchrone.
-   L’objet [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) qui sera utilisé pour transcoder des fichiers multimédias.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

Le système appelle la méthode [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) d’une tâche en arrière-plan lorsque la tâche est lancée. Définissez l’objet [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) transmis dans la méthode sur la variable membre correspondante. Enregistrez un gestionnaire de l’événement [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798), qui sera déclenché si le système a besoin d’arrêter la tâche en arrière-plan. Ensuite, affectez à la propriété [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) la valeur zéro.

Appelez ensuite la méthode [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700507) de l’objet de tâche en arrière-plan pour obtenir un report. Cela indique au système de ne pas arrêter votre tâche car vous exécutez des opérations asynchrones.

Ensuite, appelez la méthode d’assistance **TranscodeFileAsync** qui est définie dans la section suivante. Si l’opération se termine correctement, une méthode d’assistance est appelée pour lancer une notification toast afin d’avertir l’utilisateur que le transcodage est terminé.

À la fin de la méthode **Run**, appelez [**Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504) sur l’objet de report pour indiquer au système que votre tâche en arrière-plan est terminée et peut être fermée.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

Dans la méthode d’assistance **TranscodeFileAsync**, les noms de fichiers des fichiers d’entrée et de sortie pour les opérations de transcodage sont récupérés à partir de l’objet [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) de votre application. Ces valeurs seront définies par votre application au premier plan. Créez un objet [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) pour les fichiers d’entrée et de sortie, puis créez un profil d’encodage à utiliser pour le transcodage.

Appelez [**PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) en passant dans le fichier d’entrée, le fichier de sortie et le profil d’encodage. L’objet [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) renvoyé à partir de cet appel vous permet de savoir si le transcodage peut être effectué. Si la propriété [**CanTranscode**](https://msdn.microsoft.com/library/windows/apps/hh700942) est true, appelez [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) pour effectuer l’opération de transcodage.

La méthode **AsTask** vous permet de suivre la progression de l’opération asynchrone ou de l’annuler. Créez un objet **Progress** en spécifiant les unités de progression souhaitées et le nom de la méthode qui sera appelée pour vous informer de la progression actuelle de la tâche. Passez l’objet **Progress** dans la méthode **AsTask** avec le jeton d’annulation qui vous permet d’annuler la tâche.

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

Dans la méthode que vous avez utilisée pour créer l’objet de progression à l’étape précédente, **Progress**, définissez la progression de l’instance de tâche en arrière-plan. Cela transmettra la progression à l’application au premier plan, si celle-ci est en cours d’exécution.

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

La méthode d’assistance **SendToastNotification** crée une notification toast en obtenant un modèle de document XML pour un toast uniquement constitué de texte. L’élément de texte du XML de toast est défini, puis un objet [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) est créé à partir du document XML. Enfin, le toast est affiché à l’utilisateur en appelant [**ToastNotifier.Show**](https://msdn.microsoft.com/library/windows/apps/br208659).

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

Dans le gestionnaire de l’événement [**Canceled**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.IBackgroundTaskInstance.Canceled), qui est appelé lorsque le système annule la tâche en arrière-plan, vous pouvez consigner l’erreur à des fins de télémétrie.

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>Enregistrer et lancer la tâche en arrière-plan

Avant de pouvoir lancer la tâche en arrière-plan à partir de votre application au premier plan, vous devez mettre à jour le fichier Package.appmanifest de votre application au premier plan pour indiquer au système que votre application utilise une tâche en arrière-plan.

1.  Dans l’**Explorateur de solutions**, double-cliquez sur l’icône du fichier Package.appmanifest pour ouvrir l’éditeur du manifeste.
2.  Sélectionnez l’onglet **Déclarations**.
3.  Dans **Déclarations disponibles**, sélectionnez **Tâches en arrière-plan**, puis cliquez sur **Ajouter**.
4.  Sous **Déclarations prises en charge**, vérifiez que l’élément **Tâches en arrière-plan** est sélectionné. Sous **Propriétés**, cochez la case **Traitement multimédia**.
5.  Dans la zone de texte **Point d’entrée**, spécifiez l’espace de noms et le nom de classe de votre test en arrière-plan, séparés par un point. Pour cet exemple, l’entrée est:
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
Ensuite, vous devez ajouter une référence à votre tâche en arrière-plan à votre application au premier plan.
1.  Dans l’**Explorateur de solutions**, sous votre projet d’application au premier plan, cliquez avec le bouton droit sur le dossier **Références** et sélectionnez **Ajouter une référence…**.
2.  Développez le nœud **Projets** et sélectionnez **Solution**.
3.  Cochez la case en regard de votre projet de tâche en arrière-plan, puis cliquez sur **OK**.

Le reste du code dans cet exemple doit être ajouté à votre application au premier plan. Tout d’abord, vous devez ajouter les espaces de noms suivants à votre projet.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

Ensuite, ajoutez les variables membres suivantes qui sont nécessaires pour enregistrer la tâche en arrière-plan.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

La méthode d’assistance **PickFilesToTranscode** utilise des objets [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) et [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) pour ouvrir les fichiers d’entrée et de sortie pour le transcodage. L’utilisateur peut sélectionner des fichiers situés à un emplacement auquel votre application n’a pas accès. Pour vous assurer que votre tâche en arrière-plan peut ouvrir les fichiers, ajoutez-les à l’objet [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de votre application.

Enfin, définissez les entrées pour les noms de fichiers d’entrée et de sortie dans l’objet [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) de votre application. La tâche en arrière-plan récupère les noms de fichiers à partir de cet emplacement.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

Pour enregistrer la tâche en arrière-plan, créez des objets [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) et [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Définissez le nom du générateur de tâches en arrière-plan afin de pouvoir l’identifier ultérieurement. Définissez [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) sur l’espace de noms et la chaîne de noms de classe utilisés dans le fichier manifeste. Affectez la propriété [**Trigger**](https://msdn.microsoft.com/library/windows/apps/dn641725) à l’instance **MediaProcessingTrigger**.

Avant d’enregistrer la tâche, assurez-vous que vous annulez l’enregistrement de toutes les tâches précédemment enregistrées en parcourant la collection [**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) et en appelant [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229870) sur toutes les tâches portant le nom que vous avez spécifié dans la propriété [**BackgroundTaskBuilder.Name**](https://msdn.microsoft.com/library/windows/apps/br224771).

Enregistrez la tâche en arrière-plan en appelant [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772). Enregistrez des gestionnaires des événements [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) et [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224808).

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

Une application standard s’inscrire leur tâche en arrière-plan lorsque l’application est lancée initialement, comme dans l’événement **OnNavigatedTo** .

Lancez la tâche en arrière-plan en appelant la méthode [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn765071) de l’objet **MediaProcessingTrigger**. L’objet [**MediaProcessingTriggerResult**](https://msdn.microsoft.com/library/windows/apps/dn806007) renvoyé par cette méthode vous permet de savoir si la tâche en arrière-plan a été démarrée. Si ce n’est pas le cas, il vous permet de savoir pourquoi la tâche en arrière-plan n’a pas été lancée. 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

Une application standard lancera la tâche en arrière-plan en réponse à une interaction utilisateur, comme dans l’événement **Click** d’un contrôle de l’interface utilisateur.

Le gestionnaire d’événements **OnProgress** est appelé lorsque la tâche en arrière-plan met à jour la progression de l’opération. Vous pouvez utiliser cette opportunité pour mettre à jour votre interface utilisateur avec les informations de progression.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

Le gestionnaire d’événements **OnCompleted** est appelé lorsque l’exécution de la tâche en arrière-plan est terminée. Il s’agit d’une autre opportunité de mettre à jour votre interface utilisateur pour fournir des informations d’état à l’utilisateur.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




