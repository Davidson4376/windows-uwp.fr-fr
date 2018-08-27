---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: Optimiser vos applications de bureau .NET avec les images natives
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, image native du compilateur
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "2855273"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Optimiser vos applications de bureau .NET avec les images natives

> [!NOTE]
> Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Vous pouvez améliorer le temps de démarrage de votre application .NET Framework par la compilation préalable vos fichiers binaires. Vous pouvez utiliser cette technologie dans les applications de grande taille qui vous empaquetez et distribuez par le biais du Windows Store. Dans certains cas, nous avons observé une amélioration des performances de 20 %. Pour plus d’informations sur cette technologie dans [présentation technique](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Nous avons publié une version d’évaluation du compilateur native image en tant que [package NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Vous pouvez appliquer ce package à n’importe quelle application .NET Framework qui cible .NET Framework version 4.6.2 ou version ultérieure. Ce package ajoute une étape de génération post qui inclut une charge native à tous les fichiers binaires utilisés par votre application. Cette charge utile optimisée sera chargée lors de l’application s’exécute dans .NET 4.7.2 et versions ultérieures alors que les versions précédentes chargera toujours le code MSIL.

Le [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) est inclus dans [mise à jour Windows 10 avril 2018](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Vous pouvez également installer cette version du .NET Framework sur PC qui exécutent Windows 7 et Windows Server 2008 R2 +.

> [!IMPORTANT]
> Si vous souhaitez générer des images natives pour votre application empaquetées par le projet d’empaquetage d’Application Windows, veillez à définir la version minimale de plateforme cible du projet à la mise à jour anniversaire de Windows.

## <a name="how-to-produce-native-images"></a>Comment générer des images natives

Suivez les instructions ci-dessous pour configurer vos projets.

1. Configurer l’infrastructure cible en tant que 4.6.2 ou au-dessus

2. Configurer la plateforme cible en tant que x86 ou x64 

3. Ajouter les packages NuGet.

4. Créer une version Release.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurer l’infrastructure cible en tant que 4.6.2 ou au-dessus

Pour configurer votre projet pour cibler .NET Framework 4.6.2, vous devrez les outils de développement .NET Framework 4.6.2 ou plus récente. Ces outils sont disponibles via le programme d’installation de Visual Studio en tant que composants facultatifs sous la charge de travail de développement de bureau de .NET:

![Installer .NET 4.6.2 outils de développement](images/desktop-to-uwp/install-4.6.2-devpack.png)

Sinon, vous pouvez obtenir les packs de développeur .NET à partir de:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurer la plateforme cible en tant que x86 ou x64

Le compilateur image native optimise le code pour une plateforme donnée. Pour l’utiliser, vous devez configurer votre application pour cibler une plate-forme spécifique tels que x86 ou x64.

Si vous disposez de plusieurs projets dans votre solution, uniquement le projet point d’entrée (probablement le projet qui produit un fichier exécutable) doit être compilé en tant que x86 ou x64. Référencé à partir du projet principal des fichiers binaires supplémentaires seront traitées avec l’architecture spécifiée dans le projet principal, même si elles sont compilés en tant que AnyCPU.

Pour configurer votre projet:

1. Avec le bouton droit de votre solution, puis sélectionnez **Le Gestionnaire de Configuration**.

2. Sélectionnez **< nouveau... >** dans le menu déroulant de **plateforme** en regard du nom du projet qui génère le fichier exécutable.

3. Dans la boîte de dialogue **Nouvelle plateforme de projet** , assurez-vous que la liste déroulante **Copier les paramètres depuis** est définie sur **Tout processeur**.

![Configurer x86](images/desktop-to-uwp/configure-x86.png)

Répétez cette étape pour `Release/x64` si vous souhaitez générer x64 binaires.

>[!IMPORTANT]
> Configuration AnyCPU n’est pas pris en charge par le compilateur image native.

## <a name="add-the-nuget-packages"></a>Ajouter les packages NuGet

Le compilateur image native est fourni sous forme de package NuGet dont vous avez besoin pour ajouter au projet Visual Studio génère le fichier exécutable. Il s’agit généralement de votre projet WPF ou Windows Forms. Pour ce faire, utilisez cette commande PowerShell.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Les packages preview sont publiés dans NuGet.org comme non répertoriés. Vous ne trouvez les par NuGet.org navigation ou à l’aide du Gestionnaire de Package UI dans Visual Studio. Toutefois, vous pouvez les installer à partir de la Console du Gestionnaire de Package et lorsque vous la restauration à partir d’un autre ordinateur. Nous apporterons des packages accessible lorsque nous publiez la première version sans aperçu.

## <a name="create-a-release-build"></a>Créer une version Release

Le package NuGet configure le projet pour exécuter un outil supplémentaire pour les versions release. Cet outil ajoute le code natif pour les mêmes fichiers binaires.
Pour vérifier que l’outil a traité les fichiers binaires, vous pouvez consulter la version de sortie pour vous assurer qu’il inclut un message tel que celui-ci:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>FAQ

**Q. Les nouveaux fichiers binaires fonctionnent sur des ordinateurs sans .NET Framework 4.7.2?**

A. Fichiers binaires optimisés bénéficieront les améliorations lors de l’exécution avec .NET Framework 4.7.2. Les clients qui exécutent des versions antérieures de .NET framework charge le code MSIL non optimisé à partir du fichier binaire.

**Q. Comment puis-je envoyer des commentaires ou signaler des problèmes?**

A. Signaler un problème à l’aide de l’outil de commentaires dans Visual Studio 2017. [Plus d’informations](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. Quel est l’impact de l’ajout de l’image native à binaires existants?**

A. Les fichiers binaires optimisées contiennent le code managé et natif, rendant les fichiers finaux supérieure.

**Q. Puis-je publier des fichiers binaires à l’aide de cette technologie?**

A. Cette version inclut une licence Go Live que vous pouvez utiliser aujourd'hui.
