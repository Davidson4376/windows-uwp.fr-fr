---
title: À l’aide de la couche visuelle avec WPF
description: Découvrez des techniques pour l’utilisation de l’API de couche visuelle en association avec un contenu WPF pour créer des animations avancées et les effets.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: a2f30ba67acc12d622acd09f9fae872ee2058a2f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215154"
---
# <a name="using-the-visual-layer-with-wpf"></a>À l’aide de la couche visuelle avec WPF

Vous pouvez utiliser les API de Composition Windows Runtime (également appelé le [couche visuelle](/windows/uwp/composition/visual-layer)) dans vos applications Windows Presentation Foundation (WPF) pour créer des expériences modernes autrement clair pour les utilisateurs de Windows 10.

Le code complet pour ce didacticiel est disponible sur GitHub : [Exemple de WPF HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition).

## <a name="prerequisites"></a>Prérequis

Le XAML UWP API d’hébergement a ces conditions préalables.

- Nous partons du principe que vous disposez des connaissances sur le développement d’applications à l’aide de WPF et UWP. Pour plus d’informations, voir :
  - [Mise en route (WPF)](/dotnet/framework/wpf/getting-started/)
  - [Prise en main les applications Windows 10](/windows/uwp/get-started/)
  - [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 ou version ultérieure
- Windows 10 version 1803 ou ultérieure
- SDK Windows 10 17134 ou version ultérieure

## <a name="how-to-use-composition-apis-in-wpf"></a>Comment utiliser les API de Composition dans WPF

Dans ce didacticiel, vous créez une application WPF simple interface utilisateur et lui ajoutez des éléments animés de Composition. Composants WPF et de Composition restent simples, mais le code interop indiqué est le même quelle que soit la complexité des composants. L’application terminée ressemble à ceci.

![L’application en cours d’exécution de l’interface utilisateur](images/visual-layer-interop/wpf-comp-interop-app-ui.png)

## <a name="create-a-wpf-project"></a>Créer un projet WPF

La première étape consiste à créer le projet d’application WPF, qui inclut une définition d’application et de la page XAML pour l’interface utilisateur.

Pour créer un nouveau projet d’Application WPF dans Visual C# nommé _HelloComposition_:

1. Ouvrez Visual Studio et sélectionnez **fichier** > **New** > **projet**.

    Le **nouveau projet** boîte de dialogue s’ouvre.
1. Sous le **installé** catégorie, développez le **Visual C#**  nœud, puis sélectionnez **Windows Desktop**.
1. Sélectionnez le **application WPF (.NET Framework)** modèle.
1. Entrez le nom _HelloComposition_, sélectionnez Framework **.NET Framework 4.7.2**, puis cliquez sur **OK**.

    Visual Studio crée le projet et ouvre le concepteur pour la fenêtre d’application par défaut nommé MainWindow.xaml.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurer le projet pour utiliser Windows Runtime APIs

Pour utiliser Windows Runtime (WinRT) API dans votre application WPF, vous devez configurer votre projet Visual Studio pour accéder à l’exécution de Windows. En outre, les vecteurs sont largement utilisées par les API de Composition, vous devez donc ajouter les références nécessaires pour utiliser des vecteurs.

Les packages NuGet sont disponibles pour répondre à ces besoins. Installez les dernières versions de ces packages pour ajouter les références nécessaires à votre projet.  

- [Microsoft.Windows.SDK.Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (nécessite l’ensemble de format de gestion de package par défaut vers PackageReference.)
- [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Bien que nous vous recommandons d’utiliser les packages NuGet pour configurer votre projet, vous pouvez ajouter manuellement les références requises. Pour plus d’informations, consultez [améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). Le tableau suivant présente les fichiers dont vous avez besoin pour ajouter des références à.

|Fichier|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program fichiers (x86) \Windows Kits\10\References\<*version_sdk*> \Windows.Foundation.UniversalApiContract\<*version*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program fichiers (x86) \Windows Kits\10\References\<*version_sdk*> \Windows.Foundation.FoundationContract\<*version*>|
|System.Numerics.Vectors.dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System.Numerics.dll|C:\Program fichiers (x86) \Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="configure-the-project-to-be-per-monitor-dpi-aware"></a>Configurer le projet pour être résolution par moniteur prenant en charge

Le contenu de la couche visuelle que vous ajoutez à votre application n’évolue pas automatiquement pour faire correspondre les paramètres PPP de l’écran sur que s’affichent. Vous devez activer la prise en charge DPI par moniteur pour votre application et assurez-vous que le code que vous utilisez pour créer votre contenu de la couche visuelle prend en compte l’échelle PPP actuelle lors de l’application s’exécute. Ici, nous configurer le projet pour être reconnaissant les résolutions. Dans les sections suivantes, nous montrons comment utiliser les informations PPP à l’échelle le contenu de la couche visuelle.

Les applications WPF sont PPP système prenant en charge par défaut, mais vous doivent déclarer eux-mêmes à être résolution par moniteur prenant en charge dans un fichier App.manifest. Pour activer la reconnaissance de résolution par moniteur de niveau de Windows dans le fichier de manifeste d’application :

1. Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le _HelloComposition_ projet.
1. Dans le menu contextuel, sélectionnez **ajouter** > **un nouvel élément...** .
1. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez « Fichier de manifeste d’Application », puis cliquez sur **ajouter**. (Vous pouvez laisser le nom par défaut.)
1. Dans le fichier App.manifest, recherchez ce xml et le commentaire il :

    ```xaml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
        </windowsSettings>
      </application>
    ```

1. Ajoutez ce paramètre après l’ouverture `<windowsSettings>` balise :

    ```xaml
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitor</dpiAwareness>
    ```

1. Vous devez également définir le **DoNotScaleForDpiChanges** dans le fichier App.config.

    Ouvrez App.Config et ajoutez ce code xml à l’intérieur du `<configuration>` élément :

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
    </runtime>
    ```

> [!NOTE]
> **AppContextSwitchOverrides** ne peut être définie qu’une seule fois. Si votre application a déjà un jeu, vous devez point-virgule délimiter ce commutateur à l’intérieur de la valeur d’attribut.

(Pour plus d’informations, consultez le [Guide du développeur PPP par moniteur et exemples](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI) sur GitHub.)

## <a name="create-an-hwndhost-derived-class-to-host-composition-elements"></a>Créer une classe dérivée de HwndHost aux éléments de composition d’hôte

Pour héberger le contenu vous créez avec la couche visuelle, vous devez créer une classe qui dérive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost). Il s’agit dans lequel vous effectuez la majeure partie de la configuration pour l’hébergement des API de Composition. Dans cette classe, vous utilisez [Platform Invocation Services (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) et [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) pour amener les API de Composition dans votre application WPF. Pour plus d’informations sur PInvoke et COM Interop, consultez [interopération avec du code non managé](/dotnet/framework/interop/index).

> [!TIP]
> Si vous le souhaitez, vérifiez le code complet à la fin du didacticiel pour vous assurer que tout le code est placées au bon endroit lorsque vous travaillez dans le didacticiel.

1. Ajoutez un nouveau fichier de classe à votre projet qui dérive de [HwndHost](/dotnet/api/system.windows.interop.hwndhost).
    - Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le _HelloComposition_ projet.
    - Dans le menu contextuel, sélectionnez **ajouter** > **classe...** .
    - Dans le **ajouter un nouvel élément** boîte de dialogue, nommez la classe _CompositionHost.cs_, puis cliquez sur **ajouter**.
1. Dans CompositionHost.cs, modifiez la définition de classe à dériver de **HwndHost**.

    ```csharp
    // Add
    // using System.Windows.Interop;

    namespace HelloComposition
    {
        class CompositionHost : HwndHost
        {
        }
    }
    ```

1. Ajoutez le code et le constructeur suivant à la classe.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    int hostHeight, hostWidth;
    object dispatcherQueue;
    ICompositionTarget compositionTarget;

    public Compositor Compositor { get; private set; }

    public Visual Child
    {
        set
        {
            if (Compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }

    internal const int
      WS_CHILD = 0x40000000,
      WS_VISIBLE = 0x10000000,
      LBS_NOTIFY = 0x00000001,
      HOST_ID = 0x00000002,
      LISTBOX_ID = 0x00000001,
      WS_VSCROLL = 0x00200000,
      WS_BORDER = 0x00800000;

    public CompositionHost(double height, double width)
    {
        hostHeight = (int)height;
        hostWidth = (int)width;
    }
    ```

1. Remplacer le **BuildWindowCore** et **DestroyWindowCore** méthodes.
    > [!NOTE]
    > Dans BuildWindowCore, vous appelez le _InitializeCoreDispatcher_ et _InitComposition_ méthodes. Vous créez ces méthodes dans les étapes suivantes.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

    protected override HandleRef BuildWindowCore(HandleRef hwndParent)
    {
        // Create Window
        hwndHost = IntPtr.Zero;
        hwndHost = CreateWindowEx(0, "static", "",
                                  WS_CHILD | WS_VISIBLE,
                                  0, 0,
                                  hostWidth, hostHeight,
                                  hwndParent.Handle,
                                  (IntPtr)HOST_ID,
                                  IntPtr.Zero,
                                  0);

        // Create Dispatcher Queue
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content
        InitComposition(hwndHost);

        return new HandleRef(this, hwndHost);
    }

    protected override void DestroyWindowCore(HandleRef hwnd)
    {
        if (compositionTarget.Root != null)
        {
            compositionTarget.Root.Dispose();
        }
        DestroyWindow(hwnd.Handle);
    }
    ```

    - [CreateWindowEx](/windows/desktop/api/winuser/nf-winuser-createwindowexa) et [DestroyWindow](/windows/desktop/api/winuser/nf-winuser-destroywindow) nécessitent une déclaration de PInvoke. Placez cette déclaration à la fin du code pour la classe.

    ```csharp
    #region PInvoke declarations

    [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                  string lpszClassName,
                                                  string lpszWindowName,
                                                  int style,
                                                  int x, int y,
                                                  int width, int height,
                                                  IntPtr hwndParent,
                                                  IntPtr hMenu,
                                                  IntPtr hInst,
                                                  [MarshalAs(UnmanagedType.AsAny)] object pvParam);

    [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
    internal static extern bool DestroyWindow(IntPtr hwnd);

    #endregion PInvoke declarations
    ```

1. Initialiser un thread avec une [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). Le répartiteur de core est responsable du traitement des messages de fenêtre et la distribution des événements pour WinRT APIs. Nouvelles instances de **CoreDispatcher** doit être créé sur un thread qui comporte un CoreDispatcher.
    - Créez une méthode nommée _InitializeCoreDispatcher_ et ajoutez le code pour configurer la file d’attente du répartiteur.

    ```csharp
    private object InitializeCoreDispatcher()
    {
        DispatcherQueueOptions options = new DispatcherQueueOptions();
        options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
        options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
        options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

        object queue = null;
        CreateDispatcherQueueController(options, out queue);
        return queue;
    }
    ```

    - La file d’attente du répartiteur requiert également une déclaration de PInvoke. Placez cette déclaration à l’intérieur de la _déclarations PInvoke_ région que vous avez créé à l’étape précédente.

    ```csharp
    //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    //{
    //    DQTAT_COM_NONE,
    //    DQTAT_COM_ASTA,
    //    DQTAT_COM_STA
    //};
    internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
    {
        DQTAT_COM_NONE = 0,
        DQTAT_COM_ASTA = 1,
        DQTAT_COM_STA = 2
    };

    //typedef enum DISPATCHERQUEUE_THREAD_TYPE
    //{
    //    DQTYPE_THREAD_DEDICATED,
    //    DQTYPE_THREAD_CURRENT
    //};
    internal enum DISPATCHERQUEUE_THREAD_TYPE
    {
        DQTYPE_THREAD_DEDICATED = 1,
        DQTYPE_THREAD_CURRENT = 2,
    };

    //struct DispatcherQueueOptions
    //{
    //    DWORD dwSize;
    //    DISPATCHERQUEUE_THREAD_TYPE threadType;
    //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    //};
    [StructLayout(LayoutKind.Sequential)]
    internal struct DispatcherQueueOptions
    {
        public int dwSize;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_TYPE threadType;

        [MarshalAs(UnmanagedType.I4)]
        public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
    };

    //HRESULT CreateDispatcherQueueController(
    //  DispatcherQueueOptions options,
    //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
    //);
    [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
    internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                            [MarshalAs(UnmanagedType.IUnknown)]
                                            out object dispatcherQueueController);
    ```

    Vous avez la file d’attente du répartiteur prêt et pouvez commencer à initialiser et créer la Composition de contenu.

1. Initialiser le [compositeur](/uwp/api/windows.ui.composition.compositor). Le constructeur est une fabrique qui crée une variété de types dans le [Windows.UI.Composition](/uwp/api/windows.ui.composition) espace de noms s’étendant sur les éléments visuels, le système effets et le système d’animation. La classe compositeur gère également la durée de vie des objets créés à partir de la fabrique.

    ```csharp
    private void InitComposition(IntPtr hwndHost)
    {
        ICompositorDesktopInterop interop;

        compositor = new Compositor();
        object iunknown = compositor as object;
        interop = (ICompositorDesktopInterop)iunknown;
        IntPtr raw;
        interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

        object rawObject = Marshal.GetObjectForIUnknown(raw);
        ICompositionTarget target = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }
    }
    ```

    - **ICompositorDesktopInterop** et **ICompositionTarget** nécessitent des importations de COM. Placez ce code après le _CompositionHost_ classe, mais à l’intérieur la déclaration d’espace de noms.

    ```csharp
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
    ```

## <a name="create-a-usercontrol-to-add-your-content-to-the-wpf-visual-tree"></a>Créer un UserControl pour ajouter votre contenu à l’arborescence visuelle de WPF

La dernière étape pour configurer l’infrastructure nécessaire à l’hôte de Composition contenu consiste à ajouter le HwndHost à l’arborescence d’éléments visuels WPF.

### <a name="create-a-usercontrol"></a>Créer un UserControl

Un UserControl est un moyen pratique pour empaqueter votre code qui crée et gère le contenu de la Composition et facilement ajouter le contenu à votre XAML.

1. Ajouter un nouveau fichier de contrôle utilisateur à votre projet.
    - Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le _HelloComposition_ projet.
    - Dans le menu contextuel, sélectionnez **ajouter** > **contrôle utilisateur...** .
    - Dans le **ajouter un nouvel élément** boîte de dialogue, nommez le contrôle utilisateur _CompositionHostControl.xaml_, puis cliquez sur **ajouter**.

    Les CompositionHostControl.xaml CompositionHostControl.xaml.cs fichiers et sont créés et ajoutés à votre projet.
1. Dans CompositionHostControl.xaml, remplacez le `<Grid> </Grid>` balises avec ce **bordure** élément, qui est le conteneur XAML qui décrivent votre HwndHost.

    ```xaml
    <Border Name="CompositionHostElement"/>
    ```

Dans le code pour le contrôle utilisateur, vous créez une instance de la classe CompositionHost que vous avez créé à l’étape précédente et l’ajouter comme élément enfant de _CompositionHostElement_, la bordure que vous avez créé dans la page XAML.

1. Dans CompositionHostControl.xaml.cs, ajoutez les variables privées pour les objets que vous utiliserez dans votre code de Composition. Ajoutez ces après la définition de classe.

    ```csharp
    CompositionHost compositionHost;
    Compositor compositor;
    Windows.UI.Composition.ContainerVisual containerVisual;
    DpiScale currentDpi;
    ```

1. Ajoutez un gestionnaire pour le contrôle utilisateur **Loaded** événement. Voici où vous configurez votre instance de CompositionHost.

    - Dans le constructeur, raccorder le Gestionnaire d’événements comme indiqué ici (`Loaded += CompositionHostControl_Loaded;`).

    ```csharp
    public CompositionHostControl()
    {
        InitializeComponent();
        Loaded += CompositionHostControl_Loaded;
    }
    ```

    - Ajoutez la méthode de gestionnaire d’événements portant le nom *CompositionHostControl_Loaded*.
    ```csharp
    private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
    {
        // If the user changes the DPI scale setting for the screen the app is on,
        // the CompositionHostControl is reloaded. Don't redo this set up if it's
        // already been done.
        if (compositionHost is null)
        {
            currentDpi = VisualTreeHelper.GetDpi(this);

            compositionHost =
                new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
            ControlHostElement.Child = compositionHost;
            compositor = compositionHost.Compositor;
            containerVisual = compositor.CreateContainerVisual();
            compositionHost.Child = containerVisual;
        }
    }
    ```

    Dans cette méthode, configurer les objets que vous utiliserez dans votre code de Composition. Voici un rapide coup de œil à ce qui se passe.

    - Tout d’abord, assurez-vous que la configuration est uniquement effectuées une seule fois en vérifiant si une instance de CompositionHost existe déjà.

    ```csharp
    // If the user changes the DPI scale setting for the screen the app is on,
    // the CompositionHostControl is reloaded. Don't redo this set up if it's
    // already been done.
    if (compositionHost is null)
    {

    }
    ```

    - Obtenir la résolution actuelle. Cela permet de correctement mettre à l’échelle de vos éléments de Composition.

    ```csharp
    currentDpi = VisualTreeHelper.GetDpi(this);
    ```

    - Créer une instance de CompositionHost et lui attribuer comme enfant de la bordure, _CompositionHostElement_.

    ```csharp
    compositionHost =
        new CompositionHost(ControlHostElement.ActualHeight, ControlHostElement.ActualWidth);
    ControlHostElement.Child = compositionHost;
    ```

    - Obtenir le compositeur à partir de la CompositionHost.

    ```csharp
    compositor = compositionHost.Compositor;
    ```

    - Pour créer un conteneur visuel, utilisez le compositeur. Il s’agit du conteneur de Composition que vous ajoutez vos éléments de Composition.

    ```csharp
    containerVisual = compositor.CreateContainerVisual();
    compositionHost.Child = containerVisual;
    ```

### <a name="add-composition-elements"></a>Ajouter des éléments de composition

Avec l’infrastructure en place, vous pouvez maintenant générer le contenu de Composition que vous souhaitez afficher.

Pour cet exemple, vous ajoutez le code qui crée et anime un carré simple [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Ajouter un élément de composition. Dans CompositionHostControl.xaml.cs, ajoutez ces méthodes à la classe CompositionHostControl.

    ```csharp
    // Add
    // using System.Numerics;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size);
        visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

        // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
        // with the bottom of the host container. This is the value to animate to.
        var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
        var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
        float bottom = (float)(hostHeightAdj - squareSizeAdj);

        // Create the animation only if it's needed.
        if (visual.Offset.Y != bottom)
        {
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
        }
    }

    private Windows.UI.Color GetRandomColor()
    {
        Random random = new Random();
        byte r = (byte)random.Next(0, 255);
        byte g = (byte)random.Next(0, 255);
        byte b = (byte)random.Next(0, 255);
        return Windows.UI.Color.FromArgb(255, r, g, b);
    }
    ```

### <a name="handle-dpi-changes"></a>Gérer les changements de résolution

Le code pour ajouter et animer un élément prend en compte l’échelle PPP actuelle lors de la création d’éléments, mais vous devez également prendre en compte les modifications de PPP pendant l’exécution de l’application. Vous pouvez gérer le [HwndHost.DpiChanged](/dotnet/api/system.windows.interop.hwndhost.dpichanged) événement pour être averti des modifications et ajuster vos calculs basé sur la nouvelle résolution.

1. Dans la méthode CompositionHostControl_Loaded, après la dernière ligne, ajoutez cette option pour raccorder le Gestionnaire d’événements DpiChanged.

    ```csharp
    compositionHost.DpiChanged += CompositionHost_DpiChanged;
    ```

1. Ajoutez la méthode de gestionnaire d’événements portant le nom _CompositionHostDpiChanged_. Ce code s’ajuste la mise à l’échelle et le décalage de chaque élément et recalcule les animations ne sont pas terminées.

    ```csharp
    private void CompositionHost_DpiChanged(object sender, DpiChangedEventArgs e)
    {
        currentDpi = e.NewDpi;
        Vector3 newScale = new Vector3((float)e.NewDpi.DpiScaleX, (float)e.NewDpi.DpiScaleY, 1);

        foreach (SpriteVisual child in containerVisual.Children)
        {
            child.Scale = newScale;
            var newOffsetX = child.Offset.X * ((float)e.NewDpi.DpiScaleX / (float)e.OldDpi.DpiScaleX);
            var newOffsetY = child.Offset.Y * ((float)e.NewDpi.DpiScaleY / (float)e.OldDpi.DpiScaleY);
            child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

            // Adjust animations for DPI change.
            AnimateSquare(child, 0);
        }
    }
    ```

## <a name="add-the-user-control-to-your-xaml-page"></a>Ajoutez le contrôle utilisateur à votre page XAML

Maintenant, vous pouvez ajouter le contrôle utilisateur à votre UI XAML.

1. Dans MainWindow.xaml, affectez à la hauteur de fenêtre à 600 et une largeur de 840.
1. Ajoutez le XAML pour l’interface utilisateur. Dans MainWindow.xaml, ajoutez ce XAML entre la racine `<Grid> </Grid>` balises.

    ```xaml
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="210"/>
        <ColumnDefinition Width="600"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="46"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <Button Content="Add composition element" Click="Button_Click"
            Grid.Row="1" Margin="12,0"
            VerticalAlignment="Top" Height="40"/>
    <TextBlock Text="Composition content" FontSize="20"
               Grid.Column="1" Margin="0,12,0,4"
               HorizontalAlignment="Center"/>
    <local:CompositionHostControl x:Name="CompositionHostControl1"
                                  Grid.Row="1" Grid.Column="1"
                                  VerticalAlignment="Top"
                                  Width="600" Height="500"
                                  BorderBrush="LightGray"
                                  BorderThickness="3"/>
    ```

1. Gérer le clic de bouton pour créer de nouveaux éléments. (L’événement Click est déjà raccordé dans le XAML.)

    Dans MainWindow.xaml.cs, ajoutez *Button_Click* méthode de gestionnaire d’événements. Ce code appelle _CompositionHost.AddElement_ pour créer un nouvel élément avec une taille généré de manière aléatoire et le décalage.

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
        float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
        CompositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Vous pouvez maintenant générer et exécuter votre application WPF. Si vous le souhaitez, vérifiez le code complet à la fin du didacticiel afin de vous assurer que tout le code est placées au bon endroit.

Lorsque vous exécutez l’application et cliquez sur le bouton, vous devez voir des carrés animés ajoutés à l’interface utilisateur.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un exemple plus complet qui s’appuie sur la même infrastructure, consultez le [WPF Visual exemple d’intégration de couche](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration) sur GitHub.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Mise en route (WPF)](/dotnet/framework/wpf/getting-started/) (.NET)
- [Interopérabilité avec du code non managé](/dotnet/framework/interop/) (.NET)
- [Prise en main les applications Windows 10](/windows/uwp/get-started/) (UWP)
- [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espace de noms Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Code complet

Voici le code complet pour ce didacticiel.

### <a name="mainwindowxaml"></a>MainWindow.xaml

```xaml
<Window x:Class="HelloComposition.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloComposition"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="840">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="210"/>
            <ColumnDefinition Width="600"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="46"/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Button Content="Add composition element" Click="Button_Click"
                Grid.Row="1" Margin="12,0"
                VerticalAlignment="Top" Height="40"/>
        <TextBlock Text="Composition content" FontSize="20"
                   Grid.Column="1" Margin="0,12,0,4"
                   HorizontalAlignment="Center"/>
        <local:CompositionHostControl x:Name="CompositionHostControl1"
                                      Grid.Row="1" Grid.Column="1"
                                      VerticalAlignment="Top"
                                      Width="600" Height="500"
                                      BorderBrush="LightGray" BorderThickness="3"/>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

```csharp
using System;
using System.Windows;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(CompositionHostControl1.ActualWidth - size));
            float offsetY = random.Next(0, (int)(CompositionHostControl1.ActualHeight/2 - size));
            CompositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolxaml"></a>CompositionHostControl.xaml

```xaml
<UserControl x:Class="HelloComposition.CompositionHostControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:local="clr-namespace:HelloComposition"
             mc:Ignorable="d"
             d:DesignHeight="450" d:DesignWidth="800">
    <Border Name="CompositionHostElement"/>
</UserControl>
```

### <a name="compositionhostcontrolxamlcs"></a>CompositionHostControl.xaml.cs

```csharp
using System;
using System.Numerics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Media;
using Windows.UI.Composition;

namespace HelloComposition
{
    /// <summary>
    /// Interaction logic for CompositionHostControl.xaml
    /// </summary>
    public partial class CompositionHostControl : UserControl
    {
        CompositionHost compositionHost;
        Compositor compositor;
        Windows.UI.Composition.ContainerVisual containerVisual;
        DpiScale currentDpi;

        public CompositionHostControl()
        {
            InitializeComponent();
            Loaded += CompositionHostControl_Loaded;
        }

        private void CompositionHostControl_Loaded(object sender, RoutedEventArgs e)
        {
            // If the user changes the DPI scale setting for the screen the app is on,
            // the CompositionHostControl is reloaded. Don't redo this set up if it's
            // already been done.
            if (compositionHost is null)
            {
                currentDpi = VisualTreeHelper.GetDpi(this);

                compositionHost = new CompositionHost(CompositionHostElement.ActualHeight, CompositionHostElement.ActualWidth);
                CompositionHostElement.Child = compositionHost;
                compositor = compositionHost.Compositor;
                containerVisual = compositor.CreateContainerVisual();
                compositionHost.Child = containerVisual;
            }
        }

        protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi)
        {
            base.OnDpiChanged(oldDpi, newDpi);
            currentDpi = newDpi;
            Vector3 newScale = new Vector3((float)newDpi.DpiScaleX, (float)newDpi.DpiScaleY, 1);

            foreach (SpriteVisual child in containerVisual.Children)
            {
                child.Scale = newScale;
                var newOffsetX = child.Offset.X * ((float)newDpi.DpiScaleX / (float)oldDpi.DpiScaleX);
                var newOffsetY = child.Offset.Y * ((float)newDpi.DpiScaleY / (float)oldDpi.DpiScaleY);
                child.Offset = new Vector3(newOffsetX, newOffsetY, 1);

                // Adjust animations for DPI change.
                AnimateSquare(child, 0);
            }
        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size);
            visual.Scale = new Vector3((float)currentDpi.DpiScaleX, (float)currentDpi.DpiScaleY, 1);
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX * (float)currentDpi.DpiScaleX, offsetY * (float)currentDpi.DpiScaleY, 0);

            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X); // Already adjusted for DPI.

            // Adjust values for DPI scale, then find the Y offset that aligns the bottom of the square
            // with the bottom of the host container. This is the value to animate to.
            var hostHeightAdj = CompositionHostElement.ActualHeight * currentDpi.DpiScaleY;
            var squareSizeAdj = visual.Size.Y * currentDpi.DpiScaleY;
            float bottom = (float)(hostHeightAdj - squareSizeAdj);

            // Create the animation only if it's needed.
            if (visual.Offset.Y != bottom)
            {
                Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
                animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
                animation.Duration = TimeSpan.FromSeconds(2);
                animation.DelayTime = TimeSpan.FromSeconds(delay);
                visual.StartAnimation("Offset", animation);
            }
        }

        private Windows.UI.Color GetRandomColor()
        {
            Random random = new Random();
            byte r = (byte)random.Next(0, 255);
            byte g = (byte)random.Next(0, 255);
            byte b = (byte)random.Next(0, 255);
            return Windows.UI.Color.FromArgb(255, r, g, b);
        }
    }
}

```

### <a name="compositionhostcs"></a>CompositionHost.cs

```csharp
using System;
using System.Runtime.InteropServices;
using System.Windows.Interop;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHost : HwndHost
    {
        IntPtr hwndHost;
        int hostHeight, hostWidth;
        object dispatcherQueue;
        ICompositionTarget compositionTarget;

        public Compositor Compositor { get; private set; }

        public Visual Child
        {
            set
            {
                if (Compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        internal const int
          WS_CHILD = 0x40000000,
          WS_VISIBLE = 0x10000000,
          LBS_NOTIFY = 0x00000001,
          HOST_ID = 0x00000002,
          LISTBOX_ID = 0x00000001,
          WS_VSCROLL = 0x00200000,
          WS_BORDER = 0x00800000;

        public CompositionHost(double height, double width)
        {
            hostHeight = (int)height;
            hostWidth = (int)width;
        }

        protected override HandleRef BuildWindowCore(HandleRef hwndParent)
        {
            // Create Window
            hwndHost = IntPtr.Zero;
            hwndHost = CreateWindowEx(0, "static", "",
                                      WS_CHILD | WS_VISIBLE,
                                      0, 0,
                                      hostWidth, hostHeight,
                                      hwndParent.Handle,
                                      (IntPtr)HOST_ID,
                                      IntPtr.Zero,
                                      0);

            // Create Dispatcher Queue
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition Tree of content
            InitComposition(hwndHost);

            return new HandleRef(this, hwndHost);
        }

        protected override void DestroyWindowCore(HandleRef hwnd)
        {
            if (compositionTarget.Root != null)
            {
                compositionTarget.Root.Dispose();
            }
            DestroyWindow(hwnd.Handle);
        }

        private object InitializeCoreDispatcher()
        {
            DispatcherQueueOptions options = new DispatcherQueueOptions();
            options.apartmentType = DISPATCHERQUEUE_THREAD_APARTMENTTYPE.DQTAT_COM_STA;
            options.threadType = DISPATCHERQUEUE_THREAD_TYPE.DQTYPE_THREAD_CURRENT;
            options.dwSize = Marshal.SizeOf(typeof(DispatcherQueueOptions));

            object queue = null;
            CreateDispatcherQueueController(options, out queue);
            return queue;
        }

        private void InitComposition(IntPtr hwndHost)
        {
            ICompositorDesktopInterop interop;

            Compositor = new Compositor();
            object iunknown = Compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }
        }

        #region PInvoke declarations

        //typedef enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        //{
        //    DQTAT_COM_NONE,
        //    DQTAT_COM_ASTA,
        //    DQTAT_COM_STA
        //};
        internal enum DISPATCHERQUEUE_THREAD_APARTMENTTYPE
        {
            DQTAT_COM_NONE = 0,
            DQTAT_COM_ASTA = 1,
            DQTAT_COM_STA = 2
        };

        //typedef enum DISPATCHERQUEUE_THREAD_TYPE
        //{
        //    DQTYPE_THREAD_DEDICATED,
        //    DQTYPE_THREAD_CURRENT
        //};
        internal enum DISPATCHERQUEUE_THREAD_TYPE
        {
            DQTYPE_THREAD_DEDICATED = 1,
            DQTYPE_THREAD_CURRENT = 2,
        };

        //struct DispatcherQueueOptions
        //{
        //    DWORD dwSize;
        //    DISPATCHERQUEUE_THREAD_TYPE threadType;
        //    DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        //};
        [StructLayout(LayoutKind.Sequential)]
        internal struct DispatcherQueueOptions
        {
            public int dwSize;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_TYPE threadType;

            [MarshalAs(UnmanagedType.I4)]
            public DISPATCHERQUEUE_THREAD_APARTMENTTYPE apartmentType;
        };

        //HRESULT CreateDispatcherQueueController(
        //  DispatcherQueueOptions options,
        //  ABI::Windows::System::IDispatcherQueueController** dispatcherQueueController
        //);
        [DllImport("coremessaging.dll", EntryPoint = "CreateDispatcherQueueController", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateDispatcherQueueController(DispatcherQueueOptions options,
                                                [MarshalAs(UnmanagedType.IUnknown)]
                                               out object dispatcherQueueController);


        [DllImport("user32.dll", EntryPoint = "CreateWindowEx", CharSet = CharSet.Unicode)]
        internal static extern IntPtr CreateWindowEx(int dwExStyle,
                                                      string lpszClassName,
                                                      string lpszWindowName,
                                                      int style,
                                                      int x, int y,
                                                      int width, int height,
                                                      IntPtr hwndParent,
                                                      IntPtr hMenu,
                                                      IntPtr hInst,
                                                      [MarshalAs(UnmanagedType.AsAny)] object pvParam);

        [DllImport("user32.dll", EntryPoint = "DestroyWindow", CharSet = CharSet.Unicode)]
        internal static extern bool DestroyWindow(IntPtr hwnd);


        #endregion PInvoke declarations

    }
    #region COM Interop

    /*
    #undef INTERFACE
    #define INTERFACE ICompositorDesktopInterop
        DECLARE_INTERFACE_IID_(ICompositorDesktopInterop, IUnknown, "29E691FA-4567-4DCA-B319-D0F207EB6807")
        {
            IFACEMETHOD(CreateDesktopWindowTarget)(
                _In_ HWND hwndTarget,
                _In_ BOOL isTopmost,
                _COM_Outptr_ IDesktopWindowTarget * *result
                ) PURE;
        };
    */
    [ComImport]
    [Guid("29E691FA-4567-4DCA-B319-D0F207EB6807")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface ICompositorDesktopInterop
    {
        void CreateDesktopWindowTarget(IntPtr hwndTarget, bool isTopmost, out IntPtr test);
    }

    //[contract(Windows.Foundation.UniversalApiContract, 2.0)]
    //[exclusiveto(Windows.UI.Composition.CompositionTarget)]
    //[uuid(A1BEA8BA - D726 - 4663 - 8129 - 6B5E7927FFA6)]
    //interface ICompositionTarget : IInspectable
    //{
    //    [propget] HRESULT Root([out] [retval] Windows.UI.Composition.Visual** value);
    //    [propput] HRESULT Root([in] Windows.UI.Composition.Visual* value);
    //}

    [ComImport]
    [Guid("A1BEA8BA-D726-4663-8129-6B5E7927FFA6")]
    [InterfaceType(ComInterfaceType.InterfaceIsIInspectable)]
    public interface ICompositionTarget
    {
        Windows.UI.Composition.Visual Root
        {
            get;
            set;
        }
    }

    #endregion COM Interop
}
```