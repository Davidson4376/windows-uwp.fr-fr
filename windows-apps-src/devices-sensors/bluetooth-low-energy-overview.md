---
title: Bluetooth Low Energy
description: Cet article fournit une rapide vue d’ensemble de Bluetooth LE dans les applications UWP.
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, bluetooth, bluetooth LE, low energy, gatt, gap, central, périphérique, client, serveur, observateur, diffuseur
ms.localizationpriority: medium
ms.openlocfilehash: 3f23bdc658d2a82e3edeefd0a7be471ca9620d33
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321614"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth Low Energy (LE) est une spécification qui définit les protocoles de détection et de communication entre des appareils à faible consommation d’énergie. La détection des appareils s’effectue par le biais du protocole de profil d’accès générique (GAP). Après la détection, la communication entre les appareils est établie par l’intermédiaire du protocole d’attribut générique (GATT). Cet article fournit une rapide vue d’ensemble de Bluetooth LE dans les applications UWP. Pour plus d’informations sur Bluetooth LE, consultez la [spécification principale Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification/) version 4.0, dans laquelle la technologie Bluetooth LE a été introduite. 

![Rôles Bluetooth LE](images/gatt-roles.png)

*Rôles GATT et GAP ont été introduits dans Windows 10 version 1703*

Les protocoles GATT et GAP peuvent être implémentés dans votre application UWP à l’aide des espaces de noms suivants :
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central et périphérique
Les deux principaux rôles de détection sont le rôle central et le rôle périphérique. En règle générale, Windows fonctionne en mode Central et se connecte à différents appareils périphériques. 

## <a name="attributes"></a>Attributs
L’un des acronymes couramment utilisés dans les API Bluetooth Windows est l’acronyme GATT (Generic Attribute, attribut générique). Le profil GATT définit la structure de données et les modes de fonctionnement permettant à deux appareils Bluetooth LE de communiquer. L’attribut est l’unité élémentaire de GATT. Les principaux types d’attributs sont les services, les caractéristiques et les descripteurs. Ces attributs ne fonctionnent pas de la même façon sur les clients que sur les serveurs ; il convient donc de décrire leurs interactions dans les sections correspondantes. 

![Hiérarchie d’attribut standard dans un profil commun](images/gatt-service.png)

*Le service de rythme cardiaque est exprimé sous forme de l’API de serveur GATT*

## <a name="client-and-server"></a>Client et serveur
Une fois qu’une connexion a été établie, l’appareil qui contient les données (généralement, un petit capteur IoT ou un appareil wearable) est appelé serveur. L’appareil qui utilise ces données pour exécuter une fonction s’appelle le client. Par exemple, un PC Windows (client) lit les données d’un moniteur de fréquence cardiaque (serveur) pour vérifier qu’un utilisateur réalise des performances optimales. Pour plus d’informations, consultez les articles [Client GATT](gatt-client.md) et [Serveur GATT](gatt-server.md).

## <a name="watchers-and-publishers-beacons"></a>Observateurs et diffuseurs (balises)
Outre les rôles Central et Périphérique, il existe des rôles Observateur et Diffuseur. Les diffuseurs sont couramment désignés sous le terme de balises et ne communiquent pas par le biais de GATT, car ils utilisent l’espace limité fourni dans le paquet d’annonce pour la communication. De même, un observateur n’a pas besoin d’établir une connexion pour recevoir des données, car il recherche les annonces à proximité. Pour configurer Windows pour l’observation des annonces à proximité, utilisez la classe [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher). Pour diffuser les charges utiles des balises, utilisez la classe [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher). Pour plus d’informations, consultez l’article concernant les [annonces](ble-beacon.md).

## <a name="see-also"></a>Voir aussi
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Spécification du principal de Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification/)