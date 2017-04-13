---
author: msatranjr
title: "FAQ sur le Bluetooth pour les développeurs"
description: "Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP."
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.openlocfilehash: 3dabc5ad2833eecfec1f397bdd5bf7f2b807a48d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="bluetooth-developer-faq"></a>FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Pourquoi mon périphérique Bluetooth LE ne répond plus après une déconnexion?

La raison courante pour laquelle cela se produit est due au fait que l’appareil distant a perdu des informations de jumelage. Un grand nombre de périphériques Bluetooth antérieurs ne nécessitent pas d’authentification. Pour protéger l’utilisateur, tous les processus de jumelage effectués à partir de l’application Paramètres exigent une authentification et certains appareils ne savent pas comment les traiter. 

À compter de Windows10 version1511, les développeurs contrôlent le processus de couplage. L’[exemple d’énumération des périphériques et de jumelage](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) détaille les différents aspects liés à l’association de nouveaux périphériques.

Dans cet exemple, nous générons le jumelage avec un périphérique sans chiffrement. Notez que cela est possible uniquement si le périphérique à distance ne nécessite pas de chiffrement ou d’authentification pour fonctionner.

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>Dois-je jumeler des périphériques Bluetooth avant de les utiliser?

Vous n’êtes pas contraint de le faire pour les appareils Bluetooth RFCOMM (standard). À compter de Windows10 version1607, vous pouvez simplement interroger les appareils à proximité et vous y connecter. L’[exemple de discussion RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) mis à jour présente cette fonctionnalité. 

Cette fonctionnalité n’est pas disponible pour Bluetooth Low Energy d’énergie (client GATT). Vous devrez donc continuer le jumelage par le biais de la page Paramètres ou à l’aide des API [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) afin d’accéder à ces périphériques.

