---
author: TylerMSFT
title: Ajouter un écran de démarrage
description: Définissez l’image et la couleur d’arrière-plan de l’écran de démarrage de votre application à l’aide de Microsoft Visual Studio.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.author: twhitney
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1b6cff7ce434e219b0d502f384d202c3b708479a
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
ms.locfileid: "1861888"
---
# <a name="add-a-splash-screen"></a>Ajouter un écran de démarrage

Définissez l’image et la couleur d’arrière-plan de l’écran de démarrage de votre application à l’aide de Microsoft Visual Studio.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Définir l’image et la couleur d’arrière-plan de l’écran de démarrage dans Visual Studio

Quand vous utilisez un modèle Visual Studio pour créer votre application, une image par défaut est ajoutée à votre projet et définie en tant qu’image de l’écran de démarrage. Par défaut, la couleur d’arrière-plan de votre écran de démarrage est gris clair. Pour modifier l’image ou la couleur par défaut de l’écran de démarrage de votre application, procédez comme suit :

1. Ouvrez votre projet existant d’application de plateforme Windows universelle (UWP) dans Visual Studio.
2. Dans l’**Explorateur de solutions**, ouvrez le fichier Package.appxmanifest. Vous pouvez également ouvrir ce fichier à partir de la barre de menus en choisissant **Projet** &gt; **Windows Store** &gt; **Modifier le manifeste d’application**.
3. Ouvrez l’onglet **Ressources visuelles** et sélectionnez **Écran de démarrage** dans le volet **Toutes les ressources visuelles** du côté gauche de la fenêtre Package.appxmanifest. Si vous changez votre écran de démarrage pour la première fois, le chemin d’accès Assets\SplashScreen.png s’affiche dans le champ **Écran de démarrage**.

    La capture d’écran suivante montre la fenêtre Package.appxmanifest dans Visual Studio. Selon le type de projet, l’ensemble des ressources visuelles peut différer légèrement.

    ![une capture d’écran de la fenêtre Package.appxmanifest dans Visual Studio2017.](images/appmanifest.png)

    Si vous ouvrez le fichier Package.appxmanifest dans un éditeur de texte, l’élément [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467) s’affiche en tant qu’enfant de l’élément [**VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471). Le balisage de l’écran de démarrage par défaut dans le fichier manifeste ressemble à ceci dans un éditeur de texte:

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. Pour sélectionner une nouvelle image d’écran de démarrage pour une application UWP, appuyez sur le bouton comportant des points de suspension, qui s’affiche à côté de l’étiquette **1240x600px** sous **Composants mis à l’échelle**. Choisissez l’image de 1240x600pixels (.png, .jpg ou .jpeg) que vous voulez utiliser comme image d’écran de démarrage.

    **Important**  Vous devez choisir une image de 620x300pixels avec un facteur d’échelle1x. Lorsque vous concevez votre écran de démarrage, notez également qu’il est inférieur à l’écran et centré. Il ne remplit pas l’écran comme l’écran de démarrage d’une application du Windows Phone Store.

5. Pour sélectionner une nouvelle image d’écran de démarrage pour une application du Windows Phone Store, appuyez sur le bouton comportant des points de suspension, qui s’affiche à côté de l’étiquette **1152x1920px** sous **Composants mis à l’échelle**. Choisissez l’image de 1152 x 1920pixels (.png, .jpg ou .jpeg) que vous voulez utiliser comme image d’écran de démarrage.

    **Important**  L’image d’écran de démarrage que vous choisissez doit être de 1152x 1920pixels, qui est la taille correcte pour le facteur d’échelle2,4x. Si c’est la seule ressource que vous fournissez, elle sera réduite pour les facteurs d’échelle de 1,4x et 1x.

6. Dans le champ **Couleur d’arrière-plan** de la section **Écran de démarrage**, définissez la couleur d’arrière-plan affichée avec l’image d’écran de démarrage. Vous pouvez entrer le nom d’une couleur ou ’\#’ et la valeur hexadécimale d’une couleur. Pour obtenir la liste des noms de couleurs disponibles, voir [**Élément SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467). La définition d’une couleur d’arrière-plan d’écran de démarrage est facultative. Si vous ne spécifiez pas de couleur pour une application UWP, la couleur d’arrière-plan de l’écran de démarrage est gris clair par défaut (valeur hexadécimale \#464646). Il s’agit de la même couleur que la couleur d’arrière-plan par défaut de la **vignette** (voir le champ **Couleur d’arrière-plan** de la section **Mosaïque et logos** de l’onglet **Ressources visuelles**). Si vous ne spécifiez pas de couleur pour un Windows Phone ou si vous la définissez sur « transparent », la couleur d’arrière-plan de l’écran de démarrage sera transparente.

## <a name="summary-and-next-steps"></a>Récapitulatif et étapes suivantes

Si votre application met un certain temps à se charger, ajoutez un écran de démarrage étendu. Pour obtenir des indications détaillées, voir [Créer un écran de démarrage personnalisé](create-a-customized-splash-screen.md).

## <a name="related-topics"></a>Rubriquesconnexes

* [Créer un écran de démarrage personnalisé](create-a-customized-splash-screen.md)
* [Référence du schéma de manifeste de package: élément SplashScreen](https://msdn.microsoft.com/library/windows/apps/br211467)
* [Classe Windows.ApplicationModel.Activation.SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763)