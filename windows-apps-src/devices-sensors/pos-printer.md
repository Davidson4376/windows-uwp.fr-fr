---
author: mukin
title: Imprimante POS
description: "Cet article comporte des informations sur la famille d’imprimantes de point de service."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: d8340af651157bb6fae82785812f259c16d0a6c0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="pos-printer"></a>Imprimante POS

Permet aux développeurs d’applications d’imprimer sur des [imprimantes de reçus](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.posprinter) connectées au réseau et Bluetooth à l’aide du langage de contrôle d’imprimante ESC/POS Epson.

## <a name="requirements"></a>Spécifications
Les applications qui requièrent cet espace de noms nécessitent l’ajout de l’élément «pointOfService» [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) au manifeste du package d’application.

## <a name="device-support"></a>Prise en charge des appareils
Windows prend en charge la capacité d’imprimer sur des imprimantes de reçus connectées au réseau et Bluetooth à l’aide du langage de contrôle d’imprimante ESC/POS Epson. Pour plus d’informations sur ESC/POS, voir [Epson ESC/POS avec mise en forme](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/epson-esc-pos-with-formatting).

Alors que les classes, les énumérations et les interfaces exposées dans l’API prennent en charge les imprimantes de reçus, les imprimantes de bordereaux et les stations d’impressions de journaux, l’interface pilote prend en charge uniquement les imprimantes de reçus. À ce stade, toute tentative d’utilisation d’une imprimante de bordereaux ou d’une station d’impression de journaux renvoie un statut de défaut d’implémentation.

La prise en charge est actuellement limitée aux modèles d’appareils réseau et Bluetooth répertoriés dans les tableaux ci-dessous. Les imprimantes connectées par USB ne sont actuellement pas prises en charge. Consultez régulièrement nos ressources, afin de vous tenir informé de l’évolution de la prise en charge.

### <a name="stationary-pos-printers-network-bluetooth"></a>Imprimantes POS fixes (réseau, Bluetooth)
| Fabricant |    Modèles |
|--------------|-----------|
| Epson |    TM-T88V, TM-T70, TM-T20, TM-U220 |

### <a name="mobile-pos-printers-bluetooth"></a>Imprimantes POS mobiles (Bluetooth)
| Fabricant |    Modèles |
|--------------|-----------|
| Epson |    Mobilink P20 (TM-P20), Mobilink P60 (TM-P60), Mobilink P80 (TM-P80) |

## <a name="examples"></a>Exemples
Consultez l’exemple d’implémentation d’imprimante POS.
+ [Exemple d’imprimante POS](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
