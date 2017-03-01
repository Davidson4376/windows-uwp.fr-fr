---
author: JordanEllis6809
title: "Intégration de jeux Unity dans UWP sur Xbox"
description: "Développement UWP Unity sur Xbox"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: fca3267a-0c0f-4872-8017-90384fb34215
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 6c01cc5afa3b3c1266464c9f04c79cea533c9741
ms.lasthandoff: 02/08/2017

---

# <a name="bringing-unity-games-to-uwp-on-xbox"></a>Intégration de jeux Unity dans UWP sur Xbox


Dans ce didacticiel détaillé, nous supposons que vous disposez déjà d’un jeu dans Unity, prêt à être généré et déployé.

Voir aussi une [version vidéo de ce didacticiel](https://www.youtube.com/watch?v=f0Ptvw7k-CE).

Vous cherchez à créer une version de votre projet UWP Unity ? Voir [Unity : Gestion de version de votre projet UWP](development-lanes-unity-versioning.md).

## <a name="step-0-ensure-unity-is-installed-correctly"></a>Étape 0 : s’assurer que Unity est correctement installé

Les composants suivants doivent être sélectionnés lors de l’installation d’Unity :

![Composants d’installation Unity](images/unity-install-components.png)

## <a name="step-1-building-the-uwp-solution"></a>Étape 1: générer la solution UWP

Dans votre projet de jeu Unity, ouvrez la fenêtre **Paramètres de génération**, qui se trouve dans **Fichier -&gt; Paramètres de build**, puis accédez au menu Options du Windows Store.

![Fenêtre Paramètres de build](images/build-settings.png)

Assurez-vous que le paramètre **SDK** est défini sur **Universel 10**. Cliquez ensuite sur le bouton **Générer** pour ouvrir une fenêtre Explorateur de fichiers qui vous demande un dossier de destination. Créez un dossier nommé **UWP** dans le répertoire **Assets** de votre projet et choisissez-le comme dossier de destination de la build.

![Dossier de destination de la build](images/build-destination.png)

Unity a ainsi créé une solution Visual Studio que nous utiliserons pour déployer votre jeu UWP.

![Solution Visual Studio UWP](images/uwp-vs-solution.png)

## <a name="step-2-deploying-your-game"></a>Étape 2: déployer votre jeu

Ouvrez la solution qui vient d’être créée dans le dossier **UWP**, puis modifiez la plateforme cible en **x64**.

![Plateforme de génération x64](images/x64-build-platform.png)

À présent que vous disposez d’une solution Visual Studio UWP pour votre jeu, [suivez ces étapes](getting-started.md) pour déployer correctement votre jeu vers votre Xbox One achetée au détail !

## <a name="step-3-modify-and-rebuild"></a>Étape 3 : modifier et régénérer

Si des modifications sont apportées à des éléments ne figurant pas dans le script, pour que ces modifications s’affichent dans la build UWP de votre jeu, le projet doit être régénéré à partir de l’éditeur (comme décrit à l’__Étape 1__).

## <a name="versioning-your-uwp-project"></a>Gestion de version de votre projet UWP

Il existe quelques situations courantes où l’ajout de parties de ce répertoire UWP nouvellement créé à la gestion de version devient nécessaire. Par exemple, si vous ajoutez une nouvelle dépendance au projet UWP (par exemple, le Kit SDK Xbox Live).  Cet exemple est étudié en détail dans [Unity : Gestion de version de votre projet UWP](development-lanes-unity-versioning.md).

## <a name="see-also"></a>Voir également
- [Intégration de jeux existants dans Xbox](development-lanes-landing.md)
- [UWP sur Xbox One](index.md)

