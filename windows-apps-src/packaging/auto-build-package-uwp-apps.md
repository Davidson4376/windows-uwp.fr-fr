---
title: Configuration de builds automatisées pour votre application UWP
description: Configuration de builds automatisées pour produire des packages de chargement indépendant et/ou pour le Store.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: de623240e275dda5b6fc4df9afee31e1adf9fd4f
ms.sourcegitcommit: 04683376dbdbff987601f546f058748442170068
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68340849"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configuration de builds automatisées pour votre application UWP

Vous pouvez utiliser Azure Pipelines pour créer des builds automatisées pour les projets UWP. Dans cet article, nous allons examiner différentes façons de procéder. Nous vous montrerons également comment effectuer ces tâches à l’aide de la ligne de commande afin que vous puissiez les intégrer à tout autre système de génération.

## <a name="create-a-new-azure-pipeline"></a>Créer un pipeline Azure

Commencez par vous [inscrire à Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) si vous ne l’avez pas déjà fait.

Ensuite, créez un pipeline que vous pouvez utiliser pour générer votre code source. Pour obtenir un didacticiel sur la création d’un pipeline pour créer un référentiel GitHub, consultez [créer votre premier pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Azure Pipelines prend en charge les types de référentiels listés [dans cet article](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configuration d’une build automatisée

Nous allons commencer par la définition de build UWP par défaut qui est disponible dans Azure dev OPS, puis vous montrer comment configurer le pipeline.

Dans la liste des modèles de définition de build, choisissez le modèle **Plateforme Windows universelle**.

![Sélectionner le modèle UWP](images/select-yaml-template.png)

Ce modèle comprend la configuration de base pour générer votre projet UWP:

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

Le modèle par défaut tente de signer le package avec le certificat spécifié dans le fichier. csproj. Si vous souhaitez signer votre package pendant la génération, vous devez avoir accès à la clé privée. Sinon, vous pouvez désactiver la signature en ajoutant le `/p:AppxPackageSigningEnabled=false` paramètre à `msbuildArgs` la section dans le fichier YAML.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Ajouter votre certificat de projet à la bibliothèque de fichiers sécurisés

Vous devez éviter de soumettre des certificats à votre référentiel si possible, et git les ignore par défaut. Pour gérer la gestion sécurisée des fichiers sensibles tels que les certificats, Azure DevOps prend en charge la fonctionnalité de [fichiers sécurisés](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops) .

Pour télécharger un certificat pour votre Build automatisée:

1. Dans Azure Pipelines, développez **pipelines** dans le volet de navigation et cliquez sur **bibliothèque**.
2. Cliquez sur l’onglet **fichiers sécurisés** , puis sur **+ fichier sécurisé**.

    ![Téléchargement d’un fichier sécurisé](images/secure-file1.png)

3. Accédez au fichier de certificat, puis cliquez sur **OK**.
4. Une fois le certificat téléchargé, sélectionnez-le pour afficher ses propriétés. Sous **autorisations**de pipeline, activez la bascule **autoriser pour une utilisation dans tous les pipelines** .

    ![Téléchargement d’un fichier sécurisé](images/secure-file2.png)

5. Si le certificat possède un mot de passe, nous vous recommandons de stocker votre mot de passe dans [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) puis de lier le mot de passe à un [groupe de variables](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Vous pouvez utiliser la variable pour accéder au mot de passe à partir du pipeline.

> [!NOTE]
> À compter de Visual Studio 2019, un certificat temporaire n’est plus généré dans les projets UWP. Pour créer ou exporter des certificats, utilisez les applets de commande PowerShell décrites dans [cet article](create-certificate-package-signing.md).

## <a name="configure-the-build-solution-build-task"></a>Configuration de la tâche Générer la solution

Cette tâche compile toutes les solutions qui se trouvent dans le dossier de travail en fichiers binaires et génère le fichier de package d’application de sortie. Cette tâche utilise des arguments MSBuild. Vous devez spécifier la valeur de ces arguments. Inspirez-vous du tableau suivant.

|**Argument MSBuild**|**Valeur**|**Description**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Définit le dossier de stockage des artefacts générés. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Vous permet de définir les plateformes à inclure dans le bundle. |
| AppxBundle | Always | Crée un fichier. msixbundle/. appxbundle avec les fichiers. msix/. AppX pour la plateforme spécifiée. |
| UapAppxPackageBuildMode | StoreUpload | Génère le fichier. msixupload/. appxupload et le dossier **_Test** pour chargement. |
| UapAppxPackageBuildMode | CI | Génère uniquement le fichier. msixupload/. appxupload. |
| UapAppxPackageBuildMode | SideloadOnly | Génère le dossier **_Test** pour chargement uniquement. |
| AppxPackageSigningEnabled | true | Active la signature de package. |
| PackageCertificateThumbprint | Empreinte de certificat | Cette valeur **doit** correspondre à l’empreinte numérique du certificat de signature ou être une chaîne vide. |
| PackageCertificateKeyFile | path | Chemin d’accès du certificat à utiliser. Elle est extraite des métadonnées de fichier sécurisé. |
| PackageCertificatePassword | Mot de passe | Mot de passe du certificat. Nous vous recommandons de stocker votre mot de passe dans [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) et de lier le mot de passe au [groupe de variables](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Vous pouvez passer la variable à cet argument. |

### <a name="configure-the-build"></a>Configurer la Build

Si vous souhaitez générer votre solution à l’aide de la ligne de commande, ou à l’aide d’un autre système de génération, exécutez MSBuild avec ces arguments.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configurer la signature du package

Pour signer le package MSIX (ou APPX), le pipeline doit récupérer le certificat de signature. Pour ce faire, ajoutez une tâche DownloadSecureFile avant la tâche VSBuild.
Vous obtiendrez ainsi l’accès au certificat de signature ```signingCert```via.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Ensuite, mettez à jour la tâche VSBuild pour référencer le certificat de signature:

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
> L’argument PackageCertificateThumbprint est défini intentionnellement sur une chaîne vide comme précaution. Si l’empreinte numérique est définie dans le projet, mais ne correspond pas au certificat de signature, la génération échoue avec l' `Certificate does not match supplied signing thumbprint`erreur:.

### <a name="review-parameters"></a>Passer en revue les paramètres

Les paramètres définis avec la `$()` syntaxe sont des variables définies dans la définition de build et sont modifiés dans d’autres systèmes de génération.

![variables par défaut](images/building-screen5.png)

Pour afficher toutes les variables prédéfinies, consultez [variables de build](https://docs.microsoft.com/azure/devops/pipelines/build/variables)prédéfinies.

## <a name="configure-the-publish-build-artifacts-task"></a>Configurer la tâche publier les artefacts de build

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

Vous pouvez voir les artefacts générés dans  l’option artefacts de la page résultats de la Build.

![artefacts](images/building-screen6.png)

Étant donné que nous avons `UapAppxPackageBuildMode` défini l' `StoreUpload`argument sur, le dossier artefacts comprend le package à envoyer au magasin (. msixupload/. appxupload). Notez que vous pouvez également envoyer un package d’application standard (. msix/. AppX) ou un bundle d’applications (. msixbundle/. appxbundle/) au magasin. Dans le cadre de cet article, nous allons utiliser le fichier .appxupload.

## <a name="address-bundle-errors"></a>Erreurs du groupe d’adresses

Si vous ajoutez plusieurs projets UWP à votre solution, puis que vous essayez de créer un bundle, vous pouvez recevoir une erreur semblable à celle-ci.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Cette erreur s’affiche car l’application qui doit apparaître dans l’offre groupée n’est pas clairement définie au niveau de la solution. Pour résoudre ce problème, ouvrez chaque fichier projet et ajoutez les propriétés suivantes à la fin du premier `<PropertyGroup>` élément.

|**Projection**|**Propriétés**|
|-------|----------|
|Application|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Ensuite, supprimez `AppxBundle` l’argument MSBuild de l’étape de génération.

## <a name="related-topics"></a>Rubriques connexes

- [Générez votre application .NET pour Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empaqueter des applications UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Chargement des applications métier dans Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Créer un certificat pour la signature du package](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
