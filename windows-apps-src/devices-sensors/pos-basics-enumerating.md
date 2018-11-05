---
author: TerryWarwick
title: Énumération d'appareils PointOfService
description: Découvrez comment énumérer les appareils PointOfService
ms.author: jken
ms.date: 10/08/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 10804b006cb7ab542c74e363af5134634b7651e3
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6033123"
---
# <a name="enumerating-point-of-service-devices"></a>Énumération d'appareils de point de service
Dans cette section, vous allez découvrir comment [définir un sélecteur d’appareil](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) qui sert à interroger les appareils disponibles pour le système et comment utiliser ce sélecteur pour énumérer les appareils de point de service à l’aide d’une des méthodes suivantes:

**Méthode 1:** [Utilisez un sélecteur d’appareil](#method-1:-use-a-device-picker)
<br/>
Afficher un sélecteur d’appareil de l’interface utilisateur et demander à l’utilisateur de choisir un appareil connecté. Cette méthode gère la mise à jour de la liste lorsque les périphériques sont reliés et supprimés et est plus simple et plus sûre que les autres méthodes.

**Méthode 2:** [Obtenir le premier périphérique disponible](#Method-1:-get-first-available-device)<br />Utilisez [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) pour accéder au premier périphérique disponible dans une classe de périphérique de Point de Service spécifique.

**Méthode 3:** [Capture instantanée d’appareils](#Method-2:-Snapshot-of-devices)<br />Énumérer une capture instantanée d’appareils de Point de Service qui sont présents sur le système à un moment donné dans le temps. Cela est utile si vous souhaitez créer votre propre interface utilisateur ou devez énumérer les appareils sans afficher d’interface à l’utilisateur. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) retient résultats jusqu'à ce que l’énumération soit terminée.

**Méthode 4:** [Énumérer et observer](#Method-3:-Enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) est un modèle d’énumération plus puissant et plus souple qui vous permet d’énumérer les appareils actuellement présents, et également recevoir des notifications lorsque les périphériques sont ajoutés ou supprimés à partir du système.  Cela est utile si vous souhaitez tenir à jour une liste des périphériques en arrière-plan pour les afficher dans votre interface utilisateur au lieu d’attendre qu'une capture instantanée se produise.

## <a name="define-a-device-selector"></a>Définir un sélecteur d’appareil
Le sélecteur d’appareil permet de limiter les appareils que vous parcourez lors de l’énumération de ceux-ci.  Cela vous permettra d’obtenir des résultats pertinents uniquement et de réduire le temps que nécessaire pour énumérer les appareils de votre choix.

Vous pouvez utiliser la méthode **GetDeviceSelector** pour le type d’appareil que vous cherchez à obtenir le sélecteur d’appareil pour ce type. Par exemple, à l’aide de [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) fournira vous avec un sélecteur pour énumérer tous les [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) connectés au système, y compris, réseau et les imprimantes de PDV Bluetooth.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Les méthodes **GetDeviceSelector** pour les différents types de périphériques sont:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

À l’aide d’une méthode **GetDeviceSelector** qui prend une valeur [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) en tant que paramètre, vous pouvez limiter votre sélecteur pour énumérer les local, réseau, ou les appareils de PDV Bluetooth connectés, réduisant ainsi le temps que nécessaire pour effectuer la requête.  L’exemple ci-dessous montre une utilisation de cette méthode pour définir un sélecteur qui prend en charge que localement connectés imprimantes POS.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Voir [Créer un sélecteur d’appareil](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) pour créer des chaînes de sélecteur plus avancées.

## <a name="method-1-use-a-device-picker"></a>Méthode 1: Utiliser un sélecteur d’appareil

La classe [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) vous permet d’afficher un menu volant sélecteur qui contient une liste des appareils pour l’utilisateur de choisir parmi les. Vous pouvez utiliser la propriété [Filter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) pour choisir les types d’appareils s’affichent dans le sélecteur. Cette propriété est de type [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Vous pouvez ajouter les types d’appareil pour le filtre à l’aide de la propriété [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) ou [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) .

Lorsque vous êtes prêt à afficher le sélecteur de périphérique, vous pouvez appeler la méthode [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) , qui affiche l’interface utilisateur du sélecteur et renvoie le périphérique sélectionné. Vous devez spécifier un [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) qui détermine où le menu volant s’affiche. Cette méthode retourne un objet [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) , par conséquent, pour l’utiliser avec le Point de Service API, vous devez utiliser la méthode **FromIdAsync** pour la classe d’appareil particulier que vous souhaitez. Vous passez la propriété [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) en tant que paramètre *d’ID d’appareil* de la méthode et que vous obtenez une instance de la classe de périphérique en tant que la valeur de retour.

L’extrait de code suivant crée un **DevicePicker**, ajoute un filtre de scanneur de code-barres, a l’utilisateur de choisir un appareil et crée ensuite un objet **BarcodeScanner** basé sur l’ID de périphérique:

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>Méthode 2: Obtenir le premier périphérique disponible

Pour obtenir un périphérique de Point de Service, le plus simple consiste à utiliser **GetDefaultAsync** pour obtenir le premier périphérique disponible au sein d’une classe de périphérique de Point de Service. 

L’exemple ci-dessous illustre l’utilisation de [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) pour [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). Le modèle de codage est identique pour toutes les classes de périphériques de Point de Service.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** doit être utilisée avec précaution car elle peut retourner un autre appareil à partir d’une session à l’autre. De nombreux événements peuvent influencer cette énumération pour donner un résultat de premier périphérique disponible différent, notamment: 
> - Modification des caméras connectées à votre ordinateur 
> - Modifier sur les lieux de périphériques de Service connectés à votre ordinateur
> - Modification des NAS appareils du Point de Service disponibles sur votre réseau
> - Modification des appareils de Point de Service Bluetooth à portée de votre ordinateur 
> - Modifications apportées à la configuration de Point de Service 
> - Installation des pilotes ou des objets de service OPOS
> - Installation des extensions de Point de Service
> - Mise à jour du système d’exploitationWindows

## <a name="method-3-snapshot-of-devices"></a>Méthode 3: Capture instantanée d’appareils

Dans ceratins scénarios, vous souhaiterez créer votre propre interface utilisateur ou devrez énumérer les appareils sans afficher d’interface à l’utilisateur.  Dans ces situations, vous pouvez énumérer une capture instantanée d’appareils actuellement connectés ou associés avec le système à l’aide de [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Cette méthode retient les résultats jusqu'à ce que l’énumération soit entièrement terminée.

> [!TIP]
> Il est recommandé d’utiliser la méthode **GetDeviceSelector** avec le paramètre **PosConnectionTypes** lors de l’utilisation de **FindAllAsync** afin de limiter votre requête au type de connexion souhaité.  Connexions réseau et Bluetooth peuvent retarder les résultats car leurs énumérations doivent être terminées avant de retourner les résultats de **FindAllAsync** .

> [!CAUTION] 
> **FindAllAsync** retourne un tableau d’appareils.  L’ordre de ce tableau peut changer d’une session à l'autre, par conséquent, il est déconseillé de s'appuyer sur un ordre spécifique en utilisant un index codé en dur dans le tableau.  Utilisez les propriétés [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) pour filtrer les résultats ou fournir une interface utilisateur pour l’utilisateur peut choisir.

Cet exemple utilise le sélecteur défini ci-dessus pour prendre une capture instantanée d’appareils à l’aide de **FindAllAsync** , puis énumère chacun des éléments renvoyés par la collection et écrit le nom de l’appareil et l’ID de la sortie de débogage. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Lorsque vous utilisez les API [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), vous devez fréquemment utiliser des objets [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) pour obtenir des informations sur un appareil spécifique. Par exemple, la propriété [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) peut être utilisée pour récupérer et réutiliser le même appareil s’il est disponible dans une session future et la propriété [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) peut être utilisée à des fins d’affichage dans votre application.  Consultez la page de référence [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) pour plus d’informations sur les autres propriétés disponibles.

## <a name="method-4-enumerate-and-watch"></a>Méthode 4: Énumérer et observer

Une méthode plus puissante et plus flexible d’énumération des appareils consiste à créer un [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  L'observateur de périphériques énumère des périphériques de façon dynamique afin que l'application reçoive des notifications si des périphériques sont ajoutés, supprimés ou modifiés à la fin de l'énumération initiale.  Un **DeviceWatcher** vous permet de détecter si un appareil connecté au réseau est en ligne, si un périphérique Bluetooth est à portée ou si un périphérique connecté en local est débranché, afin que vous puissiez prendre les mesures appropriées au sein de votre application.

Cet exemple utilise le sélecteur défini ci-dessus pour créer un **DeviceWatcher** aussi définit les gestionnaires d’événements pour les notifications [ajouté](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [supprimé](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)et [mis à jour](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) . Vous devez indiquer les détails des actions que vous souhaitez effectuer lors de chaque notification.

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
> Pour plus d’informations sur l’utilisation d’un **DeviceWatcher**, consultez [énumérer et observer des appareils]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) .

## <a name="see-also"></a>Voir aussi
* [Prise en main de la technologie de point de service](pos-basics.md)
* [Classe DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Classe PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [Énumération PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Classe BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Classe DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]