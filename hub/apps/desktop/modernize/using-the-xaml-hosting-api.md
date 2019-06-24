---
description: Cet article décrit comment héberger l’interface utilisateur du XAML UWP dans votre application de bureau.
title: À l’aide de le XAML UWP API d’hébergement dans une application de bureau
ms.date: 04/16/2019
ms.topic: article
keywords: Windows 10, uwp, WinForms, wpf, win32, îles de xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 16f61c1f950583ee0fef7f30b7e17939df7ea538
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317755"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>À l’aide de le XAML UWP API d’hébergement dans une application de bureau

À compter de Windows 10, version 1903, applications de bureau non UWP (y compris WPF, Windows Forms, et C++ applications Win32) peut utiliser le *XAML UWP API d’hébergement* pour héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur qui est associé à un handle de fenêtre (HWND). Cette API permet aux applications de bureau de non UWP d’utiliser les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Par exemple, les applications de bureau non UWP peuvent utiliser cette API pour héberger des contrôles UWP qui utilisent le [Fluent Design System](/windows/uwp/design/fluent-design-system/index) et prennent en charge [Windows Ink](/windows/uwp/design/pen-and-stylus-interactions).

Le XAML UWP API d’hébergement fournit la base pour un ensemble plus large de contrôles que nous mettons à disposition pour permettre aux développeurs de proposer de l’interface utilisateur Fluent à non UWP applications de bureau. Cette fonctionnalité est appelée *XAML îles*. Pour une vue d’ensemble de cette fonctionnalité, consultez [dans les applications de bureau, les contrôles UWP](xaml-islands.md).

> [!NOTE]
> Si vous avez des commentaires concernant les îles de XAML, créez un nouveau problème dans le [Microsoft.Toolkit.Win32 référentiel](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) et y laisser vos commentaires. Si vous préférez envoyer vos commentaires en privé, vous pouvez l’envoyer à XamlIslandsFeedback@microsoft.com. Vos idées et les scénarios sont extrêmement importantes pour nous.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>Devez-vous utiliser le XAML UWP API d’hébergement ?

Le XAML UWP API d’hébergement fournit l’infrastructure de bas niveau pour l’hébergement de contrôles UWP dans les applications de bureau. Certains types d’applications de bureau ont la possibilité d’utiliser des API alternatives et plus pratique à atteindre cet objectif.  

* Si vous avez une application de bureau Win32 C++ et que vous souhaitez dans votre application pour héberger des contrôles UWP, vous devez utiliser le XAML UWP API d’hébergement. Il n’existe aucune alternative pour ces types d’applications.

