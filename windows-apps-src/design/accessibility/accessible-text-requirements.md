---
Description: This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: Exigences de texte accessible
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bdfcdc0782515928740a9c01b409b5170540cb27
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8468321"
---
# <a name="accessible-text-requirements"></a>Exigences de texte accessible  




Ici sont décrites les meilleures pratiques d’accessibilité du texte, en s’assurant que les couleurs et les arrière-plans respectent le coefficient de contraste. Elle traite également des rôles MicrosoftUIAutomation que peuvent avoir les éléments de texte dans une application UWP et des meilleures pratiques relatives au texte des graphiques.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>Coefficients de contraste  
Bien que les utilisateurs aient toujours la possibilité de basculer en mode de contraste élevé, la conception de votre application en ce qui concerne le texte doit considérer cette option comme un dernier recours. L’idéal consiste à s’assurer que le texte de votre application remplit certains critères établis quant au niveau de contraste entre le texte et son arrière-plan. L’évaluation du niveau de contraste est basée sur des techniques déterministes qui ne prennent pas en compte la teinte. Par exemple, si vous avez du texte rouge sur fond vert, ce texte risque de ne pas être lisible pour quelqu’un souffrant de daltonisme. La vérification et la correction du coefficient de contraste peuvent éliminer ce genre de problème d’accessibilité.

