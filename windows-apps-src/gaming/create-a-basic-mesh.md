---
author: mtoepke
title: Créer et afficher un maillage de base
description: Les jeux de plateforme Windows universelle (UWP) 3D utilisent généralement des polygones pour représenter les objets et les surfaces dans le jeu.
ms.assetid: bfe0ed5b-63d8-935b-a25b-378b36982b7d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, uwp, jeux, maillage, directx
ms.localizationpriority: medium
ms.openlocfilehash: e3ae6416217efa16d70b65b8ff55e36654a11557
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7290685"
---
# <a name="create-and-display-a-basic-mesh"></a>Créer et afficher un maillage de base



Les jeux de plateforme Windows universelle (UWP) 3D utilisent généralement des polygones pour représenter les objets et les surfaces dans le jeu. Les listes de vertex qui forment la structure de ces surfaces et objets polygonaux sont appelées maillages. Ici, nous créons un maillage de base pour un objet cube et le fournissons au pipeline nuanceur pour le rendu et l’affichage.

> **Important**  l’exemple de code inclus ici utilise des types (DirectX::XMFLOAT3 et DirectX::XMFLOAT4X4) et des méthodes inline déclarées dans DirectXMath.h. Si vous coupez et collez ce code, ajoutez \#include &lt;DirectXMath.h&gt; dans votre projet.

 

## <a name="what-you-need-to-know"></a>Ce que vous devez savoir


### <a name="technologies"></a>Technologies

-   [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh769064)

### <a name="prerequisites"></a>Prérequis

-   Notions de base d’algèbre linéaire et de systèmes de coordonnées 3D
-   Un Visual Studio 2015 ou une version ultérieure modèle Direct3D

## <a name="instructions"></a>Instructions

Ces étapes vous montrent comment créer un cube de maillage de base. 


Si vous préférez une explication vocale de ces concepts, regardez cette vidéo.
</br>
</br>
<iframe src="https://channel9.msdn.com/Series/Introduction-to-C-and-DirectX-Game-Development/03/player#time=7m39s:paused" width="600" height="338" allowFullScreen frameBorder="0"></iframe>


### <a name="step-1-construct-the-mesh-for-the-model"></a>Étape1: Construire le maillage pour le modèle

Dans la plupart des jeux, le maillage d’un objet du jeu est chargé à partir d’un fichier qui contient les données de vertex spécifiques. L’organisation de ces vertex dépend de l’application, mais généralement ils sont sérialisés en bandes ou en éventails. Les données de vertex peuvent provenir de n’importe quelle source logicielle, ou être créées manuellement. C’est votre jeu qui interprète les données de manière compatible avec le traitement du nuanceur de vertex.

Dans notre exemple, nous utilisons un maillage simple de cube. Le cube, comme n’importe quel maillage à ce stade dans le pipeline, est représenté dans son propre système de coordonnées. Le nuanceur de vertex prend les coordonnées, applique les matrices de transformation que vous fournissez et retourne la projection finale de vue 2D dans un système de coordonnées homogène.

Définir le maillage d’un cube. (Ou chargez-le à partir d’un fichier. C’est à vous de décider !)

```cpp
SimpleCubeVertex cubeVertices[] =
{
    { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
    { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 0.0f) },
    { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 1.0f) },
    { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 1.0f) },

    { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
    { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 1.0f) },
    { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f) },
    { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f) },
};
```

Le système de coordonnées du cube place le centre du cube à l’origine, l’axe Y orienté du haut vers le bas selon un système de coordonnées pour gaucher. Les valeurs de coordonnées sont exprimées par des valeurs flottantes 32 bits comprises entre -1 et 1.

Dans chaque paire entre crochets, le deuxième groupe de valeurs DirectX::XMFLOAT3 spécifie la couleur associée au vertex sous forme de valeur RVB. Par exemple, le premier vertex (-0,5 0,5 -0,5) a une couleur totalement verte (la valeur V est définie sur 1.0, et les valeurs R et B sont définies sur 0).

Par conséquent, vous avez 8 vertex, chacun avec une couleur spécifique. Chaque paire vertex/couleur représente les données complètes pour un vertex dans notre exemple. Lorsque vous spécifiez la mémoire tampon de vertex, vous devez garder ce schéma-là à l’esprit. Nous fournissons ce schéma d’entrée au nuanceur de vertex afin qu’il puisse interpréter les données de vertex.

### <a name="step-2-set-up-the-input-layout"></a>Étape2: Configurer le schéma d’entrée

Les vertex figurent à présent dans la mémoire. Toutefois, votre périphérique graphique dispose de sa propre mémoire, et vous utilisez Direct3D pour y accéder. Pour entrer vos données vertex dans le périphérique graphique à des fins de traitement, vous devez ouvrir la voie, pour ainsi dire, en déclarant la façon dont les données vertex sont disposées pour que le périphérique graphique puisse les interpréter lorsqu’il les obtient à partir de votre jeu. Pour ce faire, vous devez utiliser [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575).

Déclarez et définissez le schéma d’entrée pour la mémoire tampon de vertex.

```cpp
const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

ComPtr<ID3D11InputLayout> inputLayout;
m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                vertexShaderBytecode->Data,
                vertexShaderBytecode->Length,
                &inputLayout)
);
```

Dans ce code, vous spécifiez un schéma pour les vertex, en particulier, les données que chaque élément de la liste de vertex contient. Ici, dans **basicVertexLayoutDesc**, vous spécifiez deuxcomposants de données:

-   **POSITION**: sémantique HLSL pour les données de position fournies à un nuanceur. Dans ce code, il s’agit d’un DirectX::XMFLOAT3, ou plus précisément, d’une structure avec 3valeurs à virgule flottante 32bits qui correspondent à des coordonnées 3D (x, y, z). Vous pouvez également utiliser un float4 si vous fournissez la coordonnée homogène (w), auquel cas vous spécifiez DXGI\_FORMAT\_R32G32B32A32\_FLOAT. Le fait que vous utilisiez un DirectX::XMFLOAT3 ou un float4 dépend des besoins spécifiques de votre jeu. Assurez-vous simplement que les données de vertex de votre maillage correspondent bien au format que vous utilisez !

    Chaque valeur de coordonnée est exprimée sous forme de valeur à virgule flottante comprise entre -1 et 1, dans l’espace de coordonnées de l’objet. Lorsque le nuanceur de vertex a terminé, le vertex transformé est dans l’espace de projection de vue homogène (perspective corrigée).

    «Mais la valeur de l’énumération indique RVB, pas XYZ!», remarquez-vous intelligemment. Bien vu! Dans les deux cas, pour les données de couleur et les données de coordonnées, vous utilisez en général 3 ou 4 valeurs composantes, alors pourquoi ne pas utiliser le même format dans les deux cas ? La sémantique HLSL, pas le nom du format, indique comment le nuanceur traite les données.

-   **COLOR**: sémantique HLSL pour les données de couleur. Comme **POSITION**, il s’agit de 3valeurs à virgule flottante 32bits (DirectX::XMFLOAT3). Chaque valeur contient une composante de couleur: rouge (r), bleu (b) ou vert (v), exprimée par un nombre flottant compris entre 0 et 1.

    Les valeurs **COLOR** sont généralement renvoyées comme valeur RVBA à 4composantes en sortie du pipeline nuanceur. Pour cet exemple, vous définirez la valeur alpha A sur 1,0 (opacité maximale) dans le pipeline nuanceur pour tous les pixels.

Pour obtenir la liste complète des formats, voir [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059). Pour consulter la liste complète des sémantiques HLSL, voir [Sémantique](https://msdn.microsoft.com/library/windows/desktop/bb509647).

Appelez [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) et créez la disposition d’entrée sur le périphérique Direct3D. Maintenant, vous devez créer une mémoire tampon qui puisse réellement contenir les données!

### <a name="step-3-populate-the-vertex-buffers"></a>Étape3: Remplir les mémoires tampon de vertex

Les mémoires tampon de vertex contiennent la liste des vertex de chaque triangle du maillage. Chaque vertex doit être unique dans cette liste. Dans notre exemple, il y a 8 vertex pour le cube. Le nuanceur de vertex s’exécute sur le périphérique graphique et lit la mémoire tampon de vertex ; il interprète les données en fonction du schéma d’entrée spécifié à l’étape précédente.

Dans l’exemple suivant, vous fournissez une description et une sous-ressource pour la mémoire tampon, qui donne à Direct3D un certain nombre d’indications sur le mappage physique des données de vertex et la façon de les traiter en mémoire sur le périphérique graphique. Cela est nécessaire, car vous utilisez un [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) générique, qui peut contenir n’importe quoi. Les structures [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) et [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) sont fournies afin de s’assurer que Direct3D comprend la configuration de la mémoire physique du tampon, notamment la taille de chaque élément vertex dans la mémoire tampon ainsi que la taille maximale de la liste de vertex. Vous pouvez également contrôler l’accès à la mémoire tampon ici et la façon de la parcourir, mais ce n’est pas vraiment l’objet de ce didacticiel.

Après avoir configuré la mémoire tampon, vous appelez [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) pour la créer effectivement. Évidemment, si vous avez plusieurs objets, créez des tampons pour chaque modèle unique.

Déclarez et créez la mémoire tampon de vertex.

```cpp
D3D11_BUFFER_DESC vertexBufferDesc = {0};
vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
vertexBufferDesc.CPUAccessFlags = 0;
vertexBufferDesc.MiscFlags = 0;
vertexBufferDesc.StructureByteStride = 0;

D3D11_SUBRESOURCE_DATA vertexBufferData;
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;

ComPtr<ID3D11Buffer> vertexBuffer;
m_d3dDevice->CreateBuffer(
                &vertexBufferDesc,
                &vertexBufferData,
                &vertexBuffer);
```

Vertex chargés. Quel est l’ordre de traitement de ces vertex? Cela est géré lorsque vous fournissez une liste d’index aux vertex; l’ordre de ces index est l’ordre dans lequel les traite le nuanceur de vertex.

### <a name="step-4-populate-the-index-buffers"></a>Étape4: Remplir les mémoires tampon d’index

Maintenant, vous fournissez une liste d’index pour tous les vertex. Ces index correspondent à la position du vertex dans la mémoire tampon de vertex, en commençant à 0. Pour vous aider à visualiser cela, considérez qu’un numéro unique est attribué à chaque vertex de votre maillage, comme un ID. Cet ID est la position (nombre entier) du vertex dans la mémoire tampon.

![cube à 8 vertex numérotés](images/cube-mesh-1.png)

Pour le cube de notre exemple, vous avez 8vertex, ce qui crée 6quadruplés pour les faces. Vous fractionnez les quadruplés en triangles, soit un total de 12 triangles qui utilisent nos 8 vertex. Avec 3vertex par triangle, cela fait 36entrées dans notre tampon d’index. Dans notre exemple, ce modèle d’index est appelé liste de triangles, et vous l’indiquez à Direct3D comme **D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLELIST** lorsque vous définissez la topologie primitive.

C’est probablement la façon la plus inefficace de lister des index, car il y a de nombreuses redondances lorsque les triangles partagent des points et des côtés. Par exemple, lorsqu’un triangle partage un côté dans une forme losange, vous répertoriez 6 index pour les 4 vertex, comme ceci :

![ordre des index lorsque vous construisez un losange](images/rhombus-surface-1.png)

-   Triangle 1: \[0, 1, 2\]
-   Triangle 2: \[0, 2, 3\]

Dans une topologie de bande ou en éventail, vous ordonnez les vertex d’une manière qui élimine beaucoup de côtés redondants lors du parcours (par exemple, le côté de l’index 0 à l’index 2 dans l’image.) Pour les grands maillages, cela réduit considérablement le nombre d’exécutions du nuanceur de vertex et améliore beaucoup les performances. Cependant, pour simplifier les choses, nous nous en tenons à la liste de triangles.

Déclarez les index pour le tampon de vertex comme une topologie simple de liste de triangles.

```cpp
unsigned short cubeIndices[] =
{   0, 1, 2,
    0, 2, 3,

    4, 5, 6,
    4, 6, 7,

    3, 2, 5,
    3, 5, 4,

    2, 1, 6,
    2, 6, 5,

    1, 7, 6,
    1, 0, 7,

    0, 3, 4,
    0, 4, 7 };
```

La redondance est élevée avec 36éléments indexés dans le tampon pour 8vertex seulement! Si vous choisissez d’éliminer certaines redondances et d’utiliser un type différent de liste de vertex, comme une bande ou un éventail, vous devez spécifier ce type lorsque vous fournissez une valeur [**D3D11\_PRIMITIVE\_TOPOLOGY**](https://msdn.microsoft.com/library/windows/desktop/ff476189) spécifique à la méthode [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455).

Pour plus d’informations sur les différentes techniques de listage d’index, voir [Topologies primitives](https://msdn.microsoft.com/library/windows/desktop/bb205124).

### <a name="step-5-create-a-constant-buffer-for-your-transformation-matrices"></a>Étape5: Créer un tampon constant pour vos matrices de transformation

Avant de pouvoir commencer à traiter des vertex, vous devrez fournir les matrices de transformation qui seront appliquées (multipliées) à chaque vertex au moment de l’exécution. Pour la plupart des jeux 3D, il en existe trois :

-   La matrice 4 x 4 qui transforme le système de coordonnées de l’objet (modèle) en système de coordonnées universel global.
-   La matrice 4 x 4 qui transforme le système de coordonnées universel en système de coordonnées caméra (vue).
-   La matrice 4 x 4 qui transforme le système de coordonnées caméra en système de coordonnées de projection de vue 2D.

Ces matrices sont passées au nuanceur dans une *mémoire tampon constante*. Un tampon constant est une région de mémoire qui reste constante tout au long de l’exécution de la passe suivante du pipeline nuanceur, et qui est directement accessible par les nuanceurs à partir de votre code HLSL. Vous définissez chaque tampon constant deux fois : d’abord dans le code C++ de votre jeu et (au moins) une fois dans la syntaxe HLSL, proche de celle du C, du code de votre nuanceur. Les deux déclarations doivent correspondre directement en termes de types et d’alignement des données. Il est facile d’introduire des erreurs difficiles à détecter lorsque le nuanceur utilise la déclaration HLSL pour interpréter les données déclarées en C++ ; les types ne correspondent pas ou l’alignement des données est mauvais !

Les tampons constants ne sont modifiés par le langage HLSL. Vous pouvez les modifier lorsque votre jeu met à jour des données spécifiques. Les développeurs de jeux créent souvent 4 classes de tampons constants : un type pour les mises à jour par image, un type pour les mises à jour par modèle/objet, un type pour les mises à jour par actualisation de l’état du jeu et un type pour les données qui ne changent jamais tout au long de la durée de vie du jeu.

Dans cet exemple, nous n’avons qu’un type de données qui ne changent jamais: les données DirectX::XMFLOAT4X4 pour les trois matrices.

> **Remarque**  l’exemple de code présenté ici utilise des matrices column-major. Vous pouvez sinon utiliser des matrices ordonnées par lignes à l’aide du mot clé **row\_major** en HLSL pour vous assurer que vos données de la matrice source sont également ordonnées par lignes. DirectXMath utilise des matrices ordonnées par lignes et peut être utilisé directement avec des matrices HLSL définies avec le mot-clé **row\_major**.

 

Déclarez et créez un tampon constant pour les trois matrices que vous utilisez pour transformer chaque vertex.

```cpp
struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};
ComPtr<ID3D11Buffer> m_constantBuffer;
ConstantBuffer m_constantBufferData;

// ...

// Create a constant buffer for passing model, view, and projection matrices
// to the vertex shader.  This allows us to rotate the cube and apply
// a perspective projection to it.

D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags = 0;
constantBufferDesc.MiscFlags = 0;
constantBufferDesc.StructureByteStride = 0;
m_d3dDevice->CreateBuffer(
                &constantBufferDesc,
                nullptr,
                &m_constantBuffer
             );

m_constantBufferData.model = DirectX::XMFLOAT4X4( // Identity matrix, since you are not animating the object
            1.0f, 0.0f, 0.0f, 0.0f,
            0.0f, 1.0f, 0.0f, 0.0f,
            0.0f, 0.0f, 1.0f, 0.0f,
            0.0f, 0.0f, 0.0f, 1.0f);

);
// Specify the view (camera) transform corresponding to a camera position of
// X = 0, Y = 1, Z = 2.  

m_constantBufferData.view = DirectX::XMFLOAT4X4(
            -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
             0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
             0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
             0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f);
```

> **Remarque**vous déclarez généralement la matrice de projection lorsque vous configurez les ressources spécifiques de l’appareil, car les résultats de la multiplication avec elle doivent correspondre à des paramètres de taille de fenêtre d’affichage 2D actuels (qui correspondent souvent à la hauteur en pixels et la largeur de la affichage). Si ceux-ci changent, vous devez mettre à l’échelle les valeurs des coordonnées x et y en conséquence.

 

```cpp
// Finally, update the constant buffer perspective projection parameters
// to account for the size of the application window.  In this sample,
// the parameters are fixed to a 70-degree field of view, with a depth
// range of 0.01 to 100.  

float xScale = 1.42814801f;
float yScale = 1.42814801f;
if (backBufferDesc.Width > backBufferDesc.Height)
{
    xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
}
else
{
    yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
}
m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

Définissez ici les tampons de vertex et d’index dans le contexte [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476149), ainsi que la topologie que vous utilisez.

```cpp
// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(SimpleCubeVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset);

m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0);

 m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
```

Parfait ! L’assembly d’entrée est terminé. Tout est en place pour le rendu. Laissons le nuanceur de vertex se mettre au travail.

### <a name="step-6-process-the-mesh-with-the-vertex-shader"></a>Étape6: Traiter le maillage avec le nuanceur de vertex

Maintenant que vous avez un tampon de vertex avec les vertex qui définissent votre maillage et un tampon d’index qui définit l’ordre dans lequel les vertex sont traités, vous les envoyez au nuanceur de vertex. Le code du nuanceur de vertex, exprimé dans un langage de haut niveau compilé, s’exécute une fois pour chaque vertex dans la mémoire tampon de vertex, vous permettant d’effectuer vos transformations par vertex. Le résultat final est en général une projection 2D.

(Avez-vous chargé votre nuanceur de vertex ? Si ce n’est pas le cas, voir [Comment charger les ressources dans votre jeu DirectX](load-a-game-asset.md).)

Ici, vous créez le nuanceur de vertex...

``` syntax
// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0);
```

... et définissez les tampons constants.

``` syntax
m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf());
```

Voici le code du nuanceur de vertex qui gère la transformation des coordonnées de l’objet en coordonnées universelles, puis vers le système de coordonnées de projection de vue 2D. Vous appliquez également un éclairage simple par vertex pour enjoliver les choses. Cela va dans le fichier HLSL de votre nuanceur de vertex (SimplerVertexShader.hlsl, dans cet exemple).

``` syntax
cbuffer simpleConstantBuffer : register( b0 )
{
    matrix model;
    matrix view;
    matrix projection;
};

struct VertexShaderInput
{
    DirectX::XMFLOAT3 pos : POSITION;
    DirectX::XMFLOAT3 color : COLOR;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
    float4 color : COLOR;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projection space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    vertexShaderOutput.pos = pos;

    // Pass the vertex color through to the pixel shader.
    vertexShaderOutput.color = float4(input.color, 1.0f);

    return vertexShaderOutput;
}
```

Notez ce **cbuffer** en haut: c’est l’élément HLSL analogue au tampon constant que nous avons déclaré précédemment dans notre code C++. Et le **VertexShaderInputstruct**: Cela ressemble au schéma d’entrée et à la déclaration des données de vertex! Il est important que le tampon constant et que les déclarations des données de vertex dans votre code C++ correspondent aux déclarations dans votre code HLSL, en incluant les types, les signes et l’alignement des données.

**PixelShaderInput** spécifie la disposition des données qui sont retournées par la fonction principale du nuanceur de vertex. Lorsque vous terminez le traitement d’un vertex, vous retournez une position du vertex dans l’espace de projection 2D et une couleur utilisée pour l’éclairage par vertex. La carte graphique utilise la sortie de données du nuanceur pour calculer les «fragments» (pixels possibles) qui doivent être colorés lorsque le nuanceur de pixels est exécuté dans l’étape suivante du pipeline.

### <a name="step-7-passing-the-mesh-through-the-pixel-shader"></a>Étape7: Passage du maillage par le nuanceur de pixels

En général, à ce stade dans le pipeline graphique, vous effectuez les opérations au niveau pixel sur les surfaces projetées visibles de vos objets. (Les gens aiment les textures.) Dans le cadre de cet exemple, cependant, cette étape se résume à un simple passage.

Tout d’abord, nous allons créer une instance du nuanceur de pixels. Le nuanceur de pixels s’exécute pour chaque pixel de la projection 2D de votre scène, en attribuant une couleur à ce pixel. Dans ce cas, nous passons la couleur du pixel retourné par le nuanceur de vertex directement.

Définissez le nuanceur de pixels.

``` syntax
m_d3dDeviceContext->PSSetShader( pixelShader.Get(), nullptr, 0 );
```

Définissez un nuanceur de pixels intermédiaire en HLSL.

``` syntax
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

Placez ce code dans un fichier HLSL distinct du code HLSL du nuanceur de vertex (tel que SimplePixelShader.hlsl). Ce code est exécuté une seule fois pour chaque pixel visible dans votre fenêtre d’affichage (une représentation en mémoire de la portion de l’écran dans laquelle vous dessinez) qui, dans ce cas, correspond à la totalité de l’écran. Maintenant, votre pipeline graphique est complètement défini!

### <a name="step-8-rasterizing-and-displaying-the-mesh"></a>Étape8: Rastérisation et affichage du maillage

Nous allons exécuter le pipeline. C’est simple: appelez [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/bb173565).

Dessinez ce cube !

```cpp
// Draw the cube.
m_d3dDeviceContext->DrawIndexed( ARRAYSIZE(cubeIndices), 0, 0 );
            
```

À l’intérieur de la carte graphique, chaque vertex est traité dans l’ordre spécifié par votre tampon d’index. Après avoir exécuté le nuanceur de vertex et les fragments 2D étant définis, le nuanceur de pixels est appelé et les triangles sont colorés.

Maintenant, placez le cube sur l’écran.

Présentez ce tampon de trame pour l’affichage.

```cpp
// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop is generally  throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the app on Present until the screen is refreshed.

m_swapChain->Present(1, 0);
```

Vous avez terminé ! Pour une scène remplie de modèles, utilisez plusieurs tampons de vertex et tampons d’index ; vous pourriez même avoir différents nuanceurs pour les différents types de modèles. N’oubliez pas que chaque modèle possède son propre système de coordonnées, et que vous devez les transformer vers le système de coordonnées universelles partagé en utilisant les matrices que vous avez définies dans le tampon constant.

## <a name="remarks"></a>Notes

Cette rubrique couvre la création et l’affichage d’une géométrie simple que vous créez vous-même. Pour plus d’informations sur le chargement d’une géométrie plus complexe à partir d’un fichier et la conversion au format de l’objet tampon de vertex (.vbo) spécifique à l’exemple, voir [Comment charger les ressources dans votre jeu DirectX](load-a-game-asset.md).  

 

## <a name="related-topics"></a>Rubriques connexes


* [Comment charger les ressources dans votre jeu DirectX](load-a-game-asset.md)

 

 




