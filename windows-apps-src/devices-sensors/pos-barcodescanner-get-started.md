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
ms.openlocfilehash: ed0fa79f5bbdfdaf8ca1f3273fa8d741f17efe1d
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833150"
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

## <a name="working-with-symbologies"></a>Utilisation des symbologies
Une [**Symbologie de code-barres**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) correspond au mappage des données dans un format spécifique de code-barres. Certaines symbologies courantes sont notamment UPC, Code 128, Code QR, etc.. Les APIS de scanneur de codes-barres UWP permettent à une application de contrôler la façon dont le scanneur traite ces symbologies sans configurer manuellement ce dernier. 

### <a name="determine-which-symbologies-are-supported"></a>Détermination des symbologies prises en charge 
Dans la mesure où votre application peut être utilisée avec des modèles de scanneur de codes-barres provenant de différents fabricants, vous pouvez souhaiter interroger le scanneur pour déterminer la liste des symbologies qu'il prend en charge.  Cela peut être utile si votre application requiert une symbologie particulière qui n'est pas forcément prise en charge par tous les scanneurs, ou si vous devez activer des symbologies qui ont été désactivées manuellement ou par programmation sur le scanneur.
Une fois que vous avez un objet [**BarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) obtenu à l’aide de [**BarcodeScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), appelez [**GetSupportedSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) pour obtenir une liste des symbologies prises en charge par l’appareil.

### <a name="determine-if-a-specific-symbology-is-supported"></a>Déterminer si une symbologie particulière est prise en charge
Pour déterminer si le scanneur prend en charge une symbologie spécifique, vous pouvez appeler [**IsSymbologySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

### <a name="changing-which-symbologies-are-recognized"></a>Modifier les symbologies reconnues
Dans certains cas, vous voudrez peut-être utiliser un sous-ensemble des symbologies prises en charge par le scanneur de codes-barres.  Cela est particulièrement utile pour bloquer les symbologies que vous ne souhaitez pas utiliser dans votre application. Par exemple, pour garantir qu'un utilisateur scanne le code-barres approprié, vous pouvez restreindre la lecture aux symbologies UPC ou EAN lors de la lecture de références d'article, ou à Code128 lors de la lecture de numéros de série.
Une fois que vous connaissez les symbologies que votre scanneur prend en charge, vous pouvez définir les symbologies que vous souhaitez qu'il reconnaisse.  Cela est possible après avoir créé un objet [**ClaimedBarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) à l'aide de [**ClaimScannerAsyc**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Vous pouvez appeler [**SetActiveSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) pour activer un ensemble spécifique de symbologies, tandis que celles qui sont omises dans votre liste sont désactivées.

### <a name="restricting-scan-data-by-data-length"></a>Restriction des données de lecture par la longueur
Certaines symbologies sont de longueur variable, telles que Code 39 ou Code 128.  Les codes-barres de cette symbologie peuvent être situés près de données différentes, souvent de longueur spécifique. Définir la longueur spécifique des données dont vous avez besoin peut empêcher des lectures non valides.

| Méthode    | Description |
| :-------- | :---------- |
| [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | Vous permet de configurer une plage de longueur souhaitée des données décodées, ainsi que la façon dont le scanneur gère le chiffre de contrôle. |
| [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Vous permet de récupérer la longueur réelle et de vérifier les configurations de chiffres. |

> [!Important] 
> Vérifiez que votre scanneur de codes-barres prend en charge l’utilisation des attributs de symbologie en vérifiant d’abord les propriétés suivantes: [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) ou [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Vous permet de récupérer la longueur réelle et de vérifier les configurations de chiffres. :
> - [**IsDecodeLengthSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [**ICheckDigitTransmissionSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [**IsCheckDigitValidationSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

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
