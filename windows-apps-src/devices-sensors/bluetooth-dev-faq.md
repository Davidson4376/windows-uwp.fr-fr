---
author: msatranjr
title: FAQ sur le Bluetooth pour les développeurs
description: Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: c0af6a19e17a62ed82c32e68ea1732e1f51d4641
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "301929"
---
# <a name="bluetooth-developer-faq"></a>FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quelles API dois-je utiliser? Bluetooth classique (RFCOMM) ou faible Bluetooth énergétiques (GATT)?
Il existe différentes discussions en ligne autour de cette rubrique générale donc conserver cette réponse approximativement sur les différences par rapport à Windows. Voici quelques règles générales:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Utilisez les API GATT lorsque vous êtes en communication avec un périphérique qui prend en charge Bluetooth faible énergie. Si vous êtes utilisez cas rare faible bande passante ou requiert un faible puissance, Bluetooth faible énergie est la réponse. L’espace de noms principal qui inclut cette fonctionnalité est [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quand ne pas utiliser LE Bluetooth**
- Large bande passante, les scénarios de haute fréquence. Si vous avez besoin conserver en permanence la synchronisation avec de grandes quantités de données, envisagez d’utiliser Wi-Fi classique ou peut-être même Bluetooth. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth classique (Windows.Devices.Bluetooth.Rfcomm)

Les API RFCOMM offrent un socket pour effectuer la communication de style bidirectionnel serial port. Une fois que vous avez un socket, les méthodes d’écrire dans et lire à partir de celui-ci sont assez standard. Une implémentation de ce est présentée dans l' [exemple Rfcomm conversation](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quand ne pas utiliser Bluetooth Rfcomm** 
- Notifications. Le protocole GATT Bluetooth a une commande spécifique pour cet et provoque beaucoup moins courant et le temps de réponse plus rapides. 
- Vérification de la détection de présence ou la proximité. Préférable d’utiliser les [API de publication](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) et de se connecter via LE Bluetooth. 


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

**(14393 et ci-dessous)** Cette fonctionnalité n’est pas disponible pour Bluetooth faible énergie (GATT Client), afin que vous aurez toujours à paire via la page Paramètres ou à l’aide de l’API [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) dans access ordre ces périphériques.

**(15030 et ultérieures)** Appariement des périphériques Bluetooth n’est plus nécessaire. Utilisez les nouvelles APIs Async comme GetGattServicesAsync et GetCharacteristicsAsync pour interroger l’état actuel du périphérique distant. Consultez les [documents du Client](gatt-client.md) pour plus d’informations. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quand dois-je paire avec un appareil avant de pouvoir communiquer avec lui?
En règle générale, si vous avez besoin d’une liaison fiable et à long terme avec un périphérique, établir une association avec (qui redirige l’utilisateur à la page Paramètres ou à l’aide de l’énumération des périphériques et API jumelage). Si vous devez simplement lire les informations le périphérique qui est exposé publiquement (détecteur de température ou balise), puis se connecter ou écouter les annonces sans apporter de toute tentative d’établir une association avec l’appareil. Cela empêche les problèmes d’interopérabilité à long terme, car un hôte de périphériques ne prennent pas en charge jumelage. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Tous les périphériques Windows prennent en charge les périphériques rôle?

Non – il s’agit d’une fonction qui en dépendent du matériel, mais une méthode est fournie (BluetoothAdapter.IsPeripheralRoleSupported) à la requête, si elle est prise en charge ou non.  Appareils pris en charge actuellement incluent Windows Phone sur 8992 + et RPi3 (IoT Windows). 

## <a name="can-i-access-these-apis-from-win32"></a>Puis-je accéder ces API de Win32?

Oui, toutes ces API devrait fonctionner. Ce blog décrit en détail la façon d’appeler des [API Windows à partir d’applications de bureau](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Cette fonctionnalité est supposée exister sur *- SKU insérer ici -*?

**LE Bluetooth**: Oui, toutes les fonctionnalités se trouve dans OneCore et doivent être disponibles sur les appareils avec une pile Bluetooth LE fonctionnement plus récentes. 
> Avertissement: Rôle périphérique est dépendent du matériel et des éditions Windows Server ne prend pas en charge Bluetooth. 

**Bluetooth BR/EDR (standard)**: il existe des variations, mais de manière générale, ils disposent de support de niveau de profil similaires. Voir la documentation sur [RFCOMM](send-or-receive-files-with-rfcomm.md) et ces documents profil pris en charge pour [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) et [téléphone](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

