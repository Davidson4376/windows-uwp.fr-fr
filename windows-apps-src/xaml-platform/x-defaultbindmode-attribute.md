---
author: jwmsft
description: Le balisage XAML spécifie un mode par défaut pour x:Bind.
title: Attribut xDefaultBindMode
ms.author: jimwalk
ms.date: 02/08/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2696cb46591757421795b15083ea7fdab54943c5
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6457266"
---
# <a name="xdefaultbindmode-attribute"></a>Attribut x:DefaultBindMode

Le balisage XAML spécifie un mode par défaut pour x:Bind.

L’attribut **x:DefaultBindMode** est disponible à partir de la version 1607 de Windows10 (mise à jour anniversaire) et de la version 14393 du kit SDK.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Notes

[x:Bind](x-bind-markup-extension.md) dispose d'un mode par défaut dont la valeur est **OneTime **. Ce mode a été choisi pour des raisons de performances, étant donné que l'utilisation de **OneWay** génère davantage de code pour effectuer la liaison et gérer la détection de modification. Vous pouvez utiliser **x:DefaultBindMode** pour modifier le mode par défaut défini pour x:Bind pour un segment spécifique de l’arborescence du balisage. Le mode spécifié applique sur cet élément et ses enfants toute expression x:Bind qui ne spécifie pas explicitement un mode dans le cadre de la liaison.

## <a name="related-topics"></a>Rubriques associées

* [Extension de balisage x:Bind](x-bind-markup-extension.md)
