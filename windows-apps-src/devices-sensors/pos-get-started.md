---
author: TerryWarwick
title: Prise en main de la technologie de point de service
description: Cet article comporte des informations sur la prise en main des API UWP de point de vente.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: f3959254787ce22bea27495520805485e0ea179b
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5920870"
---
# <a name="getting-started-with-point-of-service"></a>Prise en main de la technologie de point de service

Les périphériques de point de vente (PDV), aussi appelés appareils de point de service (POS), sont des périphériques informatiques utilisés pour faciliter les transactions de vente au détail. Les périphériques de POS incluent notamment les caisses enregistreuses électroniques, les scanneurs de code-barres, les lecteurs de bandes magnétiques et les imprimantes de reçus.

Vous allez apprendre ici les principes fondamentaux d’interfaçage avec des appareils de point de service à l’aide des API PointOfService de la plateforme Windows universelle (UWP). Nous aborderons l’énumération d'appareils, la vérification des fonctionnalités des appareils, la revendication d'appareils et le partage d'appareils. Dans cet exemple, nous utilisons un scanneur de code-barres, mais presque tous les conseils fournis ici s’appliquent à l'ensemble des appareils de point de service compatibles UWP. (Pour obtenir la liste des périphériques pris en charge, voir [Prise en charge des périphériques de point de service](pos-device-support.md)).

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>Recherche et connexion aux périphériques de point de service

Avant qu'un périphérique de point de service puisse être utilisé par une application, il doit être couplé au PC sur lequel l’application s’exécute. Il existe plusieurs façons de se connecter aux périphériques de point de service, soit par programmation, soit par le biais de l’application Paramètres.

### <a name="connecting-to-devices-by-using-the-settings-app"></a>Connexion aux appareils à l’aide de l’application Paramètres
Lorsque vous connectez un périphérique de point de service comme un scanneur de code-barres à un PC, il apparaît comme n'importe quel autre appareil. Vous pouvez le trouver dans la section **Appareils > Bluetooth et autres paramètres d’appareils** de l’application Paramètres. Pour effectuer un couplage avec un périphérique de point de service, sélectionnez **Ajouter un appareil Bluetooth ou un autre appareil**.

Certains périphériques de point de service peuvent ne pas apparaître dans l’application Paramètres tant qu'ils n'ont pas été énumérés par programmation à l’aide des API de point de service.

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>Obtention d’un seul périphérique de point de service avec GetDefaultAsync
Dans un cas d’utilisation simple, vous pouvez n'avoir qu'un seul périphérique de point de service connecté au PC sur lequel l’application s'exécute, et souhaiter configurer celui-ci aussi rapidement que possible. Pour ce faire, récupérez l’appareil «par défaut» avec la méthode **GetDefaultAsync**, comme illustré ici.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

Si l’appareil par défaut est trouvé, l’objet appareil récupéré est prêt pour la revendication. La «revendication» d'un appareil fournit à une application un accès exclusif à celui-ci, ce qui empêche les conflits de commandes provenant de divers processus.

> [!NOTE] 
> Si plusieurs périphériques de point de service sont connectés au PC, **GetDefaultAsync** renvoie le premier appareil détecté. Pour cette raison, utilisez **FindAllAsync**, sauf si êtes sûr qu’un seul périphérique de point de service est visible par l’application.

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>Énumération d’une collection d’appareils avec FindAllAsync

Lorsque vous êtes connecté à plusieurs appareils, vous devez énumérer la collection des objets appareils **PointOfService** pour trouver celui que vous souhaitez revendiquer. Par exemple, le code suivant crée une collection de tous les scanneurs de code-barres actuellement connectés, puis recherche dans la collection un scanneur portant un nom spécifique.

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>Étendue de la sélection d’appareils
Lors de la connexion à un appareil, vous pouvez limiter votre recherche à un sous-ensemble de périphériques de point de service auxquels votre application a accès. La méthode **GetDeviceSelector** vous permet d'étendre la sélection afin de récupérer les appareils connectés uniquement par une méthode donnée (Bluetooth, USB, etc.). Vous pouvez créer un sélecteur qui recherche les appareils connectés via **Bluetooth**, **IP**, **Local**, ou **Tous les types de connexion**. Cela peut s'avérer utile, car la détection d’appareils sans fil prend beaucoup plus de temps que la détection locale (câblée). Vous pouvez garantir un temps d’attente déterministe pour la connexion d’appareils locale en limitant **FindAllAsync** aux types de connexion **Local**. Par exemple, ce code récupère tous les scanneurs de code-barres accessibles via une connexion locale. 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>Réaction aux changements de connexion d'appareils avec DeviceWatcher