* Pour les applications WPF et Windows Forms, nous recommandons fortement d’utiliser le [encapsulé les contrôles](xaml-islands.md#wrapped-controls) et [contrôles hôtes](xaml-islands.md#host-controls) dans le Kit de ressources de communauté Windows au lieu d’utiliser le XAML UWP directement l’API d’hébergement. Ces contrôles utilisent le XAML UWP API d’hébergement en interne et implémentent le comportement que vous devrez sinon gérer vous-même si vous avez utilisé le XAML UWP API directement, y compris les modifications de navigation et la disposition de clavier d’hébergement. Toutefois, vous pouvez utiliser le XAML UWP API directement dans ces types d’applications d’hébergement si vous choisissez.

Cet article décrit comment utiliser le XAML UWP directement dans votre application plutôt que les contrôles fournis par le Kit de ressources de communauté de Windows, les API d’hébergement.

## <a name="prerequisites"></a>Prérequis

Le XAML UWP API d’hébergement a ces conditions préalables :

* Windows 10, version 1903 (ou version ultérieure) et correspondant de génération du SDK Windows.
* Configurer votre projet pour utiliser Windows APIs Runtime et activer des îles de XAML en suivant [ces instructions](xaml-islands.md#requirements).

## <a name="architecture-of-xaml-islands"></a>Architecture des îles de XAML

Le XAML UWP API d’hébergement inclut ces principaux types Windows Runtime et les interfaces COM :

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Cette classe représente le framework UWP XAML. Cette classe fournit un statique unique [ **InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) méthode qui initialise le framework UWP XAML sur le thread actuel dans l’application de bureau.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Cette classe représente une instance de contenu UWP XAML que vous hébergez dans votre application de bureau. Le membre le plus important de cette classe est la [ **contenu** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) propriété. Vous affectez à cette propriété un [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que vous souhaitez héberger. Cette classe a également d’autres membres de routage navigation du focus clavier vers et depuis les îles de XAML.

* **IDesktopWindowXamlSourceNative** et **IDesktopWindowXamlSourceNative2** interfaces COM. **IDesktopWindowXamlSourceNative** fournit le **AttachToWindow** (méthode), qui vous permet d’attacher un îlot de XAML dans votre application à un élément d’interface utilisateur parent. **IDesktopWindowXamlSourceNative2** fournit le **PreTranslateMessage** (méthode), ce qui permet à l’infrastructure UWP XAML traiter correctement les certains messages Windows.
    > [!NOTE]
    > Ces interfaces sont déclarées dans le **windows.ui.xaml.hosting.desktopwindowxamlsource.h** fichier d’en-tête dans le SDK Windows. Par défaut, ce fichier se trouve dans % ProgramFiles% (x86) %\Windows Kits\10\Include\\< numéro de build\>\um. Dans un projet Win32 C++, vous pouvez référencer directement ce fichier d’en-tête. Dans un projet WPF ou Windows Forms, vous devez déclarer les interfaces dans votre code d’application en utilisant le [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) attribut. Assurez-vous que les déclarations d’interface correspondre exactement aux déclarations dans **windows.ui.xaml.hosting.desktopwindowxamlsource.h**.

Le diagramme suivant illustre la hiérarchie des objets dans un îlot de XAML qui est hébergé dans une application de bureau.

![DesktopWindowXamlSource architecture](images/xaml-islands/xaml-hosting-api-rev2.png)

* Au niveau de base est l’élément d’interface utilisateur dans votre application où vous voulez héberger l’îlot de XAML. Cet élément d’interface utilisateur doit avoir un handle de fenêtre (HWND). Exemples d’éléments d’interface utilisateur dans laquelle vous pouvez héberger un îlot de XAML [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) pour les applications WPF, [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) pour les applications Windows Forms et un [fenêtre](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) pour C++ applications Win32.

* Au prochain niveau est un **DesktopWindowXamlSource** objet. Cet objet fournit l’infrastructure pour l’hébergement de l’îlot de XAML. Votre code est responsable de la création de cet objet et la lier à l’élément d’interface parent.

* Lorsque vous créez un **DesktopWindowXamlSource**, cet objet crée automatiquement une fenêtre enfant natif pour héberger votre contrôle UWP. Cette fenêtre enfant natif est principalement dépendent plus votre code, mais vous pouvez accéder à son handle (HWND) si nécessaire.

* Enfin, au niveau supérieur est le contrôle UWP que vous souhaitez héberger dans votre application de bureau. Cela peut être n’importe quel objet UWP qui dérive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris de n’importe quel contrôle UWP fournis par le Kit de développement Windows, ainsi que les contrôles utilisateur personnalisés.

## <a name="related-samples"></a>Exemples connexes

Vous utilisez le XAML UWP API d’hébergement dans votre code dépend de votre type d’application, la conception de votre application et d’autres facteurs. Pour illustrer l’utilisation de cette API dans le contexte d’une application complète, cet article fait référence au code dans les exemples suivants.

### <a name="c-win32"></a>C++ Win32

[C++Win32, exemple](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island). Cet exemple montre une implémentation complète d’hébergement d’un contrôle utilisateur UWP dans un décompressé C++ application Win32 (autrement dit, une application qui n’est pas intégrée à un package MSIX).

### <a name="wpf-and-windows-forms"></a>WPF et Windows Forms

Le [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) contrôle dans la boîte à outils de la Communauté Windows agit comme un échantillon de référence pour l’utilisation de la plateforme Windows universelle API d’hébergement dans les applications WPF et Windows Forms. Le code source est disponible aux emplacements suivants :

  * Pour obtenir la version WPF du contrôle, [ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). Dérive de la version WPF [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

  * Pour obtenir la version de Windows Forms du contrôle, [ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). Dérive de la version de Windows Forms [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-uwp-xaml-controls"></a>Contrôle hôte UWP XAML

Voici les principales étapes pour l’utilisation du XAML UWP API d’hébergement pour héberger un contrôle UWP dans votre application.

1. Initialiser le framework UWP XAML pour le thread actuel avant que votre application crée de la [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objets destiné à héberger dans le [  **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Si votre application crée le **DesktopWindowXamlSource** de l’objet avant de créer de la **Windows.UI.Xaml.UIElement** objets, cette infrastructure sera initialisée pour vous lorsque vous instanciez le **DesktopWindowXamlSource** objet. Dans ce scénario, vous n’avez pas besoin d’ajouter du code de votre choix pour initialiser le framework.

    * Toutefois, si votre application crée le **Windows.UI.Xaml.UIElement** objets avant de créer le **DesktopWindowXamlSource** objet qui les hébergent votre application doit appeler la méthode statique [ **WindowsXamlManager.InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) méthode pour initialiser explicitement le framework UWP XAML avant la **Windows.UI.Xaml.UIElement** sont des objets instancié. Votre application doit appeler généralement cette méthode lorsque l’élément d’interface parent qui héberge le **DesktopWindowXamlSource** est instancié.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Cette méthode retourne un [ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) objet qui contient une référence à l’infrastructure UWP XAML. Vous pouvez créer autant **WindowsXamlManager** objets que vous le souhaitez sur un thread donné. Toutefois, étant donné que chaque objet conserve une référence à l’infrastructure UWP XAML, supprimez les objets pour vous assurer que les ressources XAML sont publiées par la suite.

2. Créer un [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) de l’objet et l’attacher à un élément d’interface utilisateur dans votre application qui est associé à un handle de fenêtre parente.

    Pour ce faire, vous devez procéder comme suit :

    1. Créer un **DesktopWindowXamlSource** de l’objet et effectuer un cast en le **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** interface COM.

    2. Appelez le **AttachToWindow** méthode de la **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** interface, puis passez le handle de fenêtre de la élément d’interface utilisateur parent dans votre application.

    3. Définir la taille initiale de la fenêtre enfant internes contenue dans le **DesktopWindowXamlSource**. Par défaut, cette fenêtre enfant interne a une largeur et une hauteur de 0. Si vous ne définissez pas la taille de la fenêtre, tous les contrôles UWP vous ajouter à la **DesktopWindowXamlSource** ne seront pas visibles. Pour accéder à la fenêtre enfant interne dans le **DesktopWindowXamlSource**, utilisez le **WindowHandle** propriété de la **IDesktopWindowXamlSourceNative** ou **IDesktopWindowXamlSourceNative2** interface. Les exemples suivants utilisent le [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) fonction permettant de définir la taille de la fenêtre.

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

3. Définir le **Windows.UI.Xaml.UIElement** vous voulez héberger à la [ **contenu** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) propriété de votre **DesktopWindowXamlSource** objet. L’exemple suivant définit un [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) nommé ```myGrid``` à la **contenu** propriété.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Pour obtenir des exemples complets qui montrent comment effectuer ces tâches dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants :

  * **Win32 C++ :** Consultez le [XamlBridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/XamlBridge.cpp) de fichiers dans le [ C++ Win32, exemple](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * **WPF :** Consultez le [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) fichiers dans la boîte à outils de la Communauté Windows.  

  * **Windows Forms :** Consultez le [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) fichiers dans la boîte à outils de la Communauté Windows.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Gérer la navigation d’entrée et le focus clavier

Il existe plusieurs choses que votre application doit effectuer pour gérer correctement la navigation d’entrée et le focus clavier lorsqu’il héberge des îles de XAML.

### <a name="keyboard-input"></a>Saisie au clavier

Pour gérer correctement l’entrée au clavier pour chaque Island XAML, votre application doit passer tous les messages Windows à l’infrastructure UWP XAML afin que certains messages peuvent être traités correctement. Pour ce faire, à un endroit dans votre application qui peut accéder à la boucle de message, effectuer un cast du **DesktopWindowXamlSource** objet pour chaque Island XAML pour un **IDesktopWindowXamlSourceNative2** interface COM. Ensuite, appelez le **PreTranslateMessage** méthode de cette interface et le passe dans le message en cours.

  * Un C++ application Win32 peut appeler **PreTranslateMessage** directement dans sa boucle de messages principale. Pour obtenir un exemple, consultez le [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L61) fichier de code dans le [ C++ Win32, exemple](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * Une application WPF peut appeler **PreTranslateMessage** du Gestionnaire d’événements pour le [ **ComponentDispatcher.ThreadFilterMessage** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) événement. Pour obtenir un exemple, consultez le [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) fichier dans la boîte à outils de la Communauté Windows.

  * Une application Windows Forms peut appeler **PreTranslateMessage** à partir d’un remplacement pour le [ **Control.PreprocessMessage** ](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) (méthode). Pour obtenir un exemple, consultez le [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) fichier dans la boîte à outils de la Communauté Windows.

### <a name="keyboard-focus-navigation"></a>Navigation du focus clavier

Lorsque l’utilisateur parcourt les éléments d’interface utilisateur dans votre application à l’aide du clavier (par exemple, en appuyant sur **onglet** ou la touche de direction haut/bas), vous devrez déplacer par programmation le focus dans et hors de la  **DesktopWindowXamlSource** objet. Lorsque la navigation au clavier de l’utilisateur atteint le **DesktopWindowXamlSource**, déplacer le focus dans la première [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objet dans l’ordre de navigation pour votre interface utilisateur, continuez à déplacer le focus à ce qui suit **Windows.UI.Xaml.UIElement** objets en tant que l’utilisateur parcourt les éléments, puis reviennent sur le focus de déplacement de la **DesktopWindowXamlSource**et dans l’élément d’interface parent.  

Le XAML UWP API d’hébergement fournit plusieurs types et membres pour vous aider à accomplir ces tâches.

* Lorsque la navigation au clavier pénètre dans votre **DesktopWindowXamlSource**, le [ **réception focus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) événement est déclenché. Gérez cet événement et déplacer par programmation le focus au premier hébergé **Windows.UI.Xaml.UIElement** à l’aide de la [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) (méthode).

* Lorsque l’utilisateur est sur le dernier élément pouvant être actif dans votre **DesktopWindowXamlSource** et appuie sur le **onglet** clé ou une touche de direction, la [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) événement est déclenché. Gérez cet événement et déplacer par programmation le focus vers l’élément suivant pouvant prendre le focus dans l’application hôte. Par exemple, dans une application WPF où le **DesktopWindowXamlSource** est hébergé dans un [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), vous pouvez utiliser la [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) méthode pour transférer le focus vers l’élément suivant pouvant prendre le focus dans l’application hôte.

Pour obtenir des exemples qui illustrent comment effectuer cette opération dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants :

  * **WPF :** Consultez le [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) fichier dans la boîte à outils de la Communauté Windows.  

  * **Windows Forms :** Consultez le [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) fichier dans la boîte à outils de la Communauté Windows.

## <a name="handle-layout-changes"></a>Gérer les changements de disposition

Lorsque l’utilisateur modifie la taille de l’élément d’interface utilisateur parent, vous devrez gérer les modifications de disposition nécessaires pour vous assurer que vos contrôles UWP afficher comme prévu. Voici quelques scénarios importants à prendre en compte.

* Dans un C++ application Win32, lorsque votre application gère le message WM_SIZE il peut repositionner l’îlot de XAML hébergé à l’aide de la [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) (fonction). Pour obtenir un exemple, consultez le [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) fichier de code dans le [ C++ Win32, exemple](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Lorsque l’élément d’interface parent a besoin obtenir la taille de la zone rectangulaire nécessaire pour contenir le **Windows.UI.Xaml.UIElement** hébergés sur le **DesktopWindowXamlSource**, appelez le [ **Mesure** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) méthode de la **Windows.UI.Xaml.UIElement**. Exemple :

    * Dans une application WPF, vous pouvez faire cela à partir de la [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) méthode de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge le  **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) fichier dans la boîte à outils de la Communauté Windows.

    * Dans une application Windows Forms, vous pouvez faire cela à partir de la [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) méthode de la [ **contrôle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge le **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) fichier dans la boîte à outils de la Communauté Windows.

* Lorsque la taille de l’élément d’interface utilisateur parent change, appelez le [ **organiser** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) méthode de la racine **Windows.UI.Xaml.UIElement** hébergés sur le  **DesktopWindowXamlSource**. Exemple :

    * Dans une application WPF, vous pouvez faire cela à partir de la [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) méthode de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) objet qui héberge le **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) fichier dans la boîte à outils de la Communauté Windows.

    * Dans une application Windows Forms vous pouvez faire cela à partir du gestionnaire pour le [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) événements de la [ **contrôle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge le **DesktopWindowXamlSource**. Pour obtenir un exemple, consultez le [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) fichier dans la boîte à outils de la Communauté Windows.

## <a name="handle-dpi-changes"></a>Gérer les changements de résolution

Le framework UWP XAML gère les changements de résolution pour les contrôles UWP hébergés automatiquement (par exemple, lorsque l’utilisateur fait glisser la fenêtre entre les écrans avec un autre écran PPP). Pour une expérience optimale, nous recommandons que vos formulaires Windows, WPF, ou C++ application Win32 est configurée pour connaître la résolution par moniteur.

Pour configurer votre application pour connaître la résolution par moniteur, ajoutez un [manifeste d’assembly de côte à côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre projet et un ensemble ```<dpiAwareness>``` élément à ```PerMonitorV2```. Pour plus d’informations sur cette valeur, consultez la description de [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Contrôle de l’hôte personnalisé UWP XAML

> [!IMPORTANT]
> Pour héberger un contrôle UWP XAML personnalisé, vous devez disposer du code source pour le contrôle afin que vous pouvez compiler sur celui-ci dans votre application.

Si vous souhaitez héberger un contrôle UWP XAML personnalisé (un contrôle que vous définissez vous-même ou un contrôle fourni par un 3e partie), vous devez effectuer les tâches supplémentaires suivantes en plus de la manière décrite dans le [section précédente](#host-uwp-xaml-controls).

1. Définir un type personnalisé qui dérive de [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) et implémente également [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Ce type agit comme un fournisseur de métadonnées de racine du chargement des métadonnées pour les types UWP XAML personnalisés dans les assemblys dans le répertoire actuel de votre application.

2. Appelez le [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) de votre fournisseur de métadonnées racine quand le nom de type du contrôle UWP XAML est affecté (méthode) (Cela peut être attribué dans le code en cours d’exécution, ou vous pouvez choisir d’activer cette option pour être attribué dans la fenêtre Propriétés de Visual Studio).

    Pour obtenir des exemples, consultez les fichiers de code suivants :
      * **Win32 C++ :** Consultez le [XamlApplication.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup/XamlApplication.cpp) fichier de code dans le [ C++ Win32, exemple](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

      * **WPF et Windows Forms**: Consultez le [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) et [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) dans le Kit de ressources de communauté de Windows, les fichiers de code. Ces fichiers font partie de l’implémentation partagée de la **WindowsXamlHost** classes pour WPF et Windows Forms, qui illustrent comment utiliser le XAML UWP API dans ces types d’applications d’hébergement.

3. Intégrer le code source pour le contrôle UWP XAML personnalisé dans votre solution d’application hôte, générer le contrôle personnalisé et l’utiliser dans votre application. Pour obtenir des instructions pour une application WPF ou Windows Forms, consultez [ces instructions](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control). Pour obtenir un exemple pour un C++ application Win32, consultez le [Microsoft.UI.Xaml.Markup](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup) et [MyApp](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/MyApp) les projets dans le [ C++ Win32, exemple](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erreur à l’aide de XAML UWP API d’hébergement dans une application UWP

| Problème | Résolution |
|-------|------------|
| Votre application reçoit un **COMException** avec le message suivant : « Impossible d’activer DesktopWindowXamlSource. Ce type ne peut pas être utilisé dans une application UWP. » ou « Impossible d’activer WindowsXamlManager. Ce type ne peut pas être utilisé dans une application UWP. » | Cette erreur indique que vous essayez d’utiliser le XAML UWP API d’hébergement (en particulier, que vous tentez d’instancier le [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) types) dans une application UWP. Le XAML UWP API d’hébergement est uniquement destinée à être utilisée dans les applications de bureau non UWP, telles que les applications WPF, Windows Forms et Win32 C++. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erreur durant l’attachement à une fenêtre sur un thread différent

| Problème | Résolution |
|-------|------------|
| Votre application reçoit un **COMException** avec le message suivant : « AttachToWindow méthode a échoué, car le HWND spécifié a été créé sur un thread différent. » | Cette erreur indique que votre application a appelé la **IDesktopWindowXamlSourceNative::AttachToWindow** (méthode) et lui a transmis le HWND de la fenêtre a été créé sur un thread différent. Vous devez passer à cette méthode le HWND de la fenêtre a été créé sur le même thread que le code à partir duquel vous appelez la méthode. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erreur durant l’attachement à une fenêtre sur une autre fenêtre de niveau supérieur

| Problème | Résolution |
|-------|------------|
| Votre application reçoit un **COMException** avec le message suivant : « AttachToWindow méthode a échoué, car le HWND spécifié descend à partir d’une autre fenêtre de niveau supérieur que le HWND qui a été précédemment passé à AttachToWindow sur le même thread. » | Cette erreur indique que votre application a appelé la **IDesktopWindowXamlSourceNative::AttachToWindow** (méthode) et lui a transmis le HWND de fenêtre qui provient d’une autre fenêtre de niveau supérieur à une fenêtre que vous avez spécifié dans un appel précédent à cette méthode sur le même thread.</p></p>Une fois votre application appelle **IDesktopWindowXamlSourceNative::AttachToWindow** sur un thread particulier, tous les autres [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) objets sur le même attachement de thread peut uniquement à windows qui sont des descendants de la même fenêtre de niveau supérieur qui a été passée dans le premier appel à **IDesktopWindowXamlSourceNative::AttachToWindow**. Lorsque tous les le **DesktopWindowXamlSource** objets sont fermées pour un thread particulier, la prochaine **DesktopWindowXamlSource** est ensuite libre d’attacher à n’importe quelle fenêtre à nouveau.</p></p>Pour résoudre ce problème, fermez tous les **DesktopWindowXamlSource** les objets qui sont liées aux autres fenêtres de niveau supérieur sur ce thread, ou créent un nouveau thread pour cette **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Rubriques connexes

* [Contrôles UWP dans les applications de bureau](xaml-islands.md)
* [C++Exemple de Win32 XAML (îles)](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
