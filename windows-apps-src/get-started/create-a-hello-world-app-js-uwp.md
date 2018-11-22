---
author: GrantMeStrength
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: Créer une application «Hello World» (JS)
description: Ce didacticiel vous expliquer comment utiliser JavaScript et HTML pour créer un simple & le \#0034; Hello, world & \#0034; application qui cible la plateforme Windows universelle (UWP) sur Windows 10.
ms.author: jken
ms.date: 03/06/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4d8fb1dc486c039007c3ea0d4ee36d72c0c511f9
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7575997"
---
# <a name="create-a-hello-world-app-js"></a>Créer une application « Hello World » (JS)

Ce didacticiel vous expliquer comment utiliser JavaScript et HTML pour créer une simple «Hello, world» qui cible la plateforme Windows universelle (UWP) sur Windows 10. Avec un seul projet dans Microsoft Visual Studio, vous pouvez créer une application qui s’exécute sur n’importe quel appareil Windows 10.

> [!NOTE]
> Ce didacticiel utilise Visual Studio Community2017. Si vous utilisez une autre version de Visual Studio, son aspect vous semblera peut-être légèrement différent.


Vous allez apprendre à effectuer les opérations suivantes:

-   Créez un nouveau projet **Visual Studio 2017** qui cible **Windows 10** et **UWP**.
-   ajouter du contenu HTML et JavaScript;
-   exécuter le projet sur l’ordinateur local dans Visual Studio.

## <a name="before-you-start"></a>Avant de commencer...

-   [Qu’est-ce qu’une applicationUWP?](universal-application-platform-guide.md).
-   Pour suivre ce didacticiel, vous avez besoin de Windows 10 et Studio2017 visuelle. [Préparation](get-set-up.md).
-   Nous partons également du principe que vous utilisez la disposition de fenêtre par défaut de Visual Studio. Si vous modifiez la disposition par défaut, vous pouvez la réinitialiser dans le menu **Fenêtre** en choisissant la commande **Rétablir la disposition de fenêtre**.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Étape1: Créer un projet dans Visual Studio

1.  Lancez Visual Studio2017.

2.  À partir du menu **Fichier**, sélectionnez **Nouveau > Projet...** pour ouvrir la boîte de dialogue *Nouveau projet*.

