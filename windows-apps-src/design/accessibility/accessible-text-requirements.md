---
Description: Cette rubrique décrit les meilleures pratiques relatives à l’accessibilité du texte dans une application, en garantissant que les couleurs et de l’arrière-plan respectent le coefficient de contraste nécessaire.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: Exigences de texte accessible
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3f474ec0c3017c3834d3eadb6f1caa989fc188a7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653334"
---
# <a name="accessible-text-requirements"></a>Exigences de texte accessible  




Cette rubrique décrit les meilleures pratiques relatives à l’accessibilité du texte dans une application, en garantissant que les couleurs et de l’arrière-plan respectent le coefficient de contraste nécessaire. Elle traite également des rôles Microsoft UI Automation que peuvent avoir les éléments de texte dans une application UWP et des meilleures pratiques relatives au texte des graphiques.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>Coefficients de contraste  
Bien que les utilisateurs aient toujours la possibilité de basculer en mode de contraste élevé, la conception de votre application en ce qui concerne le texte doit considérer cette option comme un dernier recours. L’idéal consiste à s’assurer que le texte de votre application remplit certains critères établis quant au niveau de contraste entre le texte et son arrière-plan. L’évaluation du niveau de contraste est basée sur des techniques déterministes qui ne prennent pas en compte la teinte. Par exemple, si vous avez du texte rouge sur fond vert, ce texte risque de ne pas être lisible pour quelqu’un souffrant de daltonisme. La vérification et la correction du coefficient de contraste peuvent éliminer ce genre de problème d’accessibilité.

