---
title: Déployer des profils de scanneurs de codes-barres avec GPM
author: PatrickFarley
description: Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.author: pafarley
ms.date: 09/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef7f1029573d2ff98e744ceb44b108a67a7c0d0b
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018082"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Déployer des profils de scanneurs de codes-barres avec GPM

**Remarque**  Cette fonctionnalité nécessite Windows10Mobile ou ultérieur.

Les profils de scanneurs de codes-barres peuvent être déployés avec un serveur GPM. Pour déployer les profils, utilisez *OemProfile* dans le [fournisseur CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025) pour les placer dans le dossier \\Data\\SharedData\\OEM\\Public\\Profile. Ces profils de scanneurs peuvent ensuite être utilisés par les fabricants de pilotes pour configurer les paramètres qui ne sont pas exposés par le biais de la surface d’API.

Microsoft ne définit pas les spécificités d’un profil de scanneur et n’indique pas comment les implémenter.

## <a name="related-topics"></a>Rubriques connexes
- [Fournisseur CSP EnterpriseExtFileSystem](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [Prise en charge des périphériques de l’analyseur code-barres](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)