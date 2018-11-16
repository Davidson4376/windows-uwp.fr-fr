---
author: mtoepke
title: Charger des ressources dans votre jeu DirectX
description: La plupart des jeux, à un moment donné, chargent des ressources et des éléments multimédias (comme les nuanceurs, les textures, les maillages prédéfinis ou d’autres données graphiques) à partir d’un stockage local ou d’autres flux de données.
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, directx, chargement des ressources
ms.localizationpriority: medium
ms.openlocfilehash: 1bea3f515ba8ff810fc6dfd6281f0488c4f3e235
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6844425"
---
# <a name="load-resources-in-your-directx-game"></a>Charger des ressources dans votre jeu DirectX



La plupart des jeux, à un moment donné, chargent des ressources et des éléments multimédias (comme les nuanceurs, les textures, les maillages prédéfinis ou d’autres données graphiques) à partir d’un stockage local ou d’autres flux de données. Cette rubrique offre une vue d’ensemble des éléments à prendre en compte lors du chargement de ces fichiers en vue de leur utilisation dans votre jeu de plateforme Windows universelle (UWP) DirectX C/C++.

Par exemple, les maillages des objets polygonaux de votre jeu peuvent avoir été créés avec un autre outil et exportés vers un format spécifique. Il en va de même pour les textures et plus encore : alors que généralement la plupart des outils peuvent écrire une image bitmap plate, sans compression, qui est comprise par la plupart des API graphiques, utiliser dans votre jeu une telle image peut s’avérer extrêmement inefficace. Ici, nous vous guidons à travers les étapes de base du chargement de trois types de ressources graphiques pour une utilisation avec Direct3D: les maillages (modèles), les textures (bitmaps) et les objets nuanceurs compilés.

## <a name="what-you-need-to-know"></a>Ce que vous devez savoir


### <a name="technologies"></a>Technologies

-   Bibliothèque de modèles parallèles (ppltasks.h)

### <a name="prerequisites"></a>Prérequis

-   Comprendre le Windows Runtime de base
-   Comprendre les tâches asynchrones
-   Comprendre les concepts de base de la programmation graphique 3D

Cet exemple inclut également trois fichiers de code permettant le chargement et la gestion des ressources. Vous rencontrerez les objets de code définis dans ces fichiers tout au long de cette rubrique.

-   BasicLoader.h/.cpp
-   BasicReaderWriter.h/.cpp
-   DDSTextureLoader.h/.cpp

Vous pouvez trouver le code complet de ces exemples dans les liens suivants.

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
<td align="left"><p><a href="complete-code-for-basicloader.md">Code complet de BasicLoader</a></p></td>
<td align="left"><p>Code complet pour une classe et des méthodes qui convertissent et chargent des objets maillés graphiques en mémoire.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="complete-code-for-basicreaderwriter.md">Code complet de BasicReaderWriter</a></p></td>
<td align="left"><p>Code complet pour une classe et des méthodes permettant de lire et écrire des fichiers de données binaires en général. Utilisé par la classe <a href="complete-code-for-basicloader.md">BasicLoader</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-ddstextureloader.md">Code complet de DDSTextureLoader</a></p></td>
<td align="left"><p>Code complet pour une classe et la méthode qui charge une texture DDS à partir de la mémoire.</p></td>
</tr>
</tbody>
</table>

 

## <a name="instructions"></a>Instructions

### <a name="asynchronous-loading"></a>Chargement asynchrone

Le chargement asynchrone est géré à l’aide du modèle **task** de la bibliothèque de modèles parallèles (PPL). Une **task** contient un appel de méthode suivi d’une expression lambda qui traite les résultats de l’appel asynchrone après qu’il se termine, et suit généralement le format:

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`.

Des tâches peuvent être enchaînées à l’aide de la syntaxe **.then()**, afin qu’à l’issue d’une opération, une autre opération asynchrone qui dépend des résultats de cette opération préalable puisse s’exécuter. Ainsi, vous pouvez charger, convertir et gérer des éléments multimédias complexes sur des threads séparés de façon pratiquement invisible pour le joueur.

Pour plus d’informations, voir [Programmation asynchrone en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

Maintenant, examinons la structure de base pour déclarer et créer une méthode de chargement de fichier asynchrone, **ReadDataAsync**.

```cpp
#include <ppltasks.h>

