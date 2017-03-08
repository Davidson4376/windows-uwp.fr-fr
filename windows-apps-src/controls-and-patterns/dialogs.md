---
author: mijacobs
Description: "Les boîtes de dialogue et les menus volants affichent des éléments temporaires d’interface utilisateur quand l’utilisateur les sollicite ou quand un événement nécessite une notification ou une approbation."
title: "Boîtes de dialogue et menus volants"
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: e76ae1e85f1512a939f2b7ee50ed205c0c55605b
ms.lasthandoff: 02/08/2017

---
# <a name="dialogs-and-flyouts"></a>Boîtes de dialogue et menus volants

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Les boîtes de dialogue et les menus volants sont des éléments temporaires d’interface utilisateur qui s’affichent quand un événement se produit qui nécessite une notification, une approbation ou d’autres informations de la part de l’utilisateur.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[Classe ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)</li>
<li>[Classe Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)</li>
</ul>
</div>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Boîtes de dialogue</b> <br/><br/>
    ![Exemple de boîte de dialogue](images/dialogs/dialog-delete-file-example.png)</p>
<p>Les boîtes de dialogue sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles sur l’application. Les boîtes de dialogue bloquent les interactions avec la fenêtre de l’application jusqu’à ce qu’elles soient masquées explicitement. Elles exigent souvent une forme d’action de la part de l’utilisateur.   
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>Menus volants</b> <br/><br/>
   ![Exemple de menu volant](images/flyout-example.png)</p>
<p>Un menu volant est une fenêtre contextuelle légère qui affiche l’interface utilisateur liée aux opérations qu’effectue l’utilisateur. Il comprend une logique de placement et de dimensionnement, et peut être utilisé pour afficher un contrôle masqué, des détails supplémentaires sur un élément, ou pour demander à l’utilisateur de confirmer une action. 
</p><p>Contrairement à une boîte de dialogue, un menu volant peut être fermé rapidement en appuyant ou en cliquant en dehors du menu volant, en appuyant sur la touche ÉCHAP ou le bouton Précédent, en redimensionnant la fenêtre d’application ou en modifiant l’orientation de l’appareil.
</p><br/>

  </div>
</div>
</div>

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

* Utilisez les boîtes de dialogue et les menus volants pour notifier les utilisateurs d’informations importantes ou pour demander une confirmation ou des informations supplémentaires avant de pouvoir effectuer une action. 
* N’utilisez pas de menu volant à la place d’une [info-bulle](tooltips.md) ou d’un [menu contextuel](menus.md). Utilisez une info-bulle pour afficher une brève description qui disparaît après une durée spécifiée. Utilisez un menu contextuel pour les actions contextuelles liées à un élément de l’interface utilisateur, comme copier et coller.  


Les boîtes de dialogue et les menus volants permettent aux utilisateurs de prendre connaissance d’informations importantes, mais elles perturbent également l’expérience utilisateur. Les boîtes de dialogue étant modales (bloquantes), elles interrompent les utilisateurs et les empêchent de faire autre chose tant qu’ils n’interagissent pas avec la boîte de dialogue. Les menus volants sont moins dérangeants, mais si vous en affichez trop, vous perturbez également l’utilisateur. 

Mesurez l’importance des informations à partager : sont-elles suffisamment importantes pour interrompre l’utilisateur ? Évaluez également la fréquence à laquelle les informations doivent être affichées. Si vous affichez une boîte de dialogue ou une notification toutes les 5 minutes, vous pouvez peut-être plutôt leur allouer un emplacement dans l’interface utilisateur principale. Par exemple, dans un client de chat, au lieu d’afficher un menu volant chaque fois qu’un ami se connecte, vous pouvez afficher la liste des amis en ligne sur le moment et mettre en évidence les amis quand ils se connectent. 

Les menus volants et les boîtes de dialogue sont fréquemment utilisés pour confirmer une action (par exemple, la suppression d’un fichier) avant de l’exécuter. Si vous voulez que l’utilisateur effectue souvent une action particulière, fournissez un moyen d’annuler l’action quand il fait une erreur plutôt que de forcer l’utilisateur à confirmer l’action chaque fois. 



## <a name="dialogs-vs-flyouts"></a>Comparaison des boîtes de dialogue et des menus volants

Une fois que vous avez déterminé que vous voulez utiliser une boîte de dialogue ou un menu volant, vous devez choisir lequel utiliser. 

Étant donné que les boîtes de dialogue bloquent les interactions contrairement aux menus volants, elles doivent être réservées aux situations dans lesquelles vous voulez que l’utilisateur interrompe tout ce qu’il est en train de faire pour se concentrer sur une information particulière ou répondre à une question. Les menus volants, quant à eux, peuvent être utilisés quand vous voulez attirer l’attention de l’utilisateur sur quelque chose, mais qu’il a la possibilité de l’ignorer. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Utilisez une boîte de dialogue pour...</b> <br/>
<ul>
<li>Afficher des informations importantes que l’utilisateur **doit** lire et accepter avant de poursuivre. Par exemple :
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
  </div>
  <div class="side-by-side-content-right">
   <p><b>Utilisez un menu volant pour...</b> <br/>
<ul>
<li>Collecter des informations supplémentaires nécessaires pour pouvoir effectuer une action.</li>
<li>Afficher des informations qui ne sont pas pertinentes le reste du temps. Par exemple, dans une application de galerie de photos, quand l’utilisateur clique sur une vignette d’image, vous pouvez utiliser un menu volant pour afficher une version agrandie de l’image.</li>
<li>Afficher des avertissements et des confirmations, notamment ceux qui sont liés à des actions potentiellement destructrices.</li>
<li>Affichage d’informations supplémentaires, comme des détails ou des descriptions plus longues sur un élément de la page.</li>
</ul></p>
  </div>
</div>
</div>

<div class="microsoft-internal-note">
Les contrôles permettant de faire disparaître les contrôles interceptent le focus du clavier et du boîtier de commande à l’intérieur de l’interface utilisateur temporaire jusqu’à le faire disparaître. Pour fournir une indication visuelle de ce comportement, les contrôles de la Xbox permettant de faire disparaître la luminosité dessinent une superposition qui assombrit l’interface utilisateur hors de portée. Ce comportement peut être modifié à l’aide de la nouvelle propriété `LightDismissOverlayMode`. Par défaut, les interfaces utilisateur temporaires dessinent la superposition permettant de faire disparaître la luminosité sur la Xbox, mais pas sur d’autres familles d’appareils. Toutefois, les applications peuvent choisir de forcer la superposition afin d’être toujours **activées** ou **désactivées**.

```xaml
<MenuFlyout LightDismissOverlayMode=\"Off\">
```
</div>

## <a name="dialogs"></a>Boîtes de dialogue
### <a name="general-guidelines"></a>Recommandations générales

-   Identifiez clairement le problème ou l’objectif de l’utilisateur dans la première ligne du texte de la boîte de dialogue.
-   Le titre de la boîte de dialogue correspond à l’instruction principale. Il est facultatif.
    -   Utilisez un titre court pour expliquer l’utilisation de cette boîte de dialogue. Les titres longs ne sont pas renvoyés à la ligne et sont tronqués.
    -   Si la boîte de dialogue est destinée à indiquer un message, une erreur ou une question simple, vous pouvez ne pas indiquer de titre. Appuyez-vous sur le texte du contenu pour fournir les informations essentielles.
    -   Assurez-vous que le titre est directement lié aux options des boutons.
-   Le contenu de la boîte de dialogue inclut le texte descriptif. Il est requis.
    -   Présentez le message, l’erreur ou la question bloquante aussi simplement que possible.
    -   Si vous indiquez un titre dans la boîte de dialogue, la zone de contenu doit fournir des détails supplémentaires ou clarifier la terminologie utilisée. Ne répétez pas le titre en utilisant une formulation légèrement différente.
-   Au moins un bouton de boîte de dialogue doit apparaître.
    -   Les boutons sont le seul mécanisme permettant aux utilisateurs d’abandonner la boîte de dialogue.
    -   Utilisez des boutons dont le texte identifie des réponses spécifiques au contenu ou à l’instruction principale. Par exemple, utilisez la question « Autorisez-vous NomApplication à accéder à votre emplacement ? », suivie des boutons Autoriser et Bloquer. Les réponses spécifiques peuvent être comprises plus rapidement, ce qui favorise une prise de décision efficace.
    - Présentez les boutons de validation dans cet ordre : 
        -   OK/[Faire l’action]/Oui
        -   [Ne pas faire l’action]/Non
        -   Annuler
        
        (où [Faire l’action] et [Ne pas faire l’action] sont des réponses spécifiques à l’instruction principale.)
   
-   Les boîtes de dialogue d’erreur incluent un message d’erreur, ainsi que les informations pertinentes. Le seul bouton utilisé dans une boîte de dialogue d’erreur doit être du type « Fermer », ou similaire.
-   N’utilisez pas de boîtes de dialogue pour les erreurs qui sont liées à un emplacement spécifique de la page, telles que les erreurs de validation (dans les champs de mot de passe, par exemple). Utilisez plutôt le canevas de l’application afin d’afficher les erreurs insérées.

### <a name="confirmation-dialogs-okcancel"></a>Boîtes de dialogue de confirmation (OK/Annuler)
Une boîte de dialogue de confirmation permet aux utilisateurs de confirmer qu’ils souhaitent effectuer une action. Ils peuvent confirmer l’action ou l’annuler.  
Une boîte de dialogue de confirmation classique comprend deux boutons : un bouton d’affirmation (« OK ») et un bouton d’annulation.  

<ul>
    <li>
        <p>En règle générale, le bouton d’affirmation doit se trouver sur la gauche (bouton principal) et le bouton Annuler (bouton secondaire) sur la droite.</p>
         ![Boîte de dialogue OK/Annuler](images/dialogs/dialog-delete-file-example.png)
        
    </li>
    <li>Comme indiqué dans la section Recommandations générales, utilisez des boutons dont le texte identifie des réponses spécifiques au contenu ou à l’instruction principale.
    </li>
</ul>

> Certaines plateformes placent le bouton d’affirmation à droite, et non à gauche. Pourquoi est-il conseillé de le placer sur la gauche ?  Si vous partons du principe que la plupart des utilisateurs sont droitiers et qu’ils tiennent leur téléphone de la main droite, ils trouveront certainement plus confortable d’appuyer sur le bouton lorsqu’il se trouve à gauche, autrement dit dans le prolongement du pouce. Les boutons placés à droite de l’écran obligent l’utilisateur à rentrer leur pouce, ce qui représente une position moins confortable.

### <a name="create-a-dialog"></a>Créer une boîte de dialogue
Pour créer une boîte de dialogue, vous utilisez la [classe ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx). Vous pouvez créer une boîte de dialogue dans le code ou dans le balisage. Bien qu’il soit généralement plus facile de définir des éléments d’interface utilisateur en XAML, dans le cas d’une boîte de dialogue simple, il est plus facile d’utiliser du code normal. Cet exemple crée une boîte de dialogue pour informer l’utilisateur qu’il n’y a pas de connexion Wi-Fi, puis utilise la méthode [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) pour l’afficher.

```csharp
private async void displayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog()
    {
        Title = "No wifi connection",
        Content = "Check connection and try again",
        PrimaryButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Quand l’utilisateur clique sur un bouton de la boîte de dialogue, la méthode [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) retourne un [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) pour indiquer le bouton sur lequel l’utilisateur clique. 

Dans cet exemple, la boîte de dialogue pose une question et utilise le [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) retourné pour déterminer la réponse de l’utilisateur. 

```csharp
private async void displayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog()
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        SecondaryButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();
    
    // Delete the file if the user clicked the primary button. 
    /// Otherwise, do nothing. 
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file. 
    }
}
```

## <a name="flyouts"></a>Menus volants
###  <a name="create-a-flyout"></a>Créer un menu volant

Un menu volant est un conteneur ouvert qui peut afficher l’interface utilisateur arbitraire comme étant son contenu. 

<div class="microsoft-internal-note">
Cela comprend les menus volants et les menus contextuels, qui peuvent être imbriqués dans d’autres menus volants.
</div>

Les menus volants sont attachés à des contrôles spécifiques. Vous pouvez utiliser la propriété [Placement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.placement.aspx) pour spécifier l’emplacement où s’affiche le menu volant : Haut, Gauche, Bas, Droite ou Plein. Si vous sélectionnez le [mode de placement Plein](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutplacementmode.aspx), l’application étire le menu volant et le centre dans la fenêtre d’application. Quand ils sont visibles, les menus volants doivent être ancrés à l’objet appelant et spécifier leur position relative préférée par rapport à l’objet : Haut, Gauche, Bas ou Droite. Le menu volant dispose également d’un mode de placement complet qui tente d’étirer le menu volant et de le centrer à l’intérieur de la fenêtre d’application. Certains contrôles, tels que [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), fournissent une propriété [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) que vous pouvez utiliser pour associer un menu volant. 

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

Si le contrôle n’a pas de propriété Flyout, vous pouvez utiliser à la place la propriété [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) jointe. Dans ce cas, vous devez également appeler la méthode [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) pour afficher le menu volant. 

Cet exemple ajoute un menu volant simple à une image. Quand l’utilisateur appuie sur l’image, l’application affiche le menu volant. 

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50" 
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock TextWrapping="Wrap" Text="This is some text in a flyout."  />
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
        <Flyout.FlyoutPresenterStyle>
            <Style TargetType="FlyoutPresenter">
                <Setter Property="ScrollViewer.ZoomMode" Value="Enabled"/>
                <Setter Property="Background" Value="Black"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="BorderThickness" Value="5"/>
                <Setter Property="MinHeight" Value="300"/>
                <Setter Property="MinWidth" Value="300"/>
            </Style>
        </Flyout.FlyoutPresenterStyle>
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
Pour appliquer un style à un menu volant, modifiez sa propriété [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx). Cet exemple montre un paragraphe d’habillage de texte et rend le bloc de texte accessible à un lecteur d’écran.

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

## <a name="get-the-samples"></a>Obtenir les exemples
*   [Informations de base sur l’interface utilisateur XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes
- [Info-bulles](tooltips.md)
- [Menus et menus contextuels](menus.md)
- [**Classe Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**Classe ContentDialog**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)

