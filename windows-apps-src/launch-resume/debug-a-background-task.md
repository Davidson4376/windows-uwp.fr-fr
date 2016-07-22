---
author: TylerMSFT
title: "Déboguer une tâche en arrière-plan"
description: "Découvrez comment déboguer une tâche en arrière-plan, notamment dans le cadre de son activation et du suivi de débogage dans le journal des événements Windows."
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 1b49c3f06456eef2f3e56227ecb5af5fc991fb13

---

# Déboguer une tâche en arrière-plan


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847)

Découvrez comment déboguer une tâche en arrière-plan, notamment dans le cadre de son activation et du suivi de débogage dans le journal des événements Windows.

Cette rubrique part du principe que vous disposez d’une application existante avec une tâche en arrière-plan à déboguer.

## Vérifier que le projet de tâche en arrière-plan est configuré comme il se doit


-   En C# etC++, vérifiez que le projet principal fait référence au projet de tâche en arrière-plan. En l’absence de cette référence, la tâche en arrière-plan n’est pas incluse dans le package d’application.
-   En C# et C++, vérifiez que le **Output type** du projet de tâches en arrière-plan est «Composant Windows Runtime».
-   La classe en arrière-plan doit être déclarée dans l’attribut de point d’entrée dans le manifeste du package.

## Déclencher des tâches en arrière-plan manuellement pour déboguer le code de la tâche en arrière-plan


Les tâches en arrière-plan peuvent être déclenchées manuellement par le biais de Microsoft Visual Studio. Vous pouvez ensuite exécuter pas à pas le code et le déboguer.

