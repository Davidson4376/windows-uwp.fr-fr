---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Boîtes de dialogue et menus volants
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9ceb698bfbe95693ff9d5785b4bea94f1ec3070c
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675376"
---
# <a name="dialogs-and-flyouts"></a>Boîtes de dialogue et menus volants



Les boîtes de dialogue et les menus volants sont des éléments temporaires d’interface utilisateur qui s’affichent quand un événement se produit qui nécessite une notification, une approbation ou d’autres informations de l’utilisateur.

> **API importantes**: [classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [classe Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::row::: :::column::: **Boîtes de dialogue**
        
        ![Example of a dialog](images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::row-end:::


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

* Utilisez les boîtes de dialogue pour notifier les utilisateurs d’informations importantes ou pour demander une confirmation ou des informations supplémentaires avant de pouvoir effectuer une action.
* N’utilisez pas de menu volant à la place d’une [info-bulle](tooltips.md) ou d’un [menu contextuel](menus.md). Utilisez une info-bulle pour afficher une brève description qui disparaît après une durée spécifiée. Utilisez un menu contextuel pour les actions contextuelles liées à un élément de l’interface utilisateur, comme copier et coller.  


Les boîtes de dialogue et les menus volants permettent aux utilisateurs de prendre connaissance d’informations importantes, mais elles perturbent également l’expérience utilisateur. Les boîtes de dialogue étant modales (bloquantes), elles interrompent les utilisateurs et les empêchent de faire autre chose tant qu’ils n’interagissent pas avec la boîte de dialogue. Les menus volants sont moins dérangeants, mais si vous en affichez trop, vous perturbez également l’utilisateur.

Mesurez l’importance des informations à partager: sont-elles suffisamment importantes pour interrompre l’utilisateur? Évaluez également la fréquence à laquelle les informations doivent être affichées. Si vous affichez une boîte de dialogue ou une notification toutes les 5minutes, vous pouvez peut-être plutôt leur allouer un emplacement dans l’interface utilisateur principale. Par exemple, dans un client de chat, au lieu d’afficher un menu volant chaque fois qu’un ami se connecte, vous pouvez afficher la liste des amis en ligne sur le moment et mettre en évidence les amis quand ils se connectent.

Les boîtes de dialogue sont fréquemment utilisées pour confirmer une action (par exemple, la suppression d’un fichier) avant de l’exécuter. Si vous voulez que l’utilisateur effectue souvent une action particulière, fournissez un moyen d’annuler l’action quand il fait une erreur plutôt que de forcer l’utilisateur à confirmer l’action chaque fois.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l'objet <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="dialogs-vs-flyouts"></a>Comparaison des boîtes de dialogue et des menus volants

Une fois que vous avez déterminé que vous voulez utiliser une boîte de dialogue ou un menu volant, vous devez choisir lequel utiliser.

Étant donné que les boîtes de dialogue bloquent les interactions contrairement aux menus volants, elles doivent être réservées aux situations dans lesquelles vous voulez que l’utilisateur interrompe tout ce qu’il est en train de faire pour se concentrer sur une information particulière ou répondre à une question. Les menus volants, quant à eux, peuvent être utilisés quand vous voulez attirer l’attention de l’utilisateur sur quelque chose, mais qu’il a la possibilité de l’ignorer.

:::row::: :::column:::
   <p><b>Utilisez une boîte de dialogue pour...</b> <br/>
<ul>
<li>Afficher des informations importantes que l’utilisateur <b>doit</b> lire et accepter avant de poursuivre. Par exemple:
<ul>
  <li>Utilisez ce type de contrôle pour indiquer à l’utilisateur toute situation d’atteinte possible à la sécurité.</li>
  <li>Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à modifier de manière irrémédiable un élément utile.</li>
  <li>Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à supprimer un élément utile.</li>
  <li>Pour confirmer un achat dans l’application</li>
</ul>

</li>
<li>Les messages d’erreur qui s’appliquent au contexte global de l’application, liés par exemple à une erreur de connectivité.</li>
<li>Utilisez une boîte de dialogue à question pour indiquer que l’application doit poser à l’utilisateur une question bloquante, parce qu’elle ne peut pas choisir telle ou telle option à la place de l’utilisateur, par exemple. Une question bloquante ne peut pas être ignorée ni reportée, et doit offrir à l’utilisateur des options clairement définies.</li>
</ul>
</p>
:::column-end::: :::column::: <p><b>Utilisez un menu volant pour...</b> <br/>
<ul>
<li>Collecter des informations supplémentaires nécessaires pour pouvoir effectuer une action.</li>
<li>Afficher des informations qui ne sont pas pertinentes le reste du temps. Par exemple, dans une application de galerie de photos, quand l’utilisateur clique sur une vignette d’image, vous pouvez utiliser un menu volant pour afficher une version agrandie de l’image.</li>
<li>Affichage d’informations supplémentaires, comme des détails ou des descriptions plus longues sur un élément de la page.</li>
</ul></p>
:::column-end::: :::row-end:::



## <a name="dialogs"></a>Boîtes de dialogue
### <a name="general-guidelines"></a>Recommandations générales

-   Identifiez clairement le problème ou l’objectif de l’utilisateur dans la première ligne du texte de la boîte de dialogue.
-   Le titre de la boîte de dialogue correspond à l’instruction principale. Il est facultatif.
    -   Utilisez un titre court pour expliquer l’utilisation de cette boîte de dialogue.
    -   Si la boîte de dialogue est destinée à indiquer un message, une erreur ou une question simple, vous pouvez ne pas indiquer de titre. Appuyez-vous sur le texte du contenu pour fournir les informations essentielles.
    -   Assurez-vous que le titre est directement lié aux options des boutons.
-   Le contenu de la boîte de dialogue inclut le texte descriptif. Il est requis.
    -   Présentez le message, l’erreur ou la question bloquante aussi simplement que possible.
    -   Si vous indiquez un titre dans la boîte de dialogue, la zone de contenu doit fournir des détails supplémentaires ou clarifier la terminologie utilisée. Ne répétez pas le titre en utilisant une formulation légèrement différente.
-   Au moins un bouton de boîte de dialogue doit apparaître.
    -   Assurez-vous que votre boîte de dialogue dispose d’au moins un bouton correspondant à une action sans échec, non destructrice, par exemple: «D’accord!», «Fermer» ou «Annuler». Utilisez l’API CloseButton pour ajouter ce bouton.
    -   Utilisez des réponses spécifiques au contenu ou à l’instruction principale, sous forme de texte de bouton. Par exemple, utilisez la question « Autorisez-vous NomApplication à accéder à votre emplacement ? », suivie des boutons Autoriser et Bloquer. Les réponses spécifiques peuvent être comprises plus rapidement, ce qui favorise une prise de décision efficace.
    - Veillez à ce que le texte des boutons d’action soit concis. Les chaînes courtes permettent à l’utilisateur de choisir de manière sûre et rapide.
    - En plus de l’action sans échec, non destructrice, vous pouvez éventuellement présenter à l’utilisateur un ou deux boutons d’action liés à l’instruction principale. Ces boutons d’action «faire» confirment le message principal de la boîte de dialogue. Utilisez les API PrimaryButton et SecondaryButton pour ajouter ces actions «faire».
    - Les boutons d’action «faire» doivent s’afficher à l’extrême gauche. L’action sans échec, non destructrice, doit s’afficher à l’extrême droite.
    - Vous pouvez éventuellement choisir de différencier l’un des trois boutons comme bouton par défaut de la boîte de dialogue. Utilisez l’API DefaultButton pour différencier l’un des boutons.  
-   N’utilisez pas de boîtes de dialogue pour les erreurs qui sont liées à un emplacement spécifique de la page, telles que les erreurs de validation (dans les champs de mot de passe, par exemple). Utilisez plutôt le canevas de l’application afin d’afficher les erreurs insérées.
- Utilisez la [classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) pour créer votre expérience de boîte de dialogue. N’utilisez pas l’API MessageDialog déconseillée.

### <a name="dialog-scenarios"></a>Scénarios de boîte de dialogue
Étant donné que les boîtes de dialogue bloquent les interactions des utilisateurs et dans la mesure où les boutons sont le principal mécanisme permettant aux utilisateurs d’ignorer la boîte de dialogue, assurez-vous que votre boîte de dialogue contient au moins un bouton «sans échec» et non destructeur, tel que «Fermer» ou «D’accord!». **Toutes les boîtes de dialogue doivent contenir au moins un bouton d’action sans échec permettant de fermer la boîte de dialogue.** Cela garantit que l’utilisateur peut fermer la boîte de dialogue en toute confiance, sans effectuer d’action.<br>![Une boîte de dialogue à un bouton](images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Lorsque les boîtes de dialogue sont utilisées pour afficher une question bloquante, votre boîte de dialogue doit proposer à l’utilisateur des boutons d’action liés à la question. Le bouton «sans échec» et non destructeur peut s’accompagner d’un ou deux boutons d’action «faire». Lorsque vous proposez plusieurs options à l’utilisateur, assurez-vous que les boutons expliquent clairement les actions sans échec «faire» et «ne pas faire» liées à la question proposée.

![Une boîte de dialogue à deux boutons](images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

Les boîtes de dialogue à trois boutons sont utilisées lorsque vous proposez à l’utilisateur deux actions «faire» et une action «ne pas faire». Les boîtes de dialogue à trois boutons doivent être utilisées avec parcimonie, en distinguant clairement l’action secondaire et l’action sans échec/fermer.

![Une boîte de dialogue à trois boutons](images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
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

### <a name="the-three-dialog-buttons"></a>Les trois boutons de la boîte de dialogue
ContentDialog présente trois différents types de boutons que vous pouvez utiliser pour créer une expérience de boîte de dialogue.

- **CloseButton**- Requis- Représente l’action sans échec, non destructrice, qui permet à l’utilisateur de fermer la boîte de dialogue. S’affiche comme bouton à l’extrême droite.
- **PrimaryButton**- Facultatif- Représente la première action «faire». S’affiche comme bouton à l’extrême gauche.
- **SecondaryButton**- Facultatif- Représente la seconde action «faire». S’affiche comme bouton du milieu.

L’utilisation des boutons intégrés positionne les boutons de manière adéquate. Assurez-vous qu’ils répondent aux événements de clavier, que la zone de commande reste visible même lorsque le clavier visuel clavier est affiché, et qu’ils offrent à la boîte de dialogue une apparence cohérente avec les autres boîtes de dialogue.

#### <a name="closebutton"></a>CloseButton
Chaque boîte de dialogue doit contenir un bouton d’action sans échec, non destructeur, qui permet à l’utilisateur de fermer la boîte de dialogue en toute confiance.

Utilisez l’API ContentDialog.CloseButton pour créer ce bouton. Cela vous permet de créer l’expérience utilisateur adéquate pour toutes les entrées, notamment souris, clavier, tactile et boîtier de commande. Cette expérience se produit dans les cas suivants:
<ol>
    <li>L’utilisateur clique ou appuie sur le CloseButton </li>
    <li>L’utilisateur appuie sur le bouton Précédent du système </li>
    <li>L’utilisateur appuie sur la touche ÉCHAP du clavier </li>
    <li>L’utilisateur appuie sur boutonB du boîtier de commande </li>
</ol>

Quand l’utilisateur clique sur un bouton de la boîte de dialogue, la méthode [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retourne un [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) pour indiquer le bouton sur lequel l’utilisateur clique. Le fait d’appuyer sur le CloseButton renvoie ContentDialogResult.None.

#### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton et SecondaryButton
Outre le CloseButton, vous pouvez éventuellement proposer à l’utilisateur un ou deux boutons d’action liés à l’instruction principale.
Tirez parti de PrimaryButton pour la première action «faire» et SecondaryButton pour la seconde action «faire». Dans les boîtes de dialogue à trois boutons, le PrimaryButton représente généralement l’action «faire» positive, tandis que le SecondaryButton représente généralement une action «faire» neutre ou secondaire.
Par exemple, une application peut inviter l’utilisateur à s’abonner à un service. En tant qu’action «faire» affirmative, le PrimaryButton intégrerait le texte S’abonner, tandis qu’en tant qu’action «faire» neutre, le SecondaryButton intégrerait le texte Essayer. Le CloseButton permettrait à l’utilisateur d’annuler sans effectuer l’une ou l’autre des actions.

Lorsque l’utilisateur clique sur le PrimaryButton, la méthode [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) renvoie ContentDialogResult.Primary.
Lorsque l’utilisateur clique sur le SecondaryButton, la méthode [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) renvoie ContentDialogResult.Secondary.

![Une boîte de dialogue à trois boutons](images/dialogs/dialog_RS2_three_button.png)

#### <a name="defaultbutton"></a>DefaultButton
Vous pouvez éventuellement choisir de différencier l’un des trois boutons comme bouton par défaut. La spécification d’un bouton par défaut déclenche les événements suivants:
- Le bouton reçoit le traitement visuel du bouton d’accent
- Le bouton répond automatiquement à la touche ENTRÉE
    - Lorsque l’utilisateur appuie sur la touche ENTRÉE du clavier, le gestionnaire d’événements de clic associé au bouton par défaut se déclenche et le ContentDialogResult renvoie la valeur associée au bouton par défaut
    - Si l’utilisateur a placé le focus du clavier sur un contrôle qui gère la touche ENTRÉE, le bouton par défaut ne répondra pas aux appuis sur ENTRÉE
- Le bouton recevra automatiquement le focus lors de l’ouverture de la boîte de dialogue, sauf si le contenu de la boîte de dialogue contient une interface utilisateur pouvant être active

Utilisez la propriété ContentDialog.DefaultButton pour spécifier le bouton par défaut. Par défaut, aucun bouton par défaut n’est défini.

![Une boîte de dialogue à trois boutons avec un bouton par défaut](images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="confirmation-dialogs-okcancel"></a>Boîtes de dialogue de confirmation (OK/Annuler)
Une boîte de dialogue de confirmation permet aux utilisateurs de confirmer qu’ils souhaitent effectuer une action. Ils peuvent confirmer l’action ou l’annuler.  
Une boîte de dialogue de confirmation classique comprend deux boutons: un bouton d’affirmation («OK») et un bouton d’annulation.  

<ul>
    <li>
        <p>En règle générale, le bouton d’affirmation doit se trouver sur la gauche (bouton principal) et le bouton Annuler (bouton secondaire) sur la droite.</p>
         ![Boîte de dialogue OK/Annuler](images/dialogs/dialog_RS2_delete_file.png)

    </li>
    <li>Comme indiqué dans la section Recommandations générales, utilisez des boutons dont le texte identifie des réponses spécifiques au contenu ou à l’instruction principale.
    </li>
</ul>

> Certaines plateformes placent le bouton d’affirmation à droite, et non à gauche. Pourquoi est-il conseillé de le placer sur la gauche?  Si vous partez du principe que la plupart des utilisateurs sont droitiers et qu’ils tiennent leur téléphone de la main droite, ils trouveront certainement plus confortable d’appuyer sur le bouton lorsqu’il se trouve à gauche, autrement dit dans le prolongement du pouce. Boutons sur le côté droit de l’écran obligent l’utilisateur à rentrer leur pouce dans une position moins confortable. Les boutons placés à droite de l’écran obligent l’utilisateur à rentrer leur pouce, ce qui représente une position moins confortable.

### <a name="create-a-dialog"></a>Créer une boîte de dialogue
Pour créer une boîte de dialogue, vous utilisez la [classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog). Vous pouvez créer une boîte de dialogue dans le code ou dans le balisage. Bien qu’il soit généralement plus facile de définir des éléments d’interface utilisateur en XAML, dans le cas d’une boîte de dialogue simple, il est plus facile d’utiliser du code normal. Cet exemple crée une boîte de dialogue pour informer l’utilisateur qu’il n’y a pas de connexion Wi-Fi, puis utilise la méthode [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) pour l’afficher.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Quand l’utilisateur clique sur un bouton de la boîte de dialogue, la méthode [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retourne un [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) pour indiquer le bouton sur lequel l’utilisateur clique.

Dans cet exemple, la boîte de dialogue pose une question et utilise le [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) retourné pour déterminer la réponse de l’utilisateur.

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="flyouts"></a>Menus volants
###  <a name="create-a-flyout"></a>Créer un menu volant

Un menu volant est un conteneur d’abandon interactif capable d’afficher l’interface utilisateur arbitraire comme étant son contenu. Les menus volants peuvent contenir d’autres menus volants ou des menus contextuels pour créer une expérience imbriquée.

![Menu contextuel imbriqué dans un menu volant](images/flyout-nested.png)

Les menus volants sont attachés à des contrôles spécifiques. Vous pouvez utiliser la propriété [Placement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) pour spécifier l’emplacement où s’affiche le menu volant: Haut, Gauche, Bas, Droite ou Plein. Si vous sélectionnez le [mode de placement Plein](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode), l’application étire le menu volant et le centre dans la fenêtre d’application. Certains contrôles, tels que [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), fournissent une propriété [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) que vous pouvez utiliser pour associer un menu volant ou un [menu contextuel](menus.md).

Cet exemple crée un menu volant simple qui affiche du texte quand l’utilisateur appuie sur le bouton.
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Si le contrôle n’a pas de propriété Flyout, vous pouvez utiliser à la place la propriété [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) jointe. Dans ce cas, vous devez également appeler la méthode [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) pour afficher le menu volant.

Cet exemple ajoute un menu volant simple à une image. Quand l’utilisateur appuie sur l’image, l’application affiche le menu volant.

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

Les exemples précédents ont défini des menus volants insérés. Vous pouvez également définir un menu volant en tant que ressource statique, puis l’utiliser avec plusieurs éléments. Cet exemple crée un menu volant plus compliqué qui affiche une version agrandie d’une image quand l’utilisateur appuie sur sa vignette.

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

### <a name="style-a-flyout"></a>Appliquer un style à un menu volant
Pour appliquer un style à un menu volant, modifiez sa propriété [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle). Cet exemple montre un paragraphe d’habillage de texte et rend le bloc de texte accessible à un lecteur d’écran.

![Menu volant accessible avec renvoi à la ligne automatique](images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

#### <a name="styling-flyouts-for-10-foot-experience"></a>Application de styles aux menus volants pour une expérience «10-foot»

Les contrôles d’abandon interactif comme les menus volants interrompent le focus des claviers et des boîtiers de commande à l’intérieur de leur interface utilisateur temporaire, jusqu’à leur fermeture. Pour fournir une indication visuelle de ce comportement, les contrôles de la Xbox permettant de faire disparaître la luminosité dessinent une superposition qui assombrit l’interface utilisateur hors de portée. Ce comportement peut être modifié à l’aide de la propriété [`LightDismissOverlayMode`](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode). Par défaut, les menus volants dessinent la superposition permettant de faire disparaître la luminosité sur laXbox, mais pas sur d’autres familles d’appareils. Toutefois, les applications peuvent choisir de forcer la superposition afin d’être toujours **activées** ou **désactivées**.

![Menu volant avec superposition estompant l’affichage](images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

### <a name="light-dismiss-behavior"></a>Comportement d’abandon interactif
Il est possible de fermer les menus volants à l’aide d’une action d’abandon interactif, notamment
-   Appui en dehors du menu volant
-   Appui sur la touche Échap du clavier
-   Appui sur le bouton Précédent du système matériel ou logiciel
-   Appui sur le boutonB du boîtier de commande

Lorsque l’une fermeture à l’aide d’un appui, ce mouvement est généralement absorbé et non transmis à l’interface utilisateur en dessous. Par exemple, si un bouton est visible derrière un menu volant ouvert, le premier appui de l’utilisateur ferme le menu volant mais n’active pas ce bouton. L’appui sur le bouton nécessite un second appui.

Vous pouvez modifier ce comportement en désignant le bouton comme un élément d’entrée directe pour le menu volant. Les actions d’abandon interactif décrites ci-dessus fermeront le menu volant et transmettront également l’événement d’appui pour à son `OverlayInputPassThroughElement` désigné. Envisagez l’adoption de ce comportement pour accélérer les interactions utilisateur sur des éléments similaires du point de vue fonctionnel. Si votre application possède une collection de favoris et que chaque élément de la collection inclut un menu volant joint, il est raisonnable de s’attendre à ce que les utilisateurs souhaitent interagir avec plusieurs menus volants en une succession rapide.

[!NOTE] Veillez à ne pas désigner un élément de superposition avec entrée directe qui se traduit par une action destructrice. Les utilisateurs se sont habitués aux actions d’abandon interactif discrètes, qui n’activent pas l’interface utilisateur principale. Les boutons destructeurs de type Fermer, Supprimer ou similaires ne doivent pas être activés par un abandon interactif, de manière à éviter les comportements inattendus et perturbateurs.

Dans l’exemple suivant, les trois boutons de la FavoritesBar seront activés par le premier appui.

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles associés
- [Info-bulles](tooltips.md)
- [Menus et menus contextuels](menus.md)
- [Classe Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Classe ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
