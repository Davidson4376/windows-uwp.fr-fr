---
author: payzer
title: "Désactivation du mode souris"
description: "Instructions pour désactiver le mode souris par défaut."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 6848d1783df4489571fcf493a55447f67e5d6fd9
ms.lasthandoff: 02/08/2017

---

# <a name="how-to-disable-mouse-mode"></a>Désactivation du mode souris
Le mode souris est activé par défaut sur toutes les applications. Toutes les applications qui ne le refusent pas reçoivent un pointeur de souris (semblable à celui du navigateur Microsoft Edge de la console). Nous vous recommandons vivement de désactiver cette fonctionnalité et d’optimiser la navigation par commande directionnelle.   
   
## <a name="html"></a>HTML   
Pour activer la navigation par commande directionnelle dans une application de plateforme Windows universelle (UWP) JavaScript, utilisez la bibliothèque JavaScript de [navigation directionnelle TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Incluez le fichier JavaScript de navigation directionnelle dans le package de votre application, et ajoutez une référence à celui-ci dans toutes les pages HTML nécessitant une navigation par commande directionnelle :

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Pour plus d’informations, voir le [wiki de navigation directionnelle](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

Si vous voulez plutôt désactiver le mode souris et utiliser directement les API de boîtier de commande DOM ou WinRT, exécutez la commande suivante pour chaque page qui l’exige : 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   Par défaut, cette propriété a la valeur `mouse` qui active le mode souris. Si vous la définissez sur `keyboard`, le mode souris est désactivé et, à la place, l’entrée de boîtier de commande génère des événements de clavier DOM. Si vous la définissez sur `gamepad`, le mode souris est désactivé. Cela ne génère pas d’événements de clavier DOM, et vous pouvez seulement utiliser les API de boîtier de commande DOM ou WinRT.

## <a name="xaml"></a>XAML    
Pour désactiver le mode souris, ajoutez le code suivant au constructeur de votre application :   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
Si vous écrivez une application C++/DirectX, vous n’avez rien à faire. Le mode souris s’applique uniquement aux applications HTML et XAML.

## <a name="see-also"></a>Voir également
- [Bonnes pratiques pour Xbox](tailoring-for-xbox.md)
- [UWP sur Xbox One](index.md)


