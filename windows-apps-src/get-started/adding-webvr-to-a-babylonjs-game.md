---
title: Ajout de la prise en charge de WebVR à un jeu 3D Babylon.js
description: Découvrez comment ajouter la prise en charge de WebVR à un jeu 3D Babylon.js.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, développement web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638554"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Ajout de la prise en charge de WebVR à un jeu 3D Babylon.js

Si vous avez créé un jeu en 3D avec Babylon.js et que vous pensez qu’il rendrait très bien en réalité virtuelle, suivez les étapes simples de ce didacticiel pour ce faire.

Nous allons ajouter la prise en charge de WebVR au jeu illustré ici. Lancez-vous et branchez une manette Xbox pour l’essayer !


<iframe height='300' scrolling='no' title='Jeu de dino Babylon.js à l’aide de Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js jeu de dino à l’aide de Babylon.GUI</a> par Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

Il s’agit d’un jeu 3D qui fonctionne bien sur un écran plat, mais qu'en est-il de la réalité virtuelle ?
Dans ce didacticiel, nous allons vous guider à travers les étapes nécessaires pour rendre ce jeu opérationnel avec WebVR. Nous allons utiliser un casque [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) capable d'utiliser la prise en charge supplémentaire pour WebVR dans Microsoft Edge. Une fois que nous aurons appliqué ces modifications au jeu,ce dernier fonctionnera avec d’autres combinaisons navigateur/casque prenant en charge WebVR.



## <a name="prerequisites"></a>Conditions préalables

