---
title: Gérer la reprise d’une application
description: Apprenez à actualiser le contenu à l’écran lorsque le système reprend l’exécution de votre application.
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.date: 07/06/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f424a274d3e96b58f32875620f3165ccfac82ba6
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8883804"
---
# <a name="handle-app-resume"></a>Gérer la reprise d’une application

**API importantes**

- [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)

Découvrez à quel endroit vous devez actualiser votre interface utilisateur lorsque le système reprend l’exécution de votre application. L’exemple présenté dans cette rubrique enregistre un gestionnaire d’événements pour l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).

## <a name="register-the-resuming-event-handler"></a>Enregistrer le gestionnaire d’événement de reprise

Enregistrez-vous pour gérer l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), qui indique que l’utilisateur revient vers votre application après s’en être éloigné.

```csharp
partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
   }
}
```

```vb
Public NonInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Resuming, AddressOf App_Resuming
   End Sub

End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Resuming({ this, &MainPage::App_Resuming });
}
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();
    Application::Current->Resuming +=
        ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
}
```

## <a name="refresh-displayed-content-and-reacquire-resources"></a>Actualiser le contenu affiché et récupérer les ressources

Le système suspend votre application quelques secondes après que l’utilisateur bascule vers une autre application ou vers le Bureau. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système restaure l’application là où elle s’était arrêtée. Pour l’utilisateur, c’est comme si l’application était exécutée en arrière-plan.

Lorsque votre application gère l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), votre application peut être suspendue pendant plusieurs heures ou jours. Elle doit alors actualiser le contenu ayant pu devenir obsolète pendant l’interruption de l’application, tel que les flux d’actualités ou l’emplacement de l’utilisateur.

C’est également le moment idéal pour restaurer toutes les ressources exclusives libérées lors de la suspension de votre application, telles que les descripteurs de fichiers, les appareils photo, les périphériques d’E/S, les périphériques externes et les ressources réseau.

```csharp
partial class MainPage
{
    private void App_Resuming(Object sender, Object e)
    {
        // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Resuming(sender As Object, e As Object)
 
        ' TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.

    End Sub
>
End Class
```

```cppwinrt
void MainPage::App_Resuming(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::Foundation::IInspectable const& /* e */)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

```cpp
void MainPage::App_Resuming(Object^ sender, Object^ e)
{
    // TODO: Refresh network data, perform UI updates, and reacquire resources like cameras, I/O devices, etc.
}
```

> [!NOTE]
> Dans la mesure où l’événement de [**reprise**](https://msdn.microsoft.com/library/windows/apps/br242339) n’est pas déclenché à partir du thread d’interface utilisateur, un répartiteur dans votre gestionnaire pour transférer les appels vers votre interface utilisateur.

## <a name="remarks"></a>Remarques

Une application jointe au débogueur Visual Studio ne sera pas suspendue. Toutefois, vous pouvez la suspendre à partir du débogueur et lui envoyer un événement **Resume** afin de déboguer votre code. Assurez-vous que la **barre d’outils Emplacement de débogage** est visible et cliquez sur la liste déroulante à côté de l’icône **Suspendre**. Ensuite, sélectionnez **Reprendre**.

Dans les applications du Windows Phone Store, l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) est toujours suivi de l’événement [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), même lorsque votre application est suspendue et que l’utilisateur relance votre application à partir d’une vignette principale ou d’une liste d’applications. Les applications peuvent ignorer l’initialisation si un contenu est déjà défini sur la fenêtre active. Vous pouvez vérifier la propriété [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) pour déterminer si l’application a été lancée à partir d’une vignette principale ou secondaire et, en fonction de l’information obtenue, décider si vous devez présenter une expérience de nouvelle exécution ou de reprise d’exécution de l’application.

## <a name="related-topics"></a>Rubriques connexes

* [Cycle de vie de l’application](app-lifecycle.md)
* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la suspension d’une application](suspend-an-app.md)
