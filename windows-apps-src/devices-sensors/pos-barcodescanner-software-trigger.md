---
author: eliotcowley
title: Utiliser un déclencheur logiciel
description: Découvrez comment contrôler l’action de l’analyse de logiciel.
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: ddd8ec979cb6d5a72b48b9b8b6a60adb73c35657
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3981608"
---
# <a name="use-a-software-trigger"></a>Utiliser un déclencheur logiciel

Il peut être utile de contrôler l'action de lecture à partir d'un logiciel, si vous utilisez un scanneur de codes-barres en mode de présentation, ou si le scanneur ne comporte pas de déclencheur physique, comme un scanneur de code-barres reposant sur une caméra. Vous pouvez lancer le processus de lecture en appelant [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).

En fonction de la valeur de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived), le scanneur peut lire un seul code-barres puis s'arrêter, ou lire en continu jusqu'à ce que vous appeliez [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Définissez la valeur souhaitée de [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) pour contrôler le comportement du scanneur à chaque décodage de code-barres.

| Valeur | Description |
| ----- | ----------- |
| Vrai   | Lire un seul code-barres puis s'arrêter |
| Faux  | Lire des codes-barres en continu sans interruption |


> [!Important]
> Vérifiez que votre scanneur de codes-barres prend en charge l’utilisation de déclencheurs logiciels en vérifiant d’abord la propriété [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).

L’exemple suivant montre comment initialiser le balayage à l’aide d’un déclencheur logiciel, qui cessera de rechercher une fois qu’il analyse un code-barre:

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