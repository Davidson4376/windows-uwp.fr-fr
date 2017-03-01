---
author: Mtoepke
title: "Ressources système pour les applications UWP et les jeux sur Xbox One"
description: "UWP sur les ressources système Xbox"
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8d6876ee6235546e74341609a55db995a77323d6
ms.lasthandoff: 02/08/2017

---

# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Ressources système pour les applications UWP et les jeux sur Xbox One

Les applications et les jeux UWP s’exécutant sur Xbox One partagent les ressources avec le système et d’autres applications. Par conséquent, les applications et les jeux UWP ont accès aux ressources suivantes :

* La mémoire maximale disponible pour une application en cours d’exécution au premier plan est de 1 Go.
    * La mémoire maximale disponible pour une application en cours d’exécution en arrière-plan est de 128 Mo.
    * Les applications qui utilisent une quantité de mémoire supérieure à la quantité requise rencontreront des échecs d’allocation de mémoire. Pour en savoir plus sur la surveillance de la mémoire, consultez la documentation de référence sur la [classe MemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx).
    
    > [!NOTE]
    > Lorsque vous exécutez votre application ou votre jeu à partir du débogueur Visual Studio, ces limites de mémoire ne s’appliquent pas. Cette limite s’applique uniquement quand l’exécution n’a pas lieu en mode débogage.

* Partage de 2 à 4 cœurs de processeur en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

* Partage de 45 % du GPU en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

* UWP sur Xbox One prend en charge la fonctionnalité DirectX 11 niveau 10. DirectX 12 n’est pas pris en charge pour l’instant.

* Toutes les applications doivent cibler l'architecture x64 afin de pouvoir être développées ou soumises au Windows Store pour Xbox.  

Pour le **développement d’applications**, il est important de garder à l’esprit que les ressources disponibles peuvent être limitées par rapport à un ordinateur standard.

Pour le **développement de jeux**, il est important de garder à l’esprit que Xbox One, comme les autres consoles de jeu, est un élément matériel spécialisé qui nécessite un kit de développement matériel spécifique pour offrir tout son potentiel. Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel Xbox One, vous pouvez vous inscrire au programme [ID@XBOX](http://www.xbox.com/Developers/id) pour accéder aux kits de développement Xbox One, qui incluent la prise en charge de DirectX 12.

## <a name="see-also"></a>Voir également
- [UWP sur Xbox One](index.md)

