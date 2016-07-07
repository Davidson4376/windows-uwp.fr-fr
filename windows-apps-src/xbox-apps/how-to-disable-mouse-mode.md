---
author: payzer
title: "Comment désactiver le mode souris"
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: bd7780f105f86d7d292c5db2535ad40af07d6e4f

---

# Comment désactiver le mode souris
Le modesouris est activé par défaut pour toutes les applications. Toutes les applications qui ne le refusent pas recevront un pointeur de souris (semblable à celui du navigateurMicrosoftEdge de la console). Nous vous recommandons vivement de désactiver cette fonctionnalité et d’optimiser l’application pour la navigation de contrôleur directionnelle.   
   
## HTML   
Pour désactiver le mode souris, ajoutez le code suivant dans un fichier JavaScript dans votre application:   
   
```code
navigator.gamepadInputEmulation = "keyboard";
```   

## XAML    
Pour désactiver le mode souris, ajoutez le code suivant dans un fichier JavaScript dans votre application:   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
        }
```

## C++/DirectX   
Si vous écrivez une application C++/DirectX, vous n’avez rien à faire. Le mode souris s’applique uniquement aux applications HTML et XAML.



<!--HONumber=Jun16_HO5-->


