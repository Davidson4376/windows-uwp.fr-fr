---
description: Cet article explique comment héberger une interface utilisateur XAML UWP dans votre application de bureau.
title: Utilisation de l’API d’hébergement XAML UWP dans une application de bureau
ms.date: 07/26/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, îlots XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 14607dc3e3b32f39a840d623c7a887bb8b3687c5
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601531"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Utilisation de l’API d’hébergement XAML UWP dans une application de bureau

À compter de Windows 10, version 1903, les applications de bureau non UWP (y compris les applications C++ WPF, Windows Forms et Win32) peuvent utiliser l' *API d’hébergement XAML UWP* pour héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur associé à un handle de fenêtre (HWND). Cette API permet aux applications de bureau non UWP d’utiliser les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Par exemple, les applications de bureau non UWP peuvent utiliser cette API pour héberger des contrôles UWP qui utilisent le [système de conception Fluent](/windows/uwp/design/fluent-design-system/index) et prennent en charge [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

L’API d’hébergement XAML UWP fournit la base d’un ensemble plus large de contrôles que nous fournissons pour permettre aux développeurs d’intégrer l’interface utilisateur Fluent à des applications de bureau non UWP. Cette fonctionnalité est appelée *îlots XAML*. Pour obtenir une vue d’ensemble de cette fonctionnalité, consultez [contrôles UWP dans les applications de bureau](xaml-islands.md).

