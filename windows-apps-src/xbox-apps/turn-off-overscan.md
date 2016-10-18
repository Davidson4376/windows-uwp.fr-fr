---
author: payzer
title: "Comment étirer l’IU vers le bord de l’écran"
description: 
translationtype: Human Translation
ms.sourcegitcommit: b5961d3266a031ab09a9da63319e9883cf050789
ms.openlocfilehash: cddde27a17e897ab8a68bbed099e532a8cd48f07

---

# Comment étirer l’IU vers le bord de l’écran   
Par défaut, des bordures sont placées autour de la fenêtre d’affichage de l’application. Elles permettent de tenir compte de la zone adaptée à l’écran de télévision. Pour plus d’informations, voir [Conception pour Xbox et TV](../input-and-devices/designing-for-tv.md#tv-safe-area). 

Nous vous recommandons de désactiver cette fonctionnalité et d’étirer l’IU vers le bord de l’écran. Pour ce faire, ajoutez le code suivant lorsque l’application démarre:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Les applications C++/DirectX n’ont pas besoin de prendre ce facteur en compte. Le système affiche toujours votre application jusqu’au bord de l’écran.

## Voir également
- [Bonnes pratiques pourXbox](tailoring-for-xbox.md)
- [UWP sur XboxOne](index.md)



<!--HONumber=Aug16_HO3-->


