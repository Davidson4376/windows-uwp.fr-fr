---
title: Fonctionnalité d'appareil PointOfService
description: La fonctionnalité PointOfService est requise pour l'utilisation de l’espace de noms Windows.Devices.PointOfService.
ms.date: 05/1/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 832881919a3ed1f44c0912703ede5948c5b10d11
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8872819"
---
# <a name="pointofservice-device-capability"></a>Fonctionnalité d'appareil PointOfService
Vous demandez l’accès aux API PointOfService en déclarant la fonctionnalité dans votre manifeste de package d’application. Vous pouvez déclarer la plupart des fonctionnalités à l’aide du concepteur de manifeste, dans MicrosoftVisual Studio, ou vous pouvez les ajouter manuellement.  

> [!Important]
> Vous recevez l’erreur **System.UnauthorizedAccessException** si vous essayez d’utiliser une API dans l’espace de noms Windows.Devices.PointOfService sans avoir déclaré la fonctionnalité **pointOfService** dans votre manifeste d'application. 

## <a name="declare-capability-using-manifest-designer"></a>Déclarez la fonctionnalité à l’aide du Concepteur de manifeste

1. Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2. Double-cliquez sur le fichier **Package.appxmanifest**.  
*Si le fichier manifeste est déjà ouvert dans le mode code XML, Visual Studio vous invite à fermer le fichier.*
3. Cliquez sur l’onglet **Fonctionnalités**.
4. Cliquez sur la case à cocher en regard de **Point de service** dans la liste des fonctionnalités pour activer la fonctionnalité d'appareil de point de service


## <a name="declare-capability-manually"></a>Déclarer la fonctionnalité manuellement

1. Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2. Cliquez avec le bouton droit sur le fichier **Package.appxmanifest** et sélectionnez **Afficher le code**
3. Ajoutez l’élément PointOfService DeviceCapability dans la section Fonctionnalités de votre manifeste d’application.  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
