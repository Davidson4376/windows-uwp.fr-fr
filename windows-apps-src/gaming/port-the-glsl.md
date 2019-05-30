---
title: Porter le langage GLSL
description: Après avoir adapté le code utilisé pour créer et configurer vos mémoires tampons et vos objets nuanceurs, vous pouvez procéder au portage du code de ces nuanceurs du langage GLSL (GL Shader Language) d’OpenGL ES 2.0 vers le langage HLSL (High-level Shader Language) de Direct3D 11.
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, glsl , portage
ms.localizationpriority: medium
ms.openlocfilehash: 210f98476a06b88e7d3d543006a6d4ec886cfd45
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368252"
---
# <a name="port-the-glsl"></a>Porter le langage GLSL




**API importantes**

-   [Sémantique HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps)
-   [Constantes de nuanceur HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)

Après avoir adapté le code utilisé pour créer et configurer vos mémoires tampons et vos objets nuanceurs, vous pouvez procéder au portage du code de ces nuanceurs du langage GLSL (GL Shader Language) d’OpenGL ES 2.0 vers le langage HLSL (High-level Shader Language) de Direct3D 11.

Dans OpenGL ES 2.0, les nuanceurs de retournent des données après l’exécution à l’aide des fonctions intrinsèques tels que **gl\_Position**, **gl\_FragColor**, ou **gl\_FragData \[n\]**  (où n est l’index pour une cible de rendu spécifique). Dans Direct3D, les nuanceurs n’utilisent pas d’intrinsèques : ils renvoient les données dans le même type que celui de leurs fonctions main() respectives.

Les données à interpoler entre les étapes des nuanceurs, telles que les variables de vertex « position » ou « normal », sont gérées au moyen de la déclaration **varying**. En revanche, cette déclaration n’existant pas dans Direct3D, vous devrez ajouter une [sémantique HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps) à toutes les données à transmettre d’une étape à l’autre dans les nuanceurs. La sémantique choisie dépend de la finalité des données. Par exemple, pour interpoler des données de vertex dans le nuanceur de fragments, déclarez-les comme suit :

`float4 vertPos : POSITION;`

ou

`float4 vertColor : COLOR;`

POSITION correspond à la sémantique définissant les données sur la position du vertex. POSITION est une sémantique particulière, car après l’interpolation, elle n’est plus accessible au nuanceur de pixels. Par conséquent, vous devez spécifier l’entrée pour le nuanceur de pixels avec SV\_POSITION et les données de sommet interpolées seront placées dans cette variable.

`float4 position : SV_POSITION;`

Les sémantiques peuvent être déclarées dans les méthodes body (main) des nuanceurs. Pour les nuanceurs de pixels, SV\_cible\[n\], ce qui indique une cible de rendu est requise sur la méthode de corps. (SV\_cible sans un suffixe numérique par défaut pour restituer l’index cible 0.)

Notez également que les nuanceurs de sommets sont requises pour générer la VP\_valeur système POSITION sémantique. Cette sémantique traduit les données de position du vertex en coordonnées, où x et y sont des valeurs comprises entre -1 et 1, z est divisé par la valeur w de la coordonnée homogène initiale (z/w), et w correspond à 1 divisé par la valeur w initiale (1/w). Nuanceurs de pixels utilisent VP\_valeur de système POSITION sémantique pour récupérer l’emplacement des pixels sur l’écran, où x est comprise entre 0 et la largeur de cible de rendu et les y est comprise entre 0 et la hauteur de cible de rendu (chaque décalage par 0,5). Fonctionnalité niveau 9\_x pixel nuanceurs ne peut pas lire à partir de cette variation\_valeur POSITION.

Les mémoires tampons constantes doivent être déclarées avec le mot clé **cbuffer** et être associées à un registre de démarrage spécifique pour la recherche.

Direct3D 11 : Une déclaration de la mémoire tampon constante HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Dans cet exemple, la mémoire tampon constante utilise le registre b0 pour mettre en attente la mémoire tampon empaquetée. Tous les registres sont référencés dans le formulaire b\#. Pour plus d’informations sur l’implémentation HLSL des mémoires tampons constantes, des registres et des données empaquetées, voir [Constantes de nuanceur (HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants).

<a name="instructions"></a>Instructions
------------
### <a name="step-1-port-the-vertex-shader"></a>Étape 1 : Port le nuanceur de sommets

Dans notre exemple simple de code OpenGL ES 2.0, le nuanceur de vertex reçoit trois données en entrée : une matrice 4x4 constante modèle-affichage-projection et deux vecteurs à quatre coordonnées. Ces vecteurs définissent la position et la couleur du vertex. Le nuanceur transforme le vecteur de position des coordonnées de point de vue et lui attribue le gl\_Position intrinsèque pour la rastérisation. La couleur du vertex est copiée dans une variable « varying » qui sera également utilisée lors de la rastérisation, pour l’interpolation.

