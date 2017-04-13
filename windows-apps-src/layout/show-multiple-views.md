---
author: Jwmsft
Description: "Aidez les utilisateurs à accroître leur productivité en leur permettant d’afficher plusieurs parties indépendantes de votre application dans des fenêtres distinctes."
title: "Afficher plusieurs vues d’une application"
ms.assetid: BAF9956F-FAAF-47FB-A7DB-8557D2548D88
label: Show multiple views for an app
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 87f3d5e75b361d1ba9d2c304e58542803da66cd4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="show-multiple-views-for-an-app"></a>Afficher plusieurs vues d’une application

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Vous pouvez aider les utilisateurs à accroître leur productivité en leur permettant d’afficher plusieurs parties indépendantes de votre application dans des fenêtres distinctes. Une application de courrier électronique est un exemple classique dans lequel la principale interface utilisateur affiche la liste des messages électroniques et un aperçu du message sélectionné. Toutefois, les utilisateurs peuvent également ouvrir les messages dans des fenêtres séparées et les afficher côte à côte.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094)</li>
<li>[**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)</li>
</ul>
</div> 

Quand vous créez plusieurs fenêtres pour une application, chacune d’elles se comporte de manière indépendante. La barre des tâches répertorie chaque fenêtre séparément. Les utilisateurs peuvent déplacer, redimensionner, afficher et masquer des fenêtres d’application indépendamment et ils peuvent basculer d’une fenêtre à une autre comme s’il s’agissait d’applications distinctes. Chaque fenêtre opère dans son propre thread.

## <a name="what-is-a-view"></a>Qu’est-ce qu’une vue?


Une vue d’application est l’association de type 1:1 d’un thread et d’une fenêtre que l’application utilise pour afficher le contenu. Elle est représentée par un objet [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017).

Les vues sont gérées par l’objet [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). Vous devez appeler [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) pour créer un objet [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). L’objet **CoreApplicationView** réunit [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) et [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) (stockés dans les propriétés [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) et [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264)). Vous pouvez considérer la **CoreApplicationView** comme l’objet qui utilise WindowsRuntime pour interagir avec le système Windows principal.

En règle générale, vous ne travaillez pas directement avec la [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). À la place, WindowsRuntime fournit la classe [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/hh701658) dans l’espace de noms [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/br242295). Cette classe fournit des propriétés, des méthodes et des événements que vous utilisez quand votre application interagit avec le système de fenêtrage. Pour fonctionner avec une **ApplicationView**, appelez la méthode statique [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672), qui obtient une instance **ApplicationView** liée au thread actuel de la **CoreApplicationView**.

De même, l’infrastructure XAML enveloppe l’objet [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) dans un objet [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041). Dans une application XAML, vous interagissez généralement avec l’objet **Window** au lieu de travailler directement avec la **CoreWindow**.

## <a name="show-a-new-view"></a>Afficher une nouvelle vue


Avant d’aller plus loin, examinons les étapes nécessaires pour créer une vue. Ici, la nouvelle vue est lancée en réponse à un clic sur un bouton.

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**Pour afficher une nouvelle vue**

