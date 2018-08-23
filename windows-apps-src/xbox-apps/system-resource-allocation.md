---
author: Mtoepke
title: Ressources système pour les applications UWP et les jeux sur XboxOne
description: UWP sur les ressources système Xbox
ms.author: mstahl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: 8d6ebe8e3344f5276939471d7ac569ae83d48311
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2814131"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Ressources système pour les applications UWP et les jeux sur Xbox One

Les applications UWP s’exécutant sur XboxOne partagent les ressources avec le système et d’autres applications. Les ressources disponibles à UWP sur une application XboxOne dépendent de votre soumission en tant qu'application ou en tant que jeu Programme créateurs XboxLive.

* Mémoire maximum disponible en cours d’exécution au premier plan:
    * Applications: 1Go
    * Jeux: 5Go

La mémoire maximale disponible pour une application en cours d’exécution en arrière-plan est de 128Mo. Le mode arrière-plan s'applique uniquement aux applications simultanées, notamment les lecteurs audio en arrière-plan.  Les jeux seront suspendus et terminés en arrière-plan.

Le dépassement de ces limites entraînera des défaillances dans l'allocation de mémoire. Pour en savoir plus sur la surveillance de l'utilisation de la mémoire, consultez la documentation de référence sur la [ClasseMemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx).
    
    > [!NOTE]
    > When running your app or game from the Visual Studio debugger, these memory constraints do not apply. This limit is only applicable when not running in debugging mode.

* CPU
    * Applications: partage de 2 à 4 cœurs de processeur en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.
    * Jeux: 4 cœurs de processeur exclusifs et 2 cœurs de processeur partagés.

* Processeur graphique
    * Applications: partage de 45% du GPU en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.
    * Jeux: accès complet aux cycles de processeur graphique disponibles.

* Prise en charge de DirectX
    * Applications: Niveau de fonctionnalité10 de DirectX11.
    * Jeux: DirectX12 et DirectX11 avec le niveau de fonctionnalité10.

* Toutes les applications et tous les jeux doivent cibler l'architecture x64 afin de pouvoir être développés ou soumis au Store pour Xbox.  

Pour le **développement d’applications**, les ressources disponibles peuvent être limitées par rapport à un PC standard et peuvent varier en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

Pour le **développement de jeux** comme les autres consoles de jeu, XboxOne est un élément matériel spécialisé qui nécessite un kit de développement matériel spécifique pour offrir tout son potentiel. Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel XboxOne, vous pouvez envisager de vous inscrire au programme [ID@Xbox](http://www.xbox.com/Developers/id) pour accéder aux kits de développement Xbox One.


Pour plus d'informations sur les ressources système pour les applicationsUWP sur XboxOne, consultez la première partie de cette vidéo.
</br>
</br>
<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developing-xbox-one-applications-16860/Video-What-s-Unique--vk0fOPf9C_2006218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="see-also"></a>Articles associés
- [Plateforme UWP sur XboxOne](index.md)
- [Prise en main du programme Créateurs Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)
- [DirectX et UWP sur Xbox 1](https://blogs.msdn.microsoft.com/chuckw/2017/12/15/directx-and-uwp-on-xbox-one/)

