---
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: Créer et convertir un mappage de groupe de contenu source
description: Pour préparer votre application de plateforme Windows universelle (UWP) pour l’installation en continu d’une application UWP, vous devez créer un mappage de groupe de contenu. Cet article vous guidera dans les spécificités de la création et la conversion d’un mappage de groupe de contenu tout en vous fournissant des conseils et astuces tout au long du processus.
ms.date: 9/30/2018
ms.topic: article
keywords: Windows10, UWP, mappage de groupe de contenu, installation en continu, installation en continu d’une application UWP, mappage de groupe de contenu source
ms.localizationpriority: medium
ms.openlocfilehash: ea6e83521007572449b28e65bdff56d9d2c11186
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8696805"
---
# <a name="create-and-convert-a-source-content-group-map"></a>Créer et convertir un mappage de groupe de contenu source

Pour préparer votre application de plateforme Windows universelle (UWP) pour l’installation en continu d’une application UWP, vous devez créer un mappage de groupe de contenu. Cet article vous guidera dans les spécificités de la création et de la conversion d’un mappage de groupe de contenu tout en vous fournissant des conseils et astuces tout au long du processus.

## <a name="creating-the-source-content-group-map"></a>Créer le mappage de groupe de contenu source

Vous devrez créer un fichier `SourceAppxContentGroupMap.xml` et ensuite utiliser Visual Studio ou l’outil **MakeAppx.exe** pour convertir ce fichier en version finale: `AppxContentGroupMap.xml`. Il est possible d’ignorer une étape en créant le fichier `AppxContentGroupMap.xml` à partir de zéro, mais il est recommandé (et généralement plus facile) de créer le fichier `SourceAppxContentGroupMap.xml` et de le convertir étant donné que les caractères génériques ne sont pas autorisés dans les fichiers `AppxContentGroupMap.xml` (et qu’ils sont vraiment utiles). 

Passons en revue un scénario simple dans lequel il peut être avantageux d’utiliser une installation en continu d’une application UWP. 

Supposons que vous avez créé un jeu UWP, mais que la taille de votre application finale est supérieure à 100Go. Qui va prendre un certain temps à télécharger à partir du Microsoft Store, ce qui peut s’avérer peu pratique. Si vous choisissez d’utiliser l’installation en continu d’une application UWP, vous pouvez spécifier l’ordre dans lequel les fichiers de votre application sont téléchargés. En indiquant au Windows Store de télécharger les fichiers essentiels en premier, l’utilisateur sera en mesure d’entrer en contact avec votre application plus tôt pendant que le téléchargement des fichiers non essentiels se poursuit en arrière-plan.

> [!NOTE]
> L’utilisation de l’installation en continu d’une application UWP repose principalement sur l’organisation des fichiers de votre application. Il est recommandé de penser au plus tôt à la disposition du contenu de votre application lors de l’installation en continu d’une application UWP, pour faciliter la segmentation des fichiers de votre application.

Tout d’abord, nous allons créer un fichier `SourceAppxContentGroupMap.xml`.

Avant d’aborder les détails, voici un exemple d’un fichier `SourceAppxContentGroupMap.xml` simple et complet:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

Il existe deux composants principaux dans un mappage de groupe de contenu: la section **requise**, qui contient le groupe de contenu requis et la section **automatique**, qui peut contenir plusieurs groupes de contenu automatique.

### <a name="required-content-group"></a>Groupe de contenu requis

Le groupe de contenu requis est un groupe de contenu unique au sein de l’élément `<Required>` du fichier `SourceAppxContentGroupMap.xml`. Un groupe de contenu requis doit contenir tous les fichiers essentiels nécessaires pour lancer l’application avec une expérience utilisateur minimale. En raison de la compilation .NET Native, la totalité du code (l’exécutable de l’application) doit faire partie du groupe requis, laissant les ressources et les autres fichiers aux groupes automatiques.

Par exemple, si votre application est un jeu, le groupe requis peut inclure des fichiers utilisés dans le menu principal ou dans l’écran d’accueil du jeu.

Voici l’extrait de code à partir de l’exemple de fichier `SourceAppxContentGroupMap.xml` d’origine: 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

Il existe quelques points importants à noter ici:

- Le `<ContentGroup>` au sein de l’élément `<Required>` **doit** être nommé «Required». Ce nom est uniquement réservé au groupe de contenu requis et ne peut être utilisé avec un autre `<ContentGroup>` dans le mappage du groupe de contenu final.
- Il n’y a qu’un `<ContentGroup>`. Ceci est intentionnel, dans la mesure où il ne doit y avoir qu’un seul groupe de fichiers essentiels.
- Le fichier dans cet exemple est un fichier `.exe` unique. Un groupe de contenu requis n’est pas limité à un seul fichier, il peut y en avoir plusieurs. 

Un moyen simple de commencer à écrire ce fichier consiste à ouvrir une nouvelle page dans l’éditeur de texte de votre choix, «Enregistrer sous» votre fichier dans le dossier de projet de votre application et nommer votre fichier nouvellement créé: `SourceAppxContentGroupMap.xml`.

> [!IMPORTANT]
> Si vous développez une application UWP en C++, vous devez ajuster les propriétés de votre fichier `SourceAppxContentGroupMap.xml`. Définissez la propriété `Content` sur **true** et la propriété `File Type` sur **fichier XML**. 

