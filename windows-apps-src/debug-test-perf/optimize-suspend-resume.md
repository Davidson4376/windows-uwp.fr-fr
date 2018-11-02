---
author: jwmsft
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: Optimiser l’interruption/la reprise
description: Créez des applications de plateforme Windows universelle (UWP) qui simplifient l’utilisation du système de gestion de la durée de vie des processus afin d’assurer une reprise efficace après une suspension ou un arrêt.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4cbaa56f9c25c0e4ea1f10c79b4f7d1100748532
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5977730"
---
# <a name="optimize-suspendresume"></a>Optimiser l’interruption/la reprise


Créez des applications de plateforme Windows universelle (UWP) qui simplifient l’utilisation du système de gestion de la durée de vie des processus afin d’assurer une reprise efficace après une suspension ou un arrêt.

## <a name="launch"></a>Lancer

Si vous réactivez une application après l’avoir suspendue/arrêtée, vérifiez si une longue durée s’est écoulée. Si tel est le cas, pensez à revenir dans la page de destination principale de l’application au lieu d’afficher les données utilisateur obsolètes. Cela entraînera également un démarrage plus rapide.

Pendant l’activation, vérifiez toujours l’état PreviousExecutionState du paramètre des arguments d’événement. Par exemple, pour les activations lancées, vérifiez LaunchActivatedEventArgs.PreviousExecutionState). Si la valeur est ClosedByUser ou NotRunning, ne perdez pas votre temps à restaurer l’état de mise en mémoire précédent. Dans ce cas, il convient de fournir une expérience toute nouvelle, laquelle entraînera un démarrage plus rapide.

Au lieu de restaurer l’état de mise en mémoire précédent, pensez à assurer le suivi de cet état et à le restaurer uniquement à la demande. Par exemple, prenez une situation dans laquelle votre application avec un état de mise en mémoire correspondant à 3 pages a été suspendue, avant d’être arrêtée. Dès son relancement, si vous décidez de renvoyer l’utilisateur à la 3e page, ne restaurez pas dynamiquement l’état des 2 premières pages. Maintenez plutôt cet état, et utilisez-le uniquement lorsque vous en avez besoin.

## <a name="while-running"></a>En cours d’exécution

Il est recommandé de ne pas attendre l’événement de suspension, puis de maintenir une grande quantité de l’état. À la place, votre application doit être maintenue de façon incrémentielle en plus petites quantités de l’état lorsqu’elle s’exécute. Cela est particulièrement important pour les applications volumineuses qui courent le risque de manquer de temps pendant la suspension si elles essaient d’enregistrer tous les éléments en même temps.

Toutefois, vous devez trouver un bon équilibre entre l’enregistrement incrémentiel et les performances de votre application en cours d’exécution. Un bon compromis consiste à effectuer le suivi incrémentiel des données qui ont été modifiées (et qui doivent donc être enregistrées) et à utiliser l’événement de suspension pour enregistrer réellement les données (ce qui est plus rapide que d’enregistrer toutes les données ou d’examiner l’état entier de l’application pour décider des éléments à enregistrer).

N’utilisez pas les événements Activated ou VisibilityChanged de la fenêtre pour choisir le moment auquel enregistrer l’état. Lorsque l’utilisateur quitte votre application, la fenêtre est désactivée, mais le système attend un court moment (environ 10 secondes) avant de suspendre l’application. Il s’agit de fournir une meilleure réactivité si jamais l’utilisateur rebascule rapidement vers votre application. Attendez l’événement de suspension avant d'exécuter la logique correspondante.

## <a name="suspend"></a>Suspendre

Lors de la suspension, réduisez l’encombrement de votre application. Si votre application utilise moins de mémoire alors qu’elle est suspendue, l’ensemble du système sera plus réactif et un plus petit nombre d’applications suspendues (y compris les vôtres) seront arrêtées. Toutefois, équilibrez ce point avec la nécessité d’effectuer des reprises réactives : ne réduisez pas trop l’encombrement, car la reprise est considérablement ralentie lorsque votre application recharge de nombreuses données en mémoire.

Pour les applications gérées, le système exécute une passe de nettoyage de la mémoire après l’exécution des gestionnaires de suspension de l’application. Veillez à tirer parti de cela en libérant les références aux objets, ce qui vous aidera à réduire l’encombrement de l’application pendant sa suspension.

Dans l’idéal, votre application en finit avec la logique de suspension en moins de 1 seconde. Plus la suspension est rapide, plus l’expérience utilisateur est réactive pour les autres applications et parties du système. Si vous y êtes contraint, sachez que votre logique de suspension peut prendre jusqu’à 5 secondes sur les appareils de bureau et 10 secondes sur les appareils mobiles. Si ces délais sont dépassés, votre application sera arrêtée brusquement. Vous ne souhaitez pas que cela se produise, car le cas échéant, si l’utilisateur rebascule vers votre application, un nouveau processus sera lancé, et l’expérience sera beaucoup plus lente que la reprise d’une application suspendue.

## <a name="resume"></a>Reprendre

La plupart des applications ne nécessitent rien de spécial lors de leur reprise ; en règle générale, vous ne gérez donc pas cet événement. Certaines applications utilisent la reprise pour restaurer les connexions qui ont été fermées lors de la suspension ou pour actualiser les données qui sont peut-être obsolètes. Si vous le préférez, vous pouvez concevoir votre application pour lancer ces activités à la demande. Cela entraîne une expérience plus rapide lorsque l’utilisateur rebascule vers une application suspendue et permet de s’assurer que vous ne faites que ce dont l’utilisateur a réellement besoin.

## <a name="avoid-unnecessary-termination"></a>Éviter les arrêts intempestifs

Le système de gestion de la durée de vie des processus UWP peut être amené à suspendre ou à arrêter une application pour diverses raisons. Il a été conçu de manière à remettre rapidement l’application dans l’état où elle se trouvait avant sa suspension ou son arrêt. Lorsque cela se passe bien, le processus de suspension ou d’arrêt reste transparent pour l’utilisateur. Nous allons vous donner quelques astuces que votre application UWP peut utiliser pour aider le système à simplifier les transitions tout au long de la durée de vie d’une application.

Une application peut être suspendue lorsque l’utilisateur la met en arrière-plan ou que le système passe dans un état de faible consommation d’énergie. Quand l’application est suspendue, elle déclenche l’événement de suspension et dispose de cinq secondes pour enregistrer ses données. Si le gestionnaire de l’événement de suspension de l’application ne s’exécute pas dans un délai de cinq secondes, le système suppose que l’application a cessé de répondre et l’arrête. Après avoir été arrêtée, l’application doit recommencer tout le processus de démarrage au lieu d’être immédiatement chargée en mémoire quand l’utilisateur y revient.

### <a name="serialize-only-when-necessary"></a>Sérialiser uniquement en cas de nécessité

De nombreuses applications sérialisent toutes leurs données lors de la mise en suspens. Toutefois, si vous ne devez stocker qu’une petite quantité de données de paramètres d’application, vous devez utiliser le magasin[**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) plutôt que de sérialiser les données. Utilisez la sérialisation pour les grandes quantités de données et pour les données autres que les données de paramètres.

Évitez de resérialiser vos données si elles n’ont pas changé. Il faut davantage de temps pour sérialiser et enregistrer les données, mais aussi pour les lire et les désérialiser lors de la réactivation de l’application. Par conséquent, votre application doit pouvoir déterminer si son état a réellement changé, et s’il a changé, sérialiser et désérialiser uniquement les données qui ont changé. Pour cela, nous vous recommandons de sérialiser périodiquement les données en arrière-plan après leur modification. Avec cette méthode, toutes les données à sérialiser au moment de la suspension ont déjà été enregistrées, ce qui élimine toute tâche supplémentaire et accélère la suspension de l’application.