Les recommandations en matière de contraste du texte documentées ici sont basées sur une norme d’accessibilité Web intitulée [G18: Ensuring that a contrast ratio of at least 4.5:1 exists between text (and images of text) and background behind the text](http://go.microsoft.com/fwlink/p/?linkid=221823). Ces conseils se trouvent dans la spécification *Techniques W3C pour WCAG 2.0*.

Pour être considéré comme accessible, le texte visible doit présenter un coefficient de contraste de luminosité minimal de 4,5 pour 1 par rapport à l’arrière-plan. Les exceptions comprennent les logos et le texte accessoire tel que le texte qui fait partie d’un composant d’interface utilisateur inactif.

Le texte décoratif et qui ne véhicule aucune information est exclu. Par exemple, si des mots aléatoires sont utilisés pour créer un arrière-plan, et que ces mots peuvent être réorganisés ou remplacés sans changer le sens, ils sont considérés comme étant décoratifs et n’ont pas besoin de répondre à ce critère.

Utilisez des outils de contraste des couleurs pour vérifier que le coefficient de contraste du texte visible est acceptable. Pour connaître les outils permettant de tester les coefficients de contraste, voir la spécification [Techniques for WCAG 2.0 G18 (section Resources)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Certains outils répertoriés par la spécification Techniques for WCAG 2.0 G18 ne peuvent pas être utilisés de manière interactive avec une application UWP. Vous pouvez être amené à saisir manuellement des valeurs de couleur de premierplan et d’arrière-plan dans l’outil, ou à effectuer des captures d’écran de l’interface utilisateur de l’application, puis à exécuter l’outil de coefficient de contraste sur l’image de capture d’écran.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>Rôles d’éléments de texte  
Une application UWP peut utiliser les éléments par défaut suivants (couramment appelés *éléments de texte* ou *contrôles d’édition de texte*) :

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) : le rôle est [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) : le rôle est [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) (et classe de débordement [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.richtextblockoverflow)) : le rôle est [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/BR227548) : le rôle est [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182)

Quand un contrôle signale qu’il a le rôle [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182), les technologies d’assistance supposent qu’il existe un ou plusieurs moyens pour l’utilisateur de modifier les valeurs. Par conséquent, si vous placez du texte statique dans un objet [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683), vous signalez de manière incorrecte le rôle et donc la structure de votre application à l’utilisateur d’accessibilité.

Dans les modèles de texte pour XAML, deux éléments sont principalement utilisés pour le texte statique : [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) et [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565). Comme aucun de ces éléments n’est une sous-classe [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390), aucun des deux ne peut être actif via le clavier ou apparaître dans l’ordre de tabulation. Toutefois, cela ne signifie pas que les technologies d’assistance ne peuvent pas ou ne veulent pas les lire. Les lecteurs d’écran sont généralement conçus pour prendre en charge plusieurs modes de lecture du contenu dans une application, notamment un mode de lecture dédié ou des modèles de navigation qui vont au-delà du focus et de l’ordre de tabulation, comme un « curseur virtuel ». Par conséquent, ne placez pas votre texte statique dans des conteneurs pouvant être actifs simplement pour que l’ordre de tabulation y amène l’utilisateur. Les utilisateurs des technologies d’assistance s’attendent à ce que tous les éléments inclus dans l’ordre de tabulation soient interactifs. Par conséquent, le fait d’y rencontrer du texte statique peut s’avérer plus déroutant qu’utile. Nous vous recommandons de tester cette fonction vous-même, via le Narrateur, afin de vous faire une idée de l’expérience utilisateur liée à votre application lorsque vous utilisez le lecteur d’écran pour examiner le texte statique de cette dernière.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>Accessibilité de suggestion automatique  
Lorsqu’un utilisateur effectue une saisie dans un champ d’entrée et qu’une liste de suggestions potentielles apparaît, il s’agit d’une suggestion automatique. Ce scénario est courant dans le champ **À:** d’un message, dans la zone de recherche Cortana de Windows, dans le champ d’entrée d’URL de MicrosoftEdge, dans le champ d’entrée de situation géographique de l’application Météo, etc. Si vous utilisez la fonction XAML [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox) ou les commandes intrinsèques HTML, cette expérience est d’ores et déjà raccordée par défaut. Pour la rendre accessible, le champ d’entrée et la liste doivent être associés. Cette procédure est expliquée dans la section [Implémentation de la suggestion automatique](#implementing_auto-suggest).

La Narrateur a été mis à jour pour rendre cette expérience accessible avec un mode spécial de suggestions. À un niveau élevé, quand le champ d’entrée et la liste sont associés de manière appropriée, l’utilisateur final:

* Aura connaissance de la liste et du moment de clôture de cette dernière
* Aura connaissance du nombre de suggestions disponibles
* Aura connaissance de l’élément sélectionné, le cas échéant
* Sera en mesure de déplacer le focus du Narrateur de la liste
* Sera en mesure de parcourir une suggestion avec l’ensemble des autres modes de lecture

![Liste de suggestions](images/autosuggest-list.png)<br/>
_Exemple de liste de suggestions_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>Implémentation de la suggestion automatique  
Pour rendre cette expérience accessible, le champ d’entrée et la liste doivent être associés dans l’arborescence UIA. Cette association est effectuée avec la propriété [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) des applications de bureau ou la propriété [ControlledPeers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) des applications UWP.

À un niveau élevé, il existe 2types d’expériences de suggestion automatique.

**Sélection par défaut**  
Si une sélection par défaut est effectuée dans la liste, le Narrateur recherche un événement [**UIA_SelectionItem_ElementSelectedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223) dans l’application de bureau, ou l’événement [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) à déclencher dans une application UWP. Chaque fois que la sélection est modifiée, quand un utilisateur saisit une autre lettre et que les suggestions ont été mises à jour et quand un utilisateur parcourt la liste, l’événement **ElementSelected** doit être déclenché.

![Liste avec une sélection par défaut](images/autosuggest-default-selection.png)<br/>
_Exemple avec une sélection par défaut_

**Aucune sélection par défaut**  
En cas d’absence de sélection par défaut, comme dans la zone de situation géographique de l’application Météo, le Narrateur recherche l’événement [**UIA_LayoutInvalidatedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223 ) de bureau ou l’événement UWP [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) à déclencher sur la liste à chaque mise à jour de cette dernière.

![Liste sans sélection par défaut](images/autosuggest-no-default-selection.png)<br/>
_Exemple sans sélection par défaut_

### <a name="xaml-implementation"></a>Implémentation XAML  
Si vous utilisez la fonction XAML par défaut [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox), l’ensemble des connexions sont déjà effectuées. Si vous développez votre propre expérience de suggestion automatique à l’aide d’un élément [**TextBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox) et d’une liste, vous devrez définir la liste en tant que [**AutomationProperties.ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) sur l’élément **TextBox**. Vous devez déclencher l’événement **AutomationPropertyChanged** de la propriété [**ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) chaque fois que vous ajoutez ou supprimez cette propriété, mais également déclencher vos propres événements [**SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) ou [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) en fonction de votre type de scénario, tel qu’expliqué plus haut dans cet article.

### <a name="html-implementation"></a>Implémentation HTML  
Si vous utilisez les contrôles intrinsèques du format HTML, l’implémentation UIA a d’ores et déjà été mappée. Vous trouverez ci-dessous un exemple d’une implémentation déjà connectée:

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 Si vous créez vos propres contrôles, vous devez définir vos propres contrôles ARIA, décrits dans les normesW3C.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>Texte dans les graphiques  
Dans la mesure du possible, évitez d’inclure du texte dans un graphique. Par exemple, tout texte que vous placez dans le fichier source d’image et qui est affiché dans l’application en tant qu’élément [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) n’est pas automatiquement accessible ou lisible par les technologies d’assistance. Si vous devez utiliser du texte dans des graphiques, assurez-vous que la valeur [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) que vous fournissez comme équivalent de alt text comprend ce texte ou un résumé de la signification du texte. Des considérations semblables s’appliquent si vous créez des caractères texte à partir de vecteurs dans le cadre d’un objet [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) ou à l’aide de [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921).

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size"></a>Modification de la taille des polices  
Beaucoup de lecteurs ont du mal à lire le texte d’une application quand celui-ci utilise une taille de police trop petite. Vous pouvez éviter que ce problème ne se produise en faisant en sorte que la police du texte de l’interface utilisateur de votre application soit suffisamment grande. Windows propose également certaines technologies d’assistance, qui permettent aux utilisateurs de modifier la taille de l’affichage des applications, ou de l’affichage en général.

* Certains utilisateurs modifient les valeurs de haute résolution de leur affichage principal dans le cadre de leurs options d’accessibilité. Cette option est disponible dans la zone **Agrandir les éléments affichés à l’écran** de la fenêtre **Options d’ergonomie**, qui redirige l’utilisateur vers une interface utilisateur du **Panneau de configuration** pour les options **Apparence et personnalisation** / **Affichage**. Les options de dimensionnement réellement disponibles varient, car elles dépendent des caractéristiques de l’appareil d’affichage.
* La Loupe peut agrandir une zone sélectionnée de l’interface utilisateur. Cependant, la Loupe est difficile à utiliser pour lire un texte.

<span id="Text_scale_factor"/>
<span id="text_scale_factor"/>
<span id="TEXT_SCALE_FACTOR"/>

## <a name="text-scale-factor"></a>Facteur d’échelle de police  
Les différents contrôles et éléments de texte ont une propriété [**IsTextScaleFactorEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.istextscalefactorenabled). La valeur par défaut de cette propriété est **true**. Lorsque sa valeur est **true**, le paramètre appelé **Mise à l’échelle du texte** sur le téléphone (**Paramètres &gt; Options d’ergonomie**) entraîne l’agrandissement de la taille du texte dans l’élément concerné. La mise à l’échelle affecte davantage le texte pour lequel la valeur **FontSize** est faible que le texte pour lequel la valeur **FontSize** est élevée. Vous pouvez toutefois désactiver cet agrandissement automatique en définissant la propriété **IsTextScaleFactorEnabled** d’un élément sur **false**. Essayez ce balisage, ajustez le paramètre **Taille du texte** sur le téléphone, puis observez les éléments **TextBlock**:

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

Cependant, ne désactivez pas l’agrandissement automatique systématiquement, car la mise à l’échelle du texte de l’interface utilisateur à travers toutes les applications constitue une expérience d’accessibilité importante pour les utilisateurs qui s’attendent à ce qu’elle fonctionne aussi dans votre application.

Vous pouvez également utiliser l’événement [**TextScaleFactorChanged**](https://msdn.microsoft.com/library/windows/apps/Dn633867) et la propriété [**TextScaleFactor**](https://msdn.microsoft.com/library/windows/apps/Dn633866) pour évaluer les incidences sur le paramètre **Taille du texte** sur le téléphone. Voici comment:

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

La valeur de **TextScaleFactor** est un double appartenant à la plage \[1,2\]. Le texte le plus petit subit un agrandissement de cette ampleur. Vous pouvez par exemple utiliser la valeur pour adapter des éléments graphiques au texte. Gardez toutefois à l’esprit que tout le texte n’est pas mis à l’échelle selon le même facteur. En règle générale, plus la taille du texte initial est élevée, moins le texte est affecté par la mise à l’échelle.

Les types suivants possèdent une propriété **IsTextScaleFactorEnabled** :  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) et classes dérivées
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [**TextElement**](https://msdn.microsoft.com/library/windows/apps/BR209967) et classes dérivées

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [Informations d’accessibilité élémentaires](basic-accessibility-information.md)
* [Exemple d’affichage de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=238579)
* [Exemple de modification de texte XAML](http://go.microsoft.com/fwlink/p/?linkid=251417)
* [Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570) 