1.  Appelez [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291) pour créer une fenêtre et un thread pour le contenu de la vue.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Suivez l’élément [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la nouvelle vue. Vous pouvez utiliser cette fonction pour afficher la vue plus tard.

    Vous souhaiterez peut-être prendre en compte la création d’une infrastructure dans votre application pour faciliter le suivi des vues que vous créez. Voir la classe `ViewLifetimeControl` dans l’[exemple MultipleViews](http://go.microsoft.com/fwlink/p/?LinkId=620574) pour plus d’informations.

    ```csharp
    int newViewId = 0;
    ```

3.  Sur le nouveau thread, remplissez la fenêtre.

    Vous devez utiliser la méthode [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) pour planifier le travail sur le thread d’interface utilisateur de la nouvelle vue. Vous utilisez une [expression lambda](http://go.microsoft.com/fwlink/p/?LinkId=389615) pour transmettre une fonction en tant qu’argument à la méthode **RunAsync**. Le travail que vous effectuez dans la fonction lambda se répercute sur le thread de la nouvelle vue.

    En XAML, vous ajoutez généralement un élément [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) à la propriété [**Content**](https://msdn.microsoft.com/library/windows/apps/br209041) de [**Window**](https://msdn.microsoft.com/library/windows/apps/br209051), puis parcourez **Frame** vers une [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) XAML dans laquelle vous avez défini le contenu de votre application. Pour en savoir plus, voir [Navigation pair à pair entre deux pages](navigate-between-two-pages.md).

    Une fois le nouvel élément [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) rempli, vous devez appeler la méthode **[Activate](https://msdn.microsoft.com/library/windows/apps/br209046)** de **Window** pour afficher l’élément **Window** ultérieurement. Cela se produit sur le thread de la nouvelle vue; ainsi la nouvelle **Window** est activée.

    Enfin, récupérez l’élément [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la nouvelle vue que vous utilisez pour afficher la vue plus tard. Une fois encore, cela se produit sur le thread de la nouvelle vue; ainsi [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) obtient l’élément **Id** de la nouvelle vue.

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  Affichez la nouvelle vue en appelant [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101).

    Après avoir créé une vue, vous pouvez l’afficher dans une nouvelle fenêtre en appelant la méthode [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101). Le paramètre *viewId* de cette méthode est un entier qui identifie de manière unique chacune des vues dans votre application. Vous récupérez l’élément [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la vue à l’aide de la propriété **ApplicationView.Id** ou de la méthode [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281109).

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>Vue principale


La première vue créée lors du démarrage de votre application s’appelle la *vue principale*. Cette vue est stockée dans la propriété [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465) et sa propriété [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) a la valeur true. Vous ne créez pas cette vue. Elle est créée par l’application. Le thread de la vue principale agit en tant que gestionnaire de l’application, et tous les évènements d’activation de l’application sont fournis sur ce thread.

Si des vues secondaires sont ouvertes, la fenêtre de la vue principale peut être masquée (par exemple, en cliquant sur le bouton de fermeture (x) dans la barre de titre de la fenêtre), mais son thread reste actif. Appeler [**Close**](https://msdn.microsoft.com/library/windows/apps/br209049) sur la [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) de la vue principale entraîne une **InvalidOperationException**. (Utilisez [**Application.Exit**](https://msdn.microsoft.com/library/windows/apps/br242327) pour fermer votre application.) Si le thread de la vue principale se termine, l’application se ferme.

## <a name="secondary-views"></a>Vues secondaires


Les autres vues, notamment les vues que vous avez créées en appelant [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) dans le code de votre application, sont des vues secondaires. La vue principale et les vues secondaires sont stockées dans la collection [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861). En règle générale, vous créez des vues secondaires en réponse à une action de l’utilisateur. Dans certains cas, le système crée des vues secondaires pour votre application.

> [!NOTE]
> Vous pouvez utiliser la fonctionnalité d’*accès affecté* de Windows pour exécuter une application en [mode plein écran](https://technet.microsoft.com/library/mt219050.aspx). Dans ce cas, le système crée une vue secondaire pour présenter l’interface utilisateur de votre application au-dessus de l’écran de verrouillage. Les vues secondaires créées par l’application ne sont pas autorisées. Ainsi, si vous essayez d’afficher votre propre vue secondaire en mode plein écran, une exception est levée.

## <a name="switch-from-one-view-to-another"></a>Basculer d’une vue à une autre

Vous devez permettre à l’utilisateur de revenir à la fenêtre principale à partir d’une fenêtre secondaire. Pour ce faire, utilisez la méthode [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097). Vous appelez cette méthode à partir du thread de la fenêtre que vous quittez et vous transmettez l’ID de vue de la fenêtre vers laquelle vous basculez.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Lorsque vous utilisez [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097), vous pouvez choisir si vous voulez fermer la fenêtre initiale et la supprimer de la barre des tâches en spécifiant la valeur de [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105).

 

 




