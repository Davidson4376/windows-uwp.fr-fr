---
description: Cet article décrit comment héberger l’interface utilisateur du XAML UWP dans votre application de bureau.
title: À l’aide de le XAML UWP API d’hébergement dans une application de bureau
ms.date: 01/11/2019
ms.topic: article
keywords: Windows 10, uwp, WinForms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: efd7dc687b9aba2e3c07b0afefa2e4fa49b882b1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618834"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>À l’aide de le XAML UWP API d’hébergement dans une application de bureau

> [!NOTE]
> L’UWP XAML hébergement XAML et API (îles) est actuellement disponibles en version préliminaire de développeur. Bien que nous vous encourageons à les tester dans votre propre code de prototype maintenant, il est déconseillé d’utiliser leur code de production pour l’instant. Ces fonctionnalités continuera à maturité et stabiliser dans les futures versions de Windows. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.
>
> Si vous avez des commentaires concernant les îles XAML, créez un nouveau problème dans le [WindowsCommunityToolkit référentiel](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) et y laisser vos commentaires. Si vous préférez envoyer vos commentaires en privé, vous pouvez l’envoyer à XamlIslandsFeedback@microsoft.com. Vos idées et les scénarios sont extrêmement importantes pour nous.

À compter de SDK Windows 10 Insider Preview build 17709, les applications de bureau non UWP (y compris les applications WPF, Windows Forms et Win32 C++) peuvent utiliser le *XAML UWP API d’hébergement* pour héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur qui est associé avec un handle de fenêtre (HWND). Cette API permet aux applications de bureau de non UWP d’utiliser les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Par exemple, les applications de bureau non UWP peuvent utiliser cette API pour héberger des contrôles UWP qui utilisent le [Fluent Design System](../design/fluent-design-system/index.md) et prennent en charge [Windows Ink](../design/input/pen-and-stylus-interactions.md).

Le XAML UWP API d’hébergement fournit la base pour un ensemble plus large de contrôles que nous mettons à disposition pour permettre aux développeurs de proposer de l’interface utilisateur Fluent à non UWP applications de bureau. Ce scénario est parfois appelé *(îles) XAML*. Pour plus d’informations sur ce scénario de développement, consultez [dans les applications de bureau, les contrôles UWP](xaml-host-controls.md).

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>Est le XAML UWP API d’hébergement pour votre application de bureau ?

Le XAML UWP API d’hébergement fournit l’infrastructure de bas niveau pour l’hébergement de contrôles UWP dans les applications de bureau. Certains types d’applications de bureau ont la possibilité d’utiliser des API alternatives et plus pratique à atteindre cet objectif.  

* Si vous avez une application de bureau Win32 C++ et que vous souhaitez dans votre application pour héberger des contrôles UWP, vous devez utiliser le XAML UWP API d’hébergement. Il n’existe aucune alternative pour ces types d’applications.

* Pour les applications WPF et Windows Forms, nous vous recommandons d’utiliser le [encapsulé les contrôles](xaml-host-controls.md#wrapped-controls) et [contrôles hôtes](xaml-host-controls.md#host-controls) dans le Kit de ressources de communauté Windows au lieu du XAML UWP API d’hébergement. Ces contrôles utilisent le XAML UWP API d’hébergement en interne et fournissent une expérience de développement plus simple. Toutefois, vous pouvez utiliser le XAML UWP API directement dans ces types d’applications d’hébergement si vous choisissez.

## <a name="related-samples"></a>Exemples connexes

Vous utilisez le XAML UWP API d’hébergement dans votre code dépend de votre type d’application, la conception de votre application et d’autres facteurs. Pour illustrer l’utilisation de cette API dans le contexte d’une application complète, cet article fait référence au code dans les exemples suivants.

### <a name="c-win32"></a>Win32 C++

Il existe plusieurs exemples qui montrent comment utiliser le XAML UWP API d’hébergement dans une application Win32 C++ sur GitHub :

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). Cet exemple montre comment ajouter la plateforme Windows universelle [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), et [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) contrôles à une application Win32 C++.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). Cet exemple montre comment ajouter plusieurs contrôles UWP base à une application Win32 C++ et de gérer les changements de PPP.

### <a name="wpf-and-windows-forms"></a>WPF et Windows Forms

