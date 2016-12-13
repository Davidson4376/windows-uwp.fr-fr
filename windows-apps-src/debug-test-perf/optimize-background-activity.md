---
author: PatrickFarley
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: "Utiliser des fonctionnalités d’économie d’énergie"
description: "Créez des applications UWP qui fonctionnent avec le système pour utiliser des tâches en arrière-plan de manière économe en énergie."
translationtype: Human Translation
ms.sourcegitcommit: 73b19e54b863693aece045e5b653bc0583a676bb
ms.openlocfilehash: 854ec43d075f8adc1f875d3b9e5e2d818434edb9

---

# <a name="optimize-background-activity"></a>Optimiser l’activité en arrière-plan

Les applications Windows universelles doivent fonctionner correctement sur toutes les familles d’appareils. Sur les appareils alimentés par batterie, la consommation d’énergie est un facteur critique dans l’expérience globale de l’utilisateur avec votre application. Une autonomie d’une journée est souhaitable pour les utilisateurs, mais elle nécessite une efficacité énergétique de tous les logiciels installés sur l’appareil, y compris du vôtre. 

Le comportement des tâches en arrière-plan est sans doute le facteur le plus important dans le coût d’énergie total d’une application. Une tâche en arrière-plan correspond à une activité d’un programme exécutée par le système, lorsque l’application n’est pas ouverte. Pour plus d’informations, consultez [Créer et inscrire une tâche en arrière-plan hors processus](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task).

## <a name="background-activity-allowance"></a>Allocation des activités en arrière-plan

Dans Windows&nbsp;10 version&nbsp;1607, l’utilisateur peut afficher l’«&nbsp;utilisation de la batterie par application&nbsp;» dans la section **Batterie** de l’application Paramètres. Là s’affichent une liste d’applications et le pourcentage de charge (par rapport au niveau d’autonomie depuis le dernier chargement) que chaque application a consommée. 

![utilisation de la batterie par application](images/battery-usage-by-app.png)

Pour les applications UWP de cette liste, les utilisateurs ont un certain contrôle sur le traitement de l’activité en arrière-plan par le système. L’activité en arrière-plan peut être spécifiée comme suit&nbsp;: «&nbsp;Toujours autorisé&nbsp;» «&nbsp;Gestion par Windows&nbsp;» (paramètre par défaut) ou «&nbsp;Jamais autorisé&nbsp;» (plus d’informations sur ci-dessous). Utilisez la valeur d’énumération **BackgroundAccessStatus** renvoyée par la méthode [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) pour connaître les activités en arrière-plan autorisées par votre application.

![autorisations des tâches en arrière-plan](images/background-task-permissions.png)

Tout cela pour dire que, si votre application ne gère pas l’activité en arrière-plan de manière responsable, l’utilisateur peut refuser des autorisations en arrière-plan à votre application, ce qui n’est pas toujours souhaitable pour les deux&nbsp;parties.

## <a name="work-with-the-battery-saver-feature"></a>Utiliser la fonctionnalité Économiseur de batterie
L’Économiseur de batterie est une fonctionnalité système que les utilisateurs peuvent configurer dans les paramètres. Il interrompt l’activité en arrière-plan de toutes les applications lorsque le niveau de batterie descend en dessous d’un seuil défini par l’utilisateur, *sauf* pour l’activité en arrière-plan des applications qui a été configurée sur «&nbsp;Toujours autorisé&nbsp;».

Si votre application est configurée avec «&nbsp;Gestion par Windows&nbsp;» et appelle **BackgroundExecutionManager.RequestAccessAsync()** pour enregistrer une activité en arrière-plan lorsque l’Économiseur de batterie est activé, elle renvoie une valeur **DeniedSubjectToSystemPolicy**. Votre application doit gérer cette situation en notifiant l’utilisateur que les tâches en arrière-plan données ne s’exécuteront pas tant que l’Économiseur de batterie est désactivé et qu’elles sont déclarées à nouveau dans le système. Si une tâche en arrière-plan a déjà été déclarée comme étant à exécuter et que l’Économiseur de batterie est activé au moment de son déclenchement, la tâche ne s’exécute pas et l’utilisateur n’est pas averti. Afin de réduire ce risque, il est recommandé de programmer votre application pour qu’elle redéclare ses tâches en arrière-plan à chaque ouverture.

Si la gestion de l’activité en arrière-plan est l’objectif principal de la fonctionnalité d’Économiseur de batterie, votre application peut effectuer des ajustements supplémentaires pour économiser davantage d’énergie lorsque l’Économiseur de batterie est activé. Vérifiez l’état du mode Économiseur de batterie dans votre application en référençant la propriété [**PowerManager.PowerSavingMode**](https://msdn.microsoft.com/library/windows/apps/windows.phone.system.power.powermanager.powersavingmode.aspx). Il s’agit d’une valeur d’énumération&nbsp;: **PowerSavingMode.Off** ou **PowerSavingMode.On**. Lorsque l’Économiseur de batterie est activé, votre application peut réduire son utilisation des animations, arrêter l’interrogation de localisation ou différer les synchronisations et les sauvegardes. 

## <a name="further-optimize-background-tasks"></a>Optimiser les tâches en arrière-plan
Voici les étapes supplémentaires que vous pouvez effectuer lors de la déclaration de vos tâches en arrière-plan pour les rendre plus économes en énergie.

Utilisez un déclencheur de maintenance. Un objet [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) peut s’utiliser à la place d’un objet [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) pour déterminer le moment de démarrage d’une tâche en arrière-plan. Les tâches qui utilisent des déclencheurs de maintenance ne s’exécutent que lorsque l’appareil est connecté à une prise de courant&nbsp;CA, et elles sont autorisées à s’exécuter plus longtemps. Pour obtenir des instructions, consultez [Utiliser un déclencheur de maintenance](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger).

Utilisez le type de condition système **BackgroundWorkCostNotHigh**. Les conditions système doivent être réunies dans l’ordre pour que les tâches en arrière-plan puissent s’exécuter. Pour plus d’informations, consultez [Définir des conditions pour exécuter une tâche en arrière-plan](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task). Le coût du travail en arrière-plan est une mesure qui évalue l’impact énergétique *relatif* de l’exécution de la tâche en arrière-plan. Une tâche en cours lorsque l’appareil est branché sur secteur est considérée comme **faible** (impact faible/nul sur la batterie). Une tâche en cours lorsque l’appareil est sur batterie avec l’écran éteint est considérée comme **haute**, car l’activité des programmes sur l’appareil est probablement faible sur l’appareil, de sorte que la tâche en arrière-plan a un coût relatif supérieur. Une tâche en cours lorsque l’appareil est sur batterie avec l’écran *allumé* est considérée comme **moyenne**, car certains programmes peuvent être en cours d’exécution et la tâche en arrière-plan a un coût énergétique un peu supérieur. La condition système **BackgroundWorkCostNotHigh** diffère simplement la capacité de votre tâche à s’exécuter lorsque l’écran est allumé ou que l’appareil est connecté à une prise de courant.

## <a name="test-battery-efficiency"></a>Tester l’efficacité de la batterie

Veillez à tester votre application sur des appareils réels pour tous les scénarios requérant une forte consommation d’énergie. Il est recommandé de tester votre application sur différents appareils, avec l’économiseur de batterie activé et désactivé, dans des environnements à niveau de sécurité réseau variable.

## <a name="related-topics"></a>Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan hors processus](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-an-outofproc-background-task)  
* [Planification des performances](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  




<!--HONumber=Dec16_HO1-->


