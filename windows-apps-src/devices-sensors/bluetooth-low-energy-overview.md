---
author: msatranjr
title: Bluetooth Low Energy
description: Cette rubrique fournit une brève présentation du Bluetooth dans les applications UWP.
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, bluetooth, bluetooth LE, faible consommation d’énergie, gatt, intervalles, central, périphérique, client, server, Observateur, publisher
ms.localizationpriority: medium
ms.openlocfilehash: 0d6cc1fb5a0b3cb96748b99c490b23a9e1df128f
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "305084"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth faible énergie (LE) est une spécification qui définit les protocoles de découverte et de communication entre les périphériques énergie. Découverte des périphériques s’effectue via le protocole de profil d’accès générique (GAP). Après la découverte, communication périphérique à s’effectue par le biais du protocole de l’attribut générique (GATT). Cette rubrique fournit une brève présentation du Bluetooth dans les applications UWP. Pour plus de détails sur LE Bluetooth, consultez la [Spécification des principaux Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification) version 4.0, où LE Bluetooth a été introduit. 

![Rôles LE Bluetooth](images/gatt-roles.png)

*Rôles GATT et intervalles ont été introduites dans Windows 10 version 1703*

Protocoles GATT et intervalles peuvent être implémentés dans votre application UWP à l’aide des espaces de noms suivants.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Centrale et périphérique
Les deux rôles primaires de découverte sont appelées centrale et le périphérique. En règle générale, Windows fonctionne en mode Central et se connecte à différents périphériques. 

## <a name="attributes"></a>Attributs
Un acronyme courantes, que vous verrez dans APIs Bluetooth Windows est attribut générique (GATT). Le profil GATT définit la structure des données et les modes de fonctionnement par lequel deux périphériques Bluetooth LE communiquent. L’attribut est le bloc de construction principal du GATT. Les principaux types d’attributs sont les services, les caractéristiques et les descripteurs. Ces attributs effectuer différemment entre clients et serveurs, il est plus utile traiter de leur interaction dans les sections correspondantes. 

![Hiérarchie d’attribut par défaut dans un profil commun](images/gatt-service.png)

*Le service de taux de cœur est exprimé sous forme de GATT Server API*

## <a name="client-and-server"></a>Client et serveur
Une fois une connexion établie, le périphérique qui contient les données (généralement un petit détecteur IoT ou portable) est appelé le serveur. Le périphérique qui utilise les données pour exécuter une fonction est appelé le Client. Par exemple, un ordinateur Windows (Client) lit à partir d’un moniteur taux cœur (serveur) pour effectuer le suivi des données qu’un utilisateur fonctionne optimale. Pour plus d’informations, consultez les rubriques [GATT Client](gatt-client.md) et [Serveur GATT](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Observateurs et éditeurs (balises)
Outre les rôles Central et des périphériques, il existe rôles observateur et de chaîne. Chaînes de télévision sont appelés balises, ils ne communiquent sur GATT parce qu’ils utilisent l’espace limité fournie dans le paquet de publication pour la communication. De même, un observateur ne dispose pas d’établir une connexion pour recevoir des données, il analyse les annonces à proximité. Pour configurer Windows à respecter à proximité de publicités, utilisez la classe [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) . Afin de charges utiles de balise de diffusion, utilisez la classe [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) . Pour plus d’informations, consultez la rubrique [publication](ble-beacon.md) .

## <a name="see-also"></a>Articles associés
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Spécification des principaux Bluetooth](https://www.bluetooth.com/specifications/bluetooth-core-specification)