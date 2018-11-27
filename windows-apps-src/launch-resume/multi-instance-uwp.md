---
title: Créer une application Windows universelle à instances multiples
description: Cette rubrique décrit comment écrire des applications UWP qui prennent en charge l’instanciation multiple.
keywords: applications UWP à instances multiples
ms.date: 09/21/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6cceac0cf4b9cc4c13c0e99ce5beffad70787256
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7710621"
---
# <a name="create-a-multi-instance-universal-windows-app"></a>Créer une application Windows universelle à instances multiples

Cette rubrique décrit comment créer une application de plateforme Windows universelle (UWP) à instances multiples.

À partir de Windows 10, version 1803 (version 10.0; Build 17134) attache, votre application UWP peut participer à prendre en charge plusieurs instances. Si une instance d’une application UWP à instances multiples est en cours d’exécution et qu’une demande d’activation consécutive est reçue, la plateforme n’activera pas l’instance existante. Au lieu de cela, elle créera une nouvelle instance qui s’exécutera dans un processus distinct.

> [!IMPORTANT]
> L’instanciation multiple est prise en charge pour les applications JavaScript, mais n’est pas la redirection de l’instanciation multiple. Dans la mesure où la redirection de l’instanciation multiple n’est pas pris en charge pour les applications JavaScript, la classe [**AppInstance**](/uwp/api/windows.applicationmodel.appinstance) n’est pas utile pour des applications.

## <a name="opt-in-to-multi-instance-behavior"></a>Participer au comportement à instances multiples

