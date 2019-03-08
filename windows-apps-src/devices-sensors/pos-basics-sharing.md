---
title: Partage d’appareil PointOfService
description: Partage de périphériques de PointOfService avec d’autres utilisateurs
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618944"
---
# <a name="pointofservice-device-sharing"></a>Partage d’appareil PointOfService

Découvrez comment partager des périphériques Bluetooth connecté ou réseau avec d’autres ordinateurs dans un environnement où plusieurs PC s’appuient sur les périphériques partagés plutôt que des périphériques dédiés attachés à chaque ordinateur.

## <a name="device-sharing"></a>Partage de périphériques

Réseau et Bluetooth périphériques PointOfService connectés sont généralement utilisés dans un environnement wheere plusieurs périphériques clients partagent les mêmes périphériques pendant la journée.  Dans un environnement de vente au détail ou des services de produits alimentaires occupé tout retard dans la possibilité pour un appareil client à attacher à un périphérique a un impact sur l’efficacité dans laquelle un collaborateur peut fermer une transaction avec le client et passer à la suivante. Dans un scénario de restaurant rapide du service dans lequel une imprimante réception est utilisée comme une imprimante cuisine pour transférer les détails d’une commande client à la cuisine pour la préparation des il y aura plusieurs périphériques clients prendre les commandes des clients.  Une fois que la commande est terminée, chaque appareil client doit être en mesure de revendication de l’imprimante partagée et imprimer immédiatement l’ordre pour la cuisine.

Dans ces environnements, il est important pour l’application entièrement **dispose** l’appareil de l’objet afin qu’un autre peut réclamer le même appareil.

Mettre à disposition un PosPrinter à la fin d’un bloc 'using'

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


Mettre à disposition un PosPrinter en appelant explicitement de Dispose()

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Méthodes d’API utilisées 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
