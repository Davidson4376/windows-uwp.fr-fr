---
title: Informations de référence sur le passage de GLSL vers HLSL
description: Si vous avez pour projet de créer un jeu pour UWP, vous allez porter votre architecture graphique OpenGLES2.0 sur Direct3D11, ce qui impliquera de porter votre code GLSL (OpenGL Shader Language) vers le code Microsoft HLSL (High Level Shader Language).
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp, glsl, hlsl, opengl, directx, nuanceurs
ms.localizationpriority: medium
ms.openlocfilehash: 8f468584d995de40ff14df1527ab1df8275c36a8
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826890"
---
# <a name="glsl-to-hlsl-reference"></a>Informations de référence sur le passage de GLSL à HLSL



Si vous avez pour projet de créer un jeu pour UWP, vous allez [porter votre architecture graphique OpenGLES2.0 sur Direct3D11](port-from-opengl-es-2-0-to-directx-11-1.md), ce qui impliquera de porter votre code GLSL (OpenGL Shader Language) vers le code Microsoft HLSL (High Level Shader Language). Les exemples de cette rubrique utilisent du code GLSL et du code HLSL compatibles avec OpenGL ES 2.0 et avec Direct3D 11, respectivement. Pour plus d’informations sur les différences entre Direct3D11 et les versions antérieures de Direct3D, voir [Mappage des fonctionnalités](feature-mapping.md).

