---
title: Porter le langage GLSL
description: Après avoir adapté le code utilisé pour créer et configurer vos mémoires tampons et vos nuanceurs, vous pouvez procéder au portage du code de ces nuanceurs du langage GLSL (GL Shader Language) d’OpenGLES2.0 vers le langage HLSL (High-level Shader Language) de Direct3D11.
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, jeux, glsl , portage
ms.localizationpriority: medium
ms.openlocfilehash: 809440f9e77af19c01f4a050eee3b6f8d1c709b7
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7830683"
---
# <a name="port-the-glsl"></a>Porter le GLSL




**API importantes**

-   [Sémantique HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574)
-   [Constantes de nuanceur (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)

Après avoir adapté le code utilisé pour créer et configurer vos mémoires tampons et vos nuanceurs, vous pouvez procéder au portage du code de ces nuanceurs du langage GLSL (GL Shader Language) d’OpenGLES2.0 vers le langage HLSL (High-level Shader Language) de Direct3D11.

Dans OpenGLES2.0, les nuanceurs exécutés renvoient les données avec des intrinsèques comme **gl\_Position**, **gl\_FragColor** ou **gl\_FragData\[n\]** (où n est l’index d’une cible de rendu spécifique). Dans Direct3D, les nuanceurs n’utilisent pas d’intrinsèques: ils renvoient les données dans le même type que celui de leurs fonctions main() respectives.

Les données à interpoler entre les étapes des nuanceurs, telles que les variables de vertex «position» ou «normal», sont gérées au moyen de la déclaration **varying**. En revanche, cette déclaration n’existant pas dans Direct3D, vous devrez ajouter une [sémantique HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574) à toutes les données à transmettre d’une étape à l’autre dans les nuanceurs. La sémantique choisie dépend de la finalité des données. Par exemple, pour interpoler des données de vertex dans le nuanceur de fragments, déclarez-les comme suit :

`float4 vertPos : POSITION;`

ou

`float4 vertColor : COLOR;`

POSITION correspond à la sémantique définissant les données sur la position du vertex. POSITION est une sémantique particulière, car après l’interpolation, elle n’est plus accessible au nuanceur de pixels. Par conséquent, vous devez indiquer les données en entrée dans le nuanceur de pixels avec SV\_POSITION afin que les données de vertex interpolées soient transmises à cette variable.

`float4 position : SV_POSITION;`

Les sémantiques peuvent être déclarées dans les méthodes body (main) des nuanceurs. Pour les nuanceurs de pixels, vous devez définir une cible de rendu avec SV\_TARGET\[n\] dans la méthode body. Lorsque SV\_TARGET ne comporte pas de suffixe numérique, la cible de rendu d’index0 est utilisée par défaut.

Notez également que les nuanceurs de vertex doivent renvoyer la sémantique de la valeur système SV\_POSITION. Cette sémantique traduit les données de position du vertex en coordonnées, où x et y sont des valeurs comprises entre -1 et 1, z est divisé par la valeur w de la coordonnée homogène initiale (z/w), et w correspond à 1 divisé par la valeur w initiale (1/w). Les nuanceurs de pixels utilisent la sémantique de la valeur système SV\_POSITION pour récupérer l’emplacement des pixels à l’écran, où x se situe entre0 et la largeur de la cible de rendu, et y entre0 et la hauteur de la cible de rendu (avec un décalage de 0,5 pour chacun). Les nuanceurs de pixels du niveau de fonctionnalité 9\_x ne peuvent pas lire la valeur SV\_POSITION.

Les mémoires tampons constantes doivent être déclarées avec le mot clé **cbuffer** et être associées à un registre de démarrage spécifique pour la recherche.

Direct3D11: déclaration de mémoire tampon constante HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Dans cet exemple, la mémoire tampon constante utilise le registre b0 pour mettre en attente la mémoire tampon empaquetée. Tous les registres sont référencés sous la forme «b\#». Pour plus d’informations sur l’implémentation HLSL des mémoires tampons constantes, des registres et des données empaquetées, voir [Constantes de nuanceur (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581).

<a name="instructions"></a>Instructions
------------
### <a name="step-1-port-the-vertex-shader"></a>Étape1: Porter le nuanceur de vertex

Dans notre exemple simple de code OpenGLES2.0, le nuanceur de vertex reçoit trois données en entrée: une matrice 4x4 constante modèle-affichage-projection et deux vecteurs à quatre coordonnées. Ces vecteurs définissent la position et la couleur du vertex. Le nuanceur traduit le vecteur de position en coordonnées de perspective, qu’il affecte ensuite à l’intrinsèque gl\_Position pour les besoins de la rastérisation. La couleur du vertex est copiée dans une variable «varying» qui sera également utilisée lors de la rastérisation, pour l’interpolation.

OpenGL ES 2.0 : nuanceur de vertex pour l’objet cube (GLSL)

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

Désormais, dans Direct3D, la matrice constante modèle-affichage-projection est contenue dans une mémoire tampon constante empaquetée dans le registre b0, et la position et la couleur du vertex sont définies avec des sémantiques HLSL spécifiques (POSITION et COLOR, respectivement). Étant donné que notre disposition en entrée correspond à un emplacement précis de ces deux valeurs de vertex, vous créez une structure pour les contenir, puis déclarez cette structure en tant que type de paramètre d’entrée pour la fonction body (main) du nuanceur. (Vous pourriez aussi définir ces deux valeurs sous forme de paramètres individuels, mais cette pratique n’est pas recommandée.) Par ailleurs, vous indiquez un type de sortie pour cette étape, qui contient la position et la couleur interpolées, puis vous le déclarez en tant que valeur de retour pour la fonction body du nuanceur de vertex.

Direct3D11: nuanceur de vertex pour l’objet cube (HLSL)

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

Le type des données en sortie (PixelShaderInput) est indiqué au moment de la rastérisation, puis transmis au nuanceur de fragments (pixels).

### <a name="step-2-port-the-fragment-shader"></a>Étape2: Porter le nuanceur de fragments

Notre exemple de nuanceur de fragments en GLSL est très simple: il suffit de fournir l’intrinsèque gl\_FragColor avec la valeur de la couleur interpolée. OpenGLES2.0 transmettra cette valeur à la cible de rendu par défaut.

OpenGL ES 2.0 : nuanceur de fragments pour l’objet cube (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Le processus Direct3D est presque aussi simple. La seule différence notable concerne la fonction body du nuanceur de pixels, qui doit renvoyer une valeur. Étant donné que la couleur est représentée par une valeur flottante (RVBA) à quatre coordonnées, vous devez indiquer un type de retour float4, puis définir la cible de rendu par défaut avec la valeur système SV\_TARGET.

Direct3D11: nuanceur de pixels pour l’objet cube (HLSL)

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

La couleur du pixel situé à la position indiquée est transmise à la cible de rendu. Pour plus d’informations sur l’affichage du contenu de la cible de rendu, voir [Dessiner à l’écran](draw-to-the-screen.md).

## <a name="previous-step"></a>Étape précédente


[Porter les données et tampons vertex](port-the-vertex-buffers-and-data-config.md) Étape suivante
---------
[Dessiner à l’écran](draw-to-the-screen.md) Remarques
-------
Pour faciliter le débogage de votre code et avoir plus de latitude pour l’optimiser, il est essentiel de bien comprendre le fonctionnement des sémantiques HLSL et du packaging des mémoires tampons constantes. Si vous le pouvez, lisez les articles [Syntaxe des variables (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509706), [Présentation des mémoires tampons dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476898) et [Procédure: Création d’une mémoire tampon constante](https://msdn.microsoft.com/library/windows/desktop/ff476896). Sinon, prenez au moins connaissance des quelques conseils ci-dessous concernant l’utilisation des sémantiques et des mémoires tampons :

-   Vérifiez toujours le code de configuration Direct3D de votre convertisseur afin de vous assurer que les structures de vos mémoires tampons constantes correspondent aux déclarations de structure cbuffer indiquées dans votre code HLSL, et que les types scalaires des composants sont identiques dans l’ensemble des déclarations.
-   Dans le codeC++ de votre convertisseur, utilisez les types [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) dans les déclarations de mémoires tampons constantes pour garantir un packaging correct des données.
-   Pour optimiser l’utilisation des mémoires tampons constantes, classez les variables de nuanceur dans ces mémoires en fonction de la fréquence de leur mise à jour. Par exemple, si certaines de vos données uniformes sont mises à jour une fois par trame, alors que d’autres ne sont mises à jour qu’après un mouvement de la caméra, il vaut mieux placer ces données dans deux mémoires tampons constantes distinctes.
-   Vérifiez soigneusement toutes les sémantiques utilisées. En effet, les premières erreurs de compilation d’un nuanceur (FXC) sont souvent dues à un oubli de sémantiques ou à leur utilisation incorrecte. La documentation fournie peut être confuse, du fait que beaucoup de pages et d’exemples publiés précédemment font référence aux différentes versions de sémantiques HLSL antérieures à Direct3D 11.
-   Assurez-vous que le niveau de fonctionnalité Direct3D utilisé est approprié pour chaque nuanceur. Les sémantiques du niveau de fonctionnalité 9\_\* ne sont pas les mêmes que celles du niveau 11\_1.
-   La sémantique SV\_POSITION traduit les données de position interpolées associées en coordonnées, où x est une valeur située entre 0 et la largeur de la cible de rendu, y une valeur située entre 0 et la hauteur de la cible de rendu, z est divisé par la valeur w de la coordonnée homogène initiale (z/w), et w correspond à 1 divisé par la valeur w initiale (1/w).

## <a name="related-topics"></a>Rubriques connexes


[Procédure: portage d’un convertisseur simple OpenGL ES2.0 sur Direct3D11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Porter les objets nuanceur](port-the-shader-config.md)

[Porter les mémoires tampons et données de vertex](port-the-vertex-buffers-and-data-config.md)

[Dessiner à l’écran](draw-to-the-screen.md)

 

 