3.  Dans la liste de modèles de gauche, ouvrez **Installés > Modèles > JavaScript**, puis choisissez **Windows universel** pour afficher la liste des modèles de projet UWP.

    (Si aucun modèle universel n’apparaît, c’est qu’il vous manque les composants permettant de créer des applications UWP. Vous pouvez répéter la procédure d’installation et ajouter la prise en charge UWP en cliquant sur **Ouvrir le programme d'installation de Visual Studio** dans la boîte de dialogue *Nouveau projet*. Voir [Préparation](get-set-up.md)

4.  Choisissez le modèle **Application vide (Windows universel)**, puis entrez «HelloWorld» comme **Nom**. Sélectionnez **OK**.

    ![Fenêtre Nouveau projet](images/win10-js-01.png)

> [!NOTE]
> Si vous utilisez Visual Studio pour la première fois, il est possible que la boîte de dialogue Paramètres s'affiche et vous demande d’activer le **Mode développeur**. Le mode développeur est un paramètre qui permet d'accéder à certaines fonctionnalités, telles que l’autorisation d’exécuter des applications directement plutôt qu’uniquement à partir du Windows Store. Pour plus d’informations, consultez [Activer votre appareil pour le développement](enable-your-device-for-development.md). Pour continuer avec ce guide, sélectionnez le **Mode développeur**, cliquez sur **Oui** et fermez la boîte de dialogue.

 ![Boîte de dialogue d'activation du mode développeur](images/win10-cs-00.png)

5.  La boîte de dialogue Version cible/Version minimale s’affiche. Les paramètres par défaut étant appropriés pour ce didacticiel, sélectionnez **OK** pour créer le projet.

    ![Fenêtre Explorateur de solutions](images/win10-cs-02.png)

6.  Votre nouveau projet s’ouvre en affichant ses fichiers dans le volet **Explorateur de solutions**, à droite. Vous devrez peut-être choisir l’onglet **Explorateur de solutions** à la place de l’onglet **Propriétés** pour voir vos fichiers.

    ![Fenêtre Explorateur de solutions](images/win10-js-02.png)

Même si le modèle **Application vide (Windows universel)** est dépouillé, il contient cependant de nombreux fichiers. Ces fichiers sont indispensables pour toutes les applications UWP en JavaScript. Ils se trouvent dans tous les projets que vous créez dans Visual Studio.


### <a name="whats-in-the-files"></a>Que contiennent les fichiers?

Pour afficher et modifier un fichier de votre projet, double-cliquez dessus dans l’**Explorateur de solutions**. 

*default.css*

-  Feuille de style par défaut utilisée par l’application.

*main.js*

- Fichier JavaScript par défaut. Il est référencé dans le fichier index.html.

*index.html*

- Page de l’application web qui se charge et s'affiche lors du lancement de l’application.

*Ensemble d’images de logo*
-   Assets/Square150x150Logo.scale-200.png représente votre application dans le menu Démarrer.
-   Assets/StoreLogo.png représente votre application dans le MicrosoftStore.
-   Assets/SplashScreen.scale-200.png est l’écran de démarrage qui s’affiche quand votre application démarre.

## <a name="step-2-adding-a-button"></a>Étape2: Ajouter un bouton

Cliquez sur *index.html* pour le sélectionner dans l’éditeur et remplacez le code HTML qu’il contient par:

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>Hello World</title>
    <script src="js/main.js"></script>
    <link href="css/default.css" rel="stylesheet" />
</head>

<body>
    <p>Click the button..</p>
    <button id="button">Hello world!</button>
</body>

</html>
```

Il doit se présenter comme suit:

 ![Code HTML du projet](images/win10-js-03.png)

Ce code HTML fait référence au fichier *main.js* qui contiendra notre code JavaScript. Il ajoute une seule ligne de texte et un bouton unique dans le corps de la page web. Un *ID* est attribué au bouton afin que le code JavaScript puissent le référencer.


## <a name="step-3-adding-some-javascript"></a>Étape3: Ajout du code JavaScript

Nous allons maintenant ajouter le code JavaScript. Cliquez sur *main.js* pour le sélectionner et ajoutez les éléments suivants:

```javascript
// Your code here!

window.onload = function () {
    document.getElementById("button").onclick = function (evt) {
        sayHello()
    }
}


function sayHello() {
    var messageDialog = new Windows.UI.Popups.MessageDialog("Hello, world!", "Alert");
    messageDialog.showAsync();
}

```

Il doit se présenter comme suit:

 ![Code JavaScript du projet](images/win10-js-04.png)

Ce code JavaScript déclare deux fonctions. La fonction *window.onload* est appelée automatiquement lorsque le fichier *index.html* s’affiche. Il trouve le bouton (à l’aide de l’ID que nous avons déclaré) et ajoute un gestionnaire onclick: la méthode qui sera appelée lorsque l’utilisateur cliquera sur le bouton.

La deuxième fonction, *sayHello()*, crée et affiche une boîte de dialogue. Elle est très similaire à la fonction *Alert()* que vous connaissez peut-être d'un précédent développement JavaScript.


## <a name="step-4-run-the-app"></a>Étape4: Exécuter l'application

Vous pouvez maintenant exécuter l’application en appuyant sur F5. L’application se charge, la page web s’affiche. Cliquez sur le bouton et la boîte de dialogue s’ouvre.

 ![Exécution du projet](images/win10-js-05.png)



## <a name="summary"></a>Résumé


Félicitations, vous avez créé une application JavaScript pour Windows 10 et UWP. Cet exemple est ridiculement simple, mais vous pouvez désormais commencer à ajouter vos bibliothèques et infrastructures JavaScript préférées pour créer votre propre application. Comme il s'agit d'une application UWP, vous pouvez la publier dans le Windows Store. Pour obtenir un exemple d’ajout d’infrastructures tierces, consultez les projets suivants:

* [Jeu UWP2D simple pour le Microsoft Store, écrit en JavaScript et CreateJS](get-started-tutorial-game-js2d.md)
* [Jeu UWP3D pour le Microsoft Store, écrit en JavaScript et threeJS](get-started-tutorial-game-js3d.md)


