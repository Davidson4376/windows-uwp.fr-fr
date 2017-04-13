---
author: mukin
title: Scanneur de codes-barres
description: Cet article comporte des informations sur la famille de scanneurs de codes-barres de point de service.
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 8d9ef9bc08fa666c2af1348450f7a5fb0a0c7b65
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="barcode-scanner"></a>Scanneur de codes-barres
Permet aux développeurs d’applications d’accéder à des [scanneurs de codes-barres](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodescanner) pour récupérer les données décodées à partir d’une variété de symbologies de codes-barres comme le code-barres et les codes QR en fonction de la prise en charge du matériel. Reportez-vous à la classe [BarcodeSymbologies](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.barcodesymbologies) pour obtenir la liste complète des symbologies prises en charge.

## <a name="requirements"></a>Spécifications
Les applications qui requièrent cet espace de noms nécessitent l’ajout de l’élément «pointOfService» [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) au manifeste du package d’application.

## <a name="device-support"></a>Prise en charge des appareils

### <a name="usb"></a>USB

#### <a name="hidscanner"></a>HID.Scanner
Windows contient un pilote de classe de scanneur de codes-barres qui est basé sur la page d’utilisation HID.Scanner (8C) définie par USB.org. Ce pilote de classe prend en charge tout scanneur de codes-barres qui implémente cette norme, par exemple: Fabricant    Modèles Honeywell    1900GSR-2, 1200g-2 Intermec    SG20

Consultez le manuel de votre scanneur de codes-barres ou contactez son fabricant pour déterminer s’il peut être configuré comme un scanneur USB.HID.Scanner.

#### <a name="hidvendor-specific"></a>Propre à HID.Vendor
Windows prend en charge l’implémentation de pilotes propres au fournisseur pour prendre en charge des scanneurs de codes-barres supplémentaires. Contactez le fabricant de votre scanneur de codes-barres pour connaître la disponibilité si l’appareil n’est pas pris en charge par le scanneur USB.HID.Scanner intégré.

### <a name="bluetooth"></a>Bluetooth
#### <a name="serial-port-protocol-spp--simple-serial-interface-ssi"></a>Protocole SPP (Serial Port Protocol) – Interface SSI (Simple Serial Interface)
Windows prend en charge les scanneurs de codes-barres Bluetooth basés sur SPP-SSI.

| Fabricant |    Modèles |
|--------------|-----------|
| Socket Mobile |    **Série CHS7:** <br/> 7Ci1D - Scanneur de codes-barres imageur <br/> 7Di1D - Scanneur de codes-barres imageur durable <br/> 7Mi1D - Scanneur de codes-barres laser <br/> 7Pi1D - Scanneur de codes-barres laser durable <br/> **Série DuraScan700:** <br/> D7001D - Scanneur de codes-barres imageur <br/> D7301D - Scanneur de codes-barres laser <br/> **Série SocketScan800:** <br/> S8001D - Scanneur de codes-barres imageur (anciennement CHS8Ci) <br/> S8502D - Scanneur de codes-barres imageur (anciennement CHS8Qi)

## <a name="examples"></a>Exemples
Consultez l’exemple de scanneur de codes-barres pour un exemple d’implémentation.
+    [Exemple de scanneur de codes-barres](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
