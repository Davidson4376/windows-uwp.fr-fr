---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Création manuelle de packages d’application
description: Cette section contient ou associe des articles sur la création manuelle de packages d’applications UWP.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
keywords: windows10, uwp, création de packages
ms.localizationpriority: medium
ms.openlocfilehash: 0268e858ecbcaaee95796fa590d4a9994dcfb505
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6983032"
---
# <a name="manual-app-packaging"></a>Création manuelle de packages d’application

Si vous souhaitez créer et signer un package d’application mais n’avez pas utilisé Visual Studio pour développer votre application, il vous faudra utiliser les outils de création manuelle de packages d’application.

> [!IMPORTANT] 
> Si vous avez utilisé Visual Studio pour développer votre application, nous vous recommandons d’utiliser l’Assistant Visual Studio pour créer et signer votre package d’application. Pour plus d’informations, voir [Créer un package d’application UWP avec Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="purpose"></a>Objectif

Cette section contient ou associe des articles sur la création manuelle de packages d’applications UWP.

| Rubrique | Description |
|-------|-------------|
| [Créer un package d’application avec l’outil MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe crée, chiffre, déchiffre et extrait les fichiers des packages d’application et des ensembles d’applications. |
| [Créer un certificat pour la signature de packages](create-certificate-package-signing.md) | Créer et exporter un certificat pour la signature de packages d’applications à l’aide d’outilsPowerShell. |
| [Signer un package d’application à l’aide de SignTool](sign-app-package-using-signtool.md) | Utilisez SignTool pour signer manuellement un package d’application à l’aide d’un certificat. |

### <a name="advanced-topics"></a>Rubriques avancées

Cette section contient des sujets plus avancés concernant l'agencement d'une application plus vaste et/ou plus complexe pour une mise en package et une installation plus efficaces. 

> [!IMPORTANT]
> Si vous avez l'intention de soumettre votre application au Store, vous devez contacter le [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) et obtenir l’autorisation d’utiliser une quelconque fonctionnalité avancée dans cette section.


| Sujet | Description |
|-------|-------------|
| [Introduction aux packages d'actifs](asset-packages.md) | Les packages d'actifs désignent un type de package qui agit en tant qu'emplacement centralisé pour les fichiers communs d'une application. Ainsi, la nécessité de dupliquer les fichiers au travers de ses packages d'architecture est efficacement éliminée. |
| [Développement de packages d'actifs et mise en dossier de packages](package-folding.md) | Découvrez comment organiser efficacement votre application avec des packages d'actifs et la mise en dossier des packages. |
| [Packages d'application d'ensemble plat](flat-bundles.md) | Décrit comment créer un ensemble plat pour les fichiers de package de votre application. |
| [Création de package à l'aide de la disposition de mise en package](packaging-layout.md) | La disposition de mise en package constitue un unique document décrivant la structure de mise en package de l'application. Il spécifie les ensembles d'une application (principaux et facultatifs), les packages contenus dans les ensembles ainsi que les fichiers contenus dans les packages. |