### <a name="serializing-data-in-c-and-visual-basic"></a>Sérialiser les données en C# et en Visual Basic

Pour la sérialisation des applications .NET, vous avez le choix entre les classes suivantes : [**System.Xml.Serialization.XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx), [**System.Runtime.Serialization.DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) et [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.json.datacontractjsonserializer.aspx)

Pour obtenir des performances optimales, utilisez la classe [**XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx). Avec **XmlSerializer**, la sérialisation et la désérialisation sont rapides, et l’encombrement mémoire reste faible. La classe **XmlSerializer** est peu dépendante de .NET Framework. Par conséquent, contrairement aux autres méthodes de sérialisation, **XmlSerializer** peut être utilisée en chargeant un nombre réduit de modules.

[**DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) simplifie la sérialisation des classes personnalisées, bien que son impact sur les performances soit supérieur à celui de **XmlSerializer**. Si vous voulez obtenir de meilleures performances, il est préférable de changer. En général, il vaut mieux ne pas charger plus d’un sérialiseur et **XmlSerializer** est préférable, à moins que vous n’ayez besoin des fonctionnalités d’un autre sérialiseur.

### <a name="reduce-memory-footprint"></a>Réduire l’encombrement mémoire

Le système essaie de conserver en mémoire autant d’applications suspendues que possible, pour que les utilisateurs puissent passer de l’une à l’autre rapidement et sans aucune difficulté. Lorsqu’une application est suspendue et qu’elle reste dans la mémoire du système, elle peut rapidement être ramenée au premier plan pour que l’utilisateur puisse s’en servir, sans qu’il soit nécessaire d’afficher un écran de démarrage ou d’effectuer une longue opération de chargement. Si les ressources pour conserver une application en mémoire sont insuffisantes, l’application est arrêtée. La gestion de la mémoire est donc importante pour les deux raisons suivantes :

-   Libérer le plus de mémoire possible au moment de la suspension de votre application réduit les risques de voir ensuite votre application arrêtée par manque de ressources disponibles.
-   Réduire la consommation de la mémoire totale de votre application limite les risques d’arrêt d’autres applications en cours de suspension.

### <a name="release-resources"></a>Libérer des ressources

Certains objets, tels que les fichiers et les périphériques, occupent une grande partie de la mémoire. C’est pourquoi nous recommandons qu’une application suspendue libère les descripteurs de ces objets et les recrée lorsque c’est nécessaire. C’est également le moment approprié pour supprimer les caches qui ne seront plus valides à la reprise de l’application. De plus, l’infrastructure XAML se charge si nécessaire du nettoyage de la mémoire dans les applications C# et Visual Basic. Vous avez ainsi l’assurance que les objets qui ne sont plus référencés dans le code de l’application sont libérés.

## <a name="resume-quickly"></a>Reprendre rapidement l’exécution de l’application

Une application peut reprendre lorsque l’utilisateur l’exécute au premier plan ou lorsque le système quitte le mode d’alimentation faible. Lorsque qu’une application suspendue reprend, elle reprend là où elle s’est arrêtée. Aucune donnée n’est perdue, car les données sont conservées en mémoire, même si l’application reste suspendue pendant une longue période.

La plupart des applications n’ont pas besoin de gérer l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/BR205859). Lorsque votre application reprend, les variables et les objets retrouvent le même état qu’au moment de la suspension de l’application. Gérez l’événement **Resuming** uniquement si vous devez mettre à jour des données ou des objets qui peuvent avoir changé entre la suspension de votre application et sa reprise, comme du contenu (par exemple des données de flux de mise à jour) ou des connexions réseau qui peuvent ne pas avoir été utilisées pendant longtemps, ou si vous devez accéder de nouveau à un appareil (par exemple une webcam).

## <a name="related-topics"></a>Rubriques connexes

* [Recommandations pour la suspension et la reprise d’une application](https://msdn.microsoft.com/library/windows/apps/Hh465088)
 

 




