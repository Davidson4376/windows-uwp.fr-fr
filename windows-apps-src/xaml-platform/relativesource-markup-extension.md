---
author: jwmsft
description: Provides a means to specify the source of a binding in terms of a relative relationship in the run-time object graph.
title: RelativeSource markup extension
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
translationtype: Human Translation
ms.sourcegitcommit: ec4c9b87655425e82a1cb792d0acc6bee265e9d2
ms.openlocfilehash: b6af0ce865713ed0da39a87aa63799d3f89b7e89

---

# {RelativeSource} markup extension

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Provides a means to specify the source of a binding in terms of a relative relationship in the run-time object graph.

## XAML attribute usage (Self mode)

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## XAML attribute usage (TemplatedParent mode)

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## XAML values

| Term | Description |
|------|-------------|
| {RelativeSource Self} | Produces a [<strong>Mode</strong>](https://msdn.microsoft.com/library/windows/apps/br209915) value of <strong>Self</strong>. The target element should be used as the source for this binding. This is useful for binding one property of an element to another property on the same element. |
| {RelativeSource TemplatedParent} | Produces a [<strong>ControlTemplate</strong>](https://msdn.microsoft.com/library/windows/apps/br209391) that is applied as the source for this binding. This is useful for applying runtime information to bindings at the template level. | 

## Remarks

A [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) can set [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) either as an attribute on a **Binding** object element or as a component within a [{Binding} markup extension](binding-markup-extension.md). This is why two different XAML syntaxes are shown.

**RelativeSource** is similar to [{Binding} markup extension](binding-markup-extension.md).  It is a markup extension that is capable of returning instances of itself, and supporting a string-based construction that essentially passes an argument to the constructor. In this case, the argument being passed is the [**Mode**](https://msdn.microsoft.com/library/windows/apps/br209915) value.

The **Self** mode is useful for binding one property of an element to another property on the same element, and is a variation on [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828) binding but does not require naming and then self-referencing the element. If you bind one property of an element to another property on the same element, either the properties must use the same property type, or you must also use a [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826) on the binding to convert the values. For example, you could use [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) as a source for [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) without conversion, but you'd need a converter to use [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) as a source for [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006).

Here's an example. This [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) uses a [{Binding} markup extension](binding-markup-extension.md) so that its [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) and [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) are always equal and it renders as a square. Only the Height is set as a fixed value. For this **Rectangle** its default [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) is **null**, not **this**. So to establish the data context source to be the object itself (and enable binding to its other properties) we use the `RelativeSource={RelativeSource Self}` argument in the {Binding} markup extension usage.

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

Another use of `RelativeSource={RelativeSource Self}` is as a way to set an object's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713) to itself.  For example, you may see this technique in some of the SDK examples where the [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) class has been extended with a custom property that's already providing a ready-to-go view model for its own data binding such as: `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**Note**  The XAML usage for **RelativeSource** shows only the usage for which it is intended: setting a value for [**Binding.RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) in XAML as part of a binding expression. Theoretically, other usages are possible if setting a property where the value is [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913).

## Related topics

* [XAML overview](xaml-overview.md)
* [Data binding in depth](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [{Binding} markup extension](binding-markup-extension.md)
* [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209913)




<!--HONumber=Aug16_HO3-->