> [!NOTE]
> Si vous avez des commentaires sur les îlots XAML, créez un nouveau problème dans [Microsoft. Toolkit. Win32 référentiel](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et laissez vos commentaires à cet endroit. Si vous préférez soumettre vos commentaires en privé, vous pouvez les envoyer à XamlIslandsFeedback@microsoft.com. Vos Insights et scénarios sont très importants pour nous.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Devez-vous utiliser l’API d’hébergement XAML UWP?

L’API d’hébergement XAML UWP fournit l’infrastructure de bas niveau pour l’hébergement de contrôles UWP dans les applications de bureau. Certains types d’applications de bureau ont la possibilité d’utiliser des API alternatives et plus pratiques pour atteindre cet objectif.  

* Si vous disposez d' C++ une application de bureau Win32 et souhaitez héberger des contrôles UWP dans votre application, vous devez utiliser l’API d’hébergement XAML UWP. Il n’existe aucune solution de remplacement pour ces types d’applications.

* Pour les applications WPF et Windows Forms, nous vous recommandons vivement d’utiliser les [contrôles encapsulés](xaml-islands.md#wrapped-controls) et les [contrôles hôtes](xaml-islands.md#host-controls) dans la boîte à outils de la communauté Windows au lieu d’utiliser directement l’API d’hébergement XAML UWP. Ces contrôles utilisent l’API d’hébergement XAML UWP en interne et implémentent tout le comportement que vous devrez sinon gérer vous-même si vous utilisiez l’API d’hébergement XAML UWP directement, y compris la navigation au clavier et les modifications de disposition. Toutefois, vous pouvez utiliser l’API d’hébergement XAML UWP directement dans ces types d’applications si vous le souhaitez.

Cet article explique comment utiliser l’API d’hébergement XAML UWP directement dans votre application au lieu des contrôles fournis par le kit de connaissances de la communauté Windows.

## <a name="prerequisites"></a>Prérequis

L’API d’hébergement XAML UWP présente les conditions préalables suivantes:

* Windows 10, version 1903 (ou ultérieure) et la Build correspondante du SDK Windows.
* Configurez votre projet pour qu’il utilise des API Windows Runtime et activez les îlots XAML en suivant [ces instructions](xaml-islands.md#requirements).

## <a name="architecture-of-xaml-islands"></a>Architecture des îlots XAML

L’API d’hébergement XAML UWP comprend ces principaux types de Windows Runtime et interfaces COM:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Cette classe représente l’infrastructure XAML UWP. Cette classe fournit une méthode [**InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) statique unique qui initialise l’infrastructure XAML UWP sur le thread actuel dans l’application de bureau.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Cette classe représente une instance du contenu XAML UWP que vous hébergez dans votre application de bureau. Le membre le plus important de cette classe est la propriété de [**contenu**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Vous assignez cette propriété à un [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que vous souhaitez héberger. Cette classe a également d’autres membres pour le routage du focus clavier vers et à partir des îlots XAML.

* Interfaces COM **IDesktopWindowXamlSourceNative** et **IDesktopWindowXamlSourceNative2** . **IDesktopWindowXamlSourceNative** fournit la méthode **AttachToWindow** , que vous utilisez pour attacher un îlot XAML dans votre application à un élément d’interface utilisateur parent. **IDesktopWindowXamlSourceNative2** fournit la méthode **PreTranslateMessage** , qui permet à l’infrastructure XAML UWP de traiter correctement certains messages Windows.
    > [!NOTE]
    > Ces interfaces sont déclarées dans le fichier d’en-tête **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h** dans le SDK Windows. Par défaut, ce fichier se trouve dans% ProgramFiles (x86)% \ Windows\\Kits\10\Include < numéro\>de build \um. Dans un C++ projet Win32, vous pouvez référencer directement ce fichier d’en-tête. Dans un projet WPF ou Windows Forms, vous devez déclarer les interfaces dans le code de votre application à l'aide de l’attribut [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute). Assurez-vous que vos déclarations d’interface correspondent exactement aux déclarations de **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h**.

Le diagramme suivant illustre la hiérarchie d’objets dans un îlot XAML hébergé dans une application de bureau.

![Architecture DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

* Au niveau de la base se trouve l’élément d’interface utilisateur dans votre application où vous souhaitez héberger l’îlot XAML. Cet élément d’interface utilisateur doit avoir un handle de fenêtre (HWND). Voici des exemples d’éléments d’interface utilisateur dans lesquels vous pouvez héberger un îlot XAML: [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) pour les applications WPF, [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) pour C++ les applications Windows Forms et une [fenêtre](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) pour Win32 applications.

* Au niveau suivant, il s’agit d’un objet **DesktopWindowXamlSource** . Cet objet fournit l’infrastructure pour l’hébergement de l’îlot XAML. Votre code est chargé de créer cet objet et de l’attacher à l’élément d’interface utilisateur parent.

* Lorsque vous créez un **DesktopWindowXamlSource**, cet objet crée automatiquement une fenêtre enfant native pour héberger votre contrôle UWP. Cette fenêtre enfant native est principalement abstraite de votre code, mais vous pouvez accéder à son handle (HWND) si nécessaire.

* Enfin, au niveau supérieur, il s’agit du contrôle UWP que vous souhaitez héberger dans votre application de bureau. Il peut s’agir de n’importe quel objet UWP dérivé de [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris tout contrôle UWP fourni par le SDK Windows, ainsi que des contrôles utilisateur personnalisés.

## <a name="related-samples"></a>Exemples connexes

La façon dont vous utilisez l’API d’hébergement XAML UWP dans votre code dépend de votre type d’application, de la conception de votre application et d’autres facteurs. Pour illustrer l’utilisation de cette API dans le contexte d’une application complète, cet article fait référence au code des exemples suivants.

### <a name="c-win32"></a>C++32

[ C++ Exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App). Cet exemple illustre une implémentation complète de l’hébergement d’un contrôle utilisateur UWP dans une application C++ Win32 non empaquetée (autrement dit, une application qui n’est pas intégrée à un package MSIX).

### <a name="wpf-and-windows-forms"></a>WPF et Windows Forms

Le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans la boîte à outils de la communauté Windows fait office d’exemple de référence pour l’utilisation de l’API d’hébergement UWP dans WPF et les applications de Windows Forms. Le code source est disponible aux emplacements suivants:

  * Pour la version WPF du contrôle, [cliquez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La version WPF dérive de [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

  * Pour obtenir la version Windows Forms du contrôle, [cliquez ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La version Windows Forms dérive de [**System. Windows. Forms. Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-uwp-xaml-controls"></a>Héberger des contrôles XAML UWP

Voici les principales étapes à suivre pour utiliser l’API d’hébergement XAML UWP pour héberger un contrôle UWP dans votre application.

1. Initialisez l’infrastructure XAML UWP pour le thread actuel avant que votre application ne crée l’un des objets [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) qu’il hébergera dans le [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Si votre application crée l’objet **DesktopWindowXamlSource** avant de créer l’un des objets **Windows. UI. Xaml. UIElement** , cette infrastructure sera initialisée pour vous lorsque vous instancierez l’objet **DesktopWindowXamlSource** . Dans ce scénario, vous n’avez pas besoin d’ajouter le code de votre choix pour initialiser le Framework.

    * Toutefois, si votre application crée les objets **Windows. UI. Xaml. UIElement** avant de créer l’objet **DesktopWindowXamlSource** qui va les héberger, votre application doit appeler la [ **Méthode WindowsXamlManager. InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) pour initialiser explicitement l’infrastructure XAML UWP avant l’instanciation des objets **Windows. UI. Xaml. UIElement** . Votre application doit généralement appeler cette méthode lorsque l’élément d’interface utilisateur parent qui héberge le **DesktopWindowXamlSource** est instancié.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Cette méthode retourne un objet [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) qui contient une référence à l’infrastructure XAML UWP. Vous pouvez créer autant d’objets **WindowsXamlManager** que vous le souhaitez sur un thread donné. Toutefois, étant donné que chaque objet contient une référence à l’infrastructure XAML UWP, vous devez supprimer les objets pour vous assurer que les ressources XAML sont libérées.

2. Créez un objet [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) et attachez-le à un élément d’interface utilisateur parent dans votre application associée à un handle de fenêtre.

    Pour ce faire, vous devez suivre les étapes ci-dessous:

    1. Créez un objet **DesktopWindowXamlSource** et effectuez un cast de celui-ci en interface com **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** .

    2. Appelez la méthode **AttachToWindow** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** , puis transmettez le handle de fenêtre de l’élément d’interface utilisateur parent dans votre application.

    3. Définissez la taille initiale de la fenêtre enfant interne contenue dans le **DesktopWindowXamlSource**. Par défaut, cette fenêtre enfant interne est définie sur une largeur et une hauteur de 0. Si vous ne définissez pas la taille de la fenêtre, les contrôles UWP que vous ajoutez au **DesktopWindowXamlSource** ne seront pas visibles. Pour accéder à la fenêtre enfant interne dans **DesktopWindowXamlSource**, utilisez la propriété **WindowHandle** de l’interface **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** . Les exemples suivants utilisent la fonction [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) pour définir la taille de la fenêtre.

    Voici quelques exemples de code qui illustrent ce processus.

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI element for your XAML Island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. Définissez le **Windows. UI. Xaml. UIElement** que vous souhaitez héberger dans la propriété [**content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) de votre objet **DesktopWindowXamlSource** . L’exemple suivant définit un [**Windows. UI. Xaml. Controls. Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) nommé ```myGrid``` sur la propriété de **contenu** .

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Pour obtenir des exemples complets qui illustrent ces tâches dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants:

  * **C++32** Consultez le fichier [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WPF** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) dans la boîte à outils de la communauté Windows.  

  * **Windows Forms:** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) dans la boîte à outils de la communauté Windows.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Gérer l’entrée au clavier et la navigation au focus

Votre application doit faire plusieurs choses pour gérer correctement l’entrée au clavier et le focus quand elle héberge des îlots XAML.

### <a name="keyboard-input"></a>Saisie au clavier

Pour gérer correctement l’entrée au clavier pour chaque îlot XAML, votre application doit passer tous les messages Windows à l’infrastructure XAML UWP afin que certains messages puissent être traités correctement. Pour ce faire, dans l’application qui peut accéder à la boucle de message, effectuez un cast de l’objet **DesktopWindowXamlSource** pour chaque îlot XAML en une interface com **IDesktopWindowXamlSourceNative2** . Ensuite, appelez la méthode **PreTranslateMessage** de cette interface et transmettez le message actuel.

  * Une C++ application Win32 peut appeler **PreTranslateMessage** directement dans sa boucle de message principale. Pour obtenir un exemple, consultez le fichier [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * Une application WPF peut appeler **PreTranslateMessage** à partir du gestionnaire d’événements pour l’événement [**ComponentDispatcher. ThreadFilterMessage**](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) . Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) dans le kit de pratiques de la communauté Windows.

  * Une application Windows Forms peut appeler **PreTranslateMessage** à partir d’une substitution pour la méthode [**Control. PreprocessMessage**](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) . Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) dans le kit de pratiques de la communauté Windows.

### <a name="keyboard-focus-navigation"></a>Navigation au focus clavier

Lorsque l’utilisateur parcourt les éléments d’interface utilisateur de votre application à l’aide du clavier (par exemple, en appuyant sur la touche **Tab** ou la touche direction/flèche), vous devez déplacer le focus par programmation vers et à partir de l’objet **DesktopWindowXamlSource** . Lorsque la navigation au clavier de l’utilisateur atteint le **DesktopWindowXamlSource**, déplacez le focus dans le premier objet [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) dans l’ordre de navigation de votre interface utilisateur, continuez à déplacer le focus sur ce qui suit  **Les objets Windows. UI. Xaml. UIElement** lorsque l’utilisateur parcourt les éléments, puis replacent le focus hors du **DesktopWindowXamlSource** et dans l’élément d’interface utilisateur parent.  

L’API d’hébergement XAML UWP fournit plusieurs types et membres pour vous aider à accomplir ces tâches.

* Lorsque la navigation au clavier entre dans votre **DesktopWindowXamlSource**, l’événement [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) est déclenché. Gérer cet événement et déplacer par programmation le focus vers le premier **Windows. UI. Xaml. UIElement** hébergé à l’aide de la méthode [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Lorsque l’utilisateur se trouve sur le dernier élément pouvant être actif dans votre **DesktopWindowXamlSource** et appuie sur la touche **Tab** ou sur une touche de direction, l’événement [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) est déclenché. Gère cet événement et déplace par programmation le focus vers l’élément pouvant être actif suivant dans l’application hôte. Par exemple, dans une application WPF où le **DesktopWindowXamlSource** est hébergé dans un [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), vous pouvez utiliser la méthode [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) pour transférer le focus vers l’élément pouvant être actif suivant dans l’application hôte.

Pour obtenir des exemples qui montrent comment effectuer cette opération dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants:

  * **WPF** Consultez le fichier [WindowsXamlHostBase.focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) dans la boîte à outils de la communauté Windows.  

  * **Windows Forms:** Consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) dans la boîte à outils de la communauté Windows.

  * **C++/Win32**: Consultez le fichier [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

## <a name="handle-layout-changes"></a>Gérer les modifications de disposition

Lorsque l’utilisateur modifie la taille de l’élément d’interface utilisateur parent, vous devez gérer les modifications de disposition nécessaires pour vous assurer que vos contrôles UWP s’affichent comme prévu. Voici quelques scénarios importants à prendre en compte.

* Dans une C++ application Win32, lorsque votre application gère le message WM_SIZE, elle peut repositionner l’îlot XAML hébergé à l’aide de la fonction [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Pour obtenir un exemple, consultez le fichier de code [SampleApp. cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Lorsque l’élément d’interface utilisateur parent doit connaître la taille de la zone rectangulaire nécessaire pour ajuster le **Windows. UI. Xaml. UIElement** que vous hébergez sur le **DesktopWindowXamlSource**, appelez la méthode [**measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows. UI. Xaml. UIElement** . Exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de la [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir de la méthode [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) du [**contrôle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

* Lorsque la taille de l’élément d’interface utilisateur parent change, appelez la méthode [**arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la racine **Windows. UI. Xaml. UIElement** que vous hébergez sur le **DesktopWindowXamlSource**. Exemple :

    * Dans une application WPF, vous pouvez effectuer cette opération à partir de la méthode [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) de l’objet [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

    * Dans une application Windows Forms, vous pouvez effectuer cette opération à partir du gestionnaire de l’événement [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) du [**contrôle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le kit de pratiques de la communauté Windows.

## <a name="handle-dpi-changes"></a>Gérer les modifications PPP

L’infrastructure XAML UWP gère automatiquement les modifications DPI pour les contrôles UWP hébergés (par exemple, lorsque l’utilisateur fait glisser la fenêtre entre les moniteurs avec différentes résolutions d’écran). Pour une expérience optimale, nous recommandons que votre application Windows Forms, WPF ou C++ Win32 soit configurée pour prendre en charge la résolution par moniteur.

Pour configurer votre application pour qu’elle prenne en charge la résolution par moniteur, ajoutez un [manifeste d’assembly côte à côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à ```<dpiAwareness>``` votre projet ```PerMonitorV2```et affectez à l’élément la valeur. Pour plus d’informations sur cette valeur, consultez la description de [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Héberger des contrôles XAML UWP personnalisés

Suivez ces étapes générales pour héberger un contrôle XAML UWP personnalisé (soit un contrôle que vous définissez vous-même ou un contrôle fourni par un tiers) dans un îlot XAML. Vous devez disposer du code source du contrôle personnalisé pour pouvoir le compiler avec votre application.

1. Dans votre solution d’application hôte, intégrez le code source pour le contrôle XAML UWP personnalisé et générez le contrôle personnalisé.

2. Ajoutez un projet d’application UWP à votre solution d’application hôte et ajoutez le package NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) à ce projet.

3. Dans le projet d’application UWP, modifiez `App` votre classe de sorte qu’elle soit issue de la classe **Microsoft. Toolkit. Win32. UI. XamlApplication** exposée par le package NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) . Ce type joue le rôle de fournisseur de métadonnées racine pour charger les métadonnées des types XAML UWP personnalisés dans les assemblys du répertoire actif de votre application.

4. Dans le constructeur de votre `App` classe, appelez la méthode **Initialize** de la classe de base **Microsoft. Toolkit. Win32. UI. XamlApplication** .

5. Dans votre projet d’application hôte, suivez le processus décrit dans la [section précédente](#host-uwp-xaml-controls) pour héberger le contrôle personnalisé dans un îlot XAML.

Pour obtenir un exemple d' C++ application Win32, consultez les projets suivants dans l' [ C++ exemple Win32](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App):

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): Ce projet implémente un contrôle XAML UWP personnalisé nommé `MyUserControl` qui contient une zone de texte, plusieurs boutons et une zone de liste déroulante.
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): Il s’agit d’un projet d’application UWP avec les modifications décrites dans les étapes précédentes.
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): Il s’agit C++ du projet d’application Win32 qui héberge le contrôle XAML UWP personnalisé dans un îlot XAML.

Pour obtenir des instructions détaillées et des exemples pour une application WPF ou Windows Forms, consultez [ces instructions](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erreur lors de l’utilisation de l’API d’hébergement XAML UWP dans une application UWP

| Problème | Résolution : |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant: «Impossible d’activer DesktopWindowXamlSource. Ce type ne peut pas être utilisé dans une application UWP.» ou «impossible d’activer WindowsXamlManager. Ce type ne peut pas être utilisé dans une application UWP.» | Cette erreur indique que vous essayez d’utiliser l’API d’hébergement XAML UWP (en particulier, vous essayez d’instancier les types [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) dans une application UWP. L’API d’hébergement XAML UWP est uniquement destinée à être utilisée dans les applications de bureau non UWP, telles que WPF, les C++ Windows Forms et les applications Win32. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erreur lors de l’attachement à une fenêtre sur un thread différent

| Problème | Résolution : |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant: «Échec de la méthode AttachToWindow, car le HWND spécifié a été créé sur un autre thread.» | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative:: AttachToWindow** et lui a passé le HWND d’une fenêtre qui a été créée sur un thread différent. Vous devez passer cette méthode au HWND d’une fenêtre qui a été créée sur le même thread que le code à partir duquel vous appelez la méthode. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erreur lors de l’attachement à une fenêtre dans une fenêtre de niveau supérieur différente

| Problème | Résolution : |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant: «Échec de la méthode AttachToWindow, car le HWND spécifié descend d’une fenêtre de niveau supérieur à celle du HWND précédemment passé à AttachToWindow sur le même thread.» | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative:: AttachToWindow** et lui a passé le HWND d’une fenêtre qui descend d’une fenêtre de niveau supérieur différente d’une fenêtre que vous avez spécifiée dans un appel précédent à cette méthode. sur le même thread.</p></p>Une fois que votre application a appelé **IDesktopWindowXamlSourceNative:: AttachToWindow** sur un thread particulier, tous les autres objets [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) sur le même thread peuvent uniquement être attachés à des fenêtres qui sont des descendants du même niveau supérieur fenêtre qui a été passée dans le premier appel à **IDesktopWindowXamlSourceNative:: AttachToWindow**. Lorsque tous les objets **DesktopWindowXamlSource** sont fermés pour un thread particulier, le **DesktopWindowXamlSource** suivant est ensuite libre de s’attacher à une nouvelle fenêtre.</p></p>Pour résoudre ce problème, fermez tous les objets **DesktopWindowXamlSource** liés à d’autres fenêtres de niveau supérieur sur ce thread, ou créez un nouveau thread pour ce **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Rubriques connexes

* [Contrôles UWP dans les applications de bureau](xaml-islands.md)
* [C++Exemple d’îlots XAML Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
