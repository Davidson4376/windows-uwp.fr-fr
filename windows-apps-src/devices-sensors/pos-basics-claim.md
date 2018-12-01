---
title: APPAREIL PointOfService revendiquer et activer le modèle
description: En savoir plus sur la revendication PointOfService et activer le modèle
ms.date: 06/19/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 7169848084b587793ba1537ea3d6ad78d31892d5
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8340727"
---
# <a name="point-of-service-device-claim-and-enable-model"></a>Revendication de périphérique de point de Service et activer le modèle

## <a name="claiming-for-exclusive-use"></a>La revendication pour une utilisation exclusive

Une fois que vous avez créé un objet d’appareil PointOfService, vous devez le revendiquer à l’aide de la méthode de revendication appropriée pour le type de périphérique avant de pouvoir utiliser le périphérique pour l'entrée ou la sortie.  La revendication accorde à l’application un accès exclusif à de nombreuses fonctions du périphérique pour vous assurer qu’une application n’interfère pas avec l’utilisation de l’appareil par une autre application.  Une seule application peut revendiquer un périphérique PointOfService pour une utilisation exclusive à la fois. 

> [!Note]
> L’action de revendication établit un accès exclusif à un appareil, mais ne le ne pas placer dans un état opérationnel.  Pour plus d’informations, consultez [Activer l’appareil pour les opérations d’e/s](#Enable-device-for-I/O-operations) .

### <a name="apis-used-to-claim--release"></a>API utilisées pour revendiquer / de publication

|Appareil|Revendication | Release | 
|-|:-|:-|
|BarcodeScanner | [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | [ClaimedBarcodeScanner.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.close) |
|CashDrawer | [CashDrawer.ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | [ClaimedCashDrawer.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.close) | 
|LineDisplay | [LineDisplay.ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |  [ClaimedineDisplay.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.close) | 
|MagneticStripeReader | [MagneticStripeReader.ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) |  [ClaimedMagneticStripeReader.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.close) | 
|PosPrinter | [PosPrinter.ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) |  [ClaimedPosPrinter.Close](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.close) | 
 | 

## <a name="enable-device-for-io-operations"></a>Activer le périphérique pour les opérations d’e/s

L’action de revendication simplement établit une droits exclusifs à l’appareil, mais ne le ne pas placer dans un état opérationnel.  Afin de pouvoir recevoir des événements ou d’effectuer des opérations de sortie, vous devez activer l’appareil à l’aide de **EnableAsync**.  À l’inverse, vous pouvez appeler **DisableAsync** pour cesser d’écouter les événements à partir de l’appareil ou l’exécution de sortie.  Vous pouvez également utiliser **IsEnabled** pour déterminer l’état de votre appareil.

### <a name="apis-used-enable--disable"></a>API utilisées Active / désactiver

| Appareil | Activer | Désactiver | IsEnabled? |
|-|:-|:-|:-|
|ClaimedBarcodeScanner | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isenabled) | 
|ClaimedCashDrawer | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.isenabled) |
|ClaimedLineDisplay | Pas Applicable¹ | Pas Applicable¹ | Pas Applicable¹ | 
|ClaimedMagneticStripeReader | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.disableasync) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.isenabled) |  
|ClaimedPosPrinter | [EnableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.enableasync) | [DisableAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.disableasyc) | [IsEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.isenabled) |
|

Affichage de ligne ¹ ne nécessite pas vous permettent d’activer explicitement l’appareil pour les opérations d’e/s.  L’activation est exécutée automatiquement par APIs LineDisplay PointOfService qui effectuent des e/s.

## <a name="code-sample-claim-and-enable"></a>Exemple de code: revendiquer et activer

Cet exemple montre comment revendiquer un scanneur de code-barres après avoir créé avec succès un objet scanneur de code-barres.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

> [!Warning]
> Une revendication peut se perdre dans les circonstances suivantes:
> 1. Une autre application a demandé une revendication du même périphérique et votre application n’a pas émis de **RetainDevice** en réponse à l'événement **ReleaseDeviceRequested**.  (Voir [Négociation de revendication](#Claim-negotiation) ci-dessous pour plus d’informations.)
> 2. Votre application a été suspendue, ce qui a entraîné la fermeture de l'objet de périphérique et par conséquent la revendication n’est plus valide. (Voir [Cycle de vie de l'objet appareil](pos-basics-deviceobject.md#device-object-lifecycle) pour plus d’informations.)


## <a name="claim-negotiation"></a>Négociation de revendication

Comme Windows est un environnement multitâche, il est possible que plusieurs applications sur le même ordinateur exigent un accès aux périphériques de manière coopérative.  Les API PointOfService fournissent un modèle de négociation qui permet à plusieurs applications de partager des périphériques connectés à l’ordinateur.

Lorsqu’une deuxième application sur le même ordinateur demande une revendication pour un périphérique PointOfService déjà réclamé par une autre application, une notification d’événements **ReleaseDeviceRequested** est publiée. L’application avec la revendication active doit répondre à la notification d’événements en appelant **RetainDevice** si elle utilise actuellement le périphérique afin d'éviter de perdre la revendication. 

Si l’application avec la revendication active ne répond pas avec **RetainDevice** immédiatement, il est supposé que l’application a été suspendue ou n’a pas besoin du périphérique. La revendication est alors révoquée et donnée à la nouvelle application. 

La première étape consiste à créer un gestionnaire d’événements qui répond à l’événement **ReleaseDeviceRequested** avec **RetainDevice**.  

```Csharp
    /// <summary>
    /// Event handler for the ReleaseDeviceRequested event which occurs when 
    /// the claimed barcode scanner receives a Claim request from another application
    /// </summary>
    void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
    {
        // Retain exclusive access to the device
        myScanner.RetainDevice();
    }
```

Puis inscrire le Gestionnaire d’événements en association avec votre périphérique revendiqué

```Csharp
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use 
        claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();

        if(claimedBarcodeScanner != null)
        {
            // register a release request handler to prevent loss of scanner during active use
            claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

            // after successful claim, enable scanner for data events to fire
            await claimedBarcodeScanner.EnableAsync();          
        }
        else
        {
            Debug.WriteLine("Failure to claim barcodeScanner");
        }
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
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
