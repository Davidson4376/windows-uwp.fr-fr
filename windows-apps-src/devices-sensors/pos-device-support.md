---
author: mukin
title: Prise en charge des appareils de PDV
description: "Cet article fournit des informations sur la prise en charge des appareils pour chaque famille d’appareils de PDV."
ms.author: mukin
ms.date: 05/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 58018385082f7f7e49edba0351d919bc840ade05
ms.sourcegitcommit: 53930c9871461f6106f785ae4fabb2296eb359f1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2017
---
# <a name="pos-device-support"></a>Prise en charge des appareils de PDV

## <a name="barcode-scanner"></a>Scanneur de code-barres
| Connectivité | Prise en charge |
| -------------|-------------|
| USB          | <p>Windows fournit un pilote pour les scanneurs de code-barres connectés par USB, basé sur la spécification du tableau d’utilisation des scanneurs pour point de vente (PDV) (8c) définie par [USB.org](http://www.usb.org/developers/hidpage/). Pour connaître la liste des appareils compatibles connus, reportez-vous au tableau ci-après.  Consultez le manuel de votre scanneur de code-barres ou contactez le fabricant du scanneur pour déterminer si ce dernier peut être configuré en mode Scanneur USB.HID.POS. </p><p>Windows prend également en charge l’implémentation des pilotes spécifiques du fournisseur pour gérer des scanneurs de code-barres supplémentaires qui ne reconnaissent pas la norme de scanneur USB.HID.POS. Pour connaître la disponibilité des pilotes spécifiques du fournisseur, contactez le fabricant de votre scanneur de code-barres.</p>|
| Bluetooth    | <p>Windows prend en charge les scanneurs de code-barres Bluetooth basés sur SPP-SSI. Pour connaître la liste des appareils compatibles connus, reportez-vous au tableau ci-après.</p> |

### <a name="compatible-hardware"></a>Matériel compatible
| Catégorie | Connectivité | Fabricant/modèle |
|--------------|-----------|-----------|
| **Scanneurs à main 1D** | **USB** |HoneywellVoyager1200g<br/>HoneywellVoyager1202g<br/>HoneywellVoyager1202-bf<br/>HoneywellVoyager145Xg (peut être mis à niveau)|
| **Scanneurs à main 1D** | **Bluetooth** |Socket Mobile CHS7Ci<br/> Socket Mobile CHS7Di<br/> Socket Mobile CHS7Mi<br/> Socket Mobile CHS7Pi<br/>Socket Mobile DuraScanD700<br/> Socket Mobile DuraScanD730<br/>Socket Mobile SocketScanS800 (anciennement CHS8Ci) <br/>|
|**Scanneurs à main 2D** | **USB** |Code Reader™950<br/>Code Reader™1021<br/>Code Reader™1421<br/> Honeywell Granit198Xi<br/>Honeywell Granit191Xi<br/>Honeywell Xenon1900g<br/>Honeywell Xenon1902g<br/>Honeywell Xenon1902g-bf<br/>Honeywell Xenon1900h<br/>Honeywell Xenon1902h<br/>HoneywellVoyager145Xg (peut être mis à niveau)<br/>Honeywell Voyager1602g<br/>IntermecSG20|
|**Scanneurs à main 2D** | **Bluetooth** |Socket Mobile SocketScanS850 (anciennement CHS8Qi)|
| **Scanneurs de présentation** | **USB** |Code Reader™5000<br/>Honeywell Genesis7580g<br/>Honeywell Orbit7190g|
| **Scanneurs en caisse** | **USB** |Honeywell Stratos2700|
| **Moteurs de numérisation** | **USB** | HoneywellN5680<br/>HoneywellN3680|
| **Appareils mobiles Windows**| **Intégrée** |BluebirdEF400<br/>BluebirdEF500<br/>BluebirdEF500R<br/>HoneywellCT50<br/>HoneywellD75e<br/>JanamXT2<br/>PanasonicFZ-E1<br/>PanasonicFZ-F1<br/>PointMobilePM80<br/>ZebraTC700j|
| **Appareils mobiles Windows**| **Personnalisée** | HP EliteX3 avec protection pour scanneur de code-barres |

## <a name="cash-drawer"></a>Caisse enregistreuse
| Connectivité | Prise en charge |
| -------------|-------------|
| Réseau/Bluetooth | <p> La connexion directe à la caisse enregistreuse peut être établie via le réseau ou par le biais de Bluetooth, selon les fonctionnalités de la caisse enregistreuse. </p>|
| Port DK | <p> Les caisses enregistreuses qui sont dépourvues de fonctionnalités Bluetooth ou réseau peuvent être connectées par le biais du portDK sur une imprimante de PDV prise en charge, ou de l’accessoire DK-AirCash de StarMicronics. </p>
| OPOS    | <p> Prend en charge tous les appareils de caisse enregistreuse qui reconnaissent les pilotes OPOS et/ou fournit les objets de service OPOS. Installez les pilotes OPOS conformément aux instructions d’installation des fabricants des appareils concernés. Cette opération assure une connectivité de type USB ou autre basée sur les spécifications du fabricant. </p> |

**Remarque:** Pour plus d’informations sur l’accessoire DK-AirCash, contactez StarMicronics.

### <a name="networkbluetooth"></a>Réseau/Bluetooth
| Fabricant |    Modèles |
|--------------|-----------|
| Caisse enregistreuse APG |    NetPRO, BluePRO |

## <a name="line-display"></a>Affichage de ligne
Prend en charge tous les appareils d’affichage de ligne qui reconnaissent les pilotes OPOS et/ou fournit les objets de service OPOS. Installez les pilotes OPOS conformément aux instructions d’installation des fabricants des appareils concernés.

## <a name="magnetic-stripe-reader"></a>Lecteur de bande magnétique

Windows fournit un pilote pour les lecteurs de bande magnétique connectés par USB, basé sur la spécification du tableau d’utilisation des scanneurs pour point de vente (PDV) (8c) définie par [USB.org](http://www.usb.org/developers/hidpage/).

### <a name="vendor-specific-support"></a>Prise en charge propre aux fournisseurs
Windows fournit une prise en charge pour les lecteurs de bande magnétique ci-après de Magtek et d’IDtech, en fonction des ID fournisseur et produit (VID/PID).

| Fabricant |     Modèles |    Numéro d’article |
|--------------|-----------|--------------|
| IDTech | SecureMag (ID fournisseur: 0ACD - ID produit: 2010) | IDRE-3x5xxxx |
| |    MiniMag (ID fournisseur: 0ACD ID produit: 0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (ID fournisseur: 0801 ID produit: 0011) |    210730xx |
| |    Dynamag (ID fournisseur: 0801 ID produit: 0002) |    210401xx |

### <a name="custom-vendor-specific"></a>Produits personnalisés spécifiques au fournisseur
Windows supporte l’implémentation de pilotes supplémentaires spécifiques aux fournisseurs, pour la prise en charge d’autres lecteurs de bande magnétique. Vérifiez la disponibilité auprès de votre fabricant de lecteur de bande magnétique.

## <a name="pos-printer"></a>Imprimante de PDV
Windows prend en charge la capacité d’imprimer sur des imprimantes de reçus connectées au réseau et Bluetooth à l’aide du langage de contrôle d’imprimante ESC/POS Epson. Pour plus d’informations sur ESC/POS, voir [Epson ESC/POS avec mise en forme](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting).

Alors que les classes, les énumérations et les interfaces exposées dans l’API prennent en charge les imprimantes de reçus, les imprimantes de bordereaux et les stations d’impressions de journaux, l’interface pilote prend uniquement en charge les imprimantes de reçus. À ce stade, toute tentative d’utilisation d’une imprimante de bordereaux ou d’une station d’impression de journaux renvoie un statut de défaut d’implémentation.

| Connectivité | Prise en charge |
| -------------|-------------|
| Réseau/Bluetooth | <p> La connexion directe à l’imprimante de PDV peut être établie par le biais du réseau ou de Bluetooth, selon les fonctionnalités de l’imprimante de PDV. </p>|
| OPOS    | <p> Prend en charge toutes les imprimantes de PDV qui reconnaissent les pilotes OPOS et/ou fournit les objets de service OPOS. Installez les pilotes OPOS conformément aux instructions d’installation des fabricants des appareils concernés. Cette opération assure une connectivité de type USB ou autre basée sur les spécifications du fabricant. </p> |

### <a name="stationary-pos-printers-networkbluetooth"></a>Imprimantes de PDV fixes (réseau/Bluetooth)
| Fabricant |    Modèles |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>Imprimantes POS mobiles (Bluetooth)
| Fabricant |    Modèles |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |