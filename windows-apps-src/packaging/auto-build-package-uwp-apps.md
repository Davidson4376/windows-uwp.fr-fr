---
title: Configuration de builds automatisées pour votre application UWP
description: Configuration de builds automatisées pour produire des packages de chargement indépendant et/ou pour le Store.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 5837674f2cb20710a59eeac0af59498bf28b197e
ms.sourcegitcommit: a86d0bd1c2f67e5986cac88a98ad4f9e667cfec5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68229374"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configuration de builds automatisées pour votre application UWP

Vous pouvez utiliser des Pipelines d’Azure pour créer des générations automatisées pour les projets UWP. Dans cet article, nous examinerons les différentes manières de procéder. Nous vous montrerons également comment effectuer ces tâches à l’aide de la ligne de commande afin que vous pouvez intégrer n’importe quel autre système de génération.

## <a name="create-a-new-azure-pipeline"></a>Créer un Pipeline Azure

Commencez par [inscription à Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) si vous n’avez pas déjà fait.

Ensuite, créez un pipeline que vous pouvez utiliser pour générer votre code source. Pour obtenir un didacticiel sur la création d’un pipeline pour créer un référentiel GitHub, consultez [créer votre premier pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Les Pipelines Azure prend en charge les types de référentiel répertoriés [dans cet article](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configuration d’une build automatisée

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

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Ajouter votre certificat de projet à la bibliothèque de fichiers sécurisés

Vous devez éviter d’envoyer des certificats à votre référentiel si possible, et les ignore par git par défaut. Pour gérer la gestion sécurisée des fichiers sensibles telles que des certificats, prend en charge Azure DevOps [sécuriser les fichiers](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops).

Pour télécharger un certificat pour la génération automatisée :

1. Dans les Pipelines d’Azure, développez **Pipelines** dans le volet de navigation et cliquez sur **bibliothèque**.
2. Cliquez sur le **sécuriser les fichiers** onglet, puis cliquez sur **+ fichier sécurisé**.

    ![comment charger un fichier sécurisé](images/secure-file1.png)

3. Recherchez le fichier de certificat et cliquez sur **OK**.
4. Après avoir téléchargé le certificat, sélectionnez-le pour afficher ses propriétés. Sous **Pipeline autorisations**, activer la **Authorize pour une utilisation dans tous les pipelines** activer/désactiver.

    ![comment charger un fichier sécurisé](images/secure-file2.png)

## <a name="configure-the-build-solution-build-task"></a>Configuration de la tâche Générer la solution

Cette tâche compile une solution qui est dans le dossier de travail pour les fichiers binaires et produit le fichier de package d’application de la sortie.
Cette tâche utilise des arguments MSBuild. Vous devez spécifier la valeur de ces arguments. Inspirez-vous du tableau suivant.

|**Argument MSBuild**|**Valeur**|**Description**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Définit le dossier de stockage des artefacts générés. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Vous permet de définir les plateformes à inclure dans le regroupement. |
| AppxBundle | Always | Crée un.msixbundle/.appxbundle avec les fichiers.msix/.appx pour la plateforme spécifiée. |
| UapAppxPackageBuildMode | StoreUpload | Génère le fichier.msixupload/.appxupload et **_Test** dossier pour le chargement indépendant. |
| UapAppxPackageBuildMode | CI | Génère le fichier.msixupload/.appxupload uniquement. |
| UapAppxPackageBuildMode | SideloadOnly | Génère le **_Test** dossier pour le chargement indépendant uniquement. |
| AppxPackageSigningEnabled | true | Permet la signature du package. |
| PackageCertificateThumbprint | Empreinte de certificat | Cette valeur **doit** correspond à l’empreinte numérique du certificat de signature, ou être une chaîne vide. |
| PackageCertificateKeyFile | path | Le chemin d’accès au certificat à utiliser. Cela est récupéré à partir des métadonnées de fichier sécurisé. |

### <a name="configure-the-build"></a>Configurez la build

Si vous souhaitez générer votre solution à l’aide de la ligne de commande, ou à l’aide de n’importe quel autre système de génération, exécutez MSBuild avec ces arguments.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configurer la signature du package

Le pipeline doit récupérer le certificat de signature pour signer le package MSIX (ou APPX). Pour ce faire, ajoutez une tâche DownloadSecureFile avant la tâche VSBuild.
Cela vous donnera accès à la signature des certificats via ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Ensuite, mettez à jour la tâche VSBuild pour référencer le certificat de signature :

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> L’argument PackageCertificateThumbprint est intentionnellement définie sur une chaîne vide comme une précaution. Si l’empreinte numérique est définie dans le projet, mais ne correspond pas au certificat de signature, la build échoue avec l’erreur : `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Passez en revue les paramètres

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

|**projet**|**Propriétés**|
|-------|----------|
|Application|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Ensuite, supprimez le `AppxBundle` argument MSBuild à partir de l’étape de génération.

## <a name="related-topics"></a>Rubriques connexes

- [Générer votre application .NET pour Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empaquetage d’applications UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Chargement de version test des applications métier dans Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Créer un certificat pour la signature du package](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
