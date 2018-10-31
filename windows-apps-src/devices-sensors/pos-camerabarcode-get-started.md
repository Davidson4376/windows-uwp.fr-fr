---
author: TerryWarwick
title: Prise en main du scanneur de code-barres à caméra
description: Apprendre à utiliser le scanneur de codes-barres à caméra
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 12aabff66fc116f510dced78aa56f3df5f84c850
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5840182"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>Prise en main du scanneur de code-barres à caméra
## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>Étape1: Ajouter des déclarations de fonctionnalités à votre manifeste d’application
1. Dans Microsoft Visual Studio, dans l’**Explorateur de solutions**, ouvrez le concepteur pour le manifeste de l’application en double-cliquant sur l’élément **package.appxmanifest**.
2. Sélectionnez l’onglet **Fonctionnalités**.
3. Cochez les cases **Webcam** et **PointOfService** 

>[!NOTE] 
> La fonctionnalité **Webcam** est nécessaire pour que le décodeur logiciel reçoive les images de la caméra à décoder et pour fournir un aperçu de votre application.

## <a name="step-2-add-using-directives"></a>Étape2: Ajouter des directives d'utilisation

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```
## <a name="step-3-define-your-device-selector"></a>Étape3: Définir votre sélecteur de périphérique

### **<a name="option-a-find-all-barcode-scanners"></a>Option A: Rechercher tous les scanneurs de codes-barres**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();       
```

### **<a name="option-b-scoping-device-selector-to-connection-type"></a>Option B: Réduire le champ du sélecteur de périphérique au type de connexion**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-barcode-scanners"></a>Étape4: Énumérer les scanneurs de codes-barres
Si vous ne prévoyez pas que la liste des périphériques change au cours de la durée de vie de votre application, vous pouvez énumérer une capture instantanée une seule fois avec *FindAllAsync*, mais si vous pensez que la liste des scanneurs de codes-barres peut changer , vous devez utiliser un *DeviceWatcher* à la place.  

> [!Important] 
> L'utilisation de GetDefaultAsync pour énumérer les périphériques PointOfService peut entraîner un comportement incohérent, car il retourne simplement le premier périphérique disponible dans la classe et cela peut changer d’une session à l’autre.

### **<a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>Option A: Énumérer une capture instantanée des scanneurs de codes-barres**
```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> Voir [*Énumérer une capture instantanée d’appareils*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices) pour plus d’informations sur l’utilisation de *FindAllAsync*.

### **<a name="option-b-enumerate-and-watch-for-changes-in-available-barcode-scanners"></a>Option B: Énumérer et suivre les modifications des scanneurs de codes-barres disponibles**
```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);

// TODO: Add Event Listeners and Handlers
```
> [!TIP]
> Voir [*Énumérer et observer les changements de périphériques*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) et [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) pour plus d’informations.

## <a name="step-5-identify-camera-barcode-scanners"></a>Étape5: Identifier les scanneurs de codes-barres à caméra
Un scanneur de codes-barres à caméra est créé dynamiquement à mesure que Windows associe les caméras connectées à votre ordinateur avec un logiciel de décodage.  Chaque paire caméra-décodeur est un scanneur de code-barres entièrement fonctionnel.

Pour chaque scanneur de codes-barres dans la collection de périphériques résultante, vous pouvez déterminer lequel est un scanneur de codes-barres à caméra par opposition aux scanneurs de code-barres physique en cochant la propriété [*BarcodeScanner.VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId).  Un VideoDeviceID non NULL indique que l’objet scanneur de codes-barres de votre collection de périphériques est un scanneur de codes-barres à caméra.  Si vous avez plusieurs scanneurs de codes-barres à caméra, vous aurez peut-être intérêt à créer une collection distincte qui exclut les scanneurs de codes-barres physique. 

Les scanneurs de codes-barres à caméra qui utilisent le décodeur fourni avec Windows sont affichés en tant que: 

> MicrosoftBarcodeScanner (*nom de votre caméra ici*)

Si votre caméra est intégrée au châssis de votre ordinateur, son nom peut se distinguer entre *avant* et *arrière*, si vous avez plus d’une caméra.  À l’avenir, d'autres décodeurs logiciels pourront être disponibles et être assortis d'un autre schéma d’affectation des noms.

## <a name="step-6-claim-the-camera-barcode-scanner"></a>Étape6: Revendication du scanneur de codes-barres à caméra 
Utilisez [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) pour obtenir l'utilisation exclusive du scanneur de codes-barres à caméra.

## <a name="step-7-system-provided-preview"></a>Étape7: Aperçu fourni par le système
Un aperçu caméra est nécessaire pour que l’utilisateur pointe avec succès la caméra sur le codes-barres.  Windows fournit un aperçu caméra simple qui lance une boîte de dialogue permettant un contrôle de base du scanneur de codes-barres à caméra.  Il vous suffit d’appeler [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) pour ouvrir la boîte de dialogue et [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) pour la fermer lorsque vous avez terminé.

> [!TIP]
> Voir [Héberger l'aperçu](pos-camerabarcode-hosting-preview.md) pour héberger l’aperçu du scanneur de codes-barres à caméra dans votre application.

## <a name="step-8-initiate-scan"></a>Étape 8: Analyse de début 
Vous pouvez lancer le processus de lecture en appelant [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).  
En fonction de la valeur de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived), le scanneur peut lire un seul code-barres puis s'arrêter, ou lire en continu jusqu'à ce que vous appeliez [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Définissez la valeur souhaitée de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) pour contrôler le comportement du scanneur à chaque décodage de code-barres.

| Valeur | Description |
| ----- | ----------- |
| Vrai   | Lire un seul code-barres puis s'arrêter |
| Faux  | Lire des codes-barres en continu sans interruption |