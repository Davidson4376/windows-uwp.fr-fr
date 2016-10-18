---
author: payzer
title: "Comment désactiver le mode souris"
description: "Instructions pour désactiver le mode souris par défaut."
translationtype: Human Translation
ms.sourcegitcommit: b4df1f944d909640791e4ed7e3bcf8d8bdf7a0d1
ms.openlocfilehash: 91e530a3313d53c4e693b88a64b849f3188a72de

---

# Comment désactiver le mode souris
Le mode souris est activé par défaut sur toutes les applications. Toutes les applications qui ne le refusent pas reçoivent un pointeur de souris (semblable à celui du navigateurMicrosoftEdge de la console). Nous vous recommandons vivement de désactiver cette fonctionnalité et d’optimiser la navigation par commande directionnelle.   
   
## HTML   
Pour activer la navigation par commande directionnelle dans une application de plateforme Windows universelle (UWP) JavaScript, utilisez la bibliothèque JavaScript de [navigation directionnelle TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Incluez le fichier JavaScript de navigation directionnelle dans le package de votre application, et ajoutez une référence à celui-ci dans toutes les pages HTML nécessitant une navigation par commande directionnelle:

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Pour plus d’informations, voir le [wiki de navigation directionnelle](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

Si vous voulez plutôt désactiver le mode souris et utiliser directement les API de boîtier de commande DOM ou WinRT, exécutez la commande suivante pour chaque page qui l’exige: 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   Par défaut, cette propriété a la valeur `mouse` qui active le mode souris. Si vous la définissez sur `keyboard`, le mode souris est désactivé et, à la place, l’entrée de boîtier de commande génère des événements de clavier DOM. Si vous la définissez sur `gamepad`, le mode souris est désactivé. Cela ne génère pas d’événements de clavier DOM, et vous pouvez seulement utiliser les API de boîtier de commande DOM ou WinRT.

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

## Voir également
- [Bonnes pratiques pourXbox](tailoring-for-xbox.md)
- [UWP sur XboxOne](index.md)




<!--HONumber=Aug16_HO3-->


