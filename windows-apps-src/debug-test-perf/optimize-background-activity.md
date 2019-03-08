---
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: Optimiser l’activité en arrière-plan
description: Créez des applications UWP qui fonctionnent avec le système pour utiliser des tâches en arrière-plan de manière économe en énergie.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71a56bc23b4b727d5be2ed35fb77afae03f0689c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613224"
---
# <a name="optimize-background-activity"></a>Optimiser l’activité en arrière-plan

Les applications Windows universelles doivent fonctionner correctement sur toutes les familles d’appareils. Sur les appareils alimentés par batterie, la consommation d’énergie est un facteur critique dans l’expérience globale de l’utilisateur avec votre application. Une autonomie d’une journée est souhaitable pour les utilisateurs, mais elle nécessite une efficacité énergétique de tous les logiciels installés sur l’appareil, y compris du vôtre. 

Le comportement des tâches en arrière-plan joue sans doute le rôle le plus important dans le coût d’énergie total d’une application. Une tâche en arrière-plan correspond à une activité d’un programme exécutée par le système, lorsque l’application n’est pas ouverte. Pour plus d’informations, consultez l’article [Créer et inscrire une tâche en arrière-plan hors processus](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

## <a name="background-activity-permissions"></a>Autorisations d’activité en arrière-plan

Sur les appareils mobiles et de bureau exécutant Windows 10, version 1607 ou ultérieure, les utilisateurs peuvent visualiser leur valeur « Utilisation de la batterie par l’application » dans la section Batterie de l’application Paramètres. Cette section affiche une liste d’applications et le pourcentage d’autonomie de la batterie consommé par chaque application (par rapport au niveau d’autonomie depuis le dernier chargement). Les utilisateurs peuvent sélectionner l’une des applications UWP de cette liste pour ouvrir les contrôles liés à l’activité en arrière-plan.

![utilisation de la batterie par application](images/battery-usage-by-app.png)

### <a name="background-permissions-on-mobile"></a>Autorisations des tâches en arrière-plan sur les appareils mobiles

Sur les appareils mobiles, les utilisateurs peuvent voir une liste de cases d’option qui spécifient le paramètre d’autorisation de tâche en arrière-plan pour l’application. L’activité en arrière-plan peut être définie sur « Toujours autorisé » « Jamais autorisé » ou « Gestion par Windows », ce qui signifie que l’activité en arrière-plan de l’application est régie par le système en fonction d’un certain nombre de facteurs. 

![Cases d’option des autorisations de tâche en arrière-plan](images/background-task-permissions.png)

### <a name="background-permissions-on-desktop"></a>Autorisations des tâches en arrière-plan sur les appareils de bureau

Sur les appareils de bureau, le paramètre « Gestion par Windows » est présenté sous la forme d’un bouton bascule, défini sur la valeur **Actif** par défaut. Si l’utilisateur bascule ce bouton vers la valeur **Inactif**, il voit apparaître une case à cocher lui permettant de définir manuellement les autorisations d’activité en arrière-plan. Si cette case est cochée, l’application sera autorisée à exécuter des tâches en arrière-plan à tout moment. Si la case est décochée, l’activité en arrière-plan sera désactivée.

![Bouton bascule d’autorisations de tâche en arrière-plan défini sur Actif](images/background-task-permissions-on.png)

![Bouton bascule d’autorisations de tâche en arrière-plan défini sur Inactif](images/background-task-permissions-off.png)

Dans votre application, vous pouvez utiliser la valeur d’énumération [**BackgroundAccessStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) renvoyée par un appel de la méthode [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) pour déterminer le paramétrage actuel de l’application concernant l’autorisation d’activité en arrière-plan.

Tout cela pour dire que, si votre application ne gère pas l’activité en arrière-plan de manière responsable, l’utilisateur peut refuser des autorisations en arrière-plan à votre application, ce qui n’est pas toujours souhaitable pour les deux parties. Si votre application n’a pas été autorisée à exécuter des tâches en arrière-plan, mais qu’elle nécessite une activité en arrière-plan pour effectuer une opération à l’intention de l’utilisateur, vous pouvez en informer l’utilisateur et diriger ce dernier vers l’application Paramètres. Vous pouvez effectuer cette opération en [lançant l’application Paramètres](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/launch-settings-app) au niveau de la page Applications en arrière-plan ou Utilisation de la batterie.

## <a name="work-with-the-battery-saver-feature"></a>Utiliser la fonctionnalité Économiseur de batterie
L’Économiseur de batterie est une fonctionnalité système que les utilisateurs peuvent configurer dans les paramètres. Il interrompt l’activité en arrière-plan de toutes les applications lorsque le niveau de batterie descend en dessous d’un seuil défini par l’utilisateur, *sauf* pour l’activité en arrière-plan des applications qui a été configurée sur « Toujours autorisé ».

Vérifiez l’état du mode Économiseur de batterie dans votre application en référençant la propriété [**PowerManager.EnergySaverStatus**](https://docs.microsoft.com/en-us/uwp/api/windows.system.power.energysaverstatus). Il s’agit de l’une des valeurs d’énumération suivantes : **EnergySaverStatus.Disabled**, **EnergySaverStatus.Off** ou **EnergySaverStatus.On**. Si votre application nécessite une activité en arrière-plan et qu’elle n’est pas définie sur « Toujours autorisé », elle doit gérer la valeur **EnergySaverStatus.On** en informant l’utilisateur que les tâches en arrière-plan requises ne s’exécuteront pas tant que l’économiseur de batterie sera désactivé. Si la gestion de l’activité en arrière-plan est l’objectif principal de la fonctionnalité d’Économiseur de batterie, votre application peut effectuer des ajustements supplémentaires pour économiser davantage d’énergie lorsque l’Économiseur de batterie est activé.  Lorsque l’Économiseur de batterie est activé, votre application peut réduire son utilisation des animations, arrêter l’interrogation de localisation ou différer les synchronisations et les sauvegardes. 

## <a name="further-optimize-background-tasks"></a>Optimiser les tâches en arrière-plan
Voici les étapes supplémentaires que vous pouvez effectuer lors de la déclaration de vos tâches en arrière-plan pour les rendre plus économes en énergie.

### <a name="use-a-maintenance-trigger"></a>Utiliser un déclencheur de maintenance 
Un objet [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) peut s’utiliser à la place d’un objet [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) pour déterminer le moment de démarrage d’une tâche en arrière-plan. Les tâches qui utilisent des déclencheurs de maintenance ne s’exécutent que lorsque l’appareil est connecté à une prise de courant CA, et elles sont autorisées à s’exécuter plus longtemps. Pour obtenir des instructions, consultez [Utiliser un déclencheur de maintenance](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger).

### <a name="use-the-backgroundworkcostnothigh-system-condition-type"></a>Utiliser le type de condition système **BackgroundWorkCostNotHigh**
Les conditions système doivent être réunies dans l’ordre pour que les tâches en arrière-plan puissent s’exécuter. Pour plus d’informations, consultez [Définir des conditions pour exécuter une tâche en arrière-plan](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task). Le coût du travail en arrière-plan est une mesure qui évalue l’impact énergétique *relatif* de l’exécution de la tâche en arrière-plan. Une tâche en cours lorsque l’appareil est branché sur secteur est considérée comme **faible** (impact faible/nul sur la batterie). Une tâche en cours lorsque l’appareil est sur batterie avec l’écran éteint est considérée comme **haute**, car l’activité des programmes sur l’appareil est probablement faible sur l’appareil, de sorte que la tâche en arrière-plan a un coût relatif supérieur. Une tâche en cours lorsque l’appareil est sur batterie avec l’écran *allumé* est considérée comme **moyenne**, car certains programmes peuvent être en cours d’exécution et la tâche en arrière-plan a un coût énergétique un peu supérieur. La condition système **BackgroundWorkCostNotHigh** diffère simplement la capacité de votre tâche à s’exécuter lorsque l’écran est allumé ou que l’appareil est connecté à une prise de courant.

## <a name="test-battery-efficiency"></a>Tester l’efficacité de la batterie

Veillez à tester votre application sur des appareils réels pour tous les scénarios impliquant une forte consommation d’énergie. Il est recommandé de tester votre application sur différents appareils, avec l’économiseur de batterie activé et désactivé, dans des environnements à niveau de sécurité réseau variable.

## <a name="related-topics"></a>Rubriques connexes

* [Créer et inscrire une tâche en arrière-plan out-of-process](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [Planification des performances](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  

