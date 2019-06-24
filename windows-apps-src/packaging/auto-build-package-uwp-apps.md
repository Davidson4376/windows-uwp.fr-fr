---
title: Configuration de builds automatisées pour votre application UWP
description: Configuration de builds automatisées pour produire des packages de chargement indépendant et/ou pour le Store.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 61525e2a4a088e37184bb93526722e0bf23fbd56
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319808"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configuration de builds automatisées pour votre application UWP

Vous pouvez utiliser des Pipelines d’Azure pour créer des générations automatisées pour les projets UWP. Dans cet article, nous examinerons les différentes manières de procéder. Nous vous montrerons également comment effectuer ces tâches à l’aide de la ligne de commande afin que vous pouvez intégrer n’importe quel autre système de génération.

## <a name="create-a-new-azure-pipeline"></a>Créer un Pipeline Azure

Commencez par [inscription à Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) si vous n’avez pas déjà fait.

Ensuite, créez un pipeline que vous pouvez utiliser pour générer votre code source. Pour obtenir un didacticiel sur la création d’un pipeline pour créer un référentiel GitHub, consultez [créer votre premier pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Les Pipelines Azure prend en charge les types de référentiel répertoriés [dans cet article](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configurer une build automatisée

Nous allons commencer par la valeur par défaut UWP définition qui est disponible dans Azure Dev Ops de build et de vous montrer comment configurer le pipeline.

Dans la liste des modèles de définition de build, choisissez le modèle **Plateforme Windows universelle**.

![Sélectionnez le modèle UWP](images/select-yaml-template.png)

Ce modèle inclut la configuration de base pour générer votre projet UWP :

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

Le modèle par défaut tente de signer le package avec le certificat spécifié dans le fichier .csproj. Si vous souhaitez signer votre package pendant la génération, vous devez avoir accès à la clé privée. Sinon, vous pouvez désactiver la signature en ajoutant le paramètre `/p:AppxPackageSigningEnabled=false` à la `msbuildArgs` section dans le fichier YAML.

## <a name="add-your-project-certificate-to-a-repository"></a>Ajouter votre certificat de projet à un référentiel

Pipelines fonctionne avec les référentiels Azure dépôts Git et TFVC. Si vous utilisez un référentiel Git, ajoutez le fichier de certificat de votre projet au référentiel pour que l’agent de build puisse signer le package de l'application. Si vous ne procédez pas ainsi, le référentiel Git ignore le fichier de certificat. Pour ajouter le fichier de certificat à votre référentiel, avec le bouton droit dans le fichier de certificat **l’Explorateur de solutions**, puis, dans le menu contextuel, choisissez le **ajouter un fichier ignoré au contrôle de code Source** commande.

![inclusion d’un certificat](images/building-screen1.png)

## <a name="configure-the-build-solution-build-task"></a>Configuration de la tâche Générer la solution

Cette tâche compile une solution qui est dans le dossier de travail pour les fichiers binaires et produit le fichier de package d’application de la sortie.
Cette tâche utilise des arguments MSBuild. Vous devez spécifier la valeur de ces arguments. Inspirez-vous du tableau suivant.

|**Argument MSBuild**|**Valeur**|**Description**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Définit le dossier de stockage des artefacts générés. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Vous permet de définir les plateformes à inclure dans le regroupement. |
| AppxBundle | Toujours | Crée un.msixbundle/.appxbundle avec les fichiers.msix/.appx pour la plateforme spécifiée. |
| UapAppxPackageBuildMode | StoreUpload | Génère le fichier.msixupload/.appxupload et **_Test** dossier pour le chargement indépendant. |
| UapAppxPackageBuildMode | CI | Génère le fichier.msixupload/.appxupload uniquement. |
| UapAppxPackageBuildMode | SideloadOnly | Génère le **_Test** dossier pour le chargement de version test uniquement |

Si vous souhaitez générer votre solution à l’aide de la ligne de commande, ou à l’aide de n’importe quel autre système de génération, exécutez MSBuild avec ces arguments.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

Les paramètres définis avec le `$()` syntaxe sont des variables définies dans la définition de build et de modification dans d’autres créer des systèmes.

![variables par défaut](images/building-screen5.png)

Pour afficher toutes les variables prédéfinies, consultez [prédéfini les variables de génération](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configurer la tâche publier les artefacts de Build

Le pipeline UWP par défaut n’enregistre pas les artefacts générés. Pour ajouter les fonctionnalités de publication à votre définition YAML, ajoutez les tâches suivantes.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

Vous pouvez voir les artefacts générés dans le **artefacts** page de résultats de l’option de la build.

![artefacts](images/building-screen6.png)

Étant donné que nous avons défini le `UapAppxPackageBuildMode` argument `StoreUpload`, le dossier d’artefacts inclut le package pour l’envoi vers le Store (.msixupload/.appxupload). Notez que vous pouvez également envoyer un package d’application standard (.msix/.appx) ou un lot d’applications (.msixbundle/.appxbundle/) vers le Store. Dans le cadre de cet article, nous allons utiliser le fichier .appxupload.

## <a name="address-bundle-errors"></a>Résoudre les erreurs d’offre groupée

Si vous ajoutez plusieurs projets UWP à votre solution et puis que vous essayez de créer un regroupement, vous pouvez recevoir une erreur similaire à celle-ci.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Cette erreur s’affiche car l’application qui doit apparaître dans l’offre groupée n’est pas clairement définie au niveau de la solution. Pour résoudre ce problème, ouvrez chaque fichier de projet et ajoutez les propriétés suivantes à la fin de la première `<PropertyGroup>` élément.

|**Project**|**Propriétés**|
|-------|----------|
|Application|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Ensuite, supprimez le `AppxBundle` argument MSBuild à partir de l’étape de génération.

## <a name="related-topics"></a>Rubriques connexes

- [Générer votre application .NET pour Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empaquetage d’applications UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Chargement de version test des applications métier dans Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Créer un certificat pour la signature du package](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