1.  En C#, insérez un point d’arrêt dans la méthode Run de la classe en arrière-plan et/ou écrivez la sortie de débogage à l’aide de l’espace de noms [**System.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441592.aspx).

    En C++, insérez un point d’arrêt dans la fonction Run de la classe en arrière-plan, et/ou écrivez la sortie de débogage au moyen de la fonction [**OutputDebugString**](https://msdn.microsoft.com/library/windows/desktop/aa363362).

2.  Exécutez votre application dans le débogueur, puis déclenchez la tâche en arrière-plan via le menu déroulant Suspendre dans la barre d’outils **Événements de cycle de vie**. Ce menu déroulant affiche les noms des tâches en arrière-plan qui peuvent être activées par Visual Studio.

    Pour que cette opération fonctionne, la tâche en arrière-plan, doit déjà être inscrite et doit toujours attendre le déclencheur. Par exemple, si une tâche en arrière-plan a été inscrite avec un TimeTrigger à déclenchement unique et que ce déclencheur a déjà été déclenché, le lancement de la tâche via Visual Studio n’aura aucun effet.

    **Remarque** Les tâches en arrière-plan qui utilisent l’objet [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) ou [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) et celles qui utilisent un [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) avec le type de déclencheur [**SmsReceived**](https://msdn.microsoft.com/library/windows/apps/br224839) ne peuvent pas être activées de cette manière.     

    ![débogage des tâches en arrière-plan](images/debugging-activation.png)

3.  Lorsque la tâche en arrière-plan est activée, le débogueur s’y associe et affiche la sortie de débogage dans Visual Studio.

## Déboguer l’activation de la tâche en arrière-plan


L’activation de la tâche en arrière-plan dépend de la correspondance correcte de trois éléments. Cette procédure montre comment vérifier et garantir que ces éléments correspondent tous.

-   nom et espace de noms de la classe de tâche en arrière-plan ;
-   attribut de point d’entrée spécifié dans le manifeste du package ;
-   point d’entrée spécifié par votre application lors de l’inscription de la tâche en arrière-plan.

1.  Utilisez Visual Studio pour noter le point d’entrée de la tâche en arrière-plan:

    -   En C# etC++, notez le nom et l’espace de noms de la classe de tâche en arrière-plan spécifiée dans le projet de tâche en arrière-plan.

2.  Utilisez le concepteur du manifeste pour vérifier que la tâche en arrière-plan est correctement déclarée dans le manifeste du package:

    -   En C# et C++, l’attribut de point d’entrée doit correspondre à l’espace de noms de la tâche en arrière-plan suivi du nom de la classe. Par exemple: RuntimeComponent1.MyBackgroundTask.
    -   Tous les types de déclencheurs utilisés avec la tâche doivent également être indiqués.
    -   Le fichier exécutable NE doit PAS être spécifié sauf si vous utilisez l’objet [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) ou [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543).

3.  Windows uniquement. Pour afficher le point d’entrée utilisé par Windows afin d’activer la tâche en arrière-plan, activez le suivi de débogage et utilisez le journal des événements Windows.

    Si vous suivez cette procédure et que le journal des événements indique un déclencheur ou point d’entrée incorrect pour la tâche en arrière-plan, votre application n’inscrit pas correctement la tâche en arrière-plan. Pour obtenir de l’aide sur cette tâche, voir [Inscrire une tâche en arrière-plan](register-a-background-task.md).

    1.  Ouvrez l’observateur d’événements en accédant à l’écran de démarrage et en recherchant eventvwr.exe.
    2.  Accédez à **Journaux des applications et des services** -&gt;**Microsoft** -&gt;**Windows** -&gt;**BackgroundTaskInfrastructure** dans l’observateur d’événements.
    3.  Dans le volet Actions, sélectionnez **Affichage** -&gt;**Afficher les journaux d’analyse et de débogage** pour activer la journalisation des diagnostics.
    4.  Sélectionnez le **journal de diagnostic**, puis cliquez sur **Activer le journal**.
    5.  Essayez à présent d’utiliser votre application pour inscrire et activer la tâche en arrière-plan une nouvelle fois.
    6.  Consultez les journaux de diagnostic à la recherche d’informations détaillées sur l’erreur. Cela comprend le point d’entrée inscrit pour la tâche en arrière-plan.

![informations de débogage des tâches en arrière-plan dans l’observateur d’événements](images/event-viewer.png)

## Tâches en arrière-plan et déploiement du package Visual Studio


Si vous déployez une application utilisant des tâches en arrière-plan à l’aide de Visual Studio et si vous mettez ensuite à jour la version (majeure et/ou mineure) précisée dans le concepteur du manifeste, le redéploiement de l’application avec Visual Studio qui s’ensuivra peut entraîner un blocage des tâches en arrière-plan de l’application. Vous pouvez remédier à ce problème de la manière suivante.

-   Utilisez Windows PowerShell pour déployer l’application mise à jour (plutôt que Visual Studio). Pour cela, exécutez le script généré avec le package.
-   Si vous avez déjà déployé l’application à l’aide de VisualStudio et si ses tâches en arrière-plan sont maintenant bloquées, redémarrez ou bien déconnectez-vous et reconnectez-vous afin de relancer l’exécution des tâches en arrière-plan de l’application.
-   Vous pouvez sélectionner l’option de débogage «Toujours réinstaller mon package» pour éviter cela dans des projetsC#.
-   Patientez jusqu’à ce que l’application soit prête pour un déploiement final avant d’incrémenter la version du package (ne la changez pas pendant le débogage).

## Remarques


-   Assurez-vous que votre application vérifie la présence d’inscriptions de tâches en arrière-plan existantes avant d’inscrire de nouveau la tâche en arrière-plan. Plusieurs inscriptions de la même tâche en arrière-plan peuvent entraîner des résultats inattendus si vous exécutez plusieurs fois la tâche en arrière-plan chaque fois qu’elle est déclenchée.
-   Sur Windows, si la tâche en arrière-plan requiert un accès à l’écran de verrouillage, veillez à placer l’application sur l’écran de verrouillage avant d’essayer de déboguer la tâche en arrière-plan. Pour plus d’informations sur la spécification des options de manifeste pour les applications compatibles avec l’écran de verrouillage, voir [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md).
-   Les paramètres d’inscription de la tâche en arrière-plan sont validés au moment de l’inscription. Une erreur est retournée si l’un des paramètres d’inscription n’est pas valide. Vérifiez que votre application gère de façon fluide les scénarios dans lesquels l’inscription de la tâche en arrière-plan échoue. En revanche, si votre application dépend d’un objet d’inscription valide après la tentative d’inscription d’une tâche, elle peut se bloquer.

Pour plus d’informations sur l’utilisation de Visual Studio pour déboguer une tâche en arrière-plan, voir [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du WindowsStore](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx).

## Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan](create-and-register-a-background-task.md)
* [Inscrire une tâche en arrière-plan](register-a-background-task.md)
* [Déclarer des tâches en arrière-plan dans le manifeste de l’application](declare-background-tasks-in-the-application-manifest.md)
* [Recommandations pour les tâches en arrière-plan](guidelines-for-background-tasks.md)
* [Comment déclencher des événements de suspension, des événements de reprise et des événements en arrière-plan dans des applications du Windows Store](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)
* [Analyse de la qualité du code des applications du Windows Store avec analyse de code Visual Studio](https://msdn.microsoft.com/library/windows/apps/xaml/hh441471.aspx)

 

 



<!--HONumber=Jun16_HO5-->


