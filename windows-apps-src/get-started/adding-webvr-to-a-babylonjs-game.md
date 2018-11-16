---
title: Prise en charge l’ajout de WebVR à un jeu Babylon.js 3D
description: Découvrez comment ajouter la prise en charge de WebVR à un jeu Babylon.js 3D.
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, développement web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 72681c3f91fc2dcbfcc4e4531359d6d668e18b80
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6856166"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Prise en charge l’ajout de WebVR à un jeu Babylon.js 3D

Si vous avez créé un jeu en 3D avec Babylon.js et pensé qu’il peut ressembler excellent dans la réalité virtuelle (VR), suivez les étapes simples dans ce didacticiel pour faire une réalité.

Nous allons ajouter la prise en charge de WebVR au jeu illustré ici. Lancez-vous et brancher une manette Xbox pour essayer!


<iframe height='300' scrolling='no' title='Jeu de dino Babylon.js à l’aide de Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>jeu de dino Babylon.js Babylon.GUI d’à l’aide</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

Il s’agit d’un jeu 3D qui fonctionne correctement sur un écran plat, mais qu’à propos de réalité virtuelle?
Dans ce didacticiel, nous allez étudier les étapes qu’il prend pour obtenir ce et en cours d’exécution avec WebVR. Nous allons utiliser un casque [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) qui peut s’appuyer sur la prise en charge pour WebVR dans Microsoft Edge. Une fois que nous appliquons ces modifications au jeu, vous bénéficiez également pour fonctionner dans d’autres combinaisons de navigateur/casque qui prennent en charge WebVR.



## <a name="prerequisites"></a>Conditions préalables

