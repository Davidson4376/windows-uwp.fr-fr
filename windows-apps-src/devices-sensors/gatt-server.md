---
title: Serveur GATT Bluetooth
description: Cet article fournit une vue d’ensemble du serveur de profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant les cas d’utilisation courants.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f59ae45486ee72f9d901898f6b03674e6b3e299c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370088"
---
# <a name="bluetooth-gatt-server"></a>Serveur GATT Bluetooth


**API importantes**
- [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)


Cet article présente les API du serveur de profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant les tâches de serveur GATT courantes : 
- Définir les services pris en charge
- Publier le serveur pour qu’il soit détectable par les clients distants
- Publier la prise en charge du service
- Répondre aux demandes de lecture et d’écriture
- Envoyer des notifications aux clients abonnés

## <a name="overview"></a>Vue d'ensemble
Windows joue généralement le rôle de client. Toutefois, de nombreux scénarios nécessitent que Windows se comporte également comme un serveur GATT Bluetooth Low Energy (BLE). La quasi-totalité des scénarios relatifs aux appareils IoT, ainsi que la plupart des communications BLE multiplateforme, exigent que Windows joue le rôle de serveur GATT. En outre, l’envoi de notifications aux appareils wearable à proximité constitue un scénario de plus en plus courant qui nécessite également l’emploi de cette technologie.  
> Avant de poursuivre, veillez à assimiler tous les concepts abordés dans la [documentation relative au client GATT](gatt-client.md).  

Les opérations de serveur concerneront le fournisseur de services et l’objet GattLocalCharacteristic. Ces deux classes fournit les fonctionnalités nécessaires pour déclarer, implémenter et exposer une hiérarchie de données à un périphérique distant.

## <a name="define-the-supported-services"></a>Définir les services pris en charge
Votre application peut déclarer un ou plusieurs services qui seront publiés par Windows. Chaque service est identifié de manière unique par un identificateur unique universel (UUID). 

### <a name="attributes-and-uuids"></a>Attributs et UUID
Chacun des services, caractéristiques et descripteurs est défini par son propre UUID 128 bits unique.
> Toutes les API Windows utilisent le terme GUID, mais la norme Bluetooth désigne cet identificateur sous le terme d’UUID. Dans notre cas, ces deux termes sont interchangeables, et nous continuerons donc à employer le terme UUID. 

Si l’attribut est standard et défini par le groupe d’intérêt spécial (SIG) Bluetooth, un identificateur court 16 bits lui est associé (par exemple, l’UUID du niveau de batterie est 0000**2A19**-0000-1000-8000-00805F9B34FB, et l’ID court correspondant est 0x2A19). Ces UUID standard sont utilisés dans les articles concernant [GattServiceUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids) et [GattCharacteristicUuids](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids).

Si votre application implémente son propre service personnalisé, vous devrez générer un UUID personnalisé. Vous pouvez effectuer cette opération très facilement dans Visual Studio à l’aide des commandes Outils -> Créer un Guid (en utilisant l’option 5 pour obtenir ce GUID au format « xxxxxxxx-xxxx-...xxxx »). Vous pourrez alors utiliser cet UUID pour déclarer de nouveaux services, caractéristiques ou descripteurs locaux.

#### <a name="restricted-services"></a>Services restreints
Les services ci-après sont réservés par le système et ne sont pas publiables pour l’instant :
1. Service d’informations de périphérique (DIS)
2. Service de profil d’attribut générique (GATT)
3. Service de profil d’accès générique (GAP)
4. Service de périphérique d’interface utilisateur (HOGP)
5. Service de paramètres de numérisation (SCP)

> Toute tentative de création d’un service bloqué entraînera le renvoi d’une valeur BluetoothError.DisabledByPolicy par l’appel de la méthode CreateAsync.

#### <a name="generated-attributes"></a>Attributs générés
Selon les éléments GattLocalCharacteristicParameters fournis lors de la création de la caractéristique, les descripteurs automatiquement générés par le système sont les suivants :
1. Configuration de la caractéristique du client (si la caractéristique est identifiée comme pouvant être indiquée ou notifiée).
2. Description de l’utilisateur de la caractéristique (si la propriété UserDescription est définie). Pour plus d’informations, reportez-vous à la propriété GattLocalCharacteristicParameters.UserDescription.
3. Format de la caractéristique (un descripteur par format de présentation spécifié).  Pour plus d’informations, reportez-vous à la propriété GattLocalCharacteristicParameters.PresentationFormats.
4. Agrégat de caractéristique (si plusieurs formats de présentation sont spécifiés).  Pour plus d’informations, reportez-vous à la propriété GattLocalCharacteristicParameters.See PresentationFormats.
5. Propriétés étendues de la caractéristique (si la caractéristique est identifiée avec le bit de propriétés étendues).

