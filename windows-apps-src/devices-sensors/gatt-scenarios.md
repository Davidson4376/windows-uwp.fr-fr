---
ms.assetid: 28B30708-FE08-4BE9-AE11-5429F963C330
title: Bluetooth GATT
description: Cet article fournit une vue d’ensemble du profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant les trois utilisations communes de GATT.
---
# Bluetooth GATT

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

** API importantes

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Cet article fournit une vue d’ensemble du profil d’attribut générique (GATT) Bluetooth pour les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant les trois utilisations communes de GATT : récupération de données Bluetooth, contrôle d’un appareil de thermométrie Bluetooth LE, et contrôle de la présentation des données d’appareil Bluetooth LE.

## Vue d’ensemble

Les développeurs peuvent utiliser les API de l’espace de noms [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) pour accéder aux services, descripteurs et caractéristiques Bluetooth LE. Les appareils Bluetooth LE exposent leurs fonctionnalités par le biais d’une collection de :

-   services principaux ;
-   services inclus ;
-   caractéristiques ;
-   descripteurs.

Les services principaux définissent le contrat fonctionnel du périphérique LE et contiennent une collection de caractéristiques qui décrivent le service. Ces caractéristiques, à leur tour, contiennent des descripteurs qui décrivent les caractéristiques.

Les API GATT Bluetooth exposent des objets et des fonctions, plutôt que d’accéder simplement au transport. Au niveau du pilote, les services principaux sont énumérés en tant que nœuds d’appareils enfants de l’appareil Bluetooth LE à l’aide des API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459).

En outre, les API GATT Bluetooth permettent aux développeurs d’utiliser des appareils Bluetooth LE en vue d’effectuer les tâches suivantes :

-   Effectuer une découverte des services, caractéristiques et descripteurs
-   Lire et écrire les valeurs des caractéristiques et des descripteurs
-   Inscrire un rappel pour l’événement de modification de la valeur d’une caractéristique

Les API GATT Bluetooth simplifient le développement en gérant des propriétés communes et en fournissant des valeurs par défaut qui facilitent raisonnablement la gestion et la configuration des appareils. Elles permettent aux développeurs d’accéder aux fonctionnalités d’un appareil Bluetooth LE à partir d’une application.

Pour créer une implémentation utile, un développeur doit savoir au préalable quels sont les services et les caractéristiques GATT que l’application est susceptible d’utiliser et traiter les valeurs spécifiques des caractéristiques de manière à ce que les données binaires fournies par l’API soient transformées en données utiles avant d’être présentées à l’utilisateur. Les API GATT Bluetooth exposent uniquement les primitives de base nécessaires pour communiquer avec un périphérique Bluetooth LE. Pour interpréter les données, un profil d’application doit être défini, soit par un profil Bluetooth SIG standard, soit par un profil personnalisé implémenté par un fournisseur de périphérique. Un profil crée un contrat de liaison entre l’application et le périphérique, qui définit ce que représentent les données échangées et la façon de les interpréter.

