---
author: msatranjr
title: Bluetooth basse énergie
description: Cette rubrique fournit une vue d’ensemble de Bluetooth LE dans les applications UWP.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: Windows 10, uwp, bluetooth, bluetooth LE, faible consommation d’énergie, gatt, écart, central, périphérique, client, serveur, l’Observateur, éditeur
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5751840"
---
# <a name="bluetooth-low-energy"></a>Bluetooth basse énergie
Bluetooth Low Energy (LE) est une spécification qui définit les protocoles de découverte et de communication entre les appareils économe en énergie. La découverte d’appareils est effectuée par le biais du protocole de profil d’accès générique (espace). Après la découverte, les appareils de communication s’effectue par le biais du protocole d’attribut générique (GATT). Cette rubrique fournit une vue d’ensemble de Bluetooth LE dans les applications UWP. Pour afficher plus de détails sur Bluetooth LE, consultez la [Spécification de Core Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) version 4.0, où Bluetooth L’a été introduit. 

![Rôles LE Bluetooth](images/gatt-roles.png)

*Rôles GATT et écart ont été introduites dans Windows 10 version 1703*

Protocoles GATT et intervalle peuvent être implémentés dans votre application UWP à l’aide des espaces de noms suivants.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Centrale et périphérique
Les deux rôles principales de découverte sont appelés Central et le périphérique. En règle générale, Windows fonctionne en mode Central et se connecte à des périphériques différents. 

## <a name="attributes"></a>Attributs
Un acronyme courants que vous verrez dans APIs Bluetooth Windows est attribut générique (GATT). Le profil de GATT définit la structure de données et les modes de fonctionnement par lequel communiquer entre deux appareils Bluetooth LE. L’attribut est le bloc de construction principal du GATT. Les principaux types d’attributs sont des services, caractéristiques et descripteurs. Ces attributs effectuent différemment entre clients et serveurs, pourquoi il est plus utile discuter de leur interaction dans les sections correspondantes. 

![Hiérarchie d’attribut classique dans un profil commun](images/gatt-service.png)

*Le service de fréquence cardiaque est exprimé sous forme de l’API de serveur GATT*

## <a name="client-and-server"></a>Client et serveur
Une fois une connexion établie, l’appareil qui contient les données (généralement un capteur IoT petit ou appareil portable) est appelé le serveur. L’appareil qui utilise ces données pour effectuer une fonction est désignée sous le Client. Par exemple, un PC Windows (Client) lit à partir d’un moniteur de fréquence cardiaque (serveur) pour effectuer le suivi les données qu’un utilisateur travaille optimale. Pour plus d’informations, consultez les rubriques [GATT Client](gatt-client.md) et [Serveur GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Observateurs et les éditeurs (balises)
Outre les rôles Central et le périphérique, il existe rôles observateur et de la chaîne de télévision. Chaînes de télévision sont couramment appelés balises, ils ne communiquent sur GATT parce qu’ils utilisent l’espace limité fourni dans le paquet de publication pour la communication. De même, un observateur n’a pas établir une connexion pour recevoir des données, il analyse pour les annonces publicitaires à proximité. Pour configurer Windows pour observer à proximité de publicités, utilisez la classe [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Afin de diffuser des charges utiles de balise, utilisez la classe [annonce BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Pour plus d’informations, voir la rubrique [d’annonce](ble-beacon.md) .

## <a name="see-also"></a>Articles associés
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Spécification Core Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)