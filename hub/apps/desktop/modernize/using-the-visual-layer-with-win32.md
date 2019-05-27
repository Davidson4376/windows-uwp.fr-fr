---
title: À l’aide de la couche visuelle avec Win32
description: Utiliser la couche visuelle pour améliorer l’interface utilisateur de votre application de bureau Win32.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, de rendu, composition, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: c9b4ec38b0dd1f6eca3f43cfded74c6292c08100
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215188"
---
# <a name="using-the-visual-layer-with-win32"></a>À l’aide de la couche visuelle avec Win32

Vous pouvez utiliser les API de Composition Windows Runtime (également appelé le [couche visuelle](/windows/uwp/composition/visual-layer)) dans vos applications Win32 pour créer des expériences modernes autrement clair pour les utilisateurs de Windows 10.

Le code complet pour ce didacticiel est disponible sur GitHub : [Exemple de Win32 HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Les Applications Windows universelles nécessitant un contrôle précis sur leur composition de l’interface utilisateur ont accès à la [Windows.UI.Composition](/uwp/api/windows.ui.composition) exercer un contrôle précis comment leur interface utilisateur est composée et rendu de l’espace de noms. Cette API de composition n’est pas limitée aux applications UWP, toutefois. Les applications de bureau Win32 peuvent tirer parti des systèmes modernes de composition dans UWP et Windows 10.

## <a name="prerequisites"></a>Prérequis

La plateforme Windows universelle API d’hébergement a ces conditions préalables.

- Nous partons du principe que vous disposez des connaissances sur le développement d’applications à l’aide de Win32 et UWP. Pour plus d’informations, voir :
  - [Prise en main Win32 etC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Prise en main les applications Windows 10](/windows/uwp/get-started/)
  - [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10 version 1803 ou ultérieure
- SDK Windows 10 17134 ou version ultérieure

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Comment utiliser les API de Composition à partir d’une application de bureau Win32

Dans ce didacticiel, vous allez créer un simple Win32 C++ application et lui ajouter des éléments de Composition de UWP. Le focus est sur la configuration du projet, création du code interop et quelque chose de simple à l’aide des API de Composition Windows correctement. L’application terminée ressemble à ceci.

![L’application en cours d’exécution de l’interface utilisateur](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Créer un C++ projet Win32 dans Visual Studio

La première étape consiste à créer le projet d’application Win32 dans Visual Studio.

Pour créer un nouveau projet d’Application Win32 dans C++ nommé _HelloComposition_:

1. Ouvrez Visual Studio et sélectionnez **fichier** > **New** > **projet**.

    Le **nouveau projet** boîte de dialogue s’ouvre.
1. Sous le **installé** catégorie, développez le **Visual C++**  nœud, puis sélectionnez **Windows Desktop**.
1. Sélectionnez le **Application de bureau Windows** modèle.
1. Entrez le nom _HelloComposition_, puis cliquez sur **OK**.

    Visual Studio crée le projet et ouvre l’éditeur pour le fichier d’application principale.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurer le projet pour utiliser Windows Runtime APIs

Pour utiliser Windows Runtime (WinRT) API dans votre application Win32, nous utilisons C++/WinRT. Vous devez configurer votre projet Visual Studio pour ajouter C++prise en charge /WinRT.

(Pour plus d’informations, consultez [prise en main C++/WinRT - modifier un projet d’application de bureau de Windows pour ajouter C++prise en charge /WinRT](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. À partir de la **projet** menu, ouvrez les propriétés du projet (_HelloComposition propriétés_) et vérifiez les paramètres suivants sont définis sur les valeurs spécifiées :

    - Pour **Configuration**, sélectionnez _toutes les Configurations_. Pour **plateforme**, sélectionnez _toutes les plateformes_.
    - **Propriétés de configuration** > **général** > **Windows SDK Version** = _10.0.17763.0_ ou supérieur

    ![Définir la version SDK](images/visual-layer-interop/sdk-version.png)

    - **C /C++** > **langage**  >   **C++ langage Standard** = _ISO C++ 17 Standard (/ stf:c ++ 17)_

    ![Norme du langage de jeu](images/visual-layer-interop/language-standard.png)

    - **L’éditeur de liens** > **entrée** > **dépendances supplémentaires** doit inclure «_windowsapp.lib_». S’il n’est pas inclus dans la liste, ajoutez-le.

    ![Ajouter une dépendance de l’éditeur de liens](images/visual-layer-interop/linker-dependencies.png)

1. Mettre à jour de l’en-tête précompilé

    - Renommer `stdafx.h` et `stdafx.cpp` à `pch.h` et `pch.cpp`, respectivement.
    - Définir la propriété de projet **C/C++** > **en-têtes précompilés** > **fichier d’en-tête précompilé** à *pch.h*.
    - Rechercher et remplacer `#include "stdafx.h"` avec `#include "pch.h"` dans tous les fichiers.

        (**Modifier** > **rechercher et remplacer** > **rechercher dans les fichiers**)

        ![Rechercher et remplacer stdafx.h](images/visual-layer-interop/replace-stdafx.png)

    - Dans `pch.h`, inclure `winrt/base.h` et `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

Il est judicieux de créer le projet à ce stade pour vérifier qu’il n’y pas d’erreurs avant de poursuivre.

## <a name="create-a-class-to-host-composition-elements"></a>Créer une classe pour les éléments de composition d’hôte

Pour héberger le contenu vous créer avec la couche visuelle, créer une classe (_CompositionHost_) pour gérer l’interopérabilité et de créer des éléments de composition. Il s’agit dans lequel vous effectuez la majeure partie de la configuration pour l’hébergement des API de Composition, y compris :

- obtenir un [compositeur](/uwp/api/windows.ui.composition.compositor), ce qui crée et gère les objets dans le [Windows.UI.Composition](/uwp/api/windows.ui.composition) espace de noms.
- Création d’un [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) pour gérer les tâches pour les APIs WinRT.
- Création d’un [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) et conteneur de Composition pour afficher les objets de la composition.

Nous en faire un singleton pour éviter les problèmes liés aux threads de classe. Par exemple, vous pouvez uniquement créer une file d’attente du répartiteur par thread, de l’instanciation d’une deuxième instance de CompositionHost sur le même thread engendre une erreur.

> [!TIP]
> Si vous le souhaitez, vérifiez le code complet à la fin du didacticiel pour vous assurer que tout le code est placées au bon endroit lorsque vous travaillez dans le didacticiel.

1. Ajoutez un nouveau fichier de classe à votre projet.
    - Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le _HelloComposition_ projet.
    - Dans le menu contextuel, sélectionnez **ajouter** > **classe...** .
    - Dans le **ajouter une classe** boîte de dialogue, nommez la classe _CompositionHost.cs_, puis cliquez sur **ajouter**.

1. Inclure des en-têtes et _les instructions using_ requis pour l’interopérabilité de composition.
    - Dans CompositionHost.h, ajoutez ces _inclut_ en haut du fichier.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - Dans CompositionHost.cpp, ajoutez ces _les instructions using_ en haut du fichier, après n’importe quel _inclut_.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Modifier la classe pour utiliser le modèle de singleton.
    - Dans CompositionHost.h, rendez le constructeur privé.
    - Déclarer statique publique _GetInstance_ (méthode).

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la _GetInstance_ (méthode).

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. Dans CompositionHost.h, déclarez membre privé variables pour le compositeur, DispatcherQueueController et DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Ajoutez une méthode publique pour initialiser les objets d’interopérabilité de composition.
    > [!NOTE]
    > Dans _initialiser_, vous appelez le _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_, et _CreateCompositionRoot_ méthodes. Vous créez ces méthodes dans les étapes suivantes.

    - Dans CompositionHost.h, déclarez une méthode publique nommée _initialiser_ qui accepte un HWND en tant qu’argument.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la _initialiser_ (méthode).

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Créer une file d’attente du répartiteur sur le thread qui utiliseront la Composition de Windows.

    Un compositeur doit être créé sur un thread qui a une file d’attente du répartiteur, donc cette méthode est appelée en premier lors de l’initialisation.

    - Dans CompositionHost.h, déclarez une méthode privée nommée _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la _EnsureDispatcherQueue_ (méthode).

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. Inscrivez votre fenêtre d’application comme une cible de composition.
    - Dans CompositionHost.h, déclarez une méthode privée nommée _CreateDesktopWindowTarget_ qui accepte un HWND en tant qu’argument.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la _CreateDesktopWindowTarget_ (méthode).

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. Créer un conteneur visuel racine pour contenir les objets visuels.
    - Dans CompositionHost.h, déclarez une méthode privée nommée _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la _CreateCompositionRoot_ (méthode).

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

Générons maintenant le projet pour vérifier qu’aucune erreur.

Ces méthodes, configurer les composants nécessaires pour l’interopérabilité entre la couche visuelle UWP et des API Win32. Vous pouvez maintenant ajouter du contenu à votre application.

### <a name="add-composition-elements"></a>Ajouter des éléments de composition

Avec l’infrastructure en place, vous pouvez maintenant générer le contenu de Composition que vous souhaitez afficher.

Pour cet exemple, vous ajoutez le code qui crée un carré de couleur au hasard [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) avec une animation qui oblige ce dernier à supprimer après un court délai.

1. Ajouter un élément de composition.
    - Dans CompositionHost.h, déclarez une méthode publique nommée _AddElement_ qui prend 3 **float** valeurs comme arguments.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - Dans CompositionHost.cpp, ajoutez la définition de la _AddElement_ (méthode).

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Créer et afficher la fenêtre

Maintenant, vous pouvez ajouter un bouton et le contenu de la composition UWP à votre interface utilisateur Win32.

1. Dans HelloComposition.cpp, en haut du fichier, incluez _CompositionHost.h_BTN_ADD de définir et obtenir une instance de CompositionHost.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. Dans le `InitInstance` (méthode), modifier la taille de la fenêtre qui est créée. (Dans cette ligne, remplacez `CW_USEDEFAULT, 0` à `900, 672`.)

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. Dans la fonction WndProc, ajoutez `case WM_CREATE` à la _message_ bloc switch. Dans ce cas, vous initialisez le CompositionHost et créez le bouton.

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. Également dans la fonction WndProc, gère le clic de bouton pour ajouter un élément de composition à l’interface utilisateur. 

    Ajouter `case BTN_ADD` à la _wmId_ bloc switch à l’intérieur du bloc WM_COMMAND.

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

Vous pouvez maintenant générer et exécuter votre application. Si vous le souhaitez, vérifiez le code complet à la fin du didacticiel afin de vous assurer que tout le code est placées au bon endroit.

Lorsque vous exécutez l’application et cliquez sur le bouton, vous devez voir des carrés animés ajoutés à l’interface utilisateur.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Exemple Win32 HelloComposition (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Prise en main Win32 etC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Prise en main les applications Windows 10](/windows/uwp/get-started/) (UWP)
- [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espace de noms Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Code complet

Voici le code complet pour la classe CompositionHost et la méthode InitInstance.

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (partielle)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```