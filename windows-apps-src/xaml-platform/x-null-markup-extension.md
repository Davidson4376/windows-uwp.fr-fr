---
description: En XAML, un balisage spécifie une valeur null pour une propriété.
title: Extension de balisage xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6ee1f4cb1fa0a97c8d1b8a447ccc15cd0b51776
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340452"
---
# <a name="xnull-markup-extension"></a>Extension de balisage {x:Null}


En XAML, un balisage spécifie une valeur **null** pour une propriété.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Notes

**null** est le mot clé de référence Null pour C# et C++. Le mot clé Microsoft Visual Basic pour une référence Null est **Nothing**.

La valeur par défaut initiale peut varier selon les propriétés de dépendance, et n’est pas nécessairement **null**. De plus, de nombreuses propriétés de dépendance n’acceptent pas **null** en tant que valeur (via le balisage ou le code), en raison de leur implémentation interne. Le cas échéant, la définition d’une valeur d’attribut XAML avec **{x:Null}** peut engendrer une exception d’analyse.

Certains types Windows Runtime ont la valeur Nullable. Quand un type Nullable n’a pas déjà la valeur **null** comme valeur par défaut, vous pouvez utiliser **{x:Null}** pour définir la valeur **null** en XAML. Si vous utilisez C++ des extensions deC++composants visuels (/CX), les types Nullable sont représentés en tant que [Platform :: iBox @ no__t-4](https://docs.microsoft.com/cpp/cppcx/platform-ibox-interface). Si vous utilisez des langages Microsoft .NET, les types Nullable sont représentés par [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1).

## <a name="related-topics"></a>Rubriques connexes

* [**Nullable @ no__t-2**](https://docs.microsoft.com/dotnet/api/system.nullable-1)
* [**IReference @ no__t-2**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_)
 

