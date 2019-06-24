---
Description: Un conseil d’apprentissage est un menu volant semi-persistant riche en contenu qui fournit des informations contextuelles.
title: Conseils d’apprentissage
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
ms.custom: 19H1
ms.localizationpriority: medium
ms.openlocfilehash: 7ea4dc1d77c5cf7199d084d4646b5862599a1d54
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63794422"
---
# <a name="teaching-tip"></a>Conseil d’apprentissage

Un conseil d’apprentissage est un menu volant semi-persistant riche en contenu qui fournit des informations contextuelles. Il est souvent utilisé pour fournir à l’utilisateur des informations, des rappels et des enseignements concernant des fonctionnalités nouvelles ou importantes susceptibles d’améliorer son expérience.

**API importantes :** [Classe TeachingTip](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

La fermeture d’un conseil d’apprentissage peut résulter d’un abandon interactif ou d’une action explicite. Un conseil d’apprentissage peut cibler un élément d’interface utilisateur spécifique avec son extension ou être utilisé sans extension ou cible.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ? 

Un contrôle **TeachingTip** permet d’attirer l’attention sur des mises à jour et fonctionnalités nouvelles ou importantes, de rappeler l’existence d’options non essentielles susceptibles d’améliorer l’expérience ou d’enseigner la manière d’accomplir une tâche. 

Le conseil d’apprentissage s’affichant de façon temporaire, son utilisation n’est pas recommandée pour informer l’utilisateur d’erreurs ou de changements d’état importants.


## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/TeachingTip">ouvrir l’application et voir le conseil d’apprentissage en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un conseil d’apprentissage peut avoir plusieurs configurations, dont les plus remarquables sont présentées ci-après.

Un conseil d’apprentissage peut cibler un élément d’interface utilisateur spécifique avec son extension pour améliorer la clarté contextuelle des informations qu’il présente. 

![Exemple d’application avec un conseil d’apprentissage ciblant le bouton Enregistrer. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-targeted.png)

Lorsque les informations présentées n’ont pas trait à un élément d’interface utilisateur particulier, il est possible de créer un conseil d’apprentissage non ciblé en supprimant l’extension.

![Exemple d’application avec un conseil d’apprentissage dans l’angle inférieur droit. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-non-targeted.png)

Un conseil d’apprentissage peut nécessiter que l’utilisateur le ferme à l’aide d’un bouton « X » dans un angle supérieur ou d’un bouton « Fermer » en bas. Un conseil d’apprentissage peut également disparaître par abandon interactif, auquel cas il est dépourvu de bouton de fermeture, mais disparaît quand un utilisateur interagit avec d’autres éléments de l’application. Compte tenu de ce comportement, le mode d’abandon interactif constitue la meilleure solution quand le conseil doit figurer dans une zone avec défilement. 

![Exemple d’application comportant un conseil d’apprentissage avec abandon interactif dans l’angle inférieur droit. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. »](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>Créer un conseil d’apprentissage

Voici le code XAML d’un conseil d’apprentissage ciblé qui montre l’apparence par défaut du conseil d’apprentissage avec un titre et un sous-titre. Notez que le conseil d’apprentissage peut apparaître n’importe où dans l’arborescence d’éléments ou le code-behind. Dans l’exemple ci-dessous, il se trouve dans un ResourceDictionary.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

Voici le résultat lorsque la page contenant le bouton et le conseil d’apprentissage s’affiche :

![Exemple d’application avec un conseil d’apprentissage ciblant le bouton Enregistrer. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>Conseils non ciblés

Certains conseils ne sont pas liés à un élément affiché à l’écran. Dans ce cas, ne définissez pas la propriété Target, de sorte que le conseil d’apprentissage apparaisse plutôt dans un positionnement relatif par rapport aux bords de la racine XAML. Il est également possible de supprimer l’extension d’un conseil d’apprentissage tout en conservant le positionnement par rapport à un élément d’interface utilisateur en définissant la propriété TailVisibility sur « Collapsed ». L’exemple suivant présente un conseil d’apprentissage non ciblé.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

Notez que, dans cet exemple, le conseil d’apprentissage figure dans l’arborescence d’éléments plutôt que dans un ResourceDictionary ou un code-behind. Cela n’a pas d’incidence sur le comportement car le conseil d’apprentissage s’affiche uniquement à l’ouverture et n’occupe aucun espace dans la disposition.

![Exemple d’application avec un conseil d’apprentissage dans l’angle inférieur droit. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Positionnement par défaut

Le conseil d’apprentissage reproduit le comportement de positionnement [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) du menu volant avec la propriété TeachingTipPlacementMode. Le mode de positionnement par défaut tente de placer un conseil d’apprentissage ciblé au-dessus de sa cible, et un conseil d’apprentissage non ciblés centré en bas de la racine XAML. Comme avec un menu volant, si le mode de positionnement par défaut n’offre pas suffisamment d’espace pour l’affichage du conseil d’apprentissage, un autre mode de positionnement est automatiquement sélectionné. 

Pour les applications qui prévoient l’utilisation d’une manette de jeu, voir [Interactions avec manette de jeu et télécommande]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Nous vous conseillons de tester l’accessibilité à la manette de jeu de chaque conseil d’apprentissage en utilisant toutes les configurations possibles de l’interface utilisateur d’une application.

Un conseil d’apprentissage ciblé avec le mode PreferredPlacement défini sur « BottomLeft » s’affiche avec l’extension centrée au bas de sa cible et le corps décalé vers la gauche.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Exemple d’application avec un bouton « Enregistrer » ciblé par un conseil d’apprentissage positionné sous son angle gauche. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-targeted-preferred-placement.png)


Un conseil d’apprentissage non ciblé avec son mode PreferredPlacement défini sur « BottomLeft » s’affiche dans l’angle inférieur gauche de la racine XAML.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![Exemple d’application avec un conseil d’apprentissage dans l’angle inférieur gauche. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-non-targeted-preferred-placement.png)

Le diagramme ci-dessous illustre le résultat des 13 définitions possibles du mode PreferredPlacement pour les conseils d’apprentissage ciblés.
![Illustration contenant 13 conseils d’apprentissage, chacun correspondant à un mode de positionnement ciblé distinct. Chaque conseil d’apprentissage est étiqueté avec le mode auquel il correspond. Le premier mot d’un mode de positionnement indique le côté de la cible sur lequel le conseil d’apprentissage est centré. L’extension du conseil d’apprentissage est toujours située au centre de ce côté de la cible et pointe vers celle-ci. Si le mode de positionnement contient un deuxième mot, le corps du conseil d’apprentissage n’est pas centré, mais décalé dans la direction spécifiée. Par exemple, le mode de positionnement « TopRight » fait apparaître le conseil d’apprentissage au-dessus de la cible et décalé vers la droite, avec son extension pointant vers le bas au centre du bord supérieur de la cible. Le corps étant décalé vers la droite, l’extension est presque sur le bord gauche du corps du conseil d’apprentissage et celui-ci s’étend au-delà du bord droit de la cible. Le mode de positionnement « Center » est unique et a pour effet que l’extension du conseil d’apprentissage pointe sur le centre de la cible et que le conseil d’apprentissage est centré sur la moitié supérieure de celle-ci.](../images/teaching-tip-targeted-preferred-placement-modes.png)

Le diagramme ci-dessous illustre le résultat des 13 modes PreferredPlacement qui peuvent être définis pour des conseils d’apprentissage non ciblés.
![Illustration contenant neuf conseils d’apprentissage, chacun correspondant à un mode de positionnement non ciblé distinct. Chaque conseil d’apprentissage est étiqueté avec le mode auquel il correspond. Le premier mot d’un mode de positionnement indique le côté de la racine XAML sur lequel le conseil d’apprentissage est centré. Si le mode de positionnement contient un deuxième mot, le conseil d’apprentissage se positionne dans l’angle spécifié de la racine XAML. Par exemple, le mode de positionnement « TopRight » a pour effet que le conseil d’apprentissage apparaît dans l’angle supérieur droit de la racine XAML. Pour les modes de positionnement non ciblés, l’ordre des deux mots n’affecte pas le positionnement. TopRight équivaut à RightTop. Le mode de positionnement « Center » est unique et a pour effet que le conseil d’apprentissage s’affiche au centre vertical et horizontal de la racine XAML.](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Ajouter une marge de positionnement  

Vous pouvez contrôler l’écart séparant un conseil d’apprentissage ciblé de sa cible, et séparant un conseil d’apprentissage non ciblé des bords de la racine XAML à l’aide de la propriété PlacementMargin. Comme la propriété [Margin](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin), la propriété PlacementMargin peut prendre quatre valeurs : left, right, top et bottom. Seules les valeurs pertinentes sont donc utilisées. Par exemple, la propriété PlacementMargin.Left s’applique lorsque le conseil se trouve à gauche de la cible ou sur le bord gauche de la racine XAML.

L’exemple suivant présente un conseil non ciblé avec les valeurs Left/Top/Right/Bottom de la propriété PlacementMargin définies sur 80.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</controls:TeachingTip>
```

![Exemple d’application avec un conseil d’apprentissage positionné vers mais pas totalement contre l’angle inférieur droit. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Ajoutez du contenu

Vous pouvez ajouter du contenu à un conseil d’apprentissage à l’aide de la propriété Content. S’il y a plus de contenu à afficher que ce que la taille d’un conseil d’apprentissage permet, une barre de défilement est automatiquement activée pour permettre à un utilisateur de faire défiler la zone de contenu. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Exemple d’application avec un conseil d’apprentissage ciblant le bouton Enregistrer. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » La zone de contenu du conseil d’apprentissage contient une case à cocher libellée « Ne pas afficher les conseils au démarrage », sous laquelle figure le texte : « Vous pouvez modifier vos préférences pour l’affichage des conseils dans Paramètres si vous changez d’avis », où « Paramètres » est un lien vers la page des paramètres de l’application. Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Ajouter des boutons

Par défaut, un bouton standard de fermeture « X » est affiché en regard du titre d’un conseil d’apprentissage. Vous pouvez personnaliser ce bouton avec la propriété CloseButtonContent, auquel cas le bouton est déplacé vers le bas du conseil d’apprentissage.

**Remarque : aucun bouton de fermeture ne s’affiche sur les conseils pour lesquels le mode d’abandon interactif est activé**

Vous pouvez ajouter une bouton d’action personnalisé en définissant la propriété ActionButtonContent (et éventuellement les propriétés ActionButtonCommand et ActionButtonCommandParameter).

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Exemple d’application avec un conseil d’apprentissage ciblant le bouton Enregistrer. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » La zone de contenu du conseil d’apprentissage contient une case à cocher libellée « Ne pas afficher les conseils au démarrage », sous laquelle figure le texte : « Vous pouvez modifier vos préférences pour l’affichage des conseils dans Paramètres si vous changez d’avis », où « Paramètres » est un lien vers la page des paramètres de l’application. Deux boutons figurent au bas du conseil d’apprentissage : un gris à gauche libellé « Désactiver », et un bleu à droite libellé « OK »](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Contenu de bannière

Vous pouvez ajouter du contenu de bord à bord à un conseil d’apprentissage en définissant la propriété HeroContent. Vous pouvez définir l’emplacement du contenu de bannière en haut ou en bas d’un conseil d’apprentissage en définissant la propriété HeroContentPlacement.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <controls:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </controls:TeachingTip.HeroContent>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Exemple d’application avec un conseil d’apprentissage ciblant le bouton Enregistrer. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » En bas du conseil d’apprentissage figure une image de bord à bord d’un homme stylisé plaçant des fichiers dans le cloud. Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Ajouter une icône

Vous pouvez ajouter une icône en regard du titre et du sous-titre à l’aide de la propriété IconSource. Les tailles d’icônes suggérées sont 16px, 24 px et 32 px. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <controls:TeachingTip.IconSource>
                <controls:SymbolIconSource Symbol="Save" />
            </controls:TeachingTip.IconSource>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![Exemple d’application avec un conseil d’apprentissage ciblant le bouton Enregistrer. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » À gauche du titre et du sous-titre figure une icône de disquette. Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Activer l’abandon interactif

La fonctionnalité d’abandon interactif est désactivée par défaut mais vous pouvez l’activer pour qu’un conseil d’apprentissage disparaisse, par exemple, quand l’utilisateur interagit avec d’autres éléments de l’application. Compte tenu de ce comportement, le mode d’abandon interactif constitue la meilleure solution quand le conseil doit figurer dans une zone avec défilement. 

Le bouton de fermeture est automatiquement supprimé d’un conseil d’apprentissage pour lequel le mode d’abandon interactif est activé afin précisément d’indiquer aux utilisateurs que ce mode est activé. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![Exemple d’application comportant un conseil d’apprentissage avec abandon interactif dans l’angle inférieur droit. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. »](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Échappement des limites de racine XAML

Sous Windows 19H1 et versions ultérieures, un conseil d’apprentissage peut échapper les limites de la racine XAML et de l’écran en définissant la propriété ShouldConstrainToRootBounds. Lorsque cette propriété est activée, un conseil d’apprentissage ne tente pas de rester dans les limites de la racine XAML ou de l’écran, et est toujours positionné conformément au mode PreferredPlacement défini. Il est conseillé d’activer la propriété IsLightDismissEnabled et de définir le mode PreferredPlacement le plus proche du centre de la racine XAML afin de garantir une expérience optimale pour les utilisateurs.

Dans les versions antérieures de Windows, cette propriété est ignorée et le conseil d’apprentissage reste toujours dans les limites de la racine XAML.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</controls:TeachingTip>
```

![Exemple d’application avec un conseil d’apprentissage à l’extérieur de l’angle inférieur droit. Le titre du conseil est « Enregistrement automatique », et le sous-titre explique : « Nous enregistrons vos modifications au fur et à mesure. Vous n’avez pas à le faire. » Un bouton de fermeture figure dans l’angle supérieur droit du conseil d’apprentissage.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>Annulation et retardement de fermeture

L’événement Closing permet d’annuler ou de retarder la fermeture d’un conseil d’apprentissage. Vous pouvez l’utiliser pour garder le conseil d’apprentissage ouvert ou laisser un peu de temps pour qu’une action ou une animation personnalisée se produise. En cas d’annulation de la fermeture d’un conseil d’apprentissage, la valeur de IsOpen redevient true, mais elle reste false en cas de retardement de l’annulation. Vous pouvez également annuler une fermeture par programmation. 

**Remarque : Si aucune option de positionnement ne permet d’afficher entièrement un conseil d’apprentissage, celui-ci effectue une itération dans son cycle de vie d’événement pour forcer une fermeture au lieu de s’afficher sans bouton de fermeture accessible. Si l’application annule l’événement Closing, il se peut que le conseil d’apprentissage reste ouvert sans bouton de fermeture accessible.**

XAML
```XAML
<controls:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</controls:TeachingTip>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="remarks"></a>Notes

### <a name="related-articles"></a>Articles connexes 

* [Boîtes de dialogue et menus volants](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>Recommandations
* Les conseils sont impermanents et ne doivent pas contenir d’informations ou d’options critiques pour l’expérience d’une application. 
* Essayez d’éviter l’affichage trop fréquent de conseils d’apprentissage. Les conseils d’apprentissage ont plus de chance d’attirer l’attention quand ils sont espacés sur de longues sessions, voire sur plusieurs sessions.    
* Veillez à ce qu’ils restent concis et à ce que leur thème soit clairement identifiable. La recherche a montré que les utilisateurs ne lisent en moyenne que de 3 à 5 mots et n’en comprennent que 2 ou 3 avant de décider d’interagir avec un conseil.
* L’accessibilité à la manette de jeu d’un conseil d’apprentissage n’est pas garantie. Pour les applications qui prévoient l’utilisation d’une manette de jeu, voir [Interactions avec manette de jeu et télécommande]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Nous vous conseillons de tester l’accessibilité à la manette de jeu de chaque conseil d’apprentissage en utilisant toutes les configurations possibles de l’interface utilisateur d’une application.
* Lorsque vous activez un conseil d’apprentissage pour échapper la racine XAML, il est conseillé d’activer la propriété IsLightDismissEnabled et de définir le mode PreferredPlacement le plus proche du centre de la racine XAML. 

### <a name="reconfiguring-an-open-teaching-tip"></a>Reconfiguration d’un conseil d’apprentissage ouvert

Il est possible de reconfigurer certains contenus et propriétés lorsque le conseil d’apprentissage est ouvert, avec effet immédiat. D’autres contenus et propriétés, tels que la propriété icon, les boutons Action et Fermer, ainsi que la reconfiguration d’abandon interactif en abandon explicite, nécessitent que le conseil d’apprentissage soit fermé, puis rouvert pour que les modifications apportées prennent effet. Notez que la modification du comportement d’abandon manuel en abandon interactif quand un conseil d’apprentissage est ouvert a pour effet que le bouton de fermeture est supprimé avant que le mode d’abandon interactif soit activé, et que le conseil peut rester bloqué à l’écran.
