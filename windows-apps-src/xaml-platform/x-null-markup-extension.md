---
description: En XAML, un balisage spécifie une valeur null pour une propriété.
title: Extension de balisage xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f0d6fd8f194a3c9c98fb969034cab5a3e9e2f0de
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8323513"
---
# <a name="xnull-markup-extension"></a>Extension de balisage {x:Null}


En XAML, un balisage spécifie une valeur **null** pour une propriété.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Remarques

**null** est le mot clé de référence Null pour C# et C++. Le mot clé Microsoft Visual Basic pour une référence Null est **Nothing**.

La valeur par défaut initiale peut varier selon les propriétés de dépendance, et n’est pas nécessairement **null**. De plus, de nombreuses propriétés de dépendance n’acceptent pas **null** en tant que valeur (via le balisage ou le code), en raison de leur implémentation interne. Le cas échéant, la définition d’une valeur d’attribut XAML avec **{x:Null}** peut engendrer une exception d’analyse.

Certains types Windows Runtime ont la valeur Nullable. Quand un type Nullable n’a pas déjà la valeur **null** comme valeur par défaut, vous pouvez utiliser **{x:Null}** pour définir la valeur **null** en XAML. Si vous utilisez des extensions de composant Visual c++ (C++ / CX), les types nullables sont représentés par [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Si vous utilisez des langages Microsoft .NET, les types Nullable sont représentés par [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## <a name="related-topics"></a>Rubriques connexes

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 

