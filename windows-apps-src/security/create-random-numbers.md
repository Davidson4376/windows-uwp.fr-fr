---
title: "Créer des nombres aléatoires"
description: "Cet exemple de code indique comment créer un nombre aléatoire ou une mémoire tampon à utiliser pour le chiffrement dans une application de plateforme Windows universelle (UWP)."
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 362bb264320fcf1256559543ce4607abd50260ed
ms.lasthandoff: 02/07/2017

---

# <a name="create-random-numbers"></a>Créer des nombres aléatoires


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Cet exemple de code indique comment créer un nombre aléatoire ou une mémoire tampon à utiliser pour le chiffrement dans une application de plateforme Windows universelle (UWP).

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
