---
title: Le partage d’appareils PointOfService
description: Le partage de périphériques PointOfService avec d’autres personnes
ms.date: 06/14/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8209302"
---
# <a name="pointofservice-device-sharing"></a>Le partage d’appareils PointOfService

Découvrez comment partager le réseau ou des périphériques Bluetooth connecté avec d’autres ordinateurs dans un environnement dans lequel plusieurs ordinateurs s’appuient sur des périphériques partagés plutôt que dédiés périphériques connectés à chaque ordinateur.

## <a name="device-sharing"></a>Le partage d’appareils

Réseau et Bluetooth connectés des périphériques PointOfService sont généralement utilisées dans un environnement wheere plusieurs appareils clients sont partage les mêmes périphériques tout au long de la journée.  Dans un environnement de vente au détail ou services cuisine occupé tout retard dans la possibilité pour un appareil client pour s’attacher à un périphérique a un impact sur l’efficacité dans lesquels une entreprise associée peut fermer une transaction avec le client et passer à la suivante. Dans un scénario de restaurant rapide de service dans lequel une imprimante de reçus est utilisée comme une imprimante de cuisine transférer les détails de commande d’un client à la cuisine de préparation seront plusieurs appareils clients en prenant les commandes des clients.  Une fois que la commande est terminée, chaque appareil client doit être en mesure d’imprimer immédiatement la commande pour la cuisine revendiquer l’imprimante partagée.

Dans ces environnements, il est important pour l’application à entièrement **Supprimer** l’objet appareil respecter afin que l’autre peut revendiquer le même appareil.

Suppression d’un PosPrinter à la fin d’un bloc «using»

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


Suppression d’un PosPrinter en appelant la méthode Dispose() explicitement

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>Méthodes de l’API utilisées 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
