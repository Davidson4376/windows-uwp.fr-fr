---
author: jwmsft
description: Provides a unique identifier for markup elements. For Universal Windows Platform (UWP) XAML, this unique identifier is used by XAML localization processes and tools, such as using resources from a .resw resource file.
title: xUid directive
ms.assetid: 9FD6B62E-D345-44C6-B739-17ED1A187D69
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c94db1dc1095f53dc836f78768ecc826470343d5

---

# x:Uid directive

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Provides a unique identifier for markup elements. For Universal Windows Platform (UWP) XAML, this unique identifier is used by XAML localization processes and tools, such as using resources from a .resw resource file.

## XAML attribute usage

``` syntax
<object x:Uid="stringID".../>
```

## XAML values

| Term | Description |
|------|-------------|
| stringID | A string that uniquely identifies a XAML element in an app, and becomes part of the resource path in a resource file. See Remarks.| 

## Remarks

Use **x:Uid** to identify an object element in your XAML. Typically this object element is an instance of a control class or other element that is displayed in a UI. The relationship between the string you use in **x:Uid** and the strings you use in a resources file is that the resource file strings are the **x:Uid** followed by a dot (.) and then by the name of a specific property of the element that's being localized. Consider this example:

``` syntax
<Button x:Uid="GoButton" Content="Go"/>
```

To specify content to replace the display text **Go**, you must specify a new resource that comes from a resource file. Your resource file should contain an entry for the resource named "GoButton.Content". [**Content**](https://msdn.microsoft.com/library/windows/apps/br209366) in this case is a specific property that's inherited by the [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) class. You might also provide localized values for other properties of this button, for example you could provide a resource-based value for "GoButton.FlowDirection". For more info on how to use **x:Uid** and resource files together, see [Quickstart: Translating UI resources](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329).

The validity of which strings can be used for an **x:Uid** value is controlled in a practical sense by which strings are legal as an identifier in a resource file and a resource path.

**x:Uid** is discrete from **x:Name** both because of the stated XAML localization scenario, and so that identifiers that are used for localization have no dependencies on the programming model implications of **x:Name**. Also, **x:Name** is governed by the XAML namescope concept, whereas uniqueness for **x:Uid** is controlled by the package resource index (PRI) system. For more info, see [Resource Management System](https://msdn.microsoft.com/library/windows/apps/jj552947).

UWP XAML has somewhat different rules for **x:Uid** uniqueness than previous XAML-utilizing technologies used. For UWP XAML it is legal for the same **x:Uid** ID value to exist as a directive on multiple XAML elements. However, each such element must then share the same resolution logic when resolving the resources in a resource file. Also, all XAML files in a project share a single resource scope for purposes of **x:Uid** resolution, there is no concept of **x:Uid** scopes being aligned to individual XAML files.

In some cases you'll be using a resource path rather than built-in functionality of the package resource index (PRI) system. Any string used as an **x:Uid** value defines a resource path that begins with ms-resource:///Resources/ and includes the **x:Uid** string. The path is completed by the names of the properties you specify in a resources file or are otherwise targeting.

Don't put **x:Uid** on property elements, that isn't allowed in Windows Runtime XAML.




<!--HONumber=Aug16_HO3-->


