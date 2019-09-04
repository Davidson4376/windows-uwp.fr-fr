---
title: Prise en main du scanneur de code-barres à caméra
description: Apprendre à utiliser le scanneur de codes-barres à caméra
ms.date: 09/02/2019
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: b35ff6b183a6344fbc8da6b44a6cb81ea695a1c9
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243295"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Prise en main du scanneur de code-barres à caméra

Les extraits de code utilisés ici sont fournis à des fins de démonstration uniquement. Pour obtenir un exemple fonctionnel, consultez l' [exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner).

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Étape 1 : Ajouter des déclarations de fonctionnalité à votre manifeste d’application

1. Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2. Sélectionnez l’onglet **Fonctionnalités**.
3. Cochez les cases **Webcam** et **PointOfService**

>[!NOTE]
> La fonctionnalité **Webcam** est nécessaire pour que le décodeur logiciel reçoive les images de la caméra à décoder et pour fournir un aperçu de votre application.

## <a name="step-2-add-using-directives"></a>Étape 2 : Ajouter des directives using

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>Étape 3 : Définir votre sélecteur d’appareil

### <a name="option-a-find-all-barcode-scanners"></a>**Option A : Rechercher tous les scanneurs de codes-barres**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**Option B : Portée du sélecteur d’appareil à un type de connexion**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>Étape 4 : Énumérer tous les scanneurs de codes-barres

Si vous ne vous attendez pas à ce que la liste des appareils change sur la durée de vie de votre application, vous pouvez énumérer une capture instantanée une seule fois avec [DeviceInformation. FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync), mais si vous pensez que la liste des scanneurs de codes-barres peut changer pendant la durée de vie de votre application, vous devez utiliser un [DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) à la place.  

> [!Important]
> L'utilisation de GetDefaultAsync pour énumérer les périphériques PointOfService peut entraîner un comportement incohérent, car il retourne simplement le premier périphérique disponible dans la classe et cela peut changer d’une session à l’autre.

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**Option A : Énumérer un instantané des scanneurs de codes-barres**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Voir [*Énumérer une capture instantanée d’appareils*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) pour plus d’informations sur l’utilisation de *FindAllAsync*.

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**Option B : Énumérer les scanneurs de codes-barres disponibles et surveiller les modifications apportées aux scanneurs disponibles**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> Voir [*Énumérer et observer les changements de périphériques*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) et [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) pour plus d’informations.

## <a name="step-5-identify-camera-barcode-scanners"></a>Étape 5 : Identifier les scanneurs de codes-barres de l’appareil photo

Un scanneur de codes-barres à caméra est créé dynamiquement à mesure que Windows associe les caméras connectées à votre ordinateur avec un logiciel de décodage.  Chaque paire caméra-décodeur est un scanneur de code-barres entièrement fonctionnel.

Pour chaque scanneur de codes-barres du regroupement d’appareils obtenu, vous pouvez différencier les scanneurs de code-barres de l’appareil photo des scanneurs de codes-barres physiques en activant la propriété [*BarcodeScanner. VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) .  Un VideoDeviceID non NULL indique que l’objet de scanneur de codes-barres de votre collection de périphériques est un scanneur de codes-barres de l’appareil photo.  Si vous avez plusieurs scanneurs de codes-barres d’appareil photo, vous souhaiterez peut-être créer un regroupement distinct qui exclut les scanneurs de codes-barres physiques.

Les scanneurs de codes-barres de l’appareil photo utilisant le décodeur fourni avec Windows sont identifiés comme suit :

> Microsoft BarcodeScanner (*nom de votre caméra ici*)

Si vous disposez de plusieurs caméras et que celles-ci sont intégrées au châssis de votre ordinateur, le nom peut faire la différence entre les caméras de l' *avant* et l' *arrière* .

> [!NOTE]
> À l’avenir, des décodeurs logiciels supplémentaires avec des schémas de nommage différents pourront être publiés.

Quand le DeviceWatcher démarre (étape 4), il énumère chaque appareil connecté. Ici, nous allons ajouter les scanneurs disponibles à une collection de scanneurs de codes-barres et lier la collection à une zone de liste.

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

Lorsque la SelectedIndex du ListBox change (le premier élément est sélectionné par défaut dans l’extrait de code précédent), nous interrogeons les informations de l’appareil.

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Étape 6 : Demander le scanneur de codes-barres de l’appareil photo

Utilisez [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) pour obtenir l'utilisation exclusive du scanneur de codes-barres à caméra.

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>Étape 7 : Version préliminaire fournie par le système

Un aperçu caméra est nécessaire pour que l’utilisateur pointe avec succès la caméra sur le codes-barres.  Windows fournit un aperçu de caméra simple qui lance une boîte de dialogue de contrôle de base du scanneur de codes-barres de l’appareil photo.  Il vous suffit d’appeler [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) pour ouvrir la boîte de dialogue et [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) pour la fermer lorsque vous avez terminé.

> [!TIP]
> Voir [Héberger l'aperçu](pos-camerabarcode-hosting-preview.md) pour héberger l’aperçu du scanneur de codes-barres à caméra dans votre application.

## <a name="step-8-initiate-scan"></a>Étape 8 : Lancer l’analyse

Vous pouvez lancer le processus de lecture en appelant [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Selon la valeur de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) , le scanneur peut analyser un seul code-barres, l’arrêter ou l’analyser en continu jusqu’à ce que vous appeliez [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Définissez la valeur souhaitée de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) pour contrôler le comportement du scanneur à chaque décodage de code-barres.

| Value | Description |
| ----- | ----------- |
| True   | Lire un seul code-barres puis s'arrêter |
| False  | Lire des codes-barres en continu sans interruption |

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>Exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
