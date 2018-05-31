---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: Boutons
label: Buttons
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f663ce9da6453922e35f850a8cd039f33770f434
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494166"
---
# <a name="buttons"></a>Boutons


Un bouton permet à l’utilisateur de déclencher une action immédiate.

> **API importantes**: [classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), [classe RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx), [événement Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

![Exemple de boutons](images/controls/button.png)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Un bouton permet à l’utilisateur d’initier une action immédiate, telle que l’envoi d’un formulaire.

N’utilisez pas de bouton lorsque l’action consiste à naviguer vers une autre page; dans ce cas, utilisez plutôt un lien. Voir [Liens hypertexte](hyperlinks.md) pour plus d’informations.
> Exception: pour la navigation dans un assistant, utilisez les boutons Précédent et Suivant. Pour les autres types de navigation vers l’arrière ou vers un niveau supérieur, utilisez un bouton Précédent.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Button">ouvrir l’application et voir le bouton en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Cet exemple utilise deux boutons, Autoriser et Bloquer, dans une boîte de dialogue demandant l’accès à la géolocalisation.

![Exemple de boutons utilisés dans une boîte de dialogue](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Créer un bouton

Cet exemple illustre un bouton qui répond à un clic.

Créez le bouton en XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

Ou créez le bouton dans le code.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Gérez l’événement Click.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>Interaction de bouton

Lorsque vous appuyez sur un bouton avec un doigt ou un stylet, ou lorsque vous cliquez dessus avec le bouton de la souris, le bouton déclenche l’événement [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Si un bouton est sélectionné au clavier, une pression sur la touche Entrée ou sur la barre d’espace déclenche également l’événement Click.

Généralement, vous ne pouvez pas gérer les événements [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) de bas niveau sur un bouton, car un comportement Click lui est affecté à la place. Pour plus d’informations, voir [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx).

Vous pouvez modifier la façon dont un bouton déclenche l’événement Click en modifiant la propriété [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). La valeur ClickMode par défaut est **Release**, mais vous pouvez également définir la propriété ClickMode d'un bouton sur **Hover** ou **Press**. Si ClickMode est défini sur **Hover**, l’événement Click ne peut pas être déclenché avec le clavier ou le mode tactile.


### <a name="button-content"></a>Contenu du bouton

Le bouton est un [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Sa propriété de contenu XAML est [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), ce qui permet une syntaxe comme celle-ci pour XAML : `<Button>A button's content</Button>`. Vous pouvez définir n’importe quel objet comme contenu du bouton. Si le contenu est un objet [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), il est affiché dans le bouton. Si le contenu est un autre type d’objet, sa représentation sous forme de chaîne est affichée dans le bouton.

Le contenu d’un bouton correspond généralement à du texte. Voici des recommandations de conception pour les boutons contenant du texte:
-   Utilisez un texte concis, précis et suffisamment explicite qui décrit clairement l’action effectuée par le bouton. Généralement, le texte d’un bouton est représenté par un seul mot, un verbe.
-   Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.
-   Pour les textes courts, évitez les boutons de commande trop étroits en utilisant une largeur de bouton d'au moins 120px.
- Pour les textes longs, évitez les boutons de commande trop larges en limitant le texte à une longueur maximale de 26caractères.
-   Si le contenu texte du bouton est dynamique (par exemple, s’il est [localisé](../globalizing/globalizing-portal.md)), songez au redimensionnement du bouton et à ses conséquences sur les contrôles environnants.

<table>
<tr>
<td> <b>Vous devez:</b><br> corriger les boutons dont le texte déborde. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Option1:</b><br> Augmentez la largeur du bouton, empilez les boutons et faites un renvoi à la ligne si la longueur du texte dépasse 26caractères. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Option2:</b><br> Augmentez la hauteur du bouton et ajustez le texte avec un renvoi à la ligne. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

Vous pouvez également personnaliser les effets visuels qui constituent l’apparence du bouton. Par exemple, vous pourriez remplacer le texte par une icône, ou utiliser une icône plus du texte.

Ici, un objet **StackPanel** qui contient une image et du texte est défini comme le contenu d’un bouton.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

Le bouton ressemble à ceci.

![Bouton avec contenu d’image et de texte](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Créer un bouton de répétition

Un [RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) est un bouton qui déclenche les événements [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) plusieurs fois à partir du moment où il est enfoncé jusqu’à ce qu’il soit relâché. Définissez la propriété [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) pour spécifier la durée pendant laquelle RepeatButton attend entre le moment où il est actionné et le moment où il commence à répéter l’action de clic. Définissez la propriété [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) pour spécifier la durée entre les répétitions de l’action de clic. Les durées pour les deux propriétés sont spécifiées en millisecondes.

L’exemple suivant montre deux contrôles RepeatButton dont les événements Click respectifs sont utilisés pour augmenter ou réduire la valeur affichée dans un bloc de texte.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="recommendations"></a>Recommandations
-   Assurez-vous que le but et l’état actuel d’un bouton sont clairs pour l’utilisateur.
-   Quand plusieurs boutons permettent de faire le même choix (comme dans une boîte de dialogue de confirmation), présentez les boutons de validation dans cet ordre, où [Faire l’action] et [Ne pas faire l’action] sont des réponses spécifiques à l’instruction principale:
    -   OK/[Faire l’action]/Oui
    -   [Ne pas faire l’action]/Non
    -   Annuler
-   Exposez seulement un ou deux boutons à la fois pour l’utilisateur, par exemple Accepter et Annuler. Si vous devez inviter l’utilisateur à effectuer plusieurs actions, pensez à utiliser des [cases à cocher](checkbox.md) ou des [cases d’option](radio-button.md) à partir desquelles l’utilisateur peut sélectionner des actions, avec un seul bouton de commande pour les déclencher.
-   Pour une action qui doit être disponible sur plusieurs pages dans votre application, pensez à utiliser une [barre d’application inférieure](app-bars.md), au lieu de dupliquer un bouton sur plusieurs pages.

### <a name="recommended-single-button-layout"></a>Disposition recommandée pour un bouton unique

Si votre disposition ne requiert qu’un seul bouton, vous devez l’aligner à gauche ou à droite en fonction du contexte de son conteneur.

-   Les boîtes de dialogue avec un seul bouton doivent **aligner à droite** le bouton. Si votre boîte de dialogue contient un seul bouton, assurez-vous que ce bouton effectue l’action sans échec, non destructrice. Si vous utilisez la classe [ContentDialog](dialogs.md) et spécifiez un seul bouton, celui-ci sera automatiquement aligné à droite.

![Un bouton dans une boîte de dialogue](images/pushbutton_doc_dialog.png)

-   Si votre bouton s’affiche dans un élément d’interface utilisateur conteneur (par exemple, au sein d’une notification toast, un menu volant ou un élément de l’affichage Liste), vous devez **aligner à droite** le bouton dans le conteneur.

![Un bouton dans un conteneur](images/pushbutton_doc_container.png)

-   Dans les pages qui contiennent un bouton unique (par exemple, un bouton «Appliquer» en bas d’une page de paramètres), vous devez **aligner à gauche** le bouton. Cela garantit que le bouton s’aligne avec le reste du contenu de la page.

![Un bouton sur une page](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Boutons Précédent

Le bouton Précédent est un élément de l’interface utilisateur fournie par le système qui permet de revenir en arrière dans la pile Back ou dans l’historique de navigation de l’utilisateur. Vous n’avez pas besoin de créer votre propre bouton précédent, mais vous devez peut-être effectuer certaines opérations pour obtenir une bonne expérience de navigation vers l’arrière. Pour plus d’informations, voir [Navigation dans l’historique et navigation vers l’arrière](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.


## <a name="related-articles"></a>Articles connexes
- [Classe Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Cases d’option](radio-button.md)
- [Cases à cocher](checkbox.md)
- [Boutons bascule](toggles.md)
