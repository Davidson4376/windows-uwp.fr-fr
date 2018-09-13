---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Résoudre les problèmes qui empêchent votre application de bureau en cours d’exécution dans un conteneur MSIX
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3957499"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Appliquer des correctifs de l’exécution à un package MSIX à l’aide de l’infrastructure de prise en charge de Package

L’infrastructure de prise en charge de Package est un kit open source qui vous permet d’appliquer des correctifs à votre application win32 existantes lorsque vous n’avez pas accès au code source, afin qu’il puisse exécuter dans un conteneur MSIX. L’infrastructure de prise en charge de Package permet à votre application de suivre les meilleures pratiques de l’environnement d’exécution moderne.

Pour créer l’infrastructure de prise en charge de Package, nous avons exploité la technologie [Detours](https://www.microsoft.com/en-us/research/project/detours) qui est une infrastructure open source développée par Microsoft Research (MSR) et contribue à la redirection des API et de raccordement.

Cette infrastructure est open source, léger, et vous pouvez l’utiliser pour résoudre les problèmes d’application rapidement. Il vous donne également la possibilité de consulter avec la Communauté dans le monde entier et à utiliser les investissements d’autres personnes.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Un coup de œil rapide à l’intérieur de l’infrastructure de prise en charge de Package

L’infrastructure de prise en charge de Package contient un fichier exécutable, un DLL du Gestionnaire de runtime et un ensemble de correctifs de l’exécution.

![Infrastructure de prise en charge de package](images/desktop-to-uwp/package-support-framework.png)

Voici le principe: Vous allez créer un fichier de configuration qui spécifie le fix(s) que vous souhaitez appliquer à votre application. Ensuite, vous allez modifier votre package pour pointer vers le fichier exécutable du Lanceur shim.

Lorsque les utilisateurs lancent votre application, le Lanceur shim est le premier exécutable qui s’exécute. Il lit votre fichier de configuration et injecte la fix(s) runtime et le Gestionnaire de runtime DLL dans le processus d’application.

![Injection de DLL Framework package prise en charge](images/desktop-to-uwp/package-support-framework-2.png)

Le Gestionnaire de runtime applique le correctif lorsqu’il est requis par l’application s’exécute à l’intérieur d’un conteneur MSIX.

Ce guide vous aidera à identifier les problèmes de compatibilité des applications, et pour rechercher, appliquer et étendre runtime des correctifs qui y remédier.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identifier les problèmes de compatibilité d’application empaquetée

Tout d’abord, créez un package pour votre application. Ensuite, installez-le, exécutez-le et observer son comportement. Vous pouvez recevoir des messages d’erreur qui peuvent vous aider à identifier un problème de compatibilité. Vous pouvez également utiliser le [Moniteur de processus](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) pour identifier les problèmes.  Problèmes courants concernent les hypothèses application concernant les autorisations de chemin de répertoire et le programme de travail.

### <a name="using-process-monitor-to-identify-an-issue"></a>L’utilisation du moniteur de processus pour identifier un problème

[Moniteur de processus](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) est un utilitaire puissant pour observer les fichiers d’une application et les opérations de Registre et leurs résultats.  Cela peut vous aider à comprendre les problèmes de compatibilité d’application.  Après l’ouverture du moniteur de processus, ajouter un filtre (filtre > filtre …) pour inclure uniquement les événements à partir de l’exécutable de l’application.

![Filtre d’application ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Une liste des événements s’affiche. Pour la plupart de ces événements, le mot **Réussite** s’affiche dans la colonne de **résultat** .

![Événements de ProcMon](images/desktop-to-uwp/procmon_events.png)

Si vous le souhaitez, vous pouvez filtrer les événements pour n’afficher que seuls les échecs.

![ProcMon Exclude réussite](images/desktop-to-uwp/procmon_exclude_success.png)

Si vous suspectez un échec d’accès de système de fichiers, recherchez des événements ayant échoués qui se trouvent sous le System32/SysWOW64 ou le chemin d’accès du fichier de package. Filtres permettent d’ici, trop. Démarrez en bas de cette liste, puis faites défiler vers le haut. Échecs qui s’affichent en bas de cette liste se sont produites plus récemment. Attention la plupart des erreurs qui contiennent des chaînes telles que «accès refusé» et «chemin/nom introuvable» et ignorer les éléments qui ne semblent pas suspectes. Le [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) a deux problèmes. Vous pouvez voir ces problèmes dans la liste qui s’affiche dans l’image suivante.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

Dans le premier problème qui s’affiche dans cette image, l’application ne peut pas lire à partir du fichier «Config.txt» qui se trouve dans le chemin d’accès «C:\Windows\SysWOW64». Il est peu probable que l’application essaie de référencer directement de ce chemin d’accès. Il est probable qu’il essaie de lire à partir de ce fichier à l’aide d’un chemin d’accès relatif et par défaut, «System32/SysWOW64» est le répertoire de travail de l’application. Cela signifie que l’application attend son répertoire de travail actuel pour être défini sur quelque part dans le package. Recherchez à l’intérieur de l’appx, nous pouvons voir que le fichier existe dans le même répertoire que le fichier exécutable.

![Application Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

Le deuxième problème s’affiche dans l’image suivante.

![Fichier journal ProcMon](images/desktop-to-uwp/procmon_logfile.png)

Dans ce problème, l’application ne peut pas écrire un fichier .log dans son chemin d’accès du package. Cela suggère qu’un shim de redirection de fichier peut-être vous aider.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Trouver un correctif runtime

CELLES contient des correctifs de runtime que vous pouvez utiliser tout de suite, tels que le shim de redirection de fichier.

### <a name="file-redirection-shim"></a>Shim de Redirection de fichier

Vous pouvez utiliser le [Fichier de Redirection Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) pour rediriger les tentatives d’écrire ou de lire les données dans un répertoire qui n’est pas accessible à partir d’une application qui s’exécute dans un conteneur MSIX.

Par exemple, si votre application écrit dans un fichier journal qui se trouve dans le même répertoire que vos applications exécutables, vous pouvez utiliser le [Fichier de Redirection Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) pour créer ce fichier journal dans un autre emplacement, par exemple, le magasin de données d’application locale.

### <a name="runtime-fixes-from-the-community"></a>Correctifs de runtime de la Communauté

Veillez à passer en revue les contributions de la Communauté à notre page [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) . Il est possible que d’autres développeurs ont résolu un problème similaire au vôtre et ont partagés un correctif d’exécution.

## <a name="apply-a-runtime-fix"></a>Appliquer un correctif runtime

Vous pouvez appliquer un correctif runtime existants avec quelques outils simples à partir du Kit de développement Windows et en suivant les étapes suivantes.

> [!div class="checklist"]
> * Créez un dossier de disposition de package
> * Obtenir les fichiers de l’infrastructure de prise en charge de Package
> * Ajouter à votre package
> * Modifier le manifeste du package
> * Créer un fichier de configuration

Examinons chaque tâche.

### <a name="create-the-package-layout-folder"></a>Créer le dossier de disposition de package

Si vous disposez déjà d’un fichier .appx, vous pouvez décompresser son contenu dans un dossier de disposition qui servira à la zone de transit de votre package.  Vous pouvez le faire à partir d’un **x64 natif invite de commandes d’outils de Visual Studio 2017**, ou manuellement avec le chemin d’accès de bin SDK dans le chemin d’accès de l’exécutable de recherche.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

Cela vous donne en quelque chose qui ressemble à ce qui suit.

![Disposition de package](images/desktop-to-uwp/package_contents.png)

Si vous n’avez pas de commencer par un fichier .appx, vous pouvez créer les fichiers et le dossier du package à partir de zéro.

### <a name="get-the-package-support-framework-files"></a>Obtenir les fichiers de l’infrastructure de prise en charge de Package

Vous pouvez obtenir le package Nuget de produits à l’aide de Visual Studio. Vous pouvez également l’obtenir à l’aide de l’outil de ligne de commande de Nuget autonome.

#### <a name="get-the-package-by-using-visual-studio"></a>Obtenir le package à l’aide de Visual Studio

Dans Visual Studio, cliquez sur le nœud de votre solution ou un projet et choisissez une des commandes de gérer les Packages Nuget.  Rechercher les **Microsoft.PackageSupportFramework** ou les **produits** rechercher le package sur Nuget.org. Ensuite, l’installer.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtenir le package à l’aide de l’outil de ligne de commande

Installer l’outil de ligne de commande de Nuget à partir de cet emplacement: https://www.nuget.org/downloads. Ensuite, à partir de la ligne de commande de Nuget, exécutez la commande suivante:

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>Ajouter les fichiers de l’infrastructure de prise en charge de Package à votre package

Ajoutez la DLL de produits requises 32 bits et 64 bits et les fichiers exécutables dans le répertoire du package. Inspirez-vous du tableau suivant. Vous devrez également inclure tous les correctifs de runtime dont vous avez besoin. Dans notre exemple, nous devons le correctif de runtime de la redirection de fichier.

| Exécutable de l’application est x64 | Exécutable de l’application est x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

Le contenu de votre package doit maintenant ressembler à ceci.

![Fichiers binaires de package](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modifier le manifeste du package

Ouvrez votre manifeste du package dans un éditeur de texte et définissez le `Executable` attribut de le `Application` élément au nom du fichier exécutable Lanceur shim.  Si vous savez que l’architecture de votre application cible, sélectionnez la version appropriée, ShimLauncher32.exe ou ShimLauncher64.exe.  Si ce n’est pas le cas, ShimLauncher32.exe fonctionnera dans tous les cas.  En voici un exemple.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="ShimLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Créer un fichier de configuration

Créer un nom de fichier ``config.json``et enregistrez-le dans le dossier racine de votre package. Modifier l’ID d’application déclarée du fichier config.json pour pointer vers le fichier exécutable qui vient d’être remplacé. À l’aide de la connaissance que vous avez obtenus à partir de l’utilisation du moniteur de processus, vous pouvez également définir le répertoire de travail mais aussi utiliser le shim de redirection de fichier pour rediriger les opérations de lecture/écriture aux fichiers .log sous le répertoire de «PSFSampleApp» relatif au package.

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "shims": [
                {
                    "dll": "FileRedirectionShim.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```
Voici un guide pour le schéma config.json:

| Tableau | key | Valeur |
|-------|-----------|-------|
| applications | id |  Utilisez la valeur de la `Id` attribut de le `Application` élément dans le manifeste du package. |
| applications | exécutable | Le chemin d’accès relatif au package de l’exécutable que vous voulez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste de package avant de le modifier. Il s’agit de la valeur de la `Executable` attribut de le `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès relatif au package à utiliser comme répertoire de travail de l’application qui démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et le chemin supprimée. |
| shim | DLL | Chemin relatif au package .appx shim à charger. |
| shim | config | (Facultatif) Contrôle le comporte de la liste de distribution shim. Le format exact de cette valeur varie sur une base de shim-par-shim comme chaque shim puisse les interpréter ce «blob» qu’il le souhaite. |

Le `applications`, `processes`, et `shims` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plusieurs applications, processus et shim DLL.


### <a name="package-and-test-the-app"></a>Package et Test de l’application

Ensuite, créez un package.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

Ensuite, signez-le.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

Pour plus d’informations, voir [comment créer un certificat de signature de package](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) et [comment signer un package à l’aide de signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

À l’aide de PowerShell, installez le package.

>[!NOTE]
> N’oubliez pas de désinstallation de tout d’abord le package.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

Exécutez l’application et observez le comportement avec correctif runtime appliquée.  Répétez le diagnostic et les étapes de création de packages en fonction des besoins.

### <a name="use-the-trace-shim"></a>Utilisez le Shim de Trace

Une autre technique à diagnostiquer les problèmes de compatibilité d’application empaquetée consiste à utiliser le Shim de Trace. Cette DLL est fournie avec celles et fournit une vue détaillée de diagnostic du comportement de l’application, similaire à un moniteur de traitement.  Il est spécialement conçu pour révéler des problèmes de compatibilité d’application.  Utiliser le Shim de Trace, ajouter la DLL au package, ajoutez le fragment suivant à votre config.json et ensuite créer un package et installer votre application.

```json
{
    "dll": "TraceShim.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

Par défaut, le Shim de Trace filtre les défaillances qui peuvent être considérés comme «prévus».  Par exemple, les applications peuvent essayer de manière inconditionnelle supprimer un fichier sans vérifier s’il existe déjà, en ignorant le résultat. Cela a les conséquences malheureusement que certains échecs inattendus peuvent obtenir filtrés, afin que dans l’exemple ci-dessus, nous opter pour recevoir tous les échecs de fonctions de système de fichiers. Nous procédons ainsi car nous savons à partir d’avant que la lecture à partir du fichier Config.txt échoue avec le message «fichier introuvable». Il s’agit d’une défaillance qui est souvent observée et pas censée pour être inattendus. Dans la pratique, il est probablement mieux commencer le filtrage uniquement à des échecs inattendus et ensuite revenir à tous les échecs s’il existe un problème qui ne peut pas toujours être identifié.

Par défaut, la sortie de la Trace du Shim est envoyée au débogueur attaché. Pour cet exemple, nous ne sont pas accédant à joindre un débogueur et utilisera à la place le programme [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) de SysInternals pour consulter sa sortie. Après l’exécution de l’application, nous pouvons voir les échecs de mêmes en comme avant, ce qui nous pointez vers ces améliorations runtime.

![TraceShim fichier introuvable](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim accès refusé](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Déboguer, étendre ou créer un correctif runtime

Vous pouvez utiliser Visual Studio pour déboguer un correctif runtime, étendre un correctif runtime ou en créer un à partir de zéro. Vous devez effectuer les opérations suivantes réussisse.

> [!div class="checklist"]
> * Ajoutez un projet de création de packages
> * Ajoutez le projet pour la résolution de l’exécution
> * Ajoutez un projet qui démarre le Lanceur Shim exécutable
> * Configurer le projet de création de packages

Lorsque vous avez terminé, votre solution doit ressembler à ceci.

![Solution complétée](images/desktop-to-uwp/runtime-fix-project-structure.png)

Examinons chaque projet dans cet exemple.

| Projet | Objectif |
|-------|-----------|
| DesktopApplicationPackage | Ce projet est basé sur le [projet de package de l’Application Windows](desktop-to-uwp-packaging-dot-net.md) et qu’il génère le package MSIX. |
| Runtimefix | Il s’agit d’un projet de bibliothèque de Dynamic-Linked C++ qui contient un ou plusieurs fonctions de remplacement qui servent à la résolution de l’exécution. |
| ShimLauncher | Il s’agit de projet C++ vide. Ce projet est un endroit pour collecter les fichiers distribuable runtime de l’infrastructure de prise en charge du Package. Elle génère un fichier exécutable. Cet exécutable est la première chose qui s’exécute lorsque vous démarrez la solution. |
| WinFormsDesktopApplication | Ce projet contient le code source d’une application de bureau. |

Pour consulter un exemple complet qui contient tous ces types de projets, voir [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Passons en revue les étapes nécessaires pour créer et configurer chacune de ces projets dans votre solution.


### <a name="create-a-package-solution"></a>Créer une solution de package

Si vous n’avez pas encore une solution pour votre application de bureau, créez une nouvelle **Solution vide** dans Visual Studio.

![Solution vide](images/desktop-to-uwp/blank-solution.png)

Vous souhaiterez également ajouter des projets d’application que vous avez.

### <a name="add-a-packaging-project"></a>Ajoutez un projet de création de packages

Si vous n’avez pas encore un **Projet de création de packages d’Application Windows**, créer un et l’ajouter à votre solution.

![Modèle de projet de package](images/desktop-to-uwp/package-project-template.png)

Pour plus d’informations sur le projet de package de l’Application Windows, voir le [Package de votre application à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md).

Dans l' **Explorateur de solutions**, cliquer sur le projet de création de packages, sélectionnez le **Modifier**et ajoutez cela vers le bas du fichier de projet:

```
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>Ajoutez le projet pour la résolution de l’exécution

Ajoutez un projet C++, **La bibliothèque de liens dynamiques (DLL)** à la solution.

![Bibliothèque de correctif d’exécution](images/desktop-to-uwp/runtime-fix-library.png)

Avec le bouton droit le que de projet, puis choisissez **Propriétés**.

Dans les pages de propriétés, recherchez le champ **Standard de langage C++** et dans la liste déroulante en regard de ce champ, sélectionnez le **ISO C ++ 17 Standard (/ std: c ++ 17)** option.

![ISO 17 Option](images/desktop-to-uwp/iso-option.png)

Avec le bouton droit de ce projet et ensuite, dans le menu contextuel, choisissez l’option de **Gérer les Packages Nuget** . Vérifiez que l’option de **source du Package** est définie à **l’ensemble** ou **nuget.org**.

Cliquez sur l’icône Paramètres ensuite ce champ.

Recherche de *produits** Nuget empaqueter et puis installez-le pour ce projet.

![package NuGet](images/desktop-to-uwp/psf-package.png)

Si vous souhaitez déboguer ou étendre un correctif runtime existant, ajoutez les fichiers de correctif runtime que vous avez obtenu en utilisant les instructions décrites dans la section [trouver un correctif de l’exécution](#find) de ce guide.

Si vous envisagez de créer un nouveau correctif, n’ajoutez pas quoi que ce soit à ce projet tout de suite. Nous allons vous aider à ajouter les fichiers appropriés à ce projet plus loin dans ce guide. Pour l’instant, nous allons continuer la configuration de votre solution.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>Ajoutez un projet qui démarre le Lanceur Shim exécutable

Ajoutez un projet C++ **Projet vide** à la solution.

![Projet vide](images/desktop-to-uwp/blank-app.png)

Ajouter le package Nuget de **produits** à ce projet à l’aide de la même recommandation décrite dans la section précédente.

Ouvrez les pages de propriétés du projet et dans la page Paramètres **généraux** , définissez la propriété de **Nom de la cible** sur ``ShimLauncher32`` ou ``ShimLauncher64`` en fonction de l’architecture de votre application.

![référence de lanceur shim](images/desktop-to-uwp/shim-exe-reference.png)

Ajoutez une référence de projet au projet de correctif runtime dans votre solution.

![référence de correctif Runtime](images/desktop-to-uwp/reference-fix.png)

Avec le bouton droit de la référence, puis, dans la fenêtre **Propriétés** , appliquez ces valeurs.

| Propriété | Valeur |
|-------|-----------|-------|
| Copiez local | Vrai |
| Copier les assemblys satellites Local | Vrai |
| Sortie d’Assembly de référence | Vrai |
| Dépendances de bibliothèque de liaison | Faux |
| Entrées de dépendance de bibliothèque de liaison | Faux |

### <a name="configure-the-packaging-project"></a>Configurer le projet de création de packages

Dans le projet de mise en package, cliquez avec le bouton droit sur le dossier **Applications**, puis sélectionnez sur **Ajouter une référence**.

![Ajouter une référence de projet](images/desktop-to-uwp/add-reference-packaging-project.png)

Choisissez le projet de lanceur shim et de votre projet d’application de bureau et cliquez sur le bouton **OK** .

![Projet de bureau](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si vous n’avez pas le code source à votre application, il suffit de choisir le projet de lanceur shim. Nous allons vous montrer comment faire référence à votre fichier exécutable lorsque vous créez un fichier de configuration.

Dans le nœud des **Applications** , avec le bouton droit de l’application de lancement shim, puis choisissez **définir comme Point d’entrée**.

![Définir le point d’entrée](images/desktop-to-uwp/set-startup-project.png)

Ajoutez un fichier nommé ``config.json`` à votre projet de création de package, puis, copiez et collez le texte json suivant dans le fichier. Définissez la propriété de **l’Action de Package** pour le **contenu**.

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "shims": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```
Fournir une valeur pour chaque clé. Appuyez-vous sur le tableau suivant.

| Tableau | key | Valeur |
|-------|-----------|-------|
| applications | id |  Utilisez la valeur de la `Id` attribut de le `Application` élément dans le manifeste du package. |
| applications | exécutable | Le chemin d’accès relatif au package de l’exécutable que vous voulez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste de package avant de le modifier. Il s’agit de la valeur de la `Executable` attribut de le `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès relatif au package à utiliser comme répertoire de travail de l’application qui démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et le chemin supprimée. |
| shim | DLL | Chemin relatif au package le shim DLL à charger. |
| shim | config | (Facultatif) Contrôle le comporte de la liste de distribution shim. Le format exact de cette valeur varie sur une base de shim-par-shim comme chaque shim puisse les interpréter ce «blob» qu’il le souhaite. |

Lorsque vous avez terminé, votre ``config.json`` fichier doit ressembler à ceci.

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "shims": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> Le `applications`, `processes`, et `shims` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plusieurs applications, processus et shim DLL.

### <a name="debug-a-runtime-fix"></a>Déboguer un correctif runtime

Dans Visual Studio, appuyez sur F5 pour démarrer le débogueur.  La première chose que démarre est l’application de lancement shim, qui à son tour, démarre votre application de bureau cible.  Pour déboguer l’application de bureau cible, vous devrez manuellement attacher au processus d’application de bureau en choisissant **Déboguer**->**Attacher au processus**et en sélectionnant le processus d’application. Pour autoriser le débogage d’une application .NET avec un correctif runtime natif DLL, sélectionnez les types de code managé et natif (débogage en mode mixte).  

Une fois que vous avez configuré cela, vous pouvez définir des points d’arrêt en regard des lignes de code dans le code de l’application de bureau et le projet de correctif runtime. Si vous n’avez pas le code source à votre application, vous serez en mesure de définir des points d’arrêt uniquement en regard des lignes de code dans votre projet de correctif runtime.

>[!NOTE]
> Tandis que Visual Studio vous donne le développement plus simple et l’expérience de débogage, il existe certaines limitations, par conséquent, plus loin dans ce guide, nous verrons autres techniques de débogage que vous pouvez appliquer.

## <a name="create-a-runtime-fix"></a>Créer un correctif runtime

Si il n’existe pas encore d’un runtime résoudre le problème que vous souhaitez résoudre, vous pouvez créer un nouveau correctif de runtime en écrivant des fonctions de remplacement, y compris les données de configuration qui convient. Examinons chaque partie.

### <a name="replacement-functions"></a>Fonctions de remplacement

Tout d’abord, identifiez quelle fonction appels échouent lorsque votre application s’exécute dans un conteneur MSIX. Ensuite, vous pouvez créer des fonctions de remplacement que vous souhaitez que le Gestionnaire de l’exécution pour appeler à la place. Cela vous donne la possibilité de remplacer l’implémentation d’une fonction avec un comportement qui respecte les règles de l’environnement d’exécution moderne.

Dans Visual Studio, ouvrez le projet de correctif de runtime que vous avez créé précédemment dans ce guide.

Déclarez la ``SHIM_DEFINE_EXPORTS`` macro, puis ajoutez une instruction include pour le `shim_framework.h` en haut de chaque. Fichiers CPP dans lequel vous envisagez d’ajouter les fonctions de votre correctif runtime.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>Assurez-vous que le `SHIM_DEFINE_EXPORTS` macro s’affiche avant l’instruction d’inclusion.

Créer une fonction qui a la même signature de la fonction qui a comportement que vous souhaitez modifier. Voici un exemple de fonction qui remplace le `MessageBoxW` fonction.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWShim(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_SHIM(MessageBoxWImpl, MessageBoxWShim);
```

L’appel à `DECLARE_SHIM` mappe le `MessageBoxW` pour votre nouvelle fonction de remplacement. Lorsque votre application tente d’appeler le `MessageBoxW` fonction, elle appelle la fonction de remplacement à la place.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protection contre les appels récurrents à des fonctions dans les correctifs runtime

Vous pouvez éventuellement appliquer le `reentrancy_guard` type à vos fonctions qui protègent contre les appels récurrents à des fonctions dans les correctifs du runtime.

Par exemple, cela peut donner une fonction de remplacement pour le `CreateFile` fonction. Votre implémentation peut appeler le `CopyFile` fonction, mais l’implémentation de la `CopyFile` fonction peut appeler le `CreateFile` fonction. Cela peut entraîner un cycle récurrent infini d’appels à le `CreateFile` fonction.

Pour plus d’informations sur `reentrancy_guard` voir [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Données de configuration

Si vous souhaitez ajouter des données de configuration à votre correctif d’exécution, envisagez d’ajouter l’application à la ``config.json``. De cette façon, vous pouvez utiliser le `ShimQueryCurrentDllConfig` facilement analyser ces données. Cet exemple analyse une valeur booléenne et chaîne à partir de ce fichier de configuration.

```c++
if (auto configRoot = ::ShimQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

## <a name="other-debugging-techniques"></a>Autres techniques de débogage

Visual Studio vous donne le développement plus simple et l’expérience de débogage, mais il existe certaines limitations.

Tout d’abord, débogage F5 exécute l’application par le déploiement de fichiers libres dans le chemin d’accès de dossier de la mise en package, au lieu de l’installation à partir d’un package .appx.  Le dossier de disposition en règle générale, n’a pas les mêmes restrictions de sécurité qu’un dossier de package installé. Par conséquent, il ne peut pas être possible de reproduire des erreurs de déni d’accès de chemin d’accès de package avant d’appliquer un correctif de runtime.

Pour résoudre ce problème, utilisez le déploiement du package .appx au lieu du déploiement de fichiers libres F5.  Pour créer un fichier de package .appx, utilisez l’utilitaire de [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) dans le Kit de développement Windows, comme décrit ci-dessus. Ou, à partir de Visual Studio, cliquez sur le nœud de projet de votre application et sélectionnez **Store**->**Créer des Packages d’application**.

Un autre problème avec Visual Studio est qu’il n’a pas de prise en charge intégrée pour l’attachement à n’importe quel processus enfants lancées par le débogueur.   Cela complique la logique dans le chemin de démarrage de l’application cible, qui doit être liée manuellement par Visual Studio après le lancement de débogage.

Pour résoudre ce problème, utilisez un débogueur qui prend en charge de processus enfant joindre.  Notez qu’il n’est généralement pas possible d’attacher un débogueur (JIT) de juste-à-temps à l’application cible.  Il s’agit dans la mesure où la plupart des techniques JIT impliquent de lancer le débogueur à la place de l’application cible, via la clé de Registre ImageFileExecutionOptions.  Cela va à l’encontre detouring mécanisme utilisé par ShimLauncher.exe pour injecter ShimRuntime.dll dans l’application cible.  WinDbg, inclus dans les [Outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)et obtenues à partir du [Kit de développement Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), attacher de processus enfant de prend en charge.  Il prend également en charge directement [lancement et de débogage d’une application UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Pour déboguer le démarrage de l’application cible comme un processus enfant, démarrer ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

À la ``WinDbg`` invite, activer le débogage des enfants et définir des points d’arrêt appropriés.

```
.childdbg 1
g
```
(exécuter en tant que l’application cible démarre et s’arrête dans le débogueur)

```
sxe ld fixup.dll
g
```
(s’exécute jusqu'à ce que la correction que DLL est chargée)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) peut également être utilisé pour joindre un débogueur à une application lors du lancement et est également inclus dans les [Outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Toutefois, il est plus complexe à utiliser que la prise en charge directe désormais fourni par WinDbg.

## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
