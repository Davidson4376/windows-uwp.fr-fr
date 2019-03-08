---
title: Utilisation de mémoires tampons de stockage connecté
description: En savoir plus sur l’utilisation de mémoires tampons de stockage connectés.
ms.assetid: 1d9d1b52-4bfe-4cd9-8b80-50ca6c0e9ae1
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox, stockage connecté
ms.localizationpriority: medium
ms.openlocfilehash: 3df95e4807e8d3457143e67eebfb62011bf365cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595024"
---
# <a name="working-with-connected-storage-buffers"></a>Utilisation de mémoires tampons de stockage connecté

Utilise l’API de stockage connecté **Windows::Storage::Streams::Buffer** instances pour passer des données vers et à partir d’une application. Étant donné que les types WinRT ne peut pas exposer des pointeurs bruts, accès aux données d’une instance de la mémoire tampon se produit via **DataReader** et **DataWriter** classes. Toutefois, **tampon** implémente également l’interface COM **IBufferByteAccess**, ce qui permet d’obtenir un pointeur directement vers les données de la mémoire tampon.

### <a name="to-get-a-pointer-to-a-buffer-instances-data"></a>Pour obtenir un pointeur vers une mémoire tampon données de l’instance

1.  Utilisez **réinterpréter\_cast** pour effectuer un cast de l’instance de la mémoire tampon en tant que **IUnknown**.

```cpp
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
```

2.  Requête la **IUnknown** interface pour le **IBufferByteAccess** interface COM.

```cpp
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
```

3.  Utilisez **IBufferByteAccess::Buffer** pour obtenir un pointeur directement aux données de la mémoire tampon.

```cpp
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
```

Par exemple, l’exemple de code suivant montre comment créer une mémoire tampon qui contient l’heure système actuelle. Dans la mesure où les mémoires tampons ont une valeur de capacité et la longueur séparée, il est nécessaire de définir explicitement la capacité et la longueur. Par défaut, la longueur est 0.

```cpp
    inline byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer)
    {
        using namespace Windows::Storage::Streams;
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
        return bytes;
    }

    IBuffer^ WrapRawBuffer( void* ptr, size_t size )
    {
        using namespace Windows::Storage::Streams;

        //uint32 size = sizeof(FILETIME);
        Buffer^ buffer = ref new Buffer(size);
        buffer->Length = size;

        memcpy(GetBufferData(buffer),ptr,size);


        return buffer;

    };
```
