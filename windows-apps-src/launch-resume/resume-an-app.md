---
author: TylerMSFT
title: "Gérer la reprise d’une application"
description: "Apprenez à actualiser le contenu à l’écran lorsque le système reprend l’exécution de votre application."
ms.assetid: DACCC556-B814-4600-A10A-90B82664EA15
ms.sourcegitcommit: e6957dd44cdf6d474ae247ee0e9ba62bf17251da
ms.openlocfilehash: dd3d75c7f3dfe325324e1fe31c039cd207b68d0b

---

# Gérer la reprise d’une application


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)

Apprenez à actualiser le contenu à l’écran lorsque le système reprend l’exécution de votre application. L’exemple présenté dans cette rubrique enregistre un gestionnaire d’événements pour l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).

**Feuille de route :** comment cette rubrique s’articule-t-elle par rapport aux autres ? Voir :

-   [Feuille de route pour les applications Windows Runtime en C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583)
-   [Feuille de route pour les applications Windows Runtime en C++](https://msdn.microsoft.com/library/windows/apps/hh700360)

## Enregistrer le gestionnaire d’événement de reprise

Enregistrez-vous pour traiter l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), qui indique que l’utilisateur revient vers votre application après s’en être éloigné.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Resuming += new EventHandler<Object>(App_Resuming);
>    }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Resuming, AddressOf App_Resuming
>    End Sub
>
> End Class
> ```
> ```cpp
> MainPage::MainPage()
> {
>     InitializeComponent();
>     Application::Current->Resuming +=
>         ref new EventHandler<Platform::Object^>(this, &MainPage::App_Resuming);
> }
> ```

## Actualiser le contenu affiché après la suspension

Lorsque votre application traite l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), elle a la possibilité d’actualiser son contenu à l’écran.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     private void App_Resuming(Object sender, Object e)
>     {
>         // TODO: Refresh network data
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Resuming(sender As Object, e As Object)
>  
>         ' TODO: Refresh network data
>
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Resuming(Object^ sender, Object^ e)
> {
>     // TODO: Refresh network data
> }
> ```

> **Remarque** Étant donné que l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) n’est pas déclenché depuis le thread d’interface utilisateur, un répartiteur doit être utilisé pour accéder au thread en question et injecter une mise à jour à l’IU, si c’est ce que vous souhaitez faire dans votre gestionnaire.

## Notes


Le système suspend votre application chaque fois que l’utilisateur bascule vers une autre application ou vers le Bureau. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan. Cependant, il se peut que l’application ait été suspendue pendant une durée significative. Elle doit dans ce cas actualiser le contenu affiché susceptible d’avoir changé pendant l’inactivité, par exemple les flux d’actualités ou la localisation de l’utilisateur.

Si votre application ne contient pas de contenu à actualiser, il n’y a alors pas besoin de gérer l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339).

> **Remarque** Lorsque votre application est jointe au débogueur Visual Studio, vous pouvez lui envoyer un événement **Resume**. Assurez-vous que la **barre d’outils Emplacement de débogage** est visible, et cliquez sur la liste déroulante à côté de l’icône **Suspendre**. Ensuite, choisissez **Reprendre**.

> **Remarque** Dans les applications du Windows Phone Store, l’événement [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) est toujours suivi de l’événement [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), même lorsque votre application est suspendue et que l’utilisateur relance votre application à partir d’une vignette principale ou d’une liste d’applications. Les applications peuvent ignorer l’initialisation si un contenu est déjà défini sur la fenêtre active. Vous pouvez vérifier la propriété [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) pour déterminer si l’application a été lancée à partir d’une vignette principale ou secondaire et, en fonction de l’information obtenue, décider si vous devez présenter une expérience de nouvelle exécution ou de reprise d’exécution de l’application.

## Rubriques connexes

* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la suspension d’une application](suspend-an-app.md)
* [Recommandations pour la suspension et la reprise d’une application](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Cycle de vie de l’application](app-lifecycle.md)



<!--HONumber=Jun16_HO4-->


