---
author: jwmsft
description: Modifies XAML compilation behavior, such that fields for named object references are defined with public access rather than the private default behavior.
title: xFieldModifier attribute
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
translationtype: Human Translation
ms.sourcegitcommit: 3144758352b99f8c145a3c7be8a6c43d6a002104
ms.openlocfilehash: f812c9498d5519aac8ab750f0c55423966a63464

---

# x:FieldModifier attribute

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Modifies XAML compilation behavior, such that fields for named object references are defined with **public** access rather than the **private** default behavior.

## XAML attribute usage

``` syntax
<object x:FieldModifier="public".../>
```

## Dependencies

[x:Name attribute](x-name-attribute.md) must also be provided on the same element.

## Remarks

The value for the **x:FieldModifier** attribute will vary by programming language. Valid values are **private**, **public**, **protected**, **internal** or **friend**. For C#, Microsoft Visual Basic or Visual C++ component extensions (C++/CX), you can give the string value "public" or "Public"; the parser doesn't enforce case on this attribute value.

**Private** access is the default.

**x:FieldModifier** is only relevant for elements with an [x:Name attribute](x-name-attribute.md), because that name is used to reference the field once it is public.

**Note**  Windows Runtime XAML doesn't support **x:ClassModifier** or **x:Subclass**.




<!--HONumber=Aug16_HO3-->


