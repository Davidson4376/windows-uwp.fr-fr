---
author: TerryWarwick
title: Prise en charge du matériel de point de service
description: Cet article contient des informations sur la prise en charge matérielle pour chacune des classes de périphérique de point de service
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb2468497115c9595f6fd17ab61b30caed507ab
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832093"
---
# <a name="supported-point-of-service-peripherals"></a>Périphériques de point de service pris en charge

## <a name="barcode-scanner"></a>Scanneur de code-barres
| Connectivité | Prise en charge |
| -------------|-------------|
| USB          | <p>Windows contient un pilote de classe prêt à l'emploi pour les scanneurs de codes-barres connectés par USB. Il repose sur la spécification du tableau d’utilisation des scanneurs pour point de service (POS) HID (8c) définie par [USB.org](http://www.usb.org/developers/hidpage/). Consultez le tableau ci-dessous pour obtenir la liste des appareils compatibles connus.  Consultez le manuel de votre scanneur de code-barres ou contactez le fabricant pour savoir comment configurer votre scanneur en mode **Scanneur USB.HID.POS**. </p><p>Windows prend également en charge l’implémentation des pilotes spécifiques du fournisseur pour gérer des scanneurs de code-barres supplémentaires qui ne reconnaissent pas la norme de scanneur USB.HID.POS. Pour connaître la disponibilité des pilotes spécifiques du fournisseur, contactez le fabricant de votre scanneur de code-barres.</p><p>Les fabricants de scanneur de code-barres peuvent consulter le [Guide de conception de pilote de scanneur de code-barres](https://aka.ms/pointofservice-drv) pour en savoir plus sur la création d’un pilote de scanneur de code-barres personnalisé</p> |
| Bluetooth    | <p>Windows prend en charge les scanneurs de codes-barres Bluetooth basés sur le protocole et l'interface SPP-SSI (Serial Port Protocol - Simple Serial Interface). Pour connaître la liste des appareils compatibles connus, reportez-vous au tableau ci-après. Consultez le manuel de votre scanneur de codes-barres ou contactez son fabricant pour savoir comment le configurer en mode **SPP-SSI**.</p> |
| Webcam       | <p>À partir de Windows10, version1803, vous pouvez lire des codes-barres via un objectif de caméra standard à partir d’une application Windows universelle. Il est recommandé d'utiliser une caméra qui prend en charge la mise au point automatique et une résolution minimale de 1920 x 1440.  Certaines caméras à résolution plus faible peuvent lire les codes-barres standards s'ils sont imprimés suffisamment grands.  Les codes-barres comportant des éléments plus fins peuvent nécessiter des caméras d'une résolution supérieure.</p>| 
|