// ...
concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename);

// ...

using concurrency;

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

Dans ce code, lorsque la méthode **ReadDataAsync** définie ci-dessus est appelée, une tâche est créée pour lire une mémoire tampon à partir du système de fichiers. Une fois qu’elle est terminée, une tâche chaînée récupère la mémoire tampon et place les octets de la mémoire tampon dans un tableau à l’aide du type statique [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119).

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

Voici l’appel que vous effectuez pour lire de façon asynchrone: **ReadDataAsync**. Lorsque c’est terminé, votre code reçoit un tableau d’octets lus à partir du fichier fourni. Puisque **ReadDataAsync** est défini comme une tâche, vous pouvez utiliser une expression lambda pour exécuter une opération spécifique lorsque le tableau d’octets est renvoyé, comme transmettre ces données à une fonction DirectX qui peut les utiliser.

Si votre jeu est assez simple, chargez vos ressources avec une méthode comme celle-ci lorsque l’utilisateur démarre le jeu. Vous pouvez le faire avant de démarrer la boucle principale du jeu à partir d’un certain point dans la séquence d’appels de votre implémentation [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505). Encore une fois, vous appelez les méthodes de chargement de vos ressources de façon asynchrone afin que le jeu puisse démarrer plus rapidement et que le joueur n’ait pas à attendre la fin du chargement pour s’engager dans les interactions précoces.

Toutefois, vous ne voulez pas démarrer le jeu à proprement parler tant que le chargement asynchrone n’est pas terminé ! Créez une méthode pour signaler que le chargement est terminé, tel qu’un champ spécifique, et utilisez les expressions lambda sur vos méthodes de chargement pour activer ce signal lorsque c’est terminé. Vérifiez la variable avant de démarrer tout composant qui utilise ces ressources chargées.

Voici un exemple d’utilisation de méthodes asynchrones définies dans BasicLoader.cpppour charger des nuanceurs, un maillage et une texture quand le jeu démarre. Notez que cela définit un champ spécifique sur l’objet du jeu, **m\_loadingComplete** une fois toutes les méthodes de chargement terminées.

```cpp
void ResourceLoading::CreateDeviceResources()
{
    // DirectXBase is a common sample class that implements a basic view provider. 
    
    DirectXBase::CreateDeviceResources(); 

    // ...

    // This flag will keep track of whether or not all application
    // resources have been loaded.  Until all resources are loaded,
    // only the sample overlay will be drawn on the screen.
    m_loadingComplete = false;

    // Create a BasicLoader, and use it to asynchronously load all
    // application resources.  When an output value becomes non-null,
    // this indicates that the asynchronous operation has completed.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    auto loadVertexShaderTask = loader->LoadShaderAsync(
        "SimpleVertexShader.cso",
        nullptr,
        0,
        &m_vertexShader,
        &m_inputLayout
        );

    auto loadPixelShaderTask = loader->LoadShaderAsync(
        "SimplePixelShader.cso",
        &m_pixelShader
        );

    auto loadTextureTask = loader->LoadTextureAsync(
        "reftexture.dds",
        nullptr,
        &m_textureSRV
        );

    auto loadMeshTask = loader->LoadMeshAsync(
        "refmesh.vbo",
        &m_vertexBuffer,
        &m_indexBuffer,
        nullptr,
        &m_indexCount
        );

    // The && operator can be used to create a single task that represents
    // a group of multiple tasks. The new task's completed handler will only
    // be called once all associated tasks have completed. In this case, the
    // new task represents a task to load various assets from the package.
    (loadVertexShaderTask && loadPixelShaderTask && loadTextureTask && loadMeshTask).then([=]()
    {
        m_loadingComplete = true;
    });

    // Create constant buffers and other graphics device-specific resources here.
}
```

