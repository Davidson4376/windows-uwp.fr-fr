---
author: mijacobs
Description: Use pointer animations to provide users with visual feedback when the user taps on an item.
title: Animations de clic avec le pointeur dans les applications UWP
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.author: jimwalk
ms.date: 08/9/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c54218582b3ef31df07a5969ad5e83be7af2952
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5768955"
---
# <a name="pointer-click-animations"></a>Animations de clic avec le pointeur



Utilisez les animations de pointeur pour fournir un retour visuel lorsqu’un utilisateur appuie sur un élément. L’animation de pointeur vers le bas réduit et incline légèrement l’élément sélectionné. Elle est lue lorsque l’utilisateur appuie pour la première fois sur un élément. L’animation de pointeur vers le haut, qui restaure l’élément à sa position d’origine, est lue quand l’utilisateur relâche le pointeur.


> **API importantes**: [**classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168), [**classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

-   Lorsque vous utilisez une animation de pointeur vers le haut, déclenchez immédiatement l’animation dès que l’utilisateur relâche le pointeur. Ceci permet de fournir un retour instantané à l’utilisateur, qui sait ainsi que son action a été reconnue, même si l’action déclenchée par l’appui (telle que la navigation vers une nouvelle page) est plus lente à se produire.

## <a name="related-articles"></a>Articles connexes

* [Vue d’ensemble des animations](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animation de clics de pointeur](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [Démarrage rapide: Animation de votre interface utilisateur avec des animations de la bibliothèque](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**Classe PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**Classe PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




