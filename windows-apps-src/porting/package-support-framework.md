---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Résoudre les problèmes qui empêchent votre application de s’exécuter dans un conteneur MSIX de bureau
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3986366"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Appliquer les correctifs de runtime à un package MSIX à l’aide de l’infrastructure de prise en charge du Package

L’infrastructure de prise en charge du Package est un kit d’open source qui vous permet d’appliquer des correctifs à votre application win32 existante lorsque vous n’avez pas accès au code source, afin qu’il peut s’exécuter dans un conteneur MSIX. L’infrastructure de prise en charge de Package permet à votre application de suivre les meilleures pratiques de l’environnement d’exécution moderne.

Pour créer l’infrastructure de prise en charge du Package, nous avons exploité la technologie [Detours](https://www.microsoft.com/en-us/research/project/detours) qui est une infrastructure open source développée par Microsoft Research (MSR) et aide à la redirection de l’API et de raccordement.

Cette structure est une source ouverte, léger, et vous pouvez l’utiliser pour résoudre les problèmes rapidement. Il vous donne également la possibilité de consulter la Communauté partout dans le monde et de créer sur les investissements des autres.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Un coup de œil à l’intérieur de l’infrastructure de prise en charge du Package

L’infrastructure de prise en charge du Package contient un fichier exécutable, un DLL de gestionnaire du runtime et un ensemble de correctifs de runtime.

![Infrastructure de prise en charge de package](images/desktop-to-uwp/package-support-framework.png)

Voici le principe: Vous allez créer un fichier de configuration qui spécifie le fix(s) que vous souhaitez appliquer à votre application. Puis, vous modifierez votre package afin de pointer vers le fichier exécutable du Lanceur shim.

Lorsque les utilisateurs démarrent votre application, le Lanceur de shim est le premier exécutable qui s’exécute. Il lit le fichier de configuration et injecte des fix(s) de l’exécution et à la DLL du Gestionnaire de runtime dans le processus d’application.

![Injection de DLL de package prise en charge du Framework](images/desktop-to-uwp/package-support-framework-2.png)

Gestionnaire du runtime applique le correctif lorsqu’il est requis par l’application s’exécute à l’intérieur d’un conteneur MSIX.

Ce guide vous aidera à identifier les problèmes de compatibilité des applications et pour trouver, d’appliquer et d’étendre le runtime résout qui y remédier.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identifier les problèmes de compatibilité d’application empaquetés

Tout d’abord, créez un package pour votre application. Ensuite, installez-le, l’exécuter et observer son comportement. Vous pouvez recevoir des messages d’erreur qui peuvent vous aider à identifier un problème de compatibilité. Vous pouvez également utiliser le [Moniteur de processus](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) pour identifier les problèmes.  Problèmes courants se rapportent aux hypothèses d’application en ce qui concerne les autorisations de chemin de répertoire et le programme de travail.

### <a name="using-process-monitor-to-identify-an-issue"></a>À l’aide du moniteur de processus pour identifier un problème

[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) est un utilitaire puissant pour observer les fichiers de l’application et les opérations de Registre et leurs résultats.  Cela peut vous aider à comprendre les problèmes de compatibilité des applications.  Après l’ouverture de Process Monitor, ajouter un filtre (filtre > filtre...) pour inclure uniquement les événements à partir de l’exécutable de l’application.

![Filtre d’application ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Une liste d’événements s’affiche. Pour la plupart de ces événements, le mot **SUCCESS** s’affiche dans la colonne de **résultat** .

![Événements de ProcMon](images/desktop-to-uwp/procmon_events.png)

Si vous le souhaitez, vous pouvez filtrer les événements pour n’afficher seulement les échecs.

![Succès de l’exclusion de ProcMon](images/desktop-to-uwp/procmon_exclude_success.png)

Si vous suspectez une défaillance d’accès de système de fichiers, recherchez les événements ayant échoué qui sont sous la System32/SysWOW64 ou le chemin d’accès du fichier de package. Filtres peuvent également être utile ici aussi. Démarrer au bas de cette liste et faites défiler vers le haut. Erreurs qui apparaissent au bas de cette liste ont eu lieu récemment. Attention la plupart des erreurs qui contiennent des chaînes telles que «accès refusé» et «nom/chemin d’accès non trouvé» et ignorer les choses qui ne semblent pas suspects. Le [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) a deux problèmes. Vous pouvez voir ces problèmes dans la liste qui s’affiche dans l’image suivante.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

Dans le premier numéro qui s’affiche dans cette image, l’application ne peut pas lire le fichier «Config.txt» qui se trouve dans le chemin d’accès «C:\Windows\SysWOW64». Il est peu probable que l’application essaie de référencer directement de ce chemin d’accès. Il est probable qu’il essaie de lire à partir de ce fichier à l’aide d’un chemin d’accès relatif, et par défaut, «System32/SysWOW64» est le répertoire de travail de l’application. Cela signifie que l’application attend son répertoire de travail actuel à définir quelque part dans le package. Dans l’appx, nous pouvons voir que le fichier existe dans le même répertoire que l’exécutable.

![App Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

Le deuxième problème apparaît dans l’image suivante.

![Journal de ProcMon](images/desktop-to-uwp/procmon_logfile.png)

Dans ce problème, l’application ne peut pas écrire un fichier .log à son chemin d’accès du package. Cela suggère qu’un correctif de la redirection du fichier peut-être vous aider.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Publication d’un correctif de runtime

Les fibres discontinues de polyesters contient les correctifs de runtime que vous pouvez utiliser tout de suite, tels que le shim de redirection du fichier.

### <a name="file-redirection-shim"></a>Fichier de Redirection Shim

Vous pouvez utiliser le [Fichier de Redirection Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) pour rediriger les tentatives d’écrire ou de lire des données dans un répertoire qui n’est pas accessible à partir d’une application qui s’exécute dans un conteneur MSIX.

Par exemple, si votre application écrit dans un fichier journal qui se trouve dans le même répertoire que vos applications exécutables, vous pouvez utiliser le [Fichier de Redirection Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) pour créer ce fichier journal dans un autre emplacement, comme le magasin de données d’application local.

### <a name="runtime-fixes-from-the-community"></a>Correctifs de runtime à partir de la Communauté

Veillez à passer en revue les contributions de la Communauté à notre page de [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) . Il est possible que d’autres développeurs ont résolu un problème similaire au vôtre et ont partagé un correctif du runtime.

## <a name="apply-a-runtime-fix"></a>Appliquer un correctif du runtime

Vous pouvez appliquer un correctif runtime existants avec quelques outils simples dans le Kit de développement Windows et en suivant ces étapes.

> [!div class="checklist"]
> * Créer un dossier de mise en page de package
> * Obtenir les fichiers de l’infrastructure de prise en charge de Package
> * Les ajouter à votre package.
> * Modifier le manifeste du package
> * Créer un fichier de configuration

Examinons chaque tâche.

### <a name="create-the-package-layout-folder"></a>Créer le dossier de mise en page

Si vous disposez déjà d’un fichier .aspx, vous pouvez décompresser son contenu dans un dossier de mise en page qui servira à la zone de transit pour votre package.  Vous pouvez le faire à partir d’un **x64 natif invite de commande des outils de VS 2017**, ou manuellement avec le chemin bin SDK dans le chemin de recherche exécutable.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

Cela vous donnera quelque chose qui ressemble à la suivante.

![Disposition de lot](images/desktop-to-uwp/package_contents.png)

Si vous n’avez pas à démarrer avec un fichier .aspx, vous pouvez créer le dossier du package et les fichiers à partir de zéro.

### <a name="get-the-package-support-framework-files"></a>Obtenir les fichiers de l’infrastructure de prise en charge de Package

Vous pouvez obtenir le package Nuget de fibres discontinues de polyesters à l’aide de Visual Studio. Vous pouvez également l’obtenir à l’aide de l’outil de ligne de commande autonome Nuget.

#### <a name="get-the-package-by-using-visual-studio"></a>Obtenir le package à l’aide de Visual Studio

Dans Visual Studio, cliquez sur le nœud de votre projet ou solution et choisir une des commandes Manage Nuget Packages.  Rechercher les **Microsoft.PackageSupportFramework** ou les **fibres discontinues de polyesters** à rechercher le package sur Nuget.org. Ensuite, installez-le.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtenir le package à l’aide de l’outil de ligne de commande

Installer l’outil de ligne de commande de Nuget à partir de cet emplacement: https://www.nuget.org/downloads. Puis, à partir de la ligne de commande de Nuget, exécutée la commande suivante:

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>Ajouter les fichiers de l’infrastructure de prise en charge de Package à votre package

Ajouter la DLL de fibres discontinues de polyesters requis 32 bits et 64 bits et les fichiers exécutables dans le répertoire du package. Inspirez-vous du tableau suivant. Vous allez également d’inclure tous les correctifs de runtime dont vous avez besoin. Dans notre exemple, nous devons le correctif runtime la redirection.

| Exécutable de l’application est x64 | Exécutable de l’application est x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

Le contenu de votre package doit maintenant ressembler à ceci.

![Fichiers binaires du package](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modifier le manifeste du package

Ouvrez votre manifeste de package dans un éditeur de texte et définissez la `Executable` attribut de la `Application` élément sur le nom du fichier exécutable Lanceur shim.  Si vous connaissez l’architecture de votre application cible, sélectionnez la version appropriée, ShimLauncher32.exe ou ShimLauncher64.exe.  Si ce n’est pas le cas, ShimLauncher32.exe fonctionne dans tous les cas.  En voici un exemple.

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

Créer un nom de fichier ``config.json``et enregistrez-le dans le dossier racine de votre package. Modifier l’ID app déclarée du fichier config.json pour pointer vers le fichier exécutable qui vient d’être remplacé. À l’aide de la base de connaissances que vous avez obtenus à partir de l’utilisation du moniteur de processus, vous pouvez également définir le répertoire de travail ainsi que le shim de redirection du fichier permet de rediriger des lectures/écritures vers les fichiers .log dans le répertoire «PSFSampleApp» de relatifs au package.

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
Voici un guide pour le schéma de config.json:

| Tableau | key | Valeur |
|-------|-----------|-------|
| applications | id |  Utilisez la valeur de la `Id` l’attribut de la `Application` élément dans le manifeste du package. |
| applications | exécutable | Le chemin d’accès relatif à un package de l’exécutable que vous souhaitez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste du package avant de le modifier. Il s’agit de la valeur de la `Executable` l’attribut de la `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès relatif à un package à utiliser comme répertoire de travail de l’application démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et le chemin d’accès supprimée. |
| shim | DLL | Chemin relatif à un package le shim .aspx à charger. |
| shim | config | (Facultatif) Contrôle le comportement de la liste de distribution du shim. La syntaxe exacte de cette valeur varie selon le shim-par-shim que chaque correctif peut interpréter ce «blob» qu’il le souhaite. |

Le `applications`, `processes`, et `shims` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plus d’une application, les processus et les DLL du shim.


### <a name="package-and-test-the-app"></a>Package et l’application de Test

Ensuite, créez un package.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

Ensuite, le signer.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

Pour plus d’informations, consultez [comment créer un package de signature de certificat](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) et [signer un package à l’aide de signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

À l’aide de PowerShell, installez le package.

>[!NOTE]
> N’oubliez pas de d’abord désinstaller le package.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

Exécutez l’application et observer le comportement avec le correctif runtime.  Répétez les étapes de conditionnement si nécessaire et le diagnostic.

### <a name="use-the-trace-shim"></a>Utilisez le Shim de Trace

Une autre technique à diagnostiquer les problèmes de compatibilité d’application package est d’utiliser le Shim de Trace. Cette DLL est incluse dans les fibres discontinues de polyesters et fournit une vue détaillée de diagnostic du comportement de l’application, semblable à un moniteur de traitement.  Il est spécialement conçu pour révéler des problèmes de compatibilité d’application.  Pour utiliser le Shim de Trace, ajouter la DLL au package, ajouter le fragment suivant à votre config.json et puis d’empaqueter et installer votre application.

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

Par défaut, le Shim de Trace filtre les défaillances qui peuvent être considérés comme des «prévus».  Par exemple, les applications peuvent tenter sans condition de supprimer un fichier sans vérifier s’il existe déjà, en ignorant le résultat. Cela a le malheureuses de certaines défaillances inattendues peuvent obtenir filtrés, dans l’exemple ci-dessus, nous choisir de recevoir tous les échecs liés à partir de fonctions de système de fichiers. Cela nous parce que nous savons de l’avant que la tentative de lecture à partir du fichier Config.txt échoue avec le message «fichier introuvable». Il s’agit d’une défaillance qui est fréquemment observée et pas censée pour être inattendu. Dans la pratique, il est susceptible de mieux commencer le filtrage uniquement aux défaillances inattendues et puis retomber à tous les échecs s’il existe un problème qui ne peut pas toujours être identifié.

Par défaut, la sortie de la Trace du Shim est envoyée vers le débogueur attaché. Pour cet exemple, nous ne va attacher un débogueur et utilisera à la place le programme [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) de SysInternals pour consulter sa sortie. Après l’exécution de l’application, nous voyons les pannes mêmes comme avant, qui nous pointeraient vers les mêmes correctifs de runtime.

![TraceShim fichier introuvable](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim accès refusé](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Déboguer, étendre ou créer un correctif du runtime

Vous pouvez utiliser Visual Studio pour déboguer un correctif du runtime, étendre un correctif runtime ou en créer un de toutes pièces. Vous devez effectuer ces opérations pour réussir.

> [!div class="checklist"]
> * Ajoutez un projet d’emballage
> * Ajouter le projet pour la résolution de runtime
> * Ajoutez un projet qui démarre le Lanceur Shim exécutable
> * Configurer le projet d’emballage

Lorsque vous avez terminé, votre solution doit ressembler à ceci.

![Solution terminée](images/desktop-to-uwp/runtime-fix-project-structure.png)

Examinons chaque projet dans cet exemple.

| Projet | Objectif |
|-------|-----------|
| DesktopApplicationPackage | Ce projet est basé sur le [projet de réorganisation des applications Windows](desktop-to-uwp-packaging-dot-net.md) et il génère le package MSIX. |
| Runtimefix | Il s’agit d’un projet de bibliothèque de Dynamic-Linked C++ qui contient une ou plusieurs fonctions de remplacement qui servent à la résolution du runtime. |
| ShimLauncher | Il s’agit de projet C++ vide. Ce projet est un endroit pour collecter les fichiers d’exécution distribuable de l’infrastructure de prise en charge du Package. Il génère un fichier exécutable. Cet exécutable est la première chose qui s’exécute lorsque vous démarrez la solution. |
| WinFormsDesktopApplication | Ce projet contient le code source d’une application de bureau. |

Pour consulter un exemple complet qui contient tous ces types de projets, consultez [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Nous allons étudier les étapes pour créer et configurer chacun de ces projets dans votre solution.


### <a name="create-a-package-solution"></a>Créer une package solution

Si vous ne disposez pas d’une solution pour votre application de bureau, créer une **Nouvelle Solution** dans Visual Studio.

![Nouvelle solution](images/desktop-to-uwp/blank-solution.png)

Vous pouvez également ajouter des projets d’application que vous avez.

### <a name="add-a-packaging-project"></a>Ajoutez un projet d’emballage

Si vous ne disposez pas d’un **Projet d’emballage Windows Application**, en créer un et l’ajouter à votre solution.

![Modèle de projet de package](images/desktop-to-uwp/package-project-template.png)

Pour plus d’informations sur le projet de réorganisation des applications Windows, consultez le [Package de votre application à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md).

Dans l' **Explorateur de solutions**, droit sur le projet d’emballage et sélectionnez **Modifier**puis ajoutez ceci à la fin du fichier projet:

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

### <a name="add-project-for-the-runtime-fix"></a>Ajouter le projet pour la résolution de runtime

Ajoutez un projet C++, **Bibliothèque de liens dynamiques (DLL)** à la solution.

![Bibliothèque d’exécution correctif](images/desktop-to-uwp/runtime-fix-library.png)

Cliquez du bouton droit que de projet, puis choisissez **Propriétés**.

Dans les pages de propriétés, recherchez le champ **Standard du langage C++** et puis dans la liste déroulante en regard de ce champ, sélectionnez le **C ++ 17 norme ISO (/ std:c ++ 17)** option.

![ISO 17 Option](images/desktop-to-uwp/iso-option.png)

Cliquez droit sur le projet et puis dans le menu contextuel, sélectionnez l’option **Manage Nuget Packages** . Assurez-vous que l’option de **source du Package** est définie à **tous les** ou **nuget.org**.

Cliquez sur l’icône paramètres suivant ce champ.

Recherche de *fibres discontinues de polyesters** Nuget empaqueter et installez-le pour ce projet.

![package NuGet](images/desktop-to-uwp/psf-package.png)

Si vous souhaitez déboguer ou étendre un correctif d’exécution existant, ajoutez les fichiers exécutables du correctif que vous avez obtenu en utilisant les instructions décrites dans la section [Rechercher un correctif du moment de l’exécution](#find) de ce guide.

Si vous souhaitez créer un nouveau correctif, n’ajoutent rien à ce projet encore. Nous allons vous aider à ajouter les fichiers corrects pour ce projet, plus loin dans ce guide. Pour l’instant, nous allons continuer la configuration de votre solution.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>Ajoutez un projet qui démarre le Lanceur Shim exécutable

Ajoutez un projet C++ **Vide de projet** à la solution.

![Projet vide](images/desktop-to-uwp/blank-app.png)

Ajoutez le package Nuget de **fibres discontinues de polyesters** à ce projet à l’aide de la même recommandation décrite dans la section précédente.

Ouvrir les pages de propriétés du projet et dans la page Paramètres **généraux** , définissez la propriété **Name de la cible** ``ShimLauncher32`` ou ``ShimLauncher64`` en fonction de l’architecture de votre application.

![référence de Lanceur de shim](images/desktop-to-uwp/shim-exe-reference.png)

Ajouter une référence de projet au projet correctif runtime dans votre solution.

![référence de correctif d’exécution](images/desktop-to-uwp/reference-fix.png)

Avec le bouton droit de la référence et puis dans la fenêtre **Propriétés** , appliquez ces valeurs.

| Propriété | Valeur |
|-------|-----------|-------|
| Copie locale | Vrai |
| Copie des assemblys satellites locaux | Vrai |
| Sortie d’Assembly de référence | Vrai |
| Dépendances de bibliothèque de liens | Faux |
| Entrées de dépendance de bibliothèque de lien | Faux |

### <a name="configure-the-packaging-project"></a>Configurer le projet d’emballage

Dans le projet de mise en package, cliquez avec le bouton droit sur le dossier **Applications**, puis sélectionnez sur **Ajouter une référence**.

![Ajouter une référence de projet](images/desktop-to-uwp/add-reference-packaging-project.png)

Choisissez le projet de Lanceur de shim et de votre projet d’application de bureau et cliquez sur le bouton **OK** .

![Projet de bureau](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si vous n’avez pas le code source pour votre application, choisissez le projet de Lanceur de shim. Nous vous montrerons comment référencer votre fichier exécutable lorsque vous créez un fichier de configuration.

Dans le nœud **Applications** , avec le bouton droit à l’application de lancement de shim et puis sélectionnez **définir comme Point d’entrée**.

![Définir le point d’entrée](images/desktop-to-uwp/set-startup-project.png)

Ajoutez un fichier nommé ``config.json`` à votre projet d’emballage, puis, copiez et collez le texte json suivant dans le fichier. Définir la propriété **Action de Package** de **contenu**.

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
Fournir une valeur pour chaque clé. Utilisez ce tableau comme guide.

| Tableau | key | Valeur |
|-------|-----------|-------|
| applications | id |  Utilisez la valeur de la `Id` l’attribut de la `Application` élément dans le manifeste du package. |
| applications | exécutable | Le chemin d’accès relatif à un package de l’exécutable que vous souhaitez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste du package avant de le modifier. Il s’agit de la valeur de la `Executable` l’attribut de la `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès relatif à un package à utiliser comme répertoire de travail de l’application démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et le chemin d’accès supprimée. |
| shim | DLL | Chemin relatif à un package de la DLL à charger du shim. |
| shim | config | (Facultatif) Contrôle le comportement de la liste de distribution du shim. La syntaxe exacte de cette valeur varie selon le shim-par-shim que chaque correctif peut interpréter ce «blob» qu’il le souhaite. |

Lorsque vous avez terminé, votre ``config.json`` fichier devrait ressembler à ceci.

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
> Le `applications`, `processes`, et `shims` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plus d’une application, les processus et les DLL du shim.

### <a name="debug-a-runtime-fix"></a>Déboguer un correctif du runtime

Dans Visual Studio, appuyez sur F5 pour démarrer le débogueur.  La première chose qui démarre est l’application de lancement shim, qui à son tour, lance votre application de bureau cible.  Pour déboguer l’application de bureau cible, vous devrez attacher manuellement le processus de l’application de bureau en choisissant **Déboguer**->**Attacher au processus**et en sélectionnant ensuite le processus d’application. Pour autoriser le débogage d’une application .NET avec une résolution native runtime DLL, sélectionnez les types de code managé et natif (débogage en mode mixte).  

Une fois que vous avez configuré cette, vous pouvez définir des points d’arrêt en regard des lignes de code dans le code de l’application de bureau et le projet de correctif du runtime. Si vous n’avez pas le code source pour votre application, vous pourrez définir des points d’arrêt qu’à côté des lignes de code dans votre projet de correctif du runtime.

>[!NOTE]
> Lorsque Visual Studio vous donne l’évolution la plus simple et l’expérience de débogage, il y a certaines limitations, plus loin dans ce guide, nous aborderons les autres techniques de débogage que vous pouvez appliquer.

## <a name="create-a-runtime-fix"></a>Créer un correctif du runtime

Si il n’est pas encore un runtime correctif pour le problème que vous souhaitez résoudre, vous pouvez créer un nouveau correctif de runtime en écrivant des fonctions de remplacement, y compris les données de configuration qui est logique. Examinons chaque partie.

### <a name="replacement-functions"></a>Fonctions de remplacement

Tout d’abord, identifier quelle fonction appels échouent lorsque votre application s’exécute dans un conteneur MSIX. Ensuite, vous pouvez créer des fonctions de remplacement que vous souhaitez voir gestionnaire du runtime pour appeler à la place. Cela vous donne la possibilité de remplacer l’implémentation d’une fonction avec un comportement qui est conforme aux règles de l’environnement d’exécution moderne.

Dans Visual Studio, ouvrez le projet de correctif d’exécution que vous avez créé précédemment dans ce guide.

Déclarer la ``SHIM_DEFINE_EXPORTS`` macro puis ajoutez une instruction include pour le `shim_framework.h` en haut de chacune. Fichier dans lequel vous souhaitez ajouter les fonctions de votre correctif de runtime.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>Assurez-vous que le `SHIM_DEFINE_EXPORTS` macro s’affiche avant l’instruction d’inclusion.

Créez une fonction qui a la même signature de la fonction qui a comportement que vous souhaitez modifier. Voici un exemple de fonction qui remplace la `MessageBoxW` fonction.

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

L’appel à `DECLARE_SHIM` mappe le `MessageBoxW` pour votre nouvelle fonction de remplacement. Lorsque votre application tente d’appeler le `MessageBoxW` fonction, il appelle la fonction de remplacement à la place.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protection contre les appels récursifs à des fonctions de runtime réparations

Vous pouvez éventuellement appliquer le `reentrancy_guard` type à vos fonctions de protection contre les appels récursifs à des fonctions de runtime correctifs.

Par exemple, cela peut donner une fonction de remplacement pour le `CreateFile` fonction. Votre implémentation peut appeler le `CopyFile` fonction, mais la mise en oeuvre de la `CopyFile` fonction peut appeler le `CreateFile` fonction. Cela peut conduire à un cycle récurrent infini d’appels de la `CreateFile` fonction.

Pour plus d’informations sur `reentrancy_guard` voir [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Données de configuration

Si vous souhaitez ajouter des données de configuration pour votre correctif de runtime, pensez à l’ajouter à la ``config.json``. De cette façon, vous pouvez utiliser la `ShimQueryCurrentDllConfig` d’analyser facilement les données. Cet exemple analyse une valeur de type boolean et string à partir de ce fichier de configuration.

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

Tandis que Visual Studio offre un développement plus simple et l’expérience de débogage, il existe certaines limitations.

Tout d’abord, le débogage F5 exécute l’application par le déploiement des fichiers à part dans le chemin d’accès du dossier de mise en page package, plutôt que de l’installation d’un package de .aspx.  Le dossier de mise en page ne possède généralement pas les mêmes restrictions de sécurité sous la forme d’un dossier de package installé. Par conséquent, il peut être pas possible de reproduire les erreurs de refus d’accès package chemin avant d’appliquer un correctif de runtime.

Pour résoudre ce problème, utilisez le déploiement du package .aspx au lieu de déploiement de fichier libre F5.  Pour créer un fichier de package .aspx, utilisez l’utilitaire [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) à partir du Kit de développement logiciel Windows, comme décrit ci-dessus. Ou, à partir de dans Visual Studio, cliquez droit sur le nœud de projet de votre application et sélectionnez **banque**->**Créer des Packages App**.

Un autre problème avec Visual Studio est qu’il n’a pas de prise en charge intégrée pour les attacher à des processus enfants lancés par le débogueur.   Cela rend difficile à déboguer la logique dans le chemin d’accès de démarrage de l’application cible, qui doit être liée manuellement par Visual Studio après le lancement.

Pour résoudre ce problème, utilisez un débogueur qui prend en charge l’attachement de processus enfant.  Notez qu’il n’est généralement pas possible d’attacher un débogueur (JIT) de juste-à-temps à l’application cible.  C’est parce que la plupart des techniques JIT implique le lancement automatique du débogueur à la place de l’application cible, via la clé de Registre ImageFileExecutionOptions.  Cela va à l’encontre du mécanisme de detouring utilisé par ShimLauncher.exe pour injecter des ShimRuntime.dll dans l’application cible.  WinDbg, inclus dans les [Outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)et obtenu à partir du [Kit de développement Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), joindre des processus enfant de prise en charge.  Il prend également en charge directement [le lancement et débogage d’une application UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Pour déboguer le démarrage de l’application cible sous la forme d’un processus enfant, démarrez ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

À la ``WinDbg`` invite, d’activer le débogage des enfants et de définir des points d’arrêt appropriés.

```
.childdbg 1
g
```
(exécuter en tant que l’application cible démarre et s’arrête dans le débogueur)

```
sxe ld fixup.dll
g
```
(exécuter jusqu'à ce que la DLL est chargée de correction)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) peut également être utilisé pour attacher un débogueur à une application lors du lancement et est également inclus dans les [Outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Toutefois, il est plus complexe à utiliser que la prise en charge directe par WinDbg.

## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
