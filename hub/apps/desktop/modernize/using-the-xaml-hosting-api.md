---
description: Cet article explique comment héberger une interface utilisateur XAML UWP dans C++ votre application Desktop Win32.
title: Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, îlots XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: cdcef66dc1f0026ff369eeb3f3c7881385d6e5ba
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339292"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Utilisation de l’API d’hébergement XAML UWP dans une application Win32 C++

À compter de Windows 10, version 1903, les applications de bureau non UWP C++ (y compris les applications Win32, WPF et Windows Forms) peuvent utiliser l' *API d’hébergement XAML UWP* pour héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur associé à un handle de fenêtre (HWND). Cette API permet aux applications de bureau non UWP d’utiliser les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Par exemple, les applications de bureau non UWP peuvent utiliser cette API pour héberger des contrôles UWP qui utilisent le [système de conception Fluent](/windows/uwp/design/fluent-design-system/index) et prennent en charge [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

L’API d’hébergement XAML UWP fournit la base d’un ensemble plus large de contrôles que nous fournissons pour permettre aux développeurs d’intégrer l’interface utilisateur Fluent à des applications de bureau non UWP. Cette fonctionnalité est appelée *îlots XAML*. Pour obtenir une vue d’ensemble de cette fonctionnalité, consultez [héberger des contrôles XAML UWP dans les applications de bureau (îlots XAML)](xaml-islands.md).

