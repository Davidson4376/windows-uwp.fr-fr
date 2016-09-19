---
author: jwmsft
description: In XAML markup, specifies a null value for a property.
title: xNull markup extension
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c81acb985c54a8dc799df5ad9c811577777dbf9b

---

# {x:Null} markup extension

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In XAML markup, specifies a **null** value for a property.

## XAML attribute usage

``` syntax
<object property="{x:Null}" .../>
```

## Remarks

**null** is the null reference keyword for C# and C++. The Microsoft Visual Basic keyword for a null reference is **Nothing**.

The initial default value can vary between dependency properties, and it is not necessarily **null**. Further, many dependency properties will not accept **null** as a value (whether through markup or code) due to their internal implementation. In such cases, setting a XAML attribute value with **{x:Null}** can result in a parser exception.

Some Windows Runtime types are nullable. In cases where a nullable type does not already have **null** as the default, you could use **{x:Null}** in XAML to set to the **null** value. If using Visual C++ component extensions (C++/CX), nullable types are represented as [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). If using Microsoft .NET languages, nullable types are represented as [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## Related topics

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 




<!--HONumber=Aug16_HO3-->


