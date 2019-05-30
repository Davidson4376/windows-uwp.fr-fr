---
title: Mapper OpenGL ES 2.0 à Direct3D 11
description: Si vous vous apprêtez à porter votre architecture graphique OpenGL ES 2.0 sur Direct3D pour la première fois, commencez par vous familiariser avec les principales différences entre les API.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, opengl, direct3d, porter
ms.localizationpriority: medium
ms.openlocfilehash: 525b97700b1362bb19a1b328183f3cbf9da3b006
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368524"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Mapper OpenGL ES 2.0 à Direct3D 11



Si vous vous apprêtez à porter votre architecture graphique OpenGL ES 2.0 sur Direct3D pour la première fois, commencez par vous familiariser avec les principales différences entre les API. Les rubriques de cette section vous aideront à planifier votre stratégie de portage ainsi que les différentes modifications à apporter en vue du transfert de votre processus de traitement graphique sur Direct3D.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">Planifier votre Portage depuis OpenGL ES 2.0 vers Direct3D</a></p></td>
<td align="left"><p>Si vous portez un jeu à partir des plateformes iOS ou Android, vous avez probablement effectué un investissement considérable dans OpenGL ES 2.0. Avant de déplacer le code base de votre pipeline graphique vers Direct3D 11 et le Windows Runtime, certains points sont à prendre en compte.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">Comparez le code EGL pour DXGI et Direct3D</a></p></td>
<td align="left"><p>L’interface graphique DirectX (DXGI) et certaines API Direct3D jouent le même rôle qu’EGL. Cette rubrique vous aidera à comprendre le fonctionnement de DXGI et Direct3D 11 sous l’ange d’EGL.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">Comparer les mémoires tampons, uniformes et attributs de sommets à Direct3D OpenGL ES 2.0</a></p></td>
<td align="left"><p>Au cours du processus de portage vers Direct3D 11 depuis OpenGL ES 2.0, vous devez modifier la syntaxe et le comportement de l’API pour passer des données entre l’application et les programmes de nuanceurs.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">Comparer le pipeline de nuanceur OpenGL ES 2.0 vers Direct3D</a></p></td>
<td align="left"><p>D’un point de vue conceptuel, le pipeline nuanceur de Direct3D 11 est très similaire à celui d’OpenGL ES 2.0. En termes de conception d’API, les composants majeurs de création et de gestion des étapes de nuanceur appartiennent aux deux interfaces principales, <a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1"><strong>ID3D11Device1</strong></a> et <a href="https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1"><strong>ID3D11DeviceContext1</strong></a>. Cette rubrique tente d’établir une correspondance dans ces interfaces entre les modèles courants d’API du pipeline nuanceur d’OpenGL ES 2.0 et ceux de Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>Remarques sur les fournisseurs OpenGL ES 2.0 spécifiques


Ces rubriques font référence à la spécification OpenGL ES 2.0 de Khronos pour les plateformes agnostiques en langage C. Les plateformes iOS et Android emploient la même spécification ; le code OpenGL ES 2.0 développé pour ces deux plateformes est similaire aux extraits de code que nous allons étudier, bien que ces derniers soient généralement exposés en tant qu’API orientées objet. Par ailleurs, en raison des complexités et des spécificités linguistiques de chaque plateforme, des différences mineures peuvent se présenter, surtout en ce qui concerne les types de paramètres de méthode et la syntaxe générale du langage. Par exemple, iOS utilise le langage Objective-C. Android a la capacité d’utiliser le langage C++, mais les développeurs peuvent opter pour une implémentation en Java exclusivement. Tout en gardant les points précédents à l’esprit, n’hésitez pas à vous référer à ces rubriques, car les concepts, la structure et le mode d’utilisation des API OpenGL ES sont globalement identiques.

 

 




