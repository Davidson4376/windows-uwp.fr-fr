---
title: Didacticiel de prise en main - Jeu UWP3D en JavaScript
description: Un jeu UWP pour le Microsoft Store, écrit en JavaScript avec three.js
author: abbycar
ms.author: abigailc
ms.date: 03/06/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.localizationpriority: medium
ms.openlocfilehash: 0183e19135758f73dfea9b63535437ff9b66011a
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5683850"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>Création d’un jeu JavaScript 3D à l’aide de three.js

## <a name="introduction"></a>Introduction

Pour les développeurs Web ou JavaScript, le développement d’applications UWP avec JavaScript est un moyen simple de publier des applications dans le monde entier. Plus besoin d’apprendre un langage tel que C# ou C++!

Dans cet exemple, nous utiliserons la bibliothèque **three.js**. Cette bibliothèque s'appuie sur WebGL, une API utilisée pour le rendu des graphiques2D et3D pour les navigateurs Web. **Three.js** part de cette API complexe et la simplifie, ce qui facilite le développement3D. 


Vous souhaitez avoir un aperçu de l’application que nous allons développer avant de continuer? Essayez-la sur CodePen!

<iframe height='300' scrolling='no' title='Jeu Dino final' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpKejy/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/'>Dino game final (Jeu dino final)</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!NOTE] 
> Ce n’est pas un jeu complet. Il est conçu pour montrer comment utiliser JavaScript et une bibliothèque tierce pour rendre une application prête à être publiée dans le Microsoft Store.


## <a name="requirements"></a>Spécifications