Les recommandations pour le contraste texte décrits ici sont basées sur une accessibilité web standard, [G18 : Assurant un rapport de contraste d’au moins 4.5:1 existe entre le texte (et les images de texte) et de l’arrière-plan derrière le texte](https://go.microsoft.com/fwlink/p/?linkid=221823). Ces conseils se trouvent dans la spécification *Techniques W3C pour WCAG 2.0*.

Pour être considéré comme accessible, le texte visible doit présenter un coefficient de contraste de luminosité minimal de 4,5 pour 1 par rapport à l’arrière-plan. Les exceptions comprennent les logos et le texte accessoire tel que le texte qui fait partie d’un composant d’interface utilisateur inactif.

Le texte décoratif et qui ne véhicule aucune information est exclu. Par exemple, si des mots aléatoires sont utilisés pour créer un arrière-plan, et que ces mots peuvent être réorganisés ou remplacés sans changer le sens, ils sont considérés comme étant décoratifs et n’ont pas besoin de répondre à ce critère.

Utilisez des outils de contraste des couleurs pour vérifier que le coefficient de contraste du texte visible est acceptable. Pour connaître les outils permettant de tester les coefficients de contraste, voir la spécification [Techniques for WCAG 2.0 G18 (section Resources)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Certains outils répertoriés par la spécification Techniques for WCAG 2.0 G18 ne peuvent pas être utilisés de manière interactive avec une application UWP. Vous pouvez être amené à saisir manuellement des valeurs de couleur de premier plan et d’arrière-plan dans l’outil, ou à effectuer des captures d’écran de l’interface utilisateur de l’application, puis à exécuter l’outil de coefficient de contraste sur l’image de capture d’écran.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>Rôles d’éléments de texte  
Une application UWP peut utiliser les éléments par défaut suivants (couramment appelés *éléments de texte* ou *contrôles d’édition de texte*) :

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652): rôle est [ **texte**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**Zone de texte**](https://msdn.microsoft.com/library/windows/apps/BR209683): rôle est [ **modifier**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichTextBlock** ](https://msdn.microsoft.com/library/windows/apps/BR227565) (et overflow classe [ **RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.richtextblockoverflow)) : le rôle est [ **texte**](https://msdn.microsoft.com/library/windows/apps/BR209182)
* [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/BR227548): rôle est [ **modifier**](https://msdn.microsoft.com/library/windows/apps/BR209182)

Quand un contrôle signale qu’il a le rôle [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182), les technologies d’assistance supposent qu’il existe un ou plusieurs moyens pour l’utilisateur de modifier les valeurs. Par conséquent, si vous placez du texte statique dans un objet [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683), vous signalez de manière incorrecte le rôle et donc la structure de votre application à l’utilisateur d’accessibilité.

Dans les modèles de texte pour XAML, deux éléments sont principalement utilisés pour le texte statique : [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) et [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565). Comme aucun de ces éléments n’est une sous-classe [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390), aucun des deux ne peut être actif via le clavier ou apparaître dans l’ordre de tabulation. Toutefois, cela ne signifie pas que les technologies d’assistance ne peuvent pas ou ne veulent pas les lire. Les lecteurs d’écran sont généralement conçus pour prendre en charge plusieurs modes de lecture du contenu dans une application, notamment un mode de lecture dédié ou des modèles de navigation qui vont au-delà du focus et de l’ordre de tabulation, comme un « curseur virtuel ». Par conséquent, ne placez pas votre texte statique dans des conteneurs pouvant être actifs simplement pour que l’ordre de tabulation y amène l’utilisateur. Les utilisateurs des technologies d’assistance s’attendent à ce que tous les éléments inclus dans l’ordre de tabulation soient interactifs. Par conséquent, le fait d’y rencontrer du texte statique peut s’avérer plus déroutant qu’utile. Nous vous recommandons de tester cette fonction vous-même, via le Narrateur, afin de vous faire une idée de l’expérience utilisateur liée à votre application lorsque vous utilisez le lecteur d’écran pour examiner le texte statique de cette dernière.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>Accessibilité de suggestion automatique  
Lorsqu’un utilisateur effectue une saisie dans un champ d’entrée et qu’une liste de suggestions potentielles apparaît, il s’agit d’une suggestion automatique. Ce scénario est courant dans le champ **À :** d’un message, dans la zone de recherche Cortana de Windows, dans le champ d’entrée d’URL de Microsoft Edge, dans le champ d’entrée de situation géographique de l’application Météo, etc. Si vous utilisez la fonction XAML [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox) ou les commandes intrinsèques HTML, cette expérience est d’ores et déjà raccordée par défaut. Pour la rendre accessible, le champ d’entrée et la liste doivent être associés. Cette procédure est expliquée dans la section [Implémentation de la suggestion automatique](#implementing_auto-suggest).

La Narrateur a été mis à jour pour rendre cette expérience accessible avec un mode spécial de suggestions. À un niveau élevé, quand le champ d’entrée et la liste sont associés de manière appropriée, l’utilisateur final :

* Aura connaissance de la liste et du moment de clôture de cette dernière
* Aura connaissance du nombre de suggestions disponibles
* Aura connaissance de l’élément sélectionné, le cas échéant
* Sera en mesure de déplacer le focus du Narrateur de la liste
* Sera en mesure de parcourir une suggestion avec l’ensemble des autres modes de lecture

![Liste de suggestions](images/autosuggest-list.png)<br/>
_Exemple d’une liste de suggestions_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>Implémentation de la suggestion automatique  
Pour rendre cette expérience accessible, le champ d’entrée et la liste doivent être associés dans l’arborescence UIA. Cette association est effectuée avec la propriété [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) des applications de bureau ou la propriété [ControlledPeers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) des applications UWP.

À un niveau élevé, il existe 2 types d’expériences de suggestion automatique.

**Sélection par défaut**  
Si une sélection par défaut est effectuée dans la liste, le Narrateur recherche un événement [**UIA_SelectionItem_ElementSelectedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223) dans l’application de bureau, ou l’événement [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) à déclencher dans une application UWP. Chaque fois que la sélection est modifiée, quand un utilisateur saisit une autre lettre et que les suggestions ont été mises à jour et quand un utilisateur parcourt la liste, l’événement **ElementSelected** doit être déclenché.

![Liste avec une sélection par défaut](images/autosuggest-default-selection.png)<br/>
_Exemple où il existe une sélection par défaut_

**Aucune sélection par défaut**  
En cas d’absence de sélection par défaut, comme dans la zone de situation géographique de l’application Météo, le Narrateur recherche l’événement [**UIA_LayoutInvalidatedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223 ) de bureau ou l’événement UWP [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) à déclencher sur la liste à chaque mise à jour de cette dernière.

![Liste sans aucune sélection par défaut](images/autosuggest-no-default-selection.png)<br/>
_Exemple où il n’existe aucune sélection par défaut_

### <a name="xaml-implementation"></a>Implémentation XAML  
Si vous utilisez la fonction XAML par défaut [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox), l’ensemble des connexions sont déjà effectuées. Si vous développez votre propre expérience de suggestion automatique à l’aide d’un élément [**TextBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox) et d’une liste, vous devrez définir la liste en tant que [**AutomationProperties.ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) sur l’élément **TextBox**. Vous devez déclencher l’événement **AutomationPropertyChanged** de la propriété [**ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) chaque fois que vous ajoutez ou supprimez cette propriété, mais également déclencher vos propres événements [**SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) ou [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) en fonction de votre type de scénario, tel qu’expliqué plus haut dans cet article.

### <a name="html-implementation"></a>Implémentation HTML  
Si vous utilisez les contrôles intrinsèques du format HTML, l’implémentation UIA a d’ores et déjà été mappée. Vous trouverez ci-dessous un exemple d’une implémentation déjà connectée :

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 Si vous créez vos propres contrôles, vous devez définir vos propres contrôles ARIA, décrits dans les normes W3C.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>Texte dans les graphiques

Dans la mesure du possible, évitez d’inclure du texte dans un graphique. Par exemple, tout texte que vous placez dans le fichier source d’image et qui est affiché dans l’application en tant qu’élément [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) n’est pas automatiquement accessible ou lisible par les technologies d’assistance. Si vous devez utiliser du texte dans des graphiques, assurez-vous que la valeur [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) que vous fournissez comme équivalent de alt text comprend ce texte ou un résumé de la signification du texte. Des considérations semblables s’appliquent si vous créez des caractères texte à partir de vecteurs dans le cadre d’un objet [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) ou à l’aide de [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921).

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>Mise à l’échelle et la taille de police de texte

Les utilisateurs peuvent avoir des difficultés à lire le texte dans une application l’utilise de polices sont tout simplement trop petite, veillez à n’importe quel texte dans votre application a une taille raisonnable en premier lieu.

Une fois que vous avez terminé l’évident, Windows inclut divers outils d’accessibilité et les paramètres que les utilisateurs peuvent tirer parti et ajuster à leurs propres besoins et les préférences pour la lecture de texte. Par exemple :

* L’outil Loupe, qui agrandit une zone sélectionnée de l’interface utilisateur. Vous devez vous assurer de que la disposition du texte dans votre application ne soit pas difficile à utiliser la loupe pour la lecture.
* Paramètres globaux de mise à l’échelle et la résolution dans **Paramètres -> système -> Affichage -> mise à l’échelle et la disposition**. Exactement quelles options de dimensionnement sont disponibles peuvent varier car cela dépend des capacités du périphérique d’affichage.
* Paramètres de taille de texte dans **Paramètres -> Options d’ergonomie -> affichage**. Ajuster la **agrandir le texte** paramètre pour spécifier uniquement la taille du texte dans la prise en charge des contrôles dans l’ensemble des applications et des écrans (tous les contrôles de texte UWP prend en charge le texte de l’expérience sans toute personnalisation ou la création de modèles de mise à l’échelle). 
> [!NOTE]
> Le **vérifier tout plus volumineuses** paramètre permet à un utilisateur de spécifier leur taille par défaut pour le texte et les applications en général sur leur écran principal uniquement.

Les différents contrôles et éléments de texte ont une propriété [**IsTextScaleFactorEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled). La valeur par défaut de cette propriété est **true**. Lorsque **true**, la taille du texte dans cet élément peut être mis à l’échelle. La mise à l’échelle affecte le texte qui a un petit **FontSize** à un niveau plus élevé que cela affecte le texte qui a un grand **FontSize**. Vous pouvez désactiver le redimensionnement automatique en définissant un élément **IsTextScaleFactorEnabled** propriété **false**. 

Consultez [mise à l’échelle du texte](https://docs.microsoft.com/windows/uwp/design/input/text-scaling) pour plus d’informations.

Ajoutez le balisage suivant à une application et exécutez-le. Ajuster la **taille du texte** définition et observez le résultat à chaque **TextBlock**.

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

Nous ne recommandons pas que vous désactivez la mise à l’échelle de texte comme texte de l’interface utilisateur mise à l’échelle universellement dans toutes les applications est une expérience d’accessibilité important pour les utilisateurs.

Vous pouvez également utiliser l’événement [**TextScaleFactorChanged**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) et la propriété [**TextScaleFactor**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) pour évaluer les incidences sur le paramètre **Taille du texte** sur le téléphone. Voici comment :

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

La valeur de **TextScaleFactor** est un double de la plage \[1,2.25\]. Le texte le plus petit subit un agrandissement de cette ampleur. Vous pouvez par exemple utiliser la valeur pour adapter des éléments graphiques au texte. Gardez toutefois à l’esprit que tout le texte n’est pas mis à l’échelle selon le même facteur. En règle générale, plus la taille du texte initial est élevée, moins le texte est affecté par la mise à l’échelle.

Les types suivants possèdent une propriété **IsTextScaleFactorEnabled** :  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [**Contrôle** ](https://msdn.microsoft.com/library/windows/apps/BR209390) et classes dérivées
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [**TextElement** ](https://msdn.microsoft.com/library/windows/apps/BR209967) et classes dérivées

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  

* [Mise à l’échelle du texte](https://docs.microsoft.com/windows/uwp/design/input/text-scaling)
* [Accessibilité](accessibility.md)
* [Informations d’accessibilité de base](basic-accessibility-information.md)
* [Exemple d’Affichage de texte XAML](https://go.microsoft.com/fwlink/p/?linkid=238579)
* [Exemple de modification de texte XAML](https://go.microsoft.com/fwlink/p/?linkid=251417)
* [Exemple d’accessibilité XAML](https://go.microsoft.com/fwlink/p/?linkid=238570) 
