---
author: QuinnRadich
Description: Learn how to enable peer-to-peer navigation between two basic pages in an Universal Windows Platform (UWP) app.
title: Navigation pair à pair entre deuxpages
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.author: quradic
ms.date: 07/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 02b2c0d24c3f963025774c87c0c0a1c588c8a249
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4122826"
---
# <a name="implement-navigation-between-two-pages"></a>Implémenter la navigation entre deux pages

Découvrez comment utiliser une trame et les pages pour activer une navigation pair à pair de base dans votre application. 

> **API importantes**: classe [**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682), classe [**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503), [Espace de noms **Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)

![Navigation pair à pair](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. Créer une application vide

1.  Dans le menu MicrosoftVisualStudio, choisissez **Fichier** > **Nouveau projet**.
2.  Dans le volet gauche de la boîte de dialogue **Nouveau projet**, choisissez le nœud **Visual C#** > ** Windows** > **Universel** ou **Visual C++** > **Windows** > **Universel**.
3.  Dans le volet central, choisissez **Application vide**.
4.  Dans le champ **Nom**, entrez **NavApp1**, puis choisissez le bouton **OK**.
    La solution est créée et les fichiers projet apparaissent dans l’**Explorateur de solutions**.
5.  Pour exécuter le programme, choisissez **Déboguer** > **Démarrer le débogage** dans le menu, ou appuyez sur F5.
    Une page vide s’affiche.
6.  Pour arrêter le débogage et revenir à VisualStudio, quittez l’application ou cliquez sur **Arrêter le débogage** dans le menu.

## <a name="2-add-basic-pages"></a>2. Ajouter des pages de base

Ajoutez ensuite deux pages au projet.

1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de projet **BlankApp** pour ouvrir le menu contextuel.
2.  Choisissez **Ajouter** > **Nouvel élément** dans le menu contextuel.
3.  Dans la boîte de dialogue **Ajouter un nouvel élément**, choisissez **Page vide** dans le volet du milieu.
4.  Dans le champ **Nom**, entrez **Page1** (ou **Page2**), puis appuyez sur le bouton **Ajouter**.
5. Répétez les étapes1 à 4 pour ajouter la deuxième page.

Ces fichiers doivent maintenant être répertoriés comme faisant partie de votre projet NavApp1.

<table>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h

</li>
</ul></td>
</tr>
</tbody>
</table>

Ajoutez le contenu suivant à l’interface utilisateur de Page1.xaml.

-   Ajoutez un élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) nommé `pageTitle` en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Modifiez la valeur de la propriété [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) en la définissant sur `Page 1`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Ajoutez l’élément [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) suivant en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) et après l’élément `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Ajoutez le code suivant dans le fichier code-behind Page1.xaml pour gérer l’événement `Click` de l’élément [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que vous avez ajouté pour accéder à Page2.xaml.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>());
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

Ajoutez le contenu suivant à l’interface utilisateur de Page2.xaml.

-   Ajoutez un élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) nommé `pageTitle` en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Modifiez la valeur de la propriété [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) en la définissant sur `Page 2`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Ajoutez l’élément [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) suivant en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) et après l’élément `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

Ajoutez le code suivant au fichier code-behind Page2.xaml pour gérer l’événement `Click` de l’élément [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) pour naviguer vers Page1.xaml.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

```cppwinrt
void Page2::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page1>());
}
```

```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

> [!NOTE]
> Pour les projetsC++, vous devez ajouter une directive `#include` dans le fichier d’en-tête de chaque page faisant référence à une autre page. Pour l’exemple de navigation entre les pages présenté ici, le fichier page1.xaml.h contient `#include "Page2.xaml.h"`, et à son tour, page2.xaml.h contient `#include "Page1.xaml.h"`.

Maintenant que nous avons préparé les pages, nous devons faire en sorte que Page1.xaml s’affiche au démarrage de l’application.

Ouvrez le fichier code-behind app.xaml et modifiez le gestionnaire `OnLaunched`.

Ici, nous spécifions `Page1` dans l’appel à [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) au lieu de `MainPage`.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
 
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();
        rootFrame.NavigationFailed += OnNavigationFailed;
 
        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }
 
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
 
    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = Frame();

        rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
        {
            // Restore the saved session state only when appropriate, scheduling the
            // final launch steps after the restore is complete
        }

        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Place the frame in the current Window
            Window::Current().Content(rootFrame);
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
    else
    {
        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = ref new Frame();

        rootFrame->NavigationFailed += 
            ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
                this, &App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }
        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;
    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

> [!NOTE]
> Le code ici utilise la valeur de retour de [**Naviguer**](https://msdn.microsoft.com/library/windows/apps/br242694) pour lever une exception d’application en cas d’échec de la navigation sur le cadre de la fenêtre initiale de l’application. Quand **Navigate** retourne **true**, la navigation a lieu.

À présent, générez et exécutez l’application. Cliquez sur le lien «Click to go to page2». La deuxième page indiquant «Page2» en haut doit être chargée et affichée dans la trame.

### <a name="about-the-frame-and-page-classes"></a>À propos des classes Trame et Page

Avant d’ajouter d’autres fonctionnalités à notre application, examinons la façon dont les pages que nous venons d’ajouter prennent en charge la navigation dans l’application.

Tout d’abord, un [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682), appelé `rootFrame`, est créé pour l’application dans la méthode `App.OnLaunched` du fichier code-behind App.xaml. La classe **Frame** prend en charge diverses méthodes de navigation telles que [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) et [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), ainsi que les propriétés telles que [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) et [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995).
 
La méthode [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) est utilisée pour afficher le contenu dans ce **Frame**. Par défaut, cette méthode charge MainPage.xaml. Dans notre exemple, `Page1` est transmis à la méthode **Navigate**, qui charge alors `Page1` dans le **Frame**. 

`Page1` est une sous-classe de la classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503). La classe **Page** possède une propriété en lecture seule **Frame** qui obtient l’objet **Frame** contenant l’objet **Page**. Lorsque le gestionnaire d’événements **Click** de l’objet **HyperlinkButton** de `Page1` appelle `this.Frame.Navigate(typeof(Page2))`, l’objet **Frame** affiche le contenu de Page2.xaml.

Pour finir, chaque fois qu’une page est chargée dans la trame, cette page est ajoutée comme une [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) à la [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) ou [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) de la [**trame**](https://msdn.microsoft.com/library/windows/apps/br227504). L’[historique et la navigation vers l’arrière](navigation-history-and-backwards-navigation.md) sont ainsi possibles.

## <a name="3-pass-information-between-pages"></a>3. Passer des informations entre les pages

Notre application navigue entre deux pages, mais elle n’effectue pour le moment rien d’intéressant. Souvent, lorsqu’une application possède plusieurs pages, celles-ci doivent partager des informations. Passons des informations de la première page à la deuxième page.

Dans Page1.xaml, remplacez l’objet **HyperlinkButton** que vous avez ajouté précédemment par l’objet [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) suivant.

Ici, nous ajoutons une étiquette [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) et un objet [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (`name`) pour entrer une chaîne de texte.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Dans le gestionnaire d’événements `HyperlinkButton_Click` du fichier code-behind Page1.xaml, ajoutez un paramètre faisant référence à la propriété `Text` de `name` **TextBox** à la méthode `Navigate`.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>(), winrt::box_value(name().Text()));
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

Dans Page2.xaml, remplacez l’objet **HyperlinkButton** que vous avez ajouté précédemment par l’objet **StackPanel** suivant.

Ici, nous ajoutons un [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) pour afficher la chaîne de texte transmise à partir de Page1.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

Dans le fichier code-behind Page2.xaml, remplacez la méthode `OnNavigatedTo` par les éléments suivants:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string && !string.IsNullOrWhiteSpace((string)e.Parameter))
    {
        greeting.Text = $"Hi, {e.Parameter.ToString()}";
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

```cppwinrt
void Page2::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto propertyValue{ e.Parameter().as<Windows::Foundation::IPropertyValue>() };
    if (propertyValue.Type() == Windows::Foundation::PropertyType::String)
    {
        greeting().Text(L"Hi, " + winrt::unbox_value<winrt::hstring>(e.Parameter()));
    }
    else
    {
        greeting().Text(L"Hi!");
    }
    __super::OnNavigatedTo(e);
}
```

```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi, " + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

Exécutez l’application, tapez votre nom dans la zone de texte, puis cliquez sur le lien indiquant **Click to go to page 2**. 

Lorsque l’événement **Click** de l’objet **HyperlinkButton** de `Page1` appelle `this.Frame.Navigate(typeof(Page2), name.Text)`, la propriété `name.Text` est transmise à `Page2`, et la valeur des données d’événement est utilisée pour le message affiché sur la page.

## <a name="4-cache-a-page"></a>4. Mettre en cache une page

Le contenu et l’état de la page ne sont pas mis en cache par défaut. Pour le faire, vous devez les activer dans chaque page de votre application.

Dans notre exemple pair à pair de base, il n’existe aucun bouton Précédent (nous expliquons la navigation vers l’arrière dans [Navigation vers l’arrière](navigation-history-and-backwards-navigation.md), mais si vous avez cliqué sur un bouton Précédent dans `Page2`, l’objet **TextBox** (et tout autre champ) de `Page1` serait défini à son état par défaut. Un moyen de contourner ce problème consiste à utiliser la propriété [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) pour spécifier l’ajout d’une page dans le cache de la page du cadre. 

Dans le constructeur de `Page1`, vous pouvez définir **NavigationCacheMode** sur **Enabled** pour conserver toutes les valeurs de contenu et d’état de la page jusqu’à ce que le cache de pages de la trame soit saturé. Définissez [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) sur [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284) si vous voulez ignorer les limites [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683), qui spécifient le nombre de pages de l’historique de navigation qui peuvent être mises en cache pour la trame. Toutefois, n’oubliez pas que la taille limite du cache peut être cruciale, selon les limites de mémoire d’un appareil.

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

```cppwinrt
Page1::Page1()
{
    InitializeComponent();
    NavigationCacheMode(Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled);
}
```

```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## <a name="related-articles"></a>Articles associés
* [Informations de base relatives à la conception de la navigation pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [Recommandations relatives aux onglets et aux sélecteurs de vue](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [Recommandations en matière de volets de navigation](https://msdn.microsoft.com/library/windows/apps/dn997766)
