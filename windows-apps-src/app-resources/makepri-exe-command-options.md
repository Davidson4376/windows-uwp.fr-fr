---
author: stevewhims
Description: MakePri.exe has the set of commands createconfig, dump, new, resourcepack, and versioned. This topic details their use.
title: Options de ligne de commande de MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: c777996dceeb443c25fcf526e3a029fca00047c1
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7569552"
---
# <a name="makepriexe-command-line-options"></a>Options de ligne de commande de MakePri.exe

[MakePri.exe](compile-resources-manually-with-makepri.md) inclut les commandes `createconfig`, `dump`, `new`, `resourcepack` et `versioned`. Cette rubrique détaille les options de ligne de commande utilisées avec ces commandes.

> [!NOTE]
> MakePri.exe est installé lorsque vous vérifiez l’option **Kit SDK Windows pour les applications UWP managées** lors de l’installation du Kit de développement Windows. Il est installé dans le chemin d’accès `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (ainsi que dans les dossiers nommés pour les autres architectures). Exemple: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="getting-help-from-the-command-line"></a>Obtention d’aide à partir de la ligne de commande

Vous pouvez exécuter `MakePri.exe help` ou `MakePri.exe /?` pour afficher les commandes que vous pouvez utiliser avec MakePri.exe. Vous pouvez également émettre `MakePri.exe <command> /?` pour connaître les spécificités sur une commande et, dans de très rares cas, même `MakePri.exe <command> <option>` pour voir plus d’informations sur une option.

## <a name="makepri-commands"></a>Commandes MakePri

```console
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Commande createconfig

La commande `createconfig` crée un nouveau fichier de configuration PRI initialisé qui définit les valeurs de qualificateur par défaut que vous spécifiez. Exécutez `MakePri.exe createconfig /?` pour afficher l’aide détaillée de cette commande.

```console
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Commande dump

La commande `dump` génère un fichierxml vidé contenant la liste de toutes les ressources d’un fichier PRI spécifié. Exécutez `MakePri.exe dump /?` pour afficher l’aide détaillée de cette commande.

> [!NOTE]
> Un pack de ressources sans schéma a été créé avec le commutateur *omitSchemaFromResourcePacks* dans le fichier de configuration IRP. Pour vider un pack de ressources sans schéma, utilisez le commutateur `/es <main_package_PRI_file>`. Si vous ne spécifiez pas le fichier principal, vous voyez le message d’erreur «*Le fichier resources.pri dans le package a été endommagée et le chiffrement a échoué (erreur PRI222: 0xdef0000f - Une erreur non spécifiée s’est produite)*».

```console
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>Commande new

La commande `new` crée un fichier PRI en indexant les fichiers de votre projet, comme indiqué par votre fichier de configuration. Exécutez `MakePri.exe new /?` pour afficher l’aide détaillée de cette commande.

```console
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the AppX package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>Commande resourcepack

La commande `resourcepack` crée un fichier PRI en indexant les fichiers de votre projet, comme indiqué par votre fichier de configuration. Un fichier PRI de pack de ressources contient uniquement les variantes supplémentaires des ressources déjà spécifiées dans un fichier PRI existant. Exécutez `MakePri.exe resourcepack /?` pour afficher l’aide détaillée de cette commande.

```console
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>Commande versioned

La commande `versioned` crée un fichier PRI avec version en indexant les fichiers de votre projet, comme indiqué par votre fichier de configuration. Exécutez `MakePri.exe versioned /?` pour afficher l’aide détaillée de cette commande.