Si vous créez une nouvelle application à instances multiples, vous pouvez installer les **modèles .VSIX de projets d’applications à instances multiples**, disponibles dans le [Visual Studio Marketplace](https://aka.ms/E2nzbv). Une fois que vous avez installé les modèles, ils sont disponibles dans la boîte de dialogue **Nouveau projet** sous **Visual C# > Windows universel** (ou **Autres langages > Visual C++ > Windows universel**).

Deux modèles sont installés: **Application UWP à instances multiples**, qui fournit le modèle pour la création d’une application à instances multiples, et **Application UWP de redirection à instances multiples**, qui fournit une logique supplémentaire sur laquelle vous pouvez vous appuyer pour lancer une nouvelle instance ou activer de manière sélective une instance qui a déjà été lancée. Par exemple, vous souhaitez peut-être éviter que plusieurs instances modifient simultanément un même document, afin de mettre au premier plan l’instance où ce fichier est ouvert, au lieu de lancer une nouvelle instance.

Ajoutent les deux modèles `SupportsMultipleInstances` à la `package.appxmanifest` fichier. Notez les préfixes d’espace de noms `desktop4` et `iot2`: seuls les projets qui ciblent le bureau, ou des projets Internet des objets (IoT), prend en charge l’instanciation multiple.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2"  
  IgnorableNamespaces="uap mp desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:SupportsMultipleInstances="true"
      iot2:SupportsMultipleInstances="true">
      ...
    </Application>
  </Applications>
   ...
</Package>
```

## <a name="multi-instance-activation-redirection"></a>Redirection de l'activation à instances multiples

 La prise en charge de l'instanciation multiple pour les applications UWP permet non seulement de lancer plusieurs instances de l’application, mais offre aussi des options de personnalisation si vous souhaitez décider de lancer ou non une nouvelle instance de votre application ou d'activer ou non une instance déjà en cours d’exécution. Par exemple, si l’application est lancée pour modifier un fichier qui est déjà en cours de modification dans une autre instance, vous pouvez rediriger l’activation vers cette instance au lieu d’ouvrir une autre instance qui est déjà en train de modifier le fichier.

Pour la voir en action, regardez cette vidéo sur la création d’applications UWP instances multiples.

> [!VIDEO https://www.youtube.com/embed/clnnf4cigd0]

Le modèle d'**application UWP de redirection à instances multiples** ajoute `SupportsMultipleInstances` au fichier package.appxmanifest comme indiqué ci-dessus, et ajoute également un objet **Program.cs** (ou **Program.cpp**, si vous utilisez la version C++ du modèle) à votre projet qui contient une fonction `Main()`. La logique de redirection de l’activation est intégrée à la fonction `Main`. Le modèle pour **Program.cs** est illustré ci-dessous.

La propriété [**AppInstance.RecommendedInstance**](/uwp/api/windows.applicationmodel.appinstance.recommendedinstance) représente l’instance fourni par l’interpréteur de commandes par défaut pour cette demande d’activation, s’il existe (ou `null` si il n’existe pas). Si l’interpréteur de commandes fournit une préférence, puis vous pouvez rediriger l’activation vers cette instance, ou vous pouvez l’ignorer si vous choisissez.

``` csharp
public static class Program
{
    // This example code shows how you could implement the required Main method to
    // support multi-instance redirection. The minimum requirement is to call
    // Application.Start with a new App object. Beyond that, you may delete the
    // rest of the example code and replace it with your custom code if you wish.

    static void Main(string[] args)
    {
        // First, we'll get our activation event args, which are typically richer
        // than the incoming command-line args. We can use these in our app-defined
        // logic for generating the key for this instance.
        IActivatedEventArgs activatedArgs = AppInstance.GetActivatedEventArgs();

        // If the Windows shell indicates a recommended instance, then
        // the app can choose to redirect this activation to that instance instead.
        if (AppInstance.RecommendedInstance != null)
        {
            AppInstance.RecommendedInstance.RedirectActivationTo();
        }
        else
        {
            // Define a key for this instance, based on some app-specific logic.
            // If the key is always unique, then the app will never redirect.
            // If the key is always non-unique, then the app will always redirect
            // to the first instance. In practice, the app should produce a key
            // that is sometimes unique and sometimes not, depending on its own needs.
            string key = Guid.NewGuid().ToString(); // always unique.
                                                    //string key = "Some-App-Defined-Key"; // never unique.
            var instance = AppInstance.FindOrRegisterInstanceForKey(key);
            if (instance.IsCurrentInstance)
            {
                // If we successfully registered this instance, we can now just
                // go ahead and do normal XAML initialization.
                global::Windows.UI.Xaml.Application.Start((p) => new App());
            }
            else
            {
                // Some other instance has registered for this key, so we'll 
                // redirect this activation to that instance instead.
                instance.RedirectActivationTo();
            }
        }
    }
}
```

`Main()` est le premier élément qui s’exécute. Elle s’exécute avant [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) et [**OnActivated**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnActivated_Windows_ApplicationModel_Activation_IActivatedEventArgs_). Cela vous permet de décider d'activer ou non cette instance ou une autre avant l'exécution de tout autre code d’initialisation dans votre application.

Le code ci-dessus détermine si une instance existante ou nouvelle de votre application est activée. Une clé est utilisée pour déterminer s’il existe une instance existante que vous souhaitez activer. Par exemple, si votre application peut être lancée pour [gérer l’activation des fichiers](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/handle-file-activation), vous pouvez utiliser le nom de fichier en tant que clé. Ensuite, vous pouvez vérifier si une instance de votre application est déjà enregistrée avec cette clé et l’activer au lieu d’ouvrir une nouvelle instance. Il s’agit de l’idée sous-jacente au code: `var instance = AppInstance.FindOrRegisterInstanceForKey(key);`

Si une instance enregistrée avec la clé est identifiée, cette instance est activée. Si la clé est introuvable, l’instance actuelle (l’instance qui s'exécute actuellement `Main`) crée son objet d'application et commence à s’exécuter.

## <a name="background-tasks-and-multi-instancing"></a>Tâches en arrière-plan et instanciation multiple

- Les tâches en arrière-plan hors processus prennent en charge l’instanciation multiple. En général, chaque nouveau déclencheur aboutit à une nouvelle instance de la tâche en arrière-plan (même si d'un point de vue technique, plusieurs tâches en arrière-plan peuvent s’exécuter dans le même processus hôte). Toutefois, une autre instance de la tâche en arrière-plan est créée.
- Les tâches en arrière-plan in-process ne prennent pas en charge l’instanciation multiple.
- Les tâches audio en arrière-plan ne prennent pas en charge l’instanciation multiple.
- Lorsqu’une application enregistre une tâche en arrière-plan, elle commence généralement par vérifier si cette tâche est déjà enregistrée avant soit de la supprimer ou de la réenregistrer, soit de ne rien faire afin de conserver l’enregistrement existant. Il s’agit également du comportement par défaut avec les applications à instances multiples. Cependant, une application à instances multiples peut choisir d’enregistrer un nom de tâche en arrière-plan différent en fonction de l'instance concernée. Cela entraîne plusieurs enregistrements pour le même déclencheur, et l'activation de plusieurs instances de tâche en arrière-plan lorsque le déclencheur se déclenche.
- Les services d’application lancent une nouvelle instance de la tâche en arrière-plan du service d’application pour chaque connexion. Cela reste inchangé pour les applications à instances multiples, à savoir que chaque instance d’une application à instances multiples est associée à sa propre instance de la tâche en arrière-plan du service d’application. 

## <a name="additional-considerations"></a>Autres éléments à prendre en considération

- L'instanciation multiple est prise en charge par les applications UWP qui ciblent le bureau et les projets IoT.
- Pour éviter des conditions de concurrence et des problèmes de contention, les applications à instances multiples doivent prendre des mesures pour partitionner/synchroniser l’accès aux paramètres, au stockage local des applications et à toute autre ressource (par exemple, des fichiers utilisateur ou un magasin de données) qui peut être partagée entre plusieurs instances. Par exemple, les mutex, sémaphores, événements et ainsi de suite, les mécanismes de synchronisation standard sont disponibles.
- Si l’application dispose de `SupportsMultipleInstances` dans son fichier Package.appxmanifest, ses extensions n’ont pas besoin de déclarer `SupportsMultipleInstances`. 
- Si vous ajoutez `SupportsMultipleInstances` à toute autre extension, hormis les tâches d'arrière-plan tâches ou les services d’application, et que l’application qui héberge l’extension ne déclare pas également `SupportsMultipleInstances` dans son fichier Package.appxmanifest, une erreur de schéma est générée.
- Les applications peuvent utiliser la déclaration de [**ResourceGroup**](https://docs.microsoft.com/windows/uwp/launch-resume/declare-background-tasks-in-the-application-manifest) dans leur manifeste pour regrouper plusieurs tâches en arrière-plan dans le même hôte. Cela est en conflit avec l’instanciation multiple, où chaque activation est intégrée à un hôte distinct. Par conséquent, une application ne peut pas déclarer à la fois `SupportsMultipleInstances` et `ResourceGroup` dans son manifeste.

## <a name="sample"></a>Exemple

Consultez l' [exemple d’instances multiples](https://aka.ms/Kcrqst) pour obtenir un exemple de redirection de l’activation des instances multiples.

## <a name="see-also"></a>Articles associés

[AppInstance.FindOrRegisterInstanceForKey](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_FindOrRegisterInstanceForKey_System_String_)
[AppInstance.GetActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_GetActivatedEventArgs)
[AppInstance.RedirectActivationTo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinstance#Windows_ApplicationModel_AppInstance_RedirectActivationTo)
[Gérer l’activation d'une application](https://docs.microsoft.com/windows/uwp/launch-resume/activate-an-app)