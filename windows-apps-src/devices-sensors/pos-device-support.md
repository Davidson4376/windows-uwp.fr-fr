---
author: TerryWarwick
title: Prise en charge du matériel de point de service
description: Cet article contient des informations sur la prise en charge matérielle pour chacune des classes de périphérique de point de service
ms.author: jken
ms.date: 06/13/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df6e2c15260759f164a37b68365e0268633b22d5
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5747164"
---
# <a name="supported-point-of-service-peripherals"></a>Périphériques de point de service pris en charge

## <a name="barcode-scanner"></a>Scanneur de code-barres
| Connectivité | Prise en charge |
| -------------|-------------|
| USB          | <p>Windows contient un pilote de classe prêt à l'emploi pour les scanneurs de codes-barres connectés par USB. Il repose sur la spécification du tableau d’utilisation des scanneurs pour point de service (POS) HID (8c) définie par [USB.org](http://www.usb.org/developers/hidpage/). Consultez le tableau ci-dessous pour obtenir la liste des appareils compatibles connus.  Consultez le manuel de votre scanneur de code-barres ou contactez le fabricant pour savoir comment configurer votre scanneur en mode **Scanneur USB.HID.POS**. </p><p>Windows prend également en charge l’implémentation des pilotes spécifiques du fournisseur pour gérer des scanneurs de code-barres supplémentaires qui ne reconnaissent pas la norme de scanneur USB.HID.POS. Pour connaître la disponibilité des pilotes spécifiques du fournisseur, contactez le fabricant de votre scanneur de code-barres.</p><p>Les fabricants de scanneur de code-barres peuvent consulter le [Guide de conception de pilote de scanneur de code-barres](https://aka.ms/pointofservice-drv) pour en savoir plus sur la création d’un pilote de scanneur de code-barres personnalisé</p> |
| Bluetooth    | <p>Windows prend en charge les scanneurs de codes-barres Bluetooth basés sur le protocole et l'interface SPP-SSI (Serial Port Protocol - Simple Serial Interface). Pour connaître la liste des appareils compatibles connus, reportez-vous au tableau ci-après. Consultez le manuel de votre scanneur de codes-barres ou contactez son fabricant pour savoir comment le configurer en mode **SPP-SSI**.</p> |
| Webcam       | <p>À partir de Windows10, version1803, vous pouvez lire des codes-barres via un objectif de caméra standard à partir d’une application Windows universelle. Il est recommandé d'utiliser une caméra qui prend en charge la mise au point automatique et une résolution minimale de 1920 x 1440.  Certaines caméras à résolution plus faible peuvent lire les codes-barres standards s'ils sont imprimés suffisamment grands.  Les codes-barres comportant des éléments plus fins peuvent nécessiter des caméras d'une résolution supérieure.</p>| 
|


| Fabricant  | Modèle                          | Fonctionnalité | Connexion    | Type         | Mode                      |
|---------------|--------------------------------|------------|--------------|--------------|---------------------------|
| Code          | Reader™ 950                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Code          | Reader™™ 1021                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Code          | Reader™™ 1421                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Code          | Reader™ 5000                   | 2D         | USB          | Présentation | Scanneur de PDV HID           |
| Honeywell     | Genesis 7580g                  | 2D         | USB          | Présentation | Scanneur de PDV HID           |
| Honeywell     | Granit 198Xi                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Granit 191Xi                   | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | N5680                          | 2D         | Interne     | Composant    | Scanneur de PDV HID           |
| Honeywell     | N3680                          | 2D         | Interne     | Composant    | Scanneur de PDV HID           |
| Honeywell     | Orbite 7190g                    | 2D         | USB          | Présentation | Scanneur de PDV HID           |
| Honeywell     | Stratos 2700                   | 2D         | USB          | Dans la zone compteur   | Scanneur de PDV HID           |
| Honeywell     | Voyager 1200g                  | 1D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 1202g                  | 1D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 1202-bf                | 1D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 145Xg                  | 1D / 2D ¹   | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Voyager 1602g                  | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Xenon 1900g                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Xenon 1902g                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Xenon 1902g-bf                 | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Xenon 1900h                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Honeywell     | Xenon 1902h                    | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| HP            | Scanneur de codes-barres de valeur (HR2150) | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Intermec      | SG20                           | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Socket Mobile | CHS 7Ci                        | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | CHS 7Di                        | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | CHS 7mi                        | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | CHS 7Pi                        | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | CHS 8Ci                        | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | DuraScan D700                  | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | DuraScan D730                  | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | DuraScan D740                  | 2D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | SocketScan S700                | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | SocketScan S730                | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | SocketScan S740                | 2D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | SocketScan S800                | 1D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Socket Mobile | SocketScan S850                | 2D         | Bluetooth    | Poche     | Profil de Port série (SPP) |
| Zebra         | DS2278                         | 2D         | USB          | Poche     | Scanneur de PDV HID           |
| Zebra         | DS8108²                        | 2D         | USB          | Poche     | Scanneur de PDV HID           |
|


¹ mise à jour pour prendre en charge des codes-barres 2D via Honeywell <br/>
² le microprogramme minimum 016 (2018.01.18) requis. Mise à jour à l’aide de Zebra [123Scan](http://www.zebra.com/123Scan). 


<hr>

### <a name="windows-devices-with-built-in-barcode-scanner"></a>Appareils Windows avec le scanneur de codes-barres intégré
| Fabricant   | Modèle | Systèmed’exploitation |
|----------------|-------|------------------|
| Innowi         | ChecOut-M | Windows10   |

### <a name="windows-mobile-devices-with-built-in-barcode-scanner"></a>Appareils Windows Mobile avec scanneur de codes-barres intégré
| Fabricant   | Modèle | Systèmed’exploitation |
|----------------|-------|------------------|
| Bluebird       | EF400 | Windows Mobile   |
| Bluebird       | EF500 | Windows Mobile   |
| Bluebird       | EF500R | Windows Mobile   |
| Honeywell      | CT50   | Windows Mobile   |
| Honeywell      | D75e | Windows Mobile   |
| Janam          | XT2      | Windows Mobile   |
| Panasonic      | FZ-E1 | Windows Mobile   |
| Panasonic      | FZ-F1 |Windows Mobile   |
| PointMobile    | PM80 | Windows Mobile   |
| Zebra          | TC700j | Windows Mobile   |
| HP             | X3 Elite veste | Windows Mobile   |




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
