---
title: Créer des nombres aléatoires
description: Cet exemple de code indique comment créer un nombre aléatoire ou une mémoire tampon à utiliser pour le chiffrement dans une application de plateformeWindows universelle (UWP).
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: f36e50f2de36df39177370d688b9add5591403e1
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8895799"
---
# <a name="create-random-numbers"></a>Créer des nombres aléatoires



Cet exemple de code indique comment créer un nombre aléatoire ou une mémoire tampon à utiliser pour le chiffrement dans une application de plateformeWindows universelle (UWP).

```cs
public string GenerateRandomData()
{
    // Define the length, in bytes, of the buffer.
    uint length = 32;

    // Generate random data and copy it to a buffer.
    IBuffer buffer = CryptographicBuffer.GenerateRandom(length);

    // Encode the buffer to a hexadecimal string (for display).
    string randomHex = CryptographicBuffer.EncodeToHexString(buffer);

    return randomHex;
}

public uint GenerateRandomNumber()
{
    // Generate a random number.
    uint random = CryptographicBuffer.GenerateRandomNumber();
    return random;
}
```