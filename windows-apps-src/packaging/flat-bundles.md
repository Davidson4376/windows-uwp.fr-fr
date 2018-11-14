---
author: laurenhughes
title: Packages d'application d'ensemble plat
description: Décrit comment créer un ensemble plat pour grouper les fichiers du package .appx de votre application avec des références vers vos packages d'application.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows10, création de packages, configuration de package, ensemble plat
ms.localizationpriority: medium
ms.openlocfilehash: b877996dd5fa32ac764fb587092f501320931527
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6656707"
---
# <a name="flat-bundle-app-packages"></a>Packages d'application d'ensemble plat 

> [!IMPORTANT]
> Si vous avez l'intention de soumettre votre application au Store, vous devez contacter le [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) pour obtenir l’autorisation d’utiliser des ensembles plats.

Plats constituent une manière améliorer de grouper des fichiers de package de votre application. Un Windows classique fichier un ensemble d’applications de l’application utilise une structure de package multiniveau dans lequel les fichiers de package d’application doivent être contenus dans l’ensemble d’applications, des ensembles plats suppriment cette nécessité en référençant uniquement les fichiers de package d’application, ce qui permet d’être en dehors de l’ensemble d’applications. Dans la mesure où les fichiers de package d’application sont ne sont plus contenus dans l’ensemble, ils peuvent être traités en parallèles, qui entraîne le chargement d’une publication plus rapide, en temps (étant donné que chaque fichier de package d’application peut être traité en même temps) est réduit et plus développement itérations.

![Diagramme d'ensemble plat](images/bundle-combined.png)

Les ensembles plats présentent d'autres avantages, notamment le besoin de créer moins de packages. Dans la mesure où les fichiers de package d’application sont uniquement référencés, deux versions de l’application peuvent référencer le même fichier de package si le package n’a pas changé entre les deux versions. Cela vous permet de créer uniquement les packages d'application qui ont été modifiés lors de la génération des packages pour la version suivante de votre application.
Par défaut, les ensembles plats référencent se fichiers de package d’application dans le même dossier que lui-même. Toutefois, cette référence peut être modifiée pour les autres chemins (chemins relatifs, réseaux partagés et emplacements HTTP). Pour ce faire, vous devez fournir directement un **BundleManifest** lors de la création de l'ensemble plat. 

## <a name="how-to-create-a-flat-bundle"></a>Procédure de création d'un ensemble plat

Les ensembles plats peuvent être créés à l'aide de l'outil MakeAppx.exe ou en utilisant la disposition de mise en package pour définir la structure de l'ensemble.

### <a name="using-makeappxexe"></a>Utilisation de MakeAppx.exe
Pour créer un ensemble plat à l’aide de MakeAppx.exe, utilisez la commande «MakeAppx.exe bundle» comme d’habitude, mais avec le commutateur /fb pour générer le fichier d’un ensemble d’applications application plat (ce qui est extrêmement petit dans la mesure où il fait référence aux fichiers de package d’application et ne contient-elle aucune charge utile réelle d’uniquement ). 

Voici un exemple de syntaxe de commande:

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

Pour en savoir plus sur l'utilisation de MakeAppx.exe, consultez [Créer un package d’application avec l’outil MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

### <a name="using-packaging-layout"></a>À l'aide de la disposition de mise en package
Sinon, vous pouvez créer un ensemble plat à l'aide de la disposition de mise en package. Pour ce faire, définissez l'attribut **FlatBundle** sur **true** dans l'élément **PackageFamily** du manifeste d'ensemble de votre application. Pour en savoir plus sur la disposition de mise en package, consultez [Création de package avec la disposition de mise en package](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Procédure de déploiement d'un ensemble plat 
Afin qu'un ensemble plat puisse être déployé, chacun des packages d'application (en plus de l'ensemble d'application) doit être signé avec le même certificat. Il s’agit, car tous les fichiers de package d’application (.appx/.msix) sont à présent indépendants et ne figurent pas dans le fichier un ensemble d’applications (.appxbundle/.msixbundle) plus. Une fois les packages signés, utilisez l' [applet de commande Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) dans PowerShell pour pointer vers le fichier de lot d’application et de déployer l’application (en supposant que des packages d’application sont là où l’ensemble d’applications s’attend à les rendre). 