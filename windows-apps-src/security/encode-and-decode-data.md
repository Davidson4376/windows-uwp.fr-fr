---
title: Encoder et décoder des données
description: Cet exemple de code indique comment encoder et décoder des données base64 et hexadécimales dans une application de plateforme Windows universelle (UWP).
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, sécurité
ms.localizationpriority: medium
ms.openlocfilehash: a9177061f70419e2a3b0e3b47f933af75a11ad68
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4506323"
---
# <a name="encode-and-decode-data"></a>Encoder et décoder des données



Cet exemple de code indique comment encoder et décoder des données base64 et hexadécimales dans une application de plateforme Windows universelle (UWP).

```cs
public void EncodeDecodeBase64()
{
    // Define a Base64 encoded string.
    String strBase64 = "uiwyeroiugfyqcajkds897945234==";

    // Decoded the string from Base64 to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromBase64String(strBase64);

    // Encode the buffer back into a Base64 string.
    String strBase64New = CryptographicBuffer.EncodeToBase64String(buffer);
}

public void EncodeDecodeHex()
{
    // Define a hexadecimal string.
    String strHex = "30310AFF";

    // Decode a hexadecimal string to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromHexString(strHex);

    // Encode the buffer back into a hexadecimal string.
    String strHexNew = CryptographicBuffer.EncodeToHexString(buffer);
}
```
