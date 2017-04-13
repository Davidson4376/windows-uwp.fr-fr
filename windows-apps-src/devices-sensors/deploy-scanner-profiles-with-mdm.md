---
title: "Déployer des profils de scanneurs de codes-barres avec GPM"
author: mukin
description: "Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM."
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.openlocfilehash: 51d3b90dd7f202fd86285bb3f95c78ded4a8b6d8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Déployer des profils de scanneurs de codes-barres avec GPM

**Remarque**  Cette fonctionnalité nécessite Windows10Mobile ou ultérieur.

Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM. Pour déployer les profils, utilisez *OemProfile* dans le [fournisseur CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025) pour les placer dans le dossier \\Data\\SharedData\\OEM\\Public\\Profile. Ces profils de scanneurs peuvent ensuite être utilisés par les fabricants de pilotes pour configurer les paramètres qui ne sont pas exposés par le biais de la surface d’API.

Microsoft ne définit pas les spécificités d’un profil de scanneur et n’indique pas comment les implémenter.

## <a name="related-topics"></a>Rubriques connexes
[Scanneur de codes-barres](barcode-scanner.md)

[Fournisseur CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025)