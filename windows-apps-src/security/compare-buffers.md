---
title: "Comparer des mémoires tampons"
description: "Cet exemple de code indique comment comparer des mémoires tampon dans une application de plateforme Windows universelle (UWP)."
ms.assetid: CB086E51-544A-470D-B7C8-C055271CD615
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 6c55e0dec2198110e0ddcbad743b04439cb3435e

---

# Comparer des mémoires tampons


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Cet exemple de code indique comment comparer des mémoires tampon dans une application de plateforme Windows universelle (UWP).

```cs
public void CompareBuffers()
{
    // Create a hexadecimal string.
    String strHex = "30310aff";

    // Create a Base64 string that is equivalent to strHex.
    String strBase64v1 = "MDEK/w==";

    // Create a Base64 string that is not equivalent to strHex.
    String strBase64v2 = "KEDM/w==";

    // Decode strHex to a buffer.
    IBuffer buff1 = CryptographicBuffer.DecodeFromHexString(strHex);

    // Decode strBase64v1 to a buffer.
    IBuffer buff2 = CryptographicBuffer.DecodeFromBase64String(strBase64v1);

    // Decode strBase64v2 to a buffer.
    IBuffer buff3 = CryptographicBuffer.DecodeFromBase64String(strBase64v2);

    // Compare the hexadecimal-decoded buff1 to the Base64-decoded buff2.
    // The code points in the two buffers are equal, and the Boolean value
    // is true.
    Boolean bVal_1 = CryptographicBuffer.Compare(buff1, buff2);

    // Compare the hexadecimal-decoded buff1 to the Base64-decoded buff3.
    // The code points in the two buffers are not equal, and the Boolean value
    // is false.
    Boolean bVal_2 = CryptographicBuffer.Compare(buff1, buff3);
}
```


<!--HONumber=Aug16_HO3-->


