---
title: "Déployer des profils de scanneurs de codes-barres avec GPM"
author: PatrickFarley
description: "Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM."
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.openlocfilehash: a63a09e64b6e2b935963a3f49ed7cbc6b82bdcef
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2017
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Déployer des profils de scanneurs de codes-barres avec GPM

**Remarque**  Cette fonctionnalité nécessite Windows10Mobile ou ultérieur.

Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM. Pour déployer les profils, utilisez *OemProfile* dans le [fournisseur CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025) pour les placer dans le dossier \\Data\\SharedData\\OEM\\Public\\Profile. Ces profils de scanneurs peuvent ensuite être utilisés par les fabricants de pilotes pour configurer les paramètres qui ne sont pas exposés par le biais de la surface d’API.

Microsoft ne définit pas les spécificités d’un profil de scanneur et n’indique pas comment les implémenter.

## <a name="related-topics"></a>Rubriques connexes
[Scanneur de codes-barres](barcode-scanner.md)

[Fournisseur CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025)