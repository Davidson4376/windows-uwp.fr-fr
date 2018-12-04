---
title: Créer un jeu UWP en JavaScript
description: Un jeu UWP simple pour le Microsoft Store, écrit en JavaScript et CreateJS
ms.date: 02/09/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.localizationpriority: medium
ms.openlocfilehash: ae8daa6141eadaac699fc49b8ec4796f1dde5c91
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476474"
---
# <a name="create-a-uwp-game-in-javascript"></a>Créer un jeu UWP en JavaScript

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-javascript-and-createjs"></a>Jeu UWP2D simple pour le Microsoft Store, écrit en JavaScript et CreateJS


![Feuille de sprite Dino en marche](images/JS2D_1.png)


## <a name="introduction"></a>Introduction


La publication d’une application dans le Microsoft Store signifie que vous pouvez partager (ou vendre!) avec des millions de personnes, sur différents appareils.  

Pour publier votre application dans le Microsoft Store, il doit être écrit en tant qu’une application UWP (plateforme Windows universelle). Or, UWP est extrêmement souple et prend en charge un grand nombre de langues et d'infrastructures. Pour le prouver, cet exemple de jeu simple écrit en JavaScript tire parti de plusieurs bibliothèques CreateJS et montre comment dessiner des sprites, créer une boucle de jeu, prendre en charge le clavier et la souris et s’adapter à différentes tailles d’écran.

Ce projet est généré avec JavaScript à l’aide de Visual Studio. Avec quelques changements mineurs, il peut également être hébergé sur un site Web ou adapté à d’autres plateformes. 

**Remarque:** Ce n’est pas un jeu complet (ni excellent); Il est conçu pour montrer comment utiliser JavaScript et une troisième bibliothèque tierce pour rendre une application prête à être publiée dans le Microsoft Store.


## <a name="requirements"></a>Spécifications

Pour jouer avec ce projet, vous aurez besoin des éléments suivants:

* Un ordinateur Windows (physique ou virtuel) exécutant la version actuelle de Windows10.
* Visual Studio. Visual Studio Community Edition peut être téléchargé gratuitement à partir de la [page d’accueil de Visual Studio](http://visualstudio.com).

Ce projet utilise l'infrastructure JavaScript CreateJS. CreateJS est un ensemble d’outils gratuit, publié sous une licence MIT, conçu pour faciliter la création de jeux sprite. Les bibliothèques CreateJS sont déjà présentes dans le projet (recherchez *js/easeljs-0.8.2.min.js* et *js/preloadjs-0.6.2.min.js* dans la vue Explorateur de solutions). Vous trouverez plus d’informations sur CreateJS sur la [page d’accueil CreateJS](http://www.createjs.com).


## <a name="getting-started"></a>Prise en main

Le code source complet de l’application est stocké sur [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d).

Pour démarrer, le plus simple est de visiter le site GitHub, de cliquer sur le bouton vert **Cloner ou télécharger**, puis de sélectionner **Ouvrir dans Visual Studio**. 

![Cloner le référentiel](images/JS2D_2.png)

Vous pouvez également télécharger le projet sous forme de fichier zip ou utiliser une autre méthode standard pour travailler avec des [projets GitHub](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-uwp-app-samples).

Une fois la solution chargée dans Visual Studio, vous y verrez plusieurs fichiers, notamment les suivants:

* Images/: dossier contenant les différentes icônes requises par les applications UWP, ainsi que le SpriteSheet du jeu et quelques autres bitmaps
* Js/: dossier contenant les fichiers JavaScript. Le fichier main.js correspond à notre jeu, les autres fichiers sont EaselJS et PreloadJS.
* index.html: page Web contenant l’objet de zone de dessin qui héberge les graphiques du jeu.

Vous pouvez maintenant exécuter le jeu!

Appuyez sur **F5** pour démarrer l’exécution de l’application. Vous devez voir une fenêtre est ouverte, notre dinosaure familier dans un environnement idyllique (quoique) paysage. Nous allons maintenant examiner l’application, expliquer certains éléments importants et découvrir le reste des fonctionnalités au fur et à mesure.

![Ce n'est qu'un banal dinosaure avec un chat ninja sur le dos](images/JS2D_3.png)

**Remarque:** quelque chose s'est mal passé? Assurez-vous que vous avez installé Visual Studio avec prise en charge web. Vous pouvez le vérifier en créant un nouveau projet: s’il n’existe aucune prise en charge de JavaScript, vous devez réinstaller Visual Studio et cocher la case *Microsoft Web Developer Tools*.

## <a name="walkthough"></a>Procédure pas à pas

Si vous avez démarré le jeu avec F5, vous vous demandez probablement ce qui se passe. Et la réponse est «pas grand chose», car une grande partie du code est actuellement commentée. Jusqu'à présent, tout s’affiche est le Dinosaure et un appel inopérant à appuyer sur espace. 

### <a name="1-setting-the-stage"></a>1. Installation du décor

Si vous ouvrez et examinez **index.html**, vous verrez qu’il est presque vide. Ce fichier est la page web par défaut qui contient notre application et il n'effectue que deux choses importantes. Tout d’abord, il comprend le code source JavaScript des bibliothèques CreateJS **EaselJS** et **PreloadJS**, ainsi que **main.js** (notre propre fichier de code source).
Ensuite, il définit une balise &lt;canvas&gt;, dans laquelle tous nos graphiques vont apparaître. Un &lt;canvas&gt; (zone de dessin) est un composant de document HTML5 standard. Nous lui donnons un nom (gameCanvas) afin que notre code dans **main.js** puisse le référencer. D'ailleurs, si vous envisagez d’écrire votre propre jeu JavaScript à partir de zéro, vous devrez également copier les fichiers **EaselJS** et **PreloadJS** dans votre solution, puis créer un objet de la zone de dessin.

EaselJS nous fournit un nouvel objet appelé *stage* (scène). La scène est liée à la zone de dessin et est utilisée pour l’affichage des images et du texte. Tout objet à afficher sur la scène doit d’abord être ajouté en tant qu’enfant de la scène, comme suit:

```
    stage.addChild(myObject);
```

Vous verrez cette ligne de code apparaître plusieurs fois dans **main.js**

À propos, il est désormais temps d'ouvrir **main.js**.

### <a name="2-loading-the-bitmaps"></a>2. Chargement des bitmaps

EaselJS fournit différents types d’objets graphiques. Nous pouvons créer des formes simples (par exemple, le rectangle bleu utilisé pour le ciel), ou des bitmaps (par exemple, les nuages que nous allons ajouter), des objets de texte et des sprites. Les sprites utilisent (SpriteSheet) [http://createjs.com/docs/easeljs/classes/SpriteSheet.html]: une seule image bitmap contenant plusieurs images. Par exemple, nous utilisons ce SpriteSheet pour stocker les différentes images de l’animation du dinosaure:

![Feuille de sprite Dino en marche](images/JS2D_4.png)

Nous faisons marcher le dinosaure, en définissant les différentes images, ainsi que la vitesse à laquelle elles doivent être animées dans ce code:

```
    // Define the animated dino walk using a spritesheet of images,
    // and also a standing-still state, and a knocked-over state.
    var data = {
        images: [loader.getResult("dino")],
        frames: { width: 373, height: 256 },
        animations: {
            stand: 0,
            lying: { 
                frames: [0, 1],
                speed: 0.1
            },
            walk: {
                frames: [0, 1, 2, 3, 2, 1],
                speed: 0.4
            }
        }
    }

    var spriteSheet = new createjs.SpriteSheet(data);
    dino_walk = new createjs.Sprite(spriteSheet, "walk");
    dino_stand = new createjs.Sprite(spriteSheet, "stand");
    dino_lying = new createjs.Sprite(spriteSheet, "lying");

```

Maintenant, nous allons ajouter quelques petits nuages floconneux à la scène. Lors de l'exécution du jeu, ils vont dériver à travers l’écran. L’image du nuage est déjà présente dans la solution, dans le dossier *images*.

Parcourez **main.js** pour rechercher la fonction **init()**. Elle est appelée lorsque le jeu démarre, et c'est par elle que nous commençons pour configurer tous nos objets graphiques.

Recherchez le code ci-dessous et supprimez les commentaires (\\) à partir de la ligne qui fait référence à l’image de nuage.

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

JavaScript a besoin d'un peu d'aide pour le chargement de ressources telles que des images. Nous utilisons donc une fonctionnalité de la bibliothèque CreateJS, appelée [LoadQueue](http://www.createjs.com/docs/preloadjs/classes/LoadQueue.html), qui permet de précharger des images. Nous ne pouvons pas savoir combien de temps prendra le chargement des images. C'est pourquoi nous utilisons la fonctionnalité LoadQueue pour s'en occuper. Une fois les images disponibles, la file d’attente nous indiquera qu’elles sont prêtes. Pour ce faire, nous créons d’abord un nouvel objet qui répertorie toutes nos images, puis un objet LoadQueue. Vous pouvez voir dans le code suivant une configuration qui permet d'appeler la fonction nommée **loadingComplete()** lorsque tout est prêt.

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

Lorsque la fonction **loadingComplete()** est appelée, les images sont chargées et prêtes à l'emploi. Vous verrez une section commentée qui crée les nuages, maintenant que leur image bitmap est disponible. Supprimez les commentaires de sorte qu’il se présentent comme suit:

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
Ce code crée trois objets nuage utilisant chacun notre image préalablement chargée, définit leur emplacement et les ajoute à la scène.

Exécutez à nouveau l’application (appuyez sur F5) et vous verrez les nuages apparaître.

### <a name="3-moving-the-clouds"></a>3. Déplacement des nuages

Nous allons maintenant faire en sorte que les nuages se déplacent. Le secret pour que les nuages (et n'importe quel élément, en fait) se déplacent consiste à configurer une fonction [ticker](http://www.createjs.com/docs/easeljs/classes/Ticker.html) qui est appelée à plusieurs reprises, plusieurs fois par seconde. Chaque fois que cette fonction est appelée, elle redessine les graphiques dans un emplacement légèrement différent.

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen">Consultez le stylet <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS - Animating clouds (CreateJS - Animation de nuages)</a> de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) sur <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
 
Le code nécessaire se trouve déjà dans le fichier **main.js**, fourni par la bibliothèque CreateJS, EaselJS. Il se présente ainsi:

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

Ce code appelle une fonction appelée **gameLoop()** entre 30 et 60images par seconde. La vitesse exacte dépend de la vitesse de votre ordinateur.

Recherchez la fonction **gameLoop()** et vers la fin, vous trouverez une fonction appelée **animateClouds()**. Modifiez-la afin qu’elle ne soit pas commentée.

```
    // Move clouds
    animateClouds();
```

Si vous examinez la définition de cette fonction, vous pouvez constater qu'elle prend les nuages un par un et modifie leur coordonnée x. Si l'ordonnée x dépasse le côté de l’écran, elle est réinitialisée à l’extrême droite. Chaque nuage se déplace également à une vitesse légèrement différente.

```
function animate_clouds()
{
    // Move the cloud sprites across the sky. If they get to the left edge, 
    // move them over to the right.

    for (var i = 0; i < 3; i++) {    
        cloud[i].x = cloud[i].x - (i+1);
        if (cloud[i].x < -128)
            cloud[i].x = width + 128;
    }
}
```

Si vous exécutez l’application maintenant, vous constaterez que les nuages ont commencé à dériver. Enfin, du mouvement!

### <a name="4-adding-keyboard-and-mouse-input"></a>4. Ajout d’entrées effectuées à l’aide du clavier et de la souris

Un jeu avec lequel vous ne pouvez pas interagir n’est pas un jeu. Nous allons donc autoriser le joueur à utiliser le clavier ou la souris pour effectuer une action. Dans la fonction **loadingComplete()**, vous pouvez voir les éléments suivants. Supprimez les commentaires.

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

Nous avons maintenant deux fonctions appelées chaque fois que le joueur appuie sur une touche ou clique sur la souris. Les deux événements appellent une fonction **userDidSomething()** qui ressemble à la variable gamestate pour déterminer l’action du jeu en cours et l’étape suivante qui doit se produire en conséquence.

Gamestate est un modèle de conception courant utilisé dans les jeux. Tout événement se produit dans la fonction **gameLoop()** appelée par le minuteur du ticker. Le gameLoop() effectue un suivi pour déterminer si le jeu est en cours ou en «game over state» (état fin de partie) ou «ready-to-play state» (état prêt à jouer) ou d'autre états définis par l’auteur, à l’aide d’une variable. Cette variable d’état est testée dans une instruction switch, qui définit les autres fonctions appelées. Par conséquent, si l’état est défini sur «playing» (lecture), les fonctions qui permettent de faire sauter le dinosaure et de déplacer les barils seront appelées. Si le dinosaure est tué par quelque chose, la variable gamestate est définie sur «game over state» et le message «Game over!» s’affiche à la place. Si les modèles de conception de jeux vous intéressent, le livre [Modèles de programmation de jeu](http://gameprogrammingpatterns.com/) est très utile.

Essayez de relancer l’application et vous pourrez enfin commencer à jouer. Appuyez sur espace (ou cliquez sur la souris ou appuyez sur l’écran) pour lancer l'animation. 

Vous verrez un baril rouler vers vous: appuyez sur la touche espace ou cliquez à nouveau au moment approprié et le dinosaure sautera. Vous avez raté le bon moment et votre jeu est terminé.

Le baril est animé de la même manière que les nuages (bien qu’il soit plus rapide à chaque fois) et nous vérifions la position du dinosaure et du baril pour savoir s'ils se sont heurtés:

```
 // Very simple check for collision between dino and barrel
                if ((barrel.x > 220 && barrel.x < 380)
                    &&
                    (!jumping))
                {
                    barrel.x = 380;
                    GameState = GameStateEnum.GameOver;
                }
```

Si le dinosaure ne saute pas et que le baril se trouve à proximité, le code remplace la variable d’état par l’état que nous avons appelé *GameOver*. Comme vous pouvez l'imaginer, *GameOver* arrête le jeu.

Les principaux mécanismes de notre jeu sont désormais terminés.

### <a name="5-resizing-support"></a>5. Prise en charge du redimensionnement

Nous avons presque terminé! Mais avant de nous arrêter, nous devons d'abord nous occuper d'un problème ennuyeux. Quand jeu est en cours d’exécution, essayez de redimensionner la fenêtre. Vous constaterez que le jeu devient vite très chaotique, les objets ne se trouvant plus là où ils sont censés être. Nous pouvons remédier à cela en créant un gestionnaire pour l’événement de redimensionnement de fenêtre généré lorsque le joueur redimensionne la fenêtre ou lorsque l’appareil pivote du mode paysage à portrait.

Le code pour effectuer cette opération est déjà présent (en fait, nous l’appelons au lancement du jeu, pour veiller au bon fonctionnement de la taille de fenêtre par défaut, car lors du lancement d'une application UWP, il est impossible de connaître la taille de la fenêtre).

Supprimez simplement le commentaire de cette ligne pour appeler la fonction lors du déclenchement de l’événement de taille de l'écran:

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

Si vous exécutez à nouveau l’application, vous devez maintenant pouvoir redimensionner la fenêtre et obtenir de meilleurs résultats.

## <a name="publishing-to-the-microsoft-store"></a>Publication dans le Microsoft Store

Maintenant que vous disposez d’une application UWP, il est possible de publier dans le Microsoft Store (en supposant que vous l’avez tout d’abord améliorée!) 

La procédure comprend quelques étapes.

1. Vous devez être [enregistré](https://developer.microsoft.com/en-us/store/register) en tant que développeur Windows.
2. Vous devez utiliser la [liste de vérification](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) de soumission d’applications.
3. L’application doit être soumise pour [certification](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process).

Pour plus d’informations, voir la [publication de votre application UWP](https://developer.microsoft.com/en-us/store/publish-apps).

## <a name="suggestions-for-other-features"></a>Suggestions pour d’autres fonctionnalités.

Quelle est l'étape suivante? Voici quelques suggestions de fonctionnalités à ajouter à votre (future) application primée.

1. Effets sonores. La bibliothèque CreateJS inclut la prise en charge de sons, à l'aide d'une bibliothèque appelée [SoundJS](http://www.createjs.com/soundjs).
2. Prise en charge du boîtier de commande. Il existe une [API disponible](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345).
3. Améliorez considérablement ce jeu! Cela dépend de vous, mais il existe un grand nombre de ressources disponibles en ligne. 

## <a name="other-links"></a>Autres liens

* [Créer un jeu Windows simple avec JavaScript](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [Sélection d’un moteur de jeu HTML/JS](https://html5gameengine.com/)
* [Utilisation de CreateJS dans votre jeu basé sur JS](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [Cours de développement de jeux sur LinkedIn Learning](https://www.linkedin.com/learning/topics/game-development)
