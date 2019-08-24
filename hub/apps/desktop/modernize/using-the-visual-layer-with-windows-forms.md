---
title: Utilisation de la couche visuelle avec Windows Forms
description: Découvrez les techniques permettant d’utiliser les API de la couche visuelle en combinaison avec le contenu Windows Forms existant pour créer des animations et des effets avancés.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 9da9dee48beef6e3c1cd38ffbe9761ed89fd940d
ms.sourcegitcommit: 93d0b2996b4742b33cd6d641e036f42672cf5238
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69999641"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>Utilisation de la couche visuelle avec Windows Forms

Vous pouvez utiliser Windows Runtime API de composition (également appelée [couche visuelle](/windows/uwp/composition/visual-layer)) dans vos applications Windows Forms pour créer des expériences modernes qui s’illuminent pour les utilisateurs de Windows 10.

Le code complet de ce didacticiel est disponible sur GitHub: [Windows Forms exemple HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Prérequis

Ces conditions préalables sont requises pour l’API d’hébergement UWP.

- Nous partons du principe que vous êtes familiarisé avec le développement d’applications à l’aide de Windows Forms et UWP. Pour plus d’informations, voir :
  - [Bien démarrer avec Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Prise en main des applications Windows 10](/windows/uwp/get-started/)
  - [Améliorez votre application bureautique pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET Framework 4.7.2 ou version ultérieure
- Windows 10 version 1803 ou ultérieure
- Windows 10 SDK 17134 ou version ultérieure

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Comment utiliser les API de composition dans Windows Forms

Dans ce didacticiel, vous allez créer une interface utilisateur simple Windows Forms et y ajouter des éléments de composition animés. Les Windows Forms et les composants de composition restent simples, mais le code d’interopérabilité indiqué est le même, quelle que soit la complexité des composants. L’application terminée ressemble à ce qui suit.

![Interface utilisateur d’application en cours d’exécution](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Créer un projet Windows Forms

La première étape consiste à créer le projet d’application Windows Forms, qui comprend une définition d’application et le formulaire principal de l’interface utilisateur.

Pour créer un projet d’application Windows Forms dans le C# visuel nommé _HelloComposition_:

1. Ouvrez Visual Studio et sélectionnez **fichier** > **nouveau** > **projet**.<br/>La boîte **de dialogue Nouveau projet** s’ouvre.
1. Sous la catégorie **installé** , développez le nœud  **C# visuel** , puis sélectionnez **Bureau Windows**.
1. Sélectionnez le modèle d' **application Windows Forms (.NET Framework)** .
1. Entrez le nom _HelloComposition,_ sélectionnez Framework **.NET Framework 4.7.2**, puis cliquez sur **OK**.

Visual Studio crée le projet et ouvre le concepteur pour la fenêtre d’application par défaut nommée Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurer le projet pour qu’il utilise des API Windows Runtime

Pour utiliser des API Windows Runtime (WinRT) dans votre application Windows Forms, vous devez configurer votre projet Visual Studio pour accéder à la Windows Runtime. En outre, les vecteurs sont largement utilisés par les API de composition. vous devez donc ajouter les références requises pour utiliser des vecteurs.

Des packages NuGet sont disponibles pour répondre à ces besoins. Installez les versions les plus récentes de ces packages pour ajouter les références nécessaires à votre projet.  

- [Microsoft. Windows. Sdk. Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) (nécessite le format de gestion de package par défaut défini sur PackageReference.)
- [System. Numerics. vectors](https://www.nuget.org/packages/System.Numerics.Vectors/)

> [!NOTE]
> Bien que nous vous recommandons d’utiliser les packages NuGet pour configurer votre projet, vous pouvez ajouter manuellement les références requises. Pour plus d’informations, consultez [améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance). Le tableau suivant répertorie les fichiers auxquels vous devez ajouter des références.

|Fichier|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\<*SDK version*> \Windows.Foundation.UniversalApiContract\<>|
|Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86) \Windows Kits\10\References\<*SDK version*> \Windows.Foundation.FoundationContract\<>|
|System. Numerics. vectors. dll|C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Numerics.Vectors\v4.0_4.0.0.0__b03f5f7f11d50a3a|
|System. Numerics. dll|C:\Program Files (x86) \Reference Assemblies\Microsoft\Framework\.NETFramework\v4.7.2|

## <a name="create-a-custom-control-to-manage-interop"></a>Créer un contrôle personnalisé pour gérer l’interopérabilité

Pour héberger le contenu que vous créez avec la couche visuelle, vous créez un contrôle personnalisé dérivé de [Control](/dotnet/api/system.windows.forms.control). Ce contrôle vous permet d’accéder à un [handle](/dotnet/api/system.windows.forms.control.handle)de fenêtre dont vous avez besoin pour créer le conteneur pour le contenu de votre couche visuelle.

C’est là que vous effectuez la majeure partie de la configuration pour l’hébergement des API de composition. Dans ce contrôle, vous utilisez [PInvoke (Platform Invocation Services)](/cpp/dotnet/calling-native-functions-from-managed-code) et [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) pour intégrer des API de Composition à votre application Windows Forms. Pour plus d’informations sur PInvoke et COM Interop, consultez interopérabilité [avec du code non managé](/dotnet/framework/interop/index).

> [!TIP]
> Si nécessaire, consultez le code complet à la fin du didacticiel pour vous assurer que tout le code se trouve à l’emplacement approprié au fur et à mesure que vous parcourez le didacticiel.

1. Ajoutez un nouveau fichier de contrôle personnalisé à votre projet qui dérive de [Control](/dotnet/api/system.windows.forms.control).
    - Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet _HelloComposition_ .
    - Dans le menu contextuel, sélectionnez **Ajouter** > **un nouvel élément...** .
    - Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **contrôle personnalisé**.
    - Nommez le contrôle _CompositionHost.cs_, puis cliquez sur **Ajouter**. CompositionHost.cs s’ouvre dans le Mode Création.

1. Passez en mode code pour CompositionHost.cs et ajoutez le code suivant à la classe.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    IntPtr hwndHost;
    object dispatcherQueue;
    protected ContainerVisual containerVisual;
    protected Compositor compositor;

    private ICompositionTarget compositionTarget;

    public Visual Child
    {
        set
        {
            if (compositor == null)
            {
                InitComposition(hwndHost);
            }
            compositionTarget.Root = value;
        }
    }
    ```

1. Ajoutez du code au constructeur.

    Dans le constructeur, vous appelez les méthodes _InitializeCoreDispatcher_ et _InitComposition_ . Vous créez ces méthodes dans les étapes suivantes.

    ```csharp
    public CompositionHost()
    {
        InitializeComponent();

        // Get the window handle.
        hwndHost = Handle;

        // Create dispatcher queue.
        dispatcherQueue = InitializeCoreDispatcher();

        // Build Composition tree of content.
        InitComposition(hwndHost);
    }
    ```

1. Initialise un thread avec un [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). Le répartiteur principal est chargé de traiter les messages de fenêtre et de distribuer les événements pour les API WinRT. Les nouvelles instances de **compositeur** doivent être créées sur un thread qui a un CoreDispatcher.
    - Créez une méthode nommée _InitializeCoreDispatcher_ et ajoutez du code pour configurer la file d’attente du répartiteur.

    ```csharp
    // Add
    // using System.Runtime.InteropServices;

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

    - La file d’attente du répartiteur requiert une déclaration PInvoke. Placez cette déclaration à la fin du code pour la classe. (Nous allons placer ce code dans une région pour que le code de classe reste Tidy.)

    ```csharp
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

    #endregion PInvoke declarations
    ```

    La file d’attente du répartiteur est maintenant prête et peut commencer à initialiser et à créer le contenu de la composition.

1. Initialisez le [compositeur](/uwp/api/windows.ui.composition.compositor). Le compositeur est une fabrique qui crée un grand nombre de types dans l’espace de noms [Windows. UI. composition](/uwp/api/windows.ui.composition) , couvrant la couche visuelle, le système d’effets et le système d’animation. La classe compository gère également la durée de vie des objets créés à partir de la fabrique.

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
        compositionTarget = (ICompositionTarget)rawObject;

        if (raw == null) { throw new Exception("QI Failed"); }

        containerVisual = compositor.CreateContainerVisual();
        Child = containerVisual;
    }
    ```

    - **ICompositorDesktopInterop** et **ICompositionTarget** requièrent les importations com. Placez ce code après la classe _CompositionHost_ , mais à l’intérieur de la déclaration d’espace de noms.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Créer un contrôle personnalisé pour héberger des éléments de composition

Il est judicieux de placer le code qui génère et gère vos éléments de composition dans un contrôle distinct qui dérive de CompositionHost. Cela permet de conserver le code d’interopérabilité que vous avez créé dans la classe CompositionHost réutilisable.

Ici, vous créez un contrôle personnalisé dérivé de CompositionHost. Ce contrôle est ajouté à la boîte à outils Visual Studio afin que vous puissiez l’ajouter à votre formulaire.

1. Ajoutez un nouveau fichier de contrôle personnalisé à votre projet qui dérive de CompositionHost.
    - Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet _HelloComposition_ .
    - Dans le menu contextuel, sélectionnez **Ajouter** > **un nouvel élément...** .
    - Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **contrôle personnalisé**.
    - Nommez le contrôle _CompositionHostControl.cs_, puis cliquez sur **Ajouter**. CompositionHostControl.cs s’ouvre dans le Mode Création.

1. Dans le volet Propriétés pour CompositionHostControl.cs mode Design, définissez la propriété **BackColor** sur **ControlLight**.

    La définition de la couleur d’arrière-plan est facultative. Nous le faisons ici pour que vous puissiez voir votre contrôle personnalisé par rapport à l’arrière-plan du formulaire.

1. Basculez en mode code pour CompositionHostControl.cs et mettez à jour la déclaration de classe pour qu’elle dérive de CompositionHost.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Mettez à jour le constructeur pour appeler le constructeur de base.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Ajouter des éléments de composition

Une fois l’infrastructure en place, vous pouvez maintenant ajouter du contenu de composition à l’interface utilisateur de l’application.

Pour cet exemple, vous ajoutez du code à la classe CompositionHostControl qui crée et anime un [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)simple.

1. Ajoutez un élément de composition.

    Dans CompositionHostControl.cs, ajoutez ces méthodes à la classe CompositionHostControl.

    ```csharp
    // Add
    // using Windows.UI.Composition;

    public void AddElement(float size, float offsetX, float offsetY)
    {
        var visual = compositor.CreateSpriteVisual();
        visual.Size = new Vector2(size, size); // Requires references
        visual.Brush = compositor.CreateColorBrush(GetRandomColor());
        visual.Offset = new Vector3(offsetX, offsetY, 0);
        containerVisual.Children.InsertAtTop(visual);

        AnimateSquare(visual, 3);
    }

    private void AnimateSquare(SpriteVisual visual, int delay)
    {
        float offsetX = (float)(visual.Offset.X);
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        float bottom = Height - visual.Size.Y;
        animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
        animation.Duration = TimeSpan.FromSeconds(2);
        animation.DelayTime = TimeSpan.FromSeconds(delay);
        visual.StartAnimation("Offset", animation);
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

## <a name="add-the-control-to-your-form"></a>Ajouter le contrôle à votre formulaire

Maintenant que vous avez un contrôle personnalisé pour héberger le contenu de la composition, vous pouvez l’ajouter à l’interface utilisateur de l’application. Ici, vous ajoutez une instance du CompositionHostControl que vous avez créé à l’étape précédente. CompositionHostControl est automatiquement ajouté à la boîte à outils Visual Studio sous le  **_nom du projet_ Components**.

1. En mode conception Form1.CS, ajoutez un bouton à l’interface utilisateur.

    - Faites glisser un bouton de la boîte à outils vers Form1. Placez-le dans le coin supérieur gauche du formulaire. (Voir l’image au début du didacticiel pour vérifier le positionnement des contrôles.)
    - Dans le volet Propriétés, affectez à la propriété **Text** de _Button1_ la valeur _Ajouter élément de composition_.
    - Redimensionnez le bouton afin que tout le texte s’affiche.

    (Pour plus d’informations, [consultez Procédure: Ajoutez des contrôles à](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms)Windows Forms.)

1. Ajoutez un CompositionHostControl à l’interface utilisateur.

    - Faites glisser un CompositionHostControl de la boîte à outils vers Form1. Placez-le à droite du bouton.
    - Redimensionnez le CompositionHost afin qu’il remplisse le reste du formulaire.

1. Gérez l’événement de clic sur le bouton.

   - Dans le volet Propriétés, cliquez sur l’éclair pour basculer vers la vue événements.
   - Dans la liste des événements, sélectionnez l’événement **Click** , tapez *Button_Click*, puis appuyez sur entrée.
   - Ce code est ajouté dans Form1.cs:

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Ajoutez du code au gestionnaire de clic de bouton pour créer de nouveaux éléments.

    - Dans Form1.cs, ajoutez du code au gestionnaire d’événements *Button_Click* que vous avez créé précédemment. Ce code appelle _CompositionHostControl1. AddElement_ pour créer un nouvel élément avec une taille et un décalage générés de manière aléatoire. (L’instance de CompositionHostControl a été nommée automatiquement _compositionHostControl1_ lorsque vous l’avez fait glisser sur le formulaire.)

    ```csharp
    // Add
    // using System;

    private void Button_Click(object sender, RoutedEventArgs e)
    {
        Random random = new Random();
        float size = random.Next(50, 150);
        float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
        float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
        compositionHostControl1.AddElement(size, offsetX, offsetY);
    }
    ```

Vous pouvez maintenant générer et exécuter votre application Windows Forms. Lorsque vous cliquez sur le bouton, vous devez voir les carrés animés ajoutés à l’interface utilisateur.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un exemple plus complet qui s’appuie sur la même infrastructure, consultez l' [exemple d’intégration de couche visuelle Windows Forms](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) sur GitHub.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Prise en main avec Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) ASP.net
- [Interopérabilité avec du code non managé](/dotnet/framework/interop/) ASP.net
- [Prise en main des applications Windows 10](/windows/uwp/get-started/) UWP
- [Améliorez votre application bureautique pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) UWP
- [Espace de noms Windows. UI. composition](/uwp/api/windows.ui.composition) UWP

## <a name="complete-code"></a>Code complet

Voici le code complet de ce didacticiel.

### <a name="form1cs"></a>Form1.cs

```csharp
using System;
using System.Windows.Forms;

namespace HelloComposition
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Random random = new Random();
            float size = random.Next(50, 150);
            float offsetX = random.Next(0, (int)(compositionHostControl1.Width - size));
            float offsetY = random.Next(0, (int)(compositionHostControl1.Height/2 - size));
            compositionHostControl1.AddElement(size, offsetX, offsetY);
        }
    }
}
```

### <a name="compositionhostcontrolcs"></a>CompositionHostControl.cs

```csharp
using System;
using System.Numerics;
using Windows.UI.Composition;

namespace HelloComposition
{
    class CompositionHostControl : CompositionHost
    {
        public CompositionHostControl() : base()
        {

        }

        public void AddElement(float size, float offsetX, float offsetY)
        {
            var visual = compositor.CreateSpriteVisual();
            visual.Size = new Vector2(size, size); // Requires references
            visual.Brush = compositor.CreateColorBrush(GetRandomColor());
            visual.Offset = new Vector3(offsetX, offsetY, 0);
            containerVisual.Children.InsertAtTop(visual);

            AnimateSquare(visual, 3);
        }

        private void AnimateSquare(SpriteVisual visual, int delay)
        {
            float offsetX = (float)(visual.Offset.X);
            Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
            float bottom = Height - visual.Size.Y;
            animation.InsertKeyFrame(1f, new Vector3(offsetX, bottom, 0f));
            animation.Duration = TimeSpan.FromSeconds(2);
            animation.DelayTime = TimeSpan.FromSeconds(delay);
            visual.StartAnimation("Offset", animation);
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
using System.Windows.Forms;
using Windows.UI.Composition;

namespace HelloComposition
{
    public partial class CompositionHost : Control
    {
        IntPtr hwndHost;
        object dispatcherQueue;
        protected ContainerVisual containerVisual;
        protected Compositor compositor;
        private ICompositionTarget compositionTarget;

        public Visual Child
        {
            set
            {
                if (compositor == null)
                {
                    InitComposition(hwndHost);
                }
                compositionTarget.Root = value;
            }
        }

        public CompositionHost()
        {
            // Get the window handle.
            hwndHost = Handle;

            // Create dispatcher queue.
            dispatcherQueue = InitializeCoreDispatcher();

            // Build Composition tree of content.
            InitComposition(hwndHost);
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

            compositor = new Compositor();
            object iunknown = compositor as object;
            interop = (ICompositorDesktopInterop)iunknown;
            IntPtr raw;
            interop.CreateDesktopWindowTarget(hwndHost, true, out raw);

            object rawObject = Marshal.GetObjectForIUnknown(raw);
            compositionTarget = (ICompositionTarget)rawObject;

            if (raw == null) { throw new Exception("QI Failed"); }

            containerVisual = compositor.CreateContainerVisual();
            Child = containerVisual;
        }

        protected override void OnPaint(PaintEventArgs pe)
        {
            base.OnPaint(pe);
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