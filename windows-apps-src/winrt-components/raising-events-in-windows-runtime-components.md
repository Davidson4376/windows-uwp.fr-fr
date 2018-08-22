---
author: msatranjr
title: Déclenchement d’événements dans les composants Windows Runtime
ms.assetid: 3F7744E8-8A3C-4203-A1CE-B18584E89000
description: Comment déclencher un événement d’un type défini par l’utilisateur de délégué sur une thread d’arrière-plan afin que JavaScript est autorisé à recevoir l’événement.
ms.author: misatran
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 89c021bb2c094aafc9b534acef9b009817669461
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2801199"
---
# <a name="raising-events-in-windows-runtime-components"></a>Déclenchement d’événements dans les composants Windows Runtime
> [!NOTE]
> Pour savoir comment déclencher des événements dans un [C + / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) composant Windows Runtime, voir [créer des événements dans C + / WinRT](../cpp-and-winrt-apis/author-events.md).

Si votre composant Windows Runtime déclenche un événement d’un type délégué défini par l’utilisateur sur un thread d’arrière-plan (thread de travail) et vous souhaitez que JavaScript puisse recevoir l’événement, vous pouvez l’implémenter ou le déclencher de plusieurs manières:

-   (Option nº 1) Déclencher l’événement via [Windows.UI.Core.CoreDispatcher](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.aspx) pour marshaler l’événement au contexte de thread JavaScript. Bien qu’il s’agisse en général de la meilleure option, elle peut dans certains cas ne pas fournir des performances optimales.
-   (Option nº 2) Utiliser [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; mais perdre les informations sur le type d’événement. Si l’option nº1 n’est pas réalisable ou que ses performances ne sont pas adéquates, cela constitue la deuxième meilleure option si la perte des informations sur le type d’événement est acceptable.
-   (Option nº 3) Créer vos propres proxy/stub pour le composant. Cette option est la plus difficile à implémenter, mais elle conserve les informations sur le type et peut offrir de meilleures performances que l’option nº 1 dans les scénarios exigeants.

Si vous déclenchez un événement sur un thread d’arrière-plan sans utiliser l’une de ces options, un client JavaScript ne recevra pas l’événement.

## <a name="background"></a>Contexte

Tous les composants et applications Windows Runtime sont essentiellement des objets COM, quel que soit le langage utilisé pour les créer. Dans l’API Windows, la plupart des composants sont des objets COM agiles qui peuvent communiquer aussi bien avec les objets sur le thread d’arrière-plan qu’avec les objets sur le thread d’interface utilisateur. Si un objet COM ne peut pas être rendu agile, il requiert des objets d’assistance appelés proxys et stubs pour communiquer avec d’autres objets COM à travers la limite thread d’interface utilisateur-thread d’arrière-plan. (En termes COM, on parle de communication entre cloisonnements de threads.)

La plupart des objets de l’API Windows sont agiles ou ont des proxys et des stubs incorporés. Toutefois, les proxys et les stubs ne peuvent pas être créés pour les types génériques comme Windows.Foundation.[TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx), car ces types ne sont pas complets tant que vous n’avez pas fourni l’argument de type. L’absence de proxys ou de stubs pose problème uniquement avec les clients JavaScript, mais si vous souhaitez que votre composant soit utilisable à partir de JavaScript, mais aussi de C++ ou d’un langage .NET, vous devez utiliser l’une des trois options suivantes.

## <a name="option-1-raise-the-event-through-the-coredispatcher"></a>(Option nº 1) Déclencher l’événement via CoreDispatcher

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

## <a name="option-2-use-eventhandlerltobjectgt-but-lose-type-information"></a>(Option nº 2) Utiliser EventHandler&lt;Object&gt; mais perdre les informations sur le type

Une autre méthode pour envoyer un événement à partir d’un thread d’arrière-plan consiste à utiliser [Windows.Foundation.EventHandler](https://msdn.microsoft.com/library/windows/apps/br206577.aspx)&lt;Object&gt; comme type de l’événement. Windows fournit cette instanciation concrète du type générique, ainsi qu’un proxy et un stub pour celui-ci. L’inconvénient est que les informations sur le type de vos arguments et de votre émetteur d’événement sont perdues. Les clients C++ et .NET doivent connaître via la documentation le type vers lequel effectuer un cast lorsque l’événement est reçu. Les clients JavaScript n’ont pas besoin des informations sur le type d’origine. Ils recherchent les propriétés des arguments, selon leurs noms dans les métadonnées.

Cet exemple montre comment utiliser Windows.Foundation.EventHandler&lt;Object&gt; en C# :

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

Cet événement est utilisé du côté JavaScript comme suit:

```javascript
toastCompletedEventHandler: function (event) {
   var toastType = event.toast.toastType;
   document.getElementById("toasterOutput").innerHTML = "<p>Made " + toastType + " toast</p>";
}
```

## <a name="option-3-create-your-own-proxy-and-stub"></a>(Option nº 3) Créer vos propres proxy/stub

Pour améliorer les performances potentielles sur les types d’événements définis par l’utilisateur qui présentent des informations entièrement préservées sur le type, vous devez créer vos propres objets proxy et stub et les inclure dans le package de votre application. En règle générale, vous devez utiliser cette option uniquement dans les rares cas où aucune des deux autres options n’est adaptée. En outre, il n’y a aucune garantie que cette option offrira de meilleures performances que les deux autres options. Les performances réelles dépendent de nombreux facteurs. Pour mesurer les performances réelles dans votre application et déterminer si l’événement est en fait un goulot d’étranglement, utilisez le profileur Visual Studio ou d’autres outils de profilage.

Le reste de cet article explique comment utiliser C# pour créer un composant Windows Runtime de base, puis comment utiliser C++ pour créer une DLL pour le proxy et le stub qui permettra à JavaScript d’utiliser un événement Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt; déclenché par le composant dans une opération asynchrone. (Vous pouvez également utiliser C++ ou Visual Basic pour créer le composant. Les étapes liées à la création des proxys et des stubs sont identiques.) Cette procédure pas à pas est basée sur l’article Création d’un exemple de composant in-process Windows Runtime (C++/CX) et contribue à expliquer les différents rôles de ce dernier.

Cette procédure pas à pas inclut les parties suivantes:

-   Ici, vous allez créer deux classes Windows Runtime de base. L’une expose un événement de type [Windows.Foundation.TypedEventHandler&lt;TSender, TResult&gt;](https://msdn.microsoft.com/library/windows/apps/br225997.aspx) et l’autre correspond au type retourné à JavaScript comme argument pour TValue. Ces classes ne peuvent pas communiquer avec JavaScript tant que vous n’avez pas effectué les étapes suivantes.
-   Cette application active l’objet de classe principal, appelle une méthode et gère un événement déclenché par le composant Windows Runtime.
-   Ces éléments sont requis par les outils qui génèrent les classes proxy et stub.
-   Vous devez ensuite utiliser le fichier IDL pour générer le code source C pour le proxy et le stub.
-   Inscrivez les objets proxy-stub afin que le runtime COM puisse les trouver, puis référencez la DLL de proxy-stub dans le projet d’application.

## <a name="to-create-the-windows-runtime-component"></a>Pour créer le composant Windows Runtime

Dans Visual Studio, dans la barre de menus, choisissez **Fichier &gt; Nouveau Projet**. Dans la boîte de dialogue **Nouveau projet**, développez **JavaScript &gt; Windows universel**, puis sélectionnez **Application vide**. Nommez le projet ToasterApplication, puis cliquez sur le bouton **OK**.

Ajoutez un composant Windows Runtime C# à la solution : dans l’Explorateur de solutions, ouvrez le menu contextuel de la solution, puis choisissez **Ajouter &gt; Nouveau projet**. Développez **Visual c# &gt; Microsoft Store** , puis sélectionnez le **Composant Windows Runtime**. Nommez le projet ToasterComponent, puis cliquez sur le bouton **OK**. ToasterComponent sera l’espace de noms racine pour les composants que vous créerez lors des étapes ultérieures.

Dans l’Explorateur de solutions, ouvrez le menu contextuel de la solution, puis choisissez **Propriétés**. Dans la boîte de dialogue **Pages de propriétés**, sélectionnez **Propriétés de configuration** dans le volet gauche, puis en haut de la boîte de dialogue, définissez **Configuration** sur **Déboguer** et **Plateforme** sur x86, x64 ou ARM. Cliquez sur le bouton **OK**.

**Important** Plateforme = Toute CPU ne fonctionnera pas, car cette option n’est pas valide pour la DLL Win32 de code natif que vous ajouterez à la solution ultérieurement.

Dans l’Explorateur de solutions, remplacez le nom class1.cs par ToasterComponent.cs afin qu’il corresponde au nom du projet. Visual Studio renomme automatiquement la classe dans le fichier pour qu’elle corresponde au nouveau nom de fichier.

Dans le fichier .cs, ajoutez une directive using pour l’espace de noms Windows.Foundation afin d’inclure TypedEventHandler dans la portée.

Lorsque vous avez besoin de proxys et de stubs, votre composant doit utiliser des interfaces pour exposer ses membres publics. Dans ToasterComponent.cs, définissez une interface pour le générateur de toasts et une autre pour le toast que le générateur produit.

**Remarque** En C#, vous pouvez ignorer cette étape. À la place, créez d’abord une classe, puis ouvrez son menu contextuel et choisissez **Refactoriser &gt; Extraire l’interface**. Dans le code généré, donnez manuellement aux interfaces une accessibilité publique.

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

Ensuite, nous avons besoin de classes qui implémentent ces interfaces, et qui soient publiques et verrouillées afin qu’elles soient accessibles depuis l’application JavaScript que vous programmerez ultérieurement.

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

**Remarque** L’appel asynchrone dans le code précédent utilise ThreadPool.RunAsync uniquement pour illustrer un moyen simple de déclencher l’événement sur un thread d’arrière-plan. Vous pouvez écrire cette méthode particulière comme indiqué dans l’exemple suivant, et elle s’exécutera correctement, car le planificateur de tâches .NET marshale automatiquement les appels asynchrones/d’attente vers le thread d’interface utilisateur.
  
```csharp
    public async void MakeToast(string message)
    {
        Toast toast = new Toast(message)
        await Task.Delay(new Random().Next(1000));
        OnToastCompleted(toast);
    }
```

Si vous générez le projet maintenant, il doit être correctement généré.

## <a name="to-program-the-javascript-app"></a>Programme de l’application de JavaScript

Nous pouvons maintenant ajouter un bouton à l’application JavaScript pour qu’elle utilise la classe que nous avons simplement défini pour rendre toast. Avant de procéder, nous devons ajouter une référence au projet ToasterComponent que nous venons de créer. Dans l’Explorateur de solutions, ouvrez le menu contextuel du projet ToasterApplication, choisissez **Ajouter &gt; références**, puis cliquez sur le bouton **Ajouter une référence** . Dans la boîte de dialogue Ajouter une référence dans le volet gauche sous la Solution, sélectionnez le projet de composant, puis dans le volet central, sélectionnez ToasterComponent. Choisissez le bouton **OK**.

Dans l’Explorateur de solutions, ouvrez le menu contextuel du projet ToasterApplication, puis choisissez **définir comme projet de démarrage**.

À la fin du fichier default.js, ajoutez un espace de noms pour contenir les fonctions pour appeler le composant et être rappelé par celle-ci. L’espace de noms aura deux fonctions, pour faire de toast et l’autre pour gérer l’événement d’achèvement toast. L’implémentation de makeToast crée un objet grille-pain inscrit le Gestionnaire d’événements et rend le toast. À ce stade, le Gestionnaire d’événements n’offre que peu, comme indiqué ici:

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

La fonction makeToast doit être reliée à un bouton. Default.html mise à jour pour inclure un bouton et espace pour sortir le résultat de l’alerte:

```html
    <body>
        <h1>Click the button to make toast</h1>
        <button onclick="ToasterApplication.makeToast()">Make Toast!</button>
        <div id="toasterOutput">
            <p>No Toast Yet...</p>
        </div>
    </body>
```

Si nous pas compliqué à l’aide d’un TypedEventHandler, il doit maintenant être en mesure d’exécuter l’application sur l’ordinateur local, cliquez sur le bouton pour rendre toast. Mais dans notre application, rien ne se produit. Pour plus d’informations, nous allons déboguer le code managé qui se déclenche la ToastCompletedEvent. Arrêtez le projet, puis, dans la barre de menus, choisissez **Debug &gt; propriétés de l’Application grille-pain**. Modifier le **Type de débogueur** gérés **uniquement**. Nouveau dans la barre de menus, choisissez **Debug &gt; Exceptions**, puis sélectionnez **Exceptions Common Language Runtime**.

Maintenant exécuter l’application, cliquez sur le bouton émettre-toast. Le débogueur intercepte une exception cast non valide. Bien qu’il ne soit pas évidente à partir de son message, cette exception se produit parce que les proxys sont manquantes pour cette interface.

![proxy manquant](./images/debuggererrormissingproxy.png)

La première étape de création d’un proxy et un stub d’un composant consiste à ajouter un ID unique ou le GUID pour les interfaces. Cependant, le format GUID à utiliser diffère selon si vous écrivez votre code en c#, Visual Basic ou un autre langage .NET, ou en C++.

## <a name="to-generate-guids-for-the-components-interfaces-c-and-other-net-languages"></a>Pour générer des GUID pour les interfaces du composant (c# et autres langages .NET)

Dans la barre de menus, choisissez outils &gt; Create GUID. Dans la boîte de dialogue, sélectionnez 5. \[GUID («xxxxxxxx-xxxx... xxxx) \]. Cliquez sur le bouton Nouveau GUID, puis choisissez le bouton Copier.

![outil Générateur de GUID](./images/guidgeneratortool.png)

Revenir à la définition d’interface et collez le nouveau GUID juste avant l’interface IToaster, comme illustré dans l’exemple suivant. (N’utilisez pas le GUID dans l’exemple. Chaque interface unique doit avoir son propre GUID.)

```cpp
[Guid("FC198F74-A808-4E2A-9255-264746965B9F")]
        public interface IToaster...
```

Ajouter un à l’aide de la directive pour l’espace de noms System.Runtime.InteropServices.

Répétez ces étapes pour l’interface IToast.

## <a name="to-generate-guids-for-the-components-interfaces-c"></a>Pour générer des GUID pour les interfaces du composant (C++)

Dans la barre de menus, choisissez outils &gt; Create GUID. Dans la boîte de dialogue, sélectionnez 3. struct const statique GUID = {...}. Cliquez sur le bouton Nouveau GUID, puis choisissez le bouton Copier.

Collez le GUID juste avant la définition de l’interface IToaster. Après avoir collé, le GUID doit ressembler à l’exemple suivant. (N’utilisez pas le GUID dans l’exemple. Chaque interface unique doit avoir son propre GUID.)
```cpp
// {F8D30778-9EAF-409C-BCCD-C8B24442B09B}
    static const GUID <<name>> = { 0xf8d30778, 0x9eaf, 0x409c, { 0xbc, 0xcd, 0xc8, 0xb2, 0x44, 0x42, 0xb0, 0x9b } };
```
Ajouter un à l’aide de la directive pour Windows.Foundation.Metadata mettre GuidAttribute à portée.

Maintenant manuellement convertir le GUID const un GuidAttribute afin qu’elle est mise en forme comme indiqué dans l’exemple suivant. Notez que les accolades sont remplacés par des crochets et des parenthèses, et le point-virgule de fin est supprimé.
```cpp
// {E976784C-AADE-4EA4-A4C0-B0C2FD1307C3}
    [GuidAttribute(0xe976784c, 0xaade, 0x4ea4, 0xa4, 0xc0, 0xb0, 0xc2, 0xfd, 0x13, 0x7, 0xc3)]
    public interface IToaster
    {...
```
Répétez ces étapes pour l’interface IToast.

Maintenant que les interfaces ont des identificateurs uniques, nous pouvons créer un fichier IDL en envoyant le fichier .winmd dans l’outil de ligne de commande winmdidl, puis générer le code source C pour le proxy et le stub par ce fichier IDL d’alimentation dans l’outil de ligne de commande MIDL. Visual Studio cela pour nous si nous créons événements post-build, comme indiqué dans les étapes suivantes.

## <a name="to-generate-the-proxy-and-stub-source-code"></a>Pour générer le proxy et stub de code source

Pour ajouter un événement post-build personnalisé, dans l’Explorateur de solutions, ouvrez le menu contextuel du projet ToasterComponent, puis choisissez Propriétés. Dans le volet gauche des pages de propriétés, sélectionnez les événements de génération, puis cliquez sur le bouton Modifier post-build. Ajoutez les commandes suivantes à la ligne de commande post-build. (Le fichier de commandes doit être appelé en premier pour définir les variables d’environnement pour trouver l’outil winmdidl.)

```cpp
call "$(DevEnvDir)..\..\vc\vcvarsall.bat" $(PlatformName)
winmdidl /outdir:output "$(TargetPath)"
midl /metadata_dir "%WindowsSdkDir%References\CommonConfiguration\Neutral" /iid "$(ProjectDir)$(TargetName)_i.c" /env win32 /h "$(ProjectDir)$(TargetName).h" /winmd "Output\$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(ProjectDir)dlldata.c" /proxy "$(ProjectDir)$(TargetName)_p.c" "Output\$(TargetName).idl"
```

**Important**  Pour un ARM ou x64 configuration de projet, remplacez le paramètre /env MIDL x64 ou arm32.

Pour vous assurer que le fichier IDL est régénéré chaque fois que le fichier .winmd est modifié, modifiez **exécuter l’événement post-build** **lorsque la génération met à jour la sortie du projet.**
La page de propriétés événements de Build doit ressembler à ceci: ![événements de build](./images/buildevents.png)

Régénérer la solution pour générer et compiler le fichier IDL.

Vous pouvez vérifier que MIDL compilé correctement la solution en recherchant ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c et dlldata.c dans le répertoire de projet ToasterComponent.

## <a name="to-compile-the-proxy-and-stub-code-into-a-dll"></a>Pour compiler le proxy et le stub du code dans une DLL

Maintenant que vous avez les fichiers requis, vous pouvez compiler afin de générer une DLL, qui est un fichier C++. Pour rendre aussi simple que possible, ajoutez un nouveau projet pour prendre en charge la création du proxy. Ouvrez le menu contextuel pour la solution ToasterApplication, puis **Ajouter > Nouveau projet**. Dans le volet gauche de la boîte de dialogue **Nouveau projet** , développez **Visual C++ &gt; Windows &gt; Universal Windows**, puis dans le volet du milieu, sélectionnez **DLL (UWP applications)**. (Notez qu’il ne s’agit pas d’un projet de composant d’exécution C++ Windows). Nommez le projet proxys, puis choisissez le bouton **OK** . Ces fichiers à jour par les événements après génération lorsqu’une modification dans la classe c#.

Par défaut, le projet de proxys génère les fichiers d’en-tête .h et les fichiers .cpp C++. Étant donné que la DLL est générée à partir des fichiers de produit à partir de MIDL, les fichiers .h et .cpp ne sont pas requis. Dans l’Explorateur de solutions, ouvrez le menu contextuel pour les, cliquez sur **Supprimer**, puis confirmer la suppression.

Maintenant que le projet est vide, vous pouvez ajouter les fichiers générés par MIDL. Ouvrez le menu contextuel pour le projet de proxys, puis **Ajouter > élément existant.** Dans la boîte de dialogue, accédez au répertoire de projet ToasterComponent et sélectionnez ces fichiers: les fichiers ToasterComponent.h, ToasterComponent_i.c, ToasterComponent_p.c et dlldata.c. Choisissez le bouton **Ajouter** .

Dans le projet de proxys, créez un fichier .def pour définir les exportations DLL décrites dans dlldata.c. Ouvrez le menu contextuel du projet, puis choisissez **Ajouter > nouvel élément**. Dans le volet gauche de la boîte de dialogue, sélectionnez le Code et puis, dans le volet du milieu, sélectionnez le fichier de définition de Module. Nommez le fichier proxies.def, puis choisissez le bouton **Ajouter** . Ouvrez ce fichier .def et d’y inclure les exportations qui sont définies dans dlldata.c:

```cpp
EXPORTS
    DllCanUnloadNow         PRIVATE
    DllGetClassObject       PRIVATE
```

Si vous générez le projet maintenant, il échoue. Pour compiler correctement ce projet, vous devez modifier la façon dont le projet est compilé et lié. Dans l’Explorateur de solutions, ouvrez le menu contextuel pour le projet de proxys, puis choisissez **Propriétés**. Modifier les pages de propriétés comme suit.

Dans le volet gauche, sélectionnez **C/C++ > préprocesseur**et puis, dans le volet droit, sélectionnez **Définitions du préprocesseur**, cliquez sur le bouton de la flèche vers le bas, puis sélectionnez **Modifier**. Ajoutez ces définitions dans la zone:

```cpp
WIN32;_WINDOWS
```
Sous **C/C++ > en-têtes précompilé**, modifier **Un en-tête précompilé** **pas à l’aide**des en-têtes précompilé et cliquez sur le bouton **Appliquer** .

Sous **Linker > Général**, modifier la **Bibliothèque d’importation ignorée** à s **pousse**et puis cliquez sur le bouton **Appliquer** .

Sous **Linker > entrée**, sélectionnez **Dépendances supplémentaires**, cliquez sur le bouton de la flèche vers le bas, puis sélectionnez **Modifier**. Ajoutez ce texte dans la zone:

```cpp
rpcrt4.lib;runtimeobject.lib
```

Ne collez pas ces bibliothèques directement dans la ligne de la liste. Utilisez la zone **Modifier** pour vous assurer que MSBuild dans Visual Studio conserve les dépendances supplémentaires corrects.

Lorsque vous avez effectué ces modifications, cliquez sur le bouton **OK** dans la boîte de dialogue **Pages de propriétés** .

Ensuite, prennent une dépendance sur le projet ToasterComponent. Cela garantit que le grille-pain va générer avant le projet proxy généré. Cela est nécessaire car le projet grille-pain est chargé de générer les fichiers pour créer le proxy.

Ouvrez le menu contextuel pour le projet de proxys, puis choisissez dépendances du projet. Activez les cases à cocher pour indiquer que le projet proxys dépend du projet ToasterComponent, pour vous assurer que Visual Studio génère les dans l’ordre correct.

Vérifiez que la solution est générée correctement en choisissant **Créer > Régénérer la Solution** sur la barre de menus Visual Studio.


## <a name="to-register-the-proxy-and-stub"></a>Pour inscrire le proxy et le stub

Dans le projet ToasterApplication, ouvrez le menu contextuel pour package.appxmanifest, puis choisissez **Ouvrir avec**. Dans la boîte de dialogue Ouvrir avec, sélectionnez **Éditeur de texte XML** , puis choisissez le bouton **OK** . Nous allons coller du texte dans du code XML qui fournit qu'une inscription de l’extension windows.activatableClass.proxyStub et qui sont basé sur les GUID dans le proxy. Pour trouver les GUID à utiliser dans le fichier .appxmanifest, ouvrez ToasterComponent_i.c. Recherchez les entrées qui ressemblent à celles de l’exemple suivant. Notez également les définitions de IToast, IToaster et une troisième interface — un gestionnaire d’événements typé qui a deux paramètres: un grille-pain et un Toast. Cela correspond à l’événement qui est défini dans la classe grille-pain. Notez que les GUID de IToast et IToaster correspondent les GUID qui sont définies sur les interfaces dans le fichier c#. Étant donné que l’interface de gestionnaire d’événement typé est généré automatiquement, le GUID de cette interface est également généré automatiquement.

```cpp
MIDL_DEFINE_GUID(IID, IID___FITypedEventHandler_2_ToasterComponent__CToaster_ToasterComponent__CToast,0x1ecafeff,0x1ee1,0x504a,0x9a,0xf5,0xa6,0x8c,0x6f,0xb2,0xb4,0x7d);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToast,0xF8D30778,0x9EAF,0x409C,0xBC,0xCD,0xC8,0xB2,0x44,0x42,0xB0,0x9B);

MIDL_DEFINE_GUID(IID, IID___x_ToasterComponent_CIToaster,0xE976784C,0xAADE,0x4EA4,0xA4,0xC0,0xB0,0xC2,0xFD,0x13,0x07,0xC3);
```

Maintenant, nous copier les GUID, collez-les dans package.appxmanifest dans un nœud, nous ajoutons et Extensions de nom et les reformater. L’entrée de manifeste ressemble à l’exemple suivant, mais là encore, n’oubliez pas d’utiliser votre propre GUID. Notez que le GUID ClassId dans le code XML est identique à ITypedEventHandler2. Il s’agit, car ce GUID est celle qui est répertorié dans ToasterComponent_i.c. Les GUID ici respectent la casse. Au lieu de reformater manuellement les GUID IToast et IToaster, vous pouvez revenir en arrière dans les définitions d’interface et obtenir la valeur GuidAttribute, qui a le format correct. En C++, il existe un GUID correctement mis en forme dans le commentaire. Dans tous les cas, vous devez manuellement reformater le GUID qui est utilisé pour le ClassId et le Gestionnaire d’événements.

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

Coller le nœud XML pour les Extensions comme un enfant direct du nœud Package et un homologue de, par exemple, le nœud de ressources.

Avant de poursuivre, il est important de vous assurer que:

-   ProxyStub ClassId est définie sur le premier GUID dans le fichier ToasterComponent\_i.c. Utilisez le premier GUID qui est défini dans ce fichier pour l’identificateur de classe. (Cela peut être la même que le GUID de ITypedEventHandler2.)
-   Le chemin d’accès est le chemin d’accès relatif de package du proxy binaire. (Dans cette procédure pas à pas, proxies.dll est dans le même dossier que ToasterApplication.winmd.)
-   Les GUID sont au format approprié. (C’est facile à obtenir incorrect).
-   L’ID d’interface dans le manifeste correspondent à l’IID dans le fichier ToasterComponent\_i.c.
-   Les noms d’interface sont uniques dans le manifeste. Étant donné que ceux-ci ne sont pas utilisés par le système, vous pouvez choisir les valeurs. Il est préférable de choisir des noms d’interface qui clairement correspondent aux interfaces que vous avez définis. Pour les interfaces générés, les noms doivent être indicatifs des interfaces générés. Vous pouvez utiliser le fichier ToasterComponent\_i.c pour vous aider à générer des noms de l’interface.

Si vous essayez d’exécuter la solution maintenant, vous obtiendrez une erreur qui proxies.dll ne fait pas partie de la charge utile. Ouvrez le menu contextuel pour le dossier **références** dans le projet ToasterApplication, puis choisissez **Ajouter une référence**. Activez la case à cocher en regard du projet de proxys. En outre, assurez-vous que la case à cocher en regard de ToasterComponent est également sélectionnée. Choisissez le bouton **OK**.

Le projet doit maintenant créer. Exécutez le projet et vérifiez que vous pouvez apporter toast.

## <a name="related-topics"></a>Rubriques connexes

* [Création de composants Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md)