Notez que les tâches ont été agrégées à l’aide de l’opérateur &amp;&amp; de sorte que l’expression lambda qui définit l’indicateur de chargement complet se déclenche uniquement une fois toutes les tâches terminées. Notez que si vous avez plusieurs indicateurs, vous avez la possibilité de conditions de concurrence critique. Par exemple, si l’expression lambda définit deux indicateurs séquentiellement sur la même valeur, un autre thread ne pourrait voir que le premier indicateur s’il les examine avant que le deuxième ne soit défini.

Vous avez vu comment charger des fichiers de ressources de manière asynchrone. Les chargements synchrones de fichiers sont beaucoup plus simples, et vous en trouverez des exemples dans le [Code complet de BasicReaderWriter](complete-code-for-basicreaderwriter.md) et le [Code complet de BasicLoader](complete-code-for-basicloader.md).

Bien sûr, les différents types de ressources et d’actifs nécessitent souvent un traitement ou une conversion supplémentaire avant de pouvoir être utilisés dans votre pipeline graphique. Nous allons examiner trois types de ressources spécifiques : les maillages, les textures et les nuanceurs.

### <a name="loading-meshes"></a>Chargement des maillages

Les maillages sont des données de vertex, soit générées de façon procédurale par du code au sein de votre jeu soit exportées vers un fichier depuis une autre application (comme 3DStudio MAX ou Alias WaveFront) ou un outil. Ces maillages représentent les modèles dans votre jeu, allant de simples primitives comme des cubes ou des sphères à des voitures, des maisons et des personnages. Ils contiennent souvent des données de couleur et d’animation, également, en fonction de leur format. Nous nous concentrerons sur les maillages qui contiennent uniquement des données de vertex.

Pour charger un maillage correctement, vous devez connaître le format des données dans le fichier du maillage. Notre type simple **BasicReaderWriter** ci-dessus lit simplement les données en entrée comme un flux d’octets, sans savoir que ces octets représentent un maillage, encore moins un format de maillage spécifique exporté par une autre application! Vous devez effectuer la conversion au fur et à mesure que vous introduisez les données du maillage en mémoire.

(Vous devriez toujours essayer d’empaqueter les données des actifs du jeu dans un format aussi proche de la représentation interne que possible. Cela permet de réduire l’utilisation des ressources et de gagner du temps.)

Nous allons récupérer les données octets du fichier de maillage. Le format de l’exemple suppose que le fichier est un format spécifique à l’exemple avec le suffixe .vbo. (Là encore, ce format est différent du format VBO de OpenGL.) Chaque vertex est mappé au type **BasicVertex**, qui est une structure définie dans le code de l’outil convertisseur obj2vbo. La disposition des données de vertex dans le fichier .vbo ressemble à ceci:

-   Les 32 premiers bits (4 octets) du flux de données contiennent le nombre de vertex (numVertices) dans le maillage, représenté par une valeur uint32.
-   Les 32 bits suivants (4 octets) du flux de données contiennent le nombre d’index (numIndices) dans le maillage, représenté par une valeur uint32.
-   Après quoi, les bits suivants (numVertices \* sizeof(**BasicVertex**)) contiennent les données de vertex.
-   Les derniers bits de données (numIndices \* 16) contiennent les données d’index, représentées comme une séquence de valeurs uint16.

La clé est donc de connaître la disposition au niveau des bits des données de maillage chargées. Assurez-vous aussi d’être cohérent sur le plan endian. Toutes les plateformes de package Windows8 sont en mode little endian.

Dans l’exemple, vous appelez une méthode, CreateMesh, à partir de la méthode **LoadMeshAsync** pour effectuer cette interprétation au niveau des bits.

```cpp
task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

**CreateMesh** interprète les données d’octets chargées à partir du fichier et crée un tampon vertex et un tampon d’index pour le maillage en transmettant les listes de vertex et d’index à [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) et en spécifiant respectivement D3D11\_BIND\_VERTEX\_BUFFER ou D3D11\_BIND\_INDEX\_BUFFER. Voici le code utilisé dans **BasicLoader**:

```cpp
void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);

    m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            );
    
    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    
    m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            );
  
    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}
