---
author: TerryWarwick
title: Utilisation des symbologies des scanneurs de codes-barres
description: Cet article contient des informations sur les symbologies de scanneur de code-barres.
ms.author: jken
ms.date: 05/3/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: f6e03d62316a1b842330f39ac958e4471a895815
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874523"
---
# <a name="working-with-symbologies"></a>Utilisation des symbologies
Une [Symbologie de code-barres](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) correspond au mappage des données dans un format spécifique de code-barres. Certaines symbologies courantes sont notamment UPC, Code 128, Code QR, etc.. Les APIS de scanneur de codes-barres UWP permettent à une application de contrôler la façon dont le scanneur traite ces symbologies sans configurer manuellement ce dernier. 

## <a name="determine-which-symbologies-are-supported"></a>Détermination des symbologies prises en charge 
Dans la mesure où votre application peut être utilisée avec des modèles de scanneur de codes-barres provenant de différents fabricants, vous pouvez souhaiter interroger le scanneur pour déterminer la liste des symbologies qu'il prend en charge.  Cela peut être utile si votre application requiert une symbologie particulière qui n'est pas forcément prise en charge par tous les scanneurs, ou si vous devez activer des symbologies qui ont été désactivées manuellement ou par programmation sur le scanneur.
Une fois que vous avez un objet [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) obtenu à l’aide de [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync), appelez [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) pour obtenir une liste des symbologies prises en charge par l’appareil.

## <a name="determine-if-a-specific-symbology-is-supported"></a>Déterminer si une symbologie particulière est prise en charge
Pour déterminer si le scanneur prend en charge une symbologie spécifique, vous pouvez appeler [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

## <a name="changing-which-symbologies-are-recognized"></a>Modifier les symbologies reconnues
Dans certains cas, vous voudrez peut-être utiliser un sous-ensemble des symbologies prises en charge par le scanneur de codes-barres.  Cela est particulièrement utile pour bloquer les symbologies que vous ne souhaitez pas utiliser dans votre application. Par exemple, pour garantir qu'un utilisateur scanne le code-barres approprié, vous pouvez restreindre la lecture aux symbologies UPC ou EAN lors de la lecture de références d'article, ou à Code128 lors de la lecture de numéros de série.
Une fois que vous connaissez les symbologies que votre scanneur prend en charge, vous pouvez définir les symbologies que vous souhaitez qu'il reconnaisse.  Cela est possible après avoir créé un objet [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) à l'aide de [ClaimScannerAsyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync). Vous pouvez appeler [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) pour activer un ensemble spécifique de symbologies, tandis que celles qui sont omises dans votre liste sont désactivées.

## <a name="restricting-scan-data-by-data-length"></a>Restriction des données de lecture par la longueur
Certaines symbologies sont de longueur variable, telles que Code 39 ou Code 128.  Les codes-barres de cette symbologie peuvent être situés près de données différentes, souvent de longueur spécifique. Définir la longueur spécifique des données dont vous avez besoin peut empêcher des lectures non valides.

| Méthode    | Description |
| :-------- | :---------- |
| [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | Vous permet de configurer une plage de longueur souhaitée des données décodées, ainsi que la façon dont le scanneur gère le chiffre de contrôle. |
| [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | Vous permet de récupérer la longueur réelle et de vérifier les configurations de chiffres. |

> [!Important] 
> Vérifiez que votre scanneur de codes-barres prend en charge l’utilisation des attributs de symbologie en vérifiant d’abord les propriétés suivantes: 
> - [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [ICheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)
