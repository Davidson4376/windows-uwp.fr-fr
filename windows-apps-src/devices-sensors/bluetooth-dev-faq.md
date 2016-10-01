---
author: msatranjr
title: "FAQ sur le Bluetooth pour les développeurs"
description: "Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP."
translationtype: Human Translation
ms.sourcegitcommit: e4c95448262c6c62956fcb50581c98d8c34d6dc0
ms.openlocfilehash: 2afc1250aa9d7a6cf6c9c8cb45dd2379b9d36984

---
# FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## Pourquoi mon périphérique Bluetooth LE ne répond plus après une déconnexion?

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

## Dois-je jumeler des périphériques Bluetooth avant de les utiliser?

Vous n’êtes pas contraint de le faire pour les appareils Bluetooth RFCOMM (standard). À compter de Windows10 version1607, vous pouvez simplement interroger les appareils à proximité et vous y connecter. L’[exemple de discussion RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) mis à jour présente cette fonctionnalité. 

Cette fonctionnalité n’est pas disponible pour Bluetooth Low Energy d’énergie (client GATT). Vous devrez donc continuer le jumelage par le biais de la page Paramètres ou à l’aide des API [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) afin d’accéder à ces périphériques.




<!--HONumber=Aug16_HO3-->