```

En général, vous créez une paire de mémoires tampons vertex/index pour chaque maillage utilisé dans votre jeu. Où et quand vous chargez les maillages vous appartient. Si vous avez un grand nombre de maillages, vous pouvez n’en charger que certains à partir du disque à des points précis du jeu, comme durant les états de chargement prédéfinis spécifiques. Pour les grands maillages, comme les données de terrain, vous pouvez lire les vertex à partir d’un cache, mais cette procédure plus complexe sort du cadre de cette rubrique.

Encore une fois, vous devez connaître le format des données de vertex ! Il existe de nombreuses façons de représenter les données de vertex selon les outils utilisés pour créer des modèles. Il existe également différentes façons de représenter la disposition d’entrée des données vertex pour Direct3D, comme les bandes et listes de triangles. Pour plus d’informations sur les données vertex, voir [Présentation des mémoires tampons dans Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476898) et [Primitives](https://msdn.microsoft.com/library/windows/desktop/bb147291).

Maintenant, penchons-nous sur le chargement des textures.

### <a name="loading-textures"></a>Chargement des textures

L’élément multimédia le plus courant d’un jeu, et celui qui comprend la plupart des fichiers sur disque et en mémoire, correspond aux textures. Comme les maillages, les textures peuvent être dans divers formats, et vous les convertissez en format utilisable par Direct3D lorsque vous les chargez. Les textures sont également disponibles dans une grande variété de types et permettent de créer différents effets. Les niveaux MIP pour les textures peuvent servir à améliorer l’apparence et les performances des objets à distance ; des cartes de poussière et d’éclairage permettent de disposer en couches des effets et détails au-dessus d’une texture de base ; et des cartes normales sont utilisées dans les calculs d’éclairage par pixel. Dans un jeu moderne, une scène typique peut contenir potentiellement des milliers de textures individuelles, et votre code doit les gérer efficacement toutes !

Comme pour les maillages, un certain nombre de formats spécifiques permettent d’optimiser l’utilisation de la mémoire. Étant donné que les textures peuvent facilement consommer une grande partie de la mémoire GPU (et système ), elles sont souvent compressées d’une certaine façon. Vous n’êtes pas obligé d’utiliser la compression pour les textures de votre jeu, et vous pouvez utiliser n’importe quel algorithme de compression/décompression du moment que vous fournissez les nuanceurs Direct3D avec les données dans un format qu’il peut comprendre (comme un bitmap [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)).

Direct3D prend en charge les algorithmes de compression de texture DXT, bien que chaque format DXT ne soit pas forcément géré par le matériel graphique du joueur. Les fichiers DDS qui contiennent des textures DXT (et autres formats de compression de texture également) ont le suffixe .dds.

Un fichier DDS est un fichier binaire qui contient les informations suivantes :

-   Un DWORD (nombre magique) contenant la valeur codée des quatre caractères "DDS " (0x20534444).

-   Une description des données du fichier.

    Les données sont décrites avec une description d’en-tête à l’aide de [**DDS\_HEADER**](https://msdn.microsoft.com/library/windows/desktop/bb943982); le format de pixel est défini à l’aide de [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984). Notez que les structures **DDS\_HEADER** et **DDS\_PIXELFORMAT** remplacent les structures obsolètes DDSURFACEDESC2, DDSCAPS2 et DDPIXELFORMAT de DirectDraw7. **DDS\_HEADER** est l’équivalent binaire de DDSURFACEDESC2 et DDSCAPS2. **DDS\_PIXELFORMAT** est l’équivalent binaire de DDPIXELFORMAT.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    Si la valeur de **dwFlags** dans [**DDS\_PIXELFORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb943984) est définie sur DDPF\_FOURCC et si **dwFourCC** est défini sur DX10, une structure [**DDS\_HEADER\_DXT10**](https://msdn.microsoft.com/library/windows/desktop/bb943983) supplémentaire sera présente pour accueillir les tableaux de texture ou les formats DXGI qui ne peuvent pas être exprimés par un format de pixel RVB, comme les formats à virgule flottante, les formats sRVB, etc. Lorsque la structure **DDS\_HEADER\_DXT10** est présente, la description entière des données ressemble à ceci.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   Pointeur vers un tableau d’octets qui contient les principales données de surface.
    ```cpp
    BYTE bdata[]
    ```

-   Pointeur vers un tableau d’octets qui contient les surfaces restantes telles que niveaux de mipmap, faces dans un plan de cube, profondeurs dans une texture de volume. Suivez ces liens pour plus d’informations sur le schéma du fichierDDS pour une [texture](https://msdn.microsoft.com/library/windows/desktop/bb205578), un [mappage de cube](https://msdn.microsoft.com/library/windows/desktop/bb205577) ou une [texture de volume](https://msdn.microsoft.com/library/windows/desktop/bb205579).

    ```cpp
    BYTE bdata2[]
    ```

De nombreux outils permettent d’exporter vers le format DDS. Si vous n’avez pas d’outil pour exporter votre texture dans ce format, envisagez d’en créer un. Pour plus de détails sur le formatDDS et pour savoir comment l’utiliser dans votre code, voir le [Guide de programmation pourDDS](https://msdn.microsoft.com/library/windows/desktop/bb943991). Dans notre exemple, nous allons utiliser DDS.

Comme avec d’autres types de ressources, vous lisez les données dans un fichier sous forme de flux d’octets. Une fois votre tâche de chargement terminée, l’appel lambda exécute du code (la méthode **CreateTexture**) pour traiter le flux d’octets dans un format utilisable par Direct3D.

```cpp
task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}
```

Dans l’extrait de code précédent, l’appel lambda vérifie si le nom du fichier a une extension dds. Dans ce cas, vous supposez qu’il s’agit d’une texture DDS. Sinon, utilisez les API WIC (Windows Imaging Component) pour découvrir le format et décoder les données sous forme de bitmap. Quelle que soit la méthode utilisée, le résultat est une image bitmap [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) (ou une erreur).

```cpp
void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        resource.As(&texture2D);
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory));
        }

        ComPtr<IWICStream> stream;
        m_wicFactory->CreateStream(&stream);

        stream->InitializeFromMemory(
                data,
                dataSize);

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder);

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        bitmapDecoder->GetFrame(0, &bitmapFrame);

        ComPtr<IWICFormatConverter> formatConverter;
        m_wicFactory->CreateFormatConverter(&formatConverter);

        formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom);

        uint32 width;
        uint32 height;
        bitmapFrame->GetSize(&width, &height);

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get());

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D);

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView);
        }
    }


    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}
