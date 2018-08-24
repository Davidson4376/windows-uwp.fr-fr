---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Résoudre les problèmes qui empêchent votre application de bureau de s’exécuter dans un conteneur MSIX
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2841361"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Appliquer les correctifs de runtime à un package MSIX à l’aide de l’infrastructure de prise en charge de Package

L’infrastructure de prise en charge de Package est un kit open source qui vous permet d’appliquer des correctifs pour votre application win32 existante lorsque vous n’avez pas accès au code source, afin qu’il peut s’exécuter dans un conteneur MSIX. L’infrastructure de prise en charge de Package permet à votre application de suivre les meilleures pratiques de l’environnement d’exécution moderne.

Pour créer l’infrastructure de prise en charge de Package, nous avons exploité la technologie [Detours](https://www.microsoft.com/en-us/research/project/detours) qui est une infrastructure open source développée par Microsoft Research (MSR) et d’aide à la redirection de l’API et l’accrochage.

Cette structure est open source, léger, et vous pouvez l’utiliser pour résoudre les problèmes rapidement. Il vous donne également la possibilité de consulter la Communauté dans le monde et pour générer par-dessus les investissements d’autres personnes.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Un aperçu rapide dans le cadre de la prise en charge de Package

L’infrastructure de prise en charge de Package contient un fichier exécutable, un gestionnaire de runtime DLL et un ensemble de correctifs de runtime.

![Infrastructure de prise en charge de package](images/desktop-to-uwp/package-support-framework.png)

Voici le principe: Vous allez créer un fichier de configuration qui spécifie le fix(s) que vous souhaitez appliquer à votre application. Puis, vous allez modifier votre package afin de pointer vers le fichier exécutable de lancement de shim.

Lorsque les utilisateurs démarrent votre application, le Lanceur de shim est le premier exécutable qui s’exécute. Il lit le fichier de configuration et insère le runtime fix(s) et le Gestionnaire de runtime DLL dans le processus d’application.

![Injection de DLL Framework package prise en charge](images/desktop-to-uwp/package-support-framework-2.png)

Le responsable de l’exécution s’applique le correctif lorsqu’il est nécessaire à l’application s’exécute dans un conteneur MSIX.

Ce guide vous aide à identifier les problèmes de compatibilité des applications, et pour trouver, appliquer et étendre le runtime résout qui les résoudre.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identifier les problèmes de compatibilité de package d’application

Tout d’abord, créez un package pour votre application. Ensuite, installez-le, exécutez-le et observer son comportement. Vous pouvez recevoir des messages d’erreur qui peuvent vous aider à identifier un problème de compatibilité. Vous pouvez également utiliser le [Moniteur de processus](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) pour identifier les problèmes.  Problèmes courants concernent les hypothèses application concernant les autorisations de chemin d’accès annuaire et le programme de travail.

### <a name="using-process-monitor-to-identify-an-issue"></a>Utilisation du moniteur de processus pour identifier un problème

[Moniteur de processus](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) est un utilitaire puissant pour observer les fichiers d’une application et d’opérations de Registre et leurs résultats.  Cela peut vous aider à comprendre les problèmes de compatibilité des applications.  Après avoir ouvert le moniteur de processus, ajouter un filtre (filtre > filtre...) pour inclure uniquement les événements à partir de l’exécutable de l’application.

![Filtre d’application ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Une liste d’événements s’affiche. Pour la plupart de ces événements, le mot **SUCCESS** s’affiche dans la colonne **résultat** .

![Événements ProcMon](images/desktop-to-uwp/procmon_events.png)

Si vous le souhaitez, vous pouvez filtrer les événements pour afficher uniquement les échecs uniquement.

![Exclure ProcMon réussite](images/desktop-to-uwp/procmon_exclude_success.png)

Si vous pensez qu’un échec d’accès du système de fichiers, recherchez les événements ayant échoués qui sont sous le System32/SysWOW64 ou le chemin d’accès du fichier de package. Filtres permet également ici, trop. Démarrer au bas de cette liste et faites défiler vers le haut. Échecs qui s’affichent en bas de cette liste se sont produites récemment. Attention la plupart des erreurs qui contiennent des chaînes telles que «accès refusé» et «chemin d’accès/nom introuvable» et ignorer les éléments qui ne semblent pas suspects. Le [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) a deux problèmes. Vous pouvez voir ces problèmes dans la liste qui apparaît dans l’image suivante.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

Dans le premier numéro qui s’affiche dans cette image, l’application ne parvient pas à lire à partir du fichier «Config.txt» qui se trouve dans le chemin d’accès «C:\Windows\SysWOW64». Il est peu probable que l’application essaie de référencer directement de ce chemin d’accès. Le plus souvent, il tente de lire à partir de ce fichier à l’aide d’un chemin d’accès relatif et par défaut, «System32/SysWOW64» est le répertoire de travail de l’application. Cela signifie que l’application attend son répertoire de travail actuel pour définir quelque part dans le package. Recherchez à l’intérieur du appx, nous constatons que le fichier existe dans le même répertoire que le fichier exécutable.

![Application Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

Le deuxième problème apparaît dans l’image suivante.

![Fichier journal ProcMon](images/desktop-to-uwp/procmon_logfile.png)

Dans ce problème, l’application ne parvient pas à écrire un fichier .log dans son chemin d’accès du package. Cela suggère qu’un fichier redirection de shim peut-être vous aider.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Trouver un correctif runtime

CELLES contient des correctifs runtime que vous pouvez utiliser, tels que le shim de redirection du fichier.

### <a name="file-redirection-shim"></a>Fichier Redirection Shim

Vous pouvez utiliser le [Fichier Redirection Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) pour rediriger les tentatives d’écrire ou lire des données dans un répertoire qui n’est pas accessible à partir d’une application qui s’exécute dans un conteneur MSIX.

Par exemple, si votre application écrit dans un fichier journal qui se trouve dans le même répertoire que vos applications exécutables, vous pouvez utiliser le [Fichier Redirection Shim](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) pour créer ce fichier journal dans un autre emplacement, tel que le magasin de données d’application local.

### <a name="runtime-fixes-from-the-community"></a>Correctifs de runtime de la Communauté

Veillez à consulter les contributions de nos [référentiels](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) de page de la Communauté. Il est possible que d’autres développeurs ont résolu un problème similaire à la vôtre et ont partagé un correctif de runtime.

## <a name="apply-a-runtime-fix"></a>Appliquer un correctif runtime

Vous pouvez appliquer un correctif runtime existante avec quelques outils simples dans le SDK de Windows et en suivant ces étapes.

> [!div class="checklist"]
> * Créez un dossier de mise en package
> * Obtenir les fichiers de l’infrastructure de prise en charge de Package
> * Les ajouter à votre package
> * Modifier le manifeste du package
> * Créer un fichier de configuration

Examinons chaque tâche.

### <a name="create-the-package-layout-folder"></a>Créer le dossier de mise en package

Si vous disposez déjà d’un fichier .aspx, vous pouvez décompresser son contenu dans un dossier de mise en page qui servira de la zone de transit pour votre package.  Vous pouvez le faire à partir d’un **x64 invite de commandes outils natifs pour VS 2017**, ou manuellement avec le chemin d’accès de la Corbeille SDK dans le chemin de recherche exécutable.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

Vous obtiendrez quelque chose qui ressemble à ceci.

![Mise en package](images/desktop-to-uwp/package_contents.png)

Si vous n’avez pas à démarrer avec un fichier .aspx, vous pouvez créer le dossier de package et les fichiers à partir de zéro.

### <a name="get-the-package-support-framework-files"></a>Obtenir les fichiers de l’infrastructure de prise en charge de Package

Vous pouvez obtenir le package Nuget produits à l’aide de Visual Studio. Vous pouvez également l’obtenir à l’aide de l’outil de ligne de commande autonome Nuget.

#### <a name="get-the-package-by-using-visual-studio"></a>Obtenir le package à l’aide de Visual Studio

Dans Visual Studio, cliquez sur le nœud de votre solution ou un projet et choisissez une des commandes de gérer les Packages Nuget.  Rechercher **Microsoft.PackageSupportFramework** ou **produits** rechercher le package sur Nuget.org. Installez ensuite.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtenir le package à l’aide de l’outil de ligne de commande

Installer l’outil de ligne de commande Nuget à partir de cet emplacement: https://www.nuget.org/downloads. Puis, à partir de la ligne de commande Nuget, exécutée la commande suivante:

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>Ajouter les fichiers de prise en charge infrastructure du Package à votre package

Ajoutez les DLL de produits 32 bits et 64 bits requis et les fichiers exécutables dans le répertoire du package. Inspirez-vous du tableau suivant. Vous souhaiterez également inclure tous les correctifs runtime dont vous avez besoin. Dans notre exemple, nous devons le correctif redirection runtime.

| Exécutable de l’application est x64 | Exécutable de l’application est x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

Le contenu de votre package doit maintenant ressembler à ceci.

![Fichiers binaires du package](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modifier le manifeste du package

Ouvrez le manifeste de votre package dans un éditeur de texte, puis définissez le `Executable` attribut de le `Application` élément sur le nom du fichier exécutable shim lancement.  Si vous connaissez l’architecture de votre application cible, sélectionnez la version appropriée, ShimLauncher32.exe ou ShimLauncher64.exe.  Si ce n’est pas le cas, ShimLauncher32.exe fonctionneront dans tous les cas.  En voici un exemple.

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

Créer un nom de fichier ``config.json``et enregistrez ce fichier dans le dossier racine de votre package. Modifier l’ID d’application déclaré du fichier config.json pour pointer vers le fichier exécutable qui vient d’être remplacé. À l’aide de la base de connaissances que vous avez obtenus à partir de l’utilisation du moniteur de processus, vous pouvez également définir le répertoire de travail ainsi que le shim de redirection du fichier permet de rediriger les opérations de lecture/écriture pour les fichiers journaux dans le répertoire de «PSFSampleApp» relative au package.

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
| applications | exécutable | Le chemin d’accès de package relative à l’exécutable que vous souhaitez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste de package avant de le modifier. Il s’agit de la valeur de la `Executable` attribut de le `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès de package relative à utiliser en tant que le répertoire de travail de l’application démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et chemin d’accès supprimée. |
| shims | DLL | Chemin relatif de package du shim .aspx à charger. |
| shims | Options de configuration | (Facultatif) Contrôle le comportement de la liste de distribution shim. Le format de cette valeur exact varie sur une base de shim-par-shim comme chaque shim peut interpréter ce «blob» qu’il le souhaite. |

Le `applications`, `processes`, et `shims` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plusieurs applications, processus et DLL du shim.


### <a name="package-and-test-the-app"></a>L’application de Test et le package

Ensuite, créez un package.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

Ensuite, vous connecter.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

Pour plus d’informations, voir [comment créer un package de certificat de signature](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) et [comment signer un package à l’aide de signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

À l’aide de PowerShell, installez le package.

>[!NOTE]
> N’oubliez pas d’abord désinstaller le package.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

Exécuter l’application et observez le comportement avec le module d’exécution du correctif.  Répétez les étapes de packaging selon les besoins et de diagnostic.

### <a name="use-the-trace-shim"></a>Utiliser le suivi du Shim

Une autre technique pour diagnostiquer les problèmes de compatibilité marchandises consiste à utiliser le suivi du Shim. Cette DLL est incluse avec les produits et fournit une vue détaillée des diagnostics de comportement de l’application, semblable à un moniteur de traitement.  Elle est conçue spécialement pour découvrir des problèmes de compatibilité des applications.  Pour utiliser le suivi du Shim, ajouter la DLL vers le package, ajouter le fragment suivant à votre config.json, puis d’empaqueter et installer votre application.

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

Par défaut, le suivi de Shim filtre les erreurs qui peuvent être considérés comme «prévus».  Par exemple, applications peuvent tenter de se branche sans condition supprimer un fichier sans vérifier s’il existe déjà, en ignorant le résultat. Ce fait malheureusement que certains échecs inattendus peuvent obtenir filtrés, afin que dans l’exemple ci-dessus, nous opter pour la réception de tous les échecs de fonctions du système de fichiers. Pour ce faire, car nous savons d’avant que la tentative de lecture à partir du fichier Config.txt échoue avec le message «fichier introuvable». Il s’agit d’une défaillance qui est souvent observée et n’est pas généralement supposée pour être inattendu. En pratique, il est probablement mieux pour démarrer le filtrage uniquement aux échecs inattendus, et puis revenant à tous les échecs s’il existe un problème toujours ne peut pas être identifié.

Par défaut, la sortie de la Trace du Shim est envoyée au débogueur attaché. Dans cet exemple, nous ne sont pas sur le point d’attacher un débogueur et le programme [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) de SysInternals utilisera pour consulter sa sortie. Après l’exécution de l’application, nous constatons les échecs de même qu’auparavant, qui nous pointez vers les correctifs runtime même.

![Fichier TraceShim introuvable](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim accès refusé](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Déboguer, étendre ou créer un correctif runtime

Vous pouvez utiliser Visual Studio pour déboguer un correctif runtime, d’étendre un correctif runtime ou créez-en un. Vous devez effectuer ces opérations aboutisse.

> [!div class="checklist"]
> * Ajouter un projet d’empaquetage
> * Ajouter le projet pour la résolution de runtime
> * Ajouter un projet qui démarre le Lanceur Shim exécutable
> * Configurer le projet d’empaquetage

Lorsque vous avez terminé, votre solution le ressemblera à ceci.

![Solution terminée](images/desktop-to-uwp/runtime-fix-project-structure.png)

Examinons chaque projet dans cet exemple.

| Projet | Objectif |
|-------|-----------|
| DesktopApplicationPackage | Ce projet est basé sur le [projet d’empaquetage d’Application Windows](desktop-to-uwp-packaging-dot-net.md) et elle renvoie le package MSIX. |
| Runtimefix | Il s’agit d’un projet de bibliothèque de Dynamic-Linked C++ qui contient une ou plusieurs fonctions de remplacement qui servent à la résolution de l’exécution. |
| ShimLauncher | Il s’agit de projet C++ vide. Ce projet est un emplacement pour collecter les fichiers distribuable d’exécution de l’infrastructure de prise en charge de Package. Il génère un fichier exécutable. Ce fichier exécutable est la première chose qui s’exécute lorsque vous démarrez la solution. |
| WinFormsDesktopApplication | Ce projet contient le code source d’une application de bureau. |

Pour consulter un exemple complet qui contient tous ces types de projets, voir [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Examinons les étapes pour créer et configurer chacun de ces projets dans votre solution.


### <a name="create-a-package-solution"></a>Créer une package solution

Si vous ne disposez pas d’une solution pour votre application de bureau, créez une nouvelle **Solution vide** dans Visual Studio.

![Solution vide](images/desktop-to-uwp/blank-solution.png)

Vous souhaiterez également ajouter des projets d’application qu'avoir.

### <a name="add-a-packaging-project"></a>Ajouter un projet d’empaquetage

Si vous ne disposez pas d’un **Projet de création d’un package d’Application Windows**, créez-en un et l’ajouter à votre solution.

![Modèle de projet de package](images/desktop-to-uwp/package-project-template.png)

Pour plus d’informations sur le projet d’empaquetage d’Application Windows, voir le [Package de votre application à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md).

Dans **L’Explorateur de solutions**, cliquez sur le projet d’empaquetage et sélectionnez **Modifier**puis ajoutez cette URL à la partie inférieure du fichier de projet:

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

Ajoutez un projet C++ **Bibliothèque de liens dynamiques (DLL)** à la solution.

![Bibliothèque d’exécution correctif](images/desktop-to-uwp/runtime-fix-library.png)

Avec le bouton droit que project, puis choisissez **Propriétés**.

Dans les pages de propriétés, rechercher le champ **Standard du langage C++** , puis dans la liste déroulante en regard de ce champ, sélectionnez le **ISO C ++ 17 Standard (/ std:c ++ 17)** option.

![ISO 17 Option](images/desktop-to-uwp/iso-option.png)

Avec le bouton droit de ce projet, puis dans le menu contextuel, choisissez l’option **Gérer les Packages Nuget** . Assurez-vous que l’option **source du Package** est définie sur **tous les** ou **nuget.org**.

Cliquez sur l’icône paramètres suivant ce champ.

Recherche de *produits** Nuget package, puis l’installer pour ce projet.

![package NuGet](images/desktop-to-uwp/psf-package.png)

Si vous souhaitez déboguer ou étendre une solution d’exécution existant, ajoutez les fichiers du correctif runtime que vous avez obtenu à l’aide les instructions décrites dans la section [Rechercher un correctif de l’exécution](#find) de ce guide.

Si vous souhaitez créer un nouveau correctif, n’ajoutez pas rien à ce projet encore. Nous allons vous aider à ajouter les fichiers de droite à ce projet plus loin dans ce guide. À ce stade, nous allons continuer la configuration de votre solution.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>Ajouter un projet qui démarre le Lanceur Shim exécutable

Ajoutez un projet C++ **Projet vide** à la solution.

![Projet vide](images/desktop-to-uwp/blank-app.png)

Ajoutez le package Nuget **produits** à ce projet à l’aide de la même instructions décrites dans la section précédente.

Ouvrir les pages de propriétés pour le projet et dans la page Paramètres **généraux** , définissez la propriété **Nom de la cible** sur ``ShimLauncher32`` ou ``ShimLauncher64`` en fonction de l’architecture de votre application.

![référence de lanceur shim](images/desktop-to-uwp/shim-exe-reference.png)

Ajouter une référence de projet au projet correctif runtime dans votre solution.

![référence de correctif Runtime](images/desktop-to-uwp/reference-fix.png)

Avec le bouton droit de la référence, puis, dans la fenêtre **Propriétés** , appliquer ces valeurs.

| Propriété | Valeur |
|-------|-----------|-------|
| Copie locale | Vrai |
| Copier des assemblys satellites locaux | Vrai |
| Sortie d’Assembly de référence | Vrai |
| Dépendances de bibliothèque de liens | Faux |
| Entrées de dépendance de bibliothèque de liens | Faux |

### <a name="configure-the-packaging-project"></a>Configurer le projet d’empaquetage

Dans le projet de mise en package, cliquez avec le bouton droit sur le dossier **Applications**, puis sélectionnez sur **Ajouter une référence**.

![Ajouter une référence de projet](images/desktop-to-uwp/add-reference-packaging-project.png)

Choisissez le projet de lancement de shim et de votre projet d’application de bureau, puis cliquez sur le bouton **OK** .

![Projet de bureau](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si vous ne disposez pas du code source à votre application, choisissez le projet de lancement de shim. Nous allons vous montrer comment faire référence à votre fichier exécutable lorsque vous créez un fichier de configuration.

Dans le nœud **Applications** , avec le bouton droit de l’application de service de lancement shim, puis sélectionnez **définir comme Point d’entrée**.

![Définir le point d’entrée](images/desktop-to-uwp/set-startup-project.png)

Ajoutez un fichier nommé ``config.json`` à votre projet d’empaquetage, puis copiez et collez le texte suivant de json dans le fichier. Définissez la propriété **Action de Package de** **contenu**.

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
| applications | id |  Utilisez la valeur de la `Id` attribut de le `Application` élément dans le manifeste du package. |
| applications | exécutable | Le chemin d’accès de package relative à l’exécutable que vous souhaitez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste de package avant de le modifier. Il s’agit de la valeur de la `Executable` attribut de le `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès de package relative à utiliser en tant que le répertoire de travail de l’application démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et chemin d’accès supprimée. |
| shims | DLL | Chemin d’accès de package relative à la DLL de chargement du shim. |
| shims | Options de configuration | (Facultatif) Contrôle le comportement de la liste de distribution shim. Le format de cette valeur exact varie sur une base de shim-par-shim comme chaque shim peut interpréter ce «blob» qu’il le souhaite. |

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
> Le `applications`, `processes`, et `shims` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plusieurs applications, processus et DLL du shim.

### <a name="debug-a-runtime-fix"></a>Déboguer un correctif runtime

Dans Visual Studio, appuyez sur F5 pour démarrer le débogueur.  La première chose qui démarre est l’application de service de lancement shim, qui à son tour, démarre votre application de bureau cible.  Pour déboguer l’application cible de bureau, vous devrez vous attachez manuellement le processus d’application de bureau en choisissant **Déboguer**->**Attacher au processus**, puis en sélectionnant le processus d’application. Pour autoriser le débogage d’une application .NET avec un correctif natif runtime DLL, sélectionnez les types de code managé et natif (débogage en mode mixte).  

Une fois que vous avez configuré cette, vous pouvez définir des points d’arrêt en regard des lignes de code dans le code de l’application de bureau et le projet de correctif runtime. Si vous ne disposez pas du code source à votre application, vous serez en mesure de définir des points d’arrêt uniquement en regard des lignes de code dans votre projet de correctif runtime.

>[!NOTE]
> Visual Studio vous donne le développement plus simple et l’expérience de débogage, il existe certaines limitations, plus loin dans ce guide, aborder autres techniques de débogage que vous pouvez appliquer.

## <a name="create-a-runtime-fix"></a>Créer un correctif runtime

Si il n’est pas encore un runtime corriger le problème que vous souhaitez résoudre, vous pouvez créer un nouveau correctif runtime en écriture de fonctions de remplacement et en incluant des données de configuration qui est significatif. Examinons chaque composant.

### <a name="replacement-functions"></a>Fonctions de remplacement

Tout d’abord, identifiez la fonction appels échouent lorsque votre application s’exécute dans un conteneur MSIX. Ensuite, vous pouvez créer des fonctions de remplacement que vous souhaitez utiliser le Gestionnaire de runtime pour appeler à la place. Cela vous donne la possibilité de remplacer l’implémentation d’une fonction dont le comportement est conforme aux règles de l’environnement d’exécution moderne.

Dans Visual Studio, ouvrez le projet de correctif runtime que vous avez créé plus haut dans ce guide.

Déclarer la ``SHIM_DEFINE_EXPORTS`` macro et ajoutez une instruction inclure pour la `shim_framework.h` en haut de chacune. Fichier dans lequel vous souhaitez ajouter les fonctions de votre correctif runtime.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>Assurez-vous que le `SHIM_DEFINE_EXPORTS` macro apparaît avant l’instruction d’inclusion.

Créez une fonction qui possède la même signature de la fonction qui a le comportement que vous souhaitez modifier. Voici un exemple de fonction qui remplace la `MessageBoxW` fonction.

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

L’appel vers `DECLARE_SHIM` mappe le `MessageBoxW` pour votre nouvelle fonction de remplacement. Lorsque votre application tente d’appeler le `MessageBoxW` fonction, il appelle la fonction de remplacement à la place.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protection contre le récursives aux fonctions de correctifs du runtime

Vous pouvez éventuellement appliquer le `reentrancy_guard` type à vos fonctions protéger récursives aux fonctions de correctifs de runtime.

Par exemple, vous pouvez produire une fonction de remplacement pour la `CreateFile` fonction. Votre implémentation peut appeler le `CopyFile` fonction, mais l’implémentation de la `CopyFile` fonction peut appeler le `CreateFile` fonction. Cela peut entraîner un cycle récurrent infini d’appels vers le `CreateFile` fonction.

Pour plus d’informations sur `reentrancy_guard` voir [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Données de configuration

Si vous souhaitez ajouter des données de configuration pour votre correctif runtime, pensez à l’ajouter à la ``config.json``. Ainsi, vous pouvez utiliser la `ShimQueryCurrentDllConfig` facilement analyser ces données. Cet exemple analyse une valeur booléenne et la chaîne à partir de ce fichier de configuration.

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

Tandis que Visual Studio offre le développement plus simple et l’expérience de débogage, il existe certaines limitations.

Tout d’abord, débogage F5 exécute l’application par le déploiement des fichiers à part du chemin d’accès du dossier de mise en package, au lieu de l’installation d’un package .aspx.  Le dossier de mise en page ne possède généralement pas les mêmes restrictions de sécurité qu’un dossier package installé. Par conséquent, il peut être pas possible de reproduire les erreurs de refus d’accès package chemin d’accès avant d’appliquer un correctif de runtime.

Pour résoudre ce problème, utilisez le déploiement du package .aspx plutôt que de déploiement du fichier libre F5.  Pour créer un fichier de package .aspx, utilisez l’utilitaire [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) dans le SDK de Windows, comme indiqué ci-dessus. Ou, dans Visual Studio, avec le bouton droit de votre nœud de projet d’application, puis sélectionnez **magasin**->**Créer des Packages App**.

Un autre problème avec Visual Studio est qu’il n’a pas de prise en charge intégrée pour attacher à n’importe quel processus enfant lancés par le débogueur.   Cela rend difficile à déboguer logique dans le chemin d’accès de démarrage de l’application cible, qui doit être liée manuellement par Visual Studio après le lancement.

Pour résoudre ce problème, utilisez un débogueur qui prend en charge des processus enfant attacher.  Notez qu’il est généralement pas possible d’attacher un débogueur de (juste) juste-à-temps à l’application cible.  Il s’agit, car la plupart des techniques juste implique le lancement du débogueur à la place de l’application cible, par le biais de la clé de Registre ImageFileExecutionOptions.  Cette action annule le mécanisme detouring utilisé par ShimLauncher.exe injecter ShimRuntime.dll dans l’application cible.  WinDbg, inclus dans les [Outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)et obtenu à partir du [Kit de développement Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), processus enfant de prend en charge la liaison.  Il prend également en charge directement [lancement et débogage d’une application UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Pour déboguer le démarrage de l’application cible en tant qu’un processus enfant, démarrer ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

À la ``WinDbg`` demander, activer le débogage des enfants et définir des points d’arrêt appropriés.

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
