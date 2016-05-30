---
author: martinekuan
title: Déclenchement d’événements dans les composants Windows Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: 
---

# Déclenchement d’événements dans les composants Windows Runtime


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]

Si votre composant Windows Runtime déclenche un événement d’un type délégué défini par l’utilisateur sur un thread d’arrière-plan (thread de travail) et vous souhaitez que JavaScript puisse recevoir l’événement, vous pouvez l’implémenter ou le déclencher de plusieurs manières :

-   (Option nº 1) Déclencher l’événement via [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) pour marshaler l’événement au contexte de thread JavaScript. Bien qu’il s’agisse en général de la meilleure option, elle peut dans certains cas ne pas fournir des performances optimales.
-   (Option nº 2) Utiliser [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; mais perdre les informations sur le type d’événement. Si l’option nº 1 n’est pas réalisable ou que ses performances ne sont pas adéquates, cela constitue la deuxième meilleure option si la perte des informations sur le type d’événement est acceptable.
-   (Option nº 3) Créer vos propres proxy/stub pour le composant. Cette option est la plus difficile à implémenter, mais elle conserve les informations sur le type et peut offrir de meilleures performances que l’option nº 1 dans les scénarios exigeants.

Si vous déclenchez un événement sur un thread d’arrière-plan sans utiliser l’une de ces options, un client JavaScript ne recevra pas l’événement.

## Contexte


Tous les composants et applications Windows Runtime sont essentiellement des objets COM, quel que soit le langage utilisé pour les créer. Dans l’API Windows, la plupart des composants sont des objets COM agiles qui peuvent communiquer aussi bien avec les objets sur le thread d’arrière-plan qu’avec les objets sur le thread d’interface utilisateur. Si un objet COM ne peut pas être rendu agile, il requiert des objets d’assistance appelés proxys et stubs pour communiquer avec d’autres objets COM à travers la limite thread d’interface utilisateur-thread d’arrière-plan. (En termes COM, on parle de communication entre cloisonnements de threads.)

La plupart des objets de l’API Windows sont agiles ou ont des proxys et des stubs incorporés. Toutefois, les proxys et les stubs ne peuvent pas être créés pour les types génériques comme Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx), car ces types ne sont pas complets tant que vous n’avez pas fourni l’argument de type. L’absence de proxys ou de stubs pose problème uniquement avec les clients JavaScript, mais si vous souhaitez que votre composant soit utilisable à partir de JavaScript, mais aussi de C++ ou d’un langage .NET, vous devez utiliser l’une des trois options suivantes.

## (Option nº 1) Déclencher l’événement via CoreDispatcher


Vous pouvez envoyer des événements de n’importe quel type délégué défini par l’utilisateur à l’aide de [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx), et JavaScript sera en mesure de les recevoir. Si vous n’êtes pas certain de l’option à utiliser, essayez celle-ci en premier. Si la latence entre le déclenchement des événements et la gestion des événements devient un problème, essayez l’une des autres options.

L’exemple suivant montre comment utiliser CoreDispatcher pour déclencher un événement fortement typé. Notez que l’argument de type est Toast, et non Object.

```csharp
public event EventHandler<Toast> ToastCompletedEvent;
private void OnToastCompleted(Toast args)
{
    var completedEvent = ToastCompletedEvent;
    if (completedEvent != null)
    {
        completedEvent(this, args);
    }
}

public void MakeToastWithDispatcher(string message)
{
    Toast toast = new Toast(message);
    // Assume you have a CoreDispatcher at class scope.
    // Initialize it here, then use it from the background thread.
    var window = Windows.UI.Core.CoreWindow.GetForCurrentThread();
    m_dispatcher = window.Dispatcher;

    Task.Run( () =>
    {
        if (ToastCompletedEvent != null)
        {
            m_dispatcher.RunAsync(CoreDispatcherPriority.Normal,
            new DispatchedHandler(() =>
            {
                this.OnToastCompleted(toast);
            })); // end m_dispatcher.RunAsync
         }
     }); // end Task.Run
}
```

## (Option nº 2) Utiliser EventHandler&lt;Object&gt; mais perdre les informations sur le type


Une autre méthode pour envoyer un événement à partir d’un thread d’arrière-plan consiste à utiliser [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; comme type de l’événement. Windows fournit cette instanciation concrète du type générique, ainsi qu’un proxy et un stub pour celui-ci. L’inconvénient est que les informations sur le type de vos arguments et de votre émetteur d’événement sont perdues. Les clients C++ et .NET doivent connaître via la documentation le type vers lequel effectuer un cast lorsque l’événement est reçu. Les clients JavaScript n’ont pas besoin des informations sur le type d’origine. Ils recherchent les propriétés des arguments, selon leurs noms dans les métadonnées.

Cet exemple montre comment utiliser Windows.Foundation.EventHandler&lt;Object&gt; en C# :

```csharp
public sealed Class1
{
// Declare the event
public event EventHandler<Object> ToastCompletedEvent;

    // Raise the event
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message);
        // Fire the event from a background thread to allow this thread to continue 
        Task.Run(() =>
        {
            if (ToastCompletedEvent != null)
            {
                OnToastCompleted(toast);
            }
        });
    }

    private void OnToastCompleted(Toast args)
    {
        var completedEvent = ToastCompletedEvent;
        if (completedEvent != null)
        {
            completedEvent(this, args);
        }
    }
}
```

Cet événement est utilisé du côté JavaScript comme suit :

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## (Option nº 3) Créer vos propres proxy/stub


Pour améliorer les performances potentielles sur les types d’événements définis par l’utilisateur qui présentent des informations entièrement préservées sur le type, vous devez créer vos propres objets proxy et stub et les inclure dans le package de votre application. En règle générale, vous devez utiliser cette option uniquement dans les rares cas où aucune des deux autres options n’est adaptée. En outre, il n’y a aucune garantie que cette option offrira de meilleures performances que les deux autres options. Les performances réelles dépendent de nombreux facteurs. Pour mesurer les performances réelles dans votre application et déterminer si l’événement est en fait un goulot d’étranglement, utilisez le profileur Visual Studio ou d’autres outils de profilage.

Le reste de cet article explique comment utiliser C# pour créer un composant Windows Runtime de base, puis comment utiliser C++ pour créer une DLL pour le proxy et le stub qui permettra à JavaScript d’utiliser un événement Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; déclenché par le composant dans une opération asynchrone. (Vous pouvez également utiliser C++ ou Visual Basic pour créer le composant. Les étapes liées à la création des proxys et des stubs sont identiques.) Cette procédure pas à pas est basée sur l’article Création d’un exemple de composant in-process Windows Runtime (C++/CX) et contribue à expliquer les différents rôles de ce dernier.

Cette procédure pas à pas inclut les parties suivantes :

-   Ici, vous allez créer deux classes Windows Runtime de base. L’une expose un événement de type [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) et l’autre correspond au type retourné à JavaScript comme argument pour TValue. Ces classes ne peuvent pas communiquer avec JavaScript tant que vous n’avez pas effectué les étapes suivantes.
-   Cette application active l’objet de classe principal, appelle une méthode et gère un événement déclenché par le composant Windows Runtime.
-   Ces éléments sont requis par les outils qui génèrent les classes proxy et stub.
-   Vous devez ensuite utiliser le fichier IDL pour générer le code source C pour le proxy et le stub.
-   Inscrivez les objets proxy-stub afin que le runtime COM puisse les trouver, puis référencez la DLL de proxy-stub dans le projet d’application.

## Pour créer le composant Windows Runtime

1.  Dans la barre de menus de Visual Studio, choisissez **Fichier &gt; Nouveau projet**. Dans la boîte de dialogue **Nouveau projet**, développez **JavaScript &gt; Windows universel**, puis sélectionnez **Application vide**. Nommez le projet ToasterApplication, puis cliquez sur le bouton **OK**.
2.  Ajoutez un composant Windows Runtime C# à la solution : dans l’Explorateur de solutions, ouvrez le menu contextuel de la solution, puis choisissez **Ajouter &gt; Nouveau projet**. Développez **Visual C# &gt; Windows Store**, puis sélectionnez **Composant Windows Runtime**. Nommez le projet ToasterComponent, puis cliquez sur le bouton **OK**. ToasterComponent sera l’espace de noms racine pour les composants que vous créerez lors des étapes ultérieures.

    Dans l’Explorateur de solutions, ouvrez le menu contextuel de la solution, puis choisissez **Propriétés**. Dans la boîte de dialogue **Pages de propriétés**, sélectionnez **Propriétés de configuration** dans le volet gauche, puis en haut de la boîte de dialogue, définissez **Configuration** sur **Déboguer** et **Plateforme** sur x86, x64 ou ARM. Cliquez sur le bouton **OK**.

    > **Important** Plateforme = Toute CPU ne fonctionnera pas, car cette option n’est pas valide pour la DLL Win32 de code natif que vous ajouterez à la solution ultérieurement.

3.  Dans l’Explorateur de solutions, remplacez le nom class1.cs par ToasterComponent.cs afin qu’il corresponde au nom du projet. Visual Studio renomme automatiquement la classe dans le fichier pour qu’elle corresponde au nouveau nom de fichier.
4.  Dans le fichier .cs, ajoutez une directive using pour l’espace de noms Windows.Foundation afin d’inclure TypedEventHandler dans la portée.
5.  Lorsque vous avez besoin de proxys et de stubs, votre composant doit utiliser des interfaces pour exposer ses membres publics. Dans ToasterComponent.cs, définissez une interface pour le générateur de toasts et une autre pour le toast que le générateur produit.

    > **Remarque** En C#, vous pouvez ignorer cette étape. À la place, créez d’abord une classe, puis ouvrez son menu contextuel et choisissez **Refactoriser &gt; Extraire l’interface**. Dans le code généré, donnez manuellement aux interfaces une accessibilité publique.

    ```csharp
    public interface IToaster
        {
            void MakeToast(String message);
            event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

        }
        public interface IToast
        {
            String ToastType { get; }
        }
    ```

    L’interface IToast comporte qui peut être récupérée pour décrire le type de toast. L’interface IToaster dispose d’une méthode pour créer un toast et d’un événement pour indiquer que le toast est créé. Comme cet événement retourne le type particulier du toast, il est appelé événement typé.

6.  Ensuite, nous avons besoin de classes qui implémentent ces interfaces, et qui soient publiques et verrouillées afin qu’elles soient accessibles depuis l’application JavaScript que vous programmerez ultérieurement.

    ```csharp
    public sealed class Toast : IToast
        {
            private string _toastType;

            public string ToastType
            {
                get
                {
                    return _toastType;
                }
            }
            internal Toast(String toastType)
            {
                _toastType = toastType;
            }

        }
        public sealed class Toaster : IToaster
        {
            public event TypedEventHandler<Toaster, Toast> ToastCompletedEvent;

            private void OnToastCompleted(Toast args)
            {
                var completedEvent = ToastCompletedEvent;
                if (completedEvent != null)
                {
                    completedEvent(this, args);
                }
            }

            public void MakeToast(string message)
            {
                Toast toast = new Toast(message);
                // Fire the event from a thread-pool thread to enable this thread to continue 
                Windows.System.Threading.ThreadPool.RunAsync(
                (IAsyncAction action) =>
                {
                    if (ToastCompletedEvent != null)
                    {
                        OnToastCompleted(toast);
                    }
                });
           }
        }
    ``` 

    Dans le code précédent, nous créons le toast, puis accélérons un élément de travail de pool de threads pour déclencher la notification. Bien que l’IDE puisse suggérer que vous appliquez le mot clé await à l’appel asynchrone, cela n’est pas nécessaire dans ce cas, parce que la méthode n’effectue aucun travail dépendant des résultats de l’opération.

    > **Remarque** L’appel asynchrone dans le code précédent utilise ThreadPool.RunAsync uniquement pour illustrer un moyen simple de déclencher l’événement sur un thread d’arrière-plan. Vous pouvez écrire cette méthode particulière comme indiqué dans l’exemple suivant, et elle s’exécutera correctement, car le planificateur de tâches .NET marshale automatiquement les appels asynchrones/d’attente vers le thread d’interface utilisateur.
  
    ````csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
    ```

    If you build the project now, it should build cleanly.

## To program the JavaScript app

1.  Now we can add a button to the JavaScript app to cause it to use the class we just defined to make toast. Before we do this, we must add a reference to the ToasterComponent project we just created. In Solution Explorer, open the shortcut menu for the ToasterApplication project, choose **Add &gt; References**, and then choose the **Add New Reference** button. In the Add Reference dialog box, in the left pane under Solution, select the component project, and then in the middle pane, select ToasterComponent. Choose the **OK** button.
2.  In Solution Explorer, open the shortcut menu for the ToasterApplication project and then choose **Set as Startup Project**.
3.  At the end of the default.js file, add a namespace to contain the functions to call the component and be called back by it. The namespace will have two functions, one to make toast and one to handle the toast-complete event. The implementation of makeToast creates a Toaster object, registers the event handler, and makes the toast. So far, the event handler doesn’t do much, as shown here:

    ```javascript
    WinJS.Namespace.define("ToasterApplication"), {
       makeToast: function () {

          var toaster = new ToasterComponent.Toaster();
          //toaster.addEventListener("ontoastcompletedevent", ToasterApplication.toastCompletedEventHandler);
          toaster.ontoastcompletedevent = ToasterApplication.toastCompletedEventHandler;
          toaster.makeToast("Peanut Butter");
       },

       toastCompletedEventHandler: function(event) {
           // The sender of the event (the delegate's first type parameter)
           // is mapped to event.target. The second argument of the delegate
           // is contained in event, which means in this case event is a 
           // Toast class, with a toastType string.
           var toastType = event.toastType;

           document.getElementById('toastOutput').innerHTML = "<p>Made " + toastType + " toast</p>";
        },
    });
    ```

4.  The makeToast function must be hooked up to a button. Update default.html to include a button and some space to output the result of making toast:

    ```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
    ```

    If we weren’t using a TypedEventHandler, we would now be able to run the app on the local machine and click the button to make toast. But in our app, nothing happens. To find out why, let’s debug the managed code that fires the ToastCompletedEvent. Stop the project, and then on the menu bar, choose **Debug &gt; Toaster Application properties**. Change **Debugger Type** to **Managed Only**. Again on the menu bar, choose **Debug &gt; Exceptions**, and then select **Common Language Runtime Exceptions**.

    Now run the app and click the make-toast button. The debugger catches an invalid cast exception. Although it’s not obvious from its message, this exception is occurring because proxies are missing for that interface.

    ![missing proxy](./images/debuggererrormissingproxy.png)

    The first step in creating a proxy and stub for a component is to add a unique ID or GUID to the interfaces. However, the GUID format to use differs depending on whether you're coding in C#, Visual Basic, or another .NET language, or in C++.

## To generate GUIDs for the component's interfaces (C# and other .NET languages)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 5. \[Guid(“xxxxxxxx-xxxx...xxxx)\]. Choose the New GUID button and then choose the Copy button.

    ![guid generator tool](./images/guidgeneratortool.png)

2.  Go back to the interface definition, and then paste the new GUID just before the IToaster interface, as shown in the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp
    [Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
    ```

3.  Add a using directive for the System.Runtime.InteropServices namespace.
4.  Repeat these steps for the IToast interface.

## To generate GUIDs for the component's interfaces (C++)

1.  On the menu bar, choose Tools &gt; Create GUID. In the dialog box, select 3. static const struct GUID = {...}. Choose the New GUID button and then choose the Copy button.

2.  Paste the GUID just before the IToaster interface definition. After you paste, the GUID should resemble the following example. (Don't use the GUID in the example. Every unique interface should have its own GUID.)

    ```cpp 
      <Extensions> <!--Use your own GUIDs!!!-->
        <Extension Category="windows.activatableClass.proxyStub">
          <ProxyStub ClassId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d">
            <Path>Proxies.dll</Path>
            <Interface Name="IToast" InterfaceId="F8D30778-9EAF-409C-BCCD-C8B24442B09B"/>
            <Interface Name="IToaster"  InterfaceId="E976784C-AADE-4EA4-A4C0-B0C2FD1307C3"/>  
            <Interface Name="ITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast" InterfaceId="1ecafeff-1ee1-504a-9af5-a68c6fb2b47d"/>
          </ProxyStub>      
        </Extension>
      </Extensions>
    ```

    Paste the Extensions XML node as a direct child of the Package node, and a peer of, for example, the Resources node.

2.  Before moving on, it’s important to ensure that:

    -   The ProxyStub ClassId is set to the first GUID in the ToasterComponent\_i.c file. Use the first GUID that's defined in this file for the classId. (This might be the same as the GUID for ITypedEventHandler2.)
    -   The Path is the package relative path of the proxy binary. (In this walkthrough, proxies.dll is in the same folder as ToasterApplication.winmd.)
    -   The GUIDs are in the correct format. (This is easy to get wrong.)
    -   The interface IDs in the manifest match the IIDs in ToasterComponent\_i.c file.
    -   The interface names are unique in the manifest. Because these are not used by the system, you can choose the values. It is a good practice to choose interface names that clearly match interfaces that you have defined. For generated interfaces, the names should be indicative of the generated interfaces. You can use the ToasterComponent\_i.c file to help you generate interface names.

3.  If you try to run the solution now, you will get an error that proxies.dll is not part of the payload. Open the shortcut menu for the **References** folder in the ToasterApplication project and then choose **Add Reference**. Select the check box next to the Proxies project. Also, make sure that the check box next to ToasterComponent is also selected. Choose the **OK** button.

    The project should now build. Run the project and verify that you can make toast.

## Related topics

* [Creating Windows Runtime Components in C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=May16_HO2-->