```

Lorsque ce code se termine, vous avez une [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) en mémoire, chargée à partir d’un fichier image. À l’instar des maillages, vous en avez probablement beaucoup dans votre jeu et dans n’importe quelle scène donnée. Envisagez de créer des caches pour les textures auxquelles le jeu accède régulièrement par scène ou par niveau, plutôt que toutes les charger au démarrage du jeu ou du niveau.

(Pour explorer dans son intégralité la méthode **CreateDDSTextureFromMemory** appelée dans l’exemple ci-dessus, voir [Code complet de DDSTextureLoader](complete-code-for-ddstextureloader.md).)

En outre, des textures individuelles ou «peau» peuvent être mappées sur des surfaces ou des polygones de maillage spécifiques. Ces données de mappage sont généralement exportées par l’outil que l’artiste ou le graphiste a utilisé pour créer le modèle et les textures. Assurez-vous que vous capturez ces informations aussi lorsque vous chargez les données exportées, car vous les utiliserez pour mapper les textures correctes sur les surfaces correspondantes lorsque vous effectuez l’ombrage de fragments.

### <a name="loading-shaders"></a>Chargement des nuanceurs

Les nuanceurs sont des fichiers HLSL (High Level Shader Language) compilés qui sont chargés en mémoire et appelés à des étapes spécifiques du pipeline graphique. Les nuanceurs essentiels les plus courants sont les nuanceurs de vertex et les nuanceurs de pixels, qui traitent les vertex particuliers de votre maillage et les pixels dans les fenêtres d’affichage de la scène, respectivement. Le code HLSL est exécuté pour transformer la géométrie, appliquer des effets de lumière et des textures et effectuer le post-traitement sur la scène rendue.

Un jeu Direct3D peut avoir plusieurs nuanceurs différents, chacun d’eux compilé dans un fichier .cso (Compiled Shader Object) distinct. Normalement, vous n’en avez pas une telle quantité pour qu’il faille les charger dynamiquement; dans la plupart des cas, vous pouvez simplement les charger quand le jeu commence, ou par niveau (par exemple, un nuanceur pour les effets de pluie).

Le code de la classe **BasicLoader** fournit plusieurs surcharges pour différents nuanceurs, y compris des nuanceurs de vertex, de pixels, de géométrie et de coque. Le code ci-dessous traite de nuanceurs de pixels par exemple. (Vous pouvez consulter l’intégralité du code dans [Code complet de BasicLoader](complete-code-for-basicloader.md)).

```cpp
concurrency::task<void> LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        
       m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);
    });
}

