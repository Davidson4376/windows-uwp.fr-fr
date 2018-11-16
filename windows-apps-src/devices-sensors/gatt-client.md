---
author: msatranjr
title: Bluetooth GATT Client
description: Cet article fournit une vue d’ensemble du Client de profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code pour les scénarios d’utilisation courants.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 345e6f82ddf97c2595dad0029ca432f075a6190b
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6990902"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT Client


**API importantes**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Cet article montre l’utilisation de l’API de Client d’attribut générique (GATT Bluetooth) pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code pour les tâches courantes de client GATT:
- Requête pour les appareils à proximité
- Se connecter à l’appareil
- Énumérer les services pris en charge et les caractéristiques de l’appareil
- Lire et écrire dans une caractéristique
- S’abonner pour les modifications de valeur de notifications lors de la caractéristique

## <a name="overview"></a>Vue d'ensemble
Les développeurs peuvent utiliser les API dans l’espace de noms [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) pour accéder aux périphériques Bluetooth LE. Les appareils Bluetooth LE exposent leurs fonctionnalités par le biais d’une collection de :

-   Services
-   caractéristiques;
-   descripteurs.

Services définissent le contrat fonctionnel du périphérique LE et contiennent une collection de caractéristiques qui définissent le service. Ces caractéristiques, à leur tour, contiennent des descripteurs qui décrivent les caractéristiques. Ce termes du contrat de 3 est appelé les attributs d’un appareil.

API GATT Bluetooth Bluetooth LE exposent des objets et des fonctions, plutôt qu’accéder au transport. Les API GATT permettent également aux développeurs d’utiliser des périphériques Bluetooth LE avec la possibilité d’effectuer les tâches suivantes:

-   Effectuer la découverte des attributs
-   Lire et écrire les valeurs d’attribut
-   Inscrire un rappel pour l’événement ValueChanged de caractéristique

Pour créer une implémentation utile à un développeur doit avoir une connaissance préalable des services GATT et caractéristiques de que l’application a l’intention d’utiliser et traiter les valeurs spécifiques des caractéristiques telles que les données binaires fournies par l’API soient transformées en données utiles avant d’être présenté à l’utilisateur. Les API GATT Bluetooth exposent uniquement les primitives de base nécessaires pour communiquer avec un périphérique Bluetooth LE. Pour interpréter les données, un profil d’application doit être défini, soit par un profil Bluetooth SIG standard, soit par un profil personnalisé implémenté par un fournisseur de périphérique. Un profil crée un contrat de liaison entre l’application et le périphérique, qui définit ce que représentent les données échangées et la façon de les interpréter.

