---
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: Performances
description: Les utilisateurs attendent de leurs applications qu’elles soient réactives, conviviales et qu’elles ne déchargent pas la batterie.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c13fb0f1b3d982d372896547bf2e47d978532ea4
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393445"
---
# <a name="performance"></a>Performances


Les utilisateurs attendent de leurs applications qu’elles soient réactives, conviviales et qu’elles ne déchargent pas la batterie. Techniquement, la performance est une exigence non fonctionnelle mais le fait de considérer les performances comme une fonctionnalité vous aidera à répondre aux attentes de vos utilisateurs. La spécification des objectifs et la mesure sont des facteurs essentiels. Déterminez quels sont les scénarios pour lesquels les performances sont essentielles et définissez ce que vous entendez par bonnes performances. Effectuez ensuite des mesures précoces et régulières tout au long du cycle de vie de votre projet pour être sûr d’atteindre vos objectifs. Cette section vous montre comment organiser votre flux de travail de performances, corriger les problèmes d’animation et de fréquence d’images, et régler le temps de démarrage, la durée de navigation par page et l’utilisation de la mémoire.

Si vous ne l’avez pas déjà fait, une étape que nous avons rencontrée entraîne des améliorations significatives en matière de performances. il suffit de porter votre application sur Windows 10. Plusieurs optimisations XAML (par exemple, [{x :bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) sont disponibles uniquement dans les applications Windows 10. Consultez [Portage d’applications vers Windows 10](https://docs.microsoft.com/windows/uwp/porting/index) et la session//Build/ [déplacée vers le plateforme Windows universelle](https://channel9.msdn.com/Events/Build/2015/3-741).

| Rubrique | Description |
|-------|-------------|
| [Planification des performances](planning-and-measuring-performance.md) | Les utilisateurs attendent de leurs applications qu’elles soient réactives, conviviales et qu’elles ne déchargent pas la batterie. Techniquement, la performance est une exigence non fonctionnelle mais le fait de considérer les performances comme une fonctionnalité vous aidera à répondre aux attentes de vos utilisateurs. La spécification des objectifs et la mesure sont des facteurs essentiels. Déterminez quels sont les scénarios pour lesquels les performances sont essentielles et définissez ce que vous entendez par bonnes performances. Effectuez ensuite des mesures précoces et régulières tout au long du cycle de vie de votre projet pour être sûr d’atteindre vos objectifs. |
| [Optimiser l’activité en arrière-plan](optimize-background-activity.md) | Créez des applications UWP qui fonctionnent avec le système pour utiliser des tâches en arrière-plan de manière économe en énergie. |
| [Optimisation de l’interface utilisateur de ListView et de GridView](optimize-gridview-and-listview.md) | Améliorez les performances du contrôle [<strong>GridView</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) et le temps de démarrage à travers la virtualisation de l’interface utilisateur, la réduction des éléments et la mise à jour progressive des éléments. |
| [Virtualisation des données ListView et GridView](listview-and-gridview-data-optimization.md) | Améliorez les performances du contrôle [<strong>GridView</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) et le temps de démarrage avec la virtualisation des données. |
| [Améliorer les performances de garbage collection](improve-garbage-collection-performance.md) | Les applications de plateforme Windows universelle (UWP) écrites en C# et Visual Basic bénéficient de la gestion automatique de la mémoire du récupérateur de mémoire .NET. Cette section résume les meilleures pratiques en termes de comportement et de performance du récupérateur de mémoire .NET pour les applications UWP. |
| [Maintenir la réactivité du thread d’interface utilisateur](keep-the-ui-thread-responsive.md) | Les utilisateurs attendent de leurs applications qu’elles restent réactives pendant les opérations de calcul, et ce sur n’importe quel type d’ordinateur. Les implications sont différentes selon les applications. Pour certaines applications, cela implique de fournir des propriétés physiques plus réalistes, d’accélérer le chargement des données à partir d’un disque ou du Web, de représenter des scènes complexes et naviguer entre les pages plus vite, de trouver un itinéraire en un clin d’œil ou encore de traiter rapidement les données. Quel que soit le type de calcul effectué, les utilisateurs veulent que leur application continue de réagir à leurs interactions et qu’elle ne donne pas l’impression de ne plus répondre pendant l’opération de &quot;calcul&quot;. |
| [Optimiser votre balisage XAML](optimize-xaml-loading.md) | L’analyse du balisage XAML pour la construction d’objets en mémoire est chronophage pour une interface utilisateur complexe. Voici quelques astuces pour améliorer l’analyse du balisage XAML ainsi que l’efficacité du temps de chargement et de la mémoire de votre application. | 
| [Optimiser votre disposition XAML](optimize-your-xaml-layout.md) | La disposition peut s’avérer coûteuse pour une application XAML, tant au niveau de l’utilisation du processeur que de la surcharge de la mémoire. Voici quelques mesures simples que vous pouvez entreprendre pour améliorer les performances de la disposition de votre application XAML. | 
| [Conseils en matière de performances pour MVVM et la langue](mvvm-performance-tips.md) | Cette rubrique décrit certaines considérations en matière de performances liées à vos choix de modèles de conception de logiciel et de langage de programmation. |
| [Meilleures pratiques pour les performances de démarrage de votre application](best-practices-for-your-app-s-startup-performance.md) | Créez des applications UWP dont le temps de démarrage est optimal en améliorant la gestion du lancement et de l’activation. |
| [Optimiser les animations, les médias et les images](optimize-animations-and-media.md) | Créez des applications UWP avec des animations fluides, une fréquence d’images élevée et une capture/lecture multimédia hautement performante. |
| [Optimiser la suspension/reprise](optimize-suspend-resume.md) | Créez des applications UWP qui simplifient l’utilisation du système de gestion de la durée de vie des processus afin d’assurer une reprise efficace après une suspension ou un arrêt. |
| [Optimiser l’accès aux fichiers](optimize-file-access.md) | Créez des applications UWP qui accèdent au système de fichiers efficacement, en évitant les problèmes de performances dus à la latence de disque et aux cycles de la mémoire et du processeur. |
| [Windows Runtime composants et optimisation de l’interopérabilité](windows-runtime-components-and-optimizing-interop.md) | Créez des applications UWP qui utilisent des composants UWP et l’interopérabilité entre les types natifs et managés, tout en évitant les problèmes de performance d’interopérabilité. |
| [Outils pour le profilage et les performances](tools-for-profiling-and-performance.md) | Microsoft fournit plusieurs outils pour vous aider à améliorer les performances de votre application UWP.|

