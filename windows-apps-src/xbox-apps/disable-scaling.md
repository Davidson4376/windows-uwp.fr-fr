---
author: payzer
title: "Désactivation de la mise à l’échelle"
description: "Instructions pour la désactivation du facteur d’échelle par défaut."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 908620cd9f8bb3b1155b3e5d1fd777a91d254ef1
ms.lasthandoff: 02/08/2017

---

# <a name="how-to-turn-off-scaling"></a>Désactivation de la mise à l’échelle   
Par défaut, les applications sont mises à une échelle de 200 % pour XAML et de 150 % pour les applications HTML. Il est possible de désactiver le facteur d’échelle par défaut. Votre application utilisera ainsi les dimensions en pixels réelles de l’appareil (1910 x 1080 pixels).   
   
## <a name="html"></a>HTML   
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant : 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

Ou, vous pouvez utiliser une méthode web conviviale :   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant :   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
Les applications DirectX/C++ ne sont pas mises à l’échelle. La mise à l’échelle automatique s’applique uniquement aux applications HTML et XAML.  

## <a name="see-also"></a>Voir également
- [Bonnes pratiques pour Xbox](tailoring-for-xbox.md)
- [UWP sur Xbox One](index.md)

