---
author: msatranjr
title: Bluetooth GATT serveur
description: Cet article fournit une vue d’ensemble du serveur de profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code pour les scénarios d’utilisation courants.
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b8a941b7b80bd5d34e88798ec586d9c1d52e2887
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5926321"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT serveur


**API importantes**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


Cet article montre attribut générique (GATT Bluetooth) serveur API pour applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code pour les tâches courantes de serveur GATT: 
- Définir les services pris en charge
- Publier le serveur afin qu’il peut être détecté par des clients distants
- Publier prise en charge pour le service
- Répondre pour lire et écrire des demandes
- Envoyer des notifications aux clients abonnés

## <a name="overview"></a>Présentation
Windows fonctionne généralement sur le rôle du client. Néanmoins, nombreux scénarios font leur apparition qui nécessitent Windows pour agir en tant qu’également un serveur GATT de Bluetooth LE. Presque tous les scénarios pour les appareils IoT, ainsi que la plupart des communications de BLE inter-plateforme nécessite Windows pour être un serveur GATT. En outre, l’envoi de notifications à des appareils portable à proximité est devenu un scénario courant qui requiert également cette technologie.  
> Assurez-vous que tous les concepts décrits dans la [documentation du Client GATT](gatt-client.md) sont clairs avant de poursuivre.  

Fonctionnement du serveur sera sont axés sur le fournisseur de services et le GattLocalCharacteristic. Ces deux classes fournit les fonctionnalités nécessaires pour déclarer, mettre en œuvre et exposer une hiérarchie de données à un appareil distant.

## <a name="define-the-supported-services"></a>Définir les services pris en charge
Votre application peut déclarer un ou plusieurs services qui seront publiées par Windows. Chaque service est identifiée par un UUID. 

### <a name="attributes-and-uuids"></a>Attributs et UUID
Chaque service, la caractéristique et le descripteur sont défini par elle est propre UUID de 128 bits unique.
> Toutes les API Windows utilisent le terme GUID, mais la norme Bluetooth définit ces derniers comme UUID. Dans notre cas, ces deux termes sont interchangeables afin que nous continuerons à utiliser le terme UUID. 

Si l’attribut est standard ou définies par défini par le Bluetooth SIG, elle doit également contenir un code court de 16 bits correspondante (par exemple, UUID de niveau de batterie est 0000**2A19**-0000-1000-8000-00805F9B34FB et l’ID court est 0x2A19). Ces UUID standard est visible dans [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx) et [GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx).

Si votre application est mise en œuvre c’est propre service personnalisé, un UUID personnalisé devra être générés. Il s’agit facilement dans Visual Studio par le biais d’outils -> CreateGuid (utilisez option 5 accéder, au format «xxxxxxxx-xxxx-… xxxx»). L’uuid peut maintenant être utilisé pour déclarer descripteurs, de caractéristiques ou de nouveaux services locales.

#### <a name="restricted-services"></a>Services restreintes
Les Services suivants sont réservés par le système et ne peut pas être publiées à ce stade:
1. Service d’informations de périphérique (DIS)
2. Service de profil d’attribut générique (GATT)
3. Service de profil d’accès générique (écart)
4. Service de périphériques d’Interface utilisateur (HOGP)
5. Analyser les paramètres Service (SCP)

> Toute tentative de création d’un service bloqué entraîne BluetoothError.DisabledByPolicy renvoyés à partir de l’appel à CreateAsync.

#### <a name="generated-attributes"></a>Attributs générés
Les descripteurs suivants sont générés automatiquement par le système, en fonction de la GattLocalCharacteristicParameters fourni lors de la création de la caractéristique:
1. Configuration de la caractéristique client (si la caractéristique est marquée comme indicatable ou déclaration obligatoire).
2. Caractéristique Description de l’utilisateur (si la valeur de propriété UserDescription). Consultez la propriété GattLocalCharacteristicParameters.UserDescription pour plus d’informations.
3. Caractéristique Format (un descripteur pour chaque format de présentation spécifié).  Consultez la propriété GattLocalCharacteristicParameters.PresentationFormats pour plus d’informations.
4. Format agrégées caractéristique (si plusieurs formats de présentation sont défini).  Propriété GattLocalCharacteristicParameters.See PresentationFormats pour plus d’informations.
5. Propriétés étendues caractéristique (si la caractéristique est marquée avec le bit de propriétés étendues).

