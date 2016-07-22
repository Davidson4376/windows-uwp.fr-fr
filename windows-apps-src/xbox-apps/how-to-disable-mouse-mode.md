---
author: payzer
title: "Comment désactiver le mode souris"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 6f4719c98d490cdcac8c799c4c68af55b217cbc5
ms.openlocfilehash: d1ee946693b9f9714b8d570b8ae3718469d2c10d

---

# Comment désactiver le mode souris
Le modesouris est activé par défaut pour toutes les applications. Toutes les applications qui ne le refusent pas recevront un pointeur de souris (semblable à celui du navigateurMicrosoftEdge de la console). Nous vous recommandons vivement de désactiver cette fonctionnalité et d’optimiser l’application pour la navigation de contrôleur directionnelle.   
   
## HTML   
Pour activer la navigation de contrôleur directionnelle dans une application UWP JavaScript, utilisez la bibliothèque JavaScript de [navigation directionnelle TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Incluez le fichier JavaScript de navigation directionnelle dans le package de votre application, et ajoutez une référence à celui-ci dans toutes les pages HTML qui requièrent une navigation de contrôleur directionnelle:
```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Voir le [wiki de navigation directionnelle](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) pour des détails complémentaires.

Si vous voulez plutôt désactiver le mode souris et utiliser directement les API de boîtier de commande DOM ou WinRT, exécutez la commande suivante pour chaque page qui l’exige: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

Par défaut, cette propriété a la valeur ```'mouse'``` qui active le mode souris. Si vous la définissez sur ```'keyboard'```, le mode souris est désactivé et, à la place, l’entrée de boîtier de commande génère des événements de clavier DOM. Si vous la définissez sur ```'gamepad'```, le mode souris est désactivé. Cela ne génère pas d’événements de clavier DOM, et vous pouvez seulement utiliser les API de boîtier de commande DOM ou WinRT.

## XAML    
Pour désactiver le mode souris, ajoutez le code suivant au constructeur de votre application:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
Si vous écrivez une application C++/DirectX, vous n’avez rien à faire. Le mode souris s’applique uniquement aux applications HTML et XAML.



<!--HONumber=Jul16_HO1-->


