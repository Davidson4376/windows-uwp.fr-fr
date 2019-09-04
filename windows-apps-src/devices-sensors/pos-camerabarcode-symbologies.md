---
title: Symbologies des scanneurs de codes-barres à caméra
description: Symbologies prises en charge par les scanneurs de codes-barres à caméra
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: cc2aaaf4e9779cb2be712119fb1dacdf946952c5
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243328"
---
# <a name="symbologies"></a>Symbologies
Cette rubrique fournit des exemples de codes-barres pour chacun des symbologies pris en charge par le décodeur de codes-barres logiciel fourni avec Windows 10, notamment : UPC/EAN, code 39, Code 128, entrelacé 2 sur 5, barre omnidirectionnel, barre empilé, code QR et GS1DWCode.

## <a name="1d-symbologies"></a>Symbologies 1D

### <a name="code-39"></a>Code 39
![Exemple de code-barres - Code 39](images/pos/sample-barcode-code39.png)

### <a name="code-128"></a>Code 128
![Exemple de code-barres - Code 128](images/pos/sample-barcode-code128.png)

### <a name="databar-omnidirectional"></a>Databar Omnidirectional
![Exemple de code barres - Databar Omnidirectional](images/pos/sample-barcode-databar-omnidirectional.png) 
### <a name="databar-stacked"></a>Databar Stacked
![Exemple de code barres - Databar Stacked](images/pos/sample-barcode-databar-stacked.png)

### <a name="ean-8"></a>EAN-8
![Exemple de code-barres - EAN 8](images/pos/sample-barcode-ean8.png)

### <a name="ean-13"></a>EAN-13
![Exemple de code-barres - EAN 13](images/pos/sample-barcode-ean13.png)

### <a name="interleaved-2-of-5"></a>Code 2 parmi 5 entrelacé
![Exemple de code-barres - Code 2 parmi 5 entrelacé](images/pos/sample-barcode-interleaved-2-of-5.png)

### <a name="upc-a"></a>UPC-A
![Exemple de code-barres - UPC A](images/pos/sample-barcode-upca.png)

### <a name="upc-e"></a>UPC-E
![Exemple de code-barres - UPC E](images/pos/sample-barcode-upce.png)

## <a name="2d-symbologies"></a>Symbologies 2D
### <a name="qr-code"></a>Code QR
![Exemple de code-barres - Code QR](images/pos/sample-barcode-qrcode.png)

## <a name="digital-watermark"></a>Filigrane numérique
### <a name="gs1-dwcode"></a>GS1-DWCode

Scannez l’image d’un paquet ci-dessous avec votre application de scanneur de code-barres à caméra pour voir le code GS1DWCode en action.  Cette image est encodée avec UPCA 856107006854.  Rendez-vous sur http://www.digimarc.com pour plus d'informations sur les fonctionnalités du code GS1DWCode.

![Exemple de code-barres - GS1DWCode](images/pos/rice-box-v7.jpg)

> [!NOTE]
> Le décodeur logiciel intégré à Windows 10 est fourni avec l'autorisation de [*Digimarc Corporation*](https://www.digimarc.com/)

## <a name="see-also"></a>Voir aussi

### <a name="samples"></a>Exemples

- [Exemple de scanneur de codes-barres](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
