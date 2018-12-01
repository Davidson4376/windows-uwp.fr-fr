---
title: Bluetooth basse énergie
description: Cette rubrique fournit une vue d’ensemble de Bluetooth LE dans les applications UWP.
ms.date: 03/15/2017
ms.topic: article
keywords: Windows 10, uwp, bluetooth, bluetooth LE, faible consommation d’énergie, gatt, écart, central, périphérique, client, serveur, l’Observateur, éditeur
ms.localizationpriority: medium
ms.openlocfilehash: 3853003e54e58b3949c248fb03cb0a83e2d6d112
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8345843"
---
# <a name="bluetooth-low-energy"></a>Bluetooth basse énergie
Bluetooth Low Energy (LE) est une spécification qui définit les protocoles de découverte et de communication entre les appareils économe en énergie. La découverte d’appareils est effectuée par le biais du protocole de profil d’accès générique (écart). Après la découverte, la communication d’appareils est effectuée par le biais du protocole d’attribut générique (GATT). Cette rubrique fournit une vue d’ensemble de Bluetooth LE dans les applications UWP. Pour afficher des informations supplémentaires sur le Bluetooth LE, consultez la [Spécification de Core Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) version 4.0, où Bluetooth L’a été introduit. 

![Rôles LE Bluetooth](images/gatt-roles.png)

*Rôles GATT et écart ont été introduites dans Windows 10 version 1703*

Protocoles GATT et intervalle peuvent être implémentés dans votre application UWP à l’aide des espaces de noms suivants.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Centrale et périphérique
Les deux rôles principales de découverte sont appelés Central et le périphérique. En règle générale, Windows fonctionne en mode Central et se connecte à différents appareils périphériques. 

## <a name="attributes"></a>Attributs
Un acronyme courants que s’affiche dans les Bluetooth APIs Windows est d’attribut générique (GATT). Le profil GATT définit la structure de données et les modes de fonctionnement par lequel communiquer entre deux appareils Bluetooth LE. L’attribut est le bloc de construction principal du GATT. Les principaux types d’attributs sont des services, caractéristiques et descripteurs. Ces attributs effectuent différemment entre les clients et serveurs, pourquoi il est plus utile discuter de leur interaction dans les sections correspondantes. 

![Hiérarchie d’attribut classique dans un profil commun](images/gatt-service.png)

*Le service de fréquence cardiaque est exprimé sous forme de l’API de serveur GATT*

## <a name="client-and-server"></a>Client et serveur
Une fois une connexion établie, l’appareil qui contient les données (généralement un capteur IoT petit ou appareil portable) est appelé le serveur. L’appareil qui utilise ces données pour effectuer une fonction est appelée sur le Client. Par exemple, un PC Windows (Client) lit à partir d’un moniteur de fréquence cardiaque (serveur) pour effectuer le suivi les données qu’un utilisateur travaille optimale. Pour plus d’informations, consultez les rubriques [GATT Client](gatt-client.md) et [Serveur GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Observateurs et éditeurs (balises)
Outre les rôles Central et de périphérique, il existe rôles observateur et de la chaîne de télévision. Chaînes de télévision sont couramment appelés balises, ils ne communiquent sur GATT parce qu’ils utilisent l’espace limité fourni dans le paquet de publication pour la communication. De même, un observateur n’a pas établir une connexion pour recevoir des données, il analyse pour les annonces publicitaires à proximité. Pour configurer Windows d’observer à proximité des annonces publicitaires, utilisez la classe [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Pour pouvoir diffuser des charges utiles de beacon, utilisez la classe [annonce BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Pour plus d’informations, voir la rubrique [d’annonce](ble-beacon.md) .

## <a name="see-also"></a>Articles associés
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Spécification Core Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)