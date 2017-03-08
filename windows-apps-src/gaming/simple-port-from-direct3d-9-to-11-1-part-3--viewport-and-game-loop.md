---
author: mtoepke
title: Porter la boucle de jeu
description: "Montre comment implémenter une fenêtre pour un jeu de plateforme Windows universelle (UWP) et comment récupérer la boucle de jeu, notamment comment créer une interface IFrameworkView pour contrôler une classe CoreWindow en plein écran."
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jeux, portage, boucle de jeu, direct3d 9, directx 11
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 23631bf464095e1d2f2aab97740d89c6a82f4a70
ms.lasthandoff: 02/07/2017

---

# <a name="port-the-game-loop"></a>Porter la boucle de jeu


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132).\]

**Récapitulatif**

-   [Partie 1 : initialiser Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Partie 2 : convertir l’infrastructure de rendu](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Partie 3 : porter la boucle de jeu


Montre comment implémenter une fenêtre pour un jeu de plateforme Windows universelle (UWP) et comment récupérer la boucle de jeu, notamment comment créer une interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) pour contrôler une classe [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) en plein écran. Partie 3 de la procédure pas à pas [Porter une application Direct3D 9 simple vers DirectX 11 et UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="create-a-window"></a>Créer une fenêtre


Pour configurer une fenêtre de bureau avec une fenêtre d’affichage Direct3D 9, nous devions implémenter l’infrastructure de fenêtrage traditionnelle des applications de bureau. Nous devions créer un HWND, définir la taille de la fenêtre, fournir un rappel de traitement de fenêtre, le rendre visible, etc.

L’environnement UWP possède un système beaucoup plus simple. Au lieu de configurer une fenêtre traditionnelle, un jeu du Windows Store qui utilise DirectX implémente l’interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478). Cette interface existe pour les applications et jeux DirectX pour s’exécuter directement dans une classe [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) à l’intérieur du conteneur d’application.

> **Remarque** Windows fournit des pointeurs gérés aux ressources tels que l’objet d’application source et la classe [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Voir [**Handle sur l’opérateur Object (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

Votre « principale » classe doit hériter de l’interface [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) et implémenter les cinq méthodes **IFrameworkView** : [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) et [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523). En plus de créer l’interface **IFrameworkView**, qui correspond (essentiellement) à l’emplacement auquel votre jeu va résider, vous devez implémenter une classe de fabrique qui crée une instance de votre interface **IFrameworkView**. Votre jeu possède quand même un exécutable avec une méthode appelée **main()**, mais celle-ci peut uniquement utiliser la fabrique pour créer l’instance **IFrameworkView**.

Fonction main

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Fabrique IFrameworkView

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>Porter la boucle de jeu


Examinons la boucle de jeu de notre implémentation Direct3D 9. Ce code existe dans la fonction main de l’application. Chaque itération de cette boucle traite un message de fenêtre ou génère le rendu d’une image.

Boucle de jeu dans un jeu sur ordinateur Direct3D 9

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

La boucle de jeu est similaire, mais plus facile, dans la version UWP de notre jeu :

La boucle de jeu va dans la méthode [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) (plutôt que **main()**) car notre jeu fonctionne au sein de la classe [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478).

Au lieu d’implémenter une infrastructure de gestion des messages et d’appeler la fonction [**PeekMessage**](https://msdn.microsoft.com/library/windows/desktop/ms644943), nous pouvons appeler la méthode [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) intégrée dans la classe [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) de notre fenêtre d’application. La boucle de jeu n’a pas besoin de se ramifier et de gérer les messages : il suffit d’appeler la méthode **ProcessEvents** et de continuer.

Boucle de jeu dans un jeu du Windows Store Direct3D 11

```cpp
// Windows Store apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

Nous avons à présent une application UWP qui configure la même infrastructure graphique de base, et génère le rendu du même cube haut en couleur que celui de notre exemple DirectX 9.

## <a name="where-do-i-go-from-here"></a>Où aller à partir d’ici ?


Marquez d’un signet le [Forum Aux Questions (FAQ) sur le portage DirectX 11](directx-porting-faq.md).

Les modèles UWP DirectX incluent une infrastructure de périphérique Direct3D robuste prête à l’emploi avec votre jeu UWP. Pour obtenir des conseils sur la sélection du modèle approprié, voir [Créer un projet de jeu DirectX à partir d’un modèle](user-interface.md).

Consultez les articles détaillés suivants sur le développement des jeux du Windows Store :

-   [Procédure pas à pas : jeu UWP simple avec DirectX](tutorial--create-your-first-metro-style-directx-game.md)
-   [Audio pour les jeux](working-with-audio-in-your-directx-game.md)
-   [Contrôles de déplacement/vue pour les jeux](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 





