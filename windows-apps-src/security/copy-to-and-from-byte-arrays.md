---
title: Copier des tableaux d’octets
description: Cet exemple de code indique comment copier des tableaux d’octets dans une application de plateforme Windows universelle (UWP).
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: 22ce577f7d70b3750a365462b64181a27e71428e
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5769015"
---
# <a name="copy-to-and-from-byte-arrays"></a>Copier des tableaux d’octets



Cet exemple de code indique comment copier des tableaux d’octets dans une application de plateforme Windows universelle (UWP).

```cs
public void ByteArrayCopy()
{
    // Initialize a byte array.
    byte[] bytes = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Create a buffer from the byte array.
    IBuffer buffer = CryptographicBuffer.CreateFromByteArray(bytes);

    // Encode the buffer into a hexadecimal string (for display);
    string hex = CryptographicBuffer.EncodeToHexString(buffer);

    // Copy the buffer back into a new byte array.
    byte[] newByteArray;
    CryptographicBuffer.CopyToByteArray(buffer, out newByteArray);
}
```