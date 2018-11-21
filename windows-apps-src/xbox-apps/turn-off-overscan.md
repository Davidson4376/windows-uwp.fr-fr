---
author: payzer
title: Comment étirer l’IU vers le bord de l’écran
description: Comment désactiver la mise à l’échelle automatique pour la zone sans titre.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 32932845db47ed47b7e80f68cf4e424ba97e85c0
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7558135"
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
