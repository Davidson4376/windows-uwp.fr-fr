---
ms.assetid: F912161D-3767-4F35-88C0-E1ECDED692A2
title: Améliorer les performances du nettoyage de la mémoire (garbage collection)
description: Les applications de plateforme Windows universelle (UWP) écrites en C# et Visual Basic bénéficient de la gestion automatique de la mémoire du récupérateur de mémoire .NET. Cette section résume les meilleures pratiques en termes de comportement et de performance du récupérateur de mémoire .NET pour les applications UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6f22b893d0c55cb9220e0894527836a0bb5e750b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625974"
---
# <a name="improve-garbage-collection-performance"></a>Améliorer les performances du nettoyage de la mémoire (garbage collection)


Les applications de plateforme Windows universelle (UWP) écrites en C# et Visual Basic bénéficient de la gestion automatique de la mémoire du récupérateur de mémoire .NET. Cette section résume les meilleures pratiques en termes de comportement et de performance du récupérateur de mémoire .NET pour les applications UWP. Pour plus d’informations sur le fonctionnement du récupérateur de mémoire .NET et les outils en matière de débogage et d’analyse des performances du récupérateur de mémoire, voir [Nettoyage de la mémoire](https://msdn.microsoft.com/library/windows/apps/xaml/0xy59wtx.aspx).

**Remarque**  avoir à intervenir dans le comportement par défaut du garbage collector est fortement indicative général des problèmes de mémoire avec votre application. Pour plus d’informations, voir [Utilisation de l’outil d’utilisation de la mémoire pendant le débogage dans Visual Studio 2015](https://blogs.msdn.com/b/visualstudioalm/archive/2014/11/13/memory-usage-tool-while-debugging-in-visual-studio-2015.aspx). Cette rubrique s’applique uniquement au code C# et Visual Basic.

 

Le récupérateur de mémoire détermine quand s’exécuter en équilibrant la consommation mémoire du tas géré avec la quantité de travail qu’il doit effectuer. L’une de ses techniques consiste à diviser le tas en générations, et à ne nettoyer généralement qu’une partie du tas. Le tas géré comporte trois générations :

-   Génération 0. Cette génération contient les objets récemment alloués, sauf s’ils représentent 85 Ko ou plus, auquel cas ils font partie d’un tas d’objets volumineux. Le tas d’objets volumineux est regroupé avec les collections de génération 2. Les collections de génération 0 sont les collections les plus fréquentes et nettoient des objets à courte durée de vie tels que les variables locales.
-   Génération 1. Cette génération contient des objets qui ont survécu aux collections de génération 0. Elle sert de mémoire tampon entre la génération 0 et la génération 2. Les collections de génération 1 se produisent moins fréquemment que les collections de génération 0 et nettoient des objets temporaires qui étaient actifs pendant les collections de génération 0. Une collection de génération 1 regroupe aussi la génération 0.
-   Génération 2. Cette génération contient des objets à durée de vie longue qui ont survécu aux collections de génération 0 et de génération 1. Les collections de génération 2 sont les moins fréquentes et regroupent le tas géré entier, notamment le tas des objets volumineux qui contient 85 Ko ou plus.

Vous pouvez mesurer les performances du récupérateur de mémoire selon 2 aspects : le temps nécessaire au nettoyage de la mémoire et la consommation de mémoire du tas géré. Si vous disposez d’une petite application et d’un tas de taille inférieure à 100 Mo, faites porter vos efforts sur la réduction de la consommation de mémoire. Si vous avez une application avec un tas géré qui fait plus de 100 Mo, concentrez-vous uniquement sur la réduction de la durée de nettoyage de la mémoire. Voici comment vous pouvez aider le récupérateur de mémoire .NET à délivrer de meilleures performances.

## <a name="reduce-memory-consumption"></a>Réduire la consommation de mémoire

### <a name="release-references"></a>Libérer les références

Une référence à un objet dans votre application empêche cet objet ainsi que tous les objets auxquels il fait référence d’être collectés. Le compilateur .NET s’avère efficace pour détecter le moment où une variable n’est plus utilisée. Les objets conservés dans cette variable pourront faire partie d’une collection. Toutefois, dans certains cas, la référence de certains objets à d’autres n’est pas évidente, car votre application peut conserver une partie du graphique d’objet dans des bibliothèques. Pour plus d’informations sur les outils et les techniques permettant d’identifier les objets qui ont survécu au nettoyage de la mémoire, voir [Nettoyage de la mémoire et performance](https://msdn.microsoft.com/library/windows/apps/xaml/ee851764.aspx).

### <a name="induce-a-garbage-collection-if-its-useful"></a>Générer un nettoyage de la mémoire uniquement en cas d’utilité

Générez un nettoyage de la mémoire uniquement après avoir mesuré les performances de votre application et déterminé que la génération d’une collection améliorera ses performances.

Vous pouvez générer un nettoyage de la mémoire d’une génération en appelant [**GC.Collect(n)**](https://msdn.microsoft.com/library/windows/apps/xaml/y46kxc5e.aspx), où n représente la génération que vous voulez collecter (0, 1 ou 2).

**Remarque**  nous vous recommandons de ne pas forcer un garbage collection dans votre application, car le garbage collector utilise des nombreuses heuristiques pour déterminer le meilleur moment pour exécuter une collecte et forcer une collection est, dans de nombreux cas, une utilisation inutile de l’UC. Toutefois, si vous savez qu’un grand nombre d’objets de votre application ne sont plus utilisés et que vous voulez réattribuer cette mémoire au système, il peut s’avérer approprié de forcer un nettoyage de la mémoire. Par exemple, dans le cadre d’un jeu, vous pouvez effectuer un nettoyage à la fin d’une séquence de chargement pour libérer de la mémoire avant de commencer à jouer.
 
Pour éviter le déclenchement accidentel d’un trop grand nombre de nettoyages de la mémoire, vous pouvez affecter à [**GCCollectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/bb495757.aspx) la valeur **Optimized**. Cela indique au récupérateur de mémoire de lancer un nettoyage uniquement s’il considère qu’il est justifié.

## <a name="reduce-garbage-collection-time"></a>Réduire la durée de nettoyage de la mémoire

Cette section s’applique si vous avez analysé votre application et constaté que les nettoyages de la mémoire prenaient un temps considérable. Les temps d’interruption liés au nettoyage de la mémoire incluent la durée nécessaire à l’exécution d’une seule passe de nettoyage de la mémoire et la durée totale consacrée par votre application à l’ensemble des nettoyages. La durée d’une création de collection dépend du volume de données dynamiques que le récupérateur de mémoire doit analyser. La génération 0 et la génération 1 sont limitées par la taille, mais la génération 2 continue de croître à mesure que davantage d’objets à durée de vie longue deviennent actifs dans votre application. Cela signifie que les durées des collections pour la génération 0 et la génération 1 sont limitées, tandis que les collections de génération 2 peuvent prendre plus de temps. La fréquence des nettoyages de la mémoire dépend principalement de la quantité de mémoire allouée, car un nettoyage libère de la mémoire pour satisfaire les demandes d’allocation.

Le Garbage Collector interrompt de manière occasionnelle votre application afin d’exécuter des tâches, mais ne l’interrompt pas nécessairement en continu pendant le nettoyage. Les temps d’interruption ne sont généralement pas perceptibles par l’utilisateur dans votre application, en particulier dans le cas des collections de génération 0 et de génération 1. La fonctionnalité de [Nettoyage de la mémoire en arrière-plan](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#background-garbage-collection) du .NET Garbage Collector permet l’exécution simultanée des collections de génération 2 pendant l’exécution de votre application. Votre application ne sera interrompue que sur de courtes périodes. Toutefois, il n’est pas toujours possible d’effectuer une collection de génération 2 en arrière-plan. Dans ce cas, l’interruption peut être perceptible par l’utilisateur si votre tas est suffisamment important (plus de 100 Mo).

Les nettoyages de mémoire fréquents peuvent contribuer à une augmentation de la consommation (et de la puissance) de l’unité centrale et des temps de chargement ou à une diminution de la fréquence d’images dans votre application. Voici quelques techniques auxquelles vous pouvez recourir en vue de réduire les temps de nettoyage de la mémoire et les interruptions liées à ces collections dans votre application UWP.

### <a name="reduce-memory-allocations"></a>Réduire les allocations de mémoire

Si vous n’allouez pas d’objets, le Garbage Collector ne s’exécute pas tant que le système présente des conditions de mémoire faibles. La réduction de la quantité de mémoire que vous allouez directement se traduit par une fréquence inférieure des nettoyages de la mémoire.

Si dans certaines sections de votre application les interruptions ne sont vraiment pas souhaitables, vous pouvez alors préallouer les objets nécessaires en amont à un moment où les conditions de performance sont moins exigeantes. À titre d’exemple, un jeu pourrait allouer tous les objets nécessaires à l’expérience de jeu pendant l’écran de chargement d’un niveau et ne pas effectuer d’allocations pendant le jeu. Ainsi, l’utilisateur n’est pas interrompu en cours de jeu et il peut bénéficier d’une fréquence d’images plus élevée et plus cohérente.

### <a name="reduce-generation-2-collections-by-avoiding-objects-with-a-medium-length-lifetime"></a>Réduire les nettoyages de génération 2 en évitant les objets à la durée de vie moyenne

Les nettoyages de la mémoire générationnels s’effectuent mieux si la durée de vie des objets de votre application est courte et/ou très longue. Les objets à durée de vie courte sont regroupés dans les collections de génération 0 et de génération 1 plus économes, et les objets à durée de vie longue sont promus en génération 2, qui est irrégulièrement collectée. Les objets à durée de vie longue sont ceux qui sont utilisés pendant toute la durée d’exécution de votre application, ou pendant une période significative, telle que pendant l’exécution d’une page ou d’un niveau de jeu spécifique.

Si vous créez fréquemment des objets qui ont une durée de vie suffisamment longue pour être promus en génération 2, davantage de collections de génération 2 coûteuses auront lieu. Vous pouvez réduire les collections de génération 2 en recyclant des objets existants ou en libérant les objets plus rapidement.

Un exemple courant d’objets à durée de vie moyenne concerne les objets qui sont utilisés pour afficher des éléments dans une liste qu’un utilisateur fait défiler. Si les objets sont créés lorsque les éléments dans la liste défilent dans l’affichage, et ne sont plus référencés tandis que les éléments de la liste défilent en dehors de l’affichage, votre application dispose généralement d’un grand nombre de collections de génération 2. Dans ce type de situations, vous pouvez préallouer et réutiliser un ensemble d’objets pour les données qui sont affichées à l’utilisateur de manière active, et utiliser des objets à durée de vie courte pour charger des informations lors de l’affichage des éléments dans la liste.

### <a name="reduce-generation-2-collections-by-avoiding-large-sized-objects-with-short-lifetimes"></a>Réduire les collection de génération 2 en évitant les objets de grande taille à courte durée de vie

Tout objet de 85 Ko ou plus est alloué au tas des objets volumineux (LOH, Large Object Heap) et est collecté dans le cadre de la génération 2. Si vous disposez de variables temporaires, telles que des mémoires tampons, qui sont supérieures à 85 Ko, alors une collection de génération 2 les nettoie. La limitation des variables temporaires à moins de 85 Ko permet de réduire le nombre de collections de génération 2 dans votre application. Une technique courante consiste à créer un pool de mémoires tampons et à réutiliser les objets du pool afin d’éviter des allocations temporaires importantes.

### <a name="avoid-reference-rich-objects"></a>Éviter les objets riches en référence

Le Garbage Collector détermine quels objets sont dynamiques en suivant les références entre les objets, en partant des racines de votre application. Pour plus d’informations, voir [Que se passe-t-il pendant un nettoyage de la mémoire ?](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#what-happens-during-a-garbage-collection) Si un objet contient de nombreuses références, le Garbage Collector doit exécuter davantage de tâches. Une technique courante (en particulier avec des objets volumineux) consiste à convertir des objets riches en référence en des objets sans aucune référence (par exemple, au lieu de stocker une référence, stocker un index). Il est clair que cette technique ne fonctionne que s’il est logiquement possible de la mettre en œuvre.

Le remplacement des références d’objet par des index peut représenter un changement perturbant et compliqué pour votre application et s’avère plus efficace dans le cas d’objets volumineux avec un grand nombre de références. Ne procédez à ce changement que si vous identifiez des durées longues de nettoyages de la mémoire dans votre application liées aux objets riches en référence.

 

 




