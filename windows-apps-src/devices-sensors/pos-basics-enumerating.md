---
author: TerryWarwick
title: Énumération d'appareils PointOfService
description: Découvrez comment énumérer les appareils PointOfService
ms.author: jken
ms.date: 06/8/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: be1fdc42295fc03ff86a69e287a4089abe547689
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018437"
---
# <a name="enumerating-point-of-service-devices"></a>Énumération d'appareils de point de service
Dans cette section, vous allez découvrir comment [**définir un sélecteur d’appareil**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) qui sert à interroger les appareils disponibles pour le système et comment utiliser ce sélecteur pour énumérer les appareils de point de service à l’aide d’une des méthodes suivantes:

**Méthode1:** [**Obtenir le premier périphérique disponible**](#Method-1:-get-first-available-device)<br />Dans cette section, vous allez apprendre à utiliser **GetDefaultAsync** pour accéder au premier périphérique disponible dans une classe d’appareil PointOfService spécifique.

**Méthode2:** [**Capture instantanée d’appareils**](#Method-2:-Snapshot-of-devices)<br />Dans cette section, vous allez apprendre à énumérer une capture instantanée d’appareils PointOfService présents sur le système à un moment donné dans le temps. Cela est utile si vous souhaitez créer votre propre interface utilisateur ou devez énumérer les appareils sans afficher d’interface à l’utilisateur. FindAllAsync retient les résultats jusqu'à ce que l’énumération soit entièrement terminée.

**Méthode3:** [**Énumérer et observer**](#Method-3:-Enumerate-and-watch)<br />Dans cette section, vous découvrirez un modèle d’énumération plus puissant et plus souple qui permet d’énumérer les appareils actuellement présents, ainsi que de recevoir des notifications lorsque des périphériques sont ajoutés ou supprimés à partir du système.  Cela est utile si vous souhaitez tenir à jour une liste des périphériques en arrière-plan pour les afficher dans votre interface utilisateur au lieu d’attendre qu'une capture instantanée se produise.
 

---
## <a name="define-a-device-selector"></a>Définir un sélecteur d’appareil
Le sélecteur d’appareil permet de limiter les appareils que vous parcourez lors de l’énumération de ceux-ci.  Cela vous permet d’obtenir uniquement des résultats pertinents et de réduire le temps nécessaire pour énumérer les appareils de votre choix.  

L'utilisation de [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) vous fournira un sélecteur pour énumérer tous les POSPrinters connectés au système, notamment les POSPrinters Bluetooth, réseau et USB.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();   

```

À l'aide de la méthode [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector_Windows_Devices_PointOfService_PosConnectionTypes_) prenant [**PosConnectionTypes**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) en tant que paramètre, vous pouvez limiter votre sélecteur pour qu'il énumère les POSPrinters connectés en local, sur le réseau ou via Bluetooth, afin de réduire le temps nécessaire pour effectuer la requête.  L’exemple ci-dessous montre l’utilisation de cette méthode pour définir un sélecteur qui prend en charge uniquement les POSPrinters connectés en local.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);   

```
> [!TIP]
> Voir [**Créer un sélecteur d’appareil**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) pour créer des chaînes de sélecteur plus avancées.

---

## <a name="method-1-get-first-available-device"></a>Méthode1: Obtenir le premier périphérique disponible

Le moyen le plus simple d'obtenir un appareil PointOfService consiste à utiliser **GetDefaultAsync** pour obtenir le premier périphérique disponible au sein d’une classe d’appareils PointOfService. 

L’exemple ci-dessous illustre l’utilisation de [**GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) pour BarcodeScanner. Le modèle de codage est identique pour toutes les classes de périphérique PointOfService.

```Csharp

using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();

```

> [!CAUTION]
> GetDefaultAsync doit être utilisé avec précaution car elle peut retourner un autre appareil d’une session à l’autre. De nombreux événements peuvent influencer cette énumération pour donner un résultat de premier périphérique disponible différent, notamment: 
> - Modification des caméras connectées à votre ordinateur 
> - Modification des appareils PointOfService connectés à votre ordinateur
> - Modification des appareils PointOfService connectés en réseau disponibles sur votre réseau
> - Modification des appareils PointOfService Bluetooth à portée de votre ordinateur 
> - Modifications apportées à la configuration PointOfService 
> - Installation des pilotes ou des objets de service OPOS
> - Installation des extensions PointOfService
> - Mise à jour du système d’exploitationWindows

---

## <a name="method-2-snapshot-of-devices"></a>Méthode2: Capture instantanée d’appareils

Dans ceratins scénarios, vous souhaiterez créer votre propre interface utilisateur ou devrez énumérer les appareils sans afficher d’interface à l’utilisateur.  Dans ces situations, vous pouvez énumérer une capture instantanée d’appareils actuellement connectés ou associés avec le système à l’aide de [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Cette méthode retient les résultats jusqu'à ce que l’énumération soit entièrement terminée.

> [!TIP]
> Il est recommandé d’utiliser la méthode GetDeviceSelector avec le paramètre PosConnectionTypes lors de l’utilisation de FindAllAsync afin de limiter votre requête au type de connexion souhaité.  Les connexions réseau et Bluetooth peuvent retarder les résultats car leurs énumérations doivent être terminées pour que FindAllAsync puisse retourner les résultats.

>[!CAUTION] 
>FindAllAsync retourne un tableau d'appareils.  L’ordre de ce tableau peut changer d’une session à l'autre, par conséquent, il est déconseillé de s'appuyer sur un ordre spécifique en utilisant un index codé en dur dans le tableau.  Utilisez les propriétés DeviceInformation pour filtrer les résultats ou fournissez une interface utilisateur dans laquelle l’utilisateur peut choisir.

Cet exemple utilise le sélecteur défini ci-dessus pour prendre une capture instantanée des appareils à l’aide de FindAllAsync, puis énumère chacun des éléments renvoyés par la collection et écrit le nom et l’ID de l’appareil dans la sortie de débogage. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Lorsque vous utilisez les API [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), vous devez fréquemment utiliser des objets [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) pour obtenir des informations sur un appareil spécifique. Par exemple, la propriété [**DeviceInformation.ID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) permet de récupérer et de réutiliser le même appareil s'il est disponible dans une session future et la propriété [**DeviceInformation.Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) peut être utilisée à des fins d’affichage dans votre application.  Consultez la page de référence [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) pour plus d’informations sur les autres propriétés disponibles.

---

## <a name="method-3-enumerate-and-watch"></a>Méthode3: Énumérer et observer

Une méthode plus puissante et plus flexible d’énumération des appareils consiste à créer un [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  L'observateur de périphériques énumère des périphériques de façon dynamique afin que l'application reçoive des notifications si des périphériques sont ajoutés, supprimés ou modifiés à la fin de l'énumération initiale.  Le DeviceWatcher vous permet de détecter si un appareil connecté au réseau est en ligne, si un appareil Bluetooth est à portée ou si un périphérique connecté en local est débranché, afin de prendre des mesures appropriées dans votre application.

Cet exemple utilise le sélecteur défini ci-dessus pour créer un DeviceWatcher et définit des gestionnaires d’événements pour les notifications Ajouté, Supprimé et Mis à jour. Vous devez indiquer les détails des actions que vous souhaitez effectuer lors de chaque notification.

```Csharp

using Windows.Devices.Enumeration;

DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

> [!TIP]
> Voir [**Énumérer et observer des appareils**]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) pour plus d’informations sur l'utilisation d'un DeviceWatcher.
