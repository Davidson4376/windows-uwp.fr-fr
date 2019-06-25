---
title: Client GATT Bluetooth
description: Cet article fournit une vue d’ensemble du client de profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant les cas d’utilisation courants.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9cc74b1c150e3103e62ec7c7af99608908cf0451
ms.sourcegitcommit: f7e3782e24d46b2043023835c5b59d12d3b4ed4b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67345680"
---
# <a name="bluetooth-gatt-client"></a>Client GATT Bluetooth


**API importantes**

-   [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

Cet article présente l’utilisation des API du client de profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant les tâches de client GATT courantes :
- Recherche des appareils à proximité
- Connexion à l’appareil
- Énumération des services et caractéristiques de l’appareil pris en charge
- Exécution d’opérations de lecture et d’écriture sur une caractéristique
- Abonnement aux notifications signalant les modifications de valeur de caractéristique

## <a name="overview"></a>Vue d'ensemble
Les développeurs peuvent utiliser les API de l’espace de noms [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) pour accéder aux appareils Bluetooth Low Energy (LE). Les appareils Bluetooth LE exposent leurs fonctionnalités par le biais d’une collection de :

-   Services
-   caractéristiques ;
-   descripteurs.

Les services définissent le contrat fonctionnel de l’appareil LE et contiennent une collection de caractéristiques qui définissent le service. Ces caractéristiques, à leur tour, contiennent des descripteurs qui décrivent les caractéristiques. Ces 3 composants sont désignés de manière générique sous le terme d’attributs d’un appareil.

Les API GATT Bluetooth LE exposent des objets et des fonctions, plutôt que d’accéder simplement au transport. En outre, les API GATT permettent aux développeurs d’utiliser des appareils Bluetooth LE en vue d’effectuer les tâches suivantes :

-   Procéder à la détection des attributs
-   Lire et écrire les valeurs d’attribut
-   Inscrire un rappel pour l’événement de modification de la valeur d’une caractéristique

Pour créer une implémentation utile, un développeur doit connaître au préalable les services et caractéristiques GATT que l’application est susceptible d’utiliser et traiter les valeurs spécifiques des caractéristiques de façon que les données binaires fournies par l’API soient transformées en données utiles avant d’être présentées à l’utilisateur. Les API GATT Bluetooth exposent uniquement les primitives de base nécessaires pour communiquer avec un périphérique Bluetooth LE. Pour interpréter les données, un profil d’application doit être défini, soit par un profil Bluetooth SIG standard, soit par un profil personnalisé implémenté par un fournisseur de périphérique. Un profil crée un contrat de liaison entre l’application et le périphérique, qui définit ce que représentent les données échangées et la façon de les interpréter.

Pour des raisons pratiques, le Bluetooth SIG gère une [liste de profils publics](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) disponibles.

## <a name="query-for-nearby-devices"></a>Recherche des appareils à proximité
Deux méthodes principales vous permettent de rechercher les appareils à proximité :
- DeviceWatcher dans Windows.Devices.Enumeration
- AdvertisementWatcher dans Windows.Devices.Bluetooth.Advertisement

La seconde méthode est décrite en détail dans la documentation concernant les [annonces](ble-beacon.md) et ne sera donc pas abordée en profondeur dans cet article. Toutefois, l’idée de base est de trouver l’adresse Bluetooth des appareils à proximité qui satisfont aux critères du [filtre AdvertisementFilter](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter) spécifiquement utilisé. Une fois que vous disposez de l’adresse, vous pouvez appeler [BluetoothLEDevice.FromBluetoothAddressAsync](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync) pour obtenir une référence à l’appareil. 

À présent, revenons à la méthode DeviceWatcher. Un appareil Bluetooth LE est comparable à tout autre appareil dans Windows et peut être recherché à l’aide des [API Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration). Utilisez la classe [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) et transmettez une chaîne de recherche spécifiant les appareils à rechercher : 

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```
Après avoir démarré DeviceWatcher, vous recevrez l’objet [DeviceInformation](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) pour chaque appareil correspondant aux termes de la requête dans le gestionnaire de l’événement [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added) pour les appareils en question. Pour plus d’informations sur la classe DeviceWatcher, consultez l’exemple d’utilisation complet [sur Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Connexion à l’appareil
Une fois qu’un appareil recherché est détecté, utilisez [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) pour obtenir l’objet d’appareil Bluetooth LE (BluetoothLEDevice) de l’appareil en question : 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
Inversement, la suppression de toutes les références à l’objet BluetoothLEDevice d’un appareil (dans le cas où aucune autre application du système ne comporte de référence à cet appareil) entraînera une déconnexion automatique de l’appareil après un bref délai d’expiration. 

```csharp
bluetoothLeDevice.Dispose();
```
Si l’application a besoin de réaccéder à l’appareil, le simple fait de recréer l’objet de l’appareil et d’accéder à une caractéristique (décrite dans la section suivante) déclenchera la reconnexion à l’appareil par le système d’exploitation au moment opportun. Si l’appareil se trouve à proximité, vous obtiendrez l’accès à l’appareil ; dans le cas contraire, le système renverra une erreur DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>Énumération des services et caractéristiques pris en charge
À présent que vous disposez d’un objet BluetoothLEDevice, l’étape suivante consiste à découvrir les données exposées par l’appareil. Pour ce faire, vous devez commencer par rechercher les services : 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Une fois que le service qui vous intéresse a été identifié, l’étape suivante consiste à rechercher les caractéristiques. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
Le système d’exploitation renvoie une liste en lecture seule d’objets GattCharacteristic sur lesquels vous pourrez ensuite effectuer des opérations.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Exécution d’opérations de lecture et d’écriture sur une caractéristique

La caractéristique est l’unité de base des communications reposant sur GATT. Elle contient une valeur représentant un élément de données spécifique sur l’appareil. Par exemple, la caractéristique de niveau de batterie présente une valeur représentant le niveau de batterie de l’appareil.

Lisez les propriétés de caractéristique pour déterminer les opérations prises en charge :
```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

Si la lecture est prise en charge, vous pouvez lire la valeur : 
```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```
L’écriture dans une caractéristique suit un modèle similaire : 
```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **Conseil**: Familiarisez-vous avec les à l’aide de [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) et [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter). Leurs fonctionnalités vous seront indispensables lorsque vous utiliserez les mémoires tampons brutes que vous obtenez de la plupart des API Bluetooth. 
## <a name="subscribing-for-notifications"></a>Abonnement aux notifications

Assurez-vous que la caractéristique prend en charge la valeur Indicate ou Notify (pour ce faire, vérifiez les propriétés de caractéristique). 

> **Côté**: Indiquer est considéré comme plus fiable, car chaque événement de la valeur modifiée est couplé avec un accusé de réception à partir de l’appareil client. La valeur Notify est plus répandue, car la plupart des transactions GATT sont davantage centrées sur l’économie d’énergie que sur la fiabilité. Dans les deux cas, l’ensemble du processus est géré au niveau de la couche du contrôleur afin que l’application ne soit pas impliquée. Nous désignons ces deux approches sous le simple terme de « notifications », mais vous connaissez désormais la différence entre les deux. 

Il existe deux opérations à effectuer avant d’obtenir des notifications :
- Écrire dans le descripteur de configuration de la caractéristique du client (CCCD)
- Gérer l’événement Characteristic.ValueChanged

L’écriture dans le CCCD indique au serveur que ce client souhaite être informé chaque fois que cette valeur de caractéristique spécifique change. Pour ce faire : 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
À présent, l’événement ValueChanged de GattCharacteristic sera appelé chaque fois que la valeur sera modifiée sur l’appareil distant. La seule opération restant à effectuer consiste à implémenter le gestionnaire à l’aide du code suivant : 

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;
// ... 

void Characteristic_ValueChanged(GattCharacteristic sender, 
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```


