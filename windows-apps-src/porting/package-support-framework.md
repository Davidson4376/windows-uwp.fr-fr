---
Description: Résoudre les problèmes qui empêchent votre application de bureau à partir de l’exécution dans un conteneur MSIX
Search.Product: eADQiWindows 10XVcnh
title: Résoudre les problèmes qui empêchent votre application de bureau à partir de l’exécution dans un conteneur MSIX
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80f9c8bad9445bd9cfef9b09c00f99929fda37aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590724"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Appliquer des correctifs de runtime à un package MSIX à l’aide de l’infrastructure de prise en charge de Package

L’infrastructure de prise en charge de Package est un kit open source qui vous permet d’appliquer des correctifs à votre application win32 existante lorsque vous n’avez pas accès au code source, afin qu’il peut s’exécuter dans un conteneur MSIX. L’infrastructure de prise en charge de Package permet à votre application de suivre les meilleures pratiques de l’environnement d’exécution modernes.

Pour plus d’informations, consultez [infrastructure de prise en charge de Package](https://docs.microsoft.com/windows/msix/package-support-framework-overview).

Ce guide vous aidera à identifier les problèmes de compatibilité, et pour rechercher, d’appliquer et d’étendre le runtime résout où elles sont traitées.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identifier les problèmes de compatibilité d’application empaquetée

Tout d’abord, créez un package pour votre application. Ensuite, installez-le, exécutez-le, puis observer son comportement. Vous pouvez recevoir des messages d’erreur qui peuvent vous aider à identifier un problème de compatibilité. Vous pouvez également utiliser [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) pour identifier les problèmes.  Problèmes courants se rapportent à des hypothèses application concernant les autorisations de chemin de répertoire et le programme de travail.

### <a name="using-process-monitor-to-identify-an-issue"></a>À l’aide de Process Monitor pour identifier un problème

[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) est un utilitaire puissant pour l’observation fichier d’une application et les opérations de Registre et leurs résultats.  Cela peut vous aider à comprendre les problèmes de compatibilité.  Après l’ouverture de Process Monitor, ajouter un filtre (filtre > filtre...) pour inclure uniquement les événements à partir de l’exécutable de l’application.

![Filtre d’application ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Une liste d’événements s’affiche. Pour la plupart de ces événements, le mot **réussite** apparaîtra dans le **résultat** colonne.

![Événements de ProcMon](images/desktop-to-uwp/procmon_events.png)

Si vous le souhaitez, vous pouvez filtrer les événements pour afficher uniquement les échecs uniquement.

![Réussite de l’exclusion de ProcMon](images/desktop-to-uwp/procmon_exclude_success.png)

Si vous suspectez une défaillance d’accès de système de fichiers, recherchez les événements ayant échoué qui se trouvent sous le System32/SysWOW64 ou le chemin d’accès du fichier de package. Filtres peuvent également vous aider ici, aussi. Commencer au bas de cette liste et faites défiler vers le haut. Les défaillances qui s’affichent au bas de cette liste se sont produites récemment. Faites attention la plupart des erreurs qui contiennent des chaînes telles que « accès refusé » et « chemin d’accès/nom introuvable » et ignorer les choses semblent suspectes. Le [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) présente deux problèmes. Vous pouvez voir ces problèmes dans la liste qui s’affiche dans l’image suivante.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

Dans le premier problème qui apparaît dans cette image, l’application ne peut pas lire le fichier « Config.txt » qui se trouve dans le chemin d’accès « C:\Windows\SysWOW64 ». Il est peu probable que l’application essaie de référencer directement de ce chemin d’accès. Très probablement, il tente de lire à partir de ce fichier à l’aide d’un chemin d’accès relatif, et par défaut, « System32/SysWOW64 » est le répertoire de travail de l’application. Cela suggère que l’application attend son répertoire de travail actuel pour définir quelque part dans le package. Recherchez à l’intérieur de l’application, nous pouvons voir que le fichier existe dans le même répertoire que l’exécutable.

![Application Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

Le deuxième problème apparaît dans l’image suivante.

![Fichier journal ProcMon](images/desktop-to-uwp/procmon_logfile.png)

Dans ce problème, l’application ne peut pas écrire un fichier .log dans son chemin d’accès du package. Cela suggère qu’une correction de la redirection du fichier peut-être vous aider.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Rechercher un correctif de runtime

CELLES contient des correctifs de runtime que vous pouvez utiliser maintenant, telles que la correction de la redirection du fichier.

### <a name="file-redirection-fixup"></a>Correction de la Redirection de fichier

Vous pouvez utiliser la [correction de la Redirection de fichier](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) pour rediriger les tentatives d’écrire ou lire les données dans un répertoire qui n’est pas accessible à partir d’une application qui s’exécute dans un conteneur MSIX.

Par exemple, si votre application écrit dans un fichier journal qui se trouve dans le même répertoire que vos applications exécutables, vous pouvez utiliser la [correction de la Redirection de fichier](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) pour créer ce fichier journal dans un autre emplacement, telles que le magasin de données d’application locale.

### <a name="runtime-fixes-from-the-community"></a>Correctifs de runtime à partir de la Communauté

Passez en revue les contributions de la Communauté à notre [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) page. Il est possible que les autres développeurs ont résolu un problème similaire au vôtre et ont partagé un correctif de runtime.

## <a name="apply-a-runtime-fix"></a>Appliquer un correctif de runtime

Vous pouvez appliquer un correctif de runtime existant avec quelques outils simples à partir du SDK Windows et en suivant ces étapes.

> [!div class="checklist"]
> * Créer un dossier de disposition de package
> * Obtenir les fichiers de l’infrastructure de prise en charge de Package
> * Ajoutez-les à votre package
> * Modifier le manifeste du package
> * Créer un fichier de configuration

Passons en revue chaque tâche.

### <a name="create-the-package-layout-folder"></a>Créer le dossier de disposition de package

Si vous avez déjà un fichier .msix (ou .appx), vous pouvez décompresser son contenu dans un dossier de disposition qui servira à la zone de transit pour votre package. Vous pouvez le faire à partir d’une invite de commandes à l’aide de l’outil de MakeAppx, selon votre chemin d’installation du Kit de développement, voici où vous trouverez l’outil makeappx.exe sur votre PC Windows 10 : x86 : C:\Program fichiers (x86) \Windows Kits\10\bin\x86\makeappx.exe x64 : C:\Program fichiers (x86) \Windows Kits\10\bin\x64\makeappx.exe

```ps
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Cela vous donnera quelque chose qui ressemble à ce qui suit.

![Disposition du package](images/desktop-to-uwp/package_contents.png)

Si vous n’avez pas un fichier .msix (ou .appx) pour commencer, vous pouvez créer les fichiers et dossiers de package à partir de zéro.

### <a name="get-the-package-support-framework-files"></a>Obtenir les fichiers de l’infrastructure de prise en charge de Package

Vous pouvez obtenir le package Nuget de produits à l’aide de l’outil de ligne de commande Nuget autonome ou par le biais de Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtenir le package à l’aide de l’outil de ligne de commande

Installer l’outil de ligne de commande Nuget à partir de cet emplacement : https://www.nuget.org/downloads. Ensuite, à partir de la ligne de commande Nuget, exécutez la commande suivante :

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>Obtenir le package à l’aide de Visual Studio

Dans Visual Studio, cliquez sur le nœud de votre solution ou le projet et choisissez une des commandes de gérer les Packages Nuget.  Recherchez **Microsoft.PackageSupportFramework** ou **produits** pour rechercher le package sur Nuget.org. Ensuite, installez-le.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Ajouter les fichiers de l’infrastructure de prise en charge de Package à votre package

Ajouter la DLL de produits 32 bits et 64 bits requises et les fichiers exécutables dans le répertoire de package. Inspirez-vous du tableau suivant. Vous allez également que vous souhaitez inclure tous les correctifs de runtime dont vous avez besoin. Dans notre exemple, nous avons besoin de la correction de runtime de la redirection de fichier.

| Exécutable de l’application est x64 | Exécutable de l’application est x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

Votre contenu du package doit maintenant ressembler à ceci.

![Fichiers binaires des packages](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modifier le manifeste du package

Ouvrez votre manifeste de package dans un éditeur de texte et définissez le `Executable` attribut de la `Application` élément pour le nom du fichier exécutable de lancement de produits.  Si vous connaissez l’architecture de votre application cible, sélectionnez la version appropriée, PSFLauncher32.exe ou PSFLauncher64.exe.  Si ce n’est pas le cas, PSFLauncher32.exe fonctionnera dans tous les cas.  Voici un exemple :

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

Créez un fichier nommé ``config.json``et enregistrez ce fichier dans le dossier racine de votre package. Modifier l’ID d’application déclaré du fichier config.json pour pointer vers le fichier exécutable qui vient d’être remplacé. À l’aide de la base de connaissances que vous avez obtenus à partir de l’aide du moniteur de processus, vous pouvez également définir le répertoire de travail comme ainsi que la correction de la redirection du fichier permet de rediriger les lectures/écritures vers les fichiers journaux sous le répertoire de « PSFSampleApp » relatifs à un package.

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

Voici un guide pour le schéma config.json :

| tableau | key | Valeur |
|-------|-----------|-------|
| applications | id |  Utilisez la valeur de la `Id` attribut de la `Application` élément dans le manifeste du package. |
| applications | exécutable | Le chemin d’accès relatif du package à l’exécutable que vous souhaitez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste du package avant de le modifier. C’est la valeur de la `Executable` attribut de la `Application` élément. |
| applications | WorkingDirectory | (Facultatif) Un chemin d’accès relatif du package à utiliser comme répertoire de travail de l’application qui démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| Processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de chemin d’accès et fichier supprimée. |
| corrections | DLL | Relatifs à un package de chemin d’accès à la correction,.msix/.appx à charger. |
| corrections | config | (Facultatif) Contrôle le comportement de la liste de distribution de correction. Le format exact de cette valeur varie sur une base de correction par correction comme chaque correction peut interpréter ce « blob » qu’il le souhaite. |

Le `applications`, `processes`, et `fixups` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json spécifier plus d’une application, processus et correction DLL.

### <a name="package-and-test-the-app"></a>Package et tester l’application

Ensuite, créez un package.

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Ensuite, signez-le.

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Pour plus d’informations, consultez [comment créer un certificat de signature du package](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) et [comment signer un package à l’aide de signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

À l’aide de PowerShell, installez le package.

>[!NOTE]
> N’oubliez pas d’abord désinstaller le package.

```ps
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

Exécutez l’application et observer le comportement avec le correctif du runtime appliqué.  Répétez les étapes de packaging en fonction des besoins et de diagnostic.

### <a name="use-the-trace-fixup"></a>Utiliser la correction de la Trace

Une autre technique à diagnostiquer les problèmes de compatibilité d’application empaquetée consiste à utiliser la correction de la Trace. Cette DLL est incluse avec celles et fournit une vue de diagnostique détaillée du comportement de l’application, similaire au moniteur de processus.  Il est spécialement conçu pour faire apparaître les problèmes de compatibilité.  Pour utiliser la correction de la Trace, ajouter la DLL au package, ajoutez le fragment suivant à votre config.json et puis empaqueter et installer votre application.

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

Par défaut, la correction de la Trace filtre les défaillances qui peuvent être considérées comme « attendus ».  Par exemple, les applications peuvent essayer inconditionnellement supprimer un fichier sans vérifier s’il existe déjà, en ignorant le résultat. Cela a la conséquence malheureuse certains échecs inattendus peuvent obtenir filtrés, donc dans l’exemple ci-dessus, nous allons opter pour recevoir tous les échecs de fonctions de système de fichiers. Nous faisons cela parce que nous les connaissons avant que la tentative de lecture à partir du fichier Config.txt échoue avec le message « fichier introuvable ». Il s’agit d’une défaillance qui est souvent observée et pas censée pour être inattendu. Dans la pratique, il est probablement mieux commencer le filtrage uniquement aux défaillances inattendues et puis revenir à tous les échecs s’il existe un problème qui ne peut pas toujours être identifié.

Par défaut, la sortie de la correction de la Trace est envoyée au débogueur joint. Pour cet exemple, nous ne sont pas attache un débogueur et utilisera à la place la [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) programme de SysInternals pour afficher sa sortie. Après avoir exécuté l’application, nous constatons mêmes défaillances comme auparavant, ce qui nous pointerait vers les mêmes correctifs de runtime.

![TraceShim fichier introuvable](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim accès refusé](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Déboguer, étendre ou créer un correctif de runtime

Vous pouvez utiliser Visual Studio pour déboguer un correctif de runtime, étendre un correctif de runtime ou créez-en un à partir de zéro. Vous devez effectuer les opérations suivantes pour réussir.

> [!div class="checklist"]
> * Ajouter un projet d’empaquetage
> * Ajouter un projet pour le correctif du runtime
> * Ajouter un projet qui démarre le service de lancement de produits exécutable
> * Configurer le projet d’empaquetage

Lorsque vous avez terminé, votre solution se présente comme suit.

![Solution terminée](images/desktop-to-uwp/runtime-fix-project-structure.png)

Examinons chaque projet dans cet exemple.

| Projet | Objectif |
|-------|-----------|
| DesktopApplicationPackage | Ce projet est basé sur le [projet d’empaquetage d’Application Windows](desktop-to-uwp-packaging-dot-net.md) et il génère le package MSIX. |
| Runtimefix | Il s’agit d’un projet C++ Dynamic-Linked bibliothèque qui contient une ou plusieurs fonctions de remplacement qui servent à la correction du runtime. |
| PSFLauncher | Il s’agit de projet C++ vide. Ce projet est un endroit pour collecter les fichiers distribuable de runtime de l’infrastructure de prise en charge de Package. Il génère un fichier exécutable. Ce fichier exécutable est la première chose qui s’exécute lorsque vous démarrez la solution. |
| WinFormsDesktopApplication | Ce projet contient le code source d’une application de bureau. |

Pour consulter un exemple complet qui contient tous ces types de projets, consultez [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Nous allons étudier les étapes pour créer et configurer chacun de ces projets dans votre solution.

### <a name="create-a-package-solution"></a>Créer une solution de package

Si vous ne disposez pas d’une solution pour votre application de bureau, créez un **nouvelle Solution** dans Visual Studio.

![Solution vide](images/desktop-to-uwp/blank-solution.png)

Vous pouvez également ajouter des projets d’application que vous avez.

### <a name="add-a-packaging-project"></a>Ajouter un projet d’empaquetage

Si vous n’avez pas déjà un **projet d’empaquetage Windows Application**, créez-en un et l’ajouter à votre solution.

![Modèle de projet de package](images/desktop-to-uwp/package-project-template.png)

Pour plus d’informations sur le projet d’empaquetage d’Application Windows, consultez [empaqueter votre application à l’aide de Visual Studio](desktop-to-uwp-packaging-dot-net.md).

Dans **l’Explorateur de solutions**, cliquez sur le projet d’empaquetage, sélectionnez **modifier**, puis ajoutez ceci au bas du fichier projet :

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

### <a name="add-project-for-the-runtime-fix"></a>Ajouter un projet pour le correctif du runtime

Ajouter un C++ **bibliothèque de liens dynamiques (DLL)** projet à la solution.

![Correctif de la bibliothèque Runtime](images/desktop-to-uwp/runtime-fix-library.png)

Clic droit qui de projet, puis choisissez **propriétés**.

Dans les pages de propriétés, recherchez la **norme du langage C++** champ, puis, dans la liste déroulante en regard de ce champ, sélectionnez le **norme ISO C ++ 17 (/ std : c ++ 17)** option.

![ISO 17 Option](images/desktop-to-uwp/iso-option.png)

Cliquez sur ce projet, puis, dans le menu contextuel, choisissez le **gérer les Packages Nuget** option. Vérifiez que le **source du Package** option est définie sur **tous les** ou **nuget.org**.

Cliquez sur l’icône des paramètres suivant ce champ.

Recherchez le *produits** Nuget package et l’installer pour ce projet.

![package NuGet](images/desktop-to-uwp/psf-package.png)

Si vous souhaitez déboguer ou étendre un correctif de runtime existant, ajoutez les fichiers du correctif runtime que vous avez obtenue en utilisant les instructions décrites dans le [trouver un correctif runtime](#find) section de ce guide.

Si vous envisagez de créer un tout nouveau correctif, n’ajoutent rien à ce projet encore. Nous vous aiderons à ajouter les fichiers appropriés à ce projet plus loin dans ce guide. Pour l’instant, nous allons continuer la configuration de votre solution.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Ajouter un projet qui démarre le service de lancement de produits exécutable

Ajouter un C++ **projet vide** projet à la solution.

![Projet vide](images/desktop-to-uwp/blank-app.png)

Ajouter le **produits** package Nuget à ce projet en utilisant les mêmes recommandations décrites dans la section précédente.

Ouvrez les pages de propriétés pour le projet et dans le **général** page Paramètres, définissez la **nom cible** propriété ``PSFLauncher32`` ou ``PSFLauncher64`` selon l’architecture de votre application.

![Référence de Lanceur de produits](images/desktop-to-uwp/shim-exe-reference.png)

Ajouter une référence de projet pour le projet de correctif d’exécution dans votre solution.

![référence du correctif Runtime](images/desktop-to-uwp/reference-fix.png)

Avec le bouton droit de la référence, puis, dans le **propriétés** fenêtre, appliquer ces valeurs.

| Propriété | Valeur |
|-------|-----------|
| Copier en local | True |
| Copiez les assemblys satellites locaux | True |
| Sortie d’Assembly de référence | True |
| Dépendances de bibliothèque de liens | False |
| Entrées de dépendance de bibliothèque de lien | False |

### <a name="configure-the-packaging-project"></a>Configurer le projet d’empaquetage

Dans le projet de mise en package, cliquez avec le bouton droit sur le dossier **Applications**, puis sélectionnez sur **Ajouter une référence**.

![Ajouter une référence de projet](images/desktop-to-uwp/add-reference-packaging-project.png)

Choisissez le projet de lancement de produits et de votre projet d’application de bureau, puis le **OK** bouton.

![Projet de bureau](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si vous n’avez pas le code source à votre application, choisissez simplement le projet de lancement de produits. Nous allons vous montrer comment référencer votre fichier exécutable lorsque vous créez un fichier de configuration.

Dans le **Applications** nœud, avec le bouton droit de l’application de lancement de produits, puis choisissez **définir en tant que Point d’entrée**.

![Définir le point d’entrée](images/desktop-to-uwp/set-startup-project.png)

Ajoutez un fichier nommé ``config.json`` à votre projet d’empaquetage, puis, copiez et collez le texte json suivant dans le fichier. Définir le **Action de Package** propriété **contenu**.

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

Fournissez une valeur pour chaque clé. Utilisez ce tableau comme guide.

| tableau | key | Valeur |
|-------|-----------|-------|
| applications | id |  Utilisez la valeur de la `Id` attribut de la `Application` élément dans le manifeste du package. |
| applications | exécutable | Le chemin d’accès relatif du package à l’exécutable que vous souhaitez démarrer. Dans la plupart des cas, vous pouvez obtenir cette valeur à partir de votre fichier manifeste du package avant de le modifier. C’est la valeur de la `Executable` attribut de la `Application` élément. |
| applications | WorkingDirectory | (Facultatif) Un chemin d’accès relatif du package à utiliser comme répertoire de travail de l’application qui démarre. Si vous ne définissez pas cette valeur, le système d’exploitation utilise le `System32` répertoire comme répertoire de travail de l’application. |
| Processus | exécutable | Dans la plupart des cas, il s’agit du nom de la `executable` configuré ci-dessus avec l’extension de chemin d’accès et fichier supprimée. |
| corrections | DLL | Chemin d’accès de package relatif aux corrections de la DLL à charger. |
| corrections | config | (Facultatif) Contrôle le comportement de la DLL de correction. Le format exact de cette valeur varie sur une base de correction par correction comme chaque correction peut interpréter ce « blob » qu’il le souhaite. |

Lorsque vous avez terminé, votre ``config.json`` fichier ressemblera à ceci.

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
> Le `applications`, `processes`, et `fixups` clés sont des tableaux. Cela signifie que vous pouvez utiliser le fichier config.json spécifier plus d’une application, processus et correction DLL.

### <a name="debug-a-runtime-fix"></a>Déboguer un correctif de runtime

Dans Visual Studio, appuyez sur F5 pour démarrer le débogueur.  La première chose qui démarre est l’application de lancement de produits, qui démarre à son tour, votre application de bureau cible.  Pour déboguer l’application de bureau cible, vous devez attacher manuellement le processus d’application de bureau en choisissant **déboguer**->**attacher au processus**, puis en sélectionnant l’application processus. Pour autoriser le débogage d’une application .NET avec un correctif de runtime natif DLL, sélectionnez les types de code managé et natif (débogage en mode mixte).  

Une fois que vous avez configuré cela, vous pouvez définir des points d’arrêt en regard des lignes de code dans le code d’application de bureau et le projet de correctif du runtime. Si vous n’avez pas le code source à votre application, vous serez en mesure de définir des points d’arrêt en regard des lignes de code dans votre projet de correctif du runtime.

>[!NOTE]
> Bien que Visual Studio vous donne le développement la plus simple et l’expérience de débogage, il existe certaines limitations, par conséquent, plus loin dans ce guide, nous aborderons autres techniques de débogage que vous pouvez appliquer.

## <a name="create-a-runtime-fix"></a>Créer un correctif de runtime

Si il n’est pas encore un runtime corriger le problème que vous souhaitez résoudre, vous pouvez créer un nouveau correctif de runtime en écriture de fonctions de remplacement et en incluant les données de configuration qui convient. Examinons chaque partie.

### <a name="replacement-functions"></a>Fonctions de remplacement

Tout d’abord, identifiez l’appels échouent lorsque votre application s’exécute dans un conteneur MSIX (fonction). Ensuite, vous pouvez créer des fonctions de remplacement que vous souhaitez que le Gestionnaire de runtime pour appeler à la place. Cela vous donne la possibilité de remplacer l’implémentation d’une fonction avec un comportement qui est conforme aux règles de l’environnement d’exécution modernes.

Dans Visual Studio, ouvrez le projet de correctif d’exécution que vous avez créé précédemment dans ce guide.

Déclarez le ``FIXUP_DEFINE_EXPORTS`` (macro), puis ajoutez une instruction include pour le `fixup_framework.h` en haut de chaque. Fichier CPP où vous envisagez d’ajouter les fonctions de votre correctif du runtime.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Assurez-vous que le `FIXUP_DEFINE_EXPORTS` macro apparaît avant l’instruction include.

Créer une fonction qui a la même signature de la fonction qui a comportement que vous souhaitez modifier. Voici un exemple de fonction qui remplace le `MessageBoxW` (fonction).

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

L’appel à `DECLARE_FIXUP` mappe le `MessageBoxW` (fonction) pour votre nouvelle fonction de remplacement. Lorsque votre application tente d’appeler le `MessageBoxW` (fonction), il appelle la fonction de remplacement à la place.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protection contre récursive des appels aux fonctions runtime correctifs

Vous pouvez éventuellement appliquer la `reentrancy_guard` type à vos fonctions qui protègent contre récursive des appels aux fonctions runtime correctifs.

Par exemple, vous pouvez générer une fonction de remplacement pour le `CreateFile` (fonction). Votre implémentation peut appeler le `CopyFile` (fonction), mais l’implémentation de la `CopyFile` fonction peut appeler le `CreateFile` (fonction). Cela peut conduire à un cycle récurrents infinis d’appels à la `CreateFile` (fonction).

Pour plus d’informations sur `reentrancy_guard` consultez [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Données de configuration

Si vous souhaitez ajouter des données de configuration pour votre correctif du runtime, envisagez d’ajouter à la ``config.json``. De cette façon, vous pouvez utiliser le `FixupQueryCurrentDllConfig` facilement analyser ces données. Cet exemple analyse une valeur booléenne et la chaîne à partir de ce fichier de configuration.

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

Bien que Visual Studio vous donne le développement la plus simple et l’expérience de débogage, il existe certaines limitations.

Tout d’abord, le débogage F5 exécute l’application par les fichiers libres dans le chemin de dossier de disposition de package de déploiement, au lieu de l’installation à partir d’un .msix / package .appx.  En général, le dossier de disposition n’a pas les mêmes restrictions de sécurité comme un dossier de package installé. Par conséquent, il peut-être pas possible de reproduire les erreurs de refus package chemin d’accès avant d’appliquer un correctif de runtime.

Pour résoudre ce problème, utilisez .msix / déploiement du package .appx plutôt que F5 perdent le déploiement de fichiers.  Pour créer un .msix / fichier de package .appx, utilisez le [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) utilitaire à partir du SDK Windows, comme décrit ci-dessus. Ou, à partir de Visual Studio, cliquez sur le nœud de votre projet d’application et sélectionnez **Store**->**créer des Packages d’application**.

Un autre problème avec Visual Studio est qu’il n’a pas de prise en charge intégrée pour l’attachement à des processus enfants lancés par le débogueur.   Cela rend difficile à déboguer la logique dans le chemin d’accès de démarrage de l’application cible, qui doit être liée manuellement par Visual Studio après le lancement.

Pour résoudre ce problème, utilisez un débogueur qui prend en charge l’attachement de processus enfant.  Notez qu’il n’est généralement pas possible d’attacher un débogueur de (JIT) juste-à-temps à l’application cible.  Il s’agit, car la plupart des techniques JIT implique le lancement du débogueur à la place de l’application cible, par le biais de la clé de Registre ImageFileExecutionOptions.  Cela va à l’encontre du mécanisme detouring utilisé par PSFLauncher.exe pour injecter FixupRuntime.dll dans l’application cible.  WinDbg, inclus dans le [outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)et obtenues à partir de la [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), attacher un processus enfant de prend en charge.  Il prend également en charge directement [lancement et le débogage d’une application UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Pour déboguer le démarrage de l’application cible en tant qu’un processus enfant, démarrer ``WinDbg``.

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

À la ``WinDbg`` inviter, activer le débogage des enfants et définissez des points d’arrêt appropriés.

```ps
.childdbg 1
g
```

(exécuter jusqu'à ce que l’application cible démarre et arrête le débogueur)

```ps
sxe ld fixup.dll
g
```

(exécuter jusqu'à ce que la DLL est chargée de correction)

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) peut également être utilisé pour attacher un débogueur à une application lors de son lancement et est également inclus dans le [outils de débogage pour Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Toutefois, il est plus complexe à utiliser que la prise en charge directe désormais fourni par WinDbg.

## <a name="support-and-feedback"></a>Support et commentaires

**Trouvez des réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
