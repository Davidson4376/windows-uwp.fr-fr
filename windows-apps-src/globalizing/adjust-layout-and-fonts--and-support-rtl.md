---
Description: Description: Develop your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction.
title: title: Adjust layout and fonts, and support RTL
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
---

# ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3


label: Adjust layout and fonts, and support RTL template: detail.hbs


Adjust layout and fonts, and support RTL

## \[ Updated for UWP apps on Windows 10.


For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \] Develop your app to support the layouts and fonts of multiple languages, including RTL (right-to-left) flow direction. <span id="Layout_guidelines"></span><span id="layout_guidelines"></span><span id="LAYOUT_GUIDELINES"></span>Layout guidelines

Some languages, such as German and Finnish, require more space than English for their text. The fonts for some languages, such as Japanese, require more height.

### And some languages, such as Arabic and Hebrew, require that text layout and app layout must be in right-to-left (RTL) reading order.

Use flexible layout mechanisms instead of absolute positioning, fixed widths, or fixed heights.

```XAML
<TextBlock x:Uid="Block1">
```

When necessary, particular UI elements can be adjusted based on language.

<span id="XAML"></span><span id="xaml"></span>XAML

Specify a **Uid** for an element: Ensure that your app's ResW file has a resource for Block1.Width, which you can set for each language that you localize into.

For Windows Store apps using C++, C\#, or Visual Basic, use the [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) property, with symmetrical padding and margins, to enable localization for other layout directions.

```XAML
<Page x:Uid="MainPage">
```

XAML layout controls such as [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) scale and flip automatically with the [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) property.

### Expose your own **FlowDirection** property in your app as a resource for localizers.

Specify a **Uid** for the main page of your app: Ensure that your app's **ResW** file has a resource for MainPage.FlowDirection, which you can set for each language that you localize into.

<span id="HTML"></span><span id="html"></span>HTML For Windows Store apps using JavaScript, use [Cascading Style Sheets (CSS)](https://msdn.microsoft.com/library/ms531209) layout mechanisms such as [-ms-grid](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#g_section) and [–ms-box](https://msdn.microsoft.com/en-us/library/windows/apps/hh465453.aspx#f_section).

**Use symmetrical padding and margins to enable localization for various layout directions.**
```CSS
.item:-ms-lang(de, fi) { width: 350px; }
```

Your app can also use the [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) pseudo-class selector to adjust CSS properties such as width on particular elements based on the language of the app. To enable this, the App Host sets the root element's **lang** attribute to the app language.

**CSS**
```CSS
body:-ms-lang(ar,he…) { direction: rtl;}
```

Windows Store apps using JavaScript that use the ui-light.css or ui-dark.css style sheets have their body layout direction set automatically, based on the app language.

The following CSS is in ui-light and ui-dark.css, and you don't need to write it yourself. CSS

This means that most app layouts are set correctly when the system uses a right-to-left language. Like [WinJS.UI](https://msdn.microsoft.com/library/windows/apps/br229782) controls, your app can use the [**:-ms-lang()**](https://msdn.microsoft.com/library/cc848867) pseudo-class selector to adjust physical CSS properties, such as **margin** and **padding**.

You don't need to adjust logical CSS properties that use keywords such as **after** and **before**.

## Don't use the **align** property or attribute in HTML.


### Instead, use the **direction** property to control alignment of particular components.

Use the [**writing-mode**](https://msdn.microsoft.com/library/ms531187) property to support vertical text layouts in CSS.

```XAML
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

### <span id="Mirroring_images"></span><span id="mirroring_images"></span><span id="MIRRORING_IMAGES"></span>Mirroring images

<span id="XAML"></span><span id="xaml"></span>XAML

```CSS
.mirrorable { transform: scaleX(-1); }
```

If your app has images that must be mirrored (that is, the same image can be flipped) for RTL, you can apply the [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) property: <span id="HTML"></span><span id="html"></span>HTML If your app has images that must be mirrored (that is, the same image can be flipped) for RTL, you can use CSS transforms to mirror your images at rendering time by adding a .mirrorable class to your elements and adding the following CSS class:

## **For both XAML and HTML:** If your app requires a different image to flip the image correctly, you can use the resource management system with the [layoutdir qualifier](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).


The system chooses an image named file.layoutdir-rtl.png when the [application language](manage-language-and-region.md) is set to an RTL language. This approach may be necessary when some part of the image is flipped, but another part isn't.

### <span id="Fonts"></span><span id="fonts"></span><span id="FONTS"></span>Fonts

**For both XAML and HTML:** Use the [**LanguageFont**](https://msdn.microsoft.com/library/windows/apps/br206864) font-mapping APIs for programmatic access to the recommended font family, size, weight, and style for a particular language. The **LanguageFont** object provides access to the correct font info for various categories of content including UI headers, notifications, body text, and user-editable document body fonts.

<span id="HTML"></span><span id="html"></span>HTML Windows Store apps using JavaScript that use the ui-light.css or ui-dark.css style sheets have their font set automatically to the most appropriate font, based on the app language.

 

 



<!--HONumber=Mar16_HO4-->