OpenGL ES 2.0 : Nuanceur de sommets pour l’objet de cube (GLSL)

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

Maintenant, dans Direct3D, la matrice de projection de modèle-vue constante est contenue dans un mémoire tampon constante, rassemblé au Registre b0, et la position du vertex et la couleur sont spécifiquement marquées avec la sémantique HLSL respectif appropriée : POSITION et la couleur. Étant donné que notre disposition en entrée correspond à un emplacement précis de ces deux valeurs de vertex, vous créez une structure pour les contenir, puis déclarez cette structure en tant que type de paramètre d’entrée pour la fonction body (main) du nuanceur. (Vous pouvez également spécifier en tant que deux paramètres individuels, mais qui pourrait se révéler fastidieux.) Également, vous spécifiez un type de sortie pour cette étape, qui contient la position interpolée et la couleur, et déclarez en tant que la valeur de retour pour le corps de la fonction de nuanceur de sommets.

Direct3D 11 : Nuanceur de sommets pour l’objet de cube (HLSL)

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

### <a name="step-2-port-the-fragment-shader"></a>Étape 2 : Port du nuanceur de fragment

Notre nuanceur de fragment d’exemple dans GLSL est extrêmement simple : fournir le gl\_FragColor intrinsèque avec la valeur de couleur interpolée. OpenGL ES 2.0 transmettra cette valeur à la cible de rendu par défaut.

OpenGL ES 2.0 : Fragment shader pour l’objet de cube (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Le processus Direct3D est presque aussi simple. La seule différence notable concerne la fonction body du nuanceur de pixels, qui doit renvoyer une valeur. Étant donné que la couleur est une valeur de type float (RVBA) 4 coordonnée, votre indiquer float4 comme type de retour, puis spécifiez la cible de rendu par défaut comme VP\_sémantique de valeur du système cible.

Direct3D 11 : Nuanceur de pixels de l’objet de cube (HLSL)

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
Pour faciliter le débogage de votre code et avoir plus de latitude pour l’optimiser, il est essentiel de bien comprendre le fonctionnement des sémantiques HLSL et du packaging des mémoires tampons constantes. Si vous avez la possibilité, lisez [Variable syntaxe HLSL ()](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-syntax), [Introduction aux mémoires tampons dans Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro), et [Comment : Créer un mémoire tampon constante](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to). Sinon, prenez au moins connaissance des quelques conseils ci-dessous concernant l’utilisation des sémantiques et des mémoires tampons :

-   Vérifiez toujours le code de configuration Direct3D de votre convertisseur afin de vous assurer que les structures de vos mémoires tampons constantes correspondent aux déclarations de structure cbuffer indiquées dans votre code HLSL, et que les types scalaires des composants sont identiques dans l’ensemble des déclarations.
-   Dans le code C++ de votre convertisseur, utilisez les types [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) dans les déclarations de mémoires tampons constantes pour garantir un packaging correct des données.
-   Pour optimiser l’utilisation des mémoires tampons constantes, classez les variables de nuanceur dans ces mémoires en fonction de la fréquence de leur mise à jour. Par exemple, si certaines de vos données uniformes sont mises à jour une fois par trame, alors que d’autres ne sont mises à jour qu’après un mouvement de la caméra, il vaut mieux placer ces données dans deux mémoires tampons constantes distinctes.
-   Vérifiez soigneusement toutes les sémantiques utilisées. En effet, les premières erreurs de compilation d’un nuanceur (FXC) sont souvent dues à un oubli de sémantiques ou à leur utilisation incorrecte. La documentation fournie peut être confuse, du fait que beaucoup de pages et d’exemples publiés précédemment font référence aux différentes versions de sémantiques HLSL antérieures à Direct3D 11.
-   Assurez-vous que le niveau de fonctionnalité Direct3D utilisé est approprié pour chaque nuanceur. La sémantique pour la fonctionnalité de niveau 9\_ \* sont différentes de celles pour 11\_1.
-   VP\_résout sémantique de POSITION les données de la position de post-interpolation associé pour coordonner les valeurs, où x est entre 0 et de la largeur de cible de rendu, y sont comprise entre 0 et le rendu cible hauteur, z est divisé par le homogène w de coordonnées d’origine valeur (z/w) et w est 1 divisée par la valeur d’origine w (1/w).

## <a name="related-topics"></a>Rubriques connexes


[Comment : port un simple convertisseur OpenGL ES 2.0 vers Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Les objets de nuanceur de port](port-the-shader-config.md)

[Les mémoires tampons de vertex et les données de port](port-the-vertex-buffers-and-data-config.md)

[Dessiner à l’écran](draw-to-the-screen.md)

 

 