-   [Comparaison entre OpenGL ES 2.0 et Direct3D 11](#comparing-opengl-es-20-with-direct3d-11)
-   [Portage des variables GLSL vers HLSL](#porting-glsl-variables-to-hlsl)
-   [Portage des types GLSL vers HLSL](#porting-glsl-types-to-hlsl)
-   [Portage des variables globales prédéfinies GLSL vers HLSL](#porting-glsl-pre-defined-global-variables-to-hlsl)
-   [Exemples de portage de variables GLSL vers HLSL](#examples-of-porting-glsl-variables-to-hlsl)
    -   [Variables uniform, attribute et varying dans GLSL](#uniform-attribute-and-varying-in-glsl)
    -   [Mémoires tampons constantes et transferts de données dans HLSL](#constant-buffers-and-data-transfers-in-hlsl)
-   [Exemples de portage du code de rendu OpenGL sur Direct3D](#examples-of-porting-opengl-rendering-code-to-direct3d)
-   [Rubriques connexes](#related-topics)

## <a name="comparing-opengl-es-20-with-direct3d-11"></a>Comparaison entre OpenGL ES 2.0 et Direct3D 11


OpenGL ES 2.0 et Direct3D 11 présentent de nombreuses similitudes. Ils offrent tous deux à peu près les mêmes pipelines de rendu et fonctionnalités graphiques. En revanche, alors que Direct3D 11 constitue une API et une implémentation de rendu, mais pas une spécification, OpenGL ES 2.0 est une spécification de rendu et une API, mais pas une implémentation. Direct3D 11 et OpenGL ES 2.0 diffèrent généralement sur les points suivants :

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Spécification agnostique du matériel et du système d’exploitation avec des implémentations propres à chaque fournisseur             | Implémentation Microsoft de l’abstraction et de la certification matérielles sur les plateformes Windows                                |
| Gestion de la plupart des ressources par le runtime grâce à l’abstraction pour la diversité du matériel                                     | Accès direct à la disposition matérielle ; gestion des ressources et du traitement par l’application                                              |
| Obtention de modules de niveau supérieur via des bibliothèques tierces, telles que Simple DirectMedia Layer (SDL) | Construction des modules de niveau supérieur, comme Direct2D, à partir de modules de niveau inférieur pour simplifier le développement pour les applications Windows             |
| Différenciation des fabricants de matériel par les extensions                                                         | Ajout à l’API, par Microsoft, de fonctionnalités facultatives génériques, indépendantes des fabricants de matériel |

 

GLSL et HLSL diffèrent généralement sur les points suivants :

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL</th>
<th align="left">HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Langage procédural et orienté étape (type C)</td>
<td align="left">Langage orienté objet et centré sur les données (type C++)</td>
</tr>
<tr class="even">
<td align="left">Compilation des nuanceurs intégrée à l’API graphique</td>
<td align="left">Le compilateurHLSL <a href="https://msdn.microsoft.com/library/windows/desktop/bb509633">compile le nuanceur</a> en une représentation binaire intermédiaire, que Direct3D transmet ensuite au pilote.
<div class="alert">
<strong>Remarque</strong>cette représentation binaire est indépendante du matériel. Elle est généralement compilée au moment de la création de l’application, plutôt qu’au moment de l’exécution de cette dernière.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">Modificateurs de stockage des <a href="#porting-glsl-variables-to-hlsl">variables</a></td>
<td align="left">Mémoires tampons constantes et transferts de données via des déclarations de dispositions d’entrée</td>
</tr>
<tr class="even">
<td align="left"><p><a href="#porting-glsl-types-to-hlsl">Types</a></p>
<p>Type de vecteur standard: vec2/3/4</p>
<p>lowp, mediump, highp</p></td>
<td align="left"><p>Type de vecteur standard : float2/3/4</p>
<p>min10float, min16float</p></td>
</tr>
<tr class="odd">
<td align="left">texture2D [Function]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509695">texture.Sample</a> [datatype.Function]</td>
</tr>
<tr class="even">
<td align="left">sampler2D [datatype]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a> [datatype]</td>
</tr>
<tr class="odd">
<td align="left">Matrices row-major (par défaut)</td>
<td align="left">Matrices column-major (par défaut)
<div class="alert">
<strong>Remarque</strong>  utilisez le modificateur de type <strong>row_major</strong> pour modifier la disposition pour une seule variable. Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/desktop/bb509706">Syntaxe des variables</a>. Vous pouvez aussi spécifier un indicateur de compilateur ou une instruction pragma pour modifier la configuration globale par défaut.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Nuanceur de fragments</td>
<td align="left">Nuanceur de pixels</td>
</tr>
</tbody>
</table>

 

> **Remarque**HLSL expose les textures et les échantillonneurs dans deux objets distincts. alors que dans GLSL (Direct3D 9, par exemple), la liaison des textures est incluse dans l’état de l’échantillonneur.

 

Dans GLSL, l’état OpenGL est souvent présenté à l’aide de variables globales prédéfinies. Par exemple, dans GLSL, vous utilisez les variables **gl\_Position** et **gl\_FragColor** pour indiquer la position du vertex et la couleur du fragment, respectivement. Dans HLSL, vous passez l’état Direct3D de manière explicite entre le code de l’application et le nuanceur. Par exemple, avec Direct3D et HLSL, les données d’entrée du nuanceur de vertex doivent être au même format que dans la mémoire tampon de vertex, tout comme la structure d’une mémoire tampon constante dans le code de l’application doit correspondre à la structure de mémoire tampon constante ([cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)) indiquée dans le code du nuanceur.

## <a name="porting-glsl-variables-to-hlsl"></a>Portage des variables GLSL vers HLSL


Dans GLSL, vous appliquez des modificateurs (qualificateurs) à la déclaration d’une variable de nuanceur globale lorsque vous souhaitez affecter à cette variable un comportement particulier dans vos nuanceurs. Dans HLSL, vous n’avez pas besoin de ces modificateurs, car vous définissez le flux du nuanceur à l’aide des arguments que vous passez au nuanceur et des arguments que celui-ci vous retourne.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Comportement des variables GLSL</th>
<th align="left">Équivalence HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>uniforme</strong></p>
<p>Vous passez une variable uniform du code de l’application au nuanceur de vertex et/ou au nuanceur de fragments. Vous devez définir les valeurs de toutes les variables uniform avant de dessiner des triangles avec ces nuanceurs. Ainsi, les valeurs ne changeront pas lors du processus de dessin d’un maillage de triangles. Ces valeurs sont uniformes. Certaines sont définies pour la trame entière, d’autres uniquement pour une paire spécifique de nuanceurs de vertex/pixels.</p>
<p>Les variables uniform s’appliquent au niveau de chaque polygone.</p></td>
<td align="left"><p>Utilisez une mémoire tampon constante.</p>
<p>Voir <a href="https://msdn.microsoft.com/library/windows/desktop/ff476896">Procédure: Création d’une mémoire tampon constante</a> et <a href="https://msdn.microsoft.com/library/windows/desktop/bb509581">Constantes de nuanceur</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>varying</strong></p>
<p>Vous initialisez une variable varying au sein du nuanceur de vertex, puis vous la passez à une variable varying de même nom dans le nuanceur de fragments. Le nuanceur de vertex définit la valeur des variables varying au niveau de chaque vertex. Par conséquent, le module de rastérisation effectue ensuite une interpolation de ces valeurs (en perspective) pour générer les valeurs de chaque fragment à passer au nuanceur de fragments. Ces variables diffèrent pour chaque triangle.</p></td>
<td align="left">Utilisez la structure renvoyée par votre nuanceur de vertex comme structure d’entrée de votre nuanceur de pixels. Assurez-vous que les valeurs des sémantiques correspondent.</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>attribut</strong></p>
<p>Un attribut est un élément de la description d’un vertex que vous passez du code de l’application au nuanceur de vertex (et seulement à lui). Contrairement à la variable uniform, vous définissez une variable attribute par vertex. Vous pouvez ainsi attribuer une valeur différente à chacun des vertex. Les variables attribute s’appliquent au niveau de chaque vertex.</p></td>
<td align="left"><p>Définissez une mémoire tampon de vertex dans le code de votre application Direct3D, puis mappez-la à l’entrée de vertex que vous avez définie dans le nuanceur de vertex. Vous pouvez éventuellement définir une mémoire tampon d’index. Voir <a href="https://msdn.microsoft.com/library/windows/desktop/ff476899">Procédure: Création d’une mémoire tampon de vertex</a> et <a href="https://msdn.microsoft.com/library/windows/desktop/ff476897">Procédure: Création d’une mémoire tampon d’index</a>.</p>
<p>Créez une disposition d’entrée dans le code de votre application Direct3D, puis mappez les valeurs de sémantiques à celles de l’entrée de vertex. Voir <a href="https://msdn.microsoft.com/library/windows/desktop/bb205117#Create_the_Input_Layout">Créer la disposition d’entrée</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>Les constantes sont compilées dans le nuanceur; elles ne changent jamais.</p></td>
<td align="left">Utilisez une constante <strong>static const</strong>. <strong>static</strong> indique que la valeur n’est pas exposée aux mémoires tampons constantes et <strong>const</strong> qu’elle ne peut pas être modifiée par le nuanceur. La valeur est donc fournie au moment de la compilation, sur la base de l’initialiseur associé.</td>
</tr>
</tbody>
</table>

 

Dans GLSL, les variables sans modificateurs sont simplement des variables globales standards qui sont privées pour chaque nuanceur.

Lorsque vous transmettez des données à des textures ([Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525) dans HLSL) et à leurs échantillonneurs associés ([SamplerState](https://msdn.microsoft.com/library/windows/desktop/bb509644) dans HLSL), vous les déclarez généralement en tant que variables globales dans le nuanceur de pixels.

## <a name="porting-glsl-types-to-hlsl"></a>Portage des types GLSL vers HLSL


Référez-vous au tableau ci-dessous lors du portage de vos types GLSL vers HLSL.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Type GLSL</th>
<th align="left">Type HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Types scalaires : float, int, bool</td>
<td align="left"><p>Types scalaires : float, int, bool</p>
<p>(et uint, double)</p>
<p>Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">Types scalaires</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Type vector</p>
<ul>
<li>vecteur à virgule flottante : vec2, vec3, vec4</li>
<li>vecteur booléen : bvec2, bvec3, bvec4</li>
<li>vecteur avec entier signé : ivec2, ivec3, ivec4</li>
</ul></td>
<td align="left"><p>Type vector</p>
<ul>
<li>float2, float3, float4 et float1</li>
<li>bool2, bool3, bool4 et bool1</li>
<li>int2, int3, int4 et int1</li>
<li><p>Ces types comportent également des extensions de vecteur similaires aux types float, bool et int :</p>
<ul>
<li>uint</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/desktop/bb509707">Type vector</a> et <a href="https://msdn.microsoft.com/library/windows/desktop/bb509568">Mots clés</a>.</p>
<p>vector peut également être défini avec le type float4 (typedef vector &lt;float, 4&gt; vector;). Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">Type défini par l’utilisateur</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Type matrix</p>
<ul>
<li>mat2 : matrice flottante 2x2</li>
<li>mat3 : matrice flottante 3x3</li>
<li>mat4 : matrice flottante 4x4</li>
</ul></td>
<td align="left"><p>Type matrix</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>float1x1, float1x2, float1x3, float1x4, float2x1, float2x3, float2x4, float3x1, float3x2, float3x4, float4x1, float4x2, float4x3</li>
<li><p>Ces types comportent également des extensions de matrice similaires au type float :</p>
<ul>
<li>int, uint, bool</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>Vous pouvez aussi définir une matrice avec le <a href="https://msdn.microsoft.com/library/windows/desktop/bb509623">type matrix</a>.</p>
<p>Exemple : matrix &lt;float, 2, 2&gt; fMatrix = {0.0f, 0.1, 2.1f, 2.2f};</p>
<p>matrix peut également être défini avec le type float4x4 (typedef matrix &lt;float, 4, 4&gt; matrix;). Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">Type défini par l’utilisateur</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Qualificateurs de précision pour les types float, int, sampler</p>
<ul>
<li><p>highp</p>
<p>Ce qualificateur fournit une précision minimale requise supérieure à celle des valeurs min16float, mais inférieure à celle des valeurs flottantes 32 bits. Équivalence HLSL:</p>
<p>highp float -&gt; float</p>
<p>highp int -&gt; int</p></li>
<li><p>mediump</p>
<p>Ce qualificateur appliqué aux valeurs float et int équivaut aux valeurs min16float et min12int dans HLSL. Mantisse sur 10 bits au minimum, à la différence du type min10float.</p></li>
<li><p>lowp</p>
<p>Ce qualificateur appliqué aux valeurs float fournit une plage de valeurs à virgule flottante comprises entre -2 et 2. Équivaut au type min10float dans HLSL.</p></li>
</ul></td>
<td align="left"><p>Types de précision</p>
<ul>
<li>min16float : valeur à virgule flottante sur 16 bits au minimum</li>
<li><p>min10float</p>
<p>Valeur à virgule fixe signée sur 2,8 bits au minimum (2 bits pour la partie entière et 8 bits pour la partie fractionnaire). La partie fractionnaire sur 8 bits peut inclure la valeur 1, au lieu de l’exclure, afin de pouvoir attribuer n’importe quelle valeur de la plage de valeurs comprises entre -2 et 2.</p></li>
<li>min16int : entier signé sur 16 bits au minimum</li>
<li><p>min12int: entier signé 12bits au minimum</p>
<p>Ce type s’applique au niveau 10Level9 (voir les <a href="https://msdn.microsoft.com/library/windows/desktop/ff476876">niveaux de fonctionnalité9_x</a>), où les entiers sont représentés par des valeurs à virgule flottante. C’est la précision que vous obtenez en émulant un entier avec une valeur à virgule flottante sur 16bits.</p></li>
<li>min16uint: entier non signé 16bits au minimum</li>
</ul>
<p>Pour plus d’informations, voir <a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">Types scalaires</a> et <a href="https://msdn.microsoft.com/library/windows/desktop/hh968108">Utilisation de la précisionHLSL minimale</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a></td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509700">TextureCube</a></td>
</tr>
</tbody>
</table>

 

## <a name="porting-glsl-pre-defined-global-variables-to-hlsl"></a>Portage des variables globales prédéfinies GLSL vers HLSL


Référez-vous au tableau ci-dessous lors du portage de vos variables globales prédéfinies GLSL vers HLSL.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Variable globale prédéfinie GLSL</th>
<th align="left">Sémantiques HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>Variable du type <strong>vec4</strong>.</p>
<p>Position du vertex</p>
<p>Exemple : - gl_Position = position;</p></td>
<td align="left"><p>SV_Position</p>
<p>POSITION dans Direct3D 9</p>
<p>Sémantique du type <strong>float4</strong>.</p>
<p>Sortie du nuanceur de vertex</p>
<p>Position du vertex</p>
<p>Exemple: - float4 vPosition: SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>Variable du type <strong>float</strong>.</p>
<p>Taille du point</p></td>
<td align="left"><p>PSIZE</p>
<p>Pas de signification hormis pour Direct3D 9</p>
<p>Sémantique du type <strong>float</strong>.</p>
<p>Sortie du nuanceur de vertex</p>
<p>Taille du point</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>Variable du type <strong>vec4</strong>.</p>
<p>Couleur du fragment</p>
<p>Exemple : - gl_FragColor = vec4(colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>COLOR dans Direct3D 9</p>
<p>Sémantique du type <strong>float4</strong>.</p>
<p>Sortie du nuanceur de pixels</p>
<p>Couleur du pixel</p>
<p>Exemple: - float4 Color[4] : SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData[n]</strong></p>
<p>Variable du type <strong>vec4</strong>.</p>
<p>Couleur du fragment pour l’association de couleur n</p></td>
<td align="left"><p>SV_Target[n]</p>
<p>Sémantique du type <strong>float4</strong>.</p>
<p>Valeur de la sortie du nuanceur de pixels qui est stockée dans la cible de rendu n, où 0 &lt;= n &lt;= 7.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>Variable du type <strong>vec4</strong>.</p>
<p>Position du fragment dans le tampon de trame</p></td>
<td align="left"><p>SV_Position</p>
<p>Non disponible dans Direct3D 9</p>
<p>Sémantique du type <strong>float4</strong>.</p>
<p>Entrée du nuanceur de pixels</p>
<p>Coordonnées de l’espace à l’écran</p>
<p>Exemple: - float4 screenSpace : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>Variable du type <strong>bool</strong>.</p>
<p>Détermine si le fragment appartient à une primitive de face.</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>VFACE dans Direct3D 9</p>
<p>SV_IsFrontFace est du type <strong>bool</strong>.</p>
<p>VFACE est du type <strong>float</strong>.</p>
<p>Entrée du nuanceur de pixels</p>
<p>Primitive de face</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>Variable du type <strong>vec2</strong>.</p>
<p>Position du fragment dans un point (rastérisation des points uniquement)</p></td>
<td align="left"><p>SV_Position</p>
<p>VPOS dans Direct3D 9</p>
<p>SV_Position est du type <strong>float4</strong>.</p>
<p>VPOS est du type <strong>float2</strong>.</p>
<p>Entrée du nuanceur de pixels</p>
<p>Position du pixel ou de l’échantillon dans l’espace à l’écran</p>
<p>Exemple: - float4 pos : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>Variable du type <strong>float</strong>.</p>
<p>Données de la mémoire tampon de profondeur</p></td>
<td align="left"><p>SV_Depth</p>
<p>DEPTH dans Direct3D 9</p>
<p>SV_Depth est du type <strong>float</strong>.</p>
<p>Sortie du nuanceur de pixels</p>
<p>Données de la mémoire tampon de profondeur</p></td>
</tr>
</tbody>
</table>

 

Les sémantiques vous permettent de définir diverses valeurs (position, couleur, etc.) pour les données d’entrée du nuanceur de vertex et les données de sortie du nuanceur de pixels. Les valeurs de sémantiques définies dans la disposition d’entrée doivent correspondre aux données d’entrée du nuanceur de vertex. Pour plus d’informations, voir [Exemples de portage de variables GLSL vers HLSL](#examples-of-porting-glsl-variables-to-hlsl), ci-dessous. Pour plus d’informations sur les sémantiques HLSL, voir [Sémantiques](https://msdn.microsoft.com/library/windows/desktop/bb509647).

## <a name="examples-of-porting-glsl-variables-to-hlsl"></a>Exemples de portage de variables GLSL vers HLSL


Dans cette section, vous trouverez des exemples d’utilisation de variables GLSL dans le code OpenGL/GLSL, suivis des exemples équivalents pour le code Direct3D/HLSL.

### <a name="uniform-attribute-and-varying-in-glsl"></a>Variables uniform, attribute et varying dans GLSL

Code de l’application OpenGL

``` syntax
// Uniform values can be set in app code and then processed in the shader code.
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// Incoming position of vertex
attribute vec4 position;
 
// Incoming color for the vertex
attribute vec3 color;
 
// The varying variable tells the shader pipeline to pass it  
// on to the fragment shader.
varying vec3 colorVarying;
```

Code GLSL du nuanceur de vertex

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

Code GLSL du nuanceur de fragments

``` syntax
void main()
{
//Pad the colorVarying vec3 with a 1.0 for alpha to create a vec4 color
//and assign that color to the gl_FragColor pre-defined global variable
//This color then becomes the fragment's color.
gl_FragColor = vec4(colorVarying, 1.0);
}
```

### <a name="constant-buffers-and-data-transfers-in-hlsl"></a>Mémoires tampons constantes et transferts de données dans HLSL

Voici un exemple du processus de passage des données au nuanceur de vertex HLSL, puis au nuanceur de pixels. Dans le code de votre application, définissez un vertex et une mémoire tampon constante. Ensuite, dans le code de votre nuanceur de vertex, définissez la mémoire tampon constante avec la constante [cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581), et enregistrez les données de vertex et les données d’entrée du nuanceur de pixels. Dans cet exemple, nous utilisons les structures **VertexShaderInput** et **PixelShaderInput**.

Code de l’application Direct3D

```cpp
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;
};
struct SimpleCubeVertex
{
    XMFLOAT3 pos;   // position
    XMFLOAT3 color; // color
};

 // Create an input layout that matches the layout defined in the vertex shader code.
 const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
 {
     { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
     { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
 };

// Create vertex and index buffers that define a geometry.
```

Code HLSL du nuanceur de vertex

``` syntax
cbuffer ModelViewProjectionCB : register( b0 )
{
    matrix model; 
    matrix view;
    matrix projection;
};
// The POSITION and COLOR semantics must match the semantics in the input layout Direct3D app code.
struct VertexShaderInput
{
    float3 pos : POSITION; // Incoming position of vertex 
    float3 color : COLOR; // Incoming color for the vertex
};

struct PixelShaderInput
{
    float4 pos : SV_Position; // Copy the vertex position.
    float4 color : COLOR; // Pass the color to the pixel shader.
};

PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // shader source code

    return vertexShaderOutput;
}
```

Code HLSL du nuanceur de pixels

``` syntax
// Collect input from the vertex shader. 
// The COLOR semantic must match the semantic in the vertex shader code.
struct PixelShaderInput
{
    float4 pos : SV_Position;
    float4 color : COLOR; // Color for the pixel
};

// Set the pixel color value for the renter target. 
float4 main(PixelShaderInput input) : SV_Target
{
    return input.color;
}
```

## <a name="examples-of-porting-opengl-rendering-code-to-direct3d"></a>Exemples de portage du code de rendu OpenGL sur Direct3D


Vous trouverez ci-dessous un exemple de code de rendu pour OpenGL ES 2.0 et son équivalent pour Direct3D 11.

Code de rendu pour OpenGL

``` syntax
// Bind shaders to the pipeline. 
// Both vertex shader and fragment shader are in a program.
glUseProgram(m_shader->getProgram());
 
// Input asssembly 
// Get the position and color attributes of the vertex.

m_positionLocation = glGetAttribLocation(m_shader->getProgram(), "position");
glEnableVertexAttribArray(m_positionLocation);

m_colorLocation = glGetAttribColor(m_shader->getProgram(), "color");
glEnableVertexAttribArray(m_colorLocation);
 
// Bind the vertex buffer object to the input assembler.
glBindBuffer(GL_ARRAY_BUFFER, m_geometryBuffer);
glVertexAttribPointer(m_positionLocation, 4, GL_FLOAT, GL_FALSE, 0, NULL);
glBindBuffer(GL_ARRAY_BUFFER, m_colorBuffer);
glVertexAttribPointer(m_colorLocation, 3, GL_FLOAT, GL_FALSE, 0, NULL);
 
// Draw a triangle with 3 vertices.
glDrawArray(GL_TRIANGLES, 0, 3);
```

Code de rendu pour Direct3D

```cpp
// Bind the vertex shader and pixel shader to the pipeline.
m_d3dDeviceContext->VSSetShader(vertexShader.Get(),nullptr,0);
m_d3dDeviceContext->PSSetShader(pixelShader.Get(),nullptr,0);
 
// Declare the inputs that the shaders expect.
m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());
m_d3dDeviceContext->IASetVertexBuffers(0, 1, vertexBuffer.GetAddressOf(), &stride, &offset);

// Set the primitive's topology.
m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Draw a triangle with 3 vertices. triangleVertices is an array of 3 vertices.
m_d3dDeviceContext->Draw(ARRAYSIZE(triangleVertices),0);
```

## <a name="related-topics"></a>Rubriques connexes


* [Portage d’OpenGL ES 2.0 vers Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 




