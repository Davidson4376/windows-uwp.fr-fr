---
author: andrewleader
Description: Learn how Win32 C# apps can send local toast notifications and handle the user clicking the toast.
title: Envoyer une notification toast locale depuis des applications de bureau en C#
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.author: mijacobs
ms.date: 01/23/2018
ms.topic: article
keywords: windows10, uwp, win32, bureau, notifications toast, envoyer un toast, envoyer un toast local, pont du bureau, C#, C sharp
ms.localizationpriority: medium
ms.openlocfilehash: 9e828787a1a342b78cd72d1e7afc3fe9df0f0eea
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5870923"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>Envoyer une notification toast locale depuis des applications de bureau en C#

Les applications de bureau (Pont du bureau et Win32 classique) peuvent envoyer des notifications toast interactives, tout comme les applications de plateforme Windows universelle (UWP). Toutefois, il existe quelques étapes spéciales pour les applications de bureau en raison des différents schémas d’activation et de l’absence potentielle d’identité de package si vous n’utilisez pas le Pont du bureau.

> [!IMPORTANT]
> Si vous écrivez une application UWP, consultez la [documentation UWP](send-local-toast.md). Pour les autres langues de bureau, consultez [Applications WRL de bureau en C++](send-local-toast-desktop-cpp-wrl.md).


## <a name="step-1-enable-the-windows-10-sdk"></a>Étape1: activer le SDK Windows10

Vous devez d’abord activer le SDK Windows10 pour votre application Win32 si ce n’est pas déjà fait.

Cliquez avec le bouton droit sur votre projet, puis sélectionnez **Décharger le projet**.

![Déchargement d’un projet](images/win32-unload-project.png)

Cliquez à nouveau avec le bouton droit sur votre projet et sélectionnez **Modifier [nom_projet].csproj**

![Modification d’un projet](images/win32-edit-project.png)

Sous le nœud `<TargetFrameworkVersion>` existant, ajoutez un nouveau nœud `<TargetPlatformVersion>` en spécifiant votre version minimale de Windows10 prise en charge. Le Kit de développement logiciel (SDK) réel utilisé est la dernière version que vous avez installée sur votre ordinateur de développement. Il spécifie simplement votre version minimale autorisée (et vous permet de référencer le SDK Windows).

```xml
...
<TargetFrameworkVersion>...</TargetFrameworkVersion>
<TargetPlatformVersion>10.0.10240.0</TargetPlatformVersion>
...
```

Enregistrez vos modifications et rechargez votre projet.

![Recharger un projet](images/win32-reload-project.png)


## <a name="step-2-reference-the-apis"></a>Étape2: référencer les API

Ouvrez le gestionnaire de références (cliquez avec le bouton droit sur le projet, sélectionnez **Ajouter -> Référence**), sélectionnez **Windows-> Core** et incluez les références suivantes:

* Windows.Data
* Windows.UI

![Gestionnaire de références](images/win32-add-windows-reference.png)


## <a name="step-3-copy-compat-library-code"></a>Étape3: copier le code de la bibliothèque de compatibilité

Copiez le [fichier DesktopNotificationManagerCompat.cs de GitHub](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CS/DesktopToastsApp/DesktopNotificationManagerCompat.cs) dans votre projet. La bibliothèque de compatibilité fait abstraction en grande partie de la complexité des notifications de bureau. Les instructions suivantes requièrent la bibliothèque de compatibilité.


## <a name="step-4-implement-the-activator"></a>Étape4: implémenter l’activateur

Vous devez implémenter un gestionnaire pour l’activation de toast, afin que lorsque l’utilisateur clique sur votre toast, votre application peut effectuer une action. Cela est nécessaire pour que votre toast soit conservé dans le centre de notifications (car l’utilisateur peut cliquer sur le toast des jours plus tard lorsque votre application est fermée). Cette classe peut être placée n’importe où dans votre projet.

Étendez la classe **NotificationActivator**, ajoutez les trois attributs répertoriés ci-dessous, puis créez un CLSID GUID unique pour votre application à l’aide de l’un des nombreux générateurs GUID en ligne. Ce CLSID (identificateur de classe) permet au centre de notifications de savoir quelle classe activer par COM.

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-5-register-with-notification-platform"></a>Étape5: s’inscrire sur la plateforme de notification

Vous devez ensuite vous inscrire sur la plateforme de notification. Il existe différentes étapes selon que vous utilisez le Pont du bureau ou Win32 classique. Si vous prenez en charge les deux, vous devez effectuer les deux étapes. Toutefois, il n’est pas nécessaire de répliquer votre code, notre bibliothèque s’en occupe pour vous!


### <a name="desktop-bridge"></a>Pont du bureau

Si vous utilisez le Pont du bureau (ou si vous prenez en charge les deux), dans votre fichier **Package.appxmanifest**, ajoutez:

1. Déclaration de **xmlns:com**
2. Déclaration de **xmlns:desktop**
3. Dans l’attribut **IgnorableNamespaces**, **com** et **desktop**
4. **com:Extension** pour l'activateur COM à l’aide du GUID de l’étape4. Veillez à inclure l’objet `Arguments="-ToastActivated"` afin que vous sachiez que votre lancement provenait d’un toast
5. **desktop:Extension** pour **windows.toastNotificationActivation** pour déclarer le CLSID de votre activateur de toast (le GUID de l’étape4).

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>Win32classique

Si vous utilisez Win32classique (ou si vous prenez en charge les deux), vous devez déclarer l’ID de modèle utilisateur de l’application (AUMID) et le CLSID de l’activateur de toast (le GUID de l’étape4) dans le raccourci de votre application dans le menu Démarrer.

Indiquez un AUMID unique qui identifie votre application Win32. Il se présente généralement sous la forme [CompanyName].[AppName], mais vous devez veiller à ce qu’il soit unique dans toutes les applications (vous pouvez ajouter des chiffres à la fin).

#### <a name="step-51-wix-installer"></a>Étape5.1: programme d’installation WiX

Si vous utilisez WiX pour votre programme d’installation, modifiez le fichier **Product.wxs** pour ajouter les deux propriétés de raccourci au raccourci du menu Démarrer, comme illustré ci-dessous. Assurez-vous que le GUID de l’étape4 est placé entre `{}`, comme indiqué ci-dessous.

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> Pour utiliser réellement les notifications, vous devez installer votre application via le programme d’installation une fois avant le débogage normal, afin que le raccourci du menu Démarrer avec votre AUMID et CLSID soit présent. Une fois que le raccourci du menu Démarrer est présent, vous pouvez déboguer à l’aide de F5 dans Visual Studio.


#### <a name="step-52-register-aumid-and-com-server"></a>Étape5.2: enregistrer l’AUMID et le serveur COM

Ensuite, quel que soit votre programme d’installation, dans le code de démarrage de votre application (avant d’appeler des API de notification), appelez la méthode **RegisterAumidAndComServer**, en spécifiant la classe de votre activateur de notification à l’étape4 et l’AUMID utilisé ci-dessus.

```csharp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

Si vous prenez en charge le Pont du bureau et Win32classique, vous pouvez appeler cette méthode avec l’application de votre choix. Si vous utilisez le Pont du bureau, cette méthode est retournée immédiatement. Il n'est pas nécessaire de répliquer votre code.

Cette méthode vous permet d’appeler les API de compatibilité pour envoyer et gérer les notifications sans avoir à fournir constamment votre AUMID. La clé de Registre LocalServer32 est également insérée pour le serveur COM.


## <a name="step-6-register-com-activator"></a>Étape6: enregistrer l’activateur COM

Pour les applications Pont du bureau et Win32classique, vous devez enregistrer votre type d’activateur de notification, afin de pouvoir gérer les activations de toast.

Dans le code de démarrage de votre application, appelez la méthode **RegisterActivator** suivante, en passant votre implémentation de la classe **NotificationActivator** que vous avez créée à l’étape4. Elle doit être appelée afin de pouvoir recevoir des activations de toast.

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-7-send-a-notification"></a>Étape7: envoyer une notification

L’envoi d’une notification est identique aux applications UWP, sauf que vous utilisez la classe **DesktopNotificationManagerCompat** pour créer un objet **ToastNotifier**. La bibliothèque de compatibilité gère automatiquement la différence entre le Pont du bureau et Win32classique, vous n’avez donc pas besoin de répliquer votre code. Pour Win32classique, la bibliothèque de compatibilité met en cache l’AUMID que vous avez fourni lors de l’appel de **RegisterAumidAndComServer** de manière à ce que vous n’ayez pas à vous soucier de fournir ou non l’AUMID.

> [!NOTE]
> Installez la [bibliothèque Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) afin de pouvoir construire des notifications avec C# comme illustré ci-dessous, au lieu d’utiliser du code XML brut.

Assurez-vous d’utiliser l’objet **ToastContent** indiqué ci-dessous (ou le modèle ToastGeneric si vous créez le code XML), car les modèles de notification toast Windows8.1 hérités n’activent pas l’activateur de notification COM que vous avez créé à l’étape4.

> [!IMPORTANT]
> Les images http sont uniquement prises en charge dans les applications Pont du bureau qui disposent de la fonctionnalité Internet dans leur manifeste. Les applications Win32classiques ne prennent pas en charge les images http; vous devez télécharger l’image dans vos données d’application locales et la référencer localement.

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContent()
{
    // Arguments when the user taps body of toast
    Launch = "action=viewConversation&conversationId=5",

    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Hello world!"
                }
            }
        }
    }
};

// Create the XML document (BE SURE TO REFERENCE WINDOWS.DATA.XML.DOM)
var doc = new XmlDocument();
doc.LoadXml(toastContent.GetContent());

// And create the toast notification
var toast = new ToastNotification(doc);

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> Les applications Win32 classiques ne peuvent pas utiliser les modèles de notifications toast hérités (comme ToastText02). L’activation des modèles hérités échoue lorsque le CLSID COM est spécifié. Vous devez utiliser les modèles de Windows10 ToastGeneric comme indiqué ci-dessus.


## <a name="step-8-handling-activation"></a>Étape8: gestion de l’activation

Lorsque l’utilisateur clique sur votre toast, la méthode **OnActivated** de votre classe **NotificationActivator** est appelée.

Dans la méthode OnActivated, vous pouvez analyser les arguments que vous avez spécifiés dans le toast et obtenir l’entrée utilisateur que l’utilisateur a saisi ou sélectionné, puis activer votre application en conséquence.

> [!NOTE]
> La méthode **OnActivated** n’est pas appelée sur le thread d’interface utilisateur. Si vous souhaitez effectuer des opérations sur le thread d’interface utilisateur, vous devez appeler `Application.Current.Dispatcher.Invoke(callback)`.

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

Pour assurer une prise en charge correcte du lancement lorsque votre application est fermée, dans votre fichier `App.xaml.cs`, vous pouvez remplacer la méthode **OnStartup** (pour les applications WPF) pour déterminer si le lancement s’effectue à partir d’un toast ou non. Si le lancement s’effectue à partir d’un toast, l’argument de lancement «-ToastActivated» est utilisé. Lorsque cet argument s’affiche, vous devez arrêter l’exécution de tout code d’activation de lancement normal et autoriser le lancement de votre code **OnActivated**.

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>Ordre d’activation des événements

Pour WPF, l’ordre d’activation est le suivant...

Si votre application est déjà en cours d’exécution:

1. **OnActivated** dans votre **NotificationActivator** est appelé

Si votre application n’est pas en cours d’exécution:

1. **OnStartup** dans `App.xaml.cs` est appelé avec «-ToastActivated» comme **Arguments**
2. **OnActivated** dans votre **NotificationActivator** est appelé


### <a name="foreground-vs-background-activation"></a>Activation au premier plan et en arrière-plan
Pour les applications de bureau, l’activation au premier plan et en arrière-plan est gérée de manière identique: votre activateur COM est appelé. C’est le code de votre application qui décide d’afficher une fenêtre ou d’exécuter simplement des tâches et de quitter. Par conséquent, spécifier **Background** comme **ActivationType** dans le contenu de votre toast ne modifie pas le comportement.


## <a name="step-9-remove-and-manage-notifications"></a>Étape9: supprimer et gérer les notifications

La suppression et la gestion des notifications sont identiques aux applications UWP. Toutefois, nous vous recommandons d’utiliser notre bibliothèque de compatibilité pour obtenir un objet **DesktopNotificationHistoryCompat**; vous n’avez donc pas à vous soucier de fournir l’AUMID si vous utilisez Win32 classique.

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-10-deploying-and-debugging"></a>Étape10: déploiement et débogage

Pour déployer et déboguer votre application Pont du bureau, consultez [Exécuter, déboguer et tester une application de bureau empaquetée](/porting/desktop-to-uwp-debug.md).

Pour déployer et déboguer votre application Win32 classique, vous devez installer votre application via le programme d’installation une fois avant le débogage normal, afin que le raccourci du menu Démarrer avec votre AUMID et CLSID soit présent. Une fois que le raccourci du menu Démarrer est présent, vous pouvez déboguer à l’aide de F5 dans Visual Studio.

Si vos notifications ne parviennent pas à s’afficher dans votre application Win32 classique (et aucune exception n’est levée), cela signifie probablement que le raccourci du menu Démarrer n’est pas présent (installez votre application via le programme d’installation), ou l’AUMID que vous avez utilisé dans le code ne correspond pas à l’AUMID dans le raccourci du menu Démarrer.

Si vos notifications s’affichent, mais ne sont pas conservées dans le centre de notifications (disparaissent une fois que la fenêtre contextuelle est ignorée), cela signifie que vous n’avez pas implémenté l’activateur COM correctement.

Si vous avez installé votre application Pont du bureau et Win32 classique, notez que l’application Pont du bureau remplace l’application Win32 classique lors de la gestion des activations du toast. Cela signifie que les toasts de l’application Win32 classique lancent toujours l’application Pont du bureau lorsque l’utilisateur clique dessus. La désinstallation de l’application Pont du bureau rétablit les activations sur l’application Win32 classique.


## <a name="known-issues"></a>Problèmes connus

**RÉSOLU: l’application n’est pas sélectionnée lorsque vous cliquez sur le toast**: dans les builds15063 et les versions antérieures, les droits au premier plan n’ont pas été transférés vers votre application lorsque nous avons activé le serveur COM. Par conséquent, votre application clignote simplement lorsque vous avez tenté de la déplacer au premier plan. Aucune solution n’était disponible pour ce problème. Nous avons résolu ce problème dans les builds 16299 et les versions ultérieures.


## <a name="resources"></a>Ressources

* [Exemple de code complet sur GitHub](https://github.com/WindowsNotifications/desktop-toasts)
* [Notifications toast à partir d'applications de bureau](toast-desktop-apps.md)
* [Documentation sur le contenu des toasts](adaptive-interactive-toasts.md)

