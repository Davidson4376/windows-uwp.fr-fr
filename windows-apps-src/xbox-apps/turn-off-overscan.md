---
author: payzer
title: "Comment étirer l’IU vers le bord de l’écran"
description: 
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 9a221672391dfbfb4af664438448307800020c6f
ms.lasthandoff: 02/08/2017

---

# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Comment étirer l’IU vers le bord de l’écran   
Par défaut, des bordures sont placées autour de la fenêtre d’affichage de l’application. Elles permettent de tenir compte de la zone adaptée à l’écran de télévision. Pour plus d’informations, voir [Conception pour Xbox et TV](../input-and-devices/designing-for-tv.md#tv-safe-area). 

Nous vous recommandons de désactiver cette fonctionnalité et d’étirer l’IU vers le bord de l’écran. Pour ce faire, ajoutez le code suivant lorsque l’application démarre :
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Les applications C++/DirectX n’ont pas besoin de prendre ce facteur en compte. Le système affiche toujours votre application jusqu’au bord de l’écran.

## <a name="see-also"></a>Voir également
- [Bonnes pratiques pour Xbox](tailoring-for-xbox.md)
- [UWP sur Xbox One](index.md)

