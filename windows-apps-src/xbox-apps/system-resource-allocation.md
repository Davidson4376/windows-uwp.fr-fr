---
author: Mtoepke
title: "Ressources système pour les applications UWP et les jeux sur XboxOne"
description: "UWP sur les ressources système Xbox"
area: Xbox
ms.sourcegitcommit: 6a34b0f657fc787eaa3be691b69a591cfdb2a669
ms.openlocfilehash: 79c47bbcf33b1493a8a961b800932ce6be021453

---

# Ressources système pour les applications UWP et les jeux sur XboxOne

Les applications et les jeux UWP s’exécutant sur XboxOne partagent les ressources avec le système et d’autres applications. Par conséquent, les applications et les jeux UWP ont accès aux ressources suivantes:

* Dans cette version d’évaluation, la mémoire maximale disponible pour une application en cours d’exécution au premier plan est d’1Go.
    * La mémoire maximale disponible pour une application en cours d’exécution en arrière-plan est de 128Mo.
    * Les applications qui utilisent une quantité de mémoire supérieure à la quantité requise rencontreront des échecs d’allocation de mémoire. Pour en savoir plus sur la surveillance de la mémoire, consultez la documentation de référence sur la [classeMemoryManager](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.memorymanager.aspx).
    * **Remarque:** &nbsp;&nbsp;lorsque vous exécutez votre application ou votre jeu à partir du débogueur VisualStudio, ces limites de mémoire ne s’appliquent pas. Cette limite s’applique uniquement quand l’exécution n’a pas lieu en mode débogage.

* Partage de 2 à 4 cœurs de processeur en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

* Partage de 45% du GPU en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

* UWP sur XboxOne prend en charge la fonctionnalité DirectX11 niveau10. DirectX12 n’est pas pris en charge pour l’instant. 

Pour le **développement d’applications**, il est important de garder à l’esprit que les ressources disponibles peuvent être limitées par rapport à un ordinateur standard.

Pour le **développement de jeux**, il est important de garder à l’esprit que Xbox One, comme les autres consoles de jeu, est un élément matériel spécialisé qui nécessite un kit de développement matériel spécifique pour offrir tout son potentiel. Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel XboxOne, vous pouvez vous inscrire auprès du programme [ID@XBOX](http://www.xbox.com/en-us/Developers/id) pour accéder aux kits de développement Xbox One, qui incluent la prise en charge de DirectX12.



<!--HONumber=Jun16_HO5-->


