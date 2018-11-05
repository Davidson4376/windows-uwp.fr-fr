---
author: laurenhughes
title: Création de package à l'aide de la disposition de mise en package
description: La disposition de mise en package constitue un unique document décrivant la structure de mise en package de l'application. Il spécifie les ensembles d'une application (principaux et facultatifs), les packages contenus dans les ensembles ainsi que les fichiers contenus dans les packages.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
keywords: windows10, création de packages, disposition de package, package d'actifs
ms.localizationpriority: medium
ms.openlocfilehash: 9342b4ce35cb50037813ed2210e2d7246411ad92
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032983"
---
# <a name="package-creation-with-the-packaging-layout"></a>Création de package à l'aide de la disposition de mise en package  

Grâce à l'introduction aux packages d'actifs, les développeurs disposent désormais des outils nécessaires à la génération de davantage de packages ainsi que d'autres types de packages. À mesure qu'une application devient plus vaste et plus complexe, elle sera souvent composée de davantage de packages. La gestion de ces packages devient alors plus difficile (en particulier si vous générez en dehors de VisualStudio en utilisant des fichiers de mappage). Pour simplifier la gestion de la structure de package d'une application, vous pouvez utiliser la disposition de mise en package prise en charge par MakeAppx.exe. 

La disposition de mise en package constitue un unique document XML décrivant la structure de mise en package de l'application. Il spécifie les ensembles d'une application (principaux et facultatifs), les packages contenus dans les ensembles ainsi que les fichiers contenus dans les packages. Les fichiers peuvent être sélectionnés depuis différents emplacements de dossiers, de disques ou de réseau. Les caractères génériques peuvent être utilisés pour sélectionner ou exclure des fichiers.

Une fois la disposition de mise en package d'une application configurée, MakeAppx.exe est utilisé pour créer tous les packages pour une application dans un appel de ligne de commande unique. La disposition de mise en package peut être modifiée pour altérer la structure de package afin qu'elle corresponde à vos besoins de déploiement. 


## <a name="simple-packaging-layout-example"></a>Exemple de disposition de mise en package simple

Voici un exemple de disposition de mise en package simple:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

Décomposons cet exemple pour comprendre son fonctionnement

### <a name="packagefamily"></a>PackageFamily
Cette disposition de mise en package créera un fichier d’un ensemble d’applications application plat unique avec une x64 package d’architecture et un package d’actifs «Media». 

L'élément **PackageFamily** est utilisé pour définir un ensemble d'applications. Vous devez utiliser l'attribut **ManifestPath** pour fournir un élément **AppxManifest** à l'ensemble. L'élément **AppxManifest** doit correspondre à l'élément **AppxManifest** pour le package d'architecture de l'ensemble. L'attribut **ID** doit également être fourni. Cet élément est utilisé avec MakeAppx.exe lors de la création de package. Ainsi, vous pouvez créer uniquement ce package si vous le souhaitez, il s'agira alors du nom de fichier pour le package obtenu. L'attribut **FlatBundle** est utilisé pour décrire le type d'ensemble que vous souhaitez créer, l'attribut **true** pour un ensemble plat (vous trouverez plus d'informations ici) et l'attribut **false** pour un ensemble classique. L'attribut **ResourceManager** est utilisé pour spécifier si les packages de ressources dans cet ensemble utiliseront MRT afin d'accéder aux fichiers. La valeur par défaut est **true**, mais à compter de la version 1803 de Windows10, il n'est pas encore prêt. L'attribut doit donc être défini sur **false**.


### <a name="package-and-assetpackage"></a>Package et AssetPackage
Dans l'élément **PackageFamily**, les packages contenus ou référencés par l'ensemble d'applications sont définis. Ici, le package d'architecture (également appelé package principal) est défini avec l'élément **Package** et le package d'actifs est défini avec l'élément **AssetPackage**. Le package d'architecture doit spécifier l'architecture pour laquelle le package est destiné: «x64», «x86» «arm» «neutral». Vous pouvez également fournir directement (facultatif) un élément **AppxManifest** spécifiquement pour ce package à l'aide de l'attribut **ManifestPath** à nouveau. Si aucun élément **AppxManifest** n’est fourni, il sera automatiquement généré à partir de l'élément **AppxManifest** fourni pour l'élément **PackageFamily**. 

