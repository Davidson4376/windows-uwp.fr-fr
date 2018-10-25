---
author: laurenhughes
title: Développement de packages d'actifs et mise en dossier de packages
description: Découvrez comment organiser efficacement votre application avec des packages d'actifs et la mise en dossier des packages.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, création de packages, disposition de package, package d'actifs
ms.localizationpriority: medium
ms.openlocfilehash: 31c27430c850f861c8b97863521202a6dcab80f7
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5520775"
---
# <a name="developing-with-asset-packages-and-package-folding"></a>Développement de packages d'actifs et mise en dossier de packages 

> [!IMPORTANT]
> Si vous avez l'intention de soumettre votre application au Store, vous devez contacter le [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support) et obtenir l’autorisation d’utiliser des packages d'actifs et la mise en dossier de packages.

Les packages d'actifs peuvent réduire la taille de package général ainsi de le délai de publication de vos applications sur le Store. Vous trouverez plus d'informations sur les packages d'actifs et de quelle manière ils peuvent accélérer vos itérations de développement en consultant [Introduction aux packages d'actifs](asset-packages.md).

Si vous envisagez d'utiliser les packages d'actifs pour vos applications, vous vous demandez probablement de quelle façon les packages d'actifs modifieront votre processus de développement. En bref, en ce qui vous concerne, le développement de l'application reste le même grâce à la mise en dossier des packages d'actifs.

## <a name="file-access-after-splitting-your-app"></a>Accès au fichier après fractionnement de votre application

