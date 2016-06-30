---
title: "Migrer votre jeu Unity vers Xbox One"
author: JordanEllis6809
ms.sourcegitcommit: 008ff2566b17a05b52dee0a8cd6c070d841b1f62
ms.openlocfilehash: cc854bc707a9c08687d3c6d92a704f5099d52d5b

---

# Migrer votre jeu Unity vers Xbox One

Dans ce didacticiel détaillé, nous supposons que vous disposez déjà d’un jeu dans Unity, prêt à être créé et déployé.

[Version vidéo de ce didacticiel.](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

## Étape 0 : s’assurer de l’installation correcte d’Unity.

Les composants suivants doivent être sélectionnés lors de l’installation d’Unity :

![Composants d’installation Unity](images/unity-install-components.png)

## Étape 1: générer la solution UWP

Dans votre projet de jeu Unity, ouvrez la fenêtre Paramètres de génération, qui se trouve dans `File -> Build Settings...`, puis accédez au menu Options du Windows Store présenté ci-dessous.

![Fenêtre Paramètres de génération](images/build-settings.png)

Assurez-vous que le paramètre `SDK` est défini sur `Universal 10`. Appuyez sur le bouton Générer en bas du menu ; une fenêtre Explorateur apparaît et vous demande de spécifier un dossier de destination. Créez un dossier nommé `UWP` dans le répertoire `Assets` de votre projet et choisissez-le comme dossier de destination pour le build.

![Dossier de destination du build](images/build-destination.png)

Unity a ainsi créé une solution Visual Studio que nous utiliserons pour déployer votre jeu UWP à l’étape suivante.

![Solution Visual Studio UWP](images/uwp-vs-solution.png)

## Étape 2: déployez votre jeu

Ouvrez la solution qui vient d’être créée dans le dossier `Assets/UWP`  Une fois ouverte, modifiez la plateforme cible en X64.

![Plateforme de génération x64](images/x64-build-platform.png)

À présent que vous disposez d’une solution Visual Studio UWP pour votre jeu, [suivez ces étapes](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started) pour déployer correctement votre jeu vers votre Xbox One achetée au détail !

## Remarques à l’attention des développeurs

- Il est recommandé d’ignorer le dossier UWP dans votre contrôle de version. Si vous cherchez à ajouter des éléments XAML supplémentaires à votre projet et devez versionner certaines ressources au sein du dossier UWP, voir [ces exemples illustratifs](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

- Si, dans Unity, vous apportez des modifications à n’importe quel contenu inclus dans la build de votre jeu (à l’exception des scripts), vous devez régénérer votre solution UWP afin de faire apparaître ces modifications lors du déploiement suivant. Cela se produit au cours de l’étape de génération d’Unity, durant laquelle l’ensemble des ressources de votre projet sont compilées en un seul fichier de ressources. Lorsqu’elle déploie le jeu, la solution UWP fait référence à ce fichier de ressources généré.




<!--HONumber=Jun16_HO4-->