Pour jouer avec ce projet, vous aurez besoin des éléments suivants:
-   Un ordinateur Windows (physique ou virtuel) exécutant la version actuelle de Windows10.
-   Visual Studio. Visual Studio Community Edition peut être téléchargé gratuitement à partir de la [page d’accueil de Visual Studio](http://visualstudio.com/).
Ce projet utilise la bibliothèque JavaScript **three.js**. **Three.js** est publié sous la licence MIT. Cette bibliothèque est déjà présente dans le projet (recherchez `js/libs` dans la vue Explorateur de solutions). Vous trouverez plus d’informations sur cette bibliothèque sur la page d’accueil de [**three.js**](https://threejs.org/).

## <a name="getting-started"></a>Prise en main

Le code source complet de l’application se trouve sur [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d).

Pour démarrer, le plus simple est de visiter le site GitHub, de cliquer sur le bouton vert Cloner ou télécharger, puis de sélectionner Ouvrir dans Visual Studio. 

![Bouton Cloner ou télécharger](images/3dclone.png)

Si vous ne voulez pas cloner le projet, vous pouvez le télécharger sous forme de fichier zip.
Une fois la solution chargée dans Visual Studio, vous y verrez plusieurs fichiers, notamment les suivants:
-   Images/: dossier contenant les différentes icônes requises par les applications UWP.
- css/: dossier contenant le fichier CSS à utiliser.
-   Js/: dossier contenant les fichiers JavaScript. Le fichier main.js correspond à notre jeu, tandis que les autres fichiers sont les bibliothèques tierces.
-   models/: dossier contenant les modèles3D. Dans ce jeu, il n'en existe qu'un seul, celui du dinosaure.
-   index.html: page Web qui héberge le moteur de rendu du jeu.

Vous pouvez maintenant lancer le jeu!

Appuyez sur F5 pour démarrer l’application. Vous devriez voir une fenêtre s'ouvrir et vous inviter à cliquer sur l’écran. Vous verrez également un dinosaure se déplacer à l'arrière-plan. Fermez le jeu, nous allons commencer à examiner l'application et ses composants clés.

> [!NOTE] 
> Quelque chose s'est mal passé? Assurez-vous que vous avez installé Visual Studio avec prise en charge Web. Vous pouvez le vérifier en créant un nouveau projet: si JavaScript n'est pas pris en charge, vous devrez réinstaller Visual Studio et cocher la case Microsoft Web Developer Tools.

## <a name="walkthrough"></a>Procédure pas à pas

Lorsque vous démarrez ce jeu, un message vous invite à cliquer sur l’écran. L'[API Pointer Lock](https://developer.mozilla.org/docs/Web/API/Pointer_Lock_API) vous permet de regarder les alentours à l'aide de la souris. Les déplacements s’effectuent en appuyant sur les touches W, A, S, D ou les touches de direction.
Le jeu consiste à rester loin du dinosaure. Si le dinosaure est suffisamment proche de vous, il vous prend en chasse jusqu'à ce que vous soyez hors de portée ou trop près, auquel cas vous perdez le jeu.

### <a name="1-setting-up-your-initial-html-file"></a>1. Configuration de votre fichier HTML initial

Dans **index.html**, vous devrez ajouter un peu de code HTML pour commencer. Ce fichier est la page Web par défaut qui contient notre application.

Nous allons maintenant la configurer avec les bibliothèques que nous allons utiliser et le `div` (appelé `container`) qui servira au rendu de nos graphiques. Nous allons également la configurer pour qu’elle pointe vers notre **main.js** (notre code de jeu).


```html
<!DOCTYPE html>
<html lang='en'>

<head>
    <link rel="stylesheet" type="text/css" href="css/stylesheet.css" />
</head>

    <body>
        <div id='container'></div>
        <script src='js/libs/three.js'></script>
        <script src="js/controls/PointerLockControls.js"></script>
        <script src="js/main.js"></script>
    </body>

</html>
```


Maintenant que notre code de démarrage HTML est prêt, passons à **main.js** pour créer quelques graphiques.

### <a name="2-creating-your-scene"></a>2. Création de votre décor

Dans la section de la procédure pas à pas, nous allons installer la base du jeu.

Nous commencerons par modeler une `scene`. Une `scene` dans **three.js** correspond à l'endroit où votre caméra, vos objets et vos lumières seront ajoutés. Vous aurez également besoin d’un convertisseur pour restituer ce que votre caméra voit de la scène et l’afficher.

Dans **main.js** nous allons créer une fonction, appelée `init()`, qui permet de faire tout cela et qui fait appel à certaines fonctions supplémentaires:

```javascript
var UNITWIDTH = 90; // Width of a cubes in the maze
var UNITHEIGHT = 45; // Height of the cubes in the maze

var camera, scene, renderer;

init();
animate();

function init() {
    // Create the scene where everything will go
    scene = new THREE.Scene();

    // Add some fog for effects
    scene.fog = new THREE.FogExp2(0xcccccc, 0.0015);

    // Set render settings
    renderer = new THREE.WebGLRenderer();
    renderer.setClearColor(scene.fog.color);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    // Get the HTML container and connect renderer to it
    var container = document.getElementById('container');
    container.appendChild(renderer.domElement);

    // Set camera position and view details
    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 2000);
    camera.position.y = 20; // Height the camera will be looking from
    camera.position.x = 0;
    camera.position.z = 0;

    // Add the camera
    scene.add(camera);

    // Add the walls(cubes) of the maze
    createMazeCubes();

    // Add lights to the scene
    addLights();

    // Listen for if the window changes sizes and adjust
    window.addEventListener('resize', onWindowResize, false);
}

```

Les autres fonctions à créer sont les suivantes:
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- Fonctions de conversion d’unités

#### <a name="createmazecubes"></a>createMazeCubes()

La fonction `createMazeCubes()` ajoute un cube simple à notre scène. Nous créerons ensuite une fonction permettant d'ajouter de nombreux cubes afin de créer notre labyrinthe.

```javascript
function createMazeCubes() {

  // Make the shape of the cube that is UNITWIDTH wide/deep, and UNITHEIGHT tall
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  // Make the material of the cube and set it to blue
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });
  
  // Combine the geometry and material to make the cube
  var cube = new THREE.Mesh(cubeGeo, cubeMat);

  // Add the cube to the scene
  scene.add(cube);

  // Update the cube's position
  cube.position.y = UNITHEIGHT / 2;
  cube.position.x = 0;
  cube.position.z = -100;
  // rotate the cube by 30 degrees
  cube.rotation.y = degreesToRadians(30);
}

```

#### <a name="addlights"></a>addLights()

La fonction `addLights()` est une fonction simple qui regroupe la création de nos lumières et les ajoute à la scène.

```javascript
function addLights() {
  var lightOne = new THREE.DirectionalLight(0xffffff);
  lightOne.position.set(1, 1, 1);
  scene.add(lightOne);

  // Add a second light with half the intensity
  var lightTwo = new THREE.DirectionalLight(0xffffff, .5);
  lightTwo.position.set(1, -1, -1);
  scene.add(lightTwo);
}
```

#### <a name="onwindowresize"></a>onWindowResize()

La fonction `onWindowResize` est appelée chaque fois que le détecteur d’événements détecte le déclenchement d'un événement `resize`. Cela se produit chaque fois que l’utilisateur modifie la taille de la fenêtre. Dans un tel cas, nous voulons que l’image reste proportionnée et s'affiche dans toute la fenêtre.

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

Le dernier élément dont nous avons besoin est la fonction `animate()`, qui appellera également la fonction `render()`. La fonction [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) permet de mettre notre convertisseur à jour en permanence. Nous utiliserons ensuite ces fonctions pour mettre à jour notre convertisseur avec des animations amusantes, telles que des déplacements dans le labyrinthe.

```javascript
function animate() {
    render();
    // Keep updating the renderer
    requestAnimationFrame(animate);
}

function render() {
    renderer.render(scene, camera);
}
```

#### <a name="unit-conversion-functions"></a>Fonctions de conversion d’unités

Dans **three.js**, les rotations sont mesurées en radians. Pour plus de simplicité, nous allons ajouter quelques fonctions qui permettent de convertir facilement les degrés en radians. 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

Qui peut se rappeler que 30degrés correspondent à 0,523radians? Il est beaucoup plus simple d'utiliser `degreesToRadians(30)` pour obtenir les valeurs de rotation utilisées dans notre fonction `createMazeCubes()`.

___

Cela nous a demandé pas mal de code, mais nous avons désormais un superbe cube dans notre `container`! Regardez le résultat dans le CodePen.

Vous pouvez copier et coller l'ensemble du code JavaScript dans ce CodePen pour vous rattraper si vous avez rencontré des problèmes, ou le modifier pour régler les lumières ou changer les couleurs. 

<iframe height='300' scrolling='no' title='Lumières, caméra, cube!' src='//codepen.io/MicrosoftEdgeDocumentation/embed/YZWygZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/YZWygZ/'>lumières, caméra, cube!</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="3-making-the-maze"></a>3. Création du labyrinthe

Bien que pouvoir admirer un cube soit déjà magique, pouvoir contempler tout un labyrinthe constitué de cubes, c'est encore bien mieux! C'est un secret de Polichinelle chez les développeurs de jeux, l'un des moyens les plus rapides pour créer un niveau est de placer des cubes partout avec un tableau2D.
 
![labyrinthe créé à l'aide d'un tableau2D](images/dinomap.png)

Mettre des1 là où il y a des cubes et des0 lorsque l’espace est vide constitue une méthode manuelle simple pour créer ou modifier le labyrinthe.

Nous achevons l'opération en remplaçant notre ancienne fonction `createMazeCubes()` par celle qui utilise une boucle imbriquée pour créer et placer plusieurs cubes. Nous allons également créer un nom pour le tableau `collidableObjects` et y ajouter les cubes pour la détection des collisions, comme nous le verrons plus loin dans ce didacticiel:

```javascript
var totalCubesWide; // How many cubes wide the maze will be
var collidableObjects = []; // An array of collidable objects used later

function createMazeCubes() {
  // Maze wall mapping, assuming even square
  // 1's are cubes, 0's are empty space
  var map = [
    [0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ]
  ];

  // wall details
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });

  // Keep cubes within boundry walls
  var widthOffset = UNITWIDTH / 2;
  // Put the bottom of the cube at y = 0
  var heightOffset = UNITHEIGHT / 2;
  
  // See how wide the map is by seeing how long the first array is
  totalCubesWide = map[0].length;

  // Place walls where 1`s are
  for (var i = 0; i < totalCubesWide; i++) {
    for (var j = 0; j < map[i].length; j++) {
      // If a 1 is found, add a cube at the corresponding position
      if (map[i][j]) {
        // Make the cube
        var cube = new THREE.Mesh(cubeGeo, cubeMat);
        // Set the cube position
        cube.position.z = (i - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        cube.position.y = heightOffset;
        cube.position.x = (j - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        // Add the cube
        scene.add(cube);
        // Used later for collision detection
        collidableObjects.push(cube);
      }
    }
  }
    // The size of the maze will be how many cubes wide the array is * the width of a cube
    mapSize = totalCubesWide * UNITWIDTH;
}

```

Maintenant que nous savons combien de cubes sont utilisés (et quelle est leur taille), nous pouvons utiliser la variable calculée `mapSize` pour définir les dimensions du plan du sol:

```javascript
var mapSize;    // The width/depth of the maze

function createGround() {
    // Create ground geometry and material
    var groundGeo = new THREE.PlaneGeometry(mapSize, mapSize);
    var groundMat = new THREE.MeshPhongMaterial({ color: 0xA0522D, side: THREE.DoubleSide});

    var ground = new THREE.Mesh(groundGeo, groundMat);
    ground.position.set(0, 1, 0);
    // Rotate the place to ground level
    ground.rotation.x = degreesToRadians(90);
    scene.add(ground);
}
```

La dernière pièce à ajouter au labyrinthe est le mur d'enceinte qui contiendra tous les éléments. Nous allons utiliser une boucle pour créer deux plans (nos murs) en une fois, en utilisant la variable `mapSize` que nous avons calculée dans `createGround()` pour déterminer la largeur à leur donner. Les nouveaux murs seront également ajoutés à notre tableau `collidableObjects` pour la future détection de collisions:

```javascript
function createPerimWalls() {
    var halfMap = mapSize / 2;  // Half the size of the map
    var sign = 1;               // Used to make an amount positive or negative

    // Loop through twice, making two perimeter walls at a time
    for (var i = 0; i < 2; i++) {
        var perimGeo = new THREE.PlaneGeometry(mapSize, UNITHEIGHT);
        // Make the material double sided
        var perimMat = new THREE.MeshPhongMaterial({ color: 0x464646, side: THREE.DoubleSide });
        // Make two walls
        var perimWallLR = new THREE.Mesh(perimGeo, perimMat);
        var perimWallFB = new THREE.Mesh(perimGeo, perimMat);

        // Create left/right wall
        perimWallLR.position.set(halfMap * sign, UNITHEIGHT / 2, 0);
        perimWallLR.rotation.y = degreesToRadians(90);
        scene.add(perimWallLR);
        // Used later for collision detection
        collidableObjects.push(perimWallLR);
        // Create front/back wall
        perimWallFB.position.set(0, UNITHEIGHT / 2, halfMap * sign);
        scene.add(perimWallFB);

        // Used later for collision detection
        collidableObjects.push(perimWallFB);

        sign = -1; // Swap to negative value
    }
}
```


N’oubliez pas d’ajouter un appel à `createGround()` et `createPerimWalls` après `createMazeCubes()` dans votre fonction `init()` pour qu’ils soient compilés!
___

Nous pouvons maintenant admirer un magnifique labyrinthe, mais il n'est pas vraiment possible de l'apprécier à sa juste valeur car la caméra est bloquée à un seul endroit. Il est temps d'aller plus loin en ajoutant quelques contrôles de caméra.

N’hésitez pas à tester les éléments dans le CodePen, par exemple en changeant les couleurs des cubes ou en supprimant le sol, en mettant en commentaire `createGround()` dans la fonction `init()`.


<iframe height='300' scrolling='no' title='Construction du labyrinthe' src='//codepen.io/MicrosoftEdgeDocumentation/embed/JWKYzG/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/JWKYzG/'>Maze building (Construction du labyrinthe)</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="4-allowing-the-player-to-look-around"></a>4. Permettre au joueur de regarder autour de lui

C'est maintenant le moment d'entrer dans ce labyrinthe et de l'explorer. Pour cela, nous allons utiliser la bibliothèque **PointerLockControls.js** et la caméra.

La bibliothèque **PoinerLockControls.js** utilise la souris pour faire pivoter la caméra dans la direction indiquée par le déplacement de la souris, ce qui permet au joueur de regarder autour de lui. 

Nous allons commencer par ajouter des éléments à notre fichier **index.html**:

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

Vous avez également besoin de tout le CSS du CodePen situé en bas de cette section. Vous devez le coller dans votre fichier **stylesheet.css**.

Revenez au **main.js**, ajoutez quelques nouvelles variables globales: `controls` pour stocker notre contrôleur, `controlsEnabled` pour assurer le suivi de l’état du contrôleur et `blocker` pour récupérer l'élément `blocker` dans **index.html**:

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


Dans notre fonction `init()`, nous pouvons maintenant créer un nouvel objet `PoinerLockControls`, le transmettre à notre `camera` et ajouter la `camera` (accessible avec `controls.getObject()`).

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

La caméra est maintenant connectée, mais nous devons permettre à la souris et au contrôleur d'interagir afin que nous puissions regarder les environs. 

Dans cette situation, l'[API Pointer Lock](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock) vient à la rescousse en nous permettant de raccorder les mouvements de la souris à la caméra. L’API Pointer Lock fait également disparaître la souris pour que le joueur soit mieux immergé dans le jeu. Une pression sur ÉCHAP, met fin à la connexion entre la caméra et la souris pour faire réapparaître la souris. L'ajout des fonctions `getPointerLock()` et `lockChange()` nous permettra d'atteindre cet objectif.

La fonction `getPointerLock()` permet de détecter un clic de souris. Après le clic, notre jeu rendu (dans l'élément `container`) tente de prendre le contrôle de la souris. Nous avons également ajouté un détecteur d’événements pour détecter l'activation ou la désactivation du verrouillage par le joueur, qui appelle ensuite `lockChange()`. 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

Notre fonction `lockChange()` doit activer ou désactiver les contrôles et l'élément `blocker`. Nous pouvons déterminer l’état de verrouillage du curseur en vérifiant si la cible de la propriété [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement) des événements de souris est définie sur notre `container`.

```javascript
function lockChange() {
    // Turn on controls
    if (document.pointerLockElement === container) {
        // Hide blocker and instructions
        blocker.style.display = "none";
        controls.enabled = true;
    // Turn off the controls
    } else {
      // Display the blocker and instruction
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

Nous pouvons maintenant ajouter un appel à `getPointerLock()` juste avant notre fonction `init()`.
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

À ce stade, nous avons désormais la possibilité de **regarder** les environs, mais ce qui est réellement impressionnant, c'est la possibilité de **se déplacer**. Les choses vont devenir un peu mathématiques avec les vecteurs, mais qu’est-ce qu'un graphisme3D sans un peu de maths?

<iframe height='300' scrolling='no' title='Regarder autour de lui' src='//codepen.io/MicrosoftEdgeDocumentation/embed/gmwbMo/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/gmwbMo/'>Look around (regarder les environs)</a> de MicrosoftEdge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="5-adding-player-movement"></a>5. Ajout de déplacements du joueur

Pour voir plus en profondeur comment nous allons faire bouger notre joueur, nous devons nous rappeler ce que nous avons appris à l'école. Nous voulons appliquer une vitesse (un mouvement) à la `camera` le long d’un vecteur donné (une direction).

Nous allons ajouter quelques variables globales supplémentaires pour garder trace de la direction dans laquelle le joueur se déplace et définir un vecteur de rapidité initial:

```javascript
// Flags to determine which direction the player is moving
var moveForward = false;
var moveBackward = false;
var moveLeft = false;
var moveRight = false;

// Velocity vector for the player
var playerVelocity = new THREE.Vector3();

// How fast the player will move
var PLAYERSPEED = 800.0;

var clock;
```

Au début de la fonction `init()`, réglez `clock` sur un nouvel objet `Clock`. Nous allons l'utiliser pour suivre la modification du temps (delta) nécessaire pour rendre de nouvelles images. Vous devrez également ajouter un appel à `listenForPlayerMovement()`, qui regroupe les entrées utilisateur. 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

Notre fonction `listenForPlayerMovement()` est ce qui fera basculer nos états de direction. Au bas de la fonction, nous avons deux détecteurs d’événements qui attendent que des touches soient enfoncées ou relâchées. Lorsqu’un de ces événements aura été déclenché, nous vérifierons s’il s’agit d’une touche que nous voulons utiliser pour déclencher ou pour arrêter un mouvement.

Pour ce jeu, nous avons fait en sorte que le joueur puisse se déplacer avec les touches W, A, S, D ou avec les flèches de direction.

```javascript
function listenForPlayerMovement() {
    
    // A key has been pressed
    var onKeyDown = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = true;
        break;

      case 37: // left
      case 65: // a
        moveLeft = true;
        break;

      case 40: // down
      case 83: // s
        moveBackward = true;
        break;

      case 39: // right
      case 68: // d
        moveRight = true;
        break;
    }
  };

  // A key has been released
    var onKeyUp = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = false;
        break;

      case 37: // left
      case 65: // a
        moveLeft = false;
        break;

      case 40: // down
      case 83: // s
        moveBackward = false;
        break;

      case 39: // right
      case 68: // d
        moveRight = false;
        break;
    }
  };

  // Add event listeners for when movement keys are pressed and released
  document.addEventListener('keydown', onKeyDown, false);
  document.addEventListener('keyup', onKeyUp, false);
}
```

Maintenant que nous pouvons déterminer la direction dans laquelle l’utilisateur souhaite aller (qui est maintenant stockée comme `true` dans un des indicateurs de direction globale), il est temps de passer à l'action. Cette action va prendre la forme de la fonction `animatePlayer()`.

Cette fonction est appelée depuis `animate()`, passant en `delta` pour récupérer la modification de l’intervalle entre les images afin que notre mouvement ne paraisse pas désynchronisé lors des changements de fréquence d’images:

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

Maintenant, nous allons pouvoir nous amuser un peu! Notre vecteur d’impulsion (`playerVeloctiy`) possède trois paramètres, `(x, y, z)`, `y` représentant l’impulsion verticale. Puisque nous ne n'utiliserons pas les sauts dans ce jeu, nous allons travailler exclusivement avec les paramètres `x` et `z`. Initialement, ce vecteur est réglé sur (0, 0, 0).

Comme le montre le code ci-dessous, une série de vérifications est effectuée pour voir quel indicateur de direction revient sur `true`. Une fois que nous avons la direction, nous ajoutons ou soustrayons `x` et `y` pour appliquer le dynamisme dans cette direction. Si aucune touche de mouvement n’est enfoncée, le vecteur est redéfini sur `(0, 0, 0)`.


```javascript

function animatePlayer(delta) {
  // Gradual slowdown
  playerVelocity.x -= playerVelocity.x * 10.0 * delta;
  playerVelocity.z -= playerVelocity.z * 10.0 * delta;

  if (moveForward) {
    playerVelocity.z -= PLAYERSPEED * delta;
  } 
  if (moveBackward) {
    playerVelocity.z += PLAYERSPEED * delta;
  } 
  if (moveLeft) {
    playerVelocity.x -= PLAYERSPEED * delta;
  } 
  if (moveRight) {
    playerVelocity.x += PLAYERSPEED * delta;
  }
  if( !( moveForward || moveBackward || moveLeft ||moveRight)) {
    // No movement key being pressed. Stop movememnt
    playerVelocity.x = 0;
    playerVelocity.z = 0;
  }
  controls.getObject().translateX(playerVelocity.x * delta);
  controls.getObject().translateZ(playerVelocity.z * delta);
}
```

Au final, nous appliquons les valeurs mises à jour `x` et `y` quelles qu'elles soient à la caméra comme traductions pour que le joueur se déplace réellement.

---

Félicitations! Vous disposez maintenant d’une caméra contrôlée par le joueur qui peut se déplacer et observer les alentours. Nous traversons toujours les murs, mais nous nous en soucierons plus tard. Ensuite, nous allons ajouter notre dinosaure.

<iframe height='300' scrolling='no' title='Déplacer.' src='//codepen.io/MicrosoftEdgeDocumentation/embed/qrbKZg/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qrbKZg/'>se déplacer</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!NOTE]
> Si vous utilisez ces contrôles dans votre application UWP, il se peut que les mouvements soient retardés et que des événements `keyUp` ne soient pas enregistrés. Nous recherchons une solution et espérons que cette partie de l’exemple sera bientôt corrigée!

### <a name="6-load-that-dino"></a>6. Chargez ce dino!

Si vous avez cloné ou téléchargé ce référentiel de projets, vous verrez un dossier `models` avec `dino.json` à l’intérieur. Ce fichier JSON est un modèle 3D de dinosaure qui a été créé et exporté à partir de Blender.


Nous allons devoir ajouter plus de variables globales pour charger ce dino:

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

Maintenant que notre `JSONLoader` est créé, nous allons y répercuter le chemin d’accès à notre **dino.json** et un rappel avec la géométrie et les matériaux collectés à partir du fichier.
Le chargement du dino est une tâche asynchrone, ce qui signifie qu'il n'y aura aucun rendu tant que le dino ne sera pas complètement chargé. Dans notre **index.html** nous avons remplacé la chaîne dans l'élément `instructions` par `"Loading..."` pour indiquer au joueur que le chargement est en cours.

Une fois le dino chargé, mettez à jour l'élément `instructions` avec les instructions réelles du jeu, puis changez la position de la fonction `animate()` de la fin de `init()` à la fin de la fonction de rappel indiquée ci-dessous:

```javascript
   // load the dino JSON model and start animating once complete
    loader.load('./models/dino.json', function (geometry, materials) {


        // Get the geometry and materials from the JSON
        var dinoObject = new THREE.Mesh(geometry, new THREE.MultiMaterial(materials));

        // Scale the size of the dino
        dinoObject.scale.set(DINOSCALE, DINOSCALE, DINOSCALE);
        dinoObject.rotation.y = degreesToRadians(-90);
        dinoObject.position.set(30, 0, -400);
        dinoObject.name = "dino";
        scene.add(dinoObject);
        
        // Store the dino
        dino = scene.getObjectByName("dino"); 

        // Model is loaded, switch from "Loading..." to instruction text
        instructions.innerHTML = "<strong>Click to Play!</strong> </br></br> W,A,S,D or arrow keys = move </br>Mouse = look around";

        // Call the animate function so that animation begins after the model is loaded
        animate();
    });
```

---

Notre modèle de dino est maintenant chargé. Vous pouvez vérifier!

<iframe height='300' scrolling='no' title='Ajout du dino' src='//codepen.io/MicrosoftEdgeDocumentation/embed/xqOwBw/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/xqOwBw/'>Adding the dino (Ajout du dino)</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="7-move-that-dino"></a>7. Déplacez ce dino!

Créer une IA pour un jeu peut être extrêmement complexe, donc pour cet exemple, nous allons donner à ce dino un comportement de mouvement simple. Notre dino se déplacera tout droit, en traversant les murs pour s'éloigner dans le brouillard distant.

Pour ce faire, ajoutez d’abord la variable globale `dinoVelocity`.

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 Ensuite, appelez la fonction `animateDino()` à partir de la fonction `animation()` et ajoutez le code ci-dessous:

```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;

    dinoVelocity.z += DINOSPEED * delta;
    // Move the dino
    dino.translateZ(dinoVelocity.z * delta);
}
```
---

Regardez le dino s'éloigner n’est pas très amusant, mais dès que nous aurons ajouté une détection de collision, cela deviendra plus intéressant.

<iframe height='300' scrolling='no' title='Déplacement du dino - aucune collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/jBMbbL/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/jBMbbL/'>Moving the dino - no collision (Déplacement du dino - aucune collision)</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="8-collision-detection-for-the-player"></a>8. Détection des collisions pour le joueur

Désormais, le joueur et le dino se déplacent, mais il reste encore le problème ennuyeux que tout le monde traverse les murs. Lorsque nous avons commencé à ajouter nos cubes et les murs plus haut dans ce didacticiel, nous les avons placés dans le tableau `collidableObjects`. Ce tableau va nous servir à déterminer si un joueur est trop près d’un élément qu’il ne peut pas traverser.

Nous allons utiliser des raycasters pour déterminer le moment où une intersection va se produire. Vous pouvez envisager un raycaster comme un rayon laser émis par la caméra dans une direction spécifiée, qui renvoie un rapport s'il touche un objet et indique la distance exacte à laquelle cet objet se trouve.

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

Nous allons commencer par créer une fonction appelée `detectPlayerCollision()` qui retourne `true` si le joueur se trouve trop près d'un objet qu'il peut heurter.
En ce qui concerne le joueur, nous allons lui appliquer un raycaster, qui change de direction en fonction de celle vers laquelle il se dirige.

Pour ce faire, nous créons `rotationMatrix`, une matrice non définie. Comme nous vérifions la direction vers laquelle nous allons, nous allons obtenir une `rotationMatrix` définie ou non définie si vous vous déplacez vers l’avant.
Si elle est définie, la `rotationMatrix` sera appliquée à la direction des contrôles. 

Un raycaster sera alors créé, partant de la caméra vers la direction `cameraDirection`.


```javascript
function detectPlayerCollision() {
    // The rotation matrix to apply to our direction vector
    // Undefined by default to indicate ray should coming from front
    var rotationMatrix;
    // Get direction of camera
    var cameraDirection = controls.getDirection(new THREE.Vector3(0, 0, 0)).clone();

    // Check which direction we're moving (not looking)
    // Flip matrix to that direction so that we can reposition the ray
    if (moveBackward) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(180));
    }
    else if (moveLeft) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(90));
    }
    else if (moveRight) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(270));
    }

    // Player is not moving forward, apply rotation matrix needed
    if (rotationMatrix !== undefined) {
        cameraDirection.applyMatrix4(rotationMatrix);
    }

    // Apply ray to player camera
    var rayCaster = new THREE.Raycaster(controls.getObject().position, cameraDirection);

    // If our ray hit a collidable object, return true
    if (rayIntersect(rayCaster, PLAYERCOLLISIONDISTANCE)) {
        return true;
    } else {
        return false;
    }
}
```

Notre fonction `detectPlayerCollision()` s’appuie sur la fonction d’assistance `rayIntersect()`.
Elle prend un raycaster et une valeur représentant la distance à laquelle nous pouvons nous approcher d'un objet dans le tableau `collidableObjects` avant de déterminer qu'une collision s’est produite.

```javascript
function rayIntersect(ray, distance) {
    var intersects = ray.intersectObjects(collidableObjects);
    for (var i = 0; i < intersects.length; i++) {
        // Check if there's a collision
        if (intersects[i].distance < distance) {
            return true;
        }
    }
    return false;
}
```

Maintenant que nous pouvons déterminer quand une collision est sur le point de se produire, nous pouvons améliorer notre fonction `animatePlayer()`:

```javascript
function animatePlayer(delta) {
    // Gradual slowdown
    playerVelocity.x -= playerVelocity.x * 10.0 * delta;
    playerVelocity.z -= playerVelocity.z * 10.0 * delta;

    // If no collision and a movement key is being pressed, apply movement velocity
    if (detectPlayerCollision() == false) {
        if (moveForward) {
            playerVelocity.z -= PLAYERSPEED * delta;
        }
        if (moveBackward) {
            playerVelocity.z += PLAYERSPEED * delta;
        } 
        if (moveLeft) {
            playerVelocity.x -= PLAYERSPEED * delta;
        }
        if (moveRight) {
            playerVelocity.x += PLAYERSPEED * delta;
        }

        controls.getObject().translateX(playerVelocity.x * delta);
        controls.getObject().translateZ(playerVelocity.z * delta);
    } else {
        // Collision or no movement key being pressed. Stop movememnt
        playerVelocity.x = 0;
        playerVelocity.z = 0;
    }
}
```

---

Nous pouvons désormais détecter les collisions du joueur, alors allez-y, essayez de foncer dans un mur!

<iframe height='300' scrolling='no' title='Déplacer le joueur - collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/qraOeO/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qraOeO/'>Moving the player - collision (Déplacement du joueur - collision)</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. Détection de collision et animation du dino

Il est temps que notre dino cesse de passer à travers les murs et qu’il aille dans une direction aléatoire lorsqu’il est trop près d'un objet qu'il peut heurter.

Nous allons d'abord déterminer le moment où notre dino entre en collision. 

Nous devons définir une autre variable globale pour la distance de collision:

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

Maintenant que nous avons spécifié à quelle distance nous voulons que notre dino entre en collision, nous allons ajouter une fonction similaire à `detectPlayerCollision()`, mais un peu plus simple.
La fonction `detectDinoCollision` est simple du fait que nous avons toujours un raycaster partant directement de l'avant du dino. Il est inutile de le faire pivoter comme pour la collision du joueur.

```javascript
function detectDinoCollision() {
    // Get the rotation matrix from dino
    var matrix = new THREE.Matrix4();
    matrix.extractRotation(dino.matrix);
    // Create direction vector
    var directionFront = new THREE.Vector3(0, 0, 1);

    // Get the vectors coming from the front of the dino
    directionFront.applyMatrix4(matrix);

    // Create raycaster
    var rayCasterF = new THREE.Raycaster(dino.position, directionFront);
    // If we have a front collision, we have to adjust our direction so return true
    if (rayIntersect(rayCasterF, DINOCOLLISIONDISTANCE))
        return true;
    else
        return false;
}
```

Voyons à quoi notre fonction `animateDino()` finale ressemblera lorsqu'elle sera rattachée à la détection de collision:


```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;


    // If no collision, apply movement velocity
    if (detectDinoCollision() == false) {
        dinoVelocity.z += DINOSPEED * delta;
        // Move the dino
        dino.translateZ(dinoVelocity.z * delta);

    } else {
        // Collision. Adjust direction
        var directionMultiples = [-1, 1, 2];
        var randomIndex = getRandomInt(0, 2);
        var randomDirection = degreesToRadians(90 * directionMultiples[randomIndex]);

        dinoVelocity.z += DINOSPEED * delta;
        dino.rotation.y += randomDirection;
    }
}
```

Nous voulons que notre dino pivote toujours de -90, de 90 ou de 180degrés. Pour simplifier, nous avons créé ci-dessus le tableau `directionMultiples` qui génère les résultats des nombres multipliés par 90.
Pour que la sélection des degrés de rotation soit aléatoire, nous avons ajouté la fonction d’assistance `getRandomInt()` pour récupérer une valeur de 0, 1 ou 2, représentant un index aléatoire du tableau.

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

Cela fait, nous multiplions l’index aléatoire du tableau par 90 pour obtenir le degré de rotation (converti en radians).
En ajoutant cette valeur à la rotation `y` du dino avec `dino.rotation.y += randomDirection;`, le dino effectue désormais des changements de direction aléatoires après une collision.


---

Nous avons réussi! Nous avons obtenu un dino doté d'une IA et qui peut se déplacer dans notre labyrinthe!

<iframe height='300' scrolling='no' title='Déplacement du dino - collision' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/bqwMXZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/bqwMXZ/'>déplacement du dino - collision</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="10-starting-the-chase"></a>10. Lancement de la chasse

Une fois que le dino se trouve à une certaine distance du joueur, nous voulons qu’il lui donne la chasse. Comme ce n'est qu'un exemple, aucun algorithme avancé n'est appliqué au dino pour attraper le joueur. À la place, le dino regardera le joueur et marchera dans sa direction. Dans une partie dégagée du labyrinthe cela fonctionne parfaitement, mais le dino reste bloqué si un mur se trouve dans le passage.

Dans notre fonction `animate()` nous allons ajouter une variable booléenne déterminée par ce qui est retourné par `triggerChase()`:

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();

    animateDino(delta);
    animatePlayer(delta);
}
```

Notre fonction `triggerChase` vérifie si le joueur se trouve dans le périmètre de chasse du dino, puis oriente le dino toujours faire face au joueur, ce qui lui permet de se déplacer dans sa direction. 

```javascript
function triggerChase() {
    // Check if in dino detection range of the player
    if (dino.position.distanceTo(controls.getObject().position) < 300) {
        // Set the dino target's y value to the current y value. We only care about x and z for movement.
        var lookTarget = new THREE.Vector3();
        lookTarget.copy(controls.getObject().position);
        lookTarget.y = dino.position.y;

        // Make dino face camera
        dino.lookAt(lookTarget);

        // Get distance between dino and camera with a unit offset
        // Game over when dino is the value of CATCHOFFSET units away from camera
        var distanceFrom = Math.round(dino.position.distanceTo(controls.getObject().position)) - CATCHOFFSET;
        // Alert and display distance between camera and dino
        dinoAlert.innerHTML = "Dino has spotted you! Distance from you: " + distanceFrom;
        dinoAlert.style.display = '';
        return true;
        // Not in agro range, don't start distance countdown
    } else {
        dinoAlert.style.display = 'none';
        return false;
    }
}
```

La seconde moitié de la fonction `triggerChase` gère l’affichage d'un texte qui indique au joueur la distance qui le sépare du dino. Nous avons également introduit `CATCHOFFSET` pour spécifier la distance à laquelle `0` doit être. Si nous n’avions pas ce décalage, `0` serait juste au-dessus du joueur, ce qui ne serait pas très cinématographique.



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

À ce stade, un dinosaure sauvage se lance à la poursuite du joueur dès que celui-ci s'approche trop près et ne s’arrête que lorsque sa position correspond à celle du joueur.
La dernière étape consiste à ajouter des conditions de fin de partie une fois que le dino se trouve à `CATCHOFFSET` unités de distance.

<iframe height='300' scrolling='no' title='La poursuite' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpRBqR/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpRBqR/'>The chase (La chasse)</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="11-ending-the-game"></a>11. Fin du jeu


Nous avons beaucoup progressé à partir d’un simple cube, mais il est maintenant temps de terminer le jeu.

Définissons tout d’abord une variable pour déterminer si le jeu est terminé ou non:

```javascript
var gameOver = false;
```

Maintenant, nous devons mettre à jour notre fonction `animate()` une dernière fois pour vérifier si le dino est trop proche du joueur.
Si le dino est trop près, nous allons lancer une nouvelle fonction appelée `caught()` et arrêter les déplacements du joueur et du dino, sinon, nous continuerons comme d’habitude et laisserons le joueur et le dino se déplacer.

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();
    // Update our frames per second monitor

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();
    // If the player is too close, trigger the end of the game
    if (dino.position.distanceTo(controls.getObject().position) < CATCHOFFSET) {
        caught();
    // Player is at an undetected distance
    // Keep the dino moving and let the player keep moving too
    } else {
        animateDino(delta);
        animatePlayer(delta);
    }
}
```

Si le dino attrape le joueur, `caught()` affichera notre élément `blocker` et mettra à jour le texte pour indiquer que le jeu est perdu.
La variable `gameOver` est également définie sur `true`, ce qui nous indique que le jeu est terminé.  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


Maintenant que nous savons si le jeu est terminé ou non, nous pouvons ajouter une vérification de fin de partie à notre fonction `lockChange()`.
Maintenant, lorsque l’utilisateur appuie sur ÉCHAP une fois que le jeu est terminé, nous pouvons ajouter `location.reload` pour redémarrer le jeu.

```javascript
function lockChange() {
    if (document.pointerLockElement === container) {
        blocker.style.display = "none";
        controls.enabled = true;
    } else {
        if (gameOver) {
            location.reload();
        }
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

---

C’est tout! Que de chemin parcouru, mais nous disposons désormais d'un jeu réalisé avec **three.js**.

Revenez en haut de la page pour voir le [CodePen final](#introduction)!


## <a name="publishing-to-the-microsoft-store"></a>Publication dans le Microsoft Store
Maintenant que vous disposez d’une application UWP, il est possible de publier dans le Microsoft Store (en supposant que vous l’avez tout d’abord améliorée!) Il existe quelques étapes du processus.

1.  Vous devez être [enregistré](https://developer.microsoft.com/store/register) en tant que développeur Windows.
2.  Vous devez utiliser la [liste de vérification](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) de soumission d’applications.
3.  L’application doit être soumise pour [certification](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process).
Pour plus d’informations, consultez [la publication de votre application UWP](https://developer.microsoft.com/store/publish-apps).

