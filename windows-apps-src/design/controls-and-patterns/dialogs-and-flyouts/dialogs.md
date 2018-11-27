---
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Contrôles de boîte de dialogue
label: Dialogs
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 520f4bdd72c51cd1508c9e655107ae909f6e4243
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7709867"
---
## <a name="dialog-controls"></a>Contrôles de boîte de dialogue

Les contrôles de boîte de dialogue sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles d’application. Ils bloquent les interactions avec la fenêtre d’application jusqu'à ce que masquées explicitement. Elles exigent souvent une forme d’action de la part de l’utilisateur.

![Exemple de boîte de dialogue](../images/dialogs/dialog_RS2_delete_file.png)


> **API importantes**: [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez les boîtes de dialogue pour notifier les utilisateurs d’informations importantes ou pour demander une confirmation ou des informations supplémentaires avant de pouvoir effectuer une action.

Pour obtenir des recommandations sur le moment d’utiliser une boîte de dialogue ou quand vous devez utiliser un menu volant (un contrôle similaire), voir les [boîtes de dialogue et menus volants](index.md). 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l'objet <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="general-guidelines"></a>Recommandations générales

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
- Utilisez la [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) pour créer votre expérience de boîte de dialogue. N’utilisez pas l’API MessageDialog déconseillée.

## <a name="how-to-create-a-dialog"></a>Comment créer une boîte de dialogue
Pour créer une boîte de dialogue, vous utilisez la [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog). Vous pouvez créer une boîte de dialogue dans le code ou dans le balisage. Bien qu’il soit généralement plus facile de définir des éléments d’interface utilisateur en XAML, dans le cas d’une boîte de dialogue simple, il est plus facile d’utiliser du code normal. Cet exemple crée une boîte de dialogue pour informer l’utilisateur qu’il n’y a pas de connexion Wi-Fi, puis utilise la méthode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) pour l’afficher.

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

Quand l’utilisateur clique sur un bouton de la boîte de dialogue, la méthode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retourne un [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) pour indiquer le bouton sur lequel l’utilisateur clique.

Dans cet exemple, la boîte de dialogue pose une question et utilise le [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) retourné pour déterminer la réponse de l’utilisateur.

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

## <a name="provide-a-safe-action"></a>Fournir une action sans échec
Étant donné que les boîtes de dialogue bloquent les interactions des utilisateurs et dans la mesure où les boutons sont le principal mécanisme permettant aux utilisateurs d’ignorer la boîte de dialogue, assurez-vous que votre boîte de dialogue contient au moins un bouton «sans échec» et non destructeur, tel que «Fermer» ou «D’accord!». **Toutes les boîtes de dialogue doivent contenir au moins un bouton d’action sans échec permettant de fermer la boîte de dialogue.** Cela garantit que l’utilisateur peut fermer la boîte de dialogue en toute confiance, sans effectuer d’action.<br>![Une boîte de dialogue à un bouton](../images/dialogs/dialog_RS2_one_button.png)

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

![Une boîte de dialogue à deux boutons](../images/dialogs/dialog_RS2_two_button.png)

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

![Une boîte de dialogue à trois boutons](../images/dialogs/dialog_RS2_three_button.png)

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

## <a name="the-three-dialog-buttons"></a>Les trois boutons de la boîte de dialogue
ContentDialog présente trois différents types de boutons que vous pouvez utiliser pour créer une expérience de boîte de dialogue.

- **CloseButton**- Requis- Représente l’action sans échec, non destructrice, qui permet à l’utilisateur de fermer la boîte de dialogue. S’affiche comme bouton à l’extrême droite.
- **PrimaryButton**- Facultatif- Représente la première action «faire». S’affiche comme bouton à l’extrême gauche.
- **SecondaryButton**- Facultatif- Représente la seconde action «faire». S’affiche comme bouton du milieu.

L’utilisation des boutons intégrés positionne les boutons de manière adéquate. Assurez-vous qu’ils répondent aux événements de clavier, que la zone de commande reste visible même lorsque le clavier visuel clavier est affiché, et qu’ils offrent à la boîte de dialogue une apparence cohérente avec les autres boîtes de dialogue.

### <a name="closebutton"></a>CloseButton
Chaque boîte de dialogue doit contenir un bouton d’action sans échec, non destructeur, qui permet à l’utilisateur de fermer la boîte de dialogue en toute confiance.

Utilisez l’API ContentDialog.CloseButton pour créer ce bouton. Cela vous permet de créer l’expérience utilisateur adéquate pour toutes les entrées, notamment souris, clavier, tactile et boîtier de commande. Cette expérience se produit dans les cas suivants:
<ol>
    <li>L’utilisateur clique ou appuie sur le CloseButton </li>
    <li>L’utilisateur appuie sur le bouton Précédent du système </li>
    <li>L’utilisateur appuie sur la touche ÉCHAP du clavier </li>
    <li>L’utilisateur appuie sur boutonB du boîtier de commande </li>
</ol>

Quand l’utilisateur clique sur un bouton de la boîte de dialogue, la méthode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) retourne un [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) pour indiquer le bouton sur lequel l’utilisateur clique. Le fait d’appuyer sur le CloseButton renvoie ContentDialogResult.None.

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton et SecondaryButton
Outre le CloseButton, vous pouvez éventuellement proposer à l’utilisateur un ou deux boutons d’action liés à l’instruction principale.
Tirez parti de PrimaryButton pour la première action «faire» et SecondaryButton pour la seconde action «faire». Dans les boîtes de dialogue à trois boutons, le PrimaryButton représente généralement l’action «faire» positive, tandis que le SecondaryButton représente généralement une action «faire» neutre ou secondaire.
Par exemple, une application peut inviter l’utilisateur à s’abonner à un service. En tant qu’action «faire» affirmative, le PrimaryButton intégrerait le texte S’abonner, tandis qu’en tant qu’action «faire» neutre, le SecondaryButton intégrerait le texte Essayer. Le CloseButton permettrait à l’utilisateur d’annuler sans effectuer l’une ou l’autre des actions.

Lorsque l’utilisateur clique sur le PrimaryButton, la méthode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) renvoie ContentDialogResult.Primary.
Lorsque l’utilisateur clique sur le SecondaryButton, la méthode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) renvoie ContentDialogResult.Secondary.

![Une boîte de dialogue à trois boutons](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
Vous pouvez éventuellement choisir de différencier l’un des trois boutons comme bouton par défaut. La spécification d’un bouton par défaut déclenche les événements suivants:
- Le bouton reçoit le traitement visuel du bouton d’accent
- Le bouton répond automatiquement à la touche ENTRÉE
    - Lorsque l’utilisateur appuie sur la touche ENTRÉE du clavier, le gestionnaire d’événements de clic associé au bouton par défaut se déclenche et le ContentDialogResult renvoie la valeur associée au bouton par défaut
    - Si l’utilisateur a placé le focus du clavier sur un contrôle qui gère la touche ENTRÉE, le bouton par défaut ne répondra pas aux appuis sur ENTRÉE
- Le bouton recevra automatiquement le focus lors de l’ouverture de la boîte de dialogue, sauf si le contenu de la boîte de dialogue contient une interface utilisateur pouvant être active

Utilisez la propriété ContentDialog.DefaultButton pour spécifier le bouton par défaut. Par défaut, aucun bouton par défaut n’est défini.

![Une boîte de dialogue à trois boutons avec un bouton par défaut](../images/dialogs/dialog_RS2_three_button_default.png)

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

## <a name="confirmation-dialogs-okcancel"></a>Boîtes de dialogue de confirmation (OK/Annuler)
Une boîte de dialogue de confirmation permet aux utilisateurs de confirmer qu’ils souhaitent effectuer une action. Ils peuvent confirmer l’action ou l’annuler.  
Une boîte de dialogue de confirmation classique comprend deux boutons: un bouton d’affirmation («OK») et un bouton d’annulation.  

<ul>
    <li>
        <p>En règle générale, le bouton d’affirmation doit se trouver sur la gauche (bouton principal) et le bouton Annuler (bouton secondaire) sur la droite.</p>
        <img alt="An OK/cancel dialog" src="../images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>Comme indiqué dans la section Recommandations générales, utilisez des boutons dont le texte identifie des réponses spécifiques au contenu ou à l’instruction principale.
    </li>
</ul>

> Certaines plateformes placent le bouton d’affirmation à droite, et non à gauche. Pourquoi est-il conseillé de le placer sur la gauche?  Si vous partez du principe que la plupart des utilisateurs sont droitiers et qu’ils tiennent leur téléphone de la main droite, ils trouveront certainement plus confortable d’appuyer sur le bouton lorsqu’il se trouve à gauche, autrement dit dans le prolongement du pouce. Boutons sur le côté droit de l’écran obligent l’utilisateur à rentrer leur pouce dans une position moins confortable. Les boutons placés à droite de l’écran obligent l’utilisateur à rentrer leur pouce, ce qui représente une position moins confortable.





## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes
- [Info-bulles](../tooltips.md)
- [Menus et menus contextuels](../menus.md)
- [Classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
