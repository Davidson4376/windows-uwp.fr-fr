---
author: mtoepke
title: "Portage d’OpenGL ES 2.0 sur Direct3D 11"
description: "Cette section propose des articles, des vues d’ensemble et des procédures pas à pas concernant le portage d’un pipeline graphique d’OpenGL ES 2.0 sur Direct3D 11 et sur Windows Runtime."
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, jeux, opengl, direct3d 11 , portage, graphiques"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 14ed2be84f295570dc95b3f1d28dfdd3720bada4
ms.lasthandoff: 02/07/2017

---

# <a name="port-from-opengl-es-20-to-direct3d-11"></a>Passer d’OpenGL ES 2.0 à Direct3D 11


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette section propose des articles, des vues d’ensemble et des procédures pas à pas concernant le portage d’un pipeline graphique d’OpenGL ES 2.0 sur Direct3D 11 et sur Windows Runtime.

> **Remarque**  Une étape intermédiaire pour le portage de votre projet OpenGL ES 2.0 consiste à utiliser ANGLE pour Windows Store. ANGLE pour Windows Store vous permet d’exécuter le contenu OpenGL ES sous Windows en convertissant les appels d’API OpenGL ES en appels d’API DirectX 11. Pour plus d’informations sur ANGLE, voir le [Wiki ANGLE pour Windows Store](http://go.microsoft.com/fwlink/p/?linkid=618387).

 

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
<td align="left"><p>[Mapper OpenGL ES 2.0 à Direct3D 11.1](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>Si vous vous apprêtez à porter votre architecture graphique OpenGL ES 2.0 sur Direct3D pour la première fois, commencez par vous familiariser avec les principales différences entre les API. Les rubriques de cette section vous aideront à planifier votre stratégie de portage ainsi que les différentes modifications à apporter en vue du transfert de votre processus de traitement graphique sur Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Procédure : porter un convertisseur OpenGL ES 2.0 simple vers Direct3D 11.1](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)</p></td>
<td align="left"><p>Cet exercice de portage permet de mettre en pratique une notion de base : porter un convertisseur simple d’OpenGL ES 2.0 sur Direct3D, afin d’adapter un cube en rotation inclus dans un nuanceur de vertex au modèle d’application DirectX 11 (Windows universelle) fourni dans Visual Studio 2015.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Informations de référence sur le passage d’OpenGL ES 2.0 à Direct3D 11.1](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>Vous trouverez dans ces rubriques de référence un index de mappage d’API ainsi que des exemples de code simples destinés à vous aider lors du portage d’OpenGL ES 2.0 vers Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

> **Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui développent des applications de la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 





