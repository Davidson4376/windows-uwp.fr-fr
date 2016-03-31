---
title: Convert between strings and binary data
description: This example code shows how to convert between strings and binary data in an Universal Windows Platform (UWP) app.
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
---

# Convert between strings and binary data


\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

This example code shows how to convert between strings and binary data in an Universal Windows Platform (UWP) app.

```cs
public void ConvertData()
{
    // Create a string to convert.
    String strIn = "Input String";

    // Convert the string to UTF16BE binary data.
    IBuffer buffUTF16BE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16BE);

    // Convert the string to UTF16LE binary data.
    IBuffer buffUTF16LE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16LE);

    // Convert the string to UTF8 binary data.
    IBuffer buffUTF8 = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf8);
}
```

 

 




<!--HONumber=Mar16_HO1-->
