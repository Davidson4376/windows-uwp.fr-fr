---
author: TerryWarwick
title: Modèle de revendication de périphérique PointOfService
description: Découvrez le modèle revendication de périphérique PointOfService
ms.author: jken
ms.date: 06/4/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 202234530945e55ef9c0d0fb68cf9ca83d2e15c3
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983729"
---
# <a name="point-of-service-device-claim-model"></a>Modèle de revendication de périphérique de point de service

## <a name="claiming-a-device-for-exclusive-use"></a>Revendication de périphérique pour une utilisation exclusive

Une fois que vous avez créé un objet d’appareil PointOfService, vous devez le revendiquer à l’aide de la méthode de revendication appropriée pour le type de périphérique avant de pouvoir utiliser le périphérique pour l'entrée ou la sortie.  La revendication accorde à l’application un accès exclusif à de nombreuses fonctions du périphérique pour vous assurer qu’une application n’interfère pas avec l’utilisation de l’appareil par une autre application.  Une seule application peut revendiquer un périphérique PointOfService pour une utilisation exclusive à la fois. 

Cet exemple montre comment revendiquer un scanneur de code-barres après avoir créé avec succès un objet scanneur de code-barres.

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

> [!Warning]
> Une revendication peut se perdre dans les circonstances suivantes:
> 1. Une autre application a demandé une revendication du même périphérique et votre application n’a pas émis de **RetainDevice** en réponse à l'événement **ReleaseDeviceRequested**.  (Voir [Négociation de revendication](#Claim-negotiation) ci-dessous pour plus d’informations.)
> 2. Votre application a été suspendue, ce qui a entraîné la fermeture de l'objet de périphérique et par conséquent la revendication n’est plus valide. (Voir [Cycle de vie de l'objet appareil](pos-basics-deviceobject.md#device-object-lifecycle) pour plus d’informations.)

### <a name="apis-used-for-claiming"></a>API utilisées pour la revendication

|Appareil|Revendication |
|-|:-|
|BarcodeScanner | [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | 
|CashDrawer | [ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | 
|LineDisplay | [ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |
|MagneticStripeReader | [ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) | 
|PosPrinter | [ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) | 
|

## <a name="claim-negotiation"></a>Négociation de revendication

Comme Windows est un environnement multitâche, il est possible que plusieurs applications sur le même ordinateur exigent un accès aux périphériques de manière coopérative.  Les API PointOfService fournissent un modèle de négociation qui permet à plusieurs applications de partager des périphériques connectés à l’ordinateur.

Lorsqu’une deuxième application sur le même ordinateur demande une revendication pour un périphérique PointOfService déjà réclamé par une autre application, une notification d’événements **ReleaseDeviceRequested** est publiée. L’application avec la revendication active doit répondre à la notification d’événements en appelant **RetainDevice** si elle utilise actuellement le périphérique afin d'éviter de perdre la revendication. 

Si l’application avec la revendication active ne répond pas avec **RetainDevice** immédiatement, il est supposé que l’application a été suspendue ou n’a pas besoin du périphérique. La revendication est alors révoquée et donnée à la nouvelle application. 

Cet exemple montre comment conserver un scanneur de code-barres revendiqué après qu’une autre application a demandé la libération de l’appareil.  

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
{
    // Retain exclusive access to the device
    myScanner.RetainDevice();  
}
```
### <a name="apis-used-for-claim-negotiation"></a>API utilisées pour la négociation de revendication

|Périphérique revendiqué|Notification de libération| Conserver l’appareil |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