Pour des raisons pratiques, le Bluetooth SIG gère une [liste de profils publics](https://www.bluetooth.com/specifications/adopted-specifications#gattspec) disponibles.

## <a name="query-for-nearby-devices"></a>Requête pour les appareils à proximité
Il existe deux méthodes principales d’interroger les appareils à proximité:
- DeviceWatcher dans Windows.Devices.Enumeration
- Utilisation de Windows.Devices.Bluetooth.Advertisement

La méthode 2e est décrite en détail dans la documentation de [l’annonce](ble-beacon.md) afin qu’il ne seront pas traité quantité ici, mais l’idée de base consiste à trouver l’adresse Bluetooth des appareils à proximité qui correspondent au particulier [Filtre d’annonce](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx). Une fois que vous avez l’adresse, vous pouvez appeler [BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx) pour obtenir une référence à l’appareil. 

Maintenant, à la méthode DeviceWatcher. Un périphérique Bluetooth LE comme n’importe quel autre appareil dans Windows et peut être interrogé à l’aide de l' [API d’énumération](https://msdn.microsoft.com/library/windows/apps/BR225459). Utilisez la classe [DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher) et transmettre une chaîne de requête en spécifiant les appareils à rechercher: 

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
Une fois que vous avez démarré le DeviceWatcher, vous recevrez [DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393) pour chaque appareil qui répond à la requête dans le gestionnaire pour l’événement [ajoutée](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added) pour les appareils en question. Pour une présentation plus détaillée de DeviceWatcher reportez-vous à la fin de l’exemple [sur Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing). 

## <a name="connecting-to-the-device"></a>Connexion à l’appareil
Une fois qu’un périphérique souhaité est détecté, utilisez le [DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id) pour obtenir l’objet de périphérique Bluetooth LE pour l’appareil en question: 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
En revanche, de toutes les références à un BluetoothLEDevice supprimer l’objet d’un appareil (et si aucune autre application sur le système n’a une référence à l’appareil) déclenchera automatique déconnecter après un délai d’expiration petite. 

```csharp
bluetoothLeDevice.Dispose();
```
Si l’application a besoin d’accéder à l’appareil à nouveau, il vous suffit de recréer l’objet de périphérique et de l’accès à une caractéristique (décrite dans la section suivante) déclenche le système d’exploitation pour vous reconnecter lorsque cela est nécessaire. Si l’appareil se trouve à proximité, vous obtiendrez l’accès à l’appareil dans le cas contraire, qu'elle renvoie avec une erreur DeviceUnreachable.  

## <a name="enumerating-supported-services-and-characteristics"></a>L’énumération des services pris en charge et caractéristiques
Maintenant que vous avez un objet BluetoothLEDevice, l’étape suivante consiste à découvrir quelles données expose l’appareil. La première étape pour effectuer cette opération consiste à rechercher les services: 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
Une fois que le service d’intérêt a été identifié, l’étape suivante consiste à rechercher les caractéristiques. 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
Le système d’exploitation renvoie qu'une liste en lecture seule de GattCharacteristic que vous pouvez ensuite effectuer des opérations sur des objets.

## <a name="perform-readwrite-operations-on-a-characteristic"></a>Effectuer des opérations de lecture/écriture sur une caractéristique

La caractéristique repose sur l’unité fondamentale de GATT communication. Il contient une valeur qui représente un élément distinct de données sur l’appareil. Par exemple, la caractéristique de niveau de batterie a une valeur qui représente le niveau de batterie de l’appareil.

Lire les propriétés caractéristiques pour déterminer quelles sont les opérations sont prises en charge:
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
Écriture d’une caractéristique suit un modèle semblable: 
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
> **Conseil**: obtenir à l’aise avec à l’aide de [DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx) et [DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx). Leur fonctionnalité sera indispensable lorsque vous travaillez avec les tampons bruts, que vous obtenez de nombreuses APIs Bluetooth. 
## <a name="subscribing-for-notifications"></a>Abonnement pour les notifications

Assurez-vous que la caractéristique prend en charge Indiquez ou avertir (Vérifiez les propriétés caractéristiques pour vous assurer que). 

> **Mettre de côté**: indiquer est considéré comme plus fiable, car chaque événement valeur modifiée est associée à un accusé de réception à partir de l’appareil du client. Avertir n’est plus répandus, car la plupart des transactions GATT seraient plutôt économiser l’énergie au lieu d’être extrêmement fiable. Dans tous les cas, tout cela est gérée sur la couche du contrôleur afin que l’application ne pas obtenir impliquée. Nous désignons collectivement sur ces derniers en tant que de simplement «notifications», mais vous savez désormais. 

Il existe deux éléments à prendre en charge avant d’obtenir des notifications:
- Écrire dans le descripteur de caractéristiques de Configuration de Client (CCCD)
- Gérer l’événement Characteristic.ValueChanged

Écriture de la CCCD indique à l’appareil de serveur que ce client souhaite connaître chaque fois que ces changements de valeur de caractéristique particulier. Pour cela, procédez comme suit: 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
À présent, l’événement ValueChanged de la GattCharacteristic est appelé chaque fois que la valeur est modifiée sur l’appareil distant. Il reste consiste à implémenter le Gestionnaire d’événements: 

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


