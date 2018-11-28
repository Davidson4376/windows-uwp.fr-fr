---
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Résoudre les problèmes qui empêchent votre application de bureau en cours d’exécution dans un conteneur MSIX
ms.date: 07/02/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 674f5977a69855ff51cbc579ca66085aa133eb5b
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7841774"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Appliquer des correctifs à l’exécution à un package MSIX à l’aide de l’infrastructure de prise en charge de Package

L’infrastructure de prise en charge de Package est un kit open source qui vous permet d’appliquer des correctifs à votre application win32 existantes lorsque vous n’avez pas accès au code source, afin qu’il puisse exécuter dans un conteneur MSIX. L’infrastructure de prise en charge de Package permet à votre application de suivre les meilleures pratiques de l’environnement d’exécution moderne.

Pour plus d’informations, voir [Infrastructure prise en charge du Package](https://docs.microsoft.com/windows/msix/package-support-framework-overview).

Ce guide vous aidera à identifier les problèmes de compatibilité d’application, et pour rechercher, appliquer et étendre runtime des correctifs qui y remédier.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identifier les problèmes de compatibilité d’application empaquetée

Tout d’abord, créez un package pour votre application. Ensuite, l’installer, exécutez-le et observer son comportement. Vous pouvez recevoir des messages d’erreur qui peuvent vous aider à identifier un problème de compatibilité. Vous pouvez également utiliser le [Moniteur de processus](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) pour identifier les problèmes.  Relation entre les problèmes courants et hypothèses d’application en ce qui concerne les autorisations de chemin de répertoire et le programme de travail.

### <a name="using-process-monitor-to-identify-an-issue"></a>L’utilisation du moniteur de processus pour identifier un problème

[Moniteur de processus](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) est un utilitaire puissant pour observer les fichiers d’une application et d’opérations de Registre et leurs résultats.  Cela peut vous aider à comprendre les problèmes de compatibilité d’application.  Après l’ouverture du moniteur de processus, ajouter un filtre (filtre > filtre …) pour inclure uniquement les événements à partir de l’exécutable de l’application.

![Filtre d’application ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Une liste d’événements s’affiche. Pour la plupart de ces événements, le mot **Réussite** s’affiche dans la colonne de **résultat** .

![Événements ProcMon](images/desktop-to-uwp/procmon_events.png)

Si vous le souhaitez, vous pouvez filtrer les événements pour ne visualiser que seuls les échecs.

![Réussite Exclude ProcMon](images/desktop-to-uwp/procmon_exclude_success.png)

Si vous suspectez un échec d’accès de système de fichiers, recherchez des événements ayant échoués qui se trouvent sous le System32/SysWOW64 ou le chemin d’accès du fichier de package. Filtres permettent d’ici, trop. Démarrer en bas de cette liste et faites défiler vers le haut. Échecs qui s’affichent en bas de cette liste se sont produits la plus récente. Y prêter attention la plupart des erreurs qui contiennent des chaînes telles que «accès refusé» et «chemin/nom introuvable» et ignorer les éléments qui ne semblent pas suspectes. Le [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) a deux problèmes. Vous pouvez voir ces problèmes dans la liste qui s’affiche dans l’image suivante.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

Dans le premier problème qui s’affiche dans cette image, l’application ne parvient pas à lire à partir du fichier «Config.txt» qui se trouve dans le chemin d’accès «C:\Windows\SysWOW64». Il est peu probable que l’application tente de faire référence à ce chemin d’accès directement. Il est probable qu’il essaie de lire à partir de ce fichier à l’aide d’un chemin d’accès relatif et par défaut, «System32/SysWOW64» est le répertoire de travail de l’application. Cela signifie que l’application s’attend à son répertoire de travail actuel pour être défini sur quelque part dans le package. Recherchez à l’intérieur de l’appx, nous pouvons voir que le fichier existe dans le même répertoire que le fichier exécutable.

![Application Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

Le deuxième problème s’affiche dans l’image suivante.

![Fichier journal ProcMon](images/desktop-to-uwp/procmon_logfile.png)

Dans ce problème, l’application ne parvient pas à écrire un fichier .log dans son chemin d’accès du package. Cela suggère qu’une correction de la redirection de fichier peut-être vous aider.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Trouver un correctif runtime

CELLES contient des correctifs à l’exécution que vous pouvez utiliser tout de suite, telles que la correction de la redirection de fichier.

### <a name="file-redirection-fixup"></a>Correction de la Redirection de fichier

Vous pouvez utiliser le [Fichier de Redirection de correction](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) pour rediriger les tentatives d’écrire ou de lire des données dans un répertoire qui n’est pas accessible à partir d’une application qui s’exécute dans un conteneur MSIX.

Par exemple, si votre application écrit dans un fichier journal qui se trouve dans le même répertoire que vos applications exécutables, vous pouvez utiliser le [Fichier de Redirection de correction](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) pour créer ce fichier journal dans un autre emplacement, par exemple, le magasin de données d’application locale.

### <a name="runtime-fixes-from-the-community"></a>Correctifs à l’exécution de la Communauté

Veillez à passer en revue les contributions de la Communauté à notre page [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) . Il est possible que d’autres développeurs ont résolu un problème similaire au vôtre et ont partagés un correctif runtime.

## <a name="apply-a-runtime-fix"></a>Appliquer un correctif runtime

Vous pouvez appliquer un correctif runtime existantes avec des outils simples quelques du SDK Windows et en suivant les étapes suivantes.

> [!div class="checklist"]
> * Créez un dossier de disposition de package
> * Obtenir les fichiers de l’infrastructure de prise en charge de Package
> * Ajouter à votre package
> * Modifier le manifeste du package
> * Créer un fichier de configuration

Revenons par le biais de chaque tâche.

### <a name="create-the-package-layout-folder"></a>Créer le dossier de disposition de package

Si vous disposez déjà d’un fichier .msix (ou .appx), vous pouvez décompresser son contenu dans un dossier de disposition qui fera office de la zone de transit de votre package. Vous pouvez le faire à partir d’une invite de commandes à l’aide de l’outil de makemsix, selon votre chemin d’installation du SDK, il s’agit dans lequel vous trouverez l’outil makemsix.exe sur votre PC Windows 10: x86: C:\Program Files (x86) \Windows Kits\10\bin\x86\makemsix.exe x64: C:\Program Files () x86) \Windows Kits\10\bin\x64\makemsix.exe

```ps
makemsix unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Cela vous donne en quelque chose qui ressemble à ce qui suit.

![Disposition de package](images/desktop-to-uwp/package_contents.png)

Si vous n’avez pas de commencer par un fichier .msix (ou .appx), vous pouvez créer le dossier du package et les fichiers à partir de zéro.

### <a name="get-the-package-support-framework-files"></a>Obtenir les fichiers de l’infrastructure de prise en charge de Package

Vous pouvez obtenir le package Nuget de produits à l’aide de l’outil de ligne de commande de Nuget autonome ou via Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtenir le package à l’aide de l’outil de ligne de commande

Installer l’outil de ligne de commande de Nuget à partir de cet emplacement: https://www.nuget.org/downloads. Ensuite, à partir de la ligne de commande de Nuget, exécutez la commande suivante:

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>Obtenir le package à l’aide de Visual Studio

Dans Visual Studio, cliquez sur le nœud de votre solution ou un projet et choisissez une des commandes gérer les Packages Nuget.  Rechercher les **Microsoft.PackageSupportFramework** ou les **produits** rechercher le package sur Nuget.org. Ensuite, l’installer.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Ajouter les fichiers de Package prise en charge Framework à votre package

Ajoutez la DLL de produits requises 32 bits et 64 bits et les fichiers exécutables dans le répertoire du package. Inspirez-vous du tableau suivant. Vous devrez également inclure tous les correctifs runtime dont vous avez besoin. Dans notre exemple, nous devons le correctif de runtime de la redirection de fichier.

| Exécutable de l’application est x64 | Exécutable de l’application est x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

Le contenu de votre package doit maintenant ressembler à ceci.

![Fichiers binaires de package](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modifier le manifeste du package

Ouvrez votre manifeste du package dans un éditeur de texte et définissez le `Executable` attribut de le `Application` élément au nom du fichier exécutable Lanceur produits.  Si vous savez que l’architecture de votre application cible, sélectionnez la version appropriée, PSFLauncher32.exe ou PSFLauncher64.exe.  Si ce n’est pas le cas, PSFLauncher32.exe fonctionnera dans tous les cas.  Voici un exemple:

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Créer un fichier de configuration

Créer un nom de fichier ``config.json``et enregistrez-le dans le dossier racine de votre package. Modifier l’ID d’application déclaré du fichier config.json pour pointer vers le fichier exécutable qui vient d’être remplacé. À l’aide de la connaissance que vous avez obtenus à partir de l’utilisation du moniteur de processus, vous pouvez également définir le répertoire de travail mais aussi utiliser la correction de la redirection de fichier pour rediriger les opérations de lecture/écriture aux fichiers .log sous le répertoire «PSFSampleApp» relatif au package.

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
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
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
| applications | exécutable | Le chemin d’accès relatif au package de l’exécutable que vous voulez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste du package avant de le modifier. Il s’agit de la valeur de la `Executable` attribut de le `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès relatif au package à utiliser comme répertoire de travail de l’application qui démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire en tant que répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et le chemin supprimée. |
| corrections | DLL | Chemin d’accès relatif au package à la correction,.msix/.appx à charger. |
| corrections | config | (Facultatif) Contrôle le comporte de la liste de distribution de correction. Le format exact de cette valeur varie sur une base de correction-par-correction comme chaque correction puisse les interpréter ce «blob» qu’il le souhaite. |

Le `applications`, `processes`, et `fixups` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plusieurs applications, processus et correction DLL.

### <a name="package-and-test-the-app"></a>Package et Test de l’application

Ensuite, créez un package.

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Ensuite, signez-le.

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Pour plus d’informations, voir [comment créer un certificat de signature de package](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) et [comment signer un package à l’aide de signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

À l’aide de PowerShell, installez le package.

>[!NOTE]
> N’oubliez pas de désinstallation de tout d’abord le package.

```ps
powershell Add-MSIXPackage .\PSFSamplePackageFixup.msix
```

Exécutez l’application et observez le comportement avec correctif runtime appliquée.  Répétez le diagnostic et les étapes de création de packages en fonction des besoins.

### <a name="use-the-trace-fixup"></a>Utiliser la correction de Trace

Une autre technique à diagnostiquer les problèmes de compatibilité d’application empaquetée consiste à utiliser la correction de Trace. Cette DLL est fournie avec celles et fournit une vue détaillée de diagnostic du comportement de l’application, similaire à un moniteur de traitement.  Il est spécialement conçu pour révéler des problèmes de compatibilité d’application.  Pour utiliser la correction de Trace, ajouter la DLL au package, ajouter le fragment suivant à votre config.json et ensuite créer un package et installer votre application.

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

Par défaut, la correction de Trace filtre les défaillances qui peuvent être considérés comme «prévus».  Par exemple, les applications peuvent essayer de manière inconditionnelle supprimer un fichier sans vérifier s’il existe déjà, en ignorant le résultat. Cela a pour les conséquences malheureusement que certains échecs inattendus peuvent obtenir filtrés, afin que dans l’exemple ci-dessus, nous opter pour recevoir tous les échecs de fonctions de système de fichiers. Nous procédons ainsi car nous savons à partir d’avant que la tentative de lecture à partir du fichier Config.txt échoue avec le message «fichier introuvable». Il s’agit d’une défaillance qui est souvent observée et pas censée pour être inattendue. Dans la pratique, il est probablement mieux pour commencer à déconnecter, puis revenir à tous les échecs s’il existe un problème qui ne peut pas toujours être identifié et filtrage uniquement à des échecs inattendus.

Par défaut, la sortie de la correction de Trace est envoyée au débogueur attaché. Pour cet exemple, nous ne sont pas accédant à joindre un débogueur et utilisera à la place le programme [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) de SysInternals pour consulter sa sortie. Après l’exécution de l’application, nous pouvons voir les échecs de mêmes en comme précédemment, ce qui nous pointez vers les même correctifs à l’exécution.

![TraceShim fichier introuvable](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim accès refusé](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Déboguer, étendre ou créer un correctif runtime

Vous pouvez utiliser Visual Studio pour déboguer un correctif runtime, étendre un correctif runtime ou en créer un à partir de zéro. Vous devez effectuer les opérations suivantes réussisse.

> [!div class="checklist"]
> * Ajoutez un projet de création de packages
> * Ajouter le projet pour la résolution de l’exécution
> * Ajoutez un projet qui démarre le Lanceur de produits exécutable
> * Configurer le projet de création de packages

Lorsque vous avez terminé, votre solution doit ressembler à ceci.

![Solution complétée](images/desktop-to-uwp/runtime-fix-project-structure.png)

Examinons chaque projet dans cet exemple.

| Projet | Objectif |
|-------|-----------|
| DesktopApplicationPackage | Ce projet est basé sur le [projet de package de l’Application Windows](desktop-to-uwp-packaging-dot-net.md) et qu’il génère le package MSIX. |
| Runtimefix | Il s’agit d’un projet de bibliothèque de Dynamic-Linked C++ qui contient une ou plusieurs fonctions de remplacement qui servent de la résolution de l’exécution. |
| PSFLauncher | Il s’agit de projet C++ vide. Ce projet est un endroit pour collecter les fichiers distribuable runtime de l’infrastructure de prise en charge du Package. Elle génère un fichier exécutable. Cet exécutable est la première chose qui s’exécute lorsque vous démarrez la solution. |
| WinFormsDesktopApplication | Ce projet contient le code source d’une application de bureau. |

Pour consulter un exemple complet qui contient tous ces types de projets, voir [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Passons en revue les étapes nécessaires pour créer et configurer chacune de ces projets dans votre solution.

### <a name="create-a-package-solution"></a>Créer une solution de package

Si vous n’avez pas encore une solution pour votre application de bureau, créez une nouvelle **Solution vide** dans Visual Studio.

![Solution vide](images/desktop-to-uwp/blank-solution.png)

Vous pouvez également ajouter des projets d’application que vous avez.

### <a name="add-a-packaging-project"></a>Ajoutez un projet de création de packages

Si vous n’avez pas encore un **Projet de création de packages d’Application Windows**, en créer un et l’ajouter à votre solution.

![Modèle de projet de package](images/desktop-to-uwp/package-project-template.png)

Pour plus d’informations sur le projet de package de l’Application Windows, voir le [Package de votre application à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md).

Dans l' **Explorateur de solutions**, cliquer sur le projet de création de packages, sélectionnez **Modifier**et ajoutez ce vers le bas du fichier de projet:

```xml
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

### <a name="add-project-for-the-runtime-fix"></a>Ajouter le projet pour la résolution de l’exécution

Ajoutez un projet C++ **Bibliothèque de liens dynamiques (DLL)** à la solution.

![Bibliothèque de correctif Runtime](images/desktop-to-uwp/runtime-fix-library.png)

Avec le bouton droit le que de projet, puis choisissez **Propriétés**.

Dans les pages de propriétés, recherchez le champ **Standard de langage C++** et dans la liste déroulante en regard de ce champ, sélectionnez le **ISO C ++ 17 Standard (/ std: c ++ 17)** option.

![ISO 17 Option](images/desktop-to-uwp/iso-option.png)

Avec le bouton droit de ce projet et puis, dans le menu contextuel, choisissez l’option de **Gérer les Packages Nuget** . Vérifiez que l’option de **source du Package** est définie à **l’ensemble** ou **nuget.org**.

Cliquez sur l’icône Paramètres ensuite ce champ.

Recherche de *produits** Nuget empaqueter et puis installez-le pour ce projet.

![package NuGet](images/desktop-to-uwp/psf-package.png)

Si vous souhaitez déboguer ou étendre un correctif runtime existant, ajoutez les fichiers de correctif runtime que vous avez obtenu en utilisant les instructions décrites dans la section [Rechercher un correctif de runtime](#find) de ce guide.

Si vous avez l’intention de créer un nouveau correctif, n’ajoutez pas quoi que ce soit à ce projet tout de suite. Nous vous aiderons à ajouter les fichiers appropriés à ce projet plus loin dans ce guide. Pour le moment, nous allons continuer la configuration de votre solution.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Ajoutez un projet qui démarre le Lanceur de produits exécutable

Ajoutez un projet C++ **Projet vide** à la solution.

![Projet vide](images/desktop-to-uwp/blank-app.png)

Ajouter le package Nuget de **produits** à ce projet en utilisant les mêmes consignes décrites dans la section précédente.

Ouvrez les pages de propriétés du projet et dans la page Paramètres **généraux** , définissez la propriété **Nom de la cible** sur ``PSFLauncher32`` ou ``PSFLauncher64`` en fonction de l’architecture de votre application.

![Référence de Lanceur de produits](images/desktop-to-uwp/shim-exe-reference.png)

Ajouter une référence de projet au projet de correctif runtime dans votre solution.

![référence de correctif Runtime](images/desktop-to-uwp/reference-fix.png)

Avec le bouton droit de la référence, puis, dans la fenêtre **Propriétés** , appliquez ces valeurs.

| Propriété | Valeur |
|-------|-----------|
| Copiez local | Vrai |
| Copier des assemblys satellites locaux | Vrai |
| Sortie d’Assembly de référence | Vrai |
| Dépendances de bibliothèque de liaison | Faux |
| Entrées de dépendance de bibliothèque de liaison | Faux |

### <a name="configure-the-packaging-project"></a>Configurer le projet de création de packages

Dans le projet de mise en package, cliquez avec le bouton droit sur le dossier **Applications**, puis sélectionnez sur **Ajouter une référence**.

![Ajouter une référence de projet](images/desktop-to-uwp/add-reference-packaging-project.png)

Choisissez le projet de Lanceur de produits et de votre projet d’application de bureau et cliquez sur le bouton **OK** .

![Projet de bureau](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si vous n’avez pas le code source à votre application, il suffit de choisir le projet de Lanceur de produits. Nous allons vous montrer comment faire référence à votre fichier exécutable lorsque vous créez un fichier de configuration.

Dans le nœud **Applications** , avec le bouton droit de l’application de lancement des produits, puis choisissez **définir comme Point d’entrée**.

![Définir le point d’entrée](images/desktop-to-uwp/set-startup-project.png)

Ajoutez un fichier nommé ``config.json`` à votre projet de création de package, puis, copiez et collez le texte json suivant dans le fichier. Définissez la propriété de **l’Action de Package** au **contenu**.

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
            "fixups": [
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
| applications | exécutable | Le chemin d’accès relatif au package de l’exécutable que vous voulez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste du package avant de le modifier. Il s’agit de la valeur de la `Executable` attribut de le `Application` élément. |
| applications | workingDirectory | (Facultatif) Un chemin d’accès relatif au package à utiliser comme répertoire de travail de l’application qui démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire en tant que répertoire de travail de l’application. |
| processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de fichier et le chemin supprimée. |
| corrections | DLL | Chemin d’accès relatif au package à la correction DLL à charger. |
| corrections | config | (Facultatif) Contrôle le comporte de la DLL de correction. Le format exact de cette valeur varie sur une base de correction-par-correction comme chaque correction puisse les interpréter ce «blob» qu’il le souhaite. |

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
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> Le `applications`, `processes`, et `fixups` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json pour spécifier plusieurs applications, processus et correction DLL.

### <a name="debug-a-runtime-fix"></a>Déboguer un correctif runtime

Dans Visual Studio, appuyez sur F5 pour démarrer le débogueur.  La première chose qui démarre est l’application de lancement de produits, qui à son tour, lance votre application de bureau cible.  Pour déboguer l’application de bureau cible, vous devrez manuellement attacher au processus d’application de bureau en choisissant **Déboguer**->**Attacher au processus**, puis en sélectionnant le processus d’application. Pour permettre le débogage d’une application .NET avec un correctif de runtime natif DLL, sélectionnez les types de code managé et natif (débogage en mode mixte).  

Une fois que vous avez configuré cela, vous pouvez définir des points d’arrêt en regard des lignes de code dans le code de l’application de bureau et le projet de correction du runtime. Si vous n’avez pas le code source à votre application, vous serez en mesure de définir des points d’arrêt uniquement en regard des lignes de code dans votre projet de correction du runtime.

>[!NOTE]
> Pendant que Visual Studio vous donne le développement plus simple et l’expérience de débogage, il existe certaines limitations, par conséquent, plus loin dans ce guide, nous aborderons autres techniques de débogage que vous pouvez appliquer.

## <a name="create-a-runtime-fix"></a>Créer un correctif runtime

Si il n’existe pas encore d’un runtime résoudre le problème que vous souhaitez résoudre, vous pouvez créer un nouveau correctif runtime en écrivant des fonctions de remplacement, y compris les données de configuration qui convient. Examinons chaque partie.

### <a name="replacement-functions"></a>Fonctions de remplacement

Tout d’abord, identifiez quelle fonction appels échouent lorsque votre application s’exécute dans un conteneur MSIX. Ensuite, vous pouvez créer des fonctions de remplacement que vous souhaitez que le Gestionnaire d’exécution pour appeler à la place. Cela vous donne la possibilité de remplacer l’implémentation d’une fonction avec un comportement qui doit être conforme aux règles de l’environnement d’exécution moderne.

Dans Visual Studio, ouvrez le projet de correctif runtime que vous avez créé précédemment dans ce guide.

Déclarez la ``FIXUP_DEFINE_EXPORTS`` macro, puis ajoutez une instruction include pour le `fixup_framework.h` en haut de chaque. Fichiers CPP dans lequel vous envisagez d’ajouter les fonctions de votre correctif runtime.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Assurez-vous que le `FIXUP_DEFINE_EXPORTS` macro s’affiche avant l’instruction d’inclusion.

Créer une fonction qui a la même signature de la fonction qui a comportement que vous souhaitez modifier. Voici un exemple de fonction qui remplace le `MessageBoxW` fonction.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

L’appel à `DECLARE_FIXUP` mappe le `MessageBoxW` pour votre nouvelle fonction de remplacement. Lorsque votre application tente d’appeler le `MessageBoxW` fonction, il appelle la fonction de remplacement à la place.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protection contre les appels récursive aux fonctions de correctifs à l’exécution

Vous pouvez éventuellement appliquer le `reentrancy_guard` type à vos fonctions qui protègent contre les appels récursive aux fonctions de correctifs à l’exécution.

Par exemple, cela peut donner une fonction de remplacement pour le `CreateFile` fonction. Votre implémentation peut appeler le `CopyFile` fonction, mais l’implémentation de la `CopyFile` fonction peut appeler le `CreateFile` fonction. Cela peut entraîner un cycle récurrent infini d’appels à le `CreateFile` fonction.

Pour plus d’informations sur `reentrancy_guard` voir [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Données de configuration

Si vous souhaitez ajouter des données de configuration à votre correctif runtime, envisagez d’ajouter l’application à la ``config.json``. De cette façon, vous pouvez utiliser la `FixupQueryCurrentDllConfig` facilement analyser ces données. Cet exemple analyse une valeur booléenne et chaîne à partir de ce fichier de configuration.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
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

Tout d’abord, débogage F5 s’exécute l’application par le déploiement de fichiers libres dans le chemin d’accès de dossier de la mise en package, au lieu de l’installation à partir d’un .msix / package .appx.  Le dossier de disposition en règle générale, n’a pas les mêmes restrictions de sécurité qu’un dossier de package installé. Par conséquent, il ne peut pas être possible de reproduire les erreurs de refus d’accès package chemin d’accès avant d’appliquer un correctif de runtime.

Pour résoudre ce problème, utilisez .msix / déploiement du package .appx plutôt que F5 perdre le déploiement de fichiers.  Pour créer un .msix / créer un package .appx fichier, utilisez l’utilitaire [MakeMSIX](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) dans le Kit de développement Windows, comme décrit ci-dessus. Ou, à partir d’au sein de Visual Studio, cliquez sur le nœud de votre projet d’application et sélectionnez **Store**->**Créer des Packages d’application**.

Un autre problème avec Visual Studio est qu’il n’a pas de prise en charge intégrée pour l’attachement à n’importe quel processus enfants lancées par le débogueur.   Cela rend difficiles à déboguer logique dans le chemin de démarrage de l’application cible, qui doit être liée manuellement par Visual Studio après le lancement.

Pour résoudre ce problème, utilisez un débogueur qui prend en charge de processus enfant joindre.  Notez qu’il n’est généralement pas possible d’attacher un débogueur (JIT) de juste-à-temps à l’application cible.  Il s’agit dans la mesure où la plupart des techniques JIT impliquent lancer le débogueur à la place de l’application cible, via la clé de Registre ImageFileExecutionOptions.  Cela va à l’encontre detouring mécanisme utilisé par PSFLauncher.exe pour injecter FixupRuntime.dll dans l’application cible.  WinDbg, inclus dans les [Outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)et obtenues à partir du [Kit de développement Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), attacher de processus enfant de prend en charge.  Il prend également en charge directement [lancement et de débogage d’une application UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Pour déboguer le démarrage de l’application cible en tant qu’un processus enfant, démarrer ``WinDbg``.

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

À la ``WinDbg`` invite, activer le débogage des enfants et définir des points d’arrêt appropriés.

```ps
.childdbg 1
g
```

(exécuter en tant que l’application cible démarre et s’arrête dans le débogueur)

```ps
sxe ld fixup.dll
g
```

(s’exécute jusqu'à ce que la correction que DLL est chargée)

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) peut également être utilisé pour joindre un débogueur à une application lors du lancement et est également inclus dans les [Outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Toutefois, il est plus complexe à utiliser que la prise en charge directe désormais fourni par WinDbg.

## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