> La valeur du descripteur de propriétés étendues est déterminée via les propriétés de caractéristique ReliableWrites et WritableAuxiliaries.

> Vous tentez de créer un descripteur réservé entraîne une exception.

> Notez que la diffusion n’est pas pris en charge pour l’instant.  En spécifiant la diffusion GattCharacteristicProperty entraîne une exception.

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>Génère la hiérarchie de services et les caractéristiques
Le GattServiceProvider est utilisé pour créer et publier la définition du service principal racine.  Chaque service requiert que c’est propre objet fournisseur qui accepte un GUID: 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Les services principaux sont le niveau supérieur de l’arborescence GATT. Les services principaux contient caractéristiques, ainsi que les autres services (appelées «Inclus» ou services secondaires). 

À présent, remplir le service avec les caractéristiques requises et les descripteurs:

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
Comme indiqué ci-dessus, c’est également un bon endroit où déclarer les gestionnaires d’événements pour les opérations de que chaque caractéristique prend en charge.  Pour répondre aux requêtes correctement, une application doit définis et de définir un gestionnaire d’événements pour chaque type de requête prend en charge de l’attribut.  Si vous enregistrez un gestionnaire ne se traduit par la demande en cours effectuée immédiatement avec *UnlikelyError* par le système.

### <a name="constant-characteristics"></a>Caractéristiques constantes
Parfois, il existe des valeurs caractéristiques qui ne changera pas au cours de la durée de vie de l’application. Dans ce cas, il est recommandé de déclarer une caractéristique constante pour empêcher l’activation d’une application inutiles: 

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
Une fois que le service a été entièrement défini, l’étape suivante consiste à publier la prise en charge pour le service. Cela informe le système d’exploitation que le service doit être retourné lorsque les appareils distants effectuent une découverte de service.  Vous devez définir deux propriétés - IsDiscoverable et IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: publie le nom convivial à des appareils distants dans la publicité, rendant l’appareil détectable.
- **IsConnectable**: publie une annonce pouvant être connectée à utiliser dans le rôle périphérique.

> Lorsqu’un service est à la fois détectable et Connectable, le système ajoutera l’Uuid du Service au paquet annonce.  Il existe uniquement 31 octets dans le paquet annonce et un UUID de 128 bits occupe 16 d'entre eux!

> Notez que lorsqu’un service est publié au premier plan, une application doit appeler StopAdvertising lorsque l’application interrompt l’exécution.

## <a name="respond-to-read-and-write-requests"></a>Répondre pour lire et écrire des demandes
Comme nous l’avons vu tandis que la déclaration les caractéristiques requises, GattLocalCharacteristics avoir 3 types d’événements - ReadRequested, WriteRequested et SubscribedClientsChanged.

### <a name="read"></a>Read
Lorsqu’un appareil distant tente de lire une valeur à partir d’une caractéristique (et il n’est pas une valeur constante), l’événement ReadRequested est appelée. La caractéristique la lecture a été appelée sur, ainsi que les arguments (contenant des informations sur l’appareil distant) est passée au délégué: 

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
Lorsqu’un appareil distant tente d’écrire une valeur dans une caractéristique, l’événement WriteRequested est appelé avec plus d’informations sur l’appareil distant, les caractéristiques d’écrire sur et la valeur elle-même: 

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
Il existe 2 types d’écritures - avec et sans réponse. GattWriteOption (il s’agit d’une propriété sur l’objet GattWriteRequest) permet de déterminer quel type d’écriture effectue l’appareil distant. 

## <a name="send-notifications-to-subscribed-clients"></a>Envoyer des notifications aux clients abonnés
La plus fréquente des opérations serveur GATT, notifications effectuer la fonction essentielle de transmettre des données aux appareils distants. Parfois, vous devez avertir tous les clients auxquels vous êtes abonnés, mais à d’autres moment que vous voudrez peut-être sélectionner les périphériques à envoyer à la nouvelle valeur: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Lorsqu’un nouvel appareil s’abonne aux notifications, l’événement SubscribedClientsChanged est appelée: 

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
> Notez qu’une application peut obtenir la taille maximale de notification pour un client particulier avec la propriété MaxNotificationSize.  Toutes les données plus grandes que la taille maximale seront tronquées par le système.
