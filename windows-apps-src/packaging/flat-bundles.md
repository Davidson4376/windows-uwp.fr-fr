---
author: laurenhughes
title: Packages d'application d'ensemble plat
description: Décrit comment créer un ensemble plat pour grouper les fichiers du package .appx de votre application avec des références vers vos packages d'application.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, création de packages, configuration de package, ensemble plat
ms.localizationpriority: medium
ms.openlocfilehash: 757f95a5f46bad6dbe650b4b552f3de486d84e1b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818318"
---
# <a name="flat-bundle-app-packages"></a>Packages d'application d'ensemble plat 

> [!IMPORTANT]
> Si vous avez l'intention de soumettre votre application au Store, vous devez contacter le [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) pour obtenir l’autorisation d’utiliser des ensembles plats.

Les ensembles plats constituent une manière améliorer de grouper les fichiers de package .appx de votre application. En général, les fichiers .appxbundle utilisent une structure de package multiniveau dans laquelle les fichiers de package .appx doivent être contenus dans un ensemble. Les ensembles plats suppriment cette nécessité par le simple référencement des fichiers de package .appx, ce qui leur permet de rester en dehors de l'ensemble d'applications. Dans la mesure où les fichiers de package .appx ne sont plus contenus dans l'ensemble, ils peuvent être traités en parallèle. Ainsi, le délai de chargement est réduit, la publication est accélérée (puisque chaque fichier de package .appx peut être traité en même temps), et les itérations de développement sont plus rapide.

![Diagramme d'ensemble plat](images/bundle-combined.png)

Les ensembles plats présentent d'autres avantages, notamment le besoin de créer moins de packages. Dans la mesure où les fichiers de package .appx sont uniquement référencés, deuxversions de l'application peuvent référencer le même fichier de package si le package est resté identique entre les deuxversions. Cela vous permet de créer uniquement les packages d'application qui ont été modifiés lors de la génération des packages pour la version suivante de votre application.
Par défaut, les ensembles plats référencent les fichiers de package .appx dans le même dossier que celui dans lequel ils se trouvent. Toutefois, cette référence peut être modifiée pour les autres chemins (chemins relatifs, réseaux partagés et emplacements HTTP). Pour ce faire, vous devez fournir directement un **BundleManifest** lors de la création de l'ensemble plat. 

## <a name="how-to-create-a-flat-bundle"></a>Procédure de création d'un ensemble plat

Les ensembles plats peuvent être créés à l'aide de l'outil MakeAppx.exe ou en utilisant la disposition de mise en package pour définir la structure de l'ensemble.

### <a name="using-makeappxexe"></a>Utilisation de MakeAppx.exe
Pour créer un ensemble plat à l'aide de MakeAppx.exe, utilisez la commande «MakeAppx.exe bundle» comme d'habitude, mais avec le commutateur /fb pour générer le fichier plat .appxbundle (qui est extrêmement petit dans la mesure où il faire uniquement référence aux packages .appx et ne contient aucune charge utile réelle). 

Voici un exemple de syntaxe de commande:

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

Pour en savoir plus sur l'utilisation de MakeAppx.exe, consultez [Créer un package d’application avec l’outil MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

### <a name="using-packaging-layout"></a>À l'aide de la disposition de mise en package
Sinon, vous pouvez créer un ensemble plat à l'aide de la disposition de mise en package. Pour ce faire, définissez l'attribut **FlatBundle** sur **true** dans l'élément **PackageFamily** du manifeste d'ensemble de votre application. Pour en savoir plus sur la disposition de mise en package, consultez [Création de package avec la disposition de mise en package](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Procédure de déploiement d'un ensemble plat 
Afin qu'un ensemble plat puisse être déployé, chacun des packages d'application (en plus de l'ensemble d'application) doit être signé avec le même certificat. En effet, tous les fichiers de package d'application (.appx) sont à présent indépendants et ne sont plus contenus dans le fichier d'ensemble plat (.appxbundle). Une fois les packages signés, utilisez l'[Applet de commande Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) dans PowerShell pour désigner le fichier .appxbundle et déployer l'application (en supposant que les packages d'application se trouvent là où l'ensemble d'application s'attend à les trouver). 