Les éléments par défaut et **AppxManifest** seront générés pour chaque package à l'intérieur de l'ensemble. Pour le package d'actifs, vous pouvez également définir l'attribut **AllowExecution**. La définition de la valeur **false** (par défaut) permet de réduire le délai de publication de votre application. En effet, pour les packages n'ayant pas besoin d'être exécutés l'analyse des virus ne bloquera pas le processus de publication (vous trouverez pus d'informations à ce sujet en consultant [Introduction aux packages d'actifs](asset-packages.md)). 

### <a name="files"></a>Fichiers
Dans chaque définition de package, vous pouvez utiliser l'élément **Fichier** afin de sélectionner les fichiers à inclure à ce package. L'attribut **SourcePath** définit l'emplacement local des fichiers. Vous pouvez sélectionner les fichiers à partir de différents dossiers (en fournissant les chemins relatifs), de différents disques (en fournissant les chemins absolus) et même à partir de réseaux partagés (en fournissant un chemin semblable à `\\myshare\myapp\*`). Le **DestinationPath** est la destination vers laquelle les fichiers sont transférés dans le package, en fonction de la racine du package. **ExcludePath** peut être utilisé (au lieu des deuxautres attributs) pour sélectionner les fichiers à exclure des fichiers sélectionnés par les autres éléments **Fichier** dans les attributs **SourcePath** au sein du même package.

Chaque élément **Fichier** peut être utilisé pour sélectionner plusieurs fichiers à l’aide de caractères génériques. En règle générale, un seul caractère générique (`*`) peut être utilisé indéfiniment dans le chemin. Toutefois, un même caractère générique correspondra uniquement aux fichiers dans un dossier et non dans les sous-dossiers. Par exemple, `C:\MyGame\*\*` peut être utilisé dans le **SourcePath** pour sélectionner les fichiers `C:\MyGame\Audios\UI.mp3` et `C:\MyGame\Videos\intro.mp4`, mais il le peut pas sélectionner `C:\MyGame\Audios\Level1\warp.mp3`. Le caractère générique double (`**`) peut également être utilisé dans les noms de dossier et de fichier pour correspondre à tout élément de manière récursive (mais il ne peut pas se trouver à côté des noms partiels). Par exemple, `C:\MyGame\**\Level1\**` peut sélectionner `C:\MyGame\Audios\Level1\warp.mp3` et `C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4`. Les caractères génériques peuvent également être utilisés pour modifier directement les noms de fichier dans le cadre du processus de mise en package si les caractères génériques sont utilisés à différents endroits entre la source et la destination. Par exemple, le chemin `C:\MyGame\Audios\*` pour **SourcePath** et le chemin `Sound\copy_*` pour **DestinationPath** peuvent sélectionner `C:\MyGame\Audios\UI.mp3` et le faire apparaître dans le package sous `Sound\copy_UI.mp3`. En général, le nombre de caractères génériques uniques et doubles doivent être les mêmes pour **SourcePath** et **DestinationPath** d'un même élément **Fichier**.


## <a name="advanced-packaging-layout-example"></a>Exemple de disposition de mise en package avancée

Voici un exemple de disposition de mise en package plus compliquée:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

Cet exemple diffère de l'exemple simple avec l'ajout des éléments **ResourcePackage** et **Facultatif**.

Les packages de ressources peuvent être spécifiés dans l'élément **ResourcePackage**. Dans le **ResourcePackage**, l'élément **Ressources** doit être utilisé pour spécifier les qualificateurs de ressource du pack de ressources. Les qualificateurs de ressource sont les ressources prises en charge par le pack de ressources. Ici, nous visualisons deuxpacks de ressources qui contiennent chacun les fichiers spécifiques en anglais et en français. Un pack de ressources peut disposer de plus d'un qualificateur. Il suffit d'ajouter un autre élément **Ressource** dans **Ressources**. Une ressource par défaut pour la dimension de ressource doit également être spécifiée si la dimension existe (les dimensions sont la langue, l'échelle, l'élément dxfl). Dans le cas présent, vous voyons que l'anglais est la langue par défaut. Donc, les utilisateurs n'ayant pas nécessairement de langue de système en français auront accès au téléchargement de secours du pack de ressources en anglais et de l'affichage en anglais.


Les packages facultatifs ont leurs propres noms de famille de package distincts et doivent être définis avec des éléments **PackageFamily** tout e spécifiant l'attribut **Facultatif** sur **true**. L'attribut **RelatedSet** est utilisé pour spécifié si le package facultatif se trouve dans l'ensemble connexe (il doit l'être par défaut), peut importe si le package facultatif doit être mis à jour avec le package principal.

