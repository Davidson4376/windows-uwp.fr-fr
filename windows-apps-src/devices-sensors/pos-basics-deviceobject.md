---
author: TerryWarwick
title: Objets appareil PointOfService
description: En savoir plus sur la création d’objets appareils PointOfService
ms.author: jken
ms.date: 06/4/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: eaaeeae3e21549510258ee9370ef6ffb0d9f9020
ms.sourcegitcommit: 633dd07c3a9a4d1c2421b43c612774c760b4ee58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "1976730"
---
# <a name="pointofservice-device-objects"></a>Objets appareil PointOfService

## <a name="creating-a-device-object"></a>Création d'un objet appareil
Une fois que vous avez identifié l'appareil PointOfService que vous souhaitez utiliser, soit à partir d’une nouvelle énumération, soit d'un ID d’appareil stocké, vous appelez simplement [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) avec le [**DeviceID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) que vous avez choisi par programme ou que l’utilisateur a sélectionné pour créer un objet appareil de point de service.

Cet exemple tente de créer un nouvel objet BarcodeScanner avec FromIdAsync à l’aide d’un ID d’appareil. Si un échec se produit lors de la création de l’objet, un message de débogage est écrit.

```Csharp
using windows.devices.enumeration;

try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);
}
catch (Exception ex)
{
    Debug.WriteLine("Failure: - " + ex.Message);
}
```

Une fois que vous avez un objet appareil, vous pouvez accéder aux méthodes, aux propriétés et aux événements de l'appareil.  

## <a name="device-object-lifecycle"></a>Cycle de vie de l'objet appareil
Avant Windows8, les applications avaient un cycle de vie simple. Les applications Win32 et .NET sont en cours d’exécution ou pas et les périphériques PointOfService ont été généralement revendiqués pour le cycle de vie complet de l’application. Lorsqu’un utilisateur les réduit ou les ferme, elles continuent de s’exécuter. Cela ne posait aucun problème jusqu’à ce que les appareils mobiles et la gestion de l’alimentation prennent une importance croissante.

Windows8 a mis en place un nouveau modèle d’application, avec les applications UWP. Globalement, un état suspendu a été ajouté. Une application UWP est suspendue, lorsque l’utilisateur la réduit ou bascule vers une autre application. Autrement dit, les threads de l’application sont arrêtés et l’application reste en mémoire, sauf si le système d’exploitation a besoin de récupérer des ressources et tous les objets appareils représentant des périphériques PointOfService sont automatiquement fermés pour autoriser d’autres applications à accéder aux périphériques. Lorsque l’utilisateur revient à l’application, celle-ci peut rapidement reprendre un état d’exécution et restaurer les connexions des périphériques PointOfService, à condition que celles-ci soient toujours disponibles à la reprise.

Vous pouvez détecter la fermeture d’un objet pour une raison quelconque avec un <DeviceObject>. Le gestionnaire de l'événement Fermé note l’ID de l'appareil pour rétablir la connexion à l’avenir.   Vous pouvez également souhaiter gérer cette situation sur une notification de Suspension d'une application pour enregistrer l'ID de l'appareil afin de rétablir les connexions du périphérique sur une notification de Reprise d'une application.  Assurez-vous de ne pas doubler les gestionnaires d’événements et de ne pas dupliquer les actions pour l’objet appareil sur les deux <DeviceObject>.Fermé et Suspension d'une application.

> [!TIP]
> Reportez-vous aux rubriques suivantes pour plus d’informations sur le cycle de vie d’une application de plateforme Windows universelle (UWP) Windows10:
> - [Cycle de vie d’une application de plateforme Windows universelle (UWP) Windows10](../launch-resume/app-lifecycle.md)
> - [Gérer la suspension d’une application](../launch-resume/suspend-an-app.md)
> - [Gérer la reprise d’une application](../launch-resume/resume-an-app.md)
