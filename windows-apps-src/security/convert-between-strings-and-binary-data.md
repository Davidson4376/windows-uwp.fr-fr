---
title: Convertir entre chaînes et données binaires
description: Cet exemple de code indique comment convertir les chaînes et les données binaires dans une application de plateforme Windows universelle (UWP).
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: b3c3a3f6f831186302fc32b1f510919da40c57cc
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532518"
---
# <a name="convert-between-strings-and-binary-data"></a>Convertir entre chaînes et données binaires



Cet exemple de code indique comment convertir les chaînes et les données binaires dans une application de plateforme Windows universelle (UWP).

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