### <a name="compatible-barcode-scanners"></a>Scanneurs de code-barres compatibles
| Catégorie | Connectivité | Fabricant/modèle |
|--------------|-----------|-----------|
| **Scanneurs à main 1D** | **USB** |HoneywellVoyager1200g<br/>HoneywellVoyager1202g<br/>HoneywellVoyager1202-bf<br/>HoneywellVoyager145Xg (peut être mis à niveau)|
| **Scanneurs à main 1D** | **Bluetooth** |Socket Mobile CHS7Ci<br/> Socket Mobile CHS7Di<br/> Socket Mobile CHS7Mi<br/> Socket Mobile CHS7Pi<br/>Socket Mobile DuraScanD700<br/> Socket Mobile DuraScanD730<br/>Socket Mobile SocketScanS800 (anciennement CHS8Ci) <br/>|
|**Scanneurs à main 2D** | **USB** |Code Reader™950<br/>Code Reader™1021<br/>Code Reader™1421<br/> Honeywell Granit198Xi<br/>Honeywell Granit191Xi<br/>Honeywell Xenon1900g<br/>Honeywell Xenon1902g<br/>Honeywell Xenon1902g-bf<br/>Honeywell Xenon1900h<br/>Honeywell Xenon1902h<br/>HoneywellVoyager145Xg (peut être mis à niveau)<br/>Honeywell Voyager1602g<br/>IntermecSG20<br/>Zebra DS2278<br/>Zebra DS8108 ¹<hr><small>¹ Microprogramme minimum 016 (2018.01.18) requis. Mise à jour possible à l’aide de [123Scan](http://www.zebra.com/123Scan)</small>|
|**Scanneurs à main 2D** | **Bluetooth** |Socket Mobile SocketScanS850 (anciennement CHS8Qi)|
| **Scanneurs de présentation** | **USB** |Code Reader™5000<br/>Honeywell Genesis7580g<br/>Honeywell Orbit7190g|
| **Scanneurs en caisse** | **USB** |Honeywell Stratos2700|
| **Moteurs de numérisation** | **USB** | HoneywellN5680<br/>HoneywellN3680|
| **Appareils mobiles Windows**| **Intégrée** |BluebirdEF400<br/>BluebirdEF500<br/>BluebirdEF500R<br/>HoneywellCT50<br/>HoneywellD75e<br/>JanamXT2<br/>PanasonicFZ-E1<br/>PanasonicFZ-F1<br/>PointMobilePM80<br/>ZebraTC700j|
| **Appareils mobiles Windows**| **Personnalisée** | HP EliteX3 avec protection pour scanneur de code-barres |


## <a name="cash-drawer"></a>Caisse enregistreuse
| Connectivité | Prise en charge |
| -------------|-------------|
| Réseau/Bluetooth | <p> La connexion directe à la caisse enregistreuse peut être établie via le réseau ou par le biais de Bluetooth, selon les fonctionnalités de la caisse enregistreuse. </p><p>Caisse enregistreuse APG: NetPRO, BluePRO</p> |
| Port DK | <p> Les caisses enregistreuses qui sont dépourvues de fonctionnalités Bluetooth ou réseau peuvent être connectées par le biais du portDK sur une imprimante de reçus prise en charge, ou de l’accessoire DK-AirCash de StarMicronics. </p>
| OPOS    | <p> Prend en charge toutes les caisses enregistreuses compatibles OPOS par le biais des objets de service OPOS fournis par le fabricant. Installez les pilotes OPOS conformément aux instructions d’installation des fabricants des appareils. </p> |


## <a name="customer-display-linedisplay"></a>Customer Display (LineDisplay)
Prend en charge tous les affichages de ligne compatibles OPOS par le biais des objets de service OPOS fournis par le fabricant. Installez les pilotes OPOS conformément aux instructions d’installation des fabricants des appareils.

## <a name="magnetic-stripe-reader"></a>Lecteur de bande magnétique
Windows fournit une prise en charge pour les lecteurs de bande magnétique ci-après de Magtek et d’IDtech, en fonction des ID fournisseur et produit (VID/PID).

| Fabricant |    Modèles |  Numéro d’article |
|--------------|-----------|--------------|
| IDTech | SecureMag (ID fournisseur: 0ACD - ID produit: 2010) | IDRE-3x5xxxx |
| | MiniMag (ID fournisseur: 0ACD ID produit: 0500) |   IDMB-3x5xxxx |
| Magtek | MagneSafe (ID fournisseur: 0801 ID produit: 0011) |  210730xx |
| | Dynamag (ID fournisseur: 0801 ID produit: 0002) |   210401xx |

 Windows supporte l’implémentation de pilotes supplémentaires spécifiques aux fournisseurs, pour la prise en charge d’autres lecteurs de bande magnétique. Vérifiez la disponibilité auprès de votre fabricant de lecteur de bande magnétique. Les fabricants de lecteur de bande magnétique peuvent consulter le [Guide de conception de pilote de lecteur de bande magnétique](https://aka.ms/pointofservice-drv) pour en savoir plus sur la création d’un pilote de lecteur de bande magnétique personnalisé.

## <a name="receipt-printer-posprinter"></a>Imprimante de reçus (POSPrinter)
| Connectivité | Support |
| -------------|-------------|
| Réseau et Bluetooth | <p>Windows prend en charge les imprimantes de reçus connectées au réseau et Bluetooth à l’aide du langage de contrôle d’imprimante ESC/POS Epson.  Les imprimantes répertoriées ci-dessous sont détectées automatiquement à l’aide des API POSPrinter. D'autres imprimantes de reçus qui fournissent une émulation ESC/POS peuvent également fonctionner, mais doivent être associées à l’aide un processus de [couplage hors-bande](https://aka.ms/pointofservice-oobpairing).</p><p>Remarque: les stations de tickets et de feuilles ne sont pas prises en charge par le biais de cette méthode.</p> |
| OPOS    | <p> Prend en charge toutes les imprimantes de reçus compatibles OPOS via des objets de service OPOS. Installez les pilotes OPOS conformément aux instructions d’installation des fabricants des appareils. </p> |

### <a name="stationary-receipt-printers-networkbluetooth"></a>Imprimantes de reçus fixes (réseau/Bluetooth)
| Fabricant |    Modèles |
|--------------|-----------|
| Epson |   TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-receipt-printers-bluetooth"></a>Imprimantes de reçus mobiles (Bluetooth)
| Fabricant |    Modèles |
|--------------|-----------|
| Epson |   Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |
