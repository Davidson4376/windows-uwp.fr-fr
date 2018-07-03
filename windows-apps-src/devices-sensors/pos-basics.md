---
author: TerryWarwick
title: Prise en main de la technologie de point de service
description: Cet article comporte des informations sur la prise en main des API UWP PointOfService.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: a0583adbcef9e45dfe0b2e56e03ce7e0451ac5bb
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983542"
---
# <a name="getting-started-with-point-of-service"></a>Prise en main de la technologie de point de service

Cette section contient des rubriques qui sont communes à toutes les catégories d’appareils de point de service.

|Sujet |Description |
|------|------------|
| [Déclaration des fonctionnalités](pos-basics-capability.md)      | Découvrez comment ajouter la fonctionnalité **pointOfService** à votre manifeste d’application.  Cette fonctionnalité est requise pour l'utilisation de l’espace de noms Windows.Devices.PointOfService.  |
| [Énumération des appareils](pos-basics-enumerating.md)        | Découvrez comment définir un sélecteur d’appareils qui sert à interroger les appareils disponibles pour le système et comment utiliser ce sélecteur pour énumérer les appareils de point de service.  |
| [Création d'un objet appareil](pos-basics-deviceobject.md)  | Découvrez comment créer un objet appareil PointOfService qui vous permet d’accéder aux propriétés en lecture seule du périphérique et de revendiquer le périphérique pour une utilisation exclusive. |
| [Revendication de périphérique pour une utilisation exclusive ](pos-basics-claim.md)  | Découvrez comment réserver un périphérique pour une utilisation exclusive avec le modèle de revendication PointOfService, tout en permettant aux autres applications sur le même ordinateur d’accéder à l'appareil PointOfService lorsqu’ils ont besoin d’une utilisation exclusive.  |
|

## <a name="see-also"></a>Articles associés
[Prise en main de Windows.Devices.PointOfService](pos-get-started.md)


## <a name="sample-code"></a>Exemple de code
+ [Exemple de scanneur de code-barres](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemple de caisse enregistreuse]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemple d’affichage de ligne](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemple de lecteur de bande magnétique](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemple d’imprimante de POS](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