L’élément **PrebuiltPackage** est utilisé pour ajouter des packages qui ne sont pas définis dans la disposition de création de packages à inclure ou à référencer dans les fichiers d’un ensemble d’applications application à générer. Dans ce cas, un autre package DLC facultatif est en cours inclus ici afin que le fichier d’un ensemble d’applications application principal puisse le référencer et inclure dans l’ensemble connexe.


## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>Générer des packages d'application avec une disposition de mise en package et l'outil MakeAppx.exe
Une fois la disposition de mise en package préparée pour votre application, vous pouvez démarrer la génération des packages pour votre application à l'aide de l'outil MakeAppx.exe. Pour générer tous les packages définis dans la disposition de mise en package, utilisez la commande:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

Néanmoins, si vous mettez à jour votre application et que certains packages ne contiennent aucun des fichiers modifiés, vous pouvez générer uniquement les package ayant été modifiés. Avec l'utilisation de l'exemple de disposition de mise en package simple sur cette page et la génération du package d'architecture x64, votre commande doit être semblable à ceci:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

L'indicateur `/id` peut être utilisé pour sélectionner les packages à générer à partir de la disposition de mise en package. Il correspond à l'attribut **ID** de la disposition. L'attribut `/ip` est utilisé pour indiquer l'emplacement de la version précédente des packages dans ce cas. La version précédente doit être fournie dans la mesure où le fichier d’un ensemble d’applications application doit tout de même faire référence à la version précédente du package de **média** . L'indicateur `/iv` est utilisé pour incrémenter automatiquement la version des packages en cours de génération (au lieu de modifier la version dans l'élément **AppxManifest**). Sinon, les commutateurs `/pv` et `/bv` peuvent être utilisés pour fournir directement une version de package (pour tous les packages à créer) et une version d'ensemble (pour tous les ensembles à créer) respectivement.
À l'aide de l'exemple de disposition de mise en package avancée de cette page, si vous souhaitez uniquement générer l'ensemble facultatif **Themes** et le package d'application **Themes.main** auquel il fait référence, vous pourriez utiliser cette commande:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

L'indicateur `/bc` est utilisé pour dénoter que les descendants de l'ensemble **Themes** doivent également être généré (dans ce cas, **Themes.main** sera généré). L'indicateur `/nbp` est utilisé pour dénoter que les parents de l'ensemble **Themes** ne doivent pas être générés. Les parents de l'ensemble **Themes**, ensemble d'applications facultatif, constituent l'ensemble d'applications principal: **MyGame**. De manière habituelle pour le package facultatif d'un ensemble connexe, l'ensemble d'applications principal doit également être généré pour que le package facultatif puisse être installé. En effet, le package facultatif est également référencé dans l'ensemble d'applications principal lorsqu'il se trouve dans un ensemble connexe (la garantie de version entre le package principal et le package facultatif). La relation parent-descendant entre les packages est illustrée dans le diagramme suivant:

![Diagramme de disposition de mise en package](images/packaging-layout.png)
