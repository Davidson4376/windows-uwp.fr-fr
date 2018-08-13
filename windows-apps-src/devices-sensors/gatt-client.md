---
author: msatranjr
title: Bluetooth GATT Client
description: Cet article fournit une vue d’ensemble du profil d’attribut générique (GATT) Bluetooth Client pour les applications universels Windows plateforme (UWP), ainsi que des exemples de code pour les scénarios d’utilisation courants.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 555fec6d534c07898acd911f9cd84a11ac66dcd8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "305108"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT Client


**API importantes**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Cet article illustre l’utilisation des API pour les applications universels Windows plateforme (UWP), ainsi que des exemples de code pour les tâches courantes de client GATT Client Bluetooth générique attribut (GATT):
- Requête de périphériques proches
- Se connecter au périphérique
- Énumérer les services pris en charge et les caractéristiques du périphérique
- Lire et écrire dans une caractéristique
- S’abonner pour les notifications lorsque caractéristique valeur est modifiée

## <a name="overview"></a>Vue d’ensemble
Les développeurs peuvent utiliser les API dans l’espace de noms [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) pour accéder aux périphériques LE Bluetooth. Les appareils Bluetooth LE exposent leurs fonctionnalités par le biais d’une collection de :

-   Services
-   caractéristiques;
-   descripteurs.

Services définissent le contrat de l’appareil LE fonctionnel et contient une collection de caractéristiques qui définissent le service. Ces caractéristiques, à leur tour, contiennent des descripteurs qui décrivent les caractéristiques. Ces 3 termes appelées générique les attributs d’un périphérique.

Les API de GATT LE Bluetooth exposer des objets et des fonctions, plutôt que d’accéder à transport brut. Les API GATT permettent également aux développeurs de travailler avec les périphériques Bluetooth LE avec la possibilité d’effectuer les tâches suivantes:

-   Effectuer la découverte des attributs
-   Lecture et écriture les valeurs d’attribut
-   Enregistrer un rappel pour un événement ValueChanged caractéristique

Pour créer une implémentation utile un développeur doit avoir connaissance préalable GATT services et les caractéristiques de que l’application a l’intention de consommer et processus la caractéristique spécifique de valeurs telles que les données binaires fournies par l’API sont transformées en données utiles avant d’être présenté à l’utilisateur. Les API GATT Bluetooth exposent uniquement les primitives de base nécessaires pour communiquer avec un périphérique Bluetooth LE. Pour interpréter les données, un profil d’application doit être défini, soit par un profil Bluetooth SIG standard, soit par un profil personnalisé implémenté par un fournisseur de périphérique. Un profil crée un contrat de liaison entre l’application et le périphérique, qui définit ce que représentent les données échangées et la façon de les interpréter.

Pour des raisons pratiques, le Bluetooth SIG gère une [liste de profils publics](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) disponibles.

## <a name="query-for-nearby-devices"></a>Requête de périphériques proches
Il existe deux méthodes principales pour interroger des périphériques proches:
- DeviceWatcher dans Windows.Devices.Enumeration
- AdvertisementWatcher dans Windows.Devices.Bluetooth.Advertisement

La méthode 2ème est décrite en détail dans la documentation de [publication](ble-beacon.md) afin qu’il ne seront pas traité quantité ici, mais l’idée de base consiste à trouver l’adresse Bluetooth de périphériques proches qui répondent à la [Publication filtre](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)particulier. Une fois que vous avez l’adresse, vous pouvez appeler [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) pour obtenir une référence à l’appareil. 

Maintenant, la méthode DeviceWatcher. Un appareil Bluetooth LE est identique à un autre périphérique dans Windows et peut être interrogé à l’aide de l' [API de l’énumération](https://msdn.microsoft.com/library/windows/apps/BR225459). Utilisez la classe [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) et transmettre une chaîne de requête spécifiant les périphériques à rechercher: 

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
Une fois que vous avez démarré le DeviceWatcher, vous recevrez [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) pour chaque périphérique correspondant aux critères de la requête dans le gestionnaire pour l’événement [ajoutée](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) pour les périphériques en question. Pour un aperçu plus détaillé de DeviceWatcher voir complet exemple de [référentiels](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Connexion à l’appareil
Une fois qu’un périphérique souhaité est découvert, utilisez le [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) pour obtenir l’objet Bluetooth LE périphérique pour le périphérique en question: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
En revanche, la suppression de toutes les références à un BluetoothLEDevice objet pour un périphérique (et si aucune autre application sur le système n’a une référence à l’appareil) déclenchera automatique se déconnecter après un délai de petite taille. 

```csharp
bluetoothLeDevice.Dispose();
```
Si l’application a besoin d’accéder au périphérique à nouveau, simplement la recréation de l’objet de périphérique et l’accès à une caractéristique (abordée dans la section suivante) déclenche le système d’exploitation pour vous reconnecter lorsque cela est nécessaire. Si le périphérique est à proximité, vous obtiendrez l’accès au périphérique dans le cas contraire, qu'il renverra avec une erreur DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>L’énumération des services pris en charge et les caractéristiques
Maintenant que vous disposez d’un objet BluetoothLEDevice, l’étape suivante consiste à découvrir les données qui expose l’appareil. Pour ce faire la première étape consiste à demander des services: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Une fois le service d’intérêt a été identifié, l’étape suivante consiste à demander des caractéristiques. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
Le système d’exploitation renvoie qu'une liste en lecture seule de GattCharacteristic que vous pouvez ensuite effectuer des opérations sur les objets.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Effectuer des opérations de lecture/écriture sur une caractéristique

La caractéristique repose sur l’unité de base du GATT de communication. Il contient une valeur qui représente un élément distinct de données sur l’appareil. Par exemple, les caractéristiques de niveau batterie a une valeur qui représente le niveau de la batterie de l’appareil.

Lire les propriétés caractéristiques pour déterminer quelles sont les opérations prises en charge:
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

Si la lecture est prise en charge, vous pouvez lire la valeur: 
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
Écrire dans une caractéristique suit un schéma similaire: 
```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other commmon functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattReadResult result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result.Status == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **Conseil**: Entraînez-vous à l’aide de [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) et [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx). Leurs fonctionnalités seront indispensables lorsque vous travaillez avec les tampons brutes que vous obtenir à partir de nombreuses APIs Bluetooth. 
## <a name="subscribing-for-notifications"></a>Abonnement à des notifications

Assurez-vous que la caractéristique prend en charge indiquer ou avertir (cochez les propriétés caractéristiques pour vous assurer que). 

> **Réserver**: indiquer est considéré comme plus fiable, car chaque événement valeur modifié est associée à un accusé de réception de l’appareil client. Avertir est plus répandu, car la plupart des transactions GATT seraient plutôt économiser de l’énergie au lieu d’être très fiable. Dans tous les cas, tout qui est géré au niveau du contrôleur afin que l’application ne pas obtenir impliquée. Nous allons collectivement faire référence à des simplement «notifications», mais vous savez maintenant. 

Il existe deux choses sont à prendre en charge avant de récupérer des notifications:
- Écrire dans le descripteur de caractéristiques de Configuration de Client (CCCD)
- Gère l’événement Characteristic.ValueChanged

Écrire dans le CCCD indique l’unité du serveur que ce client souhaite savoir chaque fois que la valeur caractéristique particulier change. Pour cela, procédez comme suit: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
À présent, l’événement ValueChanged de la GattCharacteristic est appelé chaque fois que la valeur est modifiée sur le périphérique distant. Il nous reste consiste à implémenter le gestionnaire: 

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


