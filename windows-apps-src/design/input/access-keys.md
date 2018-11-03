---
author: Karl-Bridge-Microsoft
Description: Learn how to improve both the usability and the accessibility of your UWP app by providing an intuitive way for users to quickly navigate and interact with an app's visible UI through a keyboard instead of a pointer device (such as touch or mouse).
title: Recommandations en matière de conception de touches d’accès rapide
label: Access keys design guidelines
keywords: clavier, touche d’accès rapide, touche d’accès, accessibilité, navigation, focus, texte, entrée, interaction utilisateur
template: detail.hbs
ms.author: kbridge
ms.date: 06/08/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 844e5d8e9442192d75f2aac07a7a2f82dd0196f3
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5994797"
---
# <a name="access-keys"></a>Touches d’accès rapide

Les touches d’accès rapide sont des raccourcis clavier qui améliorent à la fois l’utilisation et l’accessibilité de vos applications Windows en introduisant une méthode intuitive permettant aux utilisateurs de naviguer et d’interagir rapidement avec l’interface utilisateur visible d’une application, par le biais du clavier plutôt qu’à l’aide d’un périphérique de pointage (comme un pointeur tactile ou une souris).

Consultez la rubrique [Touches de raccourci](keyboard-accelerators.md) pour plus d’informations sur la façon d’appeler les actions courantes dans une application Windows à l’aide de raccourcis clavier. 

> [!NOTE]
> Un clavier est indispensable pour les utilisateurs qui souffrent d’un handicap (voir [Accessibilité du clavier](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)). C’est aussi un outil important pour les utilisateurs qui préfèrent se servir d’un clavier, voyant cet outil comme un moyen d’interaction avec une application efficace.

La plateforme Windows universelle (UWP) fournit des supports intégrés à travers les commandes de plateforme, à la fois pour les touches d’accès rapide d’un clavier et pour les commentaires d’interface utilisateur associés, par le biais de signaux visuels que l’on appelle des touches d’accès.

## <a name="overview"></a>Vue d’ensemble

Une touche d’accès rapide est une combinaison de la touche Alt et d’une ou plusieurs touches alphanumériques, que l’on appelle parfois *mnémonique*. En général, on appuie successivement plutôt que simultanément sur ces touches.

Les touches d’accès sont des badges affichés en regard des commandes qui prennent en charge les touches d’accès rapide lorsque l’utilisateur appuie sur la touche Alt. Chaque touche d’accès contient les touches alphanumériques qui activent la commande associée.

> [!NOTE]
> Les raccourcis claviers sont automatiquement pris en charge pour les touches d’accès rapide avec un seul caractère alphanumérique. Par exemple, appuyer simultanément sur Alt+F dans Word ouvre le menu Fichier sans afficher les touches d’accès.

Appuyer sur la touche Alt initialise la fonctionnalité de la touche d’accès rapide et affiche toutes les combinaisons actuellement disponibles dans les touches d’accès. Les frappes suivantes sont gérées par l’infrastructure de touche d’accès rapide, qui rejette les touches non valides jusqu’à ce que l’utilisateur appuie sur une touche d’accès rapide, sur Entrée, Échap, Tab, ou sur des touches de direction afin de désactiver les touches d’accès rapide et renvoyer la gestion de la touche vers l’application.

Les applications MicrosoftOffice offrent une prise en charge étendue des touches d’accès rapide. L’image suivante montre l’onglet Accueil de Word avec les touches d’accès rapide activées (notez la prise en charge pour les nombres et les frappes multiples).

![Badges KeyTip pour les touches d’accès rapide dans MicrosoftWord](images/accesskeys/keytip-badges-word.png)

_Badges KeyTip pour les touches d’accès rapide dans MicrosoftWord_

Pour ajouter une touche d’accès rapide à une commande, utilisez la **propriété AccessKey**. La valeur de cette propriété spécifie la séquence de touches d’accès rapide, le raccourci (si un seul caractère alphanumérique) et la touche d’accès.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Quand utiliser les touches d’accès rapide

Nous vous recommandons de spécifier les touches d’accès rapide dans votre interface utilisateur lorsque cela est approprié et de prendre en charge les touches d’accès rapide dans toutes les commandes personnalisées.

1.  **Les touches d’accès rapide rendent votre application plus accessible** aux utilisateurs souffrant de troubles psychomoteurs, y compris aux utilisateurs qui ne peuvent pas appuyer simultanément sur les touches, ou qui ont des difficultés à utiliser une souris.

    Une interface utilisateur de clavier bien conçue représente un aspect important de l’accessibilité logicielle. Elle permet aux utilisateurs malvoyants ou souffrant d’un handicap moteur de naviguer dans une application et d’interagir avec ses fonctionnalités. Les utilisateurs qui ne sont pas en mesure d’utiliser une souris peuvent avoir recours à diverses technologies d’assistance, telles que les outils de clavier amélioré, les claviers visuels, les écrans élargis, les lecteurs d’écran et les utilitaires d’entrée vocale. Pour ces utilisateurs, une couverture complète de la commande est essentielle.

2.  **Les touches d’accès rapide rendent votre application plus facile d’utilisation** pour les utilisateurs avancés qui préfèrent interagir par le biais d’un clavier.

    Les utilisateurs expérimentés ont souvent une préférence marquée pour l’utilisation du clavier, car les commandes clavier peuvent être entrées plus rapidement et ne nécessitent pas de retirer les mains du clavier. Pour ces utilisateurs, l’efficacité et la cohérence sont essentielles. L’exhaustivité n’est importante que pour les commandes les plus fréquemment utilisées.

## <a name="set-access-key-scope"></a>Configurer l’étendue de la touche d’accès rapide

Lorsqu’il existe de nombreux éléments sur l’écran qui prennent en charge les touches d’accès rapide, nous vous recommandons d’étendre les touches d’accès rapide afin de réduire la **charge cognitive**. Cela permet de réduire le nombre de touches d’accès rapide à l’écran, ce qui les rend plus faciles à trouver et permet de gagner en efficacité et en productivité.

Par exemple, MicrosoftWord fournit deux étendues de clé d’accès: une portée principale pour les onglets du ruban et une étendue secondaire pour les commandes de l’onglet sélectionné.

Les images suivantes présentent les deux étendues des touches d’accès rapide dans Word. Le premier exemple montre les touches d’accès rapide principales qui permettent à un utilisateur de sélectionner un onglet et d’autres commandes de niveau supérieur. Le second exemple présente les touches d’accès rapide secondaire pour l’onglet Accueil.

