---
author: payzer
title: "Comment désactiver la mise à l’échelle"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 192de32bf3afd11cd375655ad92d194ccb09dae1
ms.openlocfilehash: 307606bc290e9c5268fc5a37b72770d6b1ada4da

---

# Comment désactiver la mise à l’échelle   
Par défaut, les applications sont mises à une échelle de 200% pour XAML et de 150% pour les applications HTML. Il est possible de désactiver le facteur d’échelle par défaut. Votre application utilisera ainsi les dimensions en pixels réelles de l’appareil (1910x1080pixels).   
   
## HTML   
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant: 
   
`var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);` 

Ou, vous pouvez utiliser une méthode web conviviale:   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## XAML
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant:   
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);`   
   
## DirectX/C++   
Les applications DirectX/C++ ne sont pas mises à l’échelle. La mise à l’échelle automatique s’applique uniquement aux applications HTML et XAML.   



<!--HONumber=Jul16_HO1-->


