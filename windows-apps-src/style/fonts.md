---
author: Jwmsft
Description: Follow these guidelines when selecting fonts and specifying font sizes and colors for UWP apps.
title: Fonts for UWP apps
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: d7236006f2c620a4ff0de4e0f413f32a2eaf5687
ms.openlocfilehash: b79a6f3ee32494f04fa472c0531c06aa0a60098b

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# Fonts for UWP apps

This article lists the recommended fonts for UWP apps. These fonts are guaranteed to be available in all Windows 10 editions that support UWP apps.

<div class="important-apis" >
<b>Important APIs</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br209655"><strong>FontFamily property</strong></a></li>
</ul>

</div>
</div>



The [UWP typography guide](typography.md) recommends that apps use the Segoe UI font, and although Segoe UI is a great choice for most apps, you don't have to use it for everything. You might use other fonts for certain scenarios, such as reading, or when displaying text in certain non-English languages. 



 
## Sans-serif fonts

Sans-serif fonts are a great choice for headings and UI elements. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Font-family</th>
<th align="left">Styles</th>
<th align="left">Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Regular, Italic, Bold, Bold Italic, Black</td>
<td align="left">Supports European and Middle Eastern scripts (Latin, Greek, Cyrillic, Arabic, Armenian, and Hebrew) Black weight supports European scripts only.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Regular, Italic, Bold, Bold Italic, Light, Light Italic</td>
<td align="left">Supports European and Middle Eastern scripts (Latin, Greek, Cyrillic, Arabic and Hebrew). Arabic available in the uprights only.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Regular, Italic, Bold, Bold Italic</td>
<td>Fixed width font that supports European scripts (Latin, Greek and Cyrillic).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Regular, Italic, Light Italic, Black Italic, Bold, Bold Italic, Light, Semilight, Semibold, Black</td>
<td>User-interface font for European and Middle East scripts (Arabic, Armenian, Cyrillic, Georgian, Greek, Hebrew, Latin), and also Lisu script.</td>
</tr>

<tr class="odd">
<td>Segoe UI Historic</td>
<td align="left">Regular</td>
<td align="left">Fallback font for historic scripts</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Regular, Semilight, Light, Bold, Semibold</td>
<td align="left">An open-source font that's metrically compatible with Segoe UI, intended for apps on other platforms that don’t want to bundle Segoe UI. [Get Selawik on GitHub.](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">Regular, Italic, Bold, Bold Italic</td>
<td align="left">Supports European scripts (Latin, Greek, Cyrillic and Armenian).</td>
</tr>

</tbody>
</table>


## Serif fonts

Serif fonts are good for presenting large amounts of text. 

<table>
<thead>
<tr class="header">
<th align="left">Font-family</th>
<th align="left">Styles</th>
<th align="left">Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Regular</td>
<td align="left">Serif font that supports European scripts (Latin, Greek, Cyrillic).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Regular, Italic, Bold, Bold Italic</td>
<td align="left">Serif fixed width font supports European and Middle Eastern scripts (Latin, Greek, Cyrillic, Arabic, Armenian, and Hebrew).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgia</td>
<td align="left">Regular, Italic, Bold, Bold Italic</td>
<td align="left">Supports European scripts (Latin, Greek and Cyrillic).</td>
</tr>


<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Regular, Italic, Bold, Bold Italic</td>
<td align="left">Legacy font that supports European scripts (Latin, Greek, Cyrillic, Arabic, Armenian, Hebrew).</td>
</tr>

</tbody>
</table>

## Symbols and icons


<table>
<thead>
<tr class="header">
<th align="left">Font-family</th>
<th align="left">Styles</th>
<th align="left">Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Regular</td>
<td align="left">User-interface font for app icons. For more info, see the [Segoe MDL2 assets article](segoe-ui-symbol-font.md).</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">Fallback font for symbols</td>
</tr>
</tbody>
</table>



## Fonts for non-Latin languages

Although many of these fonts provide Latin characters.

<table>
<thead>
<tr class="header">
<th align="left">Font-family</th>
<th align="left">Styles</th>
<th align="left">Notes</th>
</tr>
</thead>
<tbody>

<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Regular, Bold</td>
<td align="left">User-interface font for African scripts (Ethiopic, N'Ko, Osmanya, Tifinagh, Vai).</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Regular, Bold</td>
<td align="left">User-interface font for North American scripts (Canadian Syllabics, Cherokee).</td>
</tr>
<tr class="even">
<td align="left">Javanese Text Regular Fallback font for Javanese script</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Javanese script</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">Regular, Semilight, Bold</td>
<td align="left">User-interface font for Southeast Asian scripts (Buginese, Lao, Khmer, Thai).</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">User-interface font for Korean.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Tibetan script.</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Regular, Bold, Light</td>
<td align="left">User-interface font for Traditional Chinese.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft New Tai Lue</td>
<td align="left">Regular</td>
<td align="left">Fallback font for New Tai Lue script.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Phags-pa script.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Tai Le script.</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Regular, Bold, Light</td>
<td align="left">User-interface font for Simplified Chinese.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Yi script.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolian Baiti</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Mongolian script.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Thaana script.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Regular</td>
<td align="left">Fallback font for Myanmar script.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Regular, Semilight, Bold</td>
<td align="left">User-interface font for South Asian scripts (Bangla, Devanagari, Gujarati, Gurmukhi, Kannada, Malayalam, Odia, Ol Chiki, Sinhala, Sora Sompeng, Tamil, Telugu)</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Regular</td>
<td align="left">A legacy Chinese UI font. </td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">Medium</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Regular</td>
<td align="left">User-interface font for Japanese.</td>
</tr>
</tbody>
</table>


## Globalizing/localizing fonts
Use the [LanguageFont font-mapping APIs](https://msdn.microsoft.com/library/windows/apps/br206864) for programmatic access to the recommended font family, size, weight, and style for a particular language. The LanguageFont object provides access to the correct font info for various categories of content including UI headers, notifications, body text, and user-editable document body fonts. For more info, see [Adjusting layout and fonts to support globalization](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl).

<!--
## Triggering a font download
If you use a font that's not listed in this article, your app might trigger an automatic download of the font data from a Microsoft service. This can have performance and other impacts that may be a concern, particularly for mobile devices. In particular, note that this might consume some of a user's mobile data plan or result in mobile data usage costs. UWP apps that will available on mobile devices should never use fonts for UI content other than fonts in this list.
-->

## Get the samples

* [Downloadable fonts sample](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [UI basics sample](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Line spacing with DirectWrite sample](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## Related articles

* [Adjusting layout and fonts to support globalization](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [Text controls)](../controls-and-patterns/text-controls.md)
* [XAML theme resources](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 







<!--HONumber=Aug16_HO3-->


