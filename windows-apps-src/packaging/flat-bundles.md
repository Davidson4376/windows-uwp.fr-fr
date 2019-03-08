---
title: Packages d'application d'ensemble plat
description: Décrit comment créer un ensemble plat pour grouper les fichiers du package .appx de votre application avec des références vers vos packages d'application.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, création de packages, configuration de package, ensemble plat
ms.localizationpriority: medium
ms.openlocfilehash: b7066b7f2e5bd72ebee3169e03c7940b6fef4dba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638484"
---
# <a name="flat-bundle-app-packages"></a>Packages d'application d'ensemble plat 

> [!IMPORTANT]
> Si vous avez l'intention de soumettre votre application au Store, vous devez contacter le [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) pour obtenir l’autorisation d’utiliser des ensembles plats.

Offres groupées plates constituent un moyen amélioré pour regrouper les fichiers de package de votre application. Un fichier de groupement d’application utilise une structure de packaging à plusieurs niveaux dans lequel les fichiers de package d’application doivent se trouver dans le groupe de Windows classique, offres groupées plates supprimer ce besoin en référençant uniquement les fichiers de package d’application, ce qui leur permet d’être en dehors de l’offre groupée d’applications. Dans la mesure où les fichiers de package d’application ne se trouvent plus dans le groupe, ils peuvent être parallèles traité, ce qui entraîne des réduit le temps, plus rapide de publication (étant donné que chaque fichier de package d’application peut être traitée en même temps) de téléchargement et finalement plus rapide développement itérations.

![Diagramme d'ensemble plat](images/bundle-combined.png)

Les ensembles plats présentent d'autres avantages, notamment le besoin de créer moins de packages. Dans la mesure où les fichiers de package d’application sont uniquement référencés, les deux versions de l’application peuvent référencer le fichier de package même si le package n’a pas changé entre les deux versions. Cela vous permet de créer uniquement les packages d'application qui ont été modifiés lors de la génération des packages pour la version suivante de votre application.
Par défaut, les regroupements plats référencera des fichiers de package d’application dans le même dossier que lui-même. Toutefois, cette référence peut être modifiée pour les autres chemins (chemins relatifs, réseaux partagés et emplacements HTTP). Pour ce faire, vous devez fournir directement un **BundleManifest** lors de la création de l'ensemble plat. 

## <a name="how-to-create-a-flat-bundle"></a>Procédure de création d'un ensemble plat

Les ensembles plats peuvent être créés à l'aide de l'outil MakeAppx.exe ou en utilisant la disposition de mise en package pour définir la structure de l'ensemble.

### <a name="using-makeappxexe"></a>Utilisation de MakeAppx.exe
Pour créer un regroupement plat à l’aide de MakeAppx.exe, utilisez la commande « MakeAppx.exe bundle » comme d’habitude, mais avec le commutateur /fb pour générer le fichier d’offre groupée application plat (qui sera extrêmement faible, car elle fait référence aux fichiers de package d’application uniquement et que vous ne contient-elle pas les charges utiles de réels ). 

Voici un exemple de syntaxe de commande :

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

Pour en savoir plus sur l'utilisation de MakeAppx.exe, consultez [Créer un package d’application avec l’outil MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

### <a name="using-packaging-layout"></a>À l'aide de la disposition de mise en package
Sinon, vous pouvez créer un ensemble plat à l'aide de la disposition de mise en package. Pour ce faire, définissez l'attribut **FlatBundle** sur **true** dans l'élément **PackageFamily** du manifeste d'ensemble de votre application. Pour en savoir plus sur la disposition de mise en package, consultez [Création de package avec la disposition de mise en package](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Procédure de déploiement d'un ensemble plat 
Afin qu'un ensemble plat puisse être déployé, chacun des packages d'application (en plus de l'ensemble d'application) doit être signé avec le même certificat. Il s’agit, car tous les fichiers de package d’application (.appx/.msix) sont désormais des fichiers indépendants et ne sont pas contenues dans le fichier de groupement (.appxbundle/.msixbundle) d’application plus. Une fois que les packages sont signés, utilisez le [applet de commande Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) dans PowerShell pour pointer vers le fichier de groupement d’application et de déployer l’application (en supposant que des packages d’application sont où le bundle d’applications les oblige à être). 