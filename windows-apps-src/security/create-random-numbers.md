---
title: Create random numbers
description: This example code shows how to create a random number or buffer for use in cryptography in an Universal Windows Platform (UWP) app.
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 71ac4a6bffe5004a044ee6feba21eb44762ab781

---

# Create random numbers


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

This example code shows how to create a random number or buffer for use in cryptography in an Universal Windows Platform (UWP) app.

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


<!--HONumber=Aug16_HO3-->


