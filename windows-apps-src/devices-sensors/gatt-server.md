---
author: msatranjr
title: Bluetooth GATT Server
description: Cet article fournit une vue d’ensemble du profil d’attribut générique (GATT) Bluetooth serveur pour les applications universels Windows plateforme (UWP), ainsi que des exemples de code pour les scénarios d’utilisation courants.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 27154fbb535b76995fba97702e65a9c0b2a8291c
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "610768"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT Server


**API importantes**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


Cet article décrit les API de serveur Bluetooth générique attribut (GATT) pour les applications universels Windows plateforme (UWP), ainsi que des exemples de code pour les tâches courantes de serveur GATT: 
- Définir les services pris en charge
- Publier le serveur afin qu’il peut être découvert par des clients distants
- Publier la prise en charge pour le service
- Répondre pour lire et écrire des demandes
- Envoyer des notifications aux clients abonnés

## <a name="overview"></a>Vue d’ensemble
Windows fonctionne généralement dans le rôle du client. Toutefois, nombreux scénarios surviennent qui nécessitent Windows agir comme un serveur Bluetooth LE GATT ainsi. Presque tous les scénarios pour les appareils IoT, ainsi que la plupart des communications de BLE multiplateforme nécessite Windows à un serveur GATT. En outre, l’envoi de notifications pour les périphériques portable proches est devenu un scénario populaires qui requiert également cette technologie.  
> Assurez-vous que tous les concepts décrits dans les [documents Client GATT](gatt-client.md) sont désactivées avant de continuer.  

Opérations Server concernent le fournisseur de services et le GattLocalCharacteristic. Ces deux classes fournira la fonctionnalité permettant de déclarer, de mettre en œuvre et d’exposer une hiérarchie de données à un périphérique distant.

## <a name="define-the-supported-services"></a>Définir les services pris en charge
Votre application peut déclarer un ou plusieurs services qui sont publiées par Windows. Chaque service est identifiée par un UUID. 

### <a name="attributes-and-uuids"></a>Attributs et UUID
Chaque service, caractéristique et le descripteur sont défini par son est propre UUID de 128 bits unique.
> Toutes les API Windows utilisent le terme GUID, mais la norme Bluetooth définit comme UUID. Dans notre cas, ces deux termes sont interchangeables afin de continuer à utiliser le terme UUID. 

Si l’attribut est standard ou définies par le Bluetooth SIG définis, elle doit également contenir un ID court de 16 bits correspondant (par exemple UUID de niveau de batterie est 0000**2A19**-0000-1000-8000-00805F9B34FB et l’ID court est 0x2A19). Ces UUID standards peuvent être vus dans [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) et [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx).

Si l’implémentation de votre application il est propre service personnalisé, un UUID personnalisé doivent être généré. Il s’agit facilement dans Visual Studio à outils -> CreateGuid (option utiliser 5 pour l’obtenir dans le format «xxxxxxxx-xxxx-... xxxx»). Ce uuid peut désormais être utilisé pour déclarer des descripteurs, les caractéristiques ou les nouveaux services locales.

#### <a name="restricted-services"></a>Services restreints
Les Services suivants sont réservés par le système et ne peut pas être publiées à ce stade:
1. Service informations d’appareil (DIS)
2. Service de profil attribut générique (GATT)
3. Service de profil d’accès générique (GAP)
4. Service de périphériques d’Interface utilisateur (HOGP)
5. Analyse des paramètres Service (SCP)

> Tentative de création d’un service bloqué entraînera BluetoothError.DisabledByPolicy renvoyés à partir de l’appel vers CreateAsync.

#### <a name="generated-attributes"></a>Attributs générés
Les descripteurs suivants sont générés automatiquement par le système, en fonction de la GattLocalCharacteristicParameters fourni lors de la création de la caractéristique:
1. Configuration de caractéristique du client (si la caractéristique est marquée comme indicatable ou déclaration obligatoire).
2. Caractéristique Description d’utilisateur (si la propriété UserDescription est définie). Voir la propriété GattLocalCharacteristicParameters.UserDescription pour plus d’informations.
3. Format caractéristique (un descripteur pour chaque format de présentation spécifié).  Voir la propriété GattLocalCharacteristicParameters.PresentationFormats pour plus d’informations.
4. Format de regroupement caractéristique (si plus d’un format de présentation n’est spécifié).  Propriété GattLocalCharacteristicParameters.See PresentationFormats pour plus d’informations.
5. Propriétés étendues caractéristique (si la caractéristique est marquée avec le bit propriétés étendues).

