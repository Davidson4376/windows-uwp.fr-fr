---
title: Compilez la source de l’API du XDK Xbox Live
description: Découvrez comment compiler la source de l’API de Xbox Live qui est livrée avec le Kit de développement Xbox (XDK).
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jeux, uwp, windows 10, xbox une, xdk
ms.localizationpriority: medium
ms.openlocfilehash: 9a98af637c8c60449cd2005c4fc6f83f9b0719cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660994"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>Compilez la source de l’API du Kit de développement Xbox (XDK) Xbox Live

Le Kit de développement Xbox (XDK) inclut la source pour la création de Microsoft.Xbox.Services.dll (XSAPI). Les développeurs peuvent suivre ces instructions pour mettre à jour leurs projets pour utiliser une build locale de la DLL.

Vous souhaiterez peut-être générer XSAPI vous-même si :
1. Si vous souhaitez déboguer un problème pour comprendre d'où provient un code d’erreur.
1. Si nous fournissons un correctif de code source pour résoudre un problème pour vous, avant que nous pouvons distribuer un correctif QFE.

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>Pour compiler le projet XDK C++ XSAPI par vous-même

<ol>
  <li> Obtenir la source Microsoft.Xbox.Services. Pour ce faire, extrayez tous les fichiers à partir de « %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox Services API\8.0\SourceDist\Xbox.Services.zip » dans un dossier accessible en écriture en dehors de « C:\Program Files (x86) », ou vous pouvez cloner la source à partir de <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Si votre projet fait référence à la DLL de pré-build, vous devez supprimer la référence</li>
    <ul>
      <li> Pour Visual Studio 2012 : Choisissez « Projet -> références... » dans Visual Studio. Si l’API de Services Xbox est répertorié comme une référence, sélectionnez-la et cliquez sur « Supprimer la référence ». Cliquez sur « OK » et enregistrez le fichier projet.</li>
      <li> Pour Visual Studio 2015 ou 2017 : Choisissez « projet -> ajouter des références... » in Visual Studio. Si l’API de Services Xbox est activée, désactivez-la. Cliquez sur « OK » et enregistrez le fichier projet.</li>
    </ul>
  <li> Si vous générez avec le XDK, choisissez « Fichier -> Ajouter -> projet existant... » dans Visual Studio pour ajouter les deux projets suivants à votre solution d’application. Les fichiers vcxproj seront situées dans le dossier que vous avez extrait la source à.</li>
Pour Visual Studio 2017 : <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141.Xbox.vcxproj</li>
    </ul>
Pour Visual Studio 2015 : <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140.Xbox.vcxproj</li>
    </ul>
Pour Visual Studio 2012 : <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
    <li> Ajouter les projets source en tant que référence en choisissant le projet -> références... et sélectionnez « Ajouter une référence ». Sous « Solution -> projets », vérifiez les entrées pour les deux projets ci-dessus, puis cliquez sur OK.</li>
    <li> Ajoutez le fichier de propriétés à votre projet en cliquant sur « Affichage -> autre Windows -> Gestionnaire de propriétés », avec le bouton droit sur votre projet, en sélectionnant « Ajouter de feuille de propriétés existante », puis enfin en sélectionnant le fichier xsapi.staticlib.props à la racine de sourch du Kit de développement logiciel.  Si votre système de génération ne prend pas en charge les fichiers de propriétés, vous devez ajouter manuellement les définitions de préprocesseur et les bibliothèques comme indiqué dans %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\ Xbox.Services.API.Cpp.props</li>
    <li> Ajouter le fichier services.h à votre source de l’application par le bouton droit sur le projet Ajouter -> élément existant et en choisissant le {SDK root}\Include\xsapi\services.h fichier source</li>
    <li> Assurez-vous que le « dossier » de sortie du projet d’application et le projet de Services Xbox sont les mêmes. Ce paramètre se trouve dans un projet Visual Studio Propriétés -> Propriétés de Configuration -> Général -> répertoire de sortie.</li>
    <li> Regénérez votre solution Visual Studio</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>Pour compiler le projet XDK WinRT XSAPI par vous-même

<ol>
  <li> Obtenir la source Microsoft.Xbox.Services. Pour ce faire, vous extrayez tous les fichiers à partir de « %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox Services API\8.0\SourceDist\Xbox.Services.zip » dans un dossier accessible en écriture en dehors de « C:\Program Files (x86) », ou vous pouvez cloner la source à partir de <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Si votre projet fait référence à la DLL de pré-build, vous devez supprimer la référence</li>
    <ul>
      <li> Pour Visual Studio 2012 : Choisissez « Projet -> références... » dans Visual Studio. Si l’API de Services Xbox est répertorié comme une référence, sélectionnez-la et cliquez sur « Supprimer la référence ». Cliquez sur « OK » et enregistrez le fichier projet.</li>
      <li> Pour Visual Studio 2015 ou 2017 : Choisissez « projet -> ajouter des références... » in Visual Studio. Si l’API de Services Xbox est activée, désactivez-la. Cliquez sur « OK » et enregistrez le fichier projet.</li>
    </ul>
  <li> Si vous générez avec le XDK, choisissez « Fichier -> Ajouter -> projet existant... » dans Visual Studio pour ajouter les deux projets suivants à votre solution d’application. Les fichiers vcxproj seront situées dans le dossier que vous avez extrait la source à.  Pour Visual Studio 2015, les projets met automatiquement à niveau vers le format de VS2015.</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
  <li> Dans Visual Studio, ajoutez les références :</li>
    <ul>
      <li> Pour Visual Studio 2012 : Choisissez « Projet -> références... » et sélectionnez « Ajouter une référence » dans Visual Studio. Dans la Solution -> projets, chceck les entrées pour les deux projets ci-dessus et cliquez sur OK.</li>
      <li> Pour Visual Studio 2015 ou 2017 : Choisissez « projet -> ajouter des références... » in Visual Studio. Sous projets, vérifiez les entrées pour les deux projets ci-dessus et cliquez sur OK.</li>
    </ul>
  <li> Assurez-vous que le « dossier » de sortie du projet d’application et le projet de Services Xbox sont les mêmes. Ce paramètre se trouve dans un projet Visual Studio Propriétés -> Propriétés de Configuration -> Général -> répertoire de sortie.</li>
  <li> Regénérez votre solution Visual Studio</li>
</ol>
