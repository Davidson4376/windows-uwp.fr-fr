---
Description: Découvrez comment naviguer dans une application de base à deux pages pair à pair de la plateforme Windows universelle (UWP).
title: Navigation pair à pair entre deux pages
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Navigation pair à pair entre deux pages
template: detail.hbs
---

# <span id="dev_navigation.peer-to-peer_navigation_between_two_pages"> </span>Navigation pair à pair entre deux pages


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Découvrez comment naviguer dans une application de base à deux pages pair à pair de la plateforme Windows universelle (UWP).

![Exemple de navigation pair à pair sur deux pages](images/nav-peertopeer-2page.png)


**API importantes**

-   [**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)
-   [**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503)
-   [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)


## <span id="Create_the_blank_app"> </span> <span id="create_the_blank_app"> </span> <span id="CREATE_THE_BLANK_APP"> </span>Créer l’application vide


1.  Dans le menu Microsoft Visual Studio, choisissez **Fichier &gt; Nouveau Projet**.
2.  Dans le volet gauche de la boîte de dialogue **Nouveau projet**, choisissez le nœud **Visual C# -&gt; Windows -&gt; Universel** ou **Visual C++ -&gt; Windows -&gt; Universel**.
3.  Dans le volet central, choisissez **Application vide**.
4.  Dans le champ **Nom**, entrez **NavApp1**, puis choisissez le bouton **OK**.

    La solution est créée et les fichiers du projet apparaissent dans l’**Explorateur de solutions**.

    **Important** Lorsque vous exécutez Visual Studio pour la première fois, vous êtes invité à vous procurer une licence de développeur. Pour plus d’informations, voir [Activer votre appareil pour le développement](https://msdn.microsoft.com/library/windows/apps/dn706236).

     

5.  Pour exécuter le programme, choisissez **Déboguer** &gt; **Démarrer le débogage** dans le menu, ou appuyez sur F5.

    Une page vide s’affiche.

6.  Appuyez sur Maj+F5 pour interrompre le débogage et revenir à Visual Studio.

## <span id="Add_basic_pages"> </span> <span id="add_basic_pages"> </span> <span id="ADD_BASIC_PAGES"> </span>Ajouter des pages de base


Ensuite, ajoutez deux pages de contenu au projet.

Effectuez les étapes suivantes deux fois de suite pour ajouter les deux pages qui vont vous permettre d’effectuer la navigation.

1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de projet **BlankApp** pour ouvrir le menu contextuel.
2.  Choisissez **Ajouter** &gt; **Nouvel élément** dans le menu contextuel.
3.  Dans la boîte de dialogue **Ajouter un nouvel élément**, choisissez **Page vide** dans le volet du milieu.
4.  Dans le champ **Nom**, entrez **Page1** (ou **Page2**), puis appuyez sur le bouton **Ajouter**.

Ces fichiers doivent maintenant être répertoriés comme faisant partie de votre projet NavApp1.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">CS</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td align="left"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h
<div class="alert">
<strong>Remarque</strong><p>Les fonctions sont déclarées dans le fichier d’en-tête (.h) et implémentées dans le fichier code-behind (.cpp).</p>
</div>
<div>
 
</div></li>
</ul></td>
</tr>
</tbody>
</table>

 

Ajoutez le contenu suivant à l’interface utilisateur de Page1.xaml.

-   Ajoutez un élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) nommé `pageTitle` en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Modifiez la valeur de la propriété [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) en la définissant sur `Page 1`.

```    XAML
<TextBlock x:Name="pageTitle" Text="Page 1" /></code></pre></td>
    </tr>
    </tbody>
    </table>
```

-   Ajoutez l’élément [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) suivant en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) et après l’élément `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).

    <span codelanguage="XAML"></span>
```    XAML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XAML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<HyperlinkButton HorizontalAlignment="Center" Content="Click to go to page 2" Click="HyperlinkButton_Click"/></code></pre></td>
    </tr>
    </tbody>
    </table>
```

Ajoutez le code suivant à la classe `Page1` dans le fichier code-behind Page1.xaml pour gérer l’événement `Click` du [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que vous avez ajouté précédemment. Ici, nous naviguons vers Page2.xaml.

<span codelanguage="ManagedCPlusPlus"></span>
```ManagedCPlusPlus
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
void Page1::HyperlinkButton_Click_nodata(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

```CSharp
private void HyperlinkButton_Click_nodata(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

Apportez les modifications suivantes à l’interface utilisateur de Page2.xaml.

-   Ajoutez un élément [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) nommé `pageTitle` en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Modifiez la valeur de la propriété [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) en la définissant sur `Page 2`.

```    XAML
<TextBlock x:Name="pageTitle" Text="Page 2" /></code></pre></td>
    </tr>
    </tbody>
    </table>
```

-   Ajoutez l’élément [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) suivant en tant qu’élément enfant de la racine [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) et après l’élément `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).

    <span codelanguage="XAML"></span>
```    XAML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XAML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<HyperlinkButton HorizontalAlignment="Center" Content="Click to go to page 1" Click="HyperlinkButton_Click"/></code></pre></td>
    </tr>
    </tbody>
    </table>
```

Ajoutez le code suivant à la classe `Page2` dans le fichier code-behind Page2.xaml pour gérer l’événement `Click` du [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que vous avez ajouté précédemment. Ici, nous naviguons vers Page1.xaml.

**Remarque**  
Pour les projets C++, vous devez ajouter une directive `#include` dans le fichier d’en-tête de chaque page faisant référence à une autre page. Pour l’exemple de navigation entre les pages présenté ici, le fichier page1.xaml.h contient `#include "Page2.xaml.h"`, et à son tour, page2.xaml.h contient `#include "Page1.xaml.h"`.

 

<span codelanguage="ManagedCPlusPlus"></span>
```ManagedCPlusPlus
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

```CSharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

Maintenant que nous avons préparé les pages de contenu, nous devons faire en sorte que Page1.xaml s’affiche au démarrage de l’application.

Ouvrez le fichier code-behind app.xaml et modifiez le gestionnaire `OnLaunched`.

Ici, nous spécifions `Page1` dans l’appel à [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) au lieu de `MainPage`.

```ManagedCPlusPlus
/// <summary>
/// Invoked when the application is launched normally by the end user. 
/// Other entry points will be used in specific cases, such as when the 
/// application is launched to open a specific file.
/// </summary>
/// <param name="e">Details about the launch request and process.</param>
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
                this, &amp;App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }

        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;

    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn&#39;t restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

```CSharp
/// <summary>
/// Invoked when the application is launched normally by the end user. 
/// Other entry points will be used in specific cases, such as when the 
/// application is launched to open a specific file.
/// </summary>
/// <param name="e">Details about the launch request and process.</param>
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
        // When the navigation stack isn&#39;t restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Remarque** Le code utilise ici la valeur de retour de [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) pour lever une exception d’application en cas d’échec de la navigation vers la fenêtre initiale de l’application. Quand **Navigate** retourne **true**, la navigation a lieu.

 

À présent, générez et exécutez l’application. Cliquez sur le lien « Click to go to page 2 ». La deuxième page indiquant « Page 2 » en haut doit être chargée et affichée dans le cadre.

## <span id="Frame_and_Page_classes"> </span> <span id="frame_and_page_classes"> </span> <span id="FRAME_AND_PAGE_CLASSES"> </span>Classes Frame et Page


Avant d’ajouter d’autres fonctionnalités à notre application, examinons la façon dont les pages que nous venons d’ajouter prennent en charge la navigation dans l’application.

Tout d’abord, un [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) (`rootFrame`) est créé pour l’application dans la méthode `App.OnLaunched` du fichier code-behind App.xaml. La méthode [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) est utilisée pour afficher le contenu dans ce **Frame**.

**Remarque**  
La classe [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) prend en charge diverses méthodes de navigation telles que [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) et [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), et les propriétés telles que [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) et [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995).

 

Dans notre exemple, `Page1` est passé à la méthode [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694). Cette méthode définit le contenu de la fenêtre active de l’application à [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) et charge le contenu de la page que vous spécifiez dans le **Frame** (Page1.xaml dans notre exemple ou MainPage.xaml par défaut).

`Page1` est une sous-classe de la classe [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503). La classe **Page** possède une propriété en lecture seule [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) qui obtient l’objet [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) contenant l’objet **Page**. Lorsque le gestionnaire d’événements [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) de l’objet [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) appelle ` Frame.Navigate(typeof(Page2))`, l’objet **Frame** dans la fenêtre de l’application affiche le contenu de Page2.xaml.

Chaque fois qu’une page est chargée dans le cadre, cette page est ajoutée comme une [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) à la [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) ou [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) de la [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504).

## <span id="Pass_information_between_pages"> </span> <span id="pass_information_between_pages"> </span> <span id="PASS_INFORMATION_BETWEEN_PAGES"> </span>Passer des informations entre les pages


Notre application navigue entre deux pages, mais elle n’effectue pour le moment rien d’intéressant. Souvent, lorsqu’une application possède plusieurs pages, celles-ci doivent partager des informations. Passons des informations de la première page à la deuxième page.

Dans Page1.xaml, remplacez l’objet [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que vous avez ajouté précédemment par l’objet [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) suivant.

Ici, nous ajoutons une étiquette [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) et un objet [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (`name`) pour entrer une chaîne de texte.

```XAML
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton HorizontalAlignment="Center" Content="Click to go to page 2" Click="HyperlinkButton_Click"/>
</StackPanel>
```

Dans le gestionnaire d’événements `HyperlinkButton_Click` du fichier code-behind Page1.xaml, ajoutez un paramètre faisant référence à la propriété `Text` de `name` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) à la méthode `Navigate`.

```ManagedCPlusPlus
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

```CSharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

Dans le fichier code-behind Page2.xaml, remplacez la méthode `OnNavigatedTo` par les éléments suivants :

```ManagedCPlusPlus
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi," + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

```CSharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string)
    {
        greeting.Text = "Hi, " + e.Parameter.ToString();
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

Exécutez l’application, tapez votre nom dans la zone de texte, puis cliquez sur le lien indiquant **Click to go to page 2**. Lorsque vous avez appelé `this.Frame.Navigate(typeof(Page2), tb1.Text)` dans l’événement [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) de [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739), la propriété `name.Text` a été passée à `Page2` et la valeur de données d’événement est utilisée pour le message affiché sur la page.

## <span id="Cache_a__page"> </span> <span id="cache_a__page"> </span> <span id="CACHE_A__PAGE"> </span>Mettre en cache une page


Le contenu et l’état de la page ne sont pas mis en cache par défaut, vous devez l’activer dans chaque page de votre application.

Dans notre exemple pair à pair de base, il n’existe aucun bouton Précédent (nous expliquons la navigation vers l’arrière dans [Navigation via le bouton Précédent](navigation-history-and-backwards-navigation.md)), mais si vous aviez cliqué sur un bouton Précédent dans `Page2`, le [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (et tout autre champ) sur `Page1` serait défini à son état par défaut. Un moyen de contourner ce problème consiste à utiliser la propriété [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) pour spécifier l’ajout d’une page dans le cache de la page du cadre.

Dans le constructeur de `Page1`, attribuez à [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) la valeur [**Enabled**](https://msdn.microsoft.com/library/windows/apps/br243284). Cela permet de conserver toutes les valeurs de contenu et d’état de la page jusqu’à ce que le cache des pages du cadre soit dépassé.

Attribuez à [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) la valeur [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284) si vous souhaitez ignorer la taille limite du cache du cadre. Toutefois, la taille limite du cache peut être cruciale, selon les limites de mémoire d’un appareil.

**Remarque** La propriété [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) spécifie le nombre de pages dans l’historique de navigation qui peuvent être mises en cache pour le cadre.

 

```ManagedCPlusPlus
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

```CSharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

## <span id="related_topics"> </span>Articles connexes

* [Informations de base relatives à la conception de la navigation pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [Recommandations relatives aux onglets et aux sélecteurs de vue](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [Recommandations en matière de volets de navigation](https://msdn.microsoft.com/library/windows/apps/dn997766)
 

 






<!--HONumber=Mar16_HO1-->