Pour des raisons pratiques, le Bluetooth SIG gère une [liste de profils publics](http://go.microsoft.com/fwlink/p/?LinkID=317977) disponibles.

## Récupérer des données Bluetooth

Dans cet exemple, l’application utilise des mesures de température d’un appareil Bluetooth LE qui implémente le service Health Thermometer Bluetooth LE. L’application spécifie qu’elle doit être avertie quand une nouvelle mesure de température est disponible. Grâce à l’inscription d’un gestionnaire d’événements pour la « modification de la valeur de la caractéristique Thermometer », l’application reçoit des notifications liées à la modification de la valeur de la caractéristique tout en s’exécutant au premier plan.

Quand l’application est suspendue, elle doit libérer toutes les ressources d’appareil, tandis qu’à sa reprise, elle doit réexécuter l’initialisation et l’énumération des appareils.

```csharp
double convertTemperatureData(byte[] temperatureData)
{
    // Read temperature data in IEEE 11703 floating point format
    // temperatureData[0] contains flags about optional data - not used
    UInt32 mantissa = ((UInt32)temperatureData[3] << 16) |
        ((UInt32)temperatureData[2] << 8) |
        ((UInt32)temperatureData[1]);

    Int32 exponent = (Int32)temperatureData[4];

    return mantissa * Math.Pow(10.0, exponent);
}

async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService firstThermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic thermometerCharacteristic =
        firstThermometerService.GetCharacteristics(
            GattCharacteristicUuids.TemperatureMeasurement)[0];

    thermometerCharacteristic.ValueChanged += temperatureMeasurementChanged;

    await thermometerCharacteristic
        .WriteClientCharacteristicConfigurationDescriptorAsync(
            GattClientCharacteristicConfigurationDescriptorValue.Notify);
}

void temperatureMeasurementChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte[] temperatureData = new byte[eventArgs.CharacteristicValue.Length];
    Windows.Storage.Streams.DataReader.FromBuffer(
        eventArgs.CharacteristicValue).ReadBytes(temperatureData);

    var temperatureValue = convertTemperatureData(temperatureData);

    temperatureTextBlock.Text = temperatureValue.ToString();
}
```

```cpp
double MainPage::ConvertTemperatureData(
    const Array<unsigned char>^ temperatureData)
{
    unsigned mantissa = ((unsigned)temperatureData[3] << 16) |
        ((unsigned)temperatureData[2] << 8) |
        ((unsigned)temperatureData[1]);

    int exponent = (int)temperatureData[4];

    return mantissa * pow(10.0, (double)exponent);
}

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id))
            .then([this] (GattDeviceService^ firstThermometerService) 
        {
            GattCharacteristic^ thermometerCharacteristic = 
                firstThermometerService->GetCharacteristics(
                    GattCharacteristicUuids::TemperatureMeasurement)
                        ->GetAt(0);

            thermometerCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>(
                        this, &amp;MainPage::TemperatureMeasurementChanged);

            create_task(thermometerCharacteristic->
                WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}


void MainPage::TemperatureMeasurementChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    auto temperatureData =  ref new Array<unsigned char>(
        eventArgs->CharacteristicValue->Length);
    DataReader::FromBuffer(eventArgs->CharacteristicValue)
        ->ReadBytes(temperatureData);

    double temperatureValue = ConvertTemperatureData(temperatureData);
    std::wstringstream str;
    str << temperatureValue;

    temperatureTextBlock->Text = ref new String(str.str().c_str());
}
```

## Contrôler un appareil de thermométrie Bluetooth LE

Dans cet exemple, une application UWP sert de contrôleur pour un appareil de thermométrie Bluetooth LE fictif. Outre les caractéristiques standard du profil [**HealthThermometer**](https://msdn.microsoft.com/library/windows/apps/Dn297603), l’appareil déclare également une caractéristique Format qui permet aux utilisateurs de récupérer la valeur en degrés Celsius ou Fahrenheit. L’application utilise des transactions d’écriture fiables pour que le format et l’intervalle de mesure soient définis en tant que valeur unique.

```csharp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid = 
    new Guid("{00000000-0000-0000-0000-000000000010}");

// Constant representing a Fahrenheit scale temperature measurement
const byte FahrenheitReading = 1;
async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService thermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic intervalCharacteristic = thermometerService
        .GetCharacteristics(GattCharacteristicUuids.MeasurementInterval)[0];

    GattCharacteristic formatCharacteristic = thermometerService
        .GetCharacteristics(formatCharacteristicUuid)[0];

    GattReliableWriteTransaction gattTransaction = 
        new GattReliableWriteTransaction();

    var writer = new Windows.Storage.Streams.DataWriter();

    // Get a temperature reading every 60 seconds
    writer.WriteUInt16(60);

    gattTransaction.WriteValue(
        intervalCharacteristic, 
        writer.DetachBuffer());

    // Get the reading on the Fahrenheit scale
    writer.WriteByte(FahrenheitReading);

    gattTransaction.WriteValue(
        formatCharacteristic, 
        writer.DetachBuffer());

    GattCommunicationStatus status = await gattTransaction.CommitAsync();

    if (GattCommunicationStatus.Unreachable == status)
    {
        statusTextBlock.Text = "Writing to your device failed !";
    }
}
```

```cpp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid(0x00000000, 0x0000, 0x0000, 0x00, 0x00, 
                                  0x00, 0x00, 0x00, 0x00, 0x00, 0x10);

// Constant representing a Fahrenheit scale temperature measurement
const unsigned char FAHRENHEIT_READING = 1;

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ thermometerService) 
        {
            GattCharacteristic^ intervalCharacteristic = 
                thermometerService->GetCharacteristics(
                    GattCharacteristicUuids::MeasurementInterval)
                        ->GetAt(0);

            GattCharacteristic^ formatCharacteristic = 
                thermometerService->GetCharacteristics(
                    formatCharacteristicUuid)->GetAt(0);

            GattReliableWriteTransaction^ gattTransaction = 
                ref new GattReliableWriteTransaction();

            DataWriter^ writer = ref new DataWriter();

            // Get a temperature reading every 60 seconds
            writer->WriteUInt16(60);

            gattTransaction->WriteValue(
                intervalCharacteristic, 
                writer->DetachBuffer());

            writer->WriteByte(FAHRENHEIT_READING);

            gattTransaction->WriteValue(
                formatCharacteristic, 
                writer->DetachBuffer());

            create_task(gattTransaction->CommitAsync())
                .then([this] (GattCommunicationStatus status) 
            {
                if (GattCommunicationStatus::Unreachable == status) 
                { 
                    statusTextBlock->Text = 
                        ref new String(L"Writing to your device failed !");
                }
            });
        });
    });

```

## Contrôler la présentation des données d’appareil Bluetooth LE

Un appareil Bluetooth LE peut exposer un service de batterie qui indique à l’utilisateur le niveau de batterie actuel. Le service de batterie comprend un descripteur [**PresentationFormats**](https://msdn.microsoft.com/library/windows/apps/Dn263742) facultatif qui permet d’interpréter les données du niveau de la batterie avec une certaine souplesse. Cette situation illustre une application qui utilise un appareil de ce type et se sert de la propriété **PresentationFormats** pour mettre en forme une valeur de caractéristique, avant de la présenter à l’utilisateur.

```csharp
async void Initialize()
{
    var batteryServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(GattServiceUuids.Battery),
        null);

    if (batteryServices.Count > 0)
    {
        // Use the first Battery service on the system
        GattDeviceService batteryService = await GattDeviceService
            .FromIdAsync(batteryServices[0].Id);

        // Use the first Characteristic of that Service
        GattCharacteristic batteryLevelCharacteristic =
            batteryService.GetCharacteristics(
                GattCharacteristicUuids.BatteryLevel)[0];

        batteryLevelCharacteristic.ValueChanged += batteryLevelChanged;
    }
    else
    {
        statusTextBlock.Text = "No Battery services found !";
    }
}

void batteryLevelChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte levelData = Windows.Storage.Streams.DataReader
        .FromBuffer(eventArgs.CharacteristicValue).ReadByte();

    double levelValue;

    if (sender.PresentationFormats.Count > 0)
    {
        levelValue = levelData * 
            Math.Pow(10.0, sender.PresentationFormats[0].Exponent);
    }
    else
    {
        levelValue = (double)levelData;
    }

    batteryLevelTextBlock.Text = levelValue.ToString();
}
```

```cpp
void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::Battery), 
        nullptr)).then([this] (DeviceInformationCollection^ batteryServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            batteryServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ batteryService) 
        {
            GattCharacteristic^ batteryLevelCharacteristic = 
                batteryService->GetCharacteristics(
                    GattCharacteristicUuids::BatteryLevel)->GetAt(0);

            batteryLevelCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>
                    (this, &amp;MainPage::BatteryLevelChanged);

            create_task(batteryLevelCharacteristic
                ->WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}

void MainPage::BatteryLevelChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    unsigned char batteryLevelData = DataReader::FromBuffer(
        eventArgs->CharacteristicValue)->ReadByte();

    // if this characteristic has a presentation format
    // use that information to format the value
    double batteryLevelValue;
    if (sender->PresentationFormats->Size > 0)
    {
        batteryLevelValue = batteryLevelData * 
            pow(10.0, sender->PresentationFormats->GetAt(0)->Exponent);
    }
    else
    {
        batteryLevelValue = batteryLevelData;
    }

    std::wstringstream str;
    str << batteryLevelValue;
    batteryLevelTextBlock->Text = 
        ref new String(str.str().c_str());
}
```





<!--HONumber=Mar16_HO1-->