Pour comprendre que la mise en dossier des packages n'a aucune incidence sur votre processus de développement, il faut d'abord comprendre ce qui se produit lorsque vous fractionnez votre application en plusieurs packages (avec les packages d'actifs ou les packages de ressources). 

À un niveau élevé, lorsque vous fractionnez certains fichiers d'application dans d'autres packages (qui ne sont pas des packages d'architecture), vous n'êtes pas en mesure d'accéder à ces fichiers directement selon l'emplacement d'exécution de votre code. En effet, ces packages sont tous installés dans des répertoires différents de l'emplacement dans lequel votre package d'architecture est installé. Par exemple, si vous élaborez un jeu et que votre jeu est localisé en Français, allemand et vous générez pour x64 et x x86 machines, puis vous devez avoir ces fichiers de package d’application au sein de l’ensemble d’applications de votre jeu:

-   MyGame_1.0_x86.appx
-   MyGame_1.0_x64.appx
-   MyGame_1.0_language-fr.appx
-   MyGame_1.0_language-de.appx

Lorsque votre jeu est installé sur la machine d’un utilisateur, chaque fichier de package d’application dispose de son propre dossier dans le répertoire **WindowsApps** . De ce fait, pour un utilisateur français exécutant un système Windows 64bits, votre jeu sera similaire à ceci:

```example
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   `-- …
|-- MyGame_1.0_language-fr
|   `-- …
`-- …(other apps)
```

Notez que le package d’application les fichiers qui ne sont pas applicables à l’utilisateur ne sera pas installé (le x86 et packages en allemand). 

Pour cet utilisateur, l'exécutable principale de votre jeu se trouve dans le dossier **MyGame_1.0_x64** et sera exécuté à partir de là. En théorie, il aura accès aux fichiers contenus dans ce dossier. Pour accéder aux fichiers du dossier **MyGame_1.0_language-fr**, vous devez utiliser les API MRT ou les API PackageManager. Les API MRT peuvent sélectionner automatiquement le fichier le plus approprié à partir des langues installées, vous pouvez en savoir plus sur les API MRT [Windows.ApplicationModel.Resources.Core](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core). Vous pouvez également trouver l'emplacement d'installation du package linguistique en français à l'aide de la [Classe PackageManager](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager). Vous ne devez jamais supposer l'emplacement d'installation des packages de votre application dans la mesure où cet emplacement peut être modifié et varier parmi les utilisateurs. 

## <a name="asset-package-folding"></a>Mise en dossier des packages d'actifs

Comment pouvez-vous accéder aux fichiers de vos packages d'actifs? Vous pouvez continuer à utiliser ces API d'accès aux fichiers si vous accédez à tout autre fichier dans votre package d'architecture. En effet, les fichiers de package d'actifs seront placés en dossier dans votre package d'architecture lors de l'installation via le processus de mise en dossier du package. En outre, dans la mesure où les fichiers de package d'actifs doivent à l'origine constituer des fichiers placé dans vos packages d'architecture, vous n'êtes pas forcé de modifier l'utilisation de l'API lorsque vous passez du déploiement de fichiers libres au déploiement en package au cours du processus de développement. 

Pour mieux comprendre le fonctionnement de la mise en dossier des package, examinons un exemple. Si vous disposez d'un projet de jeu ayant la structure de fichier suivante:

```example
MyGame
|-- Audios
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Videos
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
```

Si vous souhaitez fractionner votre jeu en 3packages, un package d'architecture x64, un package d'actifs pour les audios et un package d'actifs pour les vidéos; votre jeu sera divisé en packages comme suit:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_1.0_Audios.appx
`-- Audios
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
MyGame_1.0_Videos.appx
`-- Videos
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
```

Lorsque vous installez votre jeu, le package x64 sera le premier à être déployé. Puis, les deux packages d'actifs sont toujours déployés dans leurs propres dossiers, à l'image de **MyGame_1.0_language-fr** dans notre exemple précédent. Toutefois, en raison de la mise en dossier des packages, les fichiers de package d'actifs seront également munis d'un lien physique afin d'apparaître dans le dossier **MyGame_1.0_x64** (de ce fait, bien que le fichiers apparaissent à deux emplacements, ils ne prennent pas deux fois plus d'espace disque). L'emplacement dans lequel les fichiers de package d'actifs apparaissent constitue exactement le même emplacement dans lequel ils sont relatifs à la racine du package. Voici donc l'aspect de la disposition finale du jeu déployé:

```example 
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   |-- Audios
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Videos
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Engine
|   |   `-- ...
|   |-- XboxLive
|   |   `-- ...
|   `-- Game.exe
|-- MyGame_1.0_Audios
|   `-- Audios
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
|-- MyGame_1.0_Videos
|   `-- Videos
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
`-- …(other apps)
```

Lors de la mise en dossier des packages d'actifs, vous pouvez toujours accéder aux fichiers que vous avez fractionnés en packages d'actifs de la même manière (notez que le dossier d'architecture dispose de la même structure que le dossier du projet d'origine), et vous pouvez ajouter des packages d'actifs ou déplacer des fichiers entre les packages d'actifs sans influer sur votre code. 

Examinons désormais un exemple plus compliqué de mise en dossier de package. Supposons que vous fractionnez plutôt vos fichiers en fonction du niveau. Si vous souhaitez conserver la même structure que le dossier de projet d'origine, vos packages doivent ressemble à ceci:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_Level1.appx
|-- Audios
|   `-- Level1
|       `-- ...
`-- Videos
    `-- Level1
        `-- ...

MyGame_Level2.appx
|-- Audios
|   `-- Level2
|       `-- ...
`-- Videos
    `-- Level2
        `-- ...
```
Cela permettra aux dossiers **Level1** et aux fichiers contenus dans le package **MyGame_Level1** ainsi qu'aux dossiers **Level2** et aux fichiers contenus dans le package **MyGame_Level2** d'être fusionnés dans les dossiers **Audios** and **Videos** au cours de la mise en dossier du package. Donc, en règle général, le chemin relatif désigné pour les fichiers en package dans le fichier de mappage ou la [disposition de mise en package](packaging-layout.md) de MakeAppx.exe est le chemin utilisé pour y accéder après la mise en dossier des packages. 

Enfin, si deuxfichiers de différents packages d'actifs présentent les mêmes chemins relatifs, une collision pourra se produire pendant la mise en dossier des packages. Si la collision se produit, le déploiement de votre application sera traduira par une erreur et l'échec. En outre, dans la mesure où la mise en dossier des packages tire partie des liens physiques, si vous utilisez les packages d'actifs, votre application ne pourra pas être déployées vers des disques non NTFS. Si vous savez que votre application sera probablement déplacées par vos utilisateurs vers des disques amovibles, vous ne devez pas utiliser les packages d'actifs. 


