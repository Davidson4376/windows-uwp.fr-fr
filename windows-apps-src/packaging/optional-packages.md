---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Création de packages facultatifs et d’ensembles associés
description: Les packages facultatifs intègrent du contenu qui peut être inclus dans un package principal. Ils sont utiles pour le contenu téléchargeable (DLC), pour diviser une application volumineuse en cas de restrictions de taille, ou pour distribuer un contenu supplémentaire indépendamment de votre application d’origine.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp, packages facultatifs, ensemble connexe, extension de package, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594024"
---
# <a name="optional-packages-and-related-set-authoring"></a>Création de packages facultatifs et d’ensembles associés
Les packages facultatifs intègrent du contenu qui peut être inclus dans un package principal. Ils sont utiles pour le contenu téléchargeable (DLC), pour diviser une application volumineuse en cas de restrictions de taille, ou pour distribuer un contenu supplémentaire indépendamment de votre application d’origine.

Les ensembles connexes sont une extension des packages facultatifs : ils permettent d’appliquer un ensemble strict de versions entre les packages principaux et facultatifs. Ils permettent également de charger du code natif (C++) à partir des packages facultatifs. 

## <a name="prerequisites"></a>Conditions préalables

- Visual Studio 2017, version 15.1
- Windows 10 version 1703
- SDK Windows 10, version 1703

Pour obtenir tous les outils de développement les plus récents, consultez [Téléchargements et outils pour Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Pour soumettre une application qui utilise des packages facultatifs et/ou les ensembles liés pour le Microsoft Store, vous aurez besoin d’autorisation. Packages facultatifs et les ensembles liés peuvent servir pour les applications métier (LOB) ou de l’entreprise sans autorisation de partenaires si elles ne sont pas envoyées au Store. Voir [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) pour obtenir l’autorisation de soumettre une application qui utilise des packages facultatifs et ensembles connexes.

### <a name="code-sample"></a>Exemple de code
Nous vous recommandons de lire cet article en le comparant à l’[exemple de code de package facultatif](https://github.com/AppInstaller/OptionalPackageSample) disponible sur GitHub pour bénéficier d’une compréhension pratique du fonctionnement des paquets optionnels et des ensembles connexes dans Visual Studio.

## <a name="optional-packages"></a>Packages facultatifs
Pour créer un package facultatif dans Visual Studio, vous devez :
1. Assurez-vous que votre application **Min Version de la plateforme cible** est définie sur : 10.0.15063.0 ou une version ultérieure.
2. À partir du **package principal** de votre projet, ouvrez le fichier `Package.appxmanifest`. Accédez à l’onglet « Packages » et notez votre **Nom de la famille de packages**, qui correspond à tout ce qui précède le caractère « _ ».
3. Depuis votre projet de **package facultatif**, cliquez avec le bouton droit sur le `Package.appxmanifest` et sélectionnez **Ouvrir avec > Éditeur XML (texte)**.
4. Recherchez l’élément `<Dependencies>` dans le fichier. Ajoutez ce qui suit :

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Remplacez `[MainPackageDependency]` par le **Nom de la famille de packages** de l’étape 2. Ceci permet d’indiquer que votre **package facultatif** dépend de votre **package principal**.

Une fois que vous avez configuré les dépendances de vos paquets entre les étapes 1 et 4, vous pouvez continuer à développer comme vous le feriez normalement. Si vous souhaitez charger dans le package principal du code provenant du package facultatif, vous devez créer un ensemble connexe. Consultez la section [Ensembles connexes](#related_sets) pour en savoir plus.

Visual Studio peut être configuré pour redéployer votre package principal à chaque fois que vous déployez un package facultatif. Pour définir la dépendance de build dans Visual Studio, vous devez :

- Cliquer avec le bouton droit sur le projet de package facultatif et sélectionner **Dépendances de build > Dépendances du projet...**
- Vérifier le projet du package principal et sélectionner « OK ». 

Maintenant, chaque fois que vous taperez F5 ou générerez un projet de package facultatif, Visual Studio générera d’abord le projet de package principal. Ainsi, la synchronisation de votre projet principal et des projets facultatifs est garantie.

## Ensembles connexes<a name="related_sets"></a>

Si vous désirez charger dans le package principal du code provenant d’un package facultatif, vous devez créer un ensemble connexe. Pour générer un ensemble connexe, le package principal et le package facultatif doivent être étroitement liés. Les métadonnées pour les ensembles liés sont spécifiée dans le fichier .appxbundle ou .msixbundle du package principal. Visual Studio vous permet d’obtenir les métadonnées correctes dans vos fichiers. Pour configurer la solution de votre application pour des ensembles connexes, suivez ces étapes :

1. Cliquez avec le bouton droit sur le projet du package principal, sélectionnez **Ajouter > Nouvel élément...**
2. Dans la fenêtre, recherchez les modèles installés pour « .txt » et ajoutez un nouveau fichier texte.
> [!IMPORTANT]
> Ce nouveau fichier texte doit être nommé : `Bundle.Mapping.txt`.

3. Dans le fichier `Bundle.Mapping.txt` spécifiez les chemins d’accès relatifs aux projets de package facultatifs ou aux packages externes. Un fichier `Bundle.Mapping.txt` d’exemple doit se présenter ainsi :

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Lorsque votre solution est configurée de cette façon, Visual Studio crée un manifeste d’ensemble pour le paquet principal comportant toutes les métadonnées requises pour les ensembles connexes. 

Notez que comme packages facultatifs, un `Bundle.Mapping.txt` fichier pour les ensembles liés ne fonctionne que sur Windows 10, version 1703 ou une version ultérieure. En outre, Version de Min de plateforme cible de votre application doit être définie à 10.0.15063.0 ou version ultérieure.

## Problèmes connus<a name="known_issues"></a>

Le débogage d’un projet facultatif d’ensembles connexes n’est actuellement pas pris en charge dans Visual Studio. Pour contourner ce problème, vous pouvez déployer et lancer l’activation (Ctrl+F5) et relier manuellement le débogueur à un processus. Pour attacher le débogueur, consultez le menu « Débogage » dans Visual Studio, sélectionnez « Attacher au processus... » et attachez le débogueur au **processus de l’application principale**.