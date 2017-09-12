---
author: tbd
description: "Le balisage XAML spécifie un mode par défaut pour x:Bind"
title: Extension de balisage xDefaultBindMode
ms.assetid: 
ms.author: 
ms.date: 
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 0fa037b4c59566cb1b9bacd4d2e36520a86c508d
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2017
---
# <a name="xdefaultbindmode-markup-extension"></a>Extension de balisage {x:DefaultBindMode}

\[ Article mis à jour pour les applications UWP sur Windows10. Pour lire des articles sur Windows8.x, consultez l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Le balisage XAML spécifie un mode par défaut pour x:Bind.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Remarques

Le mode par défaut défini pour x:Bind est OneTime. Ce mode a été choisi pour des raisons de performances, étant donné que l'utilisation de OneTime générera davantage de code pour effectuer la liaison et gérer la détection de modification. **x:DefaultBindMode** peut servir à modifier le mode par défaut défini pour x:Bind pour un segment spécifique de l’arborescence du balisage. Le mode sélectionné appliquera sur cet élément et ses enfants toute expression x:Bind qui ne spécifie pas explicitement un mode dans le cadre de la liaison.

## <a name="related-topics"></a>Rubriques associées

* [**x:Bind**](https://docs.microsoft.com/en-us/windows/uwp/xaml-platform/x-bind-markup-extension)
