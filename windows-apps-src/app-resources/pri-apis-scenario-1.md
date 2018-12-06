---
Description: In this scenario, we'll make a new app to represent our custom build system. We'll create a resource indexer and add strings and other kinds of resources to it. Then we'll generate and dump a PRI file.
title: Scénario 1 Générer un fichier PRI à partir de ressources de chaîne et de fichiers de ressources
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: 9b14e413a5629dfb5447750e32c42c4efafef8fa
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8744765"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>Scénario1: Générer un fichier IRP à partir de ressources de chaîne et de fichiers de ressources
Dans ce scénario, nous allons utiliser les [API d’indexation de ressource de package (IRP)](https://msdn.microsoft.com/library/windows/desktop/mt845690) pour créer une application pour représenter notre système de génération personnalisé. N’oubliez pas que l’objectif de ce système de génération personnalisé est de créer des fichiers PRI pour une application UWP cible. Par conséquent, dans le cadre de cette procédure pas à pas, nous allons créer des exemples de fichiers de ressources (contenant des chaînes et autres types de ressources) pour représenter les ressources de cette application UWP cible.

## <a name="new-project"></a>Nouveau projet
Commencez par créer un nouveau projet dans Microsoft Visual Studio. Créez un projet d’**application de console Windows VisualC++** et nommez-le *CBSConsoleApp* (pour «application de console de système de génération personnalisé»).

Choisissez *x64* dans la liste déroulante **Plateformes de solution**.

## <a name="headers-static-library-and-dll"></a>En-têtes, bibliothèque statique et dll
Les API IRP sont déclarées dans le fichier d’en-tête MrmResourceIndexer.h (qui est installé dans `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\`). Ouvrez le fichier `CBSConsoleApp.cpp`et ajoutez l’en-tête, ainsi que tout autre en-tête dont vous aurez besoin.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

Les API sont implémentées dans MrmSupport.dll, auquel vous accédez en l’associant à la bibliothèque statique MrmSupport.lib. Ouvrez les **Propriétés** de votre projet, cliquez sur **Éditeur de liens** > **Entrée**, modifiez **Dépendances supplémentaires** et ajoutez `MrmSupport.lib`.

Générez la solution, puis copiez `MrmSupport.dll` de `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` dans votre dossier de sortie (probablement `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\`).

Ajoutez la fonction d’assistance suivante pour `CBSConsoleApp.cpp`, car vous allez en avoir besoin.

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

Dans la fonction `main()`, ajoutez des appels pour initialiser et annuler l’initialisation de COM.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>Fichiers de ressources appartenant à l’application UWP cible
Nous allons maintenant avoir besoin d’exemples de fichiers de ressources (contenant des chaînes et autres types de ressources) pour représenter les ressources de l’application UWP cible. Celles-ci peuvent, bien entendu, résider n’importe où dans votre système de fichiers. Cependant, dans le cadre de cette procédure pas à pas, il est judicieux de les placer dans le dossier de projet de CBSConsoleApp afin que tous les éléments se trouvent au même endroit. Il suffit d’ajouter ces fichiers de ressources au système de fichiers; ne les ajoutez pas au projet CBSConsoleApp.

Dans le dossier qui contient `CBSConsoleApp.vcxproj`, ajoutez un nouveau sous-dossier nommé `UWPAppProjectRootFolder`. Dans ce nouveau sous-dossier, créez ces exemples de fichiers de ressources.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
Ce fichier peut contenir n’importe quelle image PNG.

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>Indexez les ressources et créez un fichier PRI
Dans la fonction `main()`, avant l’appel d’initialisation de COM, déclarez certaines chaînes dont vous aurez besoin, mais également le dossier de sortie dans lequel vous allons générer le fichier PRI.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

Immédiatement après l’appel d’initialisation de COM, déclarez un descripteur d’indexeur de ressources, puis appelez [**MrmCreateResourceIndexer**]() pour créer un indexeur de ressources.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

Voici une explication des arguments passés à **MrmCreateResourceIndexer **.

- Nom de la famille de packages de l’application UWP cible, qui sera utilisé comme nom de mappage de ressources lorsque de la génération d’un fichier PRI à partir de cet indexeur de ressources.
- Racine du projet de l’application UWP cible. En d’autres termes, le chemin d’accès aux fichiers de ressources. Précisez-le pour pouvoir ensuite indiquer des chemins d’accès par rapport à cette racine dans les appels d’API suivants au même indexeur de ressources.
- Version de Windows à cibler.
- Liste des qualificateurs de ressources par défaut.
- Pointeur vers le descripteur d’indexeur de ressources, pour que la fonction puisse le définir.

L’étape suivante consiste à ajouter vos ressources à l’indexeur de ressources que vous venez de créer. `resources.resw` est un fichier de ressources (.resw) qui contient les chaînes neutres pour l’application UWP cible. Faites défiler la page vers le haut (de cette rubrique) pour afficher son contenu. `de-DE\resources.resw` contient les chaînes en allemand et `en-US\resources.resw` les chaînes en anglais. Pour ajouter les ressources de chaîne contenues dans un fichier de ressources à un indexeur de ressources, vous appelez [**MrmIndexResourceContainerAutoQualifiers **](). Enfin, appelez la fonction [**MrmIndexFile**]() d’un fichier contenant une ressource d’image neutre dans l’indexeur de ressources.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

Dans l’appel à **MrmIndexFile**, la valeur L" ms-resource:///Files/sample-image.png" est l’uri de ressource. Le premier segment du chemin d’accès est «Files», qui sera utilisé comme nom de sous-arborescence de mappage de ressources lorsque vous génèrerez un fichier PRI à partir de l’indexeur de cette ressource.

Maintenant que vous avez informé l’indexeur de ressources des fichiers de ressources, il est temps qu’il génère un fichier PRI sur le disque en appelant la fonction [**MrmCreateResourceFile**]().

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

À ce stade, un fichier PRI nommé `resources.pri` a été créé dans un dossier nommé `Generated PRIs`. Maintenant que vous avez terminé avec l’indexeur de ressource, nous appelons [**MrmDestroyIndexerAndMessages**]() pour détruire son descripteur et libérer les ressources de l’ordinateur qu’il a allouées.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

Dans la mesure où un fichier PRI est binaire, il sera plus facile d’afficher ce que nous venons de générer si nous vidons le fichier PRI binaire au format XML équivalent. C’est ce que fait un appel à [**MrmDumpPriFile**]().

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

Voici une explication des arguments passés à **MrmDumpPriFile**.

- Chemin d’accès au fichier PRI à vider. Nous n’utilisons pas l’indexeur de ressource dans cet appel (nous venons de le détruire), nous devons donc spécifier un chemin d’accès complet au fichier.
- Aucun fichier de schéma. Nous verrons ce qu’est un schéma plus loin dans la rubrique.
- Voici simplement les informations de base.
- Chemin d’accès au fichier XML à créer.

Voici ce que contient le fichier PRI, vidé ici, au format XML.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

Les informations commencent par un mappage des ressources, qui est nommé avec le nom de la famille de packages de notre application UWP cible. Délimité par le mappage des ressources, deux sous-arborescences de mappage de ressources: une pour les ressources de fichier que vous avez indexées et l’autre pour vos ressources de chaîne. Notez la façon dont le nom de la famille de packages a été inséré dans toutes les URI de ressource.

La première chaîne de ressource est *EnOnlyString* de `en-US\resources.resw` et elle contient un seul candidat (qui correspond au qualificateur *langue - en-US*). Vient ensuite *LocalizedString1* de `resources.resw` et `en-US\resources.resw`. Par conséquent, elle contient deux candidats: une correspondant à *language-en-US*et un candidat neutre de secours qui correspond à tout contexte. De même, *LocalizedString2* a deux candidats: *language-de-DE* et neutre. Et, enfin, *NeutralOnlyString* existe uniquement dans un format neutre. Je lui ai donné ce nom pour qu’il soit clair que ceci ne doit pas être localisée.

## <a name="summary"></a>Résumé
Dans ce scénario, nous vous avons montré comment utiliser les [API d’indexation de ressource de package (IRP)](https://msdn.microsoft.com/library/windows/desktop/mt845690) pour créer un indexeur de ressources. Nous avons ajouté des ressources de chaîne et des fichiers de ressources à l’indexeur de ressources. Ensuite, nous avons utilisé l’indexeur de ressources pour généré un fichier PRI binaire. Enfin, nous avons vidé le fichier PRI binaire au format XML afin de confirmer qu’elle contient les informations prévues.

## <a name="important-apis"></a>API importantes
* [Référence d’indexation de ressource de package (IRP)](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Rubriquesassociées
* [API d’indexation de ressources de package (IRP) et systèmes de génération personnalisés](pri-apis-custom-build-systems.md)
