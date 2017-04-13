---
author: mukin
title: "Lecteur de bande magnétique"
description: "Cet article comporte des informations sur la famille d’appareils de point de service à lecteur de bande magnétique."
ms.author: wdg-dev-content
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: a11fe7a63c0444ac986e7bfe0d50472249e5196e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="magnetic-stripe-reader"></a>Lecteur de bande magnétique

Permet aux développeurs d’applications d’accéder aux [lecteurs de bande magnétique](https://docs.microsoft.com/en-us/uwp/api/windows.devices.pointofservice.magneticstripereader) pour récupérer des données des cartes compatibles comme les cartes de crédit, les cartes de fidélité, les cartes d’accès, etc.

## <a name="requirements"></a>Spécifications
Les applications qui requièrent cet espace de noms nécessitent l’ajout de l’élément «pointOfService» [DeviceCapability](https://msdn.microsoft.com/library/4353c4fd-f038-4986-81ed-d2ec0c6235ef) au manifeste du package d’application.

## <a name="device-support"></a>Prise en charge des appareils
### <a name="usb"></a>USB
### <a name="supported-vendor-specific"></a>Prise en charge spécifique aux fournisseurs
Windows fournit une prise en charge pour les lecteurs de bande magnétique suivants de Magtek et d’IDtech, en fonction des ID fournisseur et produit.

| Fabricant |     Modèles |    Numéro d’article |
|--------------|-----------|--------------|
| IDTech | SecureMag (ID fournisseur: 0ACD - ID produit: 2010) | IDRE-3x5xxxx |
| |    MiniMag (ID fournisseur: 0ACD ID produit: 0500) |    IDMB-3x5xxxx |
| Magtek | MagneSafe (ID fournisseur: 0801 ID produit: 0011) |    210730xx |
| |    Dynamag (ID fournisseur: 0801 ID produit: 0002) |    210401xx |

### <a name="custom-vendor-specific"></a>Produits personnalisés spécifiques au fournisseur
Windows supporte l’implémentation de pilotes supplémentaires spécifiques aux fournisseurs, pour la prise en charge d’autres lecteurs de bande magnétique. Vérifiez la disponibilité auprès de votre fabricant de lecteur de bande magnétique.

## <a name="examples"></a>Exemples
À titre d’exemple d’implémentation, consultez cette instance de lecteur de bande magnétique.
+    [Exemple de lecteur de bande magnétique](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
