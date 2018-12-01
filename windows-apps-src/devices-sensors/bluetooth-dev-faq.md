---
title: FAQ sur le Bluetooth pour les développeurs
description: Cet article contient des réponses aux questions fréquentes relatives à l’API de bluetooth UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 03b72b5722a3ece0165fc63e7ce4abc1238bc135
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8343774"
---
# <a name="bluetooth-developer-faq"></a>FAQ sur le Bluetooth pour les développeurs

Cet article contient des réponses aux questions les plus fréquentes concernant les API Bluetooth UWP.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Quelles API utiliser? Bluetooth classique (RFCOMM) ou Bluetooth Low Energy (GATT)?
Il existe diverses discussions en ligne autour de cette rubrique générale par conséquent, nous allons conserver cette réponse parfaitement sur la différence par rapport à Windows. Voici quelques recommandations générales:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Utilisez les API GATT lorsque vous communiquez avec un appareil qui prend en charge de Bluetooth Low Energy. Si vous êtes utiliser cas est la bande passante faible, rare ou nécessite de faible consommation d’énergie, Bluetooth Low Energy est la réponse. L’espace de noms principal qui inclut cette fonctionnalité est [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Quand ne pas utiliser Bluetooth LE**
- Bande passante élevée, les scénarios de haute fréquence. Si vous avez besoin de conserver en permanence la synchronisation avec de grandes quantités de données, envisagez d’utiliser Bluetooth et Wi-Fi classique ou peut-être même. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth classique (Windows.Devices.Bluetooth.Rfcomm)

Les API RFCOMM fournit aux développeurs un socket à établir une communication de style de port série et bidirectionnel. Une fois que vous disposez d’un socket, les méthodes d’écriture dans et de lecture à partir de celui-ci sont relativement standard. Une implémentation de cette est présentée dans l' [exemple de conversation Rfcomm](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Quand ne pas utiliser de Bluetooth Rfcomm** 
- Notifications. Le protocole Bluetooth GATT dispose d’une commande spécifique pour ce faire et entraîne beaucoup moins courant et diminution du délai de réponse. 
- Vérification de la détection de présence ou de proximité. Préférable d’utiliser les [API d’annonce](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) et se connecter via Bluetooth LE. 


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

**(14393 et ci-dessous)** Cette fonctionnalité n’est pas disponible pour Bluetooth Low Energy (Client GATT), afin que vous aurez toujours à paire soit par le biais de la page des paramètres ou à l’aide de l’API [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) dans accès ordre ces périphériques.

**(15030 et ci-dessus)** COUPLAGE d’appareils Bluetooth n’est plus nécessaire. Utilisez les nouvelles APIs asynchrone, tels que GetGattServicesAsync et GetCharacteristicsAsync afin d’interroger l’état actuel de l’appareil distant. Consultez la [documentation du Client](gatt-client.md) pour plus d’informations. 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Quand dois-je jumeler avec un appareil avant de pouvoir communiquer avec celle-ci?
En règle générale, si vous avez besoin d’une obligation de confiance, à long terme avec un appareil, jumeler avec ce dernier (qui redirige l’utilisateur à la page Paramètres ou à l’aide de l’énumération des appareils et les API de couplage). Si vous devez simplement lus d’informations sur l’appareil qui est exposée publiquement (un capteur de température ou beacon), puis se connecter ou écouter les annonces sans apporter aucun effort de coupler avec l’appareil. Cela empêchera les problèmes d’interopérabilité à long terme, car un grand nombre d’appareils ne prennent pas en charge le couplage. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Tous les appareils Windows prennent en charge le rôle périphérique?

Non, il s’agit d’un composant dépendant du matériel, mais une méthode est fournie (BluetoothAdapter.IsPeripheralRoleSupported) pour rechercher si elle est prise en charge ou non.  Incluent des périphériques actuellement pris en charge Windows Phone sur 8992 + et RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Puis-je accéder à ces API à partir de Win32?

Oui, toutes ces API doit fonctionner. Ce blog décrit en détail la façon d’appeler des [API Windows à partir d’applications de bureau](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/). 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Cette fonctionnalité est censée exister sur *- Insérer référence (SKU) ici -*?

**Bluetooth LE**: Oui, toutes les fonctionnalités dans OneCore et devrait être disponible sur les appareils la plus récentes avec une pile Bluetooth LE ne fonctionnent pas. 
> Avertissement: Le rôle périphérique est dépend du matériel et certaines éditions de Windows Server ne prend pas en charge le Bluetooth. 

**Bluetooth BR/EDR (classique)**: il existe des variations, mais de manière générale, ils ont une prise en charge très similaire de niveau profil. Voir la documentation sur [RFCOMM](send-or-receive-files-with-rfcomm.md) et ces documents profil pris en charge pour les [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) et votre [téléphone](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

