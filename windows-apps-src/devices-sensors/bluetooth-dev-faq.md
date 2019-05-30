---
title: FAQ sur le Bluetooth pour les développeurs
description: Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 72e45f8ef0f5684b3a712056eb367975f8e6103a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370348"
---
# <a name="bluetooth-developer-faq"></a>FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quelles API dois-je utiliser ? Bluetooth Classic (RFCOMM) ou Bluetooth Low Energy (GATT) ?
Cette question générale fait l’objet de nombreuses discussions en ligne. Nous examinerons donc la différence entre ces deux types d’API strictement du point de vue de Windows. Voici quelques recommandations générales :

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Utilisez les API GATT lorsque vous communiquez avec un appareil qui prend en charge la technologie Bluetooth Low Energy. Si l’utilisation que vous en faites est peu fréquente ou ne nécessite qu’une faible bande passante ou une faible consommation d’énergie, choisissez la technologie Bluetooth Low Energy. L’espace de noms principal incluant cette fonctionnalité est [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quand ne pas utiliser Bluetooth LE**
- Scénarios d’utilisation très fréquente, impliquant une bande passante élevée. Si vous devez vous synchroniser continuellement avec de grandes quantités de données, envisagez d’utiliser Bluetooth Classic ou encore le WiFi. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

Les API RFCOMM fournissent aux développeurs un socket permettant d’établir une communication bidirectionnelle de type port série. Une fois que vous disposez d’un socket, les méthodes d’écriture et de lecture en direction et en provenance de ce dernier sont relativement standard. Une implémentation de cette approche est présentée dans l’[exemple de conversation RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quand ne pas utiliser Bluetooth Rfcomm** 
- Notifications : le protocole GATT Bluetooth dispose d’une commande spécifique à cet effet et entraînera une économie d’énergie substantielle, ainsi que des temps de réponse beaucoup plus courts. 
- Vérification de proximité ou détection de présence : il est préférable d’utiliser les [API Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) et de se connecter par le biais de Bluetooth LE. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Pourquoi mon périphérique Bluetooth LE ne répond plus après une déconnexion ?

La raison courante pour laquelle cela se produit est due au fait que l’appareil distant a perdu des informations de jumelage. Un grand nombre de périphériques Bluetooth antérieurs ne nécessitent pas d’authentification. Pour protéger l’utilisateur, tous les processus de jumelage effectués à partir de l’application Paramètres exigent une authentification et certains appareils ne savent pas comment les traiter. 

À compter de Windows 10 version 1511, les développeurs contrôlent le processus de couplage. L’[exemple d’énumération des périphériques et de jumelage](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) détaille les différents aspects liés à l’association de nouveaux périphériques.

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

Vous n’êtes pas contraint de le faire pour les appareils Bluetooth RFCOMM (standard). À compter de Windows 10 version 1607, vous pouvez simplement interroger les appareils à proximité et vous y connecter. L’[exemple de discussion RFCOMM](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) mis à jour présente cette fonctionnalité. 

**(14393 et antérieur)** Cette fonctionnalité n’est pas disponible pour Bluetooth Low Energy (client GATT). Vous devrez donc continuer le couplage par le biais de la page Paramètres ou à l’aide des API [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) pour accéder à ces appareils.

**(15030 et ultérieur)** Le couplage d’appareils Bluetooth n’est plus nécessaire. Pour rechercher l’état actuel de l’appareil distant, utilisez les nouvelles API Async, telles que GetGattServicesAsync et GetCharacteristicsAsync. Pour plus d’informations, consultez la [documentation relative au client](gatt-client.md). 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quand dois-je effectuer un couplage à un appareil avant de communiquer avec ce dernier ?
En règle générale, si vous avez besoin d’établir une liaison à long terme approuvée avec un appareil, effectuez un couplage à ce dernier (soit en dirigeant l’utilisateur vers la page de paramètres, soit en utilisant les API d’énumération des appareils et de couplage). Si vous devez simplement lire les informations hors tension l’appareil qui est exposée publiquement (un capteur de température ou signalisation d’incidents), puis vous connecter ou écouter les annonces sans apporter de tout effort pour coupler avec l’appareil. Cette approche évitera les problèmes d’interopérabilité à long terme, car de nombreux appareils ne prennent pas en charge le couplage. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Tous les appareils Windows prennent-ils en charge le rôle périphérique ?

Non. Cette fonctionnalité dépendant du matériel. Toutefois, il existe une méthode (BluetoothAdapter.IsPeripheralRoleSupported) permettant de vérifier si ce rôle est ou non pris en charge.  Les appareils actuellement pris en charge comprennent Windows Phone sur 8992+ et RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Puis-je accéder à ces API à partir de Win32 ?

Oui, toutes ces API doivent fonctionner. Ce blog décrit la procédure à suivre pour appeler des [API Windows à partir d’applications de bureau](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Cette fonctionnalité est-elle censée exister sur *-insérer la référence SKU ici-*  ?

**Bluetooth LE**: Oui, toutes les fonctionnalités sont dans OneCore et doivent être disponible sur les appareils plus récente avec une pile Bluetooth LE fonctionnement. 
> Inconvénient : Rôle de périphérique est dépendent du matériel, et certaines éditions de Windows Server ne prennent pas en charge le Bluetooth. 

**Bluetooth BR/EDR (classique)** : Il existe des variations mais ensemble, ils ont très similaire prise en charge de niveau de profil. Consultez la documentation concernant [RFCOMM](send-or-receive-files-with-rfcomm.md), ainsi que ces articles sur les profils pris en charge pour [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) et pour [appareils mobiles](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

