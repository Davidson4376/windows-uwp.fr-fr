---
title: Porter un convertisseur OpenGLES2.0 simple vers Direct3D11
description: 'Le premier exercice de portage nous permettra de mettre en pratique une notion de base : porter un convertisseur simple d’OpenGL ES 2.0 sur Direct3D, afin d’adapter un cube en rotation inclus dans un nuanceur de vertex au modèle d’application DirectX 11 (Windows universelle) fourni dans Visual Studio 2015.'
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, opengl, direct3d11 , portage
ms.localizationpriority: medium
ms.openlocfilehash: 78bcf3c2cae53fba4e67ecd4b3bcc44adddde1bf
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8325307"
---
# <a name="port-a-simple-opengl-es-20-renderer-to-direct3d-11"></a>Porter un convertisseur OpenGLES2.0 simple vers Direct3D11



Cet exercice de portage permet de mettre en pratique une notion de base: porter un convertisseur simple d’OpenGLES2.0 sur Direct3D, afin d’adapter un cube en rotation inclus dans un nuanceur de vertex au modèle d’application DirectX11 (Windows universelle) fourni dans VisualStudio2015. À mesure que nous avancerons dans le processus de portage, nous découvrirons comment effectuer les différentes tâches suivantes :

-   Porter un ensemble simple de mémoires tampons de vertex vers des mémoires tampons d’entrée Direct3D
-   Porter des variables uniform et attribute vers des mémoires tampons constantes
-   Configurer des objets nuanceur Direct3D
-   Utiliser des sémantiques HLSL simples pour développer un nuanceur Direct3D
-   Porter du code GLSL très simple vers HLSL

Cette rubrique suppose que vous avez déjà créé votre projet DirectX 11. Pour savoir comment créer un projet DirectX 11, voir [Créer un projet DirectX 11 pour la plateforme Windows universelle (UWP)](user-interface.md).

Si vous avez créé votre projet à partir d’un de ces liens, ce projet contient tout le code requis pour l’infrastructure [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476345). Vous pouvez donc commencer immédiatement le processus de portage de votre convertisseur d’Open GL ES 2.0 sur Direct3D 11.

Cette rubrique examine deux chemins de code qui effectuent la même tâche graphique de base : afficher un cube en forme de vertex qui tourne dans une fenêtre. Dans les deux cas, le code couvre le processus suivant :

1.  Création d’un maillage du cube à partir des données codées en dur. Ce maillage est représenté par une liste de vertex, où chaque vertex est associé à une position, un vecteur normal et un vecteur de couleur. Il est ensuite stocké dans une mémoire tampon de vertex en attendant d’être traité par le pipeline de nuanceurs.
2.  Création des deux objets nuanceur requis pour le traitement du maillage du cube. Le nuanceur de vertex traitera les vertex utilisés lors de la rastérisation et le nuanceur de fragments (pixels) appliquera une couleur à chacun des pixels du cube après la rastérisation. Ces pixels seront transmis à la cible de rendu pour affichage.
3.  Écriture du code d’ombrage utilisé pour le traitement des vertex et des pixels dans les nuanceurs de vertex et de fragments, respectivement.
4.  Affichage à l’écran du cube rendu.

![Cube OpenGL simple](images/simple-opengl-cube.png)

Au terme de cette procédure pas à pas, vous aurez normalement passé en revue les principales différences entre Open GL ES2.0 et Direct3D11:

-   Représentation des mémoires tampons et données de vertex
-   Processus de création et de configuration des nuanceurs
-   Codes d’ombrage, et entrées et sorties des nuanceurs
-   Comportements du dessin à l’écran

Cette procédure pas à pas utilise une structure de convertisseur OpenGL simple et générale, définie comme suit :

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

Cette structure n’a qu’une seule instance; elle contient tous les éléments requis pour effectuer le rendu d’un maillage très simple d’un nuanceur de vertex.

> **Remarque**OpenGL ES 2.0 tout code de cette rubrique est basé sur l’implémentation de l’API Windows fournie par Khronos Group et utilise la syntaxe de programmation Windows C.

 

## <a name="what-you-need-to-know"></a>Ce que vous devez savoir


### <a name="technologies"></a>Technologies

-   [Microsoft Visual C++](http://msdn.microsoft.com/library/vstudio/60k1461a.aspx)
-   OpenGL ES2.0

### <a name="prerequisites"></a>Prérequis

-   Facultatif. Consultez la rubrique [Comparer le code EGL avec DXGI et Direct3D](moving-from-egl-to-dxgi.md). Cette rubrique vous explique plus en détail le fonctionnement de l’interface graphique fournie par DirectX.

## <a name="span-idkeylinksstepsheadingspansteps"></a><span id="keylinks_steps_heading"></span>Étapes


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
<td align="left"><p><a href="port-the-shader-config.md">Porter les objets nuanceurs</a></p></td>
<td align="left"><p>Dans le cadre du portage du convertisseur simple OpenGL ES 2.0, vous devez commencer par créer les objets des nuanceurs de vertex et de fragments équivalents dans Direct3D 11, mais également vous assurer que le programme principal sera en mesure de communiquer avec ces différents objets une fois compilés.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-the-vertex-buffers-and-data-config.md">Porter les mémoires tampons et données de vertex</a></p></td>
<td align="left"><p>Lors de cette étape, vous allez définir les mémoires tampons de vertex qui contiendront vos maillages ainsi que les mémoires tampons d’index qui permettront aux nuanceurs de parcourir les vertex dans l’ordre indiqué.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="port-the-glsl.md">Porter le langage GLSL</a></p></td>
<td align="left"><p>Après avoir adapté le code utilisé pour créer et configurer vos mémoires tampons et vos nuanceurs, vous pouvez procéder au portage du code de ces nuanceurs du langage GLSL (GL Shader Language) d’OpenGLES2.0 vers le langage HLSL (High-level Shader Language) de Direct3D11.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="draw-to-the-screen.md">Dessiner à l’écran</a></p></td>
<td align="left"><p>Pour finir, nous portons le code qui trace le cube tournant à l’écran.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idadditionalresourcesspanadditional-resources"></a><span id="additional_resources"></span>Ressources supplémentaires


-   [Préparer votre environnement pour le développement de jeux UWP DirectX](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [Créer un projet DirectX 11 pour UWP](user-interface.md)
-   [Mapper les concepts et l’infrastructure OpenGL ES 2.0 à Direct3D 11](map-concepts-and-infrastructure.md)

 

 