> La valeur du descripteur de propriétés étendues est déterminée par les propriétés de caractéristique ReliableWrites et WritableAuxiliaries.

> Toute tentative de création d’un descripteur réservé entraînera une exception.

> Notez que la diffusion n’est pas prise en charge pour l’instant.  La définition de la propriété de caractéristique GATT sur la valeur Broadcast entraînera une exception.

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>Générer la hiérarchie des services et les caractéristiques
L’objet GattServiceProvider permet de créer et de publier la définition des services principaux racines.  Chaque service requiert son propre objet ServiceProvider qui accepte un GUID : 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> Les services principaux constituent le premier niveau de l’arborescence GATT. Les services principaux contiennent des caractéristiques, ainsi que d’autres services (appelés services inclus ou services secondaires). 

Vous pouvez alors remplir le service avec les caractéristiques et les descripteurs requis :

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
Comme indiqué ci-dessus, cette étape constitue également l’endroit approprié pour déclarer les gestionnaires d’événements relatifs aux opérations prises en charge par chaque caractéristique.  Pour répondre correctement aux demandes, une application doit définir un gestionnaire d’événements pour chaque type de demande pris en charge par l’attribut.  L’échec de l’inscription d’un gestionnaire entraînera l’arrêt immédiat de la demande par le système avec la valeur *UnlikelyError*.

### <a name="constant-characteristics"></a>Caractéristiques constantes
Certaines valeurs de caractéristique restent invariables pendant toute la durée de vie de l’application. Il est donc recommandé de déclarer ces caractéristiques comme constantes afin d’empêcher toute activation d’application inutile : 

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
Une fois le service entièrement défini, l’étape suivante consiste à publier la prise en charge du service. Cette opération informe le système d’exploitation que le service doit être renvoyé lorsque les appareils distants procèdent à une détection des services.  Vous devrez définir deux propriétés, IsDiscoverable et IsConnectable :  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**: Publie le nom convivial pour les appareils distants dans la publication, en rendant l’appareil détectable.
- **IsConnectable**:  Publie une publication connectable pour une utilisation dans le rôle de périphérique.

> Lorsqu’un service est à la fois détectable et connectable, le système ajoute l’UUID du service au paquet d’annonce.  Le paquet d’annonce ne comprend que 31 octets, et un UUID 128 bits utilise 16 d’entre eux.

> Notez que lorsqu’un service est publié au premier plan, une application doit appeler StopAdvertising lorsque l’exécution de l’application est suspendue.

## <a name="respond-to-read-and-write-requests"></a>Répondre aux demandes de lecture et d’écriture
Comme nous l’avons vu ci-dessus lors de la déclaration des caractéristiques requises, GattLocalCharacteristics comporte 3 types d’événements : ReadRequested, WriteRequested et SubscribedClientsChanged.

### <a name="read"></a>Read
Lorsqu’un appareil distant essaie de lire une valeur à partir d’une caractéristique (et qu’il ne s’agit pas d’une valeur constante), l’événement ReadRequested est appelé. La caractéristique sur laquelle la lecture a été appelée est transmise au délégué avec des arguments (contenant des informations sur l’appareil distant) : 

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
Lorsqu’un appareil distant essaie d’écrire une valeur dans une caractéristique, l’événement WriteRequested est appelé avec des détails sur l’appareil distant, la caractéristique dans laquelle écrire la valeur et la valeur proprement dite : 

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
Il existe 2 types d’écritures : avec et sans réponse. Utilisez GattWriteOption (propriété sur l’objet GattWriteRequest) pour déterminer le type d’écriture effectué par l’appareil distant. 

## <a name="send-notifications-to-subscribed-clients"></a>Envoyer des notifications aux clients abonnés
Les notifications, qui constituent les opérations de serveur GATT les plus fréquentes, assurent la tâche cruciale de transmission des données aux appareils distants. Dans certains cas, vous souhaiterez avertir tous les clients abonnés, mais à d’autres moments, vous voudrez peut-être choisir les appareils auxquels envoyer la nouvelle valeur : 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

Lorsqu’un nouvel appareil s’abonne aux notifications, l’événement SubscribedClientsChanged est appelé : 

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
> Notez qu’une application peut obtenir la taille de notification maximale pour un client spécifique avec la propriété MaxNotificationSize.  Toutes les données supérieures à la taille maximale seront tronquées par le système.
