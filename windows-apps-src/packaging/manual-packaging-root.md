---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Packages d'application manuel
description: Cette section contient ou associe des articles sur la création manuelle de packages d’applications UWP.
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, uwp, création de packages
ms.localizationpriority: medium
ms.openlocfilehash: d90307560c315741751a5ab58ccdf0eaf35d97fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372794"
---
# <a name="manual-app-packaging"></a>Packages d'application manuel

Si vous souhaitez créer et signer un package d’application mais n’avez pas utilisé Visual Studio pour développer votre application, il vous faudra utiliser les outils de création manuelle de packages d’application.

> [!IMPORTANT] 
> Si vous avez utilisé Visual Studio pour développer votre application, nous vous recommandons d’utiliser l’Assistant Visual Studio pour créer et signer votre package d’application. Pour plus d’informations, voir [Créer un package d’application UWP avec Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="purpose"></a>Objectif

Cette section contient ou associe des articles sur la création manuelle de packages d’applications UWP.

| Rubrique | Description |
|-------|-------------|
| [Créer un package d’application avec l’outil MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe crée, chiffre, déchiffre et extrait les fichiers des packages d’application et des ensembles d’applications. |
| [Créer un certificat pour la signature du package](create-certificate-package-signing.md) | Créer et exporter un certificat pour la signature de packages d'applications à l'aide d'outils PowerShell. |
| [Signer un package d’application à l’aide de SignTool](sign-app-package-using-signtool.md) | Utilisez SignTool pour signer manuellement un package d'application à l'aide d'un certificat. |

### <a name="advanced-topics"></a>Rubriques avancées

Cette section contient des sujets plus avancés concernant l'agencement d'une application plus vaste et/ou plus complexe pour une mise en package et une installation plus efficaces. 

> [!IMPORTANT]
> Si vous avez l'intention de soumettre votre application au Store, vous devez contacter le [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) et obtenir l’autorisation d’utiliser une quelconque fonctionnalité avancée dans cette section.


| Rubrique | Description |
|-------|-------------|
| [Introduction aux packages d’actif](asset-packages.md) | Les packages d'actifs désignent un type de package qui agit en tant qu'emplacement centralisé pour les fichiers communs d'une application. Ainsi, la nécessité de dupliquer les fichiers au travers de ses packages d'architecture est efficacement éliminée. |
| [Développement avec les packages d’actif et le pliage de package](package-folding.md) | Découvrez comment organiser efficacement votre application avec des packages d'actifs et la mise en dossier des packages. |
| [Packages d’application bundle plat](flat-bundles.md) | Décrit comment créer un groupe plat pour les fichiers de package de votre application. |
| [Création d’un package avec la mise en page de l’empaquetage](packaging-layout.md) | La disposition de mise en package constitue un unique document décrivant la structure de mise en package de l'application. Il spécifie les ensembles d'une application (principaux et facultatifs), les packages contenus dans les ensembles ainsi que les fichiers contenus dans les packages. |
