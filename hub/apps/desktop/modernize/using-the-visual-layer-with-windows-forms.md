---
title: À l’aide de la couche visuelle avec Windows Forms
description: Découvrez des techniques pour l’utilisation de l’API de couche Visual en combinaison avec le contenu existant de Windows Forms pour créer des animations avancées et les effets.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0a3aff0bee68b971accd96f895601343726024d0
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985029"
---
# <a name="using-the-visual-layer-with-windows-forms"></a>À l’aide de la couche visuelle avec Windows Forms

Vous pouvez utiliser les API de Composition Windows Runtime (également appelé le [couche visuelle](/windows/uwp/composition/visual-layer)) dans vos applications Windows Forms pour créer des expériences modernes autrement clair pour les utilisateurs de Windows 10.

Le code complet pour ce didacticiel est disponible sur GitHub : [Exemple de Windows Forms HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition).

## <a name="prerequisites"></a>Prérequis

La plateforme Windows universelle API d’hébergement a ces conditions préalables.

- Nous partons du principe que vous disposez des connaissances sur le développement d’applications à l’aide de Windows Forms et UWP. Pour plus d’informations, voir :
  - [Bien démarrer avec Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms)
  - [Prise en main les applications Windows 10](/windows/uwp/get-started/)
  - [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- .NET framework 4.7.2 ou version ultérieure
- Windows 10 version 1803 ou ultérieure
- SDK Windows 10 17134 ou version ultérieure

## <a name="how-to-use-composition-apis-in-windows-forms"></a>Comment utiliser les API de Composition dans les Windows Forms

Dans ce didacticiel, vous créez une interface utilisateur de simple Windows Forms et lui ajoutez des éléments animés de Composition. Les Windows Forms et la Composition des composants sont conservés simples, mais le code interop indiqué est le même quelle que soit la complexité des composants. L’application terminée ressemble à ceci.

![L’application en cours d’exécution de l’interface utilisateur](images/visual-layer-interop/wf-comp-interop-app-ui.png)

## <a name="create-a-windows-forms-project"></a>Créez un projet Windows Forms

La première étape consiste à créer le projet d’application Windows Forms, qui inclut une définition d’application et le formulaire principal pour l’interface utilisateur.

Pour créer un nouveau projet d’Application de Windows Forms dans Visual C# nommé _HelloComposition_:

1. Ouvrez Visual Studio et sélectionnez **fichier** > **New** > **projet**.<br/>Le **nouveau projet** boîte de dialogue s’ouvre.
1. Sous le **installé** catégorie, développez le **Visual C#**  nœud, puis sélectionnez **Windows Desktop**.
1. Sélectionnez le **Windows Forms App (.NET Framework)** modèle.
1. Entrez le nom _HelloComposition,_ sélectionnez Framework **.NET Framework 4.7.2**, puis cliquez sur **OK**.

Visual Studio crée le projet et ouvre le concepteur pour la fenêtre d’application par défaut appelé Form1.cs.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurer le projet pour utiliser Windows Runtime APIs

Pour utiliser Windows Runtime (WinRT) API dans votre application Windows Forms, vous devez configurer votre projet Visual Studio pour accéder à l’exécution de Windows. En outre, les vecteurs sont largement utilisées par les API de Composition, vous devez donc ajouter les références nécessaires pour utiliser des vecteurs.

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

## <a name="create-a-custom-control-to-manage-interop"></a>Créer un contrôle personnalisé pour gérer l’interopérabilité

Pour héberger le contenu vous créez avec la couche visuelle, vous créez un contrôle personnalisé dérivé de [contrôle](/dotnet/api/system.windows.forms.control). Ce contrôle vous permet d’accéder à une fenêtre [gérer](/dotnet/api/system.windows.forms.control.handle), dont vous avez besoin pour créer le conteneur pour votre contenu de la couche visuelle.

Il s’agit dans lequel vous effectuez la majeure partie de la configuration pour l’hébergement des API de Composition. Dans ce contrôle, vous utilisez [Platform Invocation Services (PInvoke)](/cpp/dotnet/calling-native-functions-from-managed-code) et [COM Interop](/dotnet/api/system.runtime.interopservices.comimportattribute) pour amener les API de Composition dans votre application Windows Forms. Pour plus d’informations sur PInvoke et COM Interop, consultez [interopération avec du code non managé](/dotnet/framework/interop/index).

> [!TIP]
> Si vous le souhaitez, vérifiez le code complet à la fin du didacticiel pour vous assurer que tout le code est placées au bon endroit lorsque vous travaillez dans le didacticiel.

1. Ajoutez un nouveau fichier de contrôle personnalisé à votre projet qui dérive de [contrôle](/dotnet/api/system.windows.forms.control).
    - Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le _HelloComposition_ projet.
    - Dans le menu contextuel, sélectionnez **ajouter** > **un nouvel élément...** .
    - Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **contrôle personnalisé**.
    - Nommez le contrôle _CompositionHost.cs_, puis cliquez sur **ajouter**. CompositionHost.cs s’ouvre en mode Design.

1. Basculez en mode code pour CompositionHost.cs et ajoutez le code suivant à la classe.

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

    Dans le constructeur, vous appelez le _InitializeCoreDispatcher_ et _InitComposition_ méthodes. Vous créez ces méthodes dans les étapes suivantes.

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

1. Initialize a thread with a [CoreDispatcher](/uwp/api/windows.ui.core.coredispatcher). The core dispatcher is responsible for processing window messages and dispatching events for WinRT APIs. New instances of **Compositor** must be created on a thread that has a CoreDispatcher.
    - Create a method named _InitializeCoreDispatcher_ and add code to set up the dispatcher queue.

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

    - La file d’attente du répartiteur requiert une déclaration de PInvoke. Placez cette déclaration à la fin du code pour la classe. (Nous placer ce code dans une région de conserver le code de classe clarté).

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

    Vous avez la file d’attente du répartiteur prêt et pouvez commencer à initialiser et créer la Composition de contenu.

1. Initialiser le [compositeur](/uwp/api/windows.ui.composition.compositor). Le constructeur est une fabrique qui crée une variété de types dans le [Windows.UI.Composition](/uwp/api/windows.ui.composition) espace de noms s’étendant sur la couche visuelle, le système effets et le système d’animation. La classe compositeur gère également la durée de vie des objets créés à partir de la fabrique.

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

## <a name="create-a-custom-control-to-host-composition-elements"></a>Créer un contrôle personnalisé à des éléments de composition d’hôte

Il est judicieux de placer le code qui génère et gère vos éléments de composition dans un contrôle séparé qui dérive de CompositionHost. Cela permet de conserver le code interop que vous avez créé dans la classe CompositionHost réutilisables.

Ici, vous créez un contrôle personnalisé dérivé de CompositionHost. Ce contrôle est ajouté à la boîte à outils Visual Studio, vous pouvez l’ajouter à votre formulaire.

1. Ajoutez un nouveau fichier de contrôle personnalisé à votre projet qui dérive de CompositionHost.
    - Dans **l’Explorateur de solutions**, avec le bouton droit cliquez sur le _HelloComposition_ projet.
    - Dans le menu contextuel, sélectionnez **ajouter** > **un nouvel élément...** .
    - Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **contrôle personnalisé**.
    - Nommez le contrôle _CompositionHostControl.cs_, puis cliquez sur **ajouter**. CompositionHostControl.cs s’ouvre en mode Design.

1. Dans le volet Propriétés pour le mode de création de CompositionHostControl.cs, définissez le **BackColor** propriété **ControlLight**.

    La définition de la couleur d’arrière-plan est facultative. Nous le faire ici afin que vous puissiez voir votre contrôle personnalisé par rapport à l’arrière-plan du formulaire.

1. Basculez en mode code pour CompositionHostControl.cs et mettre à jour de la déclaration de classe à dériver de CompositionHost.

    ```csharp
    class CompositionHostControl : CompositionHost
    ```

1. Mettre à jour le constructeur pour appeler le constructeur de base.

    ```csharp
    public CompositionHostControl() : base()
    {

    }
    ```

### <a name="add-composition-elements"></a>Ajouter des éléments de composition

Avec l’infrastructure en place, vous pouvez maintenant ajouter le contenu de Composition à l’interface utilisateur de l’application.

Pour cet exemple, vous ajoutez le code à la classe CompositionHostControl qui crée et anime un simple [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Ajouter un élément de composition.

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

Maintenant que vous avez un contrôle personnalisé pour héberger du contenu de la Composition, vous pouvez l’ajouter à l’interface utilisateur de l’application. Ici, vous ajoutez une instance de la CompositionHostControl que vous avez créé à l’étape précédente. CompositionHostControl est automatiquement ajouté à la boîte à outils de Visual Studio sous  **_nom_projet_ composants**.

1. En mode de conception de Form1.CS, ajoutez un bouton à l’interface utilisateur.

    - Faites glisser un bouton à partir de la boîte à outils vers Form1. Placez-le dans le coin supérieur gauche du formulaire. (Voir l’image au début du didacticiel afin de vérifier le placement des contrôles.)
    - Dans le volet Propriétés, modifiez le **texte** propriété à partir de _button1_ à _élément de composition ajouter_.
    - Redimensionner le bouton pour que tout le texte s’affiche.

    (Pour plus d’informations, consultez [Comment : Ajouter des contrôles aux Windows Forms](/dotnet/framework/winforms/controls/how-to-add-controls-to-windows-forms).)

1. Ajouter un CompositionHostControl à l’interface utilisateur.

    - Faites glisser un CompositionHostControl à partir de la boîte à outils vers Form1. Placez-le à droite du bouton.
    - Redimensionner le CompositionHost afin qu’il remplisse le reste du formulaire.

1. Handle du bouton Cliquez sur l’événement.

   - Dans le volet Propriétés, cliquez sur l’ampoule pour basculer vers l’affichage des événements.
   - Dans la liste des événements, sélectionnez le **cliquez sur** événement, type *Button_Click*, puis appuyez sur ENTRÉE.
   - Ce code est ajouté dans Form1.cs :

    ```csharp
    private void Button_Click(object sender, EventArgs e)
    {

    }
    ```

1. Ajouter du code au bouton Cliquez sur Gestionnaire pour créer de nouveaux éléments.

    - Dans Form1.cs, ajoutez le code à la *Button_Click* Gestionnaire d’événements créé précédemment. Ce code appelle _CompositionHostControl1.AddElement_ pour créer un nouvel élément avec une taille généré de manière aléatoire et le décalage. (L’instance de CompositionHostControl a été nommé automatiquement _compositionHostControl1_ lorsque vous avez glisser sur le formulaire.)

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

Vous pouvez maintenant générer et exécuter votre application Windows Forms. Lorsque vous cliquez sur le bouton, vous devez voir des carrés animés ajoutés à l’interface utilisateur.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un exemple plus complet qui s’appuie sur la même infrastructure, consultez le [Windows Forms Visual exemple d’intégration de couche](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration) sur GitHub.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Bien démarrer avec Windows Forms](/dotnet/framework/winforms/getting-started-with-windows-forms) (.NET)
- [Interopérabilité avec du code non managé](/dotnet/framework/interop/) (.NET)
- [Prise en main les applications Windows 10](/windows/uwp/get-started/) (UWP)
- [Améliorer votre application de bureau pour Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espace de noms Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Code complet

Voici le code complet pour ce didacticiel.

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