Lors de l'exécution de votre application, des appareils seront parfois déconnectés ou mis à jour, ou de nouveaux appareils devront être ajoutés. Vous pouvez utiliser la classe **DeviceWatcher** pour accéder aux événements relatifs aux appareils, afin que votre application puisse y répondre en conséquence. Voici un exemple illustrant comment utiliser **DeviceWatcher** avec des stubs de méthode à appeler si un appareil est ajouté, supprimé ou mis à jour.

```Csharp
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

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>Vérification des fonctionnalités d’un périphérique de point de service
Même au sein d’une classe d’appareil, telle que des scanneurs de code-barres, les attributs de chaque appareil peuvent varier considérablement entre les modèles. Si votre application requiert un attribut d'appareil spécifique, vous devrez peut-être inspecter chaque objet appareil connecté afin de déterminer si l’attribut est pris en charge. Par exemple, votre entreprise a peut-être besoin de créer des étiquettes à l’aide d’un modèle d’impression de code-barres spécifique. Voici comment vous pouvez vérifier si un scanneur de code-barres connecté prend en charge une symbologie donnée. 

> [!NOTE]
> Un symbolisme est le mappage de langue utilisé par un code-barres pour encoder les messages.

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>Utilisation de la classe Device.Capabilities
La classe **Device.Capabilities** est un attribut de toutes les classes de périphériques de point de service, qui peut être utilisé pour obtenir des informations générales sur chaque appareil. Ainsi, cet exemple détermine si un appareil prend en charge la création de rapports statistiques et, si tel est le cas, récupère des statistiques pour tous les types pris en charge.

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>Revendication d'un périphérique de point de service
Avant de pouvoir utiliser un périphérique de point de service pour des entrées ou sorties actives, vous devez le revendiquer en accordant à l'application l’accès exclusif à la plupart de ses fonctions. Ce code indique comment revendiquer un scanneur de code-barres, une fois que vous avez trouvé l’appareil en utilisant l’une des méthodes décrites précédemment.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>Conservation de l’appareil
Lorsque vous utilisez un périphérique de point de service via une connexion réseau ou Bluetooth, il se peut que vous soyez amené à partager l’appareil avec d’autres applications sur le réseau. (Pour plus d’informations à ce sujet, voir [Partage d'appareils](#sharing-a-device-between-apps).) Dans d'autres cas, vous voudrez peut-être rester connecté à l’appareil pour une utilisation prolongée. Cet exemple montre comment conserver un scanneur de code-barres revendiqué après qu’une autre application a demandé la libération de l’appareil.

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>Entrées et sorties

Une fois que vous avez revendiqué un appareil, vous êtes presque prêt à l’utiliser. Pour recevoir des entrées de l’appareil, vous devez configurer un délégué et l'activer pour recevoir des données. Dans l’exemple ci-dessous, nous allons revendiquer un scanneur de code-barres, définir sa propriété de décodage, puis appeler **EnableAsync** pour activer les entrées décodées depuis l’appareil. Ce processus varie selon les classes d'appareils, par conséquent, pour obtenir des instructions sur la façon de configurer un délégué pour les appareils ne prenant pas en charge les code-barres, reportez-vous à l'[exemple d’application UWP](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors) approprié.

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>Partage d’un appareil entre des applications

Les périphériques de point de service sont souvent utilisés dans des cas où plusieurs applications doivent y accéder sur une courte période.  Un appareil peut être partagé lorsqu'il est connecté à plusieurs applications en local (USB ou autre connexion câblée), ou via un réseau IP ou Bluetooth. Selon les besoins de chaque application, un processus peut avoir besoin de supprimer sa revendication sur l’appareil. Ce code supprime notre scanneur de code-barres revendiqué, autorisant ainsi sa revendication et son utilisation par d’autres applications.

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> Les classes de périphériques de point de service revendiquées et non revendiquées implémentent l'[interface IClosable](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable). Si un périphérique est connecté à une application via le réseau ou Bluetooth, les objets revendiqués et non revendiqués doivent être supprimés avant qu'une autre application puisse s'y connecter.

## <a name="see-also"></a>Articles associés
+ [Exemple de scanneur de code-barres](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemple de caisse enregistreuse]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemple d’affichage de ligne](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemple de lecteur de bande magnétique](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemple d’imprimante de POS](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