```console
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll(ex)

Vous utilisez l’option DLL d’extension (/ex) avec `createconfig`, `dump`, `new`, `resourcepack` et `versioned` pour spécifier l’emplacement de la DLL d’extension d’environnement du système de gestion des ressources.

## <a name="logging47metadata-file"></a>Fichier journal&#47;de métadonnées

MakePri peut inclure des informations spécifiques à un pack de ressources dans le fichier de métadonnées indexeur. Voici un exemple de fichier journal pour `resources.pri` avec les fichiers PRI de ressources `german.pri` et `highresolution.pri`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>Option &#47;IndexFile(if)

Vous utilisez l’option de fichier d’index (/if) avec `dump`, `resourcepack` et `versioned` pour spécifier un fichier PRI d’entrée.

Pour `resourcepack` et `versioned`, au lieu de fournir un fichier PRI en tant que paramètre d’entrée pour /IndexFile(if), vous pouvez fournir un fichier de schéma.

```console
/IndexFile(if) <FILEPATH>
```

**FILEPATH** est un jeton qui spécifie l’emplacement du fichier PRI d’entrée ou du fichier de schéma PRI.

## <a name="47indexoptionsio-option"></a>& #47;IndexOptions(io) option

Vous utilisez l’option d’options d’index (/ e/s) avec `new`, `resourcepack`, et `versioned` pour spécifier les options qui fournissent un contrôle précis sur le comportement des indexeurs de ressource. Options d’index sont désactivées par défaut.

```console
/IndexOptions(io) <OPTIONS>
```

**OPTIONS** est une liste séparée par des virgules constituée des options suivantes.

- +/-HiddenFiles(hf). Index (+) ou ignorer (-) les fichiers et dossiers cachés.
- +/-LinkedFiles(lf). Index (+) ou ignorer (-) lié des fichiers et dossiers.

## <a name="47mappingfilemf-option"></a>Option &#47;MappingFile(mf)

Vous utilisez l’option de fichier de mappage (/mf) avec `new`, `resourcepack` et `versioned` pour générer un fichier de mappage. [MakeAppx.exe](../packaging/create-app-package-with-makeappx-tool.md) utilise le fichier de mappage pour générer les packages d’application.

```console
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** est un jeton qui spécifie le format du fichier de mappage. Le seul format valide pris en charge est `appx`.

```console
/mf appx
```

Voici un exemple de contenu d’un fichier de mappage principal.

```console
"ResourceDimensions"                   "language-de-de"
```

Ceci est un exemple de contenu du fichier de mappage d’un pack de ressources.

```console
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>Résumé de sortie

Lorsque des packs de ressources sont créés, le résumé de sortie de MakePri.exe est plus détaillé. Voici un exemple.

```console
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>Option &#47;Overwrite(o)

Si l’option de remplacement (/o) n’est pas fournie, et que les fichiers de sortie spécifiés existent déjà, MakePri.exe requiert une confirmation avant de procéder au remplacement.

```console
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>Option &#47;OutputFile(of)

Vous utilisez l’option de fichier de sortie (/of) avec `dump`, `new`, `resourcepack` et `versioned` pour spécifier l’emplacement de sortie et le nom du fichier PRI à générer. Si MakePri.exe génère plusieurs fichiers PRI de ressources, il les place dans le dossier parent du fichier cible. Par exemple, si vous spécifiez `/of MyParentFolder\TargetFile.pri`, MakePri.exe génère `TargetFile.language-en.pri` et `TargetFile.scale-100.pri` avec `TargetFile.pri` sous `ParentFolder`.

Voici un exemple de condition d’erreur et le message d’erreur correspondant.

| Condition d’erreur | Message d’erreur |
| --------------- | ------------- |
| Le nom du fichier de sortie est identique à un des noms de pack de ressources dans la configuration. | Configuration non valide: le nom de pack de ressources <resource pack name> ne peut pas être identique à celui du fichier de sortie <nomfichiersortie.pri>. |

## <a name="reversemaprm-option"></a>Options /ReverseMap(rm)

Vous utilisez l’option de mappage inversé (/rm) avec `new`, `resourcepack` et `versioned` pour générer une section de mappage inversé dans le fichier PRI, afin de l’utiliser à des fins de débogage.

## <a name="47schemafilesf-option"></a>Option &#47;SchemaFile(sf)

Vous utilisez l’option de fichier de schéma (/sf) avec `new`, `resourcepack` et `versioned` pour écrire un fichier de schéma à l’emplacement spécifié.

Pour `resourcepack` et `versioned`, au lieu de fournir un fichier PRI en tant que paramètre d’entrée pour /IndexFile(if), vous pouvez fournir un fichier de schéma.

```console
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH** est un jeton qui spécifie l’emplacement où sera écrit le fichier de schéma.

Voici un exemple de fichier de schéma.

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>L’option &#47;VersionMajor(vma) est déconseillée

L’option de version principale (/vma) (pour la commande `new`) est déconseillée et son utilisation entraîne l’affichage du message d’avertissement suivant.

```console
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

Pour fournir le numéro de version principale, utilisez l’attribut [resources@majorVersion](makepri-exe-configuration.md) dans votre fichier de configuration.

## <a name="related-topics"></a>Rubriques associées

* [MakePri.exe](compile-resources-manually-with-makepri.md)
