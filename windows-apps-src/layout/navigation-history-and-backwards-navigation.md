---
author: mijacobs
Description: "La navigation dans les applications de plateforme Windows universelle (UWP) est basée sur un modèle flexible de structures et d’éléments de navigation, et de fonctionnalités au niveau du système."
title: "Informations de base en matière de conception de la navigation pour les applications de plateforme Windows universelle (UWP)"
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 75e8c342775f7d6c564cb1014519f8e4707a0632
ms.openlocfilehash: f18fc0806313cc1656860b0fd8b5ae692fa3d4c6

---

#  Navigation dans l’historique et navigation vers l’arrière

Sur le web, les sites web individuels fournissent leurs propres systèmes de navigation, tels que des tables des matières, des boutons, des menus, des listes simples de liens, etc. L’expérience de navigation peut varier sensiblement d’un site web à l’autre. Il existe toutefois une expérience de navigation cohérente : le bouton Précédent. La plupart des navigateurs fournissent un bouton Précédent qui a le même comportement, quel que soit le site web consulté.

Pour des raisons similaires, la plateforme Windows universelle (UWP) fournit un système de navigation cohérent pour la fonctionnalité Précédent afin que l’utilisateur puisse parcourir l’historique de navigation dans une application et, en fonction de l’appareil, d’une application à l’autre.

L’interface utilisateur pour le bouton Précédent du système est optimisée pour chaque type de facteur de forme et d’appareil d’entrée, mais l’expérience de navigation est globale et cohérente sur les différents appareils et applications UWP.

Voici les facteurs de forme principaux avec l’interface utilisateur du bouton Précédent :


<table>
    <tr>
        <td colspan="2">Appareils</td>
        <td>Comportement du Bouton Précédent</td>
     </tr>
    <tr>
        <td>Téléphone</td>
        <td>![système de retour sur un téléphone](images/back-systemback-phone.png)</td>
        <td>
        <ul>
<li>Toujours présent.</li>
<li>Bouton logiciel ou matériel en bas de l’appareil.</li>
<li>Navigation vers l’arrière globale au sein de l’application et entre les applications.</li>
</ul>
</td>
     </tr>
     <tr>
        <td>Tablette</td>
        <td>![système de retour sur une tablette (en mode tablette)](images/back-systemback-tablet.png)</td>
        <td>
<ul>
<li>Toujours présent en mode tablette.

    Not available in Desktop mode. Title bar back button can be enabled, instead. See [PC, Laptop, Tablet](#PC).

    Users can switch between running in Tablet mode and Desktop mode by going to **Settings &gt; System &gt; Tablet mode** and setting **Make Windows more touch-friendly when using your device as a tablet**.</li>

<li> Bouton logiciel dans la barre de navigation en bas de l’appareil.</li>
<li>Navigation vers l’arrière globale au sein de l’application et entre les applications.</li></ul>        
        </td>
     </tr>
    <tr>
        <td>PC, ordinateur portable, tablette</td>
        <td>![système de retour sur un pc ou un ordinateur portable](images/back-systemback-pc.png)</td>
        <td>
<ul>
<li>Facultatif en mode Bureau.

    Not available in Tablet mode. See [Tablet](#Tablet).

    Disabled by default. Must opt in to enable it.

    Users can switch between running in Tablet mode and Desktop mode by going to **Settings &gt; System &gt; Tablet mode** and setting **Make Windows more touch-friendly when using your device as a tablet**.</li>

<li>Bouton logiciel dans la barre de titre de l’application.</li>
<li>Navigation vers l’arrière au sein de l’application uniquement. Ne prend pas en charge la navigation entre les applications.</li></ul>        
        </td>
     </tr>
    <tr>
        <td>Surface Hub</td>
        <td>![système de retour sur un surface hub](images/nav/nav-back-surfacehub.png)</td>
        <td>
<ul>
<li>Facultatif.</li>
<li>Désactivé par défaut. Choisir pour l’activer.</li>
<li>Bouton logiciel dans la barre de titre de l’application.</li>
<li>Navigation vers l’arrière au sein de l’application uniquement. Ne prend pas en charge la navigation entre les applications.</li></ul>        
        </td>
     </tr>     
<table>


Voici d’autres types d’entrée qui ne font pas appel à une interface utilisateur de bouton Précédent, mais qui néanmoins fournissent exactement les mêmes fonctionnalités.


<table>
<tr><td colspan="3">Appareils d’entrée</td></tr>
<tr><td>Clavier</td><td>![clavier](images/keyboard-wireframe.png)</td><td>Touche Windows + Retour arrière</td></tr>
<tr><td>Cortana</td><td>![fonctions vocales](images/speech-wireframe.png)</td><td>Dites «Hey Cortana, reviens en arrière»</td></tr>
</table>
 

Lorsque votre application est exécutée sur un téléphone, une tablette, un PC ou un ordinateur portable sur lequel un retour système est activé, le système informe votre application lorsque l’utilisateur appuie sur le bouton Précédent. Lorsqu’il appuie sur le bouton Précédent, l’utilisateur s’attend à accéder à l’emplacement précédent dans l’historique de navigation de l’application. C’est à vous qu’il incombe de décider des actions de navigation à ajouter à l’historique de navigation et de la réponse à un appui sur le bouton Précédent.


## Comment activer la prise en charge de la navigation vers l’arrière du système


Les applications doivent activer la navigation vers l’arrière pour tous les boutons Précédent des systèmes matériels et logiciels. Pour ce faire, vous devez inscrire un écouteur pour l’événement [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) et définir un gestionnaire correspondant.

Ici, nous inscrivons un écouteur global pour l’événement [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) dans le fichier code-behind App.xaml. Vous pouvez vous inscrire pour cet événement dans chaque page si vous souhaitez exclure des pages spécifiques de la navigation vers l’arrière, ou si vous souhaitez exécuter du code de niveau page avant d’afficher la page.

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += 
    App_BackRequested;
```
```cpp
Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->
    BackRequested += ref new Windows::Foundation::EventHandler<
    Windows::UI::Core::BackRequestedEventArgs^>(
        this, &amp;App::App_BackRequested);
```

Voici le gestionnaire d’événements [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) correspondant qui appelle [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) dans l’image racine de l'application.

Ce gestionnaire est appelé sur un événement arrière global. Si la pile Back dans l’application est vide, le système peut naviguer jusqu’à l’application précédente dans la pile de l’application ou jusqu’à l’écran de démarrage. Il n’existe aucune pile Back in-app en mode bureau, et l’utilisateur reste dans l’application même lorsque la pile Back de l’application est épuisée.

> [!div class="tabbedCodeSnippets"]
```csharp
>private void App_BackRequested(object sender, 
>    Windows.UI.Core.BackRequestedEventArgs e)
>{
>    Frame rootFrame = Window.Current.Content as Frame;
>    if (rootFrame == null)
>        return;
>
>    // Navigate back if possible, and if the event has not 
>    // already been handled .
>    if (rootFrame.CanGoBack &amp;&amp; e.Handled == false)
>    {
>        e.Handled = true;
>        rootFrame.GoBack();
>    }
>}
```
```cpp
>void App::App_BackRequested(
>    Platform::Object^ sender, 
>    Windows::UI::Core::BackRequestedEventArgs^ e)
>{
>    Frame^ rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
>    if (rootFrame == nullptr)
>        return;
>
>    // Navigate back if possible, and if the event has not
>    // already been handled.
>    if (rootFrame->CanGoBack && e->Handled == false)
>    {
>        e->Handled = true;
>        rootFrame->GoBack();
>    }
>}
```

## Comment activer le bouton Précédent de la barre de titre


Les appareils qui prennent en charge le mode bureau (en règle générale, les PC et ordinateurs portables, mais également certaines tablettes) et sur lesquels le paramètre est activé (**Paramètres &gt; Système &gt; Mode tablette**) ne fournissent pas de barre de navigation globale avec le bouton Précédent du système.

En mode bureau, chaque application s’exécute dans une fenêtre avec une barre de titre. Vous pouvez fournir un autre bouton Précédent pour votre application qui est affiché dans cette barre de titre.

Le bouton Précédent de la barre de titre est uniquement disponible dans les applications qui s’exécutent sur des appareils en mode bureau et ne prend en charge que l’historique de navigation dans l’application. Il ne prend pas en charge l’historique de navigation entre applications.

**Important** Le bouton Précédent de la barre de titre n’est pas affiché par défaut. Vous devez le sélectionner.

 

|                                                             |                                                        |
|-------------------------------------------------------------|--------------------------------------------------------|
| ![Aucun retour système en mode bureau](images/nav-noback-pc.png) | ![Retour système en mode bureau](images/nav-back-pc.png) |
| Mode bureau, aucune navigation vers l’arrière.                           | Mode bureau, navigation vers l’arrière activée.                 |

 

Remplacez l’événement [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) et attribuez à [**AppViewBackButtonVisibility**](https://msdn.microsoft.com/library/windows/apps/dn986448) la valeur [**Visible**](https://msdn.microsoft.com/library/windows/apps/dn986276) dans le fichier code-behind pour chaque page pour laquelle vous souhaitez activer le bouton Précédent de la barre de titre.

Pour cet exemple, nous répertorions chaque page de la pile Back et nous activons le bouton Précédent si la propriété [**CanGoBack**](https://msdn.microsoft.com/library/windows/apps/br242685) de l’image a la valeur **true**.

> [!div class="tabbedCodeSnippets"]
>```csharp
>protected override void OnNavigatedTo(NavigationEventArgs e)
>{
>    Frame rootFrame = Window.Current.Content as Frame;
>
>    string myPages = "";
>    foreach (PageStackEntry page in rootFrame.BackStack)
>    {
>        myPages += page.SourcePageType.ToString() + "\n";
>    }
>    stackCount.Text = myPages;
>
>    if (rootFrame.CanGoBack)
>    {
>        // Show UI in title bar if opted-in and in-app backstack is not empty.
>        SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
>            AppViewBackButtonVisibility.Visible;
>    }
>    else
>    {
>        // Remove the UI from the title bar if in-app back stack is empty.
>        SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
>            AppViewBackButtonVisibility.Collapsed;
>    }
>}
>```
>```cpp
>void StartPage::OnNavigatedTo(NavigationEventArgs^ e)
>{
>    auto rootFrame = dynamic_cast<Windows::UI::Xaml::Controls::Frame^>(Window::Current->Content);
>
>    Platform::String^ myPages = "";
>
>    if (rootFrame == nullptr)
>        return;
>
>    for each (PageStackEntry^ page in rootFrame->BackStack)
>    {
>        myPages += page->SourcePageType.ToString() + "\n";
>    }
>    stackCount->Text = myPages;
>
>    if (rootFrame->CanGoBack)
>    {
>        // If we have pages in our in-app backstack and have opted in to showing back, do so
>        Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->AppViewBackButtonVisibility =
>            Windows::UI::Core::AppViewBackButtonVisibility::Visible;
>    }
>    else
>    {
>        // Remove the UI from the title bar if there are no pages in our in-app back stack
>        Windows::UI::Core::SystemNavigationManager::GetForCurrentView()->AppViewBackButtonVisibility =
>            Windows::UI::Core::AppViewBackButtonVisibility::Collapsed;
>    }
>}
>```


### Recommandations sur le comportement personnalisé de navigation vers l’arrière

Si vous choisissez de fournir votre propre navigation de pile Back, l’expérience doit être cohérente avec les autres applications. Nous vous recommandons de suivre les modèles d’actions de navigation suivants :

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Action de navigation</th>
<th align="left">Ajouter à l’historique de navigation ?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Page à page, différents groupes d’homologues</strong></p></td>
<td align="left"><strong>Oui</strong>
<p>Dans cette illustration, l’utilisateur navigue du niveau1 de l’application vers le niveau2, en traversant des groupes d’homologues. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deux groupes d’homologues du même niveau. La navigation est donc ajoutée à l’historique de navigation.</p>
<p><img src="images/nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Page à page, même groupe d’homologues, pas d’élément de navigation à l’écran</strong></p>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Aucun élément de navigation (par exemple les onglets/tableaux croisés dynamiques ou un volet de navigation ancré) fournissant une navigation directe vers les deux pages n’est toujours présent.</p></td>
<td align="left"><strong>Oui</strong>
<p>Dans l’illustration suivante, l’utilisateur navigue entre deuxpages dans le même groupe d’homologues. Comme les pages n’utilisent pas d’onglets ni de volet de navigation ancré, la navigation est ajoutée à l’historique de navigation.</p>
<p><img src="images/nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>Page à page, même groupe d’homologues, avec un élément de navigation à l’écran</strong></p>
<p>L’utilisateur navigue d’une page à une autre dans le même groupe d’homologues. Les deux pages sont affichées dans le même élément de navigation. Par exemple, les deuxpages utilisent le même élément onglets/tableaux croisés dynamiques, ou elles s’affichent dans un volet de navigation ancré.</p></td>
<td align="left"><strong>Non</strong>
<p>Lorsque l’utilisateur appuie sur le bouton Précédent, il retourne à la dernière page avant d’avoir accédé au groupe d’homologues actuel.</p>
<p><img src="images/nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td align="left"><strong>Afficher une interface utilisateur temporaire</strong>
<p>L’application affiche une fenêtre indépendante ou une fenêtre enfant, comme une boîte de dialogue, un écran de démarrage ou un clavier visuel. Elle passe également en mode spécial, comme le mode de sélection multiple.</p></td>
<td align="left"><strong>Non</strong>
<p>Lorsque l’utilisateur appuie sur le bouton Précédent, il ignore l’interface utilisateur temporaire (masque le clavier visuel, annule la boîte de dialogue, etc.) et retourne dans la page qui a généré l’interface utilisateur temporaire.</p>
<p><img src="images/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td align="left"><strong>Énumérer les éléments</strong>
<p>L’application affiche le contenu d’un élément visuel, tel que les détails de l’élément sélectionné dans la liste des éléments maîtres/détails.</p></td>
<td align="left"><strong>Non</strong>
<p>L’énumération des éléments est semblable à la navigation au sein d’un groupe d’homologues. Lorsque l’utilisateur appuie sur le bouton Précédent, il navigue vers la page qui a précédé la page actuelle qui comporte l’énumération d’éléments.</p>
<img src="images/nav/nav-enumerate.png" alt="Iterm enumeration" /></td>
</tr>
</tbody>
</table>


### Reprise

Quand l’utilisateur bascule vers une autre application et retourne dans votre application, nous recommandons un retour à la dernière page de l’historique de navigation.


## Obtenir les exemples
*   [Exemple de bouton Précédent](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackButton)<br/>
    Montre comment configurer un gestionnaire d’événements pour l’événement de bouton Précédent et comment activer le bouton Précédent de la barre de titre quand l’application est en mode Bureau avec fenêtres.

## Articles connexes
* [Notions de base sur la navigation](navigation-basics.md)

 







<!--HONumber=Aug16_HO3-->


