---
author: TylerMSFT
title: Gérer la suspension d’une application
description: Découvrez comment enregistrer d’importantes données d’application lorsque le système suspend l’exécution de votre application.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 7cb93c410f583884f75f21d9beda03db87c024f9
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7144915"
---
# <a name="handle-app-suspend"></a>Gérer la suspension d’une application

**API importantes**

- [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341)

Apprenez à enregistrer d’importantes données d’application lorsque le système suspend votre application. L’exemple inscrit un gestionnaire pour l’événement [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) et enregistre une chaîne dans un fichier.

## <a name="register-the-suspending-event-handler"></a>Enregistrer le gestionnaire d’événements de suspension

Enregistrez-vous pour gérer l’événement [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341), qui indique que votre application doit enregistrer ses données d’application avant que le système la suspende.

```csharp
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

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Suspending({ this, &MainPage::App_Suspending });
}
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

## <a name="save-application-data-before-suspension"></a>Enregistrer des données d’application avant sa suspension

Lorsque votre application traite l’événement [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341), elle a la possibilité d’enregistrer ses données d’application importantes dans la fonction du gestionnaire. L’application doit utiliser l’API de stockage [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) pour enregistrer les données d’application simples de manière synchrone.

```csharp
partial class MainPage
{
    async void App_Suspending(
        Object sender,
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object,
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated.
    End Sub

End Class
```

```cppwinrt
void MainPage::App_Suspending(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::ApplicationModel::SuspendingEventArgs const& /* e */)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

## <a name="release-resources"></a>Libérer des ressources

Vous devez également libérer les ressources exclusives et les descripteurs de fichiers pour permettre aux autres applications d’y accéder lorsque votre application est suspendue. Appareils photo, périphériques d’E/S, appareils externes et ressources réseau sont autant d’exemples de ressources exclusives. En libérant explicitement les ressources exclusives et les descripteurs de fichiers, vous permettez aux autres applications d’y accéder lorsque votre application est suspendue. Lorsqu’elle est réactivée, l’application doit se réapproprier ses ressources exclusives et descripteurs de fichiers.

## <a name="remarks"></a>Remarques

Le système suspend votre application chaque fois que l’utilisateur passe à une autre application, au Bureau ou à l’écran d’accueil. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan.

Le système tente de conserver votre application et ses données en mémoire pendant sa suspension. Toutefois, si le système ne dispose pas des ressources pour conserver votre application en mémoire, il met fin à votre application. Lorsque l’utilisateur rebascule vers une application suspendue qui a été arrêtée, le système envoie un événement [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) et doit restaurer ses données d’application dans sa méthode [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

Le système ne vous notifie pas de l’arrêt d’une application. Celle-ci doit donc enregistrer ses données d’application et libérer les ressources exclusives et descripteurs de fichiers au moment où elle est mise en suspens pour ensuite les restaurer lorsque l’application est activée après avoir été arrêtée.

Si vous effectuez un appel asynchrone depuis votre gestionnaire, le contrôle renvoie immédiatement un retour de cet appel. Cela signifie que l’exécution peut ensuite revenir de votre gestionnaire d’événements et votre application prend l’état suivant, même si l’appel asynchrone n’est pas encore terminé. Utilisez la méthode [**GetDeferral**](http://aka.ms/Kt66iv) sur l’objet [**EnteredBackgroundEventArgs**](http://aka.ms/Ag2yh4) qui est transmis à votre gestionnaire d’événements pour retarder la suspension jusqu'à ce que vous appeliez la méthode [**Complete**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx) sur l’objet [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) renvoyé.

Un report n’augmente pas le temps d’exécution nécessaire de votre code avant l’arrêt de votre application. Cela ne retarde que l’arrêt jusqu'à ce que la méthode *Complete* soit appelée ou que la date d’échéance ne soit passée, *la première de ces deuxéventualités prévalant*. Pour étendre la durée en l’état d’interruption en cours usage [ **ExtendedExecutionSession**](run-minimized-with-extended-execution.md)

> [!NOTE]
> Pour améliorer la réactivité du système dans Windows8.1, les applications disposent d’un accès de faible priorité aux ressources de cas de suspension. Pour prendre en charge cette nouvelle priorité, le délai de l’opération de suspension est prolongé afin que l’application dispose d’un délai de 5secondes en priorité normale sur Windows ou de 1 à 10secondes sur Windows Phone. Vous ne pouvez pas étendre ni modifier ce délai.

**Remarque concernant le débogage à l’aide de Visual Studio :** Visual Studio empêche Windows de suspendre une application qui est jointe au débogueur afin que l’utilisateur puisse voir l’interface de débogage de Visual Studio pendant l’exécution de l’application. Lorsque vous déboguez une application, vous pouvez lui envoyer un événement de suspension à l’aide de Visual Studio. Assurez-vous que la barre d’outils **Emplacement de débogage** est visible et cliquez sur l’icône **Suspendre**.

## <a name="related-topics"></a>Rubriques connexes

* [Cycle de vie de l’application](app-lifecycle.md)
* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la reprise d’une application](resume-an-app.md)
* [Recommandations en matière d’expérience utilisateur pour le lancement, la suspension et la reprise](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [Exécution étendue](run-minimized-with-extended-execution.md)

 

 
