---
author: mtoepke
title: Walkthrough-- Implement shadow volumes using depth buffers in Direct3D 11
description: This walkthrough demonstrates how to render shadow volumes using depth maps, using Direct3D 11 on devices of all Direct3D feature levels.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: a323c299d588cdcff7b83d538a705d64207c96b2

---

# Walkthrough: Implement shadow volumes using depth buffers in Direct3D 11


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

This walkthrough demonstrates how to render shadow volumes using depth maps, using Direct3D 11 on devices of all Direct3D feature levels.
## 
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
<td align="left"><p>[Create depth buffer device resources](create-depth-buffer-resource--view--and-sampler-state.md)</p></td>
<td align="left"><p>Learn how to create the Direct3D device resources necessary to support depth testing for shadow volumes.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Render the shadow map to the depth buffer](render-the-shadow-map-to-the-depth-buffer.md)</p></td>
<td align="left"><p>Render from the point of view of the light to create a two-dimensional depth map representing the shadow volume.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Render the scene with depth testing](render-the-scene-with-depth-testing.md)</p></td>
<td align="left"><p>Create a shadow effect by adding depth testing to your vertex (or geometry) shader and your pixel shader.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Support shadow maps on a range of hardware](target-a-range-of-hardware.md)</p></td>
<td align="left"><p>Render higher-fidelity shadows on faster devices and faster shadows on less powerful devices.</p></td>
</tr>
</tbody>
</table>

 

## Shadow mapping application to Direct3D 9 desktop porting


Windows 8 adde d depth comparison functionality to feature level 9\_1 and 9\_3. Now you can migrate rendering code with shadow volumes to DirectX 11, and the Direct3D 11 renderer will be downlevel compatible with feature level 9 devices. This walkthrough shows how any Direct3D 11 app or game can implement traditional shadow volumes using depth testing. The code covers the following process:

1.  Creating Direct3D device resources for shadow mapping.
2.  Adding a rendering pass to create the depth map.
3.  Adding depth testing to the main rendering pass.
4.  Implementing the necessary shader code.
5.  Options for fast rendering on downlevel hardware.

Upon completing this walkthrough, you should be familiar with how to implement a basic compatible shadow volume technique in Direct3D 11 that's compatible with feature level 9\_1 and above.

## Prerequisites


You should [Prepare your dev environment for Universal Windows Platform (UWP) DirectX game development](prepare-your-dev-environment-for-windows-store-directx-game-development.md). You don't need a template yet, but you'll need Microsoft Visual Studio 2015 to build the code sample for this walkthrough.

## Related topics


**Direct3D**

* [Writing HLSL Shaders in Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Create a new DirectX 11 project for UWP](user-interface.md)

**Shadow mapping technical articles**

* [Common Techniques to Improve Shadow Depth Maps](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [Cascaded Shadow Maps](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 







<!--HONumber=Aug16_HO3-->


