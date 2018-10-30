---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Contrôles de menu volant
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e68f8f48ca9ba67a29c8a52a5d59767a080f642b
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5759545"
---
# <a name="flyouts"></a>Menus volants

Un menu volant est un conteneur d’abandon interactif capable d’afficher l’interface utilisateur arbitraire comme étant son contenu. Les menus volants peuvent contenir d’autres menus volants ou des menus contextuels pour créer une expérience imbriquée.

![Menu contextuel imbriqué dans un menu volant](../images/flyout-nested.png)

> **API importantes**: [classe Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

* N’utilisez pas de menu volant à la place d’une [info-bulle](../tooltips.md) ou d’un [menu contextuel](../menus.md). Utilisez une info-bulle pour afficher une brève description qui disparaît après une durée spécifiée. Utilisez un menu contextuel pour les actions contextuelles liées à un élément de l’interface utilisateur, comme copier et coller.

Pour savoir à quel moment utiliser un menu volant et quand utiliser une boîte de dialogue (un contrôle similaire), consultez [les boîtes de dialogue et menus volants](index.md). 

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

##  <a name="how-to-create-a-flyout"></a>Comment créer un menu volant


Les menus volants sont attachés à des contrôles spécifiques. Vous pouvez utiliser la propriété [Placement](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) pour spécifier l’emplacement où s’affiche le menu volant: Haut, Gauche, Bas, Droite ou Plein. Si vous sélectionnez le [mode de placement Plein](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode), l’application étire le menu volant et le centre dans la fenêtre d’application. Certains contrôles, tels que [Button](/uwp/api/Windows.UI.Xaml.Controls.Button), fournissent une propriété [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) que vous pouvez utiliser pour associer un menu volant ou un [menu contextuel](../menus.md).

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

## <a name="style-a-flyout"></a>Appliquer un style à un menu volant
Pour appliquer un style à un menu volant, modifiez sa propriété [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle). Cet exemple montre un paragraphe d’habillage de texte et rend le bloc de texte accessible à un lecteur d’écran.

![Menu volant accessible avec renvoi à la ligne automatique](../images/flyout-wrapping-text.png)

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

## <a name="styling-flyouts-for-10-foot-experiences"></a>Application de styles aux menus volants pour des expériences «10-foot»

Les contrôles d’abandon interactif comme les menus volants interrompent le focus des claviers et des boîtiers de commande à l’intérieur de leur interface utilisateur temporaire, jusqu’à leur fermeture. Pour fournir une indication visuelle de ce comportement, les contrôles de la Xbox permettant de faire disparaître la luminosité dessinent une superposition qui assombrit l’interface utilisateur hors de portée. Ce comportement peut être modifié à l’aide de la propriété [`LightDismissOverlayMode`](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode). Par défaut, les menus volants dessinent la superposition permettant de faire disparaître la luminosité sur laXbox, mais pas sur d’autres familles d’appareils. Toutefois, les applications peuvent choisir de forcer la superposition afin d’être toujours **activées** ou **désactivées**.

![Menu volant avec superposition estompant l’affichage](../images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

## <a name="light-dismiss-behavior"></a>Comportement d’abandon interactif
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

## <a name="related-articles"></a>Articles connexes
- [Info-bulles](../tooltips.md)
- [Menus et menus contextuels](../menus.md)
- [Classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)