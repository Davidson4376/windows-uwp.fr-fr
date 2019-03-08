---
title: Prise en main de la technologie de point de service
description: Cet article comporte des informations sur la prise en main des API UWP PointOfService.
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 1b4ff9443c40cf44e171bf898b627de3e2819034
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656464"
---
# <a name="getting-started-with-point-of-service"></a>Prise en main de la technologie de point de service

## <a name="pointofservice-basics"></a>Notions de base PointOfService

Cette section contient des rubriques qui sont communes à toutes les catégories d’appareils de point de service.

|Rubrique |Description |
|------|------------|
| [Déclaration de fonction](pos-basics-capability.md)      | Découvrez comment ajouter la fonctionnalité **pointOfService** à votre manifeste d’application.  Cette fonctionnalité est requise pour l'utilisation de l’espace de noms Windows.Devices.PointOfService.  |
| [Énumération des périphériques](pos-basics-enumerating.md)        | Découvrez comment définir un sélecteur d’appareils qui sert à interroger les appareils disponibles pour le système et comment utiliser ce sélecteur pour énumérer les appareils de point de service.  |
| [Création d’un objet de l’appareil](pos-basics-deviceobject.md)  | Découvrez comment créer un objet appareil PointOfService qui vous permet d’accéder aux propriétés en lecture seule du périphérique et de revendiquer le périphérique pour une utilisation exclusive. |
| [Revendication et activer ](pos-basics-claim.md)  | Découvrez comment réserver une PointOfService périphérique pour une utilisation exclusive et activer pour les opérations d’e/s.  |
| [Partage de périphériques avec d’autres utilisateurs](pos-basics-sharing.md) | Découvrez comment partager des périphériques Bluetooth connecté ou réseau avec d’autres ordinateurs dans un environnement où plusieurs PC s’appuient sur les périphériques partagés plutôt que des périphériques dédiés attachés à chaque ordinateur.
| [PointOfService end-to-end](pos-get-started.md)  | Il s’agit d’un exemple de bout en bout montrant comment interagir avec les périphériques PointOfService utilisant les exemples ci-dessus. |
|

## <a name="see-also"></a>Voir également

| Rubrique   | Description |
|:--------|:------------|
| [Distribution de l’application](../publish/distribute-lob-apps-to-enterprises.md) | En savoir plus sur les options de distribution de votre application aux clients d’entreprise. |
| [Cycle de vie des applications](../launch-resume/app-lifecycle.md) | En savoir plus sur le cycle de vie d’une application UWP et que se passe-t-il quand Windows démarre, suspend et reprend votre application. |
| [Ressources d’application](../app-resources/index.md) | Découvrez comment créer, package et de consommer la chaîne de votre application, l’image et les ressources de fichier. |
| [Liaison de données](../data-binding/index.md) | Découvrez comment utiliser la liaison de données pour afficher des données dans l’interface utilisateur de votre application. |
| [Énumération du périphérique](enumerate-devices.md) | Découvrez les techniques d’énumération utilisation avancée pour rechercher des périphériques.|
| [Applications évolutives de version](../debug-test-perf/version-adaptive-apps.md) | Apprenez à concevoir votre application afin qu’elle s’exécute sur plusieurs versions de Windows 10.|
|


## <a name="sample-code"></a>Exemple de code
+ [Exemple de scanneur de codes-barres](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Exemple de tiroir de trésorerie]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Exemple d’affichage de ligne](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Exemple de lecteur de bande magnétique](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Exemple de POSPrinter](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

