---
author: TerryWarwick
title: Prise en main de la technologie de point de service
description: Cet article comporte des informations sur la prise en main des API UWP PointOfService.
ms.author: jken
ms.date: 06/13/2018
ms.topic: article
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 46dd1f615e42f6e89ee9a92cb980299e9a0e5205
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5882169"
---
# <a name="getting-started-with-point-of-service"></a>Prise en main de la technologie de point de service

## <a name="pointofservice-basics"></a>Notions de base PointOfService

Cette section contient des rubriques qui sont communes à toutes les catégories d’appareils de point de service.

|Sujet |Description |
|------|------------|
| [Déclaration des fonctionnalités](pos-basics-capability.md)      | Découvrez comment ajouter la fonctionnalité **pointOfService** à votre manifeste d’application.  Cette fonctionnalité est requise pour l'utilisation de l’espace de noms Windows.Devices.PointOfService.  |
| [Énumération des appareils](pos-basics-enumerating.md)        | Découvrez comment définir un sélecteur d’appareils qui sert à interroger les appareils disponibles pour le système et comment utiliser ce sélecteur pour énumérer les appareils de point de service.  |
| [Création d'un objet appareil](pos-basics-deviceobject.md)  | Découvrez comment créer un objet appareil PointOfService qui vous permet d’accéder aux propriétés en lecture seule du périphérique et de revendiquer le périphérique pour une utilisation exclusive. |
| [Revendication et activation ](pos-basics-claim.md)  | Découvrez comment réserver un périphérique pour une utilisation exclusive et activer pour les opérations d’e/s.  |
| [Partage de périphériques avec d’autres personnes](pos-basics-sharing.md) | Découvrez comment partager le réseau ou des périphériques Bluetooth connecté avec d’autres ordinateurs dans un environnement dans lequel plusieurs ordinateurs s’appuient sur des périphériques partagés plutôt que dédiés périphériques connectés à chaque ordinateur.
| [PointOfService de bout en bout](pos-get-started.md)  | Il s’agit d’un exemple de bout en bout illustrant comment interagir avec les périphériques PointOfService utilisant les exemples ci-dessus. |
|

## <a name="see-also"></a>Voir aussi

| Rubrique   | Description |
|:--------|:------------|
| [Distribution de l’application](../publish/distribute-lob-apps-to-enterprises.md) | Apprenez-en plus sur les options de distribution de votre application aux clients d’entreprise. |
| [Cycle de vie d’application](../launch-resume/app-lifecycle.md) | En savoir plus sur le cycle de vie d’une application UWP et que se passe-t-il quand Windows lance, suspend et reprend l’exécution de votre application. |
| [Ressources d’application](../app-resources/index.md) | Découvrez comment créer, mettre en package et consommer la chaîne de votre application, image et les ressources de fichiers. |
| [Liaison de données](../data-binding/index.md) | Découvrez comment utiliser la liaison de données pour afficher des données dans l’interface utilisateur de votre application. |
| [Énumération des appareils](enumerate-devices.md) | Découvrez les techniques de l’énumération d’utilisation avancée pour rechercher vos périphériques.|
| [Applications adaptatives de version](../debug-test-perf/version-adaptive-apps.md) | Apprenez à concevoir votre application afin qu’elle s’exécute sur plusieurs versions de Windows 10.|
|


## <a name="sample-code"></a>Exemple de code
+ [Exemple de scanneur de code-barres](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemple de caisse enregistreuse]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemple d’affichage de ligne](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemple de lecteur de bande magnétique](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemple d’imprimante de POS](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