> La valeur du descripteur de propriétés étendues est déterminée par le biais des propriétés ReliableWrites et WritableAuxiliaries caractéristiques.

> Tentative de création d’un descripteur réservé provoquera une exception.

> Notez que la diffusion n’est pas pris en charge pour l’instant.  Spécification de la diffusion GattCharacteristicProperty entraînera une exception.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>Créer la hiérarchie des services et les caractéristiques
Le GattServiceProvider est utilisé pour créer et publier la définition du service principal racine.  Chaque service nécessite qu’il est propre objet de fournisseur qui utilise un GUID: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Principaux services sont le niveau supérieur de l’arborescence GATT. Principaux services contiennent des caractéristiques ainsi que d’autres services (appelés «Inclus» ou services secondaires). 

À présent, remplissez le service avec les caractéristiques requises et les descripteurs:

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
Comme indiqué ci-dessus, c’est également un endroit idéal pour déclarer des gestionnaires d’événements pour les opérations de que chaque caractéristique prend en charge.  Pour répondre aux demandes correctement, une application doit définie et un gestionnaire d’événements pour chaque type de demande l’attribut prend en charge.  Échec enregistrer un gestionnaire entraînera la demande ne soient effectuée immédiatement avec *UnlikelyError* par le système.

### <a name="constant-characteristics"></a>Caractéristiques de la constantes
Dans certains cas, il existe des valeurs caractéristiques ne changent pas au cours de la durée de vie de l’application. Dans ce cas, il est recommandé de déclarer une constante caractéristique pour empêcher l’activation de l’application inutiles: 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>Publier le service
Une fois que le service a été entièrement défini, l’étape suivante consiste à publier la prise en charge pour le service. Cela informe le système d’exploitation que le service doit être renvoyé lors de périphériques distants effectuer une découverte de service.  Vous devez définir deux propriétés - IsDiscoverable et IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: affiche le nom convivial sur des périphériques à distance de la publication, en rendant le périphérique détectable.
- **IsConnectable**: publie une annonce connectable pour une utilisation dans le rôle de périphérique.

> Lorsqu’un service est détectable et Connectable, le système ajoutera l’Uuid Service pour le paquet de publication.  31 octets uniquement dans le paquet de publication et un UUID de 128 bits prend jusqu'à 16 d'entre eux!

> Notez que lorsqu’un service est publié au premier plan, une application doit appeler StopAdvertising lors de l’application suspend.

## <a name="respond-to-read-and-write-requests"></a>Répondre pour lire et écrire des demandes
Comme nous l’avons vu tandis que la déclaration les caractéristiques requises, GattLocalCharacteristics ont 3 types d’événements - ReadRequested, WriteRequested et SubscribedClientsChanged.

### <a name="read"></a>Read
Lorsqu’un périphérique distant tente de lire une valeur d’une caractéristique (et il n’est pas une valeur constante), l’événement ReadRequested est appelée. Les caractéristiques de que lecture a été appelée sur ainsi qu’args (contenant des informations sur le périphérique distant) sont passé au délégué: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>Écriture
Lorsqu’un périphérique distant tente d’écrire une valeur dans une caractéristique, l’événement WriteRequested est appelée avec plus d’informations sur le périphérique à distance, les caractéristiques d’écrire dans et la valeur elle-même: 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
Il existe 2 types d’écritures - avec et sans réponse. Utilisez GattWriteOption (une propriété sur l’objet GattWriteRequest) pour déterminer le type d’écriture effectue le périphérique distant. 

## <a name="send-notifications-to-subscribed-clients"></a>Envoyer des notifications aux clients abonnés
Les plus fréquentes des opérations de serveur GATT notifications effectuent la fonction d’envoi des données sur les périphériques distants critique. Parfois, vous souhaiterez informer tous les clients abonnés mais à d’autres moment que vous souhaiterez peut-être choisir les périphériques à envoyer à la nouvelle valeur: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Lorsqu’un nouveau périphérique s’abonne pour les notifications, l’événement SubscribedClientsChanged est appelé: 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> Notez qu’une application peut obtenir la taille maximale de notification pour un client particulier avec la propriété MaxNotificationSize.  Toutes les données supérieures à la taille maximale sera tronquée par le système.
