---
author: mtoepke
title: Port from OpenGL ES 2.0 to Direct3D 11
description: Includes articles, overviews, and walkthroughs for porting an OpenGL ES 2.0 graphics pipeline to a Direct3D 11 and the Windows Runtime.
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
translationtype: Human Translation
ms.sourcegitcommit: 814f056eaff5419b9c28ba63cf32012bd82cc554
ms.openlocfilehash: aab0c3e9f3816e0657dfb6fec4917d62f2be5280

---

# Port from OpenGL ES 2.0 to Direct3D 11


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Includes articles, overviews, and walkthroughs for porting an OpenGL ES 2.0 graphics pipeline to a Direct3D 11 and the Windows Runtime.

> **Note**   An intermediate step to porting your OpenGL ES 2.0 project is to use ANGLE for Windows Store. ANGLE allows you to run OpenGL ES content on Windows by translating OpenGL ES API calls to DirectX 11 API calls. For more information about ANGLE, go to the [ANGLE for Windows Store Wiki](http://go.microsoft.com/fwlink/p/?linkid=618387).

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Topic</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Map OpenGL ES 2.0 to Direct3D 11.1](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>When starting the process of porting your graphics architecture from OpenGL ES 2.0 to Direct3D for the first time, familiarize yourself with the key differences between the APIs. The topics in this section help you plan your port strategy and the API changes that you must make when moving your graphics processing to Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[How to: port a simple OpenGL ES 2.0 renderer to Direct3D 11.1](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)</p></td>
<td align="left"><p>For this porting exercise, we'll start with the basics: bringing a simple renderer for a spinning, vertex-shaded cube from OpenGL ES 2.0 into Direct3D, such that it matches the DirectX 11 App (Universal Windows) template from Visual Studio 2015.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[OpenGL ES 2.0 to Direct3D 11.1 reference](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>Use these reference topics to look up API mapping and short code samples when porting from OpenGL ES 2.0 to Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

> **Note**  
This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 







<!--HONumber=Aug16_HO3-->


