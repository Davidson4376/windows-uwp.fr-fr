---
title&#58; Unity&#58; Migrer votre jeu pour XboxOne - Auteur&#58; JordanEllis6809 
---

# Unity&#58; Migrer votre jeu vers XboxOne

Dans ce didacticiel détaillé, nous supposons que vous disposez déjà d’un jeu dans Unity, prêt à être créé et déployé.

[Version vidéo de ce didacticiel.](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

Vous cherchez à créer une version de votre projet UWP Unity?  
            [Voir ici](development-lanes-unity-versioning.md).

## Étape0: S’assurer de l’installation correcte d’Unity.

Les composants suivants doivent être sélectionnés lors de l’installation d’Unity:

![Composants d’installation Unity](images/unity-install-components.png)

## Étape1: générer la solution UWP

Dans votre projet de jeu Unity, ouvrez la fenêtre Paramètres de génération, qui se trouve dans `File -> Build Settings...`, puis accédez au menu Options du Windows Store présenté ci-dessous.

![Fenêtre Paramètres de génération](images/build-settings.png)

Assurez-vous que le paramètre `SDK` est défini sur `Universal 10`. Appuyez sur le bouton Générer en bas du menu; une fenêtre Explorateur apparaît et vous demande de spécifier un dossier de destination. Créez un dossier nommé `UWP` dans le répertoire `Assets` de votre projet et choisissez-le comme dossier de destination pour le build.

![Dossier de destination du build](images/build-destination.png)

Unity a ainsi créé une solution VisualStudio que nous utiliserons pour déployer votre jeu UWP à l’étape suivante.

![Solution Visual Studio UWP](images/uwp-vs-solution.png)

## Étape2: déployez votre jeu

Ouvrez la solution qui vient d’être créée dans le dossier `Assets/UWP`  Une fois ouverte, modifiez la plateforme cible en X64.

![Plateforme de génération x64](images/x64-build-platform.png)

À présent que vous disposez d’une solution VisualStudioUWP pour votre jeu, [suivez ces étapes](https://msdn.microsoft.com/windows/uwp/xbox-apps/getting-started) pour déployer correctement votre jeu vers votre XboxOne achetée au détail!

## Étape 3: Modifier et régénérer

Si des modifications sont apportées à des éléments ne figurant pas dans le script, pour que ces modifications s’affichent dans la buildUWP de votre jeu, le projet doit être régénéré à partir de l’éditeur (comme décrit à l’__Étape1__).

## Contrôle de version de votre projetUWP

Il existe quelques situations courantes où l’ajout de parties à ce répertoire UWP nouvellement créé pour le contrôle de version devient nécessaire.  Par exemple, si vous ajoutez une nouvelle dépendance au projet UWP (p.ex., le Kit de développement logiciel Xbox Live).  
            [Ici](development-lanes-unity-versioning.md), nous passons en revue les détails de cet exemple.



<!--HONumber=Jul16_HO2-->