Le [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) contrôle dans la boîte à outils de la Communauté Windows agit comme un échantillon de référence pour l’utilisation de la plateforme Windows universelle API d’hébergement dans les applications WPF et Windows Forms. Le code source est disponible aux emplacements suivants :

  * Pour obtenir la version WPF du contrôle, [ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). Dérive de la version WPF [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).
  * Pour obtenir la version de Windows Forms du contrôle, [ici](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). Dérive de la version de Windows Forms [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="prerequisites"></a>Conditions préalables

Le XAML UWP API d’hébergement a ces conditions préalables.

* Windows 10 Insider Preview build 17709 (ou une version ultérieure) et de la génération d’Insider Preview correspondante du SDK Windows. Générer, car il s’agit d’une fonctionnalité en constante évolution, pour une expérience optimale, que nous vous recommandons d’utiliser la dernière version disponible.

* Pour utiliser le XAML UWP API d’hébergement dans votre application de bureau, vous devez configurer votre projet afin que vous pouvez appeler des API UWP.

    * **Win32 C++ :** Nous vous recommandons de configurer votre projet pour utiliser [C++ / c++ / WinRT](../cpp-and-winrt-apis/index.md). Pour obtenir des instructions, consultez [modifier un projet d’application de bureau de Windows pour ajouter C + c++ / WinRT support](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

    * **Windows Forms et WPF :** Suivez [ces instructions](../porting/desktop-to-uwp-enhance.md).

## <a name="architecture-of-xaml-islands"></a>Architecture des îles XAML

Inclut le XAML UWP API d’hébergement [ **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [ **WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)et d’autres types connexes dans le [ **Windows.UI.Xaml.Hosting** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) espace de noms. Applications de bureau peuvent utiliser cette API pour restituer les contrôles UWP et d’itinéraire de navigation du focus clavier vers et depuis les éléments. Applications de bureau peuvent également dimensionner et positionner les contrôles UWP comme vous le souhaitez.

Lorsque vous créez un îlot de XAML à l’aide de l’API d’hébergement dans une application de bureau de XAML, vous disposerez de la hiérarchie d’objets suivante :

* Au niveau de base est l’élément d’interface utilisateur dans votre application où vous voulez héberger l’îlot de XAML. Cet élément d’interface utilisateur doit avoir un handle de fenêtre (HWND). Exemples d’éléments d’interface utilisateur dans laquelle vous pouvez héberger un îlot de XAML [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) pour les applications WPF, [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) pour les applications Windows Forms et un [fenêtre](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) pour les applications Win32 C++.

* Au prochain niveau est un **DesktopWindowXamlSource** objet. Cet objet fournit l’infrastructure pour l’hébergement de l’îlot de XAML. Votre code est responsable de la création de cet objet et la lier à l’élément d’interface parent.

* Lorsque vous créez un **DesktopWindowXamlSource**, cet objet crée automatiquement une fenêtre enfant natif pour héberger votre contrôle UWP. Cette fenêtre enfant natif est principalement dépendent plus votre code, mais vous pouvez accéder à son handle (HWND) si nécessaire.

* Enfin, au niveau supérieur est le contrôle UWP que vous souhaitez héberger dans votre application de bureau. Cela peut être n’importe quel objet UWP qui dérive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris de n’importe quel contrôle UWP fournis par le Kit de développement Windows, ainsi que les contrôles utilisateur personnalisés.

Le diagramme suivant illustre la hiérarchie des objets dans un îlot de XAML.

![Architecture de DesktopWindowXamlSource](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>Comment héberger les contrôles UWP XAML

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

    1. Créer un **DesktopWindowXamlSource** de l’objet et effectuer un cast en le **IDesktopWindowXamlSourceNative** interface COM. Cette interface est déclarée dans le ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` fichier d’en-tête dans le SDK Windows. Dans un projet Win32 C++, vous pouvez référencer directement ce fichier d’en-tête. Dans un projet WPF ou Windows Forms, vous devez déclarer cette interface dans votre code d’application en utilisant le [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) attribut. Assurez-vous que votre déclaration d’interface correspond exactement à la déclaration d’interface dans ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. Appelez le **AttachToWindow** méthode de la **IDesktopWindowXamlSourceNative** interface, puis passez le handle de fenêtre de l’élément d’interface utilisateur parent dans votre application.

    3. Définir la taille initiale de la fenêtre enfant internes contenue dans le **DesktopWindowXamlSource**. Par défaut, cette fenêtre enfant interne a une largeur et une hauteur de 0. Si vous ne définissez pas la taille de la fenêtre, tous les contrôles UWP vous ajouter à la **DesktopWindowXamlSource** ne seront pas visibles. Pour accéder à la fenêtre enfant interne dans le **DesktopWindowXamlSource**, utilisez le **WindowHandle** propriété de la **IDesktopWindowXamlSourceNative** interface. Les exemples suivants utilisent le [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) fonction permettant de définir la taille de la fenêtre.

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
    // that will act as the parent UI elemnt for your XAML island. It also assumes
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

  * **Win32 C++ :** Consultez le [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) de fichiers dans le [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) exemple ou le [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) de fichiers dans le [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) exemple.
  * **WPF :** Consultez le [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) fichiers dans la boîte à outils de la Communauté Windows.  
  * **Windows Forms :** Consultez le [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) fichiers dans la boîte à outils de la Communauté Windows.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>Hôte personnalisé UWP XAML de contrôles

> [!IMPORTANT]
> Actuellement, les contrôles UWP XAML personnalisés à partir de la 3e parties sont uniquement pris en charge dans C# applications WPF et Windows Forms. Vous devez disposer du code source pour les contrôles afin que vous pouvez compiler sur celui-ci dans votre application.

Si vous souhaitez héberger un contrôle UWP XAML personnalisé (un contrôle que vous définissez vous-même ou un contrôle fourni par un 3e partie), vous devez effectuer les tâches supplémentaires suivantes en plus de la manière décrite dans le [section précédente](#how-to-host-uwp-xaml-controls).

1. Définir un type personnalisé qui dérive de [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) et implémente également [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Ce type agit comme un fournisseur de métadonnées de racine du chargement des métadonnées pour les types UWP XAML personnalisés dans les assemblys dans le répertoire actuel de votre application.

    Pour obtenir un exemple qui montre comment effectuer cette opération, consultez le [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) fichier de code dans la boîte à outils de la Communauté Windows. Ce fichier fait partie de l’implémentation partagée de la **WindowsXamlHost** classes pour WPF et Windows Forms, qui illustrent comment utiliser le XAML UWP API dans ces types d’applications d’hébergement.

2. Appelez le [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) de votre fournisseur de métadonnées racine quand le nom de type du contrôle UWP XAML est affecté (méthode) (Cela peut être attribué dans le code en cours d’exécution, ou vous pouvez choisir d’activer cette option pour être attribué dans la fenêtre Propriétés de Visual Studio).

    Pour obtenir un exemple qui montre comment effectuer cette opération, consultez le [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) fichier de code dans la boîte à outils de la Communauté Windows. Ce fichier fait partie de l’implémentation partagée de la **WindowsXamlHost** classes pour WPF et Windows Forms.

3. Intégrer le code source pour le contrôle UWP XAML personnalisé dans votre solution d’application hôte, générer le contrôle personnalisé et utilisez-la dans votre application en suivant [ces instructions](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="how-to-handle-keyboard-focus-navigation"></a>Comment gérer la navigation du focus clavier

Lorsque l’utilisateur parcourt les éléments d’interface utilisateur dans votre application à l’aide du clavier (par exemple, en appuyant sur **onglet** ou la touche de direction haut/bas), vous devrez déplacer par programmation le focus dans et hors de la  **DesktopWindowXamlSource** objet. Lorsque la navigation au clavier de l’utilisateur atteint le **DesktopWindowXamlSource**, déplacer le focus dans la première [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objet dans l’ordre de navigation pour votre interface utilisateur, continuez à déplacer le focus à ce qui suit **Windows.UI.Xaml.UIElement** objets en tant que l’utilisateur parcourt les éléments, puis reviennent sur le focus de déplacement de la **DesktopWindowXamlSource**et dans l’élément d’interface parent.  

Le XAML UWP API d’hébergement fournit plusieurs types et membres pour vous aider à accomplir ces tâches.

1. Lorsque la navigation au clavier pénètre dans votre **DesktopWindowXamlSource**, le [ **réception focus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) événement est déclenché. Gérez cet événement et déplacer par programmation le focus au premier hébergé **Windows.UI.Xaml.UIElement** à l’aide de la [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) (méthode).

2. Lorsque l’utilisateur est sur le dernier élément pouvant être actif dans votre **DesktopWindowXamlSource** et appuie sur le **onglet** clé ou une touche de direction, la [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) événement est déclenché. Gérez cet événement et déplacer par programmation le focus vers l’élément suivant pouvant prendre le focus dans l’application hôte. Par exemple, dans une application WPF où le **DesktopWindowXamlSource** est hébergé dans un [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), vous pouvez utiliser la [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) méthode pour transférer le focus vers l’élément suivant pouvant prendre le focus dans l’application hôte.

Pour obtenir des exemples qui illustrent comment effectuer cette opération dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants :
  * **WPF :** Consultez le [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) fichier dans la boîte à outils de la Communauté Windows.  
  * **Windows Forms :** Consultez le [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) fichier dans la boîte à outils de la Communauté Windows.

## <a name="how-to-handle-layout-changes"></a>Comment gérer les changements de disposition

Lorsque l’utilisateur modifie la taille de l’élément d’interface utilisateur parent, vous devrez gérer les modifications de disposition nécessaires pour vous assurer que vos contrôles UWP afficher comme prévu. Voici quelques scénarios importants à prendre en compte.

1. Lorsque l’élément d’interface parent a besoin obtenir la taille de la zone rectangulaire nécessaire pour contenir le **Windows.UI.Xaml.UIElement** hébergés sur le **DesktopWindowXamlSource**, appelez le [ **Mesure** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) méthode de la **Windows.UI.Xaml.UIElement**. Exemple :
    * Dans une application WPF, vous pouvez faire cela à partir de la [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) méthode de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge le  **DesktopWindowXamlSource**.
    * Dans une application Windows Forms, vous pouvez faire cela à partir de la [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) méthode de la [ **contrôle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge le **DesktopWindowXamlSource**.

2. Lorsque la taille de l’élément d’interface utilisateur parent change, appelez le [ **organiser** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) méthode de la racine **Windows.UI.Xaml.UIElement** hébergés sur le  **DesktopWindowXamlSource**. Exemple :
    * Dans une application WPF, vous pouvez faire cela à partir de la [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) méthode de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) objet qui héberge le **DesktopWindowXamlSource**.
    * Dans une application Windows Forms vous pouvez faire cela à partir du gestionnaire pour le [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) événements de la [ **contrôle** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge le **DesktopWindowXamlSource**.

Pour obtenir des exemples qui illustrent comment effectuer cette opération dans le contexte d’un exemple d’application fonctionnel, consultez les fichiers de code suivants :
  * **WPF :** Consultez le [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) fichier dans la boîte à outils de la Communauté Windows.  
  * **Windows Forms :** Consultez le [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) fichier dans la boîte à outils de la Communauté Windows.

## <a name="how-to-handle-dpi-changes"></a>Comment gérer les modifications de PPP

Si vous souhaitez gérer les changements de résolution dans la fenêtre qui héberge votre UWP contrôler (par exemple, si l’utilisateur fait glisser la fenêtre entre les écrans avec un autre écran PPP), vous devez configurer le contrôle UWP avec une transformation de rendu, écoute des modifications de résolution de votre application et mettre à jour la position de la fenêtre et restituer la transformation du contrôle UWP en réponse aux modifications PPP.

Les étapes suivantes expliquent comment gérer ce processus dans le contexte d’une application Win32 C++. Pour obtenir un exemple complet, consultez la [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) et [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) code des fichiers dans le [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) exemple sur GitHub.

1. Maintenir un [ **ScaleTransform** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) de l’objet dans votre application et assignez-la à la [ **RenderTransform** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) méthode de votre contrôle UWP. L’exemple suivant effectue cette opération pour un [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) contrôle dans une application Win32 C++.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. Dans votre [ **WindowProc** ](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) de fonction, d’écouter le [ **WM_DPICHANGED** ](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) message. En réponse à ce message :
    * Utilisez le [ **SetWindowPos** ](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) fonction pour redimensionner la fenêtre qui contient le contrôle UWP vers le rectangle passé au message.
    * Mise à jour les axes x et y des facteurs de mise à l’échelle votre **ScaleTransform** objet en fonction de la nouvelle valeur en DPI.
    * Procédez aux ajustements nécessaires à l’apparence et la disposition de votre contrôle UWP. L’exemple de code suivant ajuste le [ **remplissage** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) de hébergé **Windows.UI.Xaml.Controls.Grid** contrôle en réponse aux modifications de PPP.

    ```cppwinrt
    LRESULT HandleDpiChange(HWND hWnd, WPARAM wParam, LPARAM lParam)
    {
        HWND hWndStatic = GetWindow(hWnd, GW_CHILD);
        if (hWndStatic != nullptr)
        {
            UINT uDpi = HIWORD(wParam);

            // Resize the window
            auto lprcNewScale = reinterpret_cast<RECT*>(lParam);

            SetWindowPos(hWnd, nullptr, lprcNewScale->left, lprcNewScale->top,
                lprcNewScale->right - lprcNewScale->left, lprcNewScale->bottom - lprcNewScale->top,
                SWP_NOZORDER | SWP_NOACTIVATE);

            NewScale(uDpi);
          }
          return 0;
    }

    void NewScale(UINT dpi) {

        auto scaleFactor = (float)dpi / 100;

        if (m_scale != nullptr) {
            m_scale.ScaleX(scaleFactor);
            m_scale.ScaleY(scaleFactor);
        }

        ApplyCorrection(scaleFactor);
    }

    void ApplyCorrection(float scaleFactor) {
        float rightCorrection = (m_rootGrid.Width() * scaleFactor - m_rootGrid.Width()) / scaleFactor;
        float bottomCorrection = (m_rootGrid.Height() * scaleFactor - m_rootGrid.Height()) / scaleFactor;

        m_rootGrid.Padding(Windows::UI::Xaml::ThicknessHelper::FromLengths(0, 0, rightCorrection, bottomCorrection));
    }
    ```

2. Pour configurer votre application pour connaître la résolution par moniteur, ajoutez un [manifeste d’assembly de côte à côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre projet et un ensemble ```<dpiAwareness>``` élément à ```PerMonitorV2```. Pour plus d’informations sur cette valeur, consultez la description de [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

    Pour un manifeste d’assembly de côte-à-côte exemple complet, consultez la [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) de fichiers dans le [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) exemple sur GitHub.

## <a name="limitations"></a>Limitations

L’API d’hébergement de XAML partage les mêmes limitations que tous les autres types de contrôles hôtes XAML pour Windows 10. Pour une liste détaillée, consultez [limitations de contrôle hôte XAML](xaml-host-controls.md#limitations).

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erreur à l’aide de XAML UWP API d’hébergement dans une application UWP

| Problème | Résolution |
|-------|------------|
| Votre application reçoit un **COMException** avec le message suivant : « Impossible d’activer DesktopWindowXamlSource. Ce type ne peut pas être utilisé dans une application UWP. » ou « Impossible d’activer WindowsXamlManager. Ce type ne peut pas être utilisé dans une application UWP. » | Cette erreur indique que vous essayez d’utiliser le XAML UWP API d’hébergement (en particulier, que vous tentez d’instancier le [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) types) dans une application UWP. Le XAML UWP API d’hébergement est uniquement destinée à être utilisée dans les applications de bureau non UWP, telles que les applications WPF, Windows Forms et Win32 C++. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erreur durant l’attachement à une fenêtre sur un thread différent

| Problème | Résolution |
|-------|------------|
| Votre application reçoit un **COMException** avec le message suivant : « AttachToWindow méthode a échoué, car le HWND spécifié a été créé sur un thread différent. » | Cette erreur indique que votre application a appelé la **IDesktopWindowXamlSourceNative.AttachToWindow** (méthode) et lui a transmis le HWND de la fenêtre a été créé sur un thread différent. Vous devez passer à cette méthode le HWND de la fenêtre a été créé sur le même thread que le code à partir duquel vous appelez la méthode. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erreur durant l’attachement à une fenêtre sur une autre fenêtre de niveau supérieur

| Problème | Résolution |
|-------|------------|
| Votre application reçoit un **COMException** avec le message suivant : « AttachToWindow méthode a échoué, car le HWND spécifié descend à partir d’une autre fenêtre de niveau supérieur que le HWND qui a été précédemment passé à AttachToWindow sur le même thread. » | Cette erreur indique que votre application a appelé la **IDesktopWindowXamlSourceNative.AttachToWindow** (méthode) et lui a transmis le HWND de fenêtre qui provient d’une autre fenêtre de niveau supérieur à une fenêtre que vous avez spécifié dans un appel précédent à cette méthode sur le même thread.</p></p>Une fois votre application appelle **IDesktopWindowXamlSourceNative.AttachToWindow** sur un thread particulier, tous les autres [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) objets sur le même attachement de thread peut uniquement à windows qui sont des descendants de la même fenêtre de niveau supérieur qui a été passée dans le premier appel à **IDesktopWindowXamlSourceNative.AttachToWindow**. Lorsque tous les le **DesktopWindowXamlSource** objets sont fermées pour un thread particulier, la prochaine **DesktopWindowXamlSource** est ensuite libre d’attacher à n’importe quelle fenêtre à nouveau.</p></p>Pour résoudre ce problème, fermez tous les **DesktopWindowXamlSource** les objets qui sont liées aux autres fenêtres de niveau supérieur sur ce thread, ou créent un nouveau thread pour cette **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Rubriques connexes

* [Contrôles UWP dans les applications de bureau](xaml-host-controls.md)
