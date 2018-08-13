---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Packages facultatifs et création d’ensembles connexes
description: Les packages facultatifs intègrent du contenu qui peut être inclus dans un package principal. Ils sont utiles pour le contenu téléchargeable (DLC), pour diviser une application volumineuse en cas de restrictions de taille, ou pour distribuer un contenu supplémentaire indépendamment de votre application d’origine.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, packages facultatifs, connexes, extension de package, de visual studio
ms.localizationpriority: medium
ms.openlocfilehash: d66a511211396190393e31bfd553149a1e89fad0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "406082"
---
# <a name="optional-packages-and-related-set-authoring"></a>Packages facultatifs et création d’ensembles connexes
Les packages facultatifs intègrent du contenu qui peut être inclus dans un package principal. Ces sont utiles pour le contenu téléchargeable (DLC), division d’une application pour les restrictions de taille, grande taille ou pour n’importe quel contenu supplémentaire de livraison séparent à partir de votre application d’origine.

Ensembles connexes sont une extension de packages facultatifs: ils permettent d’appliquer un ensemble de versions entre les packages principales et facultatifs. Elles permettent également de la charge du code natif (C++) à partir de packages facultatifs. 

## <a name="prerequisites"></a>Conditions préalables

- Visual Studio 2017, version 15.1
- Windows10 version1703
- Windows 10, version 1703 SDK

Pour obtenir tous les outils de développement le plus récent, voir [téléchargements et outils pour Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Pour soumettre une application qui utilise des packages facultatifs et/ou jeux liés à Microsoft Store, vous aurez besoin d’autorisation. S’ils ne sont pas soumis au Store, les packages facultatifs et ensembles connexes peuvent servir pour les applications métier (LOB) ou de l’entreprise sans autorisation du centre de développement. Voir [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) pour obtenir l’autorisation de soumettre une application qui utilise des packages facultatifs et ensembles connexes.

### <a name="code-sample"></a>Exemple de code
Pendant que vous êtes en train de lire cet article, il est recommandé de suivre ainsi que l' [exemple de code de package facultatif](https://github.com/AppInstaller/OptionalPackageSample) sur les référentiels pour une connaissance pratique de packages comment facultatifs et liées ensembles de travail dans Visual Studio.

## <a name="optional-packages"></a>Packages facultatifs
Pour créer un package facultatif dans Visual Studio, vous devez:
1. Assurez-vous que votre application **Min Version de la plateforme cible** est défini sur: 10.0.15063.0.
2. À partir de votre projet **principal du lot** , ouvrez le `Package.appxmanifest` fichier. Accédez à l’onglet «Emballage» et prenez note de votre **nom de famille de package**, c'est-à-dire tout ce qui précède le caractère «_».
3. À partir de votre projet de **package facultatif** , bouton droit sur le `Package.appxmanifest` et sélectionnez **Ouvrir avec > Éditeur XML (texte)**.
4. Recherchez la `<Dependencies>` élément dans le fichier. Ajouter les éléments suivants:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Remplacez `[MainPackageDependency]` avec votre **nom de famille de package** à l’étape 2. Il spécifie que votre **package facultatif** dépend de votre **package principal**.

Une fois que les dépendances de package configurer des étapes 1 à 4, vous pouvez continuer de développement comme vous le feriez normalement. Si vous souhaitez charger le code à partir du package facultatif dans le package principal, vous devrez créer un jeu connexe. Consultez la section [liée définit](#related_sets) pour plus d’informations.

Visual Studio peut être configuré pour redéployer votre package principal chaque fois que vous déployez un package facultatif. Pour définir la dépendance de génération dans Visual Studio, vous devez:

- Cliquez avec le bouton droit sur le projet de package facultatif et sélectionnez **dépendances créer > dépendances du projet...**
- Archiver le projet principal du lot et sélectionnez «OK». 

Désormais, chaque fois que vous entrez F5 ou créez un projet de package facultatif, Visual Studio générez le projet principal du lot tout d’abord. Cela permet de garantir que votre projet principal et les projets facultatifs sont synchronisés.

## Ensembles connexes<a name="related_sets"></a>

Si vous souhaitez charger le code à partir d’un package facultatif dans le package principal, vous devrez créer un jeu connexe. Pour créer un jeu connexe, votre package principal et facultatif doivent être étroitement couplés. Les métadonnées pour des ensembles connexes sont spécifiée dans le `.appxbundle` fichier du package principal. Visual Studio vous permet d’obtenir les métadonnées correctes dans vos fichiers. Pour configurer la solution de votre application pour les jeux, procédez comme suit:

1. Cliquez avec le bouton droit sur le projet principal du lot, sélectionnez **Ajouter > nouvel élément**
2. Dans la fenêtre, recherchez les modèles installés pour «.txt» et ajouter un nouveau fichier texte.
> [!IMPORTANT]
> Le nouveau fichier texte doit être nommé: `Bundle.Mapping.txt`.
3. Dans la `Bundle.Mapping.txt` fichier, vous devez spécifier les chemins d’accès relatifs à n’importe quel package facultatif projets ou des packages externes. Un exemple de `Bundle.Mapping.txt` fichier doit ressembler à ceci:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Lorsque votre solution est configurée de cette manière, Visual Studio crée un manifeste de groupement pour le package principal avec toutes les métadonnées requises pour les jeux. 

Notez que comme les packages facultatifs, un `Bundle.Mapping.txt` fichier pour des ensembles connexes ne fonctionne que sur Windows 10, version 1703. En outre, la plate-forme Min Version de votre application cible doit être définie à 10.0.15063.0.

## Problèmes connus<a name="known_issues"></a>

Débogage d’un projet facultatif associées n’est pas actuellement pris en charge dans Visual Studio. Pour contourner ce problème, vous pouvez déployer et lancer l’activation (Ctrl + F5) et manuellement attacher le débogueur à un processus. Pour attacher le débogueur, passez le menu «Debug» dans Visual Studio, sélectionnez «Attacher au processus …» et attacher le débogueur au **processus de l’application principale**.