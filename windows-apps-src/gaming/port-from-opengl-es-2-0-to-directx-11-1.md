---
title: Portage d’OpenGL ES 2.0 vers Direct3D 11
description: Cette section propose des articles, des vues d’ensemble et des procédures pas à pas concernant le portage d’un pipeline graphique d’OpenGL ES 2.0 sur Direct3D 11 et sur Windows Runtime.
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
---

# Portage d’OpenGL ES 2.0 vers Direct3D 11


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cette section propose des articles, des vues d’ensemble et des procédures pas à pas concernant le portage d’un pipeline graphique d’OpenGL ES 2.0 sur Direct3D 11 et sur Windows Runtime.

> **Remarque** Une étape intermédiaire pour le portage de votre projet OpenGL ES 2.0 consiste à utiliser ANGLE pour Windows Store. ANGLE pour Windows Store vous permet d’exécuter le contenu OpenGL ES sous Windows en convertissant les appels d’API OpenGL ES en appels d’API DirectX 11. Pour plus d’informations sur ANGLE, voir le [Wiki ANGLE pour Windows Store](http://go.microsoft.com/fwlink/p/?linkid=618387).

 

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
<td align="left"><p>[Map OpenGL ES 2.0 to Direct3D 11.1](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>Si vous vous apprêtez à porter votre architecture graphique OpenGL ES 2.0 sur Direct3D pour la première fois, commencez par vous familiariser avec ces API afin de découvrir les principales caractéristiques qui les distinguent. Les rubriques de cette section vous aideront à planifier votre stratégie de portage ainsi que les différentes modifications à apporter en vue du transfert de votre processus de traitement graphique sur Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Walkthrough sample ports from OpenGL ES 2.0](walkthrough-sample-ports-from-opengl-es-2-0.md)</p></td>
<td align="left"><p>Cet ensemble de rubrique présente plusieurs scénarios de portage de pipeline graphique OpenGL ES 2.0 de différente complexité.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[OpenGL ES 2.0 to Direct3D 11.1 reference](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>Vous trouverez dans ces rubriques de référence un index de mappage d’API ainsi que des exemples de code simples destinés à vous aider lors du portage d’OpenGL ES 2.0 sur Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

> **Remarque**  
Cet article s’adresse aux développeurs de Windows 10 qui développent des applications pour la plateforme Windows universelle (UWP). Si vous développez une application pour Windows 8.x ou Windows Phone 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 






<!--HONumber=Mar16_HO1-->