- Un éditeur de texte (comme [Visual Studio Code](https://code.visualstudio.com/download))
- Une manette Xbox branchée à votre ordinateur
- Windows 10 Creators Update
- Un ordinateur doté [de la configuration minimale requise pour exécuter Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Un appareil Windows Mixed Reality (facultatif) 



## <a name="getting-started"></a>Prise en main

Le moyen le plus simple pour commencer est d'accéder à [Windows-tutorials-web GitHub repo](https://github.com/Microsoft/Windows-tutorials-web), d'appuyer sur le bouton vert **Cloner ou télécharger** et de sélectionner **Ouvrir dans Visual Studio**.

![Bouton Cloner ou télécharger](images/3dclone.png)

Si vous ne voulez pas cloner le projet, vous pouvez le télécharger en tant que fichier zip.
Vous disposerez ensuite de deux dossiers [avant](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) et [après ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). Le dossier « avant » correspond à notre jeu avant que les autres fonctionnalités VR soient ajoutées et le dossier « après » est le jeu terminé avec la prise en charge VR.

Les dossiers avant et après contiennent les fichiers suivants :
-   **textures /** : un dossier contenant les images utilisées dans le jeu.
-   **CSS /** : un dossier contenant le code CSS pour le jeu.
-   **js /** : un dossier contenant les fichiers JavaScript. Le fichier main.js est notre jeu, et les autres fichiers sont les bibliothèques utilisées.
-   **modèles /** : un dossier contenant les modèles 3D. Pour ce jeu nous n'avons qu'un modèle, pour le dinosaure.
-   **index.HTML** -la page Web qui héberge le moteur de rendu du jeu. Ouverture de cette page dans Microsoft Edge lance le jeu.

Vous pouvez tester les deux versions du jeu en ouvrant leurs fichiers index.html respectifs dans Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>Le Portail de réalité mixte

Si vous n’êtes pas familiarisé avec Windows Mixed Reality et que vous avez installé Windows 10 Creators Update sur un ordinateur doté d'une carte graphique compatible, essayez d’ouvrir l'application **Portail de réalité mixte** à partir du menu Démarrer de Windows 10.

![Recherche sur le Portail de réalité mixte](images/mixed-reality-portal.png)

Si vous remplissez toutes les conditions requises, vous pouvez activer des fonctionnalités de développement et simuler un casque Windows Mixed Reality branché à votre ordinateur. Si vous avez la chance de disposer d'un casque réel, branchez-le et exécutez le programme d’installation.

> [!IMPORTANT]
> Le portail de réalité mixte doit rester ouvert tout le long de ce didacticiel.

Vous êtes maintenant prêt pour expérimenter WebVR avec Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interface utilisateur 2D dans un monde virtuel

>[!NOTE]
> Saisissez le [ **avant** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) dossier pour obtenir l’exemple starter.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) est une bibliothèque VR conviviale, ce qui vous pour créer simple, qui fonctionnent bien pour VR d’interfaces utilisateur interactives et non-VR affiche.
Une extension à Babylon.js, le `GUI` bibliothèque est throuhout utilisé l’exemple pour créer des éléments 2D.


Un texte 2D `GUI` élément peut être créé avec quelques lignes en fonction des attributs combien vous souhaitez modifier.
L’extrait de code suivant est déjà notre [ **avant** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) échantillonner, mais nous allons procédure pas à pas, ce qui se passe.
Nous avons tout d’abord de rendre un [ `AdvancedDynamicTexture` ](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objet pour établir les éléments est traités dans l’interface utilisateur graphique. L’exemple définit cette valeur sur `CreateFullScreenUI()`, ce qui signifie que notre interface utilisateur couvre la totalité de l’écran. Avec `AdvancedDynamicTexture` créé, nous puis effectuez une zone de texte 2D qui s’affiche au démarrage du jeu à l’aide `GUI.Rectanlge()` et `GUI.TextBlock()`.


Ce code est ajouté au sein de [ **main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Cette interface utilisateur est visible une fois créé, mais peut être activé ou désactivé avec `isVisible` selon ce qui se passe dans le jeu.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Détection des casques

Il est conseillé pour les applications de réalité virtuelle d’avoir deux types d’appareils photo afin que plusieurs scénarios peuvent être pris en charge. Pour ce jeu, nous allons prendre en charge une caméra qui nécessite qu'un casque en bon état de marche soit connecté, et une autre caméra qui n’utilise pas de casque. Pour déterminer quelle caméra le jeu utilisera, nous devons vérifier tout d’abord si un casque a été détecté. Pour ce faire, nous allons utiliser [ `navigator.getVRDisplays()` ](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


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

Grâce aux informations stockées dans la variable `headset`, nous allons désormais pouvoir choisir la caméra qui convient à l’utilisateur.


## <a name="creating-and-selecting-the-initial-camera"></a>Créant et en sélectionnant la caméra initiale

Avec Babylon.js, WebVR peut être ajouté rapidement en utilisant le [ `WebVRFreeCamera` ](https://doc.babylonjs.com/classes/3.1/webvrfreecamera). Cette caméra peut capturer la saisie sur un clavier et vous permet d’utiliser un casque VR pour contrôler la rotation de votre « tête ».


### <a name="step-1-checking-for-headsets"></a>Étape 1 : Vérification des casques

Pour notre secours caméra, nous allons utiliser le [ `UniversalCamera` ](https://doc.babylonjs.com/classes/3.1/universalcamera) qui est actuellement utilisé dans le jeu d’origine.

Nous allons consulter notre `headset` variable pour déterminer si nous pouvons utiliser le `WebVRFreeCamera` appareil photo.

Remplacez `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);`par le code suivant.
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


### <a name="step-2-activating-the-webvrfreecamera"></a>Étape 2 : Activation de la WebVRFreeCamera
Pour activer cette caméra dans la plupart des navigateurs, l’utilisateur doit effectuer certaines interactions qui nécessitent une expérience visuelle.
Nous allons associer cette fonctionnalité jusqu'à un clic de souris.


Collez le code dans la fonction `createScene()` après `camera.applyGravity = true;`.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Un clic dans le jeu maintenant crée une invite semblable à la suivante, ou affiche le jeu dans le casque immédiatement si l’utilisateur a accepté l’invite avant.

![invite immersive](images/immersiveview.png)

Nous pouvons également ajouter un morceau de code qui affiche le `UniversalCamera` afficher avant de nous basculons vers notre `WebVRFreeCamera`, permettant à l’utilisateur d’examiner le jeu plutôt qu’une fenêtre bleu. 

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

### <a name="step-3-adding-gamepad-support"></a>Étape 3 : Ajout de la prise en charge du boîtier de commande

Dans la mesure où le `WebVRFreeCamera` initialement ne gère pas les boîtiers de commande, nous allons mapper nos boutons gamepad aux touches de direction du clavier. Nous allons le faire rentrer dans le `inputs` propriété de l’appareil photo. Nous ajoutons les codes associés afin que le stick analogique gauche haut, bas, gauche et droite corresponde aux touches de direction, ce qui rendra notre boîtier de commande opérationnel.


Ajoutez le code ci-dessous le `scene.onPointerDown = function() {...}` appeler.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Étape 4 : Faites un essai !

Si nous ouvrons **index.html** avec nos casque et le contrôleur de jeu branché, un clic gauche sur la fenêtre de jeu bleu passe notre jeu VR mode ! Lancez-vous et mettez votre casque pour vérifier les résultats. 


<iframe height='300' scrolling='no' title='Jeu de dino Babylon.js à l’aide de Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consultez le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js jeu de dino à l’aide de Babylon.GUI - WebVR</a> par Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusion

Félicitations ! Vous disposez maintenant d’un jeu Babylon.js complet avec prise en charge WebVR. Vous pouvez désormais vous servir de ce que vous avez appris pour créer un jeu encore mieux, ou améliorer celui-ci.
