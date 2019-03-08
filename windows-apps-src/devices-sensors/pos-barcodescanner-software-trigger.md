---
title: Utiliser un déclencheur logiciel
description: Découvrez comment contrôler le fait de l’analyse à partir de logiciels.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 2b6f06ea66767a1bcdd7e20fa05aa7af275eb892
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645524"
---
# <a name="use-a-software-trigger"></a>Utiliser un déclencheur logiciel

Il peut être utile de contrôler l'action de lecture à partir d'un logiciel, si vous utilisez un scanneur de codes-barres en mode de présentation, ou si le scanneur ne comporte pas de déclencheur physique, comme un scanneur de code-barres reposant sur une caméra. Vous pouvez lancer le processus d’analyse en appelant [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

Selon la valeur de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) scanner le code-barres qu’une seule puis arrêter ou le scanneur peut analyser en continu jusqu'à ce que vous appeliez [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Définissez la valeur souhaitée de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) pour contrôler le comportement de scanneur quand un code-barres est décodé.

| Valeur | Description |
| ----- | ----------- |
| True   | Lire un seul code-barres puis s'arrêter |
| False  | Lire des codes-barres en continu sans interruption |


> [!Important]
> Vérifiez que votre scanneur de codes-barres prend en charge l’utilisation du déclencheur de logiciel en vérifiant d’abord la propriété [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

L’exemple suivant montre comment lancer l’analyse à l’aide d’un déclencheur de logiciels, qui arrête une fois qu’il analyse un code-barres d’analyse :

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]