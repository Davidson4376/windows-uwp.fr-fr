---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Packages facultatifs et création d’ensembles connexes
description: Les packages facultatifs intègrent du contenu qui peut être inclus dans un package principal. Ils sont utiles pour le contenu téléchargeable (DLC), pour diviser une application volumineuse en cas de restrictions de taille, ou pour distribuer un contenu supplémentaire indépendamment de votre application d’origine.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, packages facultatifs, ensemble connexe, empaqueter une extension, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 4864bdaa1f32b980c5c8b159ca71bb6a56da4ec5
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4693354"
---
# <a name="optional-packages-and-related-set-authoring"></a>Packages facultatifs et création d’ensembles connexes
Les packages facultatifs intègrent du contenu qui peut être inclus dans un package principal. Celles-ci sont utiles pour diviser une application pour les restrictions de taille, le contenu téléchargeable (DLC), ou à distribuer un contenu supplémentaire distinct à partir de votre application d’origine.

Ensembles connexes sont une extension de packages facultatifs, ils permettent d’appliquer un ensemble strict des versions parmi les packages facultatifs et les principaux. Elles vous permettent également de charger du code natif (C++) des packages facultatifs. 

## <a name="prerequisites"></a>Conditions préalables

- Visual Studio 2017, version 15.1
- Windows10, version1703
- Windows 10, version 1703 SDK

Pour obtenir tous les derniers outils de développement, consultez les [téléchargements et outils pour Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Pour soumettre une application qui utilise des packages facultatifs et/ou des ensembles connexes dans le Microsoft Store, vous devez l’autorisation. S’ils ne sont pas soumis au Store, les packages facultatifs et ensembles connexes peuvent servir pour les applications métier (LOB) ou de l’entreprise sans autorisation du centre de développement. Voir [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) pour obtenir l’autorisation de soumettre une application qui utilise des packages facultatifs et ensembles connexes.

### <a name="code-sample"></a>Exemple de code
Pendant que vous lisez cet article, il est recommandé que vous procédez ainsi que l' [exemple de code de package facultatif](https://github.com/AppInstaller/OptionalPackageSample) sur GitHub pour une connaissance pratique de packages facultatifs comment et liées à des jeux de travail au sein de Visual Studio.

## <a name="optional-packages"></a>Packages facultatifs
Pour créer un package facultatif dans Visual Studio, vous devez:
1. Assurez-vous de votre application **Min Version de la plateforme cible** est défini sur: 10.0.15063.0.
2. À partir de votre projet de **package principal** , ouvrez le `Package.appxmanifest` fichier. Accédez à l’onglet «Packages» et prenez note de votre **nom de famille de package**, qui est tout ce qui précède le caractère «_».
3. À partir de votre projet de **package facultatif** , bouton droit sur le `Package.appxmanifest` et sélectionnez **Ouvrir avec > Éditeur XML (texte)**.
4. Recherchez le `<Dependencies>` élément dans le fichier. Ajoutez la ligne suivante:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Remplacer `[MainPackageDependency]` avec votre **nom de famille de package** à l’étape 2. Cela spécifiera que votre **package facultatif** dépend de votre **package principal**.

Une fois que vous avez votre dépendances du package configurer des étapes 1 à 4, vous pouvez continuer à développer comme vous le feriez normalement. Si vous souhaitez charger du code à partir du package facultatif dans le package principal, vous devez créer un ensemble connexe. Consultez la section [associé définit](#related_sets) pour plus d’informations.

Visual Studio peut être configuré pour redéployer votre package principal à chaque fois que vous déployez un package facultatif. Pour définir la dépendance de génération dans Visual Studio, vous devez:

- Cliquez avec le bouton droit sur le projet de package facultatif et sélectionnez **dépendances de Build > dépendances du projet …**
- Le projet de package principal, sélectionnez «OK». 

Désormais, chaque fois que vous entrez F5 ou que vous générez un projet de package facultatif, Visual Studio générez le projet de package principal tout d’abord. Cela permet de garantir que votre projet principal et les projets facultatives sont synchronisés.

## Ensembles connexes<a name="related_sets"></a>

Si vous souhaitez charger du code à partir d’un package facultatif dans le package principal, vous devez créer un ensemble connexe. Pour générer un ensemble connexe, votre package principal et le package facultatif doivent être étroitement. Les métadonnées pour les ensembles connexes sont spécifiée dans le fichier .appxbundle ou .msixbundle du package principal. Visual Studio vous permet d’obtenir les métadonnées correcte dans vos fichiers. Pour configurer la solution de votre application pour des ensembles connexes, procédez comme suit:

1. Cliquez avec le bouton droit sur le projet de package principal, sélectionnez **Ajouter > nouvel élément …**
2. À partir de la fenêtre, recherchez les modèles installés pour «.txt» et ajoutez un nouveau fichier de texte.
> [!IMPORTANT]
> Le nouveau fichier texte doit être nommé: `Bundle.Mapping.txt`.
3. Dans le `Bundle.Mapping.txt` fichier que vous allez spécifier les chemins d’accès relatifs à des projets de package facultatif ou les packages externes. Un échantillon `Bundle.Mapping.txt` fichier doit ressembler à ceci:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Lorsque votre solution est configurée de cette façon, Visual Studio va créer un manifeste de l’ensemble d’applications pour le package principal à toutes les métadonnées requises pour les ensembles connexes. 

Notez que comme les packages facultatifs, un `Bundle.Mapping.txt` fichier d’ensembles connexes ne fonctionne que sur Windows 10, version 1703. En outre, cible plateforme Min Version de votre application doit être définie sur 10.0.15063.0.

## Problèmes connus<a name="known_issues"></a>

Débogage d’un projet facultatif ensemble connexe n’est actuellement pas prise en charge dans Visual Studio. Pour contourner ce problème, vous pouvez déployer et lancer l’activation (Ctrl + F5) et attacher manuellement le débogueur à un processus. Pour joindre le débogueur, consultez le menu «Débogage» dans Visual Studio, sélectionnez «attacher au processus» et attachez le débogueur au **processus principal de l’application**.