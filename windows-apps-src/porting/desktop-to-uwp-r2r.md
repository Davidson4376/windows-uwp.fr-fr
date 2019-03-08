---
Description: Ce guide explique comment configurer votre Solution Visual Studio pour optimiser les fichiers binaires d’application avec les images natives.
Search.Product: eADQiWindows 10XVcnh
title: Optimisez vos applications de bureau .NET avec les images natives
ms.date: 06/11/2018
ms.topic: article
keywords: Windows 10, les images natives du compilateur
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642874"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimisez vos applications de bureau .NET avec les images natives

> [!NOTE]
> Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Vous pouvez améliorer le temps de démarrage de votre application .NET Framework en les compilant vos fichiers binaires. Vous pouvez utiliser cette technologie sur les applications volumineuses qui vous conditionnez et distribuez via le Microsoft Store. Dans certains cas, nous avons constaté une amélioration des performances de 20 %. Plus d’informations sur cette technologie dans le [vue d’ensemble technique](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Nous avons publié une version d’évaluation du compilateur images natives comme un [package NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Vous pouvez appliquer ce package à n’importe quelle application .NET Framework qui cible le .NET Framework version 4.6.2 ou version ultérieure. Ce package ajoute une étape de génération de billet qui inclut une charge utile native pour tous les fichiers binaires utilisés par votre application. Cette charge utile optimisée soit chargée lorsque l’application s’exécute dans .NET 4.7.2 et versions ultérieures tandis que les versions précédentes seront chargera le code MSIL.

Le [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) est inclus dans le [mettre à jour du 10 avril 2018 Windows](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Vous pouvez également installer cette version du .NET Framework sur les PC qui exécutent Windows 7 + et Windows Server 2008 R2 +.

> [!IMPORTANT]
> Si vous souhaitez produire des images natives pour votre application packagée par le projet d’empaquetage d’Application Windows, veillez à définir la version minimale de plateforme cible du projet pour la mise à jour anniversaire de Windows.

## <a name="how-to-produce-native-images"></a>Comment produire des images natives

Suivez ces instructions pour configurer vos projets.

1. Configurer le framework cible en tant que 4.6.2 ou ultérieure

2. Configurer la plateforme cible en tant que x86 ou x64 

3. Ajoutez les packages NuGet.

4. Créer une version Release.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurer le framework cible en tant que 4.6.2 ou ultérieure

Pour configurer votre projet doit cibler .NET Framework 4.6.2, vous devrez les outils de développement .NET Framework 4.6.2 ou version ultérieure. Ces outils sont disponibles via le programme d’installation de Visual Studio en tant que composants facultatifs sous la charge de travail de développement bureautique .NET :

![Installer .NET 4.6.2 outils de développement](images/desktop-to-uwp/install-4.6.2-devpack.png)

Vous pouvez également obtenir les packs de développeurs .NET à partir de : [https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurer la plateforme cible en tant que x86 ou x64

Le compilateur d’images natives optimise le code pour une plateforme donnée. Pour l’utiliser, vous devez configurer votre application pour cibler une plateforme spécifique tel que x86 ou x64.

Si vous avez plusieurs projets dans votre solution, uniquement le projet point d’entrée (très probablement le projet qui génère un fichier exécutable) doit être compilé en tant que x86 ou x64. Les fichiers binaires supplémentaires référencés à partir du projet principal seront traitées avec l’architecture spécifiée dans le projet principal, même si elles sont compilées sous la forme AnyCPU.

Pour configurer votre projet :

1. Avec le bouton droit de votre solution, puis sélectionnez **Configuration Manager**.

2. Sélectionnez **< nouveau... >** dans le **plateforme** menu déroulant en regard du nom du projet qui génère votre fichier exécutable.

3. Dans le **nouvelle plateforme de projet** boîte de dialogue zone, assurez-vous que le **copier les paramètres à partir de** liste déroulante est définie sur **Any CPU**.

![Configurer x86](images/desktop-to-uwp/configure-x86.png)

Répétez cette étape pour `Release/x64` si vous souhaitez produire x64 les fichiers binaires.

>[!IMPORTANT]
> Configuration de AnyCPU n’est pas prise en charge par le compilateur d’images natives.

## <a name="add-the-nuget-packages"></a>Ajoutez les packages NuGet

Le compilateur d’image native est fourni sous forme de package NuGet dont vous avez besoin pour ajouter au projet Visual Studio qui génère le fichier exécutable. Il s’agit généralement de votre projet Windows Forms ou WPF. Pour ce faire, utilisez cette commande PowerShell.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Les packages de préversion sont publiés dans NuGet.org comme non répertorié. Vous ne les trouverez par navigation NuGet.org ou à l’aide de l’UI du Gestionnaire de Package dans Visual Studio. Toutefois, vous pouvez les installer à partir de la Console du Gestionnaire de Package et lorsque vous la restauration à partir d’un autre ordinateur. Nous allons rendre les packages entièrement accessible lorsque nous publions la première version non-preview.

## <a name="create-a-release-build"></a>Créer une version Release

Le package NuGet configure le projet pour exécuter un outil supplémentaire pour les versions release. Cet outil ajoute le code natif pour les mêmes fichiers binaires.
Pour vérifier que l’outil a traité les fichiers binaires, vous pouvez consulter la sortie de génération dans Vérifiez qu’il inclut un message tel que celui-ci :

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Forum Aux Questions

**Q. Les nouveaux fichiers binaires fonctionnent sur des machines sans .NET Framework 4.7.2 ?**

A. Fichiers binaires optimisés bénéficieront des améliorations lors de l’exécution avec .NET Framework 4.7.2. Les clients qui exécutent des versions précédentes du .NET framework chargera le code MSIL non optimisé du fichier binaire.

**Q. Comment puis-je fournir des commentaires ou signaler des problèmes ?**

A. Signaler un problème lié à l’aide de l’outil de commentaires dans Visual Studio 2017. [Plus d’informations](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. Quel est l’impact de l’ajout de l’image native pour les fichiers binaires existants ?**

A. Les fichiers binaires optimisés contiennent le code managé et natif, rendre les fichiers finaux supérieure.

**Q. Puis-je publier des fichiers binaires à l’aide de cette technologie ?**

A. Cette version inclut une licence Go Live que vous pouvez utiliser dès aujourd'hui.