- Un éditeur de texte (tel que [Visual Studio Code](https://code.visualstudio.com/download))
- Un contrôleur Xbox qui est connecté à votre ordinateur
- Windows10 Creators Update
- Un ordinateur avec les [minimales spécifications requises pour exécuter Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Un appareil Windows Mixed Reality (facultatif) 



## <a name="getting-started"></a>Prise en main

Le moyen le plus simple pour commencer est de visiter le [référentiel Windows-didacticiels-web GitHub](https://github.com/Microsoft/Windows-tutorials-web), appuyez sur le vert **cloner ou télécharger** bouton, puis sélectionnez **ouvert dans Visual Studio**.

![Bouton Cloner ou télécharger](images/3dclone.png)

Si vous ne voulez pas cloner le projet, vous pouvez le télécharger sous forme de fichier zip.
Vous devez ensuite deux dossiers, [avant](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) et [après](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). Le dossier «avant» correspond à notre jeu avant que toutes les fonctionnalités VR sont ajoutées, et le dossier «après» est le jeu terminé avec prise en charge de réalité virtuelle.

L’avant et après ces fichiers contiennent des dossiers:
-   **textures /** : dossier contenant toutes les images utilisées dans le jeu.
-   **css /** - un dossier contenant le fichier CSS pour le jeu.
-   **js /** : dossier contenant les fichiers JavaScript. Le fichier main.js correspond à notre jeu, et les autres fichiers sont les bibliothèques utilisées.
-   **modèles /** - un dossier contenant les modèles 3D. Pour ce jeu, nous n'avons qu’un seul modèle, celui du dinosaure.
-   **index.html** : page Web qui héberge le moteur de rendu du jeu. Ouverture de cette page dans Microsoft Edge lance le jeu.

Vous pouvez tester les deux versions du jeu en ouvrant leurs fichiers respectifs index.html dans Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>Le portail de réalité mixte

Si vous n’êtes pas familiarisé avec Windows Mixed Reality et que vous avez installé sur un ordinateur avec une carte graphique compatible Windows 10 Creators Update, essayez d’ouvrir l’application **Portail de réalité mixte** du menu Démarrer dans Windows 10.

![Recherche de portail de réalité mixte](images/mixed-reality-portal.png)

Si vous remplissez toutes les conditions, vous pouvez activer les fonctionnalités de développeur et simuler un casque Windows Mixed Reality connecté à votre ordinateur. Si vous avez la chance de disposer d’un casque réels à proximité, branché et exécutez le programme d’installation.

> [!IMPORTANT]
> Le portail de réalité mixte doit être ouvert à tout moment au cours de ce didacticiel.

Vous êtes maintenant prêt à rencontrer WebVR avec Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interface utilisateur 2D dans un monde virtuel

>[!NOTE]
> Obtenir le dossier [**avant**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) pour obtenir l’exemple de démarrage.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) est une bibliothèque de VR compatible, ce qui vous pour créer simple, qui fonctionnent bien pour VR d’interfaces utilisateur interactif et non-VR affiche.
Une extension à Babylon.js, le `GUI` bibliothèque est utilisé throuhout l’exemple pour créer des éléments 2D.


Un texte 2D `GUI` élément peut être créé avec quelques lignes en fonction du nombre d’attributs vous souhaitez modifier.
L’extrait de code suivant est déjà dans notre exemple [**avant**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) , mais nous allons procédure pas à pas, ce qui se passe.
Tout d’abord, nous effectuons un [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objet pour établir l’interface utilisateur graphique couvrira. L’exemple définit cette valeur à `CreateFullScreenUI()`, ce qui signifie que notre interface utilisateur couvre la totalité de l’écran. Avec `AdvancedDynamicTexture` créé, nous puis apportez une zone de texte 2D qui s’affiche lors du démarrage du jeu à l’aide `GUI.Rectanlge()` et `GUI.TextBlock()`.


Ce code est ajouté au sein de [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


Cette interface utilisateur est visible une fois créée, mais peuvent être activés ou désactivés avec `isVisible` en fonction de ce qui se passe dans le jeu.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Détection des casques

Il est recommandé pour les applications de réalité virtuelle d’avoir deux types d’appareils photo afin que plusieurs scénarios peuvent être pris en charge. Pour ce jeu, nous allons prendre en charge une caméra qui nécessite un casque de travail doit être branché et l’autre qui n’utilise aucun casque. Pour déterminer l’objet jeu utilisera, nous devons d’abord vérifier pour voir si un casque a été détecté. Pour ce faire, nous allons utiliser [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Ajoutez le code ci-dessus `window.addEventListener('DOMContentLoaded')` dans **main.js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Avec les informations stockées dans le `headset` variable, nous serons désormais en mesure de choisir la caméra est adaptée à l’utilisateur.


## <a name="creating-and-selecting-the-initial-camera"></a>Création et en sélectionnant la caméra initiale

Avec Babylon.js, WebVR peut être ajouté rapidement à l’aide de la [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera). Cette caméra peut prendre de saisie au clavier et vous permet d’utiliser un casque de réalité virtuelle pour contrôler la rotation de «head».


### <a name="step-1-checking-for-headsets"></a>Étape 1: Vérification des casques

Pour notre caméra de secours, nous allons utiliser le [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) qui est actuellement utilisée dans le jeu d’origine.

Nous allons cocher notre `headset` variable pour déterminer si nous pouvons utiliser la `WebVRFreeCamera` caméra.

Remplacez `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` avec le code suivant.
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>Étape 2: Activation de la WebVRFreeCamera
Pour activer cette caméra dans la plupart des navigateurs, l’utilisateur doit effectuer une intervention dont les demandes de l’expérience virtuel.
Nous allons raccorder cette fonctionnalité jusqu'à un clic de souris.


Collez le code dans `createScene()` fonctionner après `camera.applyGravity = true;` .
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Un clic dans le jeu maintenant crée une invite de commandes qui suit, ou affiche le jeu dans le casque immédiatement si l’utilisateur a accepté l’invite avant.

![invite immersive](images/immersiveview.png)

Nous pouvons également ajouter un morceau de code qui affiche le `UniversalCamera` afficher avant de nous basculer vers notre `WebVRFreeCamera`, permettant à l’utilisateur d’examiner le jeu au lieu d’une fenêtre bleue. 

Ajoutez le code suivant après `engine.runRenderLoop(function () {`.
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>Étape 3: Ajouter la prise en charge du boîtier de commande

Dans la mesure où le `WebVRFreeCamera` initialement ne gère pas les boîtiers de commande, nous allons faire correspondre à notre boutons du boîtier de commande pour les touches de direction du clavier. Nous allons le faire en plonger dans les `inputs` propriété de la caméra. En ajoutant les codes correspondantes pour le stick analogique gauche haut, bas, gauche et droite pour faire correspondre avec les touches de direction, notre boîtier de commande est en action.


Ajoutez le code ci-dessous le `scene.onPointerDown = function() {...}` appeler.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Étape 4: Essayer vous-même!

Si nous allons ouvrir **index.html** avec notre casque et branchés de contrôleur de jeu, un clic gauche sur la fenêtre de jeu bleu passe notre jeu en mode VR! Lancez-vous et placer sur votre casque à examiner les résultats. 


<iframe height='300' scrolling='no' title='Jeu de dino Babylon.js à l’aide de Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>jeu de dino Babylon.js Babylon.GUI - WebVR d’à l’aide</a> de Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusion

Félicitations! Vous disposez désormais d’un jeu Babylon.js complète avec prise en charge WebVR. À partir de là, vous pouvez prendre ce que vous avez appris générer un jeu d’une bien meilleure ou génération celle-ci.