```

Dans cet exemple, vous utilisez l’instance de **BasicReaderWriter** (**m\_basicReaderWriter**) pour lire le fichier d’objet nuanceur compilé (.cso) fourni dans un flux d’octets. Une fois cette tâche terminée, la lambda appelle [**ID3D11Device::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) avec les données d’octets chargées à partir du fichier. Votre rappel doit définir un indicateur signalant que le chargement a réussi, et votre code doit vérifier cet indicateur avant l’exécution du nuanceur.

Les nuanceurs de vertex sont un peu plus complexes. Pour un nuanceur de vertex, vous chargez également un schéma d’entrée distinct qui définit les données de vertex. Le code suivant permet de charger de façon asynchrone un nuanceur de vertex, ainsi qu’un schéma d’entrée de vertex personnalisé. N’oubliez pas que les informations de vertex que vous chargez à partir de vos maillages peuvent être correctement représentées par ce schéma d’entrée !

Nous allons créer le schéma d’entrée avant de charger le nuanceur de vertex.

```cpp
void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout);
    }
    else
    {
        m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout);
    }
}
```

Dans ce schéma particulier, le nuanceur de vertex traite les données suivantes de chaque vertex :

-   une position de coordonnées 3D (x, y, z) dans l’espace de coordonnées du modèle, représentée par un trio de valeurs à virgule flottante 32 bits ;
-   un vecteur normal pour le vertex, également représenté par trois valeurs à virgule flottante 32 bits ;
-   une valeur de coordonnées de texture 2D (u, v) transformée, représentée par une paire de valeurs flottantes 32 bits.

Ces éléments d’entrée par vertex sont appelés [sémantique HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509647); il s’agit d’ensembles de registres définis, utilisés pour transmettre des données en direction et à partir de votre objet nuanceur compilé. Votre pipeline exécute le nuanceur de vertex une fois pour chaque vertex du maillage que vous avez chargé. La sémantique définit l’entrée (et la sortie) du nuanceur de vertex pour son exécution et fournit ces données pour vos calculs par vertex dans le code HLSL de votre nuanceur.

Maintenant, chargez l’objet nuanceur de vertex.

```cpp
concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
       m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout);   
        }
    });
}

```

Dans ce code, une fois que vous avez lu les données octets du fichier CSO, vous créez le nuanceur de vertex en appelant [**ID3D11Device::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524). Après cela, vous créez votre schéma d’entrée pour le nuanceur dans la même lambda.

D’autres types de nuanceurs, comme les nuanceurs de coque et de géométrie, peuvent également nécessiter une configuration spécifique. Le code complet d’une variété de méthodes de chargement de nuanceur est fourni dans [Code complet de BasicLoader](complete-code-for-basicloader.md) et dans l’[exemple de chargement de ressources Direct3D]( http://go.microsoft.com/fwlink/p/?LinkID=265132).

## <a name="remarks"></a>Remarques

À ce stade, vous devez savoir comment créer ou modifier les méthodes de chargement asynchrone des ressources et éléments multimédias de jeu communs, tels que les maillages, les textures et les nuanceurs compilés.

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de chargement de ressources Direct3D]( http://go.microsoft.com/fwlink/p/?LinkID=265132)
* [Code complet de BasicLoader](complete-code-for-basicloader.md)
* [Code complet de BasicReaderWriter](complete-code-for-basicreaderwriter.md)
* [Code complet de DDSTextureLoader](complete-code-for-ddstextureloader.md)

 

 




