---
author: mcleblanc
title: Gérer la suspension d’une application
description: Apprenez à enregistrer d’importantes données d’application lorsque le système suspend votre application.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
---

# Gérer la suspension d’une application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341)

Apprenez à enregistrer d’importantes données d’application lorsque le système suspend votre application. L’exemple inscrit un gestionnaire pour l’événement [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) et enregistre une chaîne dans un fichier.

## Enregistrer le gestionnaire d’événements de suspension


Enregistrez-vous pour gérer l’événement [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341), qui indique que votre application doit enregistrer ses données d’application avant que le système la suspende.

> [!div class="tabbedCodeSnippets"]
```cs
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```
```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent() 
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```
```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending += 
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## Enregistrer des données d’application avant sa suspension


Lorsque votre application traite l’événement [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341), elle a la possibilité d’enregistrer ses données d’application importantes dans la fonction du gestionnaire. L’application doit utiliser l’API de stockage [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) pour enregistrer les données d’application simples de manière synchrone.

> [!div class="tabbedCodeSnippets"]
```cs
partial class MainPage
{
    async void App_Suspending(
        Object sender, 
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated
    }
}
```
```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object, 
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated
    End Sub

End Class
```
```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated
}
```

## Remarques


Le système suspend votre application chaque fois que l’utilisateur passe à une autre application, au Bureau ou à l’écran d’accueil. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan.

Le système tente de conserver votre application et ses données en mémoire pendant sa suspension. Toutefois, si le système ne dispose pas des ressources pour conserver votre application en mémoire, il met fin à votre application. Lorsque l’utilisateur rebascule vers une application suspendue qui a été arrêtée, le système envoie un événement [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) et doit restaurer ses données d’application dans sa méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

Le système ne vous notifie pas de l’arrêt d’une application. Celle-ci doit donc enregistrer ses données d’application et libérer les ressources exclusives et descripteurs de fichiers au moment où elle est mise en suspens pour ensuite les restaurer lorsque l’application est activée après avoir été arrêtée.

> **Remarque** Si vous devez effectuer des tâches asynchrones lorsque votre application est en cours de suspension, vous devez différer l’exécution de la suspension tant que vos tâches ne sont pas terminées. Vous pouvez utiliser la méthode [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) sur l’objet [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) (disponible via les arguments de l’événement) pour retarder la suspension jusqu’à ce que vous appeliez la méthode [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) et qu’elle soit appliquée sur l’objet [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) retourné.

> **Remarque** Pour améliorer la réactivité du système dans Windows 8.1, les applications disposent d’un accès à faible priorité aux ressources en cas de suspension. Pour prendre en charge cette nouvelle priorité, le délai de l’opération de suspension est prolongé afin que l’application dispose d’un délai de 5 secondes en priorité normale sur Windows ou de 1 à 10 secondes sur Windows Phone. Vous ne pouvez pas étendre ni modifier ce délai.

> **Remarque concernant le débogage à l’aide de Visual Studio :** Visual Studio empêche Windows de suspendre une application qui est jointe au débogueur afin que l’utilisateur puisse voir l’interface de débogage de Visual Studio pendant l’exécution de l’application. Lorsque vous déboguez une application, vous pouvez lui envoyer un événement de suspension à l’aide de Visual Studio. Assurez-vous que la barre d’outils **Emplacement de débogage** est visible et cliquez sur l’icône **Suspendre**.

## Rubriques connexes


* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la reprise d’une application](resume-an-app.md)
* [Recommandations en matière d’expérience utilisateur pour le lancement, la suspension et la reprise](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [Cycle de vie de l’application](app-lifecycle.md)

 

 





<!--HONumber=May16_HO2-->


