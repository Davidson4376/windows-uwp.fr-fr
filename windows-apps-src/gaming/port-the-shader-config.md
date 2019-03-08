---
title: Porter les objets nuanceur
description: Dans le cadre du portage du convertisseur simple OpenGL ES 2.0, vous devez commencer par créer les objets des nuanceurs de vertex et de fragments équivalents dans Direct3D 11, mais également vous assurer que le programme principal sera en mesure de communiquer avec ces différents objets une fois compilés.
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux , portage, nuanceur, direct3d, opengl
ms.localizationpriority: medium
ms.openlocfilehash: f061d31ca779cb4c6cbe76f163e190996a6985cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618744"
---
# <a name="port-the-shader-objects"></a>Porter les objets nuanceur




**API importantes**

-   [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)
-   [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)

Dans le cadre du portage du convertisseur simple OpenGL ES 2.0, vous devez commencer par créer les objets des nuanceurs de vertex et de fragments équivalents dans Direct3D 11, mais également vous assurer que le programme principal sera en mesure de communiquer avec ces différents objets une fois compilés.

> **Remarque**    vous avez créé un nouveau projet de Direct3D ? suivez les instructions fournies dans l’article [Créer un projet DirectX 11 pour la plateforme Windows universelle (UWP)](user-interface.md). Cette procédure pas à pas suppose que vous disposez des ressources DXGI et Direct3D nécessaires pour le dessin à l’écran (celles fournies dans le modèle).

 

De la même façon que dans OpenGL ES 2.0, les nuanceurs compilés dans Direct3D doivent être associés à un contexte de dessin. Toutefois, comme Direct3D n’utilise pas le concept d’objet programme de nuanceur, vous devez à la place affecter directement les nuanceurs à un objet [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385). Cette étape, qui suit le processus OpenGL ES 2.0 de création et de liaison d’objets nuanceur, vous permet d’obtenir les comportements d’API correspondants dans Direct3D.

<a name="instructions"></a>Instructions
------------

### <a name="step-1-compile-the-shaders"></a>Étape 1 : Compiler les nuanceurs

Dans cet exemple simple de code OpenGL ES 2.0, les nuanceurs sont enregistrés sous forme de fichiers texte, puis chargés en tant que données de type chaîne à compiler au moment de l’exécution.

OpenGL ES 2.0 : Compiler un nuanceur

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

Dans Direct3D, les nuanceurs ne sont pas compilés au moment de l’exécution : ils sont toujours compilés dans des fichiers CSO en même temps que les autres éléments du programme. Lorsque vous compilez votre application avec Microsoft Visual Studio, les fichiers HLSL sont compilés dans des fichiers CSO (.cso) que votre application doit ensuite charger. Vous devez donc veiller à bien inclure ces fichiers CSO dans le package de votre application !

> **Remarque**    l’exemple suivant effectue la nuanceur le chargement et la compilation à l’aide de façon asynchrone le **automatique** syntaxe lambda et le mot clé. La méthode ReadDataAsync(), implémentée pour le modèle, lit les données d’un fichier CSO sous forme de tableau d’octets (fileData).

 

Direct3D 11 : Compiler un nuanceur

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>Étape 2 : Créer et charger le vertex et des nuanceurs (en pixels) du fragment

OpenGL ES 2.0 renferme la notion de « programme » de nuanceur. Ce programme joue le rôle d’interface de communication entre le programme principal exécuté par le processeur UC et les différents nuanceurs qui sont exécutés par le processeur GPU. Les nuanceurs sont compilés (ou chargés à partir des ressources compilées), puis ils sont associés à un programme exécutable par le processeur GPU.

OpenGL ES 2.0 : Charger les nuanceurs de sommets et de fragment dans un programme de trame

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

Direct3D n’utilise pas le concept d’objet programme de nuanceur. À la place, les nuanceurs sont créés lors de l’appel d’une méthode de création de nuanceur sur l’interface [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (par exemple, [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) ou [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)). Pour définir des nuanceurs adaptés au contexte de dessin actuel, nous les associons aux objets [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) correspondants à l’aide d’une méthode « set shader », telle que [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) pour le nuanceur de vertex ou [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) pour le nuanceur de fragments.

Direct3D 11 : Définissez les nuanceurs pour le contexte de dessin du périphérique graphique.

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>Étape 3 : Définir les données à fournir pour les nuanceurs

Dans notre exemple de code OpenGL ES 2.0, nous devons déclarer la variable **uniform** pour le pipeline des nuanceurs :

-   **u\_mvpMatrix**: un tableau de 4 x 4 de valeurs en virgule flottante qui représente la matrice de transformation de modèle-vue-projection finale qui utilise le modèle de coordonnées pour le cube et les transforme en coordonnées 2D de projection pour la conversion d’analyse.

Nous devons également déclarer deux valeurs **attribute** pour les données de vertex :

-   **un\_position**: un vecteur de 4-float pour les coordonnées de modèle d’un vertex.
-   **un\_couleur**: Un vecteur de 4-float pour la valeur de couleur RVBA associé au vertex.

Open GL ES 2.0 : Définitions de GLSL pour les attributs et uniformes

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

Ici, les variables du programme principal correspondantes sont définies sous forme de champs de l’objet convertisseur. (Reportez-vous à l’en-tête dans [Comment : port un simple convertisseur OpenGL ES 2.0 vers Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md).) Une fois que nous avons fait cela, nous devons spécifier les emplacements dans la mémoire où le programme principal fournit ces valeurs pour le pipeline de nuanceur, ce que nous faisons généralement juste avant un appel de dessin :

OpenGL ES 2.0 : L’emplacement de données uniforme et d’attribut

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

Direct3D n’utilise pas le concept de variables uniformes (« uniform ») et d’attributs (« attribute ») sous le même angle (ou pas avec la même syntaxe). À la place, il se sert de mémoires tampons constantes, représentant des sous-ressources Direct3D qui sont partagées entre le programme principal et les programmes de nuanceur. Certaines de ces sous-ressources, telles que les positions et couleurs de vertex, sont décrites sous forme de sémantiques HLSL. Pour plus d’informations sur les mémoires tampons constantes et les sémantiques HLSL sous-jacentes aux concepts OpenGL ES 2.0, voir [Porter des objets tampon de trame, des variables uniformes et des attributs](porting-uniforms-and-attributes.md).

Pour adapter ce processus à Direct3D, nous convertissons la variable uniforme en mémoire tampon constante Direct3D (cbuffer) et nous lui affectons un registre pour la recherche, avec la sémantique HLSL **register**. Les deux attributs de vertex sont gérés en tant qu’éléments d’entrée lors des étapes du pipeline de nuanceurs et sont également associés à des [sémantiques HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574) (POSITION et COLOR0) qui renseignent les nuanceurs sur la position et la couleur du vertex. Le nuanceur de pixels prend un SV\_POSITION, avec la VP\_ préfixe indiquant qu’il est une valeur de système générée par le GPU. (Dans ce cas, il est une position de pixel générée pendant la conversion d’analyse.) VertexShaderInput et PixelShaderInput ne sont pas déclarés comme constante met en mémoire tampon car le premier sera utilisé pour définir la mémoire tampon vertex (consultez [les mémoires tampons de vertex et les données de Port](port-the-vertex-buffers-and-data-config.md)), et les données de ce dernier sont générées comme résultat d’un étape précédente dans le pipeline, ce qui est dans ce cas le nuanceur de sommets.

Direct3D : Définitions de HLSL pour les mémoires tampons constantes et les données de vertex

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Pour plus d’informations sur le portage des mémoires tampons constantes et l’application de sémantiques HLSL, voir [Porter des objets tampon de trame, des variables uniformes et des attributs](porting-uniforms-and-attributes.md).

Voici les structures de disposition des données qui sont transmises au pipeline de nuanceurs avec une mémoire tampon constante ou de vertex.

Direct3D 11 : Déclaration de la disposition de mémoires tampons vertex et (constante)

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

Utiliser le XM DirectXMath\* types pour la constante de la mémoire tampon éléments, car ils fournissent compression appropriée et l’alignement du contenu lorsqu’ils sont envoyés vers le pipeline de nuanceur. Si vous utilisez les types et tableaux de valeurs flottantes standard de la plateforme Windows, vous devez effectuer le packaging et l’alignement manuellement.

Pour lier un mémoire tampon constante, créez une description de la mise en page comme un [ **CD3D11\_tampon\_DESC** ](https://msdn.microsoft.com/library/windows/desktop/jj151620) structurer et transmettez-le à [ **ID3DDevice :: CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501). Ensuite, dans votre méthode de rendu, transmettez la mémoire tampon constante à [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) avant de procéder au dessin.

Direct3D 11 : Lier la mémoire tampon constante

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

La mémoire tampon de vertex est créée et mise à jour de manière similaire. Nous verrons cela dans le cadre de la prochaine étape, [Porter les mémoires tampons et données de vertex](port-the-vertex-buffers-and-data-config.md).

<a name="next-step"></a>Étape suivante
---------

[Les mémoires tampons de vertex et les données de port](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>Rubriques connexes


[Comment : port un simple convertisseur OpenGL ES 2.0 vers Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Les mémoires tampons de vertex et les données de port](port-the-vertex-buffers-and-data-config.md)

[Port du GLSL](port-the-glsl.md)

[Dessiner à l’écran](draw-to-the-screen.md)

 

 




