---
ms.openlocfilehash: 15379e51f8c272d0cc1e888684322104186bb200
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249501"
---
# <a name="teaching-tip"></a>Info-bulle de l’enseignement

Une info-bulle de l’apprentissage est un menu volant semi-structurées persistant et riches en contenu qui fournit des informations contextuelles. Il est souvent utilisé pour informer, rappelant et enseigne aux utilisateurs sur les fonctionnalités nouvelles et importantes qui peuvent améliorer leur expérience.

**API importantes :** [Classe de TeachingTip](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

Une info-bulle de l’enseignement peut-être faire disparaître la lumière ou nécessitent une action explicite pour la fermer. Un Conseil d’enseignement peut cibler un élément d’interface utilisateur spécifique avec sa Queue et également être utilisé sans fin ou cible.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ? 

Utilisez un **TeachingTip** contrôle peut se consacrer à attention de l’utilisateur nouveau ou importantes mises à jour et fonctionnalités, effectuer un rappel à un utilisateur des options non essentiels qui auraient améliorer leur expérience ou apprendre à un utilisateur comment une tâche doit être terminée. 

Étant donné que l’info-bulle de l’enseignement est temporaire, il serait le contrôle recommandé pour avertir les utilisateurs sur les erreurs ou des modifications d’état importantes.


## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous avez le <strong style="font-weight: semi-bold">galerie de contrôles XAML</strong> application installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/TeachingTip">ouvrez l’application et consultez le TeachingTip en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Une info-bulle pour l’enseignant peut avoir plusieurs configurations, y compris les plus importants.

Un Conseil d’enseignement peut cibler un élément d’interface utilisateur spécifique avec sa Queue pour améliorer la clarté contextuelle des informations présentées. 

![Un exemple d’application avec une info-bulle de l’enseignement ciblant l’enregistrement bouton. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-targeted.png)

Lorsque les informations présentées ne s’applique pas à un élément d’interface utilisateur particulier, une info-bulle de l’enseignement nontargeted peut être créée en supprimant la fin.

![Un exemple d’application avec un apprentissage info-bulle dans le coin inférieur droit. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-non-targeted.png)

Une info-bulle de l’apprentissage peut nécessiter l’utilisateur à faire disparaître via un bouton « X » dans un coin ou un bouton « Fermer » en bas. Un Conseil d’enseignement peuvent également être light-ignorer activé auquel cas il n’est pas de faire disparaître le bouton et l’info-bulle de l’enseignement fait disparaître au lieu de cela lorsqu’un utilisateur fait défiler ou interagit avec d’autres éléments de l’application. Étant donné ce comportement, ignorer light conseils sont la meilleure solution quand une info-bulle doit être placée dans une zone avec défilement. 

![Un exemple d’application avec une lumière-ignorer le Conseil d’apprentissage dans le coin inférieur droit. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. »](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>Créer une info-bulle de l’enseignement

Voici le XAML pour un apprentissage ciblé Conseil de contrôle qui montre l’apparence par défaut de la TeachingTip avec un titre et un sous-titre. Notez que l’info-bulle d’enseignement peut apparaître n’importe où dans l’arborescence d’éléments ou d’un code-behind. Dans l’exemple ci-dessous, il se trouve dans un ResourceDictionary.

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

Voici le résultat lorsque la Page contenant le bouton et enseigne info-bulle s’affiche :

![Un exemple d’application avec une info-bulle de l’enseignement ciblant l’enregistrement bouton. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>Conseils non ciblés

Pas tous les conseils sont liées à un élément à l’écran. Pour ces scénarios, ne définissez pas la propriété cible et l’info-bulle de l’enseignement s’affichera à la place par rapport aux bords de la racine du xaml. Toutefois, une info-bulle de l’enseignement peut avoir la fin supprimée tout en conservant le positionnement d’un élément d’interface utilisateur en définissant la propriété TailVisibility sur « Collapsed ». L’exemple suivant est d’un Conseil enseignement non ciblés.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

Notez que dans cet exemple le TeachingTip est dans l’arborescence d’éléments plutôt que dans un ResourceDictionary ou dans le code-behind. Cela n’a aucun effet sur le comportement ; le TeachingTip uniquement affiche l’ouverture et occupe aucun espace de disposition.

![Un exemple d’application avec un apprentissage info-bulle dans le coin inférieur droit. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>Placement par défaut

Info-bulle de l’enseignement réplique du menu volant [FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) comportement de positionnement avec la propriété TeachingTipPlacementMode. Le mode de sélection élective par défaut tente de placer un Conseil enseignement ciblé au-dessus de sa cible et une info-bulle de l’enseignement non ciblés centré en bas de la racine du xaml. Comme avec le menu volant, si le mode de sélection élective par défaut ne laisserait pas de place pour l’info-bulle de l’enseignement à afficher, un autre mode de sélection élective sera automatiquement sélectionné. 

Pour les applications qui prédisent gamepad entrée, consultez [gamepad et contrôle à distance des interactions]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Nous vous conseillons de tester l’accessibilité de gamepad de chaque conseil d’apprentissage à l’aide de toutes les configurations possibles de l’interface utilisateur d’une application.

Enseignement ciblé avec son PreferredPlacement défini sur « BottomLeft » s’affichent avec la queue centrée au bas de sa cible avec le corps de l’info-bulle enseignement décalé vers la gauche.

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

![Un exemple d’application avec un bouton « Enregistrer » qui est ciblé par un Conseil d’apprentissage en dessous de son coin supérieur gauche. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-targeted-preferred-placement.png)


Une info-bulle de l’enseignement non ciblés avec son PreferredPlacement défini sur « BottomLeft » s’affiche dans le coin inférieur gauche de la racine du xaml.

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![Un exemple d’application avec un apprentissage info-bulle dans le coin inférieur gauche. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-non-targeted-preferred-placement.png)

Le diagramme ci-dessous illustre le résultat de tous les modes PreferredPlacement 13 peuvent être définies pour ciblés enseigne les conseils.
![Trois objets étiqueté « cible » avec ciblés enseigne conseils permet d’afficher les modes de placement préférée ciblé du Conseil d’enseignement autour d’elles. Centrés au milieu de la première cible est un Conseil enseignement ciblé étiqueté « Center » pointant vers le bas au niveau de la cible avec sa fin. Centré au-dessus de la première cible est un Conseil enseignement ciblé intitulé « Top » pointant vers le bas au niveau de la cible avec sa fin. Centré à droite de la première cible est un Conseil enseignement ciblé étiqueté « Droite » pointant vers la gauche au niveau de la cible avec sa fin. Centrée sous la première cible est un Conseil enseignement ciblé intitulé « Bottom » pointant vers le haut sur la cible avec sa fin. Centré à gauche de la première cible est un Conseil enseignement ciblé étiqueté « Gauche » pointant vers la droite au niveau de la cible avec sa fin. À gauche de la deuxième cible est une info-bulle de l’enseignement intitulée « LeftTop » vers la droite au niveau de la cible et son corps a déplacé vers le haut. Au-dessus de la deuxième cible est une info-bulle de l’enseignement intitulée « TopLeft » qui pointe vers le bas au niveau de la cible et a son corps décalée vers la gauche. À droite de la deuxième cible est un Conseil d’apprentissage étiqueté « RightBottom » pointe vers la gauche au niveau de la cible et de son corps a déplacé vers le bas. Sous la deuxième cible est une info-bulle de l’enseignement intitulée « BottomRight » qui pointe vers le haut de la cible et son corps a déplacé vers la droite. Au-dessus de la troisième cible est une info-bulle de l’enseignement intitulée « Droit » qui pointe vers le bas au niveau de la cible et son corps a déplacé vers la droite. À droite de la troisième cible est un Conseil d’apprentissage étiqueté « RightTop » pointe vers la gauche au niveau de la cible et de son corps a déplacé vers le haut. Ci-dessous la troisième cible est une info-bulle de l’enseignement intitulée « BottomLeft » qui pointe vers le haut de la cible et a son corps décalée vers la gauche. À gauche de la troisième cible est une info-bulle de l’enseignement intitulée « LeftBottom » vers la droite au niveau de la cible et son corps a déplacé vers le bas.](../images/teaching-tip-targeted-preferred-placement-modes.png)

Le diagramme ci-dessous illustre le résultat de tous les modes PreferredPlacement 13 qui peuvent être définies pour obtenir des conseils pour l’enseignant non ciblés.
![Une fenêtre d’application affichant les neuf astuces enseignement non ciblés pour illustrer les modes de non ciblés de placement préférée de l’enseignement-bulle. L’info-bulle de l’apprentissage dans le coin supérieur gauche de l’application est étiqueté « TopLeft ou LeftTop. » L’info-bulle de l’enseignement centrée sur le bord supérieur de l’application est étiqueté « Top ». L’info-bulle de l’enseignement dans l’angle supérieur droit de l’application est étiqueté « Droit ou RightTop. » L’info-bulle de l’enseignement centrée sur le bord gauche de l’application est étiqueté « Left ». L’info-bulle de l’enseignement centré au milieu de l’application est étiqueté « Center ».
L’info-bulle de l’enseignement centrée sur le bord droit de l’application est étiqueté « Droite ». L’info-bulle de l’apprentissage dans le coin inférieur gauche de l’application est étiqueté « BottomLeft ou LeftBottom. » L’info-bulle de l’enseignement centrée sur le bord inférieur de l’application est étiqueté « Bottom ». L’info-bulle de l’enseignement dans l’angle inférieur droit de l’application est étiqueté « BottomRight ou RightBottom. »](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>Ajouter une marge de sélection élective  

Vous pouvez contrôler l’amplitude une info-bulle de l’enseignement ciblé est définie en dehors de sa cible et jusqu’où une info-bulle de l’enseignement non ciblés a mis à part les bords de la racine du xaml à l’aide de la propriété PlacementMargin. Comme [marge](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin), PlacementMargin a quatre valeurs : gauche, droite, haut et bas – uniquement les valeurs appropriées sont utilisées. Par exemple, PlacementMargin.Left s’applique lorsque l’info-bulle reste de la cible ou sur le bord gauche de la racine du xaml.

L’exemple suivant montre une info-bulle non ciblés avec gauche/haut/droite/bas du PlacementMargin tous la valeur 80.

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

![Un exemple d’application avec une info-bulle de l’enseignement positionnée vers, mais pas entièrement sur l’angle inférieur droit. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>Ajoutez du contenu

Contenu peut être ajouté à une info-bulle de l’apprentissage à l’aide de la propriété de contenu. S’il existe plus de contenu à afficher à la taille d’un Conseil enseignement permettra, une barre de défilement est automatiquement activée pour autoriser un utilisateur à faire défiler la zone de contenu. 

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

![Un exemple d’application avec une info-bulle de l’enseignement ciblant l’enregistrement bouton. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Dans la zone de contenu de l’info-bulle pour l’enseignant est une case à cocher intitulée « Ne pas afficher conseils au démarrage » et est en dessous du texte qui se lit « Vous pouvez modifier vos préférences de l’info-bulle dans les paramètres si vous changez d’avis » où « Paramètres » sont un lien vers la page des paramètres de l’application. Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>Ajouter des boutons

Par défaut, une norme de « X » le bouton Fermer est affichée à côté du titre d’une info-bulle de l’enseignement. Le bouton Fermer peut être personnalisé avec la propriété CloseButtonContent, dans laquelle les cas, le bouton est déplacé vers le bas de l’info-bulle de l’enseignement.

**Remarque: Aucun bouton Fermer n’apparaîtra sur light-ignorer les conseils est activées**

Un bouton d’action personnalisée peut être ajouté en définissant ActionButtonContent propriété (et éventuellement le ActionButtonCommand et les propriétés ActionButtonCommandParameter).

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

![Un exemple d’application avec une info-bulle de l’enseignement ciblant l’enregistrement bouton. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Dans la zone de contenu de l’info-bulle pour l’enseignant est une case à cocher intitulée « Ne pas afficher conseils au démarrage » et est en dessous du texte qui se lit « Vous pouvez modifier vos préférences de l’info-bulle dans les paramètres si vous changez d’avis » où « Paramètres » sont un lien vers la page des paramètres de l’application. En bas de l’apprentissage sont deux boutons, une grise à gauche qui se lit « Désactiver » et un bleu à droite qui lit « ça ! »](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Contenu de héros

Edge au contenu de bord peut être ajouté à une info-bulle de l’enseignement en définissant la propriété HeroContent. Peut indiquer l’emplacement du contenu de héros vers le haut ou le bas d’une info-bulle de l’enseignement en définissant la propriété HeroContentPlacement.

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

![Un exemple d’application avec une info-bulle de l’enseignement ciblant l’enregistrement bouton. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » En bas de l’info-bulle pour l’enseignant est une image de bordure pour la bordure d’un homme de dessin animé placer les fichiers dans le cloud. Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>Ajouter une icône

Une icône peut être ajoutée en regard du titre et le sous-titre à l’aide de la propriété IconSource. Tailles d’icônes recommandées incluent 16px, 24 px et 32 px. 

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

![Un exemple d’application avec une info-bulle de l’enseignement ciblant l’enregistrement bouton. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » À gauche du titre et sous-titre se trouve une icône de disquette. Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>Activer light-fermer

Faire disparaître la lumière de fonctionnalité est désactivée par défaut, mais elle peut activée afin qu’une info-bulle de l’enseignement fait disparaître, par exemple, lorsqu’un utilisateur fait défiler ou interagit avec d’autres éléments de l’application. Étant donné ce comportement, ignorer light conseils sont la meilleure solution quand une info-bulle doit être placée dans une zone avec défilement. 

Le bouton Fermer est automatiquement supprimé à partir d’un Conseil d’enseignement activé disparition rapide pour identifier ses light-ignorer le comportement pour les utilisateurs. 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![Un exemple d’application avec une lumière-ignorer le Conseil d’apprentissage dans le coin inférieur droit. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. »](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>Séquence d’échappement les limites de racine du xaml

Sur Windows Conseil de 1 et versions ultérieures, un enseignement de 19H version échapper les limites de la racine du xaml et de l’écran en définissant la propriété ShouldConstrainToRootBounds. Lorsque cette propriété est activée, une info-bulle pour l’enseignant ne tentera pas rester dans les limites de la racine de xaml ni de l’écran et sera toujours position à l’ensemble PreferredPlacement mode. Il est encouragé à activer la propriété IsLightDismissEnabled et définir le mode de PreferredPlacement plus proche du centre de la racine du xaml pour garantir la meilleure expérience pour les utilisateurs.

Dans les versions antérieures de Windows, cette propriété est ignorée et l’info-bulle de l’enseignement reste toujours dans les limites de la racine du xaml.

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

![Un exemple d’application avec une info-bulle de l’enseignement en dehors de bas à droite l’application. Le titre de Conseil intitulée « Enregistrement automatique » et le sous-titre « Nous enregistrer vos modifications fur - vous donc jamais. » Il existe un bouton de fermeture dans l’angle supérieur droit de l’info-bulle de l’enseignement.](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>L’annulation et report fermer

L’événement Closing peut être utilisé pour annuler les et/ou de différer la fermeture d’une info-bulle de l’enseignement. Cela peut être utilisé pour ne fermez pas l’info-bulle de l’enseignement ou à attendre une action ou une animation personnalisée se produise. Lors de la fermeture d’une info-bulle de l’enseignement est annulée, IsOpen reviens sur true, toutefois, il reste false lors du report. Une fermeture par programmation peut également être annulée. 

**Remarque: Si aucune option de sélection élective ne permettrait une info-bulle de l’enseignement afficher entièrement, info-bulle de l’enseignement effectue une itération dans son cycle de vie d’événement pour forcer une fermeture plutôt que d’afficher sans un bouton Fermer accessible. Si l’application annule l’événement Closing, l’info-bulle pour l’enseignant peut rester ouverte sans un bouton Fermer accessible.**

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
* Conseils ne sont pas et ne doivent pas contenir d’informations ou des options qui sont critiques pour l’expérience d’une application. 
* Essayez d’éviter l’affichage des conseils d’enseignement trop souvent. Conseils d’apprentissage sont probablement chaque réception d’une attention lorsqu’ils sont échelonnées tout au long de longues sessions ou entre plusieurs sessions.    
* Conservez les conseils concis et leur rubrique clair. Études montrent les utilisateurs, en moyenne, uniquement lire les mots de 3 à 5 et comprendre uniquement des mots de 2-3 avant de décider d’interagir avec une info-bulle.
* Accessibilité Gamepad d’un Conseil d’apprentissage n’est pas garantie. Pour les applications qui prédisent gamepad entrée, consultez [gamepad et contrôle à distance des interactions]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction). Nous vous conseillons de tester l’accessibilité de gamepad de chaque conseil d’apprentissage à l’aide de toutes les configurations possibles de l’interface utilisateur d’une application.
* Lorsque vous activez un Conseil d’enseignement à la séquence d’échappement de la racine du xaml, nous vous conseillons également activer la propriété IsLightDismissEnabled et définir le mode de PreferredPlacement plus proche du centre de la racine du xaml. 

### <a name="reconfiguring-an-open-teaching-tip"></a>Reconfiguration d’une info-bulle de l’enseignement ouvert

Certains contenus et les propriétés peuvent être reconfigurées lorsque l’info-bulle de l’enseignement est ouvert et prend effet immédiatement. Autre contenu et les propriétés, telles que la propriété icon, les boutons d’Action et de fermeture et reconfiguration entre faire disparaître la lumière et explicite ignorer nécessiteront tous l’info-bulle de l’enseignement être fermé et rouvert pour les modifications apportées à ces propriétés prenne effet. Notez que la modification du comportement de licenciement de manuel-ignorer pour ignorer light lorsqu’une info-bulle de l’enseignement est ouverte entraîne l’info-bulle de l’apprentissage d’avoir son bouton Fermer avant du faire disparaître la lumière comportement est activé et l’info-bulle peut rester bloqué à l’écran.
