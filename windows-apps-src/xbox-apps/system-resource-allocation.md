---
title: Ressources système pour les applications UWP et les jeux sur Xbox One
description: UWP sur les ressources système Xbox
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 8cc6ca24453be83f5c10cc6c86c508a5a3f99c4c
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131921"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Ressources système pour les applications UWP et les jeux sur Xbox One

Les applications UWP s’exécutant sur Xbox One partagent les ressources avec le système et d’autres applications. Les ressources disponibles à UWP sur une application Xbox One dépendent de votre soumission en tant qu'application ou en tant que jeu Programme créateurs Xbox Live.

* Mémoire maximum disponible en cours d’exécution au premier plan :
    * Applications : 1 Go
    * Jeux : 5 Go

La mémoire maximale disponible pour une application en cours d’exécution en arrière-plan est de 128 Mo. Le mode arrière-plan s'applique uniquement aux applications simultanées, notamment les lecteurs audio en arrière-plan.  Les jeux seront suspendus et terminés en arrière-plan.

Le dépassement de ces limites entraînera des défaillances dans l'allocation de mémoire. Pour en savoir plus sur la surveillance de la mémoire, consultez la documentation de référence sur la [classe MemoryManager](https://docs.microsoft.com/uwp/api/windows.system.memorymanager).

> [!NOTE]
> Lorsque vous exécutez votre application ou votre jeu à partir du débogueur Visual Studio, ces contraintes de mémoire ne s’appliquent pas. Cette limite s’applique uniquement quand l’exécution n’a pas lieu en mode débogage.

* Processeur
    * Applications : partage de 2 à 4 cœurs de processeur en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.
    * Jeux : 4 exclusif et 2 partagé cœurs de processeur.

* Processeur graphique
    * Applications : partage de 45 % du GPU en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.
    * Jeux : accès complet aux cycles de processeur graphique disponibles.

* Prise en charge de DirectX
    * Applications : Niveau de fonctionnalité DirectX 11 10.
    * Jeux : DirectX 12 et le niveau de fonctionnalité DirectX 11 10.

* Toutes les applications et tous les jeux doivent cibler l'architecture x64 afin de pouvoir être développés ou soumis au Store pour Xbox.  

Pour le **développement d’applications**, les ressources disponibles peuvent être limitées par rapport à un PC standard et peuvent varier en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

Pour le **développement de jeux** comme les autres consoles de jeu, Xbox One est un élément matériel spécialisé qui nécessite un kit de développement matériel spécifique pour offrir tout son potentiel. Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel Xbox One, vous pouvez envisager de vous inscrire au programme [ID@Xbox](https://www.xbox.com/Developers/id) pour accéder aux kits de développement Xbox One.

## <a name="see-also"></a>Voir aussi
- [UWP sur Xbox One](index.md)
- [Bien démarrer avec le programme Xbox Live Creators](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-program)
- [DirectX et UWP sur Xbox One](https://walbourn.github.io/)