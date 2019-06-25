---
title: FAQ sur le Bluetooth pour les développeurs
description: Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 704ce146f95ed64a5891c130fee4d78c90dff034
ms.sourcegitcommit: 58d35b89662d4ad240650933e43fee0b00e9a962
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67344523"
---
# <a name="bluetooth-developer-faq"></a>FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quelles API dois-je utiliser ? Bluetooth Classic (RFCOMM) ou Bluetooth Low Energy (GATT) ?
Cette question générale fait l’objet de nombreuses discussions en ligne. Nous examinerons donc la différence entre ces deux types d’API strictement du point de vue de Windows. Voici quelques recommandations générales :

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Utilisez les API GATT lorsque vous communiquez avec un appareil qui prend en charge la technologie Bluetooth Low Energy. Si votre cas d’usage est peu fréquente faible bande passante ou nécessite une faible consommation d’énergie, Bluetooth faible énergie est la réponse. L’espace de noms principal incluant cette fonctionnalité est [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quand ne pas utiliser Bluetooth LE**
- Scénarios d’utilisation très fréquente, impliquant une bande passante élevée. Si vous devez vous synchroniser continuellement avec de grandes quantités de données, envisagez d’utiliser Bluetooth Classic ou encore le WiFi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

Les API RFCOMM permettent aux développeurs un socket pour établir une communication de style de port série bidirectionnel. Une fois que vous avez un socket, les méthodes pour écrire et lire sont relativement standard. Une implémentation de cette approche est présentée dans l’[exemple de conversation RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quand ne pas utiliser Bluetooth Rfcomm** 
- Notifications : le protocole GATT Bluetooth dispose d’une commande spécifique à cet effet et entraînera une économie d’énergie substantielle, ainsi que des temps de réponse beaucoup plus courts. 
- Vérification de proximité ou détection de présence : il est préférable d’utiliser les [API Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) et de se connecter par le biais de Bluetooth LE. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Pourquoi mon périphérique Bluetooth LE ne répond plus après une déconnexion ?

La raison la plus courante dans ce cas est, car le périphérique distant a perdu des informations de correspondance. Un grand nombre d’anciens appareils Bluetooth ne nécessite pas d’authentification. Pour protéger les utilisateurs, toutes les transactions appariement effectuées à partir de l’application paramètres nécessiteront l’authentification, et certains appareils n’ont pas été conçues avec cela à l’esprit. 

À compter de Windows 10 version 1511, les développeurs ont un contrôle sur le protocole de transfert appariement. L’[exemple d’énumération des périphériques et de jumelage](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) détaille les différents aspects liés à l’association de nouveaux périphériques.

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

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>Dois-je jumeler des périphériques Bluetooth avant de les utiliser ?

Vous n’êtes pas obligé les appareils de paire avant de les utiliser si en tirant parti de Bluetooth RFCOMM (classic). À compter de Windows 10 version 1607, vous pouvez simplement interroger les appareils à proximité et vous y connecter. L’[exemple de discussion RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) mis à jour présente cette fonctionnalité. 

**(14393 et antérieur)** Cette fonctionnalité n’est pas disponible pour Bluetooth Low Energy (client GATT). Vous devrez donc continuer le couplage par le biais de la page Paramètres ou à l’aide des API [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) pour accéder à ces appareils.

**(15030 et ultérieur)** Le couplage d’appareils Bluetooth n’est plus nécessaire. Pour rechercher l’état actuel de l’appareil distant, utilisez les nouvelles API Async, telles que GetGattServicesAsync et GetCharacteristicsAsync. Pour plus d’informations, consultez la [documentation relative au client](gatt-client.md). 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quand dois-je effectuer un couplage à un appareil avant de communiquer avec ce dernier ?
En règle générale, si vous avez besoin d’une obligation de confiance, à long terme avec un appareil, paire avec elle en soit redirigeant l’utilisateur à la page Paramètres ou à l’aide de l’énumération du périphérique et le jumelage des API. Si vous devez simplement à lire les informations à partir de l’appareil est exposées publiquement (un capteur de température ou signalisation d’incidents), puis vous connecter ou écouter les annonces sans apporter de tout effort pour coupler avec l’appareil. Cela empêche les problèmes d’interopérabilité à long terme, car un grand nombre d’appareils ne prennent pas en charge le couplage. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Tous les appareils Windows prennent-ils en charge le rôle périphérique ?

Non. Il s’agit d’une fonctionnalité dépendent du matériel, mais une méthode est fournie, BluetoothAdapter.IsPeripheralRoleSupported, pour interroger si elle est prise en charge ou non.  Les appareils actuellement pris en charge comprennent Windows Phone sur 8992+ et RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Puis-je accéder à ces API à partir de Win32 ?

Oui, toutes ces API doivent fonctionner. Ce blog décrit la procédure à suivre pour appeler des [API Windows à partir d’applications de bureau](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Cette fonctionnalité est-elle censée exister sur *-insérer la référence SKU ici-*  ?

**Bluetooth LE**: Oui, toutes les fonctionnalités sont dans OneCore et doivent être disponible sur les appareils plus récente avec une pile Bluetooth LE fonctionnement. 
> Inconvénient : Rôle périphérique dépend du matériel et de certaines éditions de Windows Server ne prennent pas en charge le Bluetooth. 

**Bluetooth BR/EDR (classique)** : Il existe des variations, mais surtout, elles ont très similaire prise en charge de niveau de profil. Consultez la documentation sur [RFCOMM](send-or-receive-files-with-rfcomm.md) et ces pris en charge pour les documents de profil [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) et [téléphone](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)
