---
author: mukin
title: Caisse enregistreuse
description: Cet article comporte des informations sur la famille de caisses enregistreuses de point de service.
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 
ms.openlocfilehash: 376272356cf720ddd9519f0077e771a1016abb1e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="cash-drawer"></a>Caisse enregistreuse

Permet aux développeurs d’applications d’interagir avec les [caisses enregistreuses](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.cashdrawer).

## <a name="requirements"></a>Spécifications
Les applications qui requièrent cet espace de noms nécessitent l’ajout de l’élément «pointOfService» [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) au manifeste du package d’application.

## <a name="device-support"></a>Prise en charge des appareils
La connexion directe à la caisse enregistreuse peut être établie via le réseau ou par le biais de Bluetooth, selon les fonctionnalités de la caisse enregistreuse. En outre, les caisses enregistreuses qui sont dépourvues de fonctionnalités Bluetooth ou réseau peuvent être connectées via le port DK sur une imprimante POS prise en charge, ou l’accessoire DK-AirCash de Star Micronics. Pour le moment, les caisses enregistreuses connectées via un port USB ou série ne sont pas prises en charge.

**Remarque:**  pour plus d’informations sur l’accessoire DK-AirCash, contactez Star Micronics.

### <a name="networkbluetooth"></a>Réseau/Bluetooth
| Fabricant |    Modèles |
|--------------|-----------|
| Caisse enregistreuse APG |    NetPRO, BluePRO |

## <a name="examples"></a>Exemples
Consultez l’exemple de caisse enregistreuse pour un exemple d’implémentation.
+    [Exemple de caisse enregistreuse](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
