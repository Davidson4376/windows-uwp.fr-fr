---
title: Énumération d'appareils PointOfService
description: Découvrez comment énumérer les appareils PointOfService
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 27d25864941b9d73c9b12e6329eab79fac1b15bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620904"
---
# <a name="enumerating-point-of-service-devices"></a>Énumération d'appareils de point de service
Dans cette section, vous allez apprendre comment [définir un sélecteur d’appareils](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) qui est utilisé pour interroger les appareils disponibles pour le système et utiliser ce sélecteur d’énumérer les appareils de Point de Service à l’aide d’une des méthodes suivantes :

**Méthode 1 :** [Utiliser un sélecteur de périphérique](#method-1-use-a-device-picker)
<br/>
Afficher un interface utilisateur du sélecteur de périphérique et inviter l’utilisateur à choisir un appareil connecté. Cette méthode gère la mise à jour de la liste lorsque les périphériques sont reliés et supprimées et est plus simple et plus sûre que d’autres méthodes.

**Méthode 2 :** [Obtenir le premier périphérique disponible](#method-2-get-first-available-device)<br />Utilisez [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) d’accéder au premier périphérique disponible dans une classe de périphérique de Point de Service spécifique.

**Méthode 3 :** [Capture instantanée d’appareils](#method-3-snapshot-of-devices)<br />Énumérer une capture instantanée d’appareils de Point de Service qui sont présents sur le système à un moment donné dans le temps. Cela est utile si vous souhaitez créer votre propre interface utilisateur ou devez énumérer les appareils sans afficher d’interface à l’utilisateur. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) retient les résultats jusqu'à ce que l’ensemble de l’énumération est terminée.

**Méthode 4 :** [Énumérer et regardez](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) est un modèle de l’énumération plus puissant et flexible qui vous permet d’énumérer les périphériques qui sont actuellement présents et également recevoir des notifications lorsque les appareils sont ajoutés ou supprimés à partir du système.  Cela est utile si vous souhaitez tenir à jour une liste des périphériques en arrière-plan pour les afficher dans votre interface utilisateur au lieu d’attendre qu'une capture instantanée se produise.

## <a name="define-a-device-selector"></a>Définir un sélecteur d’appareil
Le sélecteur d’appareil permet de limiter les appareils que vous parcourez lors de l’énumération de ceux-ci.  Cela vous permettra uniquement à obtenir des résultats pertinents et réduire le temps que nécessaire pour énumérer les appareils de votre choisis.

Vous pouvez utiliser la **GetDeviceSelector** méthode pour le type d’appareil que vous recherchez pour obtenir le sélecteur d’appareils pour ce type. Par exemple, à l’aide de [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) vous fournira un sélecteur pour énumérer toutes les [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) attachés au système, y compris les imprimantes USB, réseau et Bluetooth POS.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Le **GetDeviceSelector** sont des méthodes pour les différents types d’appareil :

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

À l’aide un **GetDeviceSelector** méthode qui prend un [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) valeur en tant que paramètre, vous pouvez restreindre votre sélecteur d’énumérer local, réseau, ou périphériques de POS attaché de Bluetooth, ce qui réduit le temps que nécessaire pour la requête soit terminée.  L’exemple ci-dessous montre une utilisation de cette méthode pour définir un sélecteur qui prend en charge uniquement localement attachés imprimantes de PDV.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Consultez [générer un sélecteur d’appareils](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) pour la création plus avancé des chaînes de sélecteur.

## <a name="method-1-use-a-device-picker"></a>Méthode 1 : Utiliser un sélecteur de périphérique

Le [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) classe vous permet d’afficher un menu volant de sélecteur qui contient une liste des appareils pour l’utilisateur de choisir à partir de. Vous pouvez utiliser la [filtre](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) à choisir quels types d’appareil pour l’affichage dans le sélecteur de propriété. Cette propriété est de type [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Vous pouvez ajouter des types d’appareils pour le filtre à l’aide de la [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) ou [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) propriété.

Lorsque vous êtes prêt à afficher le sélecteur d’appareil, vous pouvez appeler la [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) (méthode), qui affiche l’interface utilisateur du sélecteur et retourner l’appareil sélectionné. Vous devez spécifier un [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) qui détermine où le menu volant s’affiche. Cette méthode retournera un [élément DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) de l’objet, donc pour l’utiliser avec le Point de Service API, vous devez utiliser le **FromIdAsync** méthode pour la classe de périphérique particulier que vous souhaitez. Vous passez le [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) propriété en tant que la méthode *deviceId* paramètre et obtenez une instance de la classe de périphérique en tant que la valeur de retour.

L’extrait de code suivant crée un **DevicePicker**, ajoute un filtre de scanneur de codes-barres, a à l’utilisateur de choisir un appareil, puis crée un **BarcodeScanner** objet basé sur l’ID d’appareil :

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

## <a name="method-2-get-first-available-device"></a>Méthode 2 : Obtenir le premier périphérique disponible

Pour obtenir un appareil de Point de Service le plus simple consiste à utiliser **GetDefaultAsync** pour obtenir le premier périphérique disponible au sein d’une classe de périphérique de Point de Service. 

L’exemple ci-dessous illustre l’utilisation de [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) pour [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). Le modèle de codage est similaire pour toutes les classes de périphérique de Point de Service.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** doit être utilisée avec précaution car elle peut retourner un autre appareil à partir d’une session à l’autre. De nombreux événements peuvent influencer cette énumération pour donner un résultat de premier périphérique disponible différent, notamment : 
> - Modification des caméras connectées à votre ordinateur 
> - Modifier sur les lieux de Service des appareils connectés à votre ordinateur
> - Modifier dans les périphériques connectés au réseau Point of Service disponibles sur votre réseau
> - Modifier dans les appareils de Point de Service Bluetooth à portée de votre ordinateur 
> - Modifications apportées à la configuration du Point de Service 
> - Installation des pilotes ou des objets de service OPOS
> - Installation des extensions de Point de Service
> - Mise à jour du système d’exploitation Windows

## <a name="method-3-snapshot-of-devices"></a>Méthode 3 : Capture instantanée d’appareils

Dans ceratins scénarios, vous souhaiterez créer votre propre interface utilisateur ou devrez énumérer les appareils sans afficher d’interface à l’utilisateur.  Dans ces situations, vous pouvez énumérer une capture instantanée d’appareils qui sont actuellement connecté ou associé avec le système à l’aide [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Cette méthode retient les résultats jusqu'à ce que l’énumération soit entièrement terminée.

> [!TIP]
> Il est recommandé d’utiliser le **GetDeviceSelector** méthode avec le **PosConnectionTypes** paramètre lors de l’utilisation **FindAllAsync** pour limiter votre requête pour le type de connexion vous le souhaitez.  Connexions réseau et Bluetooth peuvent retarder les résultats comme leurs énumérations doivent s’achever avant **FindAllAsync** résultats sont retournés.

> [!CAUTION] 
> **FindAllAsync** retourne un tableau des périphériques.  L’ordre de ce tableau peut changer d’une session à l'autre, par conséquent, il est déconseillé de s'appuyer sur un ordre spécifique en utilisant un index codé en dur dans le tableau.  Utilisez [élément DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) propriétés à filtrer vos résultats ou de fournir une interface utilisateur pour l’utilisateur de choisir à partir de.

Cet exemple utilise le sélecteur défini ci-dessus pour prendre un instantané de périphériques à l’aide de **FindAllAsync** énumère au sein de chacun des éléments renvoyés par la collection, puis écrit le nom de l’appareil et l’ID de la sortie de débogage. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Lorsque vous travaillez avec le [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API, vous devez fréquemment utiliser [élément DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) objets à obtenir des informations sur un appareil spécifique. Par exemple, le [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) propriété peut être utilisée pour récupérer et de réutiliser le même appareil s’il est disponible dans une session ultérieure et le [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) propriété peut être utilisée pour l’affichage à des fins dans votre application.  Consultez le [élément DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) page de référence pour plus d’informations sur les propriétés supplémentaires disponibles.

## <a name="method-4-enumerate-and-watch"></a>Méthode 4 : Énumérer et regardez

Une méthode plus puissante et plus flexible d’énumération des appareils consiste à créer un [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  L'observateur de périphériques énumère des périphériques de façon dynamique afin que l'application reçoive des notifications si des périphériques sont ajoutés, supprimés ou modifiés à la fin de l'énumération initiale.  Un **DeviceWatcher** vous permet de détecter lorsqu’un réseau connecté appareil est mis en ligne, un appareil Bluetooth est à portée, ainsi que si un périphérique connecté en local est débranché afin que vous puissiez prendre les mesures appropriées dans votre application.

Cet exemple utilise le sélecteur défini ci-dessus pour créer un **DeviceWatcher** également comme définit des gestionnaires d’événements pour le [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [supprimé](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed), et [mis à jour ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) notifications. Vous devez indiquer les détails des actions que vous souhaitez effectuer lors de chaque notification.

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
> Consultez [appareils Enumerate et espion]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) pour plus d’informations sur l’utilisation d’un **DeviceWatcher**.

## <a name="see-also"></a>Voir également
* [Mise en route avec le Point de Service](pos-basics.md)
* [Classe de l’élément DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Classe de PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes Enum](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Classe de BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Classe de DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]