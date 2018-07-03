---
author: TerryWarwick
title: Prise en main du scanneur de code-barres
description: Découvrez comment interagir avec un scanneur de codes-barres à partir d’une application Windows universelle
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 0fddfa3aa78c274735634315b1230b0893020805
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874197"
---
# <a name="getting-started-with-barcode-scanners"></a>Prise en main du scanneur de code-barres

Découvrez comment interagir avec un scanneur de codes-barres à partir d’une application Windows universelle.  Cette rubrique fournit des informations sur les fonctionnalités spécifiques des scanneurs de code-barres.

## <a name="configuring-your-barcode-scanner"></a>Configuration de votre scanneur de code-barres
Les scanneurs de codes-barres peuvent être configurés selon plusieurs modes différents.  Il est important que votre scanneur de codes-barres soit configuré correctement pour l’application prévue.  De nombreux scanneurs de codes-barres peuvent être configurés en mode **insertion clavier** qui fait paraître le scanneur de codes-barres comme un clavier à Windows.  Cela vous permet de scanner des codes-barres dans des applications qui ne sont pas spécifiquement dédiées à cette activité, comme le Bloc-notes.  Lorsque vous scannez un code-barres dans ce mode, les données décodées du scanneur de codes-barres sont insérées au point d’insertion comme si vous les tapiez au clavier.  Si vous souhaitez mieux contrôler votre scanneur de codes-barres à partir de votre application UWP, vous devez le configurer dans un mode autre que l'insertion clavier.

### <a name="usb-barcode-scanner"></a>Scanneur de codes-barres USB
Un scanneur de code-barres connectée par USB doit être configuré en mode **Scanneur de PDV HID** pour fonctionner avec le pilote de scanneur de code-barres qui est inclus dans Windows. Ce pilote est une implémentation de la spécification **Tables d'utilisation de point de vente HID** publiée sur [**USB-HID**](http://www.usb.org/developers/hidpage/).  Reportez-vous à la documentation de votre scanneur de code-barres ou contactez son fabricant pour obtenir les instructions d'activation du mode **Scanneur de PDV HID**.  Une fois configuré comme un **Scanneur de PDV HID**, votre scanneur de codes-barres apparaît dans le Gestionnaire de périphériques sous le nœud **Scanneur de codes-barres de PDV** en tant que **Scanneur de codes-barres de PDV HID**.
Le fabricant de votre scanneur de code-barres peut également avoir un pilote spécifique au fournisseur qui prend en charge les API de scanneur de code-barres UWP dans mode autre que **Scanneur de PDV HID**.  Si vous avez déjà installé un pilote fourni par le fabricant compatible avec les API de scanneur de code-barres UWP, vous pouvez voir un périphérique spécifique au fournisseur répertorié sous **Scanneur de codes-barres de PDV** dans le Gestionnaire de périphériques.

### <a name="bluetooth-barcode-scanner"></a>Scanneur de codes-barres Bluetooth
Un scanneur connecté par Bluetooth doit être configuré dans le mode **Protocole Port série - Interface série simple (SPP-SSI)** pour fonctionner avec les API de scanneur de code-barres UWP.  Reportez-vous à la documentation de votre scanneur de code-barres ou contactez son fabricant pour obtenir les instructions d'activation du **mode SPP-SSI**.  
Avant de pouvoir utiliser votre scanneur de codes-barres Bluetooth, vous devez l’associer: Paramètres - Périphériques - Bluetooth et autres périphériques - Ajouter un périphérique Bluetooth ou autre.  
Vous pouvez lancer et contrôler la séquence d'association en utilisant l'espace de noms **Windows.Devices.Enumeration**.  Voir [**Coupler des appareils**](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices) pour plus d'informations.

## <a name="using-software-trigger-with-barcode-scanners"></a>Utiliser un déclencheur logiciel avec des scanneurs de codes-barres
### <a name="initiate-scan-from-software"></a>Lancer la lecture à partir d'un logiciel
Il peut être utile de contrôler l'action de lecture à partir d'un logiciel, si vous utilisez un scanneur de codes-barres en mode de présentation, ou si le scanneur ne comporte pas de déclencheur physique, comme un scanneur de code-barres reposant sur une caméra. Vous pouvez lancer le processus de lecture en appelant [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync).  
En fonction de la valeur de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived), le scanneur peut lire un seul code-barres puis s'arrêter, ou lire en continu jusqu'à ce que vous appeliez [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync).

Définissez la valeur souhaitée de [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) pour contrôler le comportement du scanneur à chaque décodage de code-barres.

| Valeur | Description |
| ----- | ----------- |
| Vrai   | Lire un seul code-barres puis s'arrêter |
| Faux  | Lire des codes-barres en continu sans interruption |


> [!Important]
> Vérifiez que votre scanneur de codes-barres prend en charge l’utilisation de déclencheurs logiciels en vérifiant d’abord la propriété [**IsSoftwareTriggerSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported).