![Touches d'accès rapides principales dans MicrosoftWord](images/accesskeys/primary-access-keys-word.png)
_Touches d’accès rapides principales dans MicrosoftWord_

![Touches d'accès rapide secondaires dans MicrosoftWord](images/accesskeys/secondary-access-keys-word.png)
_Touches d'accès rapide secondaires dans MicrosoftWord_

Les touches d’accès rapide peuvent être dupliquées pour les éléments dans des étendues différentes. Dans l’exemple précédent, «2» est la touche d’accès rapide pour l’option Annuler dans l’étendue principale et pour l’option «Italique» dans l’étendue secondaire.

Nous montrons ici comment définir une étendue de touche d’accès.

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![Touches d’accès rapide principales pour CommandBar](images/accesskeys/primary-access-keys-commandbar.png)

_Étendue principale CommandBar et touches d’accès rapide prises en charge_

![Touches d’accès rapide secondaires pour CommandBar](images/accesskeys/secondary-access-keys-commandbar.png)

_Étendue secondaire CommandBar et touches d’accès rapide prises en charge_

### <a name="windows-10-creators-update-and-older"></a>Windows10CreatorsUpdate et versions antérieures

Avant Windows10 Fall Creators Update, certaines commandes, telles que CommandBar, ne prenaient pas en charge les étendues intégrées de touches d’accès rapide.

L’exemple suivant montre comment prendre en charge la propriété SecondaryCommands de la classe CommandBar grâce aux touches d’accès rapide qui sont disponibles une fois qu’une commande parent est appelée (comme le ruban dans Word).

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>Éviter les conflits de touches d’accès rapide

Des conflits de touches d’accès rapide se produisent lorsque deux ou plusieurs éléments dans la même étendue ont des touches d’accès rapide en double, ou démarrent avec les mêmes caractères alphanumériques.

Le système résout le problème des touches d’accès rapide en double en traitant la touche d’accès rapide du premier élément ajouté à l’arborescence visuelle, en ignorant tous les autres.

Lorsque plusieurs touches d’accès rapide démarrent avec le même caractère (par exemple, «A», «A1» et «AB»), le système traite la touche d’accès rapide à caractère unique et ignore tous les autres.

Éviter les conflits à l’aide des touches d’accès unique ou en étendant les commandes.

## <a name="choose-access-keys"></a>Choisir des touches d’accès rapide

Considérez les éléments suivants lors du choix des touches d’accès rapide:

-   Utiliser un caractère unique afin de minimiser les frappes et de prendre en charge des touches d’accès rapide par défaut (Alt+AccessKey)
-   Évitez d’utiliser plus de deux caractères
-   Évitez les conflits de touches d’accès rapide
-   Évitez les caractères qui sont difficiles à différencier d’autres caractères, par exemple la lettre «I» et le nombre «1», ou la lettre «O» et le nombre «0»
-   Utilisez des antécédents connus à partir d’autres applications populaires telles que Word («F» pour «Fichier»), «H» pour «Accueil» [Home en anglais], etc.)
-   Utilisez le premier caractère du nom de commande, ou un caractère avec une association de fermeture à la commande pour faciliter la mémorisation
    -   Si la première lettre est déjà affectée, utilisez une lettre qui est aussi proche que possible de la première lettre du nom de commande («N» pour Insérer)
    -   Utilisez une consonne distinctive à partir du nom de commande («W» pour Afficher [View en anglais])
    -   Utilisez une voyelle à partir du nom de commande.

## <a name="localize-access-keys"></a>Localiser des touches d’accès rapide

Si votre application doit être localisée dans plusieurs langues, vous devez également **envisager de localiser les touches d’accès rapide**. Par exemple, pour «H» pour «Home» en en-US et «I» pour «Incio» en es-ES.

Utilisez l’extension x:Uid dans le balisage pour appliquer des ressources localisées, comme illustré ici:

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Des ressources pour chaque langue sont ajoutées à des dossiers de chaîne correspondants dans le projet:

![Dossiers de chaîne de ressources en anglais et en espagnol](images/accesskeys/resource-string-folders.png)

_Dossiers de chaîne de ressources en anglais et en espagnol_

Les touches d’accès localisées sont spécifiées dans votre fichier de projets resources.resw:

![Indiquez la propriété AccessKey spécifiée dans le fichier resources.resw](images/accesskeys/resource-resw-file.png)

_Indiquez la propriété AccessKey spécifiée dans le fichier resources.resw_

Pour plus d’informations, consultez [Traduire des ressources d’interface utilisateur ](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965329(v=win.10).aspx)

## <a name="key-tip-positioning"></a>Positionnement des touches d’accès

Les touches d’accès sont affichées en tant que badges flottants relatifs à l’élément d’interface utilisateur correspondant, en prenant en compte la présence d’autres éléments d’interface utilisateur, d’autres touches d’accès, ainsi que la présence du bord de l’écran.

En règle générale, l’emplacement par défaut de la touche d’accès est satisfaisant et fournit une prise en charge intégrée pour l’interface utilisateur adaptative.

![Exemple de placement automatique de la touche d’accès](images/accesskeys/auto-keytip-position.png)

_Exemple de placement automatique de la touche d’accès_

Nous vous recommandons toutefois les éléments suivants, au cas où vous auriez besoin de plus de contrôle sur le positionnement de la touche d’accès:

1.  **Principe d’association évident**: l’utilisateur peut facilement associer la commande à la touche d’accès.

    a.  La touche d’accès doit être **proche** de l’élément qui possède la touche d’accès rapide (le propriétaire).  
    b.  La touche d’accès doit **éviter des éléments de couverture activés** qui ont des touches d’accès rapide.   
    c.  Si une touche d’accès ne peut pas être placée proche de son propriétaire, elle doit le chevaucher. 

2.  **Détectabilité**: l’utilisateur peut rapidement découvrir la commande à l’aide de la touche d’accès.

    a.  La touche d’accès ne **chevauche** jamais d’autres touches d’accès.  

3.  **Analyse simple:** l’utilisateur peut parcourir facilement les touches d’accès.

    a.  Les touches d’accès doivent être **alignées** entre elles et avec l’élément d’interface utilisateur.
    b.  Les touches d’accès doivent être **regroupées** autant que possible. 

### <a name="relative-position"></a>Position relative

Utilisez la propriété **KeyTipPlacementMode** afin de personnaliser le placement de la touche d’accès sur une base par élément ou par groupe.

Les modes de placement sont: Haut, Bas, Droite, Gauche, Masqué, Centrer et Auto.

![Modes de placement d’une touche d’accès](images/accesskeys/keytip-postion-modes.png)

_Modes de placement d’une touche d’accès_

La ligne centrale de la commande est utilisée pour calculer l’alignement vertical et horizontal de la touche d’accès.

L’exemple suivant montre comment configurer le placement de la touche d’accès d’un groupe de commandes, en utilisant la propriété KeyTipPlacementMode d’un conteneur StackPanel.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Décalages

Utilisez les propriétés KeyTipHorizontalOffset et KeyTipVerticalOffset d’un élément pour obtenir un contrôle plus précis de l’emplacement de la touche d’accès.

> [!NOTE]
> Les décalages ne peuvent pas être configurés lorsque la propriété KeyTipPlacementMode est définie sur le mode Auto.

La propriété KeyTipHorizontalOffset indique jusqu’à quel endroit, vers la gauche ou vers la droite, déplacer la touche d’accès. L’exemple montre comment configurer les décalages de la touche d’accès pour une touche.

![Modes de placement d’une touche d’accès](images/accesskeys/keytip-offsets.png)

_Définir les décalages horizontaux et verticaux d’une touche d’accès_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Alignement avec le bord de l’écran {#screen-edge-alignement .ListParagraph}

L’emplacement d’une touche d’accès est automatiquement ajusté au bord de l’écran afin de garantir que la touche d’accès est bien visible. Lorsque cela se produit, la distance entre la commande et le point d’alignement de la touche d’accès peut être différente des valeurs spécifiées pour les décalages horizontaux et verticaux.

![Modes de placement d’une touche d’accès](images/accesskeys/keytips-screen-edge.png)

_Le bord de l’écran provoque le repositionnement automatique de la touche d’accès._

## <a name="key-tip-style"></a>Style des touches d’accès

Nous vous conseillons d’utiliser le support intégré de touche d’accès pour les thèmes de plateforme, y compris à contraste élevé.

Si vous avez besoin de spécifier vos propres styles de touche d’accès, utilisez des ressources d’application, telles que KeyTipFontSize (taille de police), KeyTipFontFamily (famille de police), KeyTipBackground (arrière-plan), KeyTipForeground (premier plan), KeyTipPadding (remplissage), KeyTipBorderBrush (couleur de bordure) et KeyTipBorderThemeThickness (épaisseur de bordure).

![Modes de placement d’une touche d’accès](images/accesskeys/keytip-customization.png)

_Options de personnalisation d’une touche d’accès_

Cet exemple montre comment modifier ces ressources d’application:

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>Touches d’accès rapide et Narrateur

L’infrastructure XAML expose les propriétés d’automatisation qui permettent aux clients UIAutomation de découvrir des informations sur des éléments dans l’interface utilisateur.

Si vous spécifiez la propriété AccessKey sur une commande UIElement ou TextElement, vous pouvez utiliser la propriété [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) afin d’obtenir cette valeur. Les clients d’accessibilité, tels que Narrateur, lisent la valeur de cette propriété à chaque fois qu’un élément obtient un focus.

## <a name="related-articles"></a>Articles connexes

* [Interactions avec le clavier](keyboard-interactions.md)
* [Raccourcis clavier](keyboard-accelerators.md)

**Exemples**
* [Galerie de contrôles XAML (c'est-à-dire XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


