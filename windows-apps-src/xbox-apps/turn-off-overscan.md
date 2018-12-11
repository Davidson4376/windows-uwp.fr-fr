---
title: Comment étirer l’IU vers le bord de l’écran
description: Comment désactiver la mise à l’échelle automatique pour la zone sans titre.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 1ac49d80f1d99a56eff565a0daa8f2f3e9289636
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8891103"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>Comment étirer l’IU vers le bord de l’écran   
Par défaut, des bordures sont placées autour de la fenêtre d’affichage de l’application. Elles permettent de tenir compte de la zone adaptée à l’écran de télévision. Pour plus d’informations, voir [Conception pour Xbox et TV](../design/devices/designing-for-tv.md#tv-safe-area). 

Nous vous recommandons de désactiver cette fonctionnalité et d’étirer l’IU vers le bord de l’écran. Pour ce faire, ajoutez le code suivant lorsque l’application démarre:
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> Les applications C++/DirectX n’ont pas besoin de prendre ce facteur en compte. Le système affiche toujours votre application jusqu’au bord de l’écran.

## <a name="see-also"></a>Voir également
- [Bonnes pratiques pourXbox](tailoring-for-xbox.md)
- [UWP sur XboxOne](index.md)