Lorsque vous créez le fichier `SourceAppxContentGroupMap.xml`, il est utile de tirer parti de l’utilisation de caractères génériques dans les noms de fichier. Pour plus d’informations, voir la section [Conseils et astuces pour l’utilisation de caractères génériques](#wildcards).

Si vous avez développé votre application à l’aide de Visual Studio, il est recommandé d’inclure ceci dans votre groupe de contenu requis:

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

Ajouter le nom de fichier générique unique inclut les fichiers ajoutés au répertoire du projet dans Visual Studio, tels que l’exécutable de l’application ou les DLL. Les dossiers WinMetadata et Propriétés servent à inclure les autres dossiers que Visual Studio génère. Les caractères génériques de la ressource servent à sélectionner les images de logo et d’élément SplashScreen qui sont nécessaires à l’installation de l’application.

Notez que vous ne pouvez pas utiliser le caractère générique double «**» à la racine de la structure de fichiers pour inclure tous les fichiers dans le projet, dans la mesure où cette opération échoue lorsque vous essayez de convertir `SourceAppxContentGroupMap.xml` en fichier final `AppxContentGroupMap.xml`.

Il est également important de noter que les fichiers d’empreinte (AppxManifest.xml, AppxSignature.p7x, resources.pri, etc.) ne doivent pas être inclus dans le mappage de groupe de contenu. Si les fichiers d’empreinte sont inclus dans un des noms de fichier à caractère générique que vous spécifiez, ils seront ignorés.

### <a name="automatic-content-groups"></a>Groupes de contenu automatique

Les groupes de contenu automatique sont les ressources qui sont téléchargées en arrière-plan, pendant que l’utilisateur interagit avec les groupes de contenu déjà téléchargés. Ils contiennent des fichiers supplémentaires qui ne sont pas indispensables au lancement de l’application. Par exemple, vous pouvez répartir des groupes de contenu automatique sur différents niveaux et définir chaque niveau comme un groupe de contenu distinct. Comme indiqué dans la section groupe de contenu requis: en raison de la compilation .NET Native, la totalité du code (l’exécutable de l’application) doit faire partie du groupe requis, laissant les ressources et les autres fichiers aux groupes automatiques.

Examinons de plus près le groupe automatique de contenu à partir de notre exemple `SourceAppxContentGroupMap.xml`:
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

La disposition du groupe automatique est assez semblable à celle du groupe requis, à quelques exceptions près:

- Il existe plusieurs groupes de contenu.
- Les groupes de contenu automatique peuvent avoir des noms uniques à l’exception du nom «Required» qui est réservé au groupe de contenu requis.
- Les groupes de contenu automatique ne peuvent contenir **aucun** fichier issu du groupe de contenu requis. 
- Un groupe de contenu automatique peut contenir des fichiers qui se trouvent également dans d’autres groupes de contenu automatique. Les fichiers ne seront téléchargés qu’une seule fois et seront téléchargés par le premier groupe de contenu automatique qui les contient.

#### Conseils et astuces pour l’utilisation de caractères génériques<a name="wildcards"></a>

La disposition d’un fichier pour les mappages de groupe de contenu est toujours relative au dossier racine de votre projet.

Dans notre exemple, les caractères génériques sont utilisés dans les deux éléments `<ContentGroup>` pour récupérer tous les fichiers d’un niveau de fichier de «Assets\Level2» ou «Assets\Level3». Si vous utilisez une structure de dossiers plus approfondie, vous pouvez utiliser le caractère générique double:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

Vous pouvez également utiliser des caractères génériques avec du texte pour les noms de fichier. Par exemple, si vous souhaitez inclure tous les fichiers dans votre dossier «Ressources» avec un nom de fichier qui contient «Level2» vous pouvez utiliser ce type d’exemple:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>Convertir SourceAppxContentGroupMap.xml en AppxContentGroupMap.xml

Pour convertir le fichier `SourceAppxContentGroupMap.xml` vers la version finale, `AppxContentGroupMap.xml`, vous pouvez utiliser Visual Studio2017 ou l’outil de ligne de commande **MakeAppx.exe**.

Pour utiliser Visual Studio pour convertir votre mappage de groupe de contenu:
1. Ajoutez le fichier `SourceAppxContentGroupMap.xml` à votre dossier de projet
2. Dans la fenêtre Propriétés, modifiez l’Action de génération de `SourceAppxContentGroupMap.xml` en «AppxSourceContentGroupMap»
2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet
3. Accédez au Windows Store -> Convertissez le fichier de mappage de groupe de contenu

Si vous n’avez pas développé votre application dans Visual Studio, ou si vous préférez simplement utiliser la ligne de commande, utilisez l’outil **MakeAppx.exe** permettant de convertir votre fichier `SourceAppxContentGroupMap.xml`. 

Une commande **MakeAppx.exe** simple peut ressembler à ceci:
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

L’option /s spécifie le chemin d’accès au fichier `SourceAppxContentGroupMap.xml`, et /f spécifie le chemin d’accès au fichier `AppxContentGroupMap.xml`. La dernière possibilité, /d, spécifie le répertoire qui doit être utilisé pour le développement de caractères génériques de nom de fichier, dans ce cas, il s’agit du répertoire de projet de l’application.

Pour plus d’informations sur les options que vous pouvez utiliser avec **MakeAppx.exe**, ouvrez une invite de commandes, accédez à **MakeAppx.exe** et entrez:

```syntax
MakeAppx convertCGM /?
```

C’est tout ce dont vous avez besoin pour obtenir votre fichier `AppxContentGroupMap.xml` final et prêt pour votre application! Il est encore plus faire avant que votre application soit entièrement prête pour le Microsoft Store. Pour plus d’informations sur l’ensemble du processus d’ajout d’une installation en continu d’une application UWP à votre application, consultez [ce billet de blog](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).
