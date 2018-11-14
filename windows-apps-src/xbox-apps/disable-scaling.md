---
author: payzer
title: Comment désactiver la mise à l’échelle
description: Instructions pour la désactivation du facteur d’échelle par défaut.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 82b42b25d3894a82e92af9a520ee5f951a5ba344
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6648221"
---
# <a name="how-to-turn-off-scaling"></a>Désactivation de la mise à l’échelle   
Par défaut, les applications sont mises à une échelle de 200% pour XAML et de 150% pour les applications HTML. Il est possible de désactiver le facteur d’échelle par défaut. Votre application utilisera ainsi les dimensions en pixels réelles de l’appareil (1910x1080pixels).   
   
## <a name="html"></a>HTML   
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant: 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

Ou, vous pouvez utiliser une méthode web conviviale:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant:   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
Les applications DirectX/C++ ne sont pas mises à l’échelle. La mise à l’échelle automatique s’applique uniquement aux applications HTML et XAML.  

## <a name="see-also"></a>Voir également
- [Bonnes pratiques pourXbox](tailoring-for-xbox.md)
- [UWP sur XboxOne](index.md)