> [!NOTE]
> Si vous avez des commentaires sur les îlots XAML, créez un nouveau problème dans [Microsoft. Toolkit. Win32 référentiel](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez vos commentaires à cet endroit. Si vous préférez soumettre vos commentaires en privé, vous pouvez les envoyer à XamlIslandsFeedback@microsoft.com. Vos Insights et scénarios sont très importants pour nous.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Devez-vous utiliser l’API d’hébergement XAML UWP ?

L’API d’hébergement XAML UWP fournit l’infrastructure de bas niveau pour l’hébergement de contrôles UWP dans les applications de bureau. Certains types d’applications de bureau ont la possibilité d’utiliser des API alternatives et plus pratiques pour atteindre cet objectif.  

* Si vous disposez d' C++ une application de bureau Win32 et souhaitez héberger des contrôles UWP dans votre application, vous devez utiliser l’API d’hébergement XAML UWP. Il n’existe aucune solution de remplacement pour ces types d’applications.

* Pour les applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser les [contrôles .net Island](xaml-islands.md#wpf-and-windows-forms-applications) dans le kit de pratiques de la communauté Windows au lieu d’utiliser directement l’API d’hébergement XAML UWP. Ces contrôles utilisent l’API d’hébergement XAML UWP en interne et implémentent tout le comportement que vous devrez sinon gérer vous-même si vous utilisiez l’API d’hébergement XAML UWP directement, y compris la navigation au clavier et les modifications de disposition.

Étant donné que nous recommandons C++ que seules les applications Win32 utilisent l’API d’hébergement XAML UWP, cet article fournit principalement C++ des instructions et des exemples pour les applications Win32. Toutefois, vous pouvez utiliser l’API d’hébergement XAML UWP dans WPF et les applications de Windows Forms si vous le souhaitez. Cet article pointe vers le code source pertinent pour les [contrôles hôtes](xaml-islands.md#host-controls) pour WPF et Windows Forms dans la boîte à outils de la communauté Windows afin que vous puissiez voir comment l’API d’hébergement XAML UWP est utilisée par ces contrôles.

## <a name="prerequisites"></a>Prérequis

Les îlots XAML requièrent Windows 10, version 1903 (ou ultérieure) et la Build correspondante du SDK Windows. Pour utiliser des îlots XAML C++ dans votre application Win32, vous devez d’abord configurer votre projet.

### <a name="support-for-cwinrt"></a>Prise en C++charge de/WinRT

Assurez-vous que votre projet prend en charge [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis):

* Pour les nouveaux projets, vous pouvez installer l' [ C++extension Visual Studio/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) et utiliser l’un C++des modèles de projet/WinRT inclus dans cette extension.
* Pour les projets existants, vous pouvez installer le package NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) dans le projet.

Pour plus d’informations sur ces options, consultez [cet article](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="configure-your-project-for-app-deployment"></a>Configurer votre projet pour le déploiement d’applications

Choisissez l’une des options suivantes pour préparer votre projet au déploiement :

* **Empaquetez votre application dans un package MSIX**. L’empaquetage de votre application dans un [package MSIX](https://docs.microsoft.com/windows/msix/) fournit de nombreux avantages en matière de déploiement et d’exécution.
    1. Installez le kit de développement logiciel (SDK) Windows 10, version 1903 (version 10.0.18362) ou une version ultérieure.
    2. Empaquetez votre application dans un package MSIX en ajoutant un [projet de packaging des applications Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution et en C++ajoutant une référence à votre projet/Win32.

* **Installez le package Microsoft. Toolkit. Win32. UI. SDK**. Si vous ne souhaitez pas empaqueter votre application dans un package MSIX, vous pouvez installer [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (version v 6.0.0-preview7 ou version ultérieure). Ce package fournit plusieurs ressources de génération et d’exécution qui permettent aux îlots XAML de fonctionner dans votre application. Assurez-vous que l’option **inclure la version préliminaire** est sélectionnée afin que vous puissiez voir les dernières versions préliminaires de ce package.

> [!NOTE]
> Dans les versions antérieures de ces instructions, vous `maxversiontested` aviez ajouté l’élément à un manifeste d’application de votre projet. Tant que vous utilisez l’une des options mentionnées ci-dessus, vous n’avez plus besoin d’ajouter cet élément à votre manifeste.

### <a name="additional-requirements-for-custom-uwp-controls"></a>Exigences supplémentaires pour les contrôles UWP personnalisés

Si vous hébergez un contrôle UWP personnalisé (par exemple, un contrôle utilisateur qui se compose de plusieurs contrôles UWP qui fonctionnent ensemble), vous devez suivre les instructions supplémentaires de [cette section](#host-a-custom-uwp-control). Vous devez également disposer du code source du contrôle personnalisé pour pouvoir le compiler avec votre application.

## <a name="architecture-of-the-api"></a>Architecture de l’API

L’API d’hébergement XAML UWP comprend ces principaux types de Windows Runtime et interfaces COM.

|  Type ou interface | Description |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Cette classe représente l’infrastructure XAML UWP. Cette classe fournit une méthode [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) statique unique qui initialise l’infrastructure XAML UWP sur le thread actuel de l’application de bureau. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Cette classe représente une instance du contenu XAML UWP que vous hébergez dans votre application de bureau. Le membre le plus important de cette classe est la propriété de [contenu](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Vous assignez cette propriété à un [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que vous souhaitez héberger. Cette classe a également d’autres membres pour le routage du focus clavier vers et à partir des îlots XAML. |
| IDesktopWindowXamlSourceNative | Cette interface COM fournit la méthode **AttachToWindow** , que vous utilisez pour attacher un îlot XAML dans votre application à un élément d’interface utilisateur parent. Chaque objet **DesktopWindowXamlSource** implémente cette interface. |
| IDesktopWindowXamlSourceNative2 | Cette interface COM fournit la méthode **PreTranslateMessage** , qui permet à l’infrastructure XAML UWP de traiter correctement certains messages Windows. Chaque objet **DesktopWindowXamlSource** implémente cette interface. |

Le diagramme suivant illustre la hiérarchie d’objets dans un îlot XAML hébergé dans une application de bureau.

* Au niveau de la base se trouve l’élément d’interface utilisateur dans votre application où vous souhaitez héberger l’îlot XAML. Cet élément d’interface utilisateur doit avoir un handle de fenêtre (HWND). Voici des exemples d’éléments d’interface utilisateur dans lesquels vous pouvez héberger un îlot C++ XAML : une [fenêtre](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) pour les applications C++ Win32, un [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) pour les applications WPF et System [Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) pour les applications Windows Forms.

* Au niveau suivant, il s’agit d’un objet **DesktopWindowXamlSource** . Cet objet fournit l’infrastructure pour l’hébergement de l’îlot XAML. Votre code est chargé de créer cet objet et de l’attacher à l’élément d’interface utilisateur parent.

* Lorsque vous créez un **DesktopWindowXamlSource**, cet objet crée automatiquement une fenêtre enfant native pour héberger votre contrôle UWP. Cette fenêtre enfant native est principalement abstraite de votre code, mais vous pouvez accéder à son handle (HWND) si nécessaire.

* Enfin, au niveau supérieur, il s’agit du contrôle UWP que vous souhaitez héberger dans votre application de bureau. Il peut s’agir de n’importe quel objet UWP dérivé de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris tout contrôle UWP fourni par le SDK Windows, ainsi que des contrôles utilisateur personnalisés.

![Architecture DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>Exemples connexes

La façon dont vous utilisez l’API d’hébergement XAML UWP dans votre code dépend de votre type d’application, de la conception de votre application et d’autres facteurs. Pour illustrer l’utilisation de cette API dans le contexte d’une application complète, cet article fait référence au code des exemples suivants.

### <a name="c-win32"></a>C++32

Les exemples suivants montrent comment utiliser l’API d’hébergement XAML UWP dans une C++ application Win32 :

* [Exemple d’îlot XAML simple](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp). Cet exemple illustre une implémentation de base de l’hébergement d’un contrôle UWP dans une C++ application Win32 non empaquetée (autrement dit, une application qui n’est pas intégrée à un package MSIX).

* [Îlot XAML avec exemple de contrôle personnalisé](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App). Cet exemple illustre une implémentation complète de l’hébergement d’un contrôle UWP personnalisé dans une application C++ Win32 non empaquetée, ainsi que la gestion d’autres comportements tels que l’entrée au clavier et la navigation dans le focus. 

### <a name="wpf-and-windows-forms"></a>WPF et Windows Forms

Le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) de la boîte à outils de la communauté Windows sert d’exemple de référence pour l’utilisation de l’API d’hébergement UWP dans WPF et les applications de Windows Forms. Le code source est disponible aux emplacements suivants :

* Pour la version WPF du contrôle, [cliquez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La version WPF dérive de [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Pour obtenir la version Windows Forms du contrôle, [cliquez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La version Windows Forms dérive de [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-a-standard-uwp-control"></a>Héberger un contrôle UWP standard

Cette section vous guide tout au long du processus d’utilisation de l’API d’hébergement XAML UWP pour héberger un contrôle UWP standard (autrement dit, un contrôle fourni par la bibliothèque SDK Windows ou C++ WinUI) dans une nouvelle application Win32. Le code est basé sur l' [exemple d’îlot XAML simple](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp), et cette section décrit quelques-unes des parties les plus importantes du code. Si vous disposez déjà C++ d’un projet d’application Win32, vous pouvez adapter ces étapes et exemples de code pour votre projet.

### <a name="configure-the-project"></a>Configurer le projet

1. Dans Visual Studio 2019 avec le kit de développement logiciel (SDK) Windows 10, version 1903 (version 10.0.18362) ou une version ultérieure installée, créez un projet d' **application de bureau Windows** . Ce projet est disponible dans les **C++** filtres de projet, **Windows**et de **Bureau** .

2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, cliquez sur **recibler la solution**, sélectionnez le **10.0.18362.0** ou une version ultérieure du kit de développement logiciel (SDK), puis cliquez sur **OK**.

3. Installez le package NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) :

    1. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur votre projet et choisissez **gérer les packages NuGet**.
    2. Sélectionnez l’onglet **Parcourir** , recherchez le package [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) et installez la dernière version de ce package.

4. Installez le package NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) :

    1. Dans la fenêtre **Gestionnaire de package NuGet** , assurez-vous que l’option **inclure la version préliminaire** est sélectionnée.
    2. Sélectionnez l’onglet **Parcourir** , recherchez le package [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) et installez la version v 6.0.0-preview7 (ou ultérieure) de ce package.

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Utiliser l’API d’hébergement XAML pour héberger un contrôle UWP

Le processus de base de l’utilisation de l’API d’hébergement XAML pour héberger un contrôle UWP suit ces étapes générales :

1. Initialisez l’infrastructure XAML UWP pour le thread actuel avant que votre application ne crée l’un des objets [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) qu’il hébergera. Il existe plusieurs façons de procéder, selon le moment où vous envisagez de créer l’objet [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) qui hébergera les contrôles.

    * Si votre application crée l’objet **DesktopWindowXamlSource** avant de créer l’un des objets **Windows. UI. Xaml. UIElement** qu’il hébergera, cette infrastructure sera initialisée pour vous lorsque vous instancierez le  **Objet DesktopWindowXamlSource** . Dans ce scénario, vous n’avez pas besoin d’ajouter le code de votre choix pour initialiser le Framework.

    * Toutefois, si votre application crée les objets **Windows. UI. Xaml. UIElement** avant de créer l’objet **DesktopWindowXamlSource** qui va les héberger, votre application doit appeler la [ Méthode WindowsXamlManager. InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) pour initialiser explicitement l’infrastructure XAML UWP avant l’instanciation des objets **Windows. UI. Xaml. UIElement** . Votre application doit généralement appeler cette méthode lorsque l’élément d’interface utilisateur parent qui héberge le **DesktopWindowXamlSource** est instancié.

    > [!NOTE]
    > Cette méthode retourne un objet [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) qui contient une référence à l’infrastructure XAML UWP. Vous pouvez créer autant d’objets **WindowsXamlManager** que vous le souhaitez sur un thread donné. Toutefois, étant donné que chaque objet contient une référence à l’infrastructure XAML UWP, vous devez supprimer les objets pour vous assurer que les ressources XAML sont libérées.

2. Créez un objet [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) et attachez-le à un élément d’interface utilisateur parent dans votre application associée à un handle de fenêtre.

    Pour ce faire, vous devez suivre les étapes ci-dessous :

    1. Créez un objet **DesktopWindowXamlSource** et effectuez un cast de celui-ci en interface com **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .
        > [!NOTE]
        > Ces interfaces sont déclarées dans le fichier d’en-tête **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h** dans le SDK Windows. Par défaut, ce fichier se trouve dans% ProgramFiles (x86)% \ Windows Kits\10\Include @ no__t-0 < numéro de build @ no__t-1\um.

    2. Appelez la méthode **AttachToWindow** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** , puis transmettez le handle de fenêtre de l’élément d’interface utilisateur parent dans votre application.

    3. Définissez la taille initiale de la fenêtre enfant interne contenue dans le **DesktopWindowXamlSource**. Par défaut, cette fenêtre enfant interne est définie sur une largeur et une hauteur de 0. Si vous ne définissez pas la taille de la fenêtre, les contrôles UWP que vous ajoutez au **DesktopWindowXamlSource** ne seront pas visibles. Pour accéder à la fenêtre enfant interne dans **DesktopWindowXamlSource**, utilisez la propriété **WindowHandle** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .

3. Enfin, assignez le **Windows. UI. Xaml. UIElement** que vous souhaitez héberger à la propriété de [contenu](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) de votre objet **DesktopWindowXamlSource** .

Les étapes et les exemples de code suivants montrent comment implémenter le processus ci-dessus :

1. Dans le dossier **fichiers sources** du projet, ouvrez le fichier **WindowsProject. cpp** par défaut. Supprimez tout le contenu du fichier et ajoutez les instructions `include` et `using` suivantes. En plus des en C++ -têtes et des espaces de noms standard et UWP, ces instructions incluent plusieurs éléments spécifiques aux îlots XAML.

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. Copiez le code suivant après la section précédente. Ce code définit la fonction **WinMain** pour l’application. Cette fonction Initialise une fenêtre de base et utilise l’API d’hébergement XAML pour héberger un contrôle **TextBlock TextBlock** simple dans la fenêtre.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. Copiez le code suivant après la section précédente. Ce code définit la [procédure de fenêtre](https://docs.microsoft.com/en-us/windows/win32/learnwin32/writing-the-window-procedure) pour la fenêtre.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. Enregistrez le fichier de code, puis générez et exécutez l’application. Vérifiez que vous voyez le contrôle de **TEXTBLOCK** UWP dans la fenêtre de l’application.
    > [!NOTE]
    > Vous pouvez voir les différents avertissements de build, `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` y `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`compris et. Ces avertissements sont des problèmes connus avec les outils actuels et les packages NuGet, et ils peuvent être ignorés.

Pour obtenir des exemples complets qui illustrent ces tâches, consultez les fichiers de code suivants :

* **C++32**
  * Consultez le fichier [HelloWindowsDesktop. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_SimpleApp/Win32DesktopApp/HelloWindowsDesktop.cpp) dans l' [exemple d’îlot XAML simple](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp).
  * Consultez le fichier [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) dans l' [exemple d’îlot XAML avec contrôle personnalisé](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

* **WPF** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) dans la boîte à outils de la communauté Windows.  

* **Windows Forms :** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) dans la boîte à outils de la communauté Windows.

## <a name="host-a-custom-uwp-control"></a>Héberger un contrôle UWP personnalisé

Le processus d’hébergement d’un contrôle UWP personnalisé (soit un contrôle que vous définissez vous-même ou un contrôle fourni par un tiers C++ ) dans une application Win32 commence par les mêmes étapes que l' [hébergement d’un contrôle standard](#host-a-standard-uwp-control). Toutefois, l’hébergement d’un contrôle UWP personnalisé requiert des projets et des étapes supplémentaires.

Pour héberger un contrôle UWP personnalisé, vous avez besoin des projets et composants suivants :

* **Le projet et le code source de votre application**. Assurez-vous que vous avez configuré votre projet pour qu’il réponde aux [conditions préalables](#prerequisites) à l’hébergement des îlots XAML.

* **Contrôle UWP personnalisé**. Vous aurez besoin du code source du contrôle UWP personnalisé que vous souhaitez héberger pour pouvoir le compiler avec votre application. En règle générale, le contrôle personnalisé est défini dans un projet de bibliothèque de classes UWP que vous référencez dans C++ la même solution que votre projet Win32.

* **Projet d’application UWP qui définit un objet XamlApplication**. Votre C++ projet Win32 doit avoir accès à une instance de la `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` classe fournie par le kit de pratiques de la communauté Windows. Ce type joue le rôle de fournisseur de métadonnées racine pour charger les métadonnées des types XAML UWP personnalisés dans les assemblys du répertoire actif de votre application. La méthode recommandée consiste à ajouter un projet **application vide (Windows universel)** à la même solution que votre C++ projet Win32 et à modifier la classe par défaut `App` dans ce projet.
  > [!NOTE]
  > Votre solution ne peut contenir qu’un seul projet qui `XamlApplication` définit un objet. Tous les contrôles UWP personnalisés de votre application partagent le `XamlApplication` même objet. Le projet qui définit l' `XamlApplication` objet doit inclure des références à toutes les autres bibliothèques et projets UWP utilisés pour héberger les contrôles UWP dans l’îlot XAML.

Pour héberger un contrôle UWP personnalisé dans C++ une application Win32, suivez ces étapes générales.

1. Dans la solution qui contient votre C++ projet d’application de bureau Win32, ajoutez un projet **application vide (Windows universel)** et configurez-le en suivant les instructions détaillées de [cette section](host-custom-control-with-xaml-islands.md#create-a-xamlapplication-object-in-a-uwp-app-project) dans la procédure pas à pas relative à WPF.

2. Dans la même solution, ajoutez le projet qui contient le code source pour le contrôle XAML UWP personnalisé (il s’agit généralement d’un projet de bibliothèque de classes UWP) et générez le projet.

3. Dans le projet d’application UWP, ajoutez une référence au projet de bibliothèque de classes UWP.

4. Dans votre C++ projet Win32, ajoutez une référence au projet d’application UWP et au projet de bibliothèque de classes UWP dans votre solution.

5. Suivez le processus décrit dans la section [utiliser l’API d’hébergement XAML pour héberger un contrôle UWP](#use-the-xaml-hosting-api-to-host-a-uwp-control) pour héberger le contrôle personnalisé dans un îlot XAML dans votre application. Assignez une instance du contrôle personnalisé à héberger à la propriété [content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) de l’objet **DesktopWindowXamlSource** dans votre code.

Pour obtenir un exemple complet pour C++ une application Win32, consultez les projets suivants dans l' [exemple d’îlot XAML avec contrôle personnalisé](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App):

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): Ce projet implémente un contrôle XAML UWP personnalisé nommé `MyUserControl` qui contient une zone de texte, plusieurs boutons et une zone de liste déroulante.
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): Il s’agit d’un projet d’application UWP avec les modifications décrites ci-dessus.
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): Il s’agit C++ du projet d’application Win32 qui héberge le contrôle XAML UWP personnalisé dans un îlot XAML.

## <a name="handle-keyboard-layout-and-dpi"></a>Gérer le clavier, la disposition et la résolution

Les sections suivantes fournissent des conseils et des liens vers des exemples de code sur la façon de gérer les scénarios de clavier et de mise en page.

* [Entrée au clavier](#keyboard-input)
* [Navigation au focus clavier](#keyboard-focus-navigation)
* [Gérer les modifications de disposition](#handle-layout-changes)
* [Gérer les modifications PPP](#handle-dpi-changes)

### <a name="keyboard-input"></a>Saisie au clavier

Pour gérer correctement l’entrée au clavier pour chaque îlot XAML, votre application doit passer tous les messages Windows à l’infrastructure XAML UWP afin que certains messages puissent être traités correctement. Pour ce faire, dans l’application qui peut accéder à la boucle de message, effectuez un cast de l’objet **DesktopWindowXamlSource** pour chaque îlot XAML en une interface com **IDesktopWindowXamlSourceNative2** . Ensuite, appelez la méthode **PreTranslateMessage** de cette interface et transmettez le message actuel.

  * **C++ Win32 :** : L’application peut appeler **PreTranslateMessage** directement dans sa boucle de message principale. Pour obtenir un exemple, consultez le fichier [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WPF** L’application peut appeler **PreTranslateMessage** à partir du gestionnaire d’événements pour l’événement [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) . Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) dans le kit de pratiques de la communauté Windows.

  * **Windows Forms :** L’application peut appeler **PreTranslateMessage** à partir d’une substitution pour la méthode [Control. PreprocessMessage](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage) . Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) dans le kit de pratiques de la communauté Windows.

### <a name="keyboard-focus-navigation"></a>Navigation au focus clavier

Lorsque l’utilisateur parcourt les éléments d’interface utilisateur de votre application à l’aide du clavier (par exemple, en appuyant sur la touche **Tab** ou la touche direction/flèche), vous devez déplacer le focus par programmation vers et à partir de l’objet **DesktopWindowXamlSource** . Lorsque la navigation au clavier de l’utilisateur atteint le **DesktopWindowXamlSource**, déplacez le focus dans le premier objet [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) dans l’ordre de navigation de votre interface utilisateur, continuez à déplacer le focus sur ce qui suit  **Les objets Windows. UI. Xaml. UIElement** lorsque l’utilisateur parcourt les éléments, puis replacent le focus hors du **DesktopWindowXamlSource** et dans l’élément d’interface utilisateur parent.  

L’API d’hébergement XAML UWP fournit plusieurs types et membres pour vous aider à accomplir ces tâches.

* Lorsque la navigation au clavier entre dans votre **DesktopWindowXamlSource**, l’événement [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) est déclenché. Gérer cet événement et déplacer par programmation le focus vers le premier **Windows. UI. Xaml. UIElement** hébergé à l’aide de la méthode [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Lorsque l’utilisateur se trouve sur le dernier élément pouvant être actif dans votre **DesktopWindowXamlSource** et appuie sur la touche **Tab** ou sur une touche de direction, l’événement [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) est déclenché. Gère cet événement et déplace par programmation le focus vers l’élément pouvant être actif suivant dans l’application hôte. Par exemple, dans une application WPF où le **DesktopWindowXamlSource** est hébergé dans un [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), vous pouvez utiliser la méthode [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) pour transférer le focus vers l’élément pouvant être actif suivant dans l’application hôte.

Pour obtenir des exemples qui montrent comment effectuer cette opération dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants :

  * **C++/Win32**: Consultez le fichier [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WPF** Consultez le fichier [WindowsXamlHostBase.focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) dans la boîte à outils de la communauté Windows.  

  * **Windows Forms :** Consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) dans la boîte à outils de la communauté Windows.

### <a name="handle-layout-changes"></a>Gérer les modifications de disposition

Lorsque l’utilisateur modifie la taille de l’élément d’interface utilisateur parent, vous devez gérer les modifications de disposition nécessaires pour vous assurer que vos contrôles UWP s’affichent comme prévu. Voici quelques scénarios importants à prendre en compte.

* Dans une C++ application Win32, lorsque votre application gère le message WM_SIZE, elle peut repositionner l’îlot XAML hébergé à l’aide de la fonction [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Pour obtenir un exemple, consultez le fichier de code [SampleApp. cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Lorsque l’élément d’interface utilisateur parent doit connaître la taille de la zone rectangulaire nécessaire pour ajuster le **Windows. UI. Xaml. UIElement** que vous hébergez sur le **DesktopWindowXamlSource**, appelez la méthode [measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows. UI. Xaml. UIElement** . Exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de la [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir de la méthode [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) du [contrôle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

* Lorsque la taille de l’élément d’interface utilisateur parent change, appelez la méthode [arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la racine **Windows. UI. Xaml. UIElement** que vous hébergez sur le **DesktopWindowXamlSource**. Exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) de l’objet [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir du gestionnaire de l’événement [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) du [contrôle](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

### <a name="handle-dpi-changes"></a>Gérer les modifications PPP

L’infrastructure XAML UWP gère automatiquement les modifications DPI pour les contrôles UWP hébergés (par exemple, lorsque l’utilisateur fait glisser la fenêtre entre les moniteurs avec différentes résolutions d’écran). Pour une expérience optimale, nous recommandons que votre application Windows Forms, WPF ou C++ Win32 soit configurée pour prendre en charge la résolution par moniteur.

Pour configurer votre application pour qu’elle prenne en charge la résolution par moniteur, ajoutez un [manifeste d’assembly côte à côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre projet et définissez l’élément **\<dpiAwareness @ no__t-3** sur **PerMonitorV2**. Pour plus d’informations sur cette valeur, consultez la description de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erreur lors de l’utilisation de l’API d’hébergement XAML UWP dans une application UWP

| Problème | Résolution : |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant : «Impossible d’activer DesktopWindowXamlSource. Ce type ne peut pas être utilisé dans une application UWP.» ou «impossible d’activer WindowsXamlManager. Ce type ne peut pas être utilisé dans une application UWP.» | Cette erreur indique que vous essayez d’utiliser l’API d’hébergement XAML UWP (en particulier, vous essayez d’instancier les types [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) dans une application UWP. L’API d’hébergement XAML UWP est uniquement destinée à être utilisée dans les applications de bureau non UWP, telles que WPF, les C++ Windows Forms et les applications Win32. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Erreur lors de la tentative d’utilisation des types WindowsXamlManager ou DesktopWindowXamlSource

| Problème | Résolution : |
|-------|------------|
| Votre application reçoit une exception avec le message suivant : «WindowsXamlManager et DesktopWindowXamlSource sont pris en charge pour les applications ciblant Windows version 10.0.18226.0 et ultérieures. Vérifiez le manifeste de l’application ou le manifeste du package et assurez-vous que la propriété MaxTestedVersion est mise à jour.» | Cette erreur indique que votre application a essayé d’utiliser les types **WindowsXamlManager** ou **DESKTOPWINDOWXAMLSOURCE** dans l’API d’hébergement XAML UWP, mais que le système d’exploitation ne peut pas déterminer si l’application a été créée pour cibler Windows 10, version 1903 ou ultérieure. L’API d’hébergement XAML UWP a été introduite pour la première fois comme version préliminaire dans une version antérieure de Windows 10, mais elle n’est prise en charge qu’à partir de Windows 10, version 1903.</p></p>Pour résoudre ce problème, créez un package MSIX pour l’application et exécutez-le à partir du package, ou installez le package NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) dans votre projet. Pour plus d’informations, consultez [cette section](#configure-your-project-for-app-deployment). |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erreur lors de l’attachement à une fenêtre sur un thread différent

| Problème | Résolution : |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant : « Échec de la méthode AttachToWindow, car le HWND spécifié a été créé sur un autre thread. » | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative :: AttachToWindow** et lui a passé le HWND d’une fenêtre qui a été créée sur un thread différent. Vous devez passer cette méthode au HWND d’une fenêtre qui a été créée sur le même thread que le code à partir duquel vous appelez la méthode. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erreur lors de l’attachement à une fenêtre dans une fenêtre de niveau supérieur différente

| Problème | Résolution : |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant : « Échec de la méthode AttachToWindow, car le HWND spécifié descend d’une fenêtre de niveau supérieur à celle du HWND précédemment passé à AttachToWindow sur le même thread. » | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative :: AttachToWindow** et lui a passé le HWND d’une fenêtre qui descend d’une fenêtre de niveau supérieur différente d’une fenêtre que vous avez spécifiée dans un appel précédent à cette méthode. sur le même thread.</p></p>Une fois que votre application a appelé **AttachToWindow** sur un thread particulier, tous les autres objets [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) sur le même thread peuvent uniquement être attachés à des fenêtres qui sont des descendants de la même fenêtre de niveau supérieur qui a été passée dans le premier appel à **AttachToWindow**. Lorsque tous les objets **DesktopWindowXamlSource** sont fermés pour un thread particulier, le **DesktopWindowXamlSource** suivant est ensuite libre de s’attacher à une nouvelle fenêtre.</p></p>Pour résoudre ce problème, fermez tous les objets **DesktopWindowXamlSource** liés à d’autres fenêtres de niveau supérieur sur ce thread, ou créez un nouveau thread pour ce **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Rubriques connexes

* [Contrôles UWP dans les applications de bureau](xaml-islands.md)
* [C++Exemple d’îlots XAML Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
