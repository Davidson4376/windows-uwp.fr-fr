---
title: Copy to and from byte arrays
description: This example code shows how to copy to and from byte arrays in an Universal Windows Platform (UWP) app.
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
---

# Copy to and from byte arrays


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

This example code shows how to copy to and from byte arrays in an Universal Windows Platform (UWP) app.

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

 

 




<!--HONumber=Mar16_HO1-->
