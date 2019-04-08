---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Liaison de données et MVVM
description: Liaison de données est au cœur du modèle de conception architecturale de l’interface utilisateur de Model-View-ViewModel (MVVM) et permet à couplage lâche entre le code de l’interface utilisateur et non l’interface utilisateur.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616734"
---
# <a name="data-binding-and-mvvm"></a>Liaison de données et MVVM

Model-View-ViewModel (MVVM) est un modèle de conception architecturale de l’interface utilisateur pour le découplage de code de l’interface utilisateur et non l’interface utilisateur. Avec MVVM, vous définissez votre interface utilisateur de façon déclarative dans XAML et utilisez le balisage de liaison de données à lier à d’autres couches contenant des données et des commandes. L’infrastructure de liaison de données fournit un couplage faible qui conserve l’interface utilisateur et les données liées synchronisées et dirige l’entrée d’utilisateur pour les commandes appropriées. 

Car elle fournit un couplage lâche, l’utilisation de la liaison de données réduit les dépendances dures entre différents types de code. Cela rend plus facile modifier les unités de code individuels (méthodes, classes, contrôles, etc.) sans provoquer des effets secondaires involontaires dans d’autres unités. Ce découplage est un exemple de la *séparation des préoccupations*, qui est un concept important dans nombreux modèles de conception. 

## <a name="benefits-of-mvvm"></a>Avantages du modèle MVVM

Découplage de votre code présente de nombreux avantages, notamment :

* Activation d’un style de codage itératif, exploratoire. Modification isolée est moins risquée et plus facile à faire des essais avec.
* Simplification des tests unitaires. Les unités de code qui sont isolées les uns des autres peuvent être testées individuellement et en dehors des environnements de production.
* Prise en charge de la collaboration d’équipe. Code découplée qui adhère à des interfaces bien conçues peut être développée par différentes personnes ou équipes et intégré plus tard.
* Amélioration de la facilité de maintenance. Résolution des bogues dans le code découplé est moins susceptible de causer des régressions dans un autre code.

Par opposition avec MVVM, une application avec une structure « code-behind » plus conventionnelle généralement utilise la liaison de données pour les données d’affichage uniquement et répond aux entrées d’utilisateur en gérant directement les événements exposés par les contrôles. Les gestionnaires d’événements sont implémentées dans les fichiers code-behind (par exemple, MainPage.xaml.cs) et sont souvent étroitement couplés aux contrôles, contenant généralement le code qui manipule l’interface utilisateur directement. Cela rend difficile, voire impossible de remplacer un contrôle sans avoir à mettre à jour le code de gestion des événements. Avec cette architecture, les fichiers code-behind accumulent souvent code qui n’est pas directement lié à l’interface utilisateur, tel que le code d’accès à la base de données, ce qui finit par être dupliqués et modifiée pour être utilisée avec d’autres pages.

## <a name="app-layers"></a>Couches d’application

Lorsque vous utilisez le modèle MVVM, une application est répartie dans les couches suivantes :

* Le **modèle** couche définit les types qui représentent les données de votre entreprise. Cela inclut tous les éléments requis pour modéliser le domaine d’application principal et inclut souvent la logique d’application principale. Cette couche est complètement indépendante de la vue et les couches de modèle de vue et souvent se trouve partiellement dans le cloud. Étant donné une couche de modèle totalement implémentée, vous pouvez créer plusieurs client différentes applications si vous le souhaitez, telles que les applications UWP et web qui fonctionnent avec les mêmes données sous-jacentes.
* Le **vue** couche définit l’interface utilisateur à l’aide de balisage XAML. Le balisage comprend les expressions de liaison de données (tel que [x : Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) qui définissent la connexion entre les composants d’interface utilisateur spécifiques et les différents membres de modèle de vue et model. Fichiers code-behind sont parfois utilisés dans le cadre de la couche view à contiennent du code supplémentaire nécessaire pour personnaliser ou de manipuler l’interface utilisateur, ou pour extraire des données à partir des arguments du Gestionnaire d’événements avant d’appeler une méthode de modèle de vue qui effectue le travail. 
* Le **modèle de vue** couche fournit des cibles de liaison de données pour la vue. Dans de nombreux cas, le modèle de vue expose le modèle directement, ou fournit des membres qui encapsulent les membres du modèle spécifique. Le modèle de vue peut également définir des membres pour assurer le suivi des données qui s’appliquent à l’interface utilisateur, mais pas au modèle, telles que l’ordre d’affichage d’une liste d’éléments. Le modèle de vue sert également de point d’intégration avec d’autres services tel que le code d’accès à la base de données. Pour les projets simples, vous ne devrez pas peut-être une couche de modèle distinct, mais uniquement un vue-modèle qui encapsule toutes les données que vous avez besoin. 

## <a name="basic-and-advanced-mvvm"></a>MVVM de base et Avancé

Comme avec n’importe quel modèle de conception, il existe plusieurs façons pour implémenter le modèle MVVM, et de nombreuses techniques sont considérées comme partie du modèle MVVM. Pour cette raison, il existe plusieurs infrastructures MVVM tierces différents prenant en charge les différentes plateformes basée sur XAML, y compris UWP. Toutefois, ces infrastructures incluent généralement plusieurs services d’application découplé architecture de la définition exacte de MVVM quelque peu ambiguë. 

Bien que des infrastructures MVVM sophistiqués peuvent être très utiles, en particulier pour les projets de l’échelle de l’entreprise, il est généralement un coût associé à l’adoption de n’importe quel modèle particulier ou une technique, et les avantages ne sont pas toujours claires, en fonction de la mise à l’échelle et la taille de votre projet. Heureusement, vous pouvez adopter uniquement ces techniques qui fournissent un avantage clair et tangible et ignorer d’autres jusqu'à ce que vous en avez besoin. 

En particulier, vous pouvez obtenir de nombreux avantages simplement par la compréhension et l’application toute la puissance de la liaison de données et la séparation de votre logique d’application en couches décrits précédemment. Cela est possible en utilisant uniquement les fonctionnalités fournies par le Kit de développement Windows et sans utiliser les infrastructures externes. En particulier, le [extension de balisage {x : Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) facilite la liaison de données et très performantes que dans XAML diverses plates-formes, éliminant le besoin de beaucoup de code boilerplate requis précédemment.

Pour obtenir des conseils supplémentaires sur l’utilisation de base, out-of-the-box MVVM, consultez le [exemple de base de données des commandes clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) sur GitHub. Un grand nombre des autres [exemples d’applications UWP](https://github.com/Microsoft?q=windows-appsample
) également utiliser une architecture MVVM base et le [exemple d’application de trafic](https://github.com/Microsoft/Windows-appsample-trafficapp) inclut le code-behind et les versions MVVM, avec des notes décrivant le [conversion de MVVM ](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Voir également

### <a name="topics"></a>Rubriques

[Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[extension de balisage {x : Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Exemples

[Exemple de base de données de commandes clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Exemple d’inventaire de VanArsdel](https://github.com/Microsoft/InventorySample)  
[Exemple d’application de trafic](https://github.com/Microsoft/Windows-appsample-trafficapp)  
