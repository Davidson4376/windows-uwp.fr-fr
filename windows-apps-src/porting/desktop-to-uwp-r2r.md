---
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: Optimisez vos applications de bureau .NET avec les images natives
ms.date: 06/11/2018
ms.topic: article
keywords: Windows 10, l’image native du compilateur
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7970183"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimisez vos applications de bureau .NET avec les images natives

> [!NOTE]
> Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Vous pouvez améliorer le temps de démarrage de votre application .NET Framework en compilant au préalable vos fichiers binaires. Vous pouvez utiliser cette technologie sur les applications volumineuses qui vous empaquetez et distribuez par le biais du Microsoft Store. Dans certains cas, nous avons observé une amélioration des performances de 20 %. Pour en savoir plus sur cette technologie dans [vue d’ensemble technique](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Nous avons publié une version d’évaluation du compilateur image native sous forme de [package NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Vous pouvez appliquer ce package à n’importe quelle application .NET Framework qui cible la version 4.6.2 de .NET Framework ou une version ultérieure. Ce package ajoute une étape de génération post qui inclut une charge utile native à tous les fichiers binaires utilisés par votre application. Cette charge utile optimisée est chargée lorsque l’application s’exécute dans .NET 4.7.2 et versions supérieures tandis que les versions précédentes chargera toujours le code MSIL.

Le [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) est inclus dans [mise à jour Windows 10 avril 2018](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Vous pouvez également installer cette version de .NET Framework sur les PC exécutant Windows 7 + et Windows Server 2008 R2 +.

> [!IMPORTANT]
> Si vous souhaitez produire des images natives pour votre application empaquetée par le projet de package de l’Application Windows, veillez à définir la version Minimum de plateforme cible du projet pour la mise à jour anniversaire de Windows.

## <a name="how-to-produce-native-images"></a>Procédure pour produire des images natives

Suivez ces instructions pour configurer vos projets.

1. Configurer l’infrastructure cible en tant que version 4.6.2 ou supérieure

2. Configurer la plateforme cible en tant que x86 ou x64 

3. Ajoutez les packages NuGet.

4. Créer une version commerciale.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurer l’infrastructure cible en tant que version 4.6.2 ou supérieure

Pour configurer votre projet pour cibler .NET Framework 4.6.2 vous devez les outils de développement 4.6.2 de .NET Framework ou une version ultérieure. Ces outils sont disponibles via le programme d’installation de Visual Studio en tant que composants facultatifs sous la charge de travail de développement de bureau .NET:

![Installer la version 4.6.2 de .NET outils de développement](images/desktop-to-uwp/install-4.6.2-devpack.png)

Par ailleurs, vous pouvez obtenir les packs du développeur de .NET à partir de:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurer la plateforme cible en tant que x86 ou x64

Le compilateur image native permet d’optimiser le code pour une plateforme donnée. Pour l’utiliser, vous devez configurer votre application afin de cibler une plateforme spécifique comme x86 ou x64.

Si vous avez plusieurs projets dans votre solution, seulement le projet point d’entrée (probablement du projet qui génère un fichier exécutable) a besoin d’être compilée en tant que x86 ou x64. Fichiers binaires supplémentaires référencés dans le projet principal seront traités avec l’architecture spécifiée dans le projet principal, même si elles sont compilées en tant que AnyCPU.

Pour configurer votre projet:

1. Avec le bouton droit de votre solution et sélectionnez **Le Gestionnaire de Configuration**.

2. Sélectionnez **<New... >** dans le menu déroulant de **plateforme** en regard du nom du projet qui produit votre fichier exécutable.

3. Dans la boîte de dialogue **Nouveau projet de plateforme** , assurez-vous que la liste déroulante de **Copier les paramètres à partir** d’est définie sur **Toute UC**.

![Configurer x86](images/desktop-to-uwp/configure-x86.png)

Répétez cette étape pour `Release/x64` si vous souhaitez que générer x64 des fichiers binaires.

>[!IMPORTANT]
> Configuration AnyCPU n’est pas pris en charge par le compilateur d’image native.

## <a name="add-the-nuget-packages"></a>Ajoutez les packages NuGet

Le compilateur image native est fourni sous forme de package NuGet dont vous avez besoin d’ajouter au projet Visual Studio qui génère le fichier exécutable. Il s’agit généralement de votre projet Windows Forms ou WPF. Utilisez cette commande PowerShell pour ce faire.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Les packages de version d’évaluation sont publiées dans NuGet.org comme non répertorié. Vous ne les trouver en NuGet.org navigation ou en utilisant le Gestionnaire de Package UI dans Visual Studio. Toutefois, vous pouvez les installer à partir de la Console du Gestionnaire de Package et quand vous restauration à partir d’un autre ordinateur. Nous allons créer les packages entièrement accessible lorsque nous publions la première version de non-version d’évaluation.

## <a name="create-a-release-build"></a>Créer une version commerciale

Le package NuGet configure le projet pour exécuter un outil supplémentaire pour les versions release. Cet outil ajoute le code natif pour les mêmes fichiers binaires.
Pour vérifier que l’outil a traité les fichiers binaires, vous pouvez consulter la génération de sortie pour vous assurer qu’il inclut un message comme celle-ci:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Forum Aux Questions

**Q. Les nouveaux fichiers binaires fonctionnent sur les ordinateurs sans .NET Framework 4.7.2?**

A. Fichiers binaires optimisés bénéficieront d’améliorations lors de l’exécution avec .NET Framework 4.7.2. Les clients qui exécutent des versions antérieures de .NET framework, le code MSIL non optimisées seront chargés à partir du fichier binaire.

**Q. Comment puis-je fournir des commentaires ou signaler des problèmes?**

A. Signaler un problème à l’aide de l’outil commentaires dans Visual Studio 2017. [Plus d’informations](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. Quel est l’impact de l’ajout de l’image native pour les fichiers binaires existants?**

A. Les fichiers binaires optimisés contiennent le code managé et natif, rendre les fichiers finales une plus grande.

**Q. Puis-je publier des fichiers binaires à l’aide de cette technologie?**

A. Cette version inclut une licence de mise en service que vous pouvez utiliser dès aujourd'hui.
