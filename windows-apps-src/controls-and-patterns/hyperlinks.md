---
author: Jwmsft
Description: "Les liens hypertexte redirigent l’utilisateur vers une autre partie de l’application ou une autre application, ou lancent un URI spécifique dans une application de navigateur distincte."
title: Liens hypertexte
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.openlocfilehash: 5079d1782188b6d2e49fc14741a23a5651995c67
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="hyperlinks"></a>Liens hypertexte

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Les liens hypertexte redirigent l’utilisateur vers une autre partie de l’application ou une autre application, ou lancent un URI spécifique dans une application de navigateur distincte. Vous pouvez ajouter un lien hypertexte à une application XAML de deux façons : à l’aide d’un élément de texte **Hyperlink** ou d’un contrôle **HyperlinkButton**.

> **API importantes**: [élément de texte Hyperlink](https://msdn.microsoft.com/library/windows/apps/dn279356), [contrôle HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739)

![Bouton Lien hypertexte](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez un lien hypertexte lorsque vous avez besoin de texte qui réponde lorsqu’il est sélectionné et dirige l’utilisateur vers plus d’informations.

Choisissez le type de lien hypertexte approprié en fonction de vos besoins:

-   Utilisez un élément de texte **Hyperlink** inclus dans un contrôle de texte. Un élément Hyperlink comprend d’autres éléments de texte et vous pouvez l’utiliser dans toute collection InlineCollection. Utilisez un lien hypertexte de type texte si vous souhaitez un saut de ligne automatique et si vous n’avez pas nécessairement besoin d’une cible large. Le texte du lien hypertexte peut être petit et difficile à cibler, notamment pour les fonctions tactiles.
-   Utilisez un contrôle **HyperlinkButton** pour les liens hypertexte autonomes. Un contrôle HyperlinkButton est un contrôle Button spécialisé que vous pouvez utiliser à tout endroit où vous utiliseriez un bouton.
-   Utilisez un contrôle **HyperlinkButton** avec un élément [Image](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) comme contenu pour créer une image sur laquelle vous pouvez cliquer.

## <a name="create-a-hyperlink-text-element"></a>Créer un élément de texte Hyperlink

Cet exemple montre comment utiliser un élément de texte Hyperlink à l’intérieur d’un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx).

```xml
<StackPanel Width="200">
    <TextBlock Text="Privacy" Style="{StaticResource SubheaderTextBlockStyle}"/>
    <TextBlock TextWrapping="WrapWholeWords">
        <Span xml:space="preserve"><Run>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Read the </Run><Hyperlink NavigateUri="http://www.contoso.com">Contoso Privacy Statement</Hyperlink><Run> in your browser.</Run> Donec pharetra, enim sit amet mattis tincidunt, felis nisi semper lectus, vel porta diam nisi in augue.</Span>
    </TextBlock>
</StackPanel>

```
Le lien hypertexte s’affiche en ligne avec le texte qui l’entoure:

![Exemple de lien hypertexte en tant qu’élément de texte](images/controls_hyperlink-element.png) 

> **Conseil**&nbsp;&nbsp;Quand vous utilisez un élément Hyperlink dans un contrôle de texte avec d’autres éléments de texte en XAML, placez le contenu dans un conteneur [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) et appliquez l’attribut `xml:space="preserve"` au conteneur Span pour conserver l’espace blanc entre l’élément Hyperlink et les autres éléments.

## <a name="create-a-hyperlinkbutton"></a>Créer un HyperlinkButton

Voici comment utiliser un HyperlinkButton, avec un texte et une image.

```xml
<StackPanel>
    <TextBlock Text="About" Style="{StaticResource TitleTextBlockStyle}"/>
    <HyperlinkButton NavigateUri="http://www.contoso.com">
        <Image Source="Assets/ContosoLogo.png"/>
    </HyperlinkButton>
    <TextBlock Text="Version: 1.0.0001" Style="{StaticResource CaptionTextBlockStyle}"/>
    <HyperlinkButton Content="Contoso.com" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Acknowledgments" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Help" NavigateUri="http://www.contoso.com"/>
</StackPanel>

```
Les boutons de lien hypertexte incluant du texte s’affichent en tant que texte marqué. L’image du logo Contoso est également un lien hypertexte sur lequel vous pouvez cliquer:

![Exemple de lien hypertexte en tant que contrôle de bouton](images/controls_hyperlink-button-image.png)

## <a name="handle-navigation"></a>Gérer la navigation

Pour les deux types de liens hypertexte, vous gérez navigation de la même manière. Vous pouvez définir la propriété **NavigateUri** ou gérez l’événement **Click**.

**Accéder à un URI**

Pour utiliser le lien hypertexte afin d’accéder à un URI, définissez la propriété NavigateUri. Lorsqu’un utilisateur clique ou appuie sur le lien hypertexte, l’URI spécifié s’ouvre dans le navigateur par défaut. Le navigateur par défaut s’exécute dans un processus distinct de votre application.

> [!NOTE]
> Vous n’êtes pas tenu d’utiliser les schémas http: ou https:. Vous pouvez utiliser des schémas tels que ms-appx:, ms-appdata: ou ms-ressources: si le contenu des ressources à ces emplacements peut être chargé dans un navigateur. Toutefois, le schéma file: est bloqué. Pour plus d’informations, voir [Schémas d’URI](https://msdn.microsoft.com/library/windows/apps/jj655406.aspx).

> Quand un utilisateur clique sur le lien hypertexte, la valeur de la propriété NavigateUri est transmise à un gestionnaire de système pour les schémas et types d’URI. Ensuite, le système lance l’application qui est inscrite pour le schéma d’URI fourni pour NavigateUri.

Si vous ne souhaitez pas que le lien hypertexte charge du contenu dans un navigateur web par défaut (et que vous ne voulez pas qu’un navigateur apparaisse), alors ne définissez pas de valeur pour NavigateUri. À la place, gérez l’événement Click et écrivez le code correspondant à ce que vous voulez faire.


**Gérer l’événement Click**

Utilisez l’événement Click pour les actions autres que le lancement d’un URI dans un navigateur, tel que la navigation au sein de l’application. Par exemple, si vous souhaitez charger une nouvelle page d’application plutôt que d’ouvrir un navigateur, appelez une méthode [Frame.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.navigate.aspx) au sein de votre gestionnaire d’événements Click pour naviguer vers la nouvelle page d’application. Si vous souhaitez qu’un URI absolu et externe effectue le chargement dans un contrôle [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) qui existe également dans votre application, appelez [WebView.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.navigate.aspx) dans le cadre de votre logique de gestionnaire d’événements Click.

En règle générale, vous n’avez pas à gérer l’événement Click et à spécifier de valeur pour NavigateUri en même temps, puisqu’il s’agit de deux méthodes distinctes d’utilisation de l’élément de lien hypertexte. Si vous souhaitez que l’URI s’ouvre dans le navigateur par défaut, et que vous avez spécifié une valeur pour NavigateUri, ne gérez pas l’événement Click. À l’inverse, si vous gérez l’événement Click, ne spécifiez pas de valeur pour NavigateUri.

Vous ne pouvez rien faire au niveau du gestionnaire d’événements Click pour empêcher le navigateur par défaut de charger toute cible valide spécifiée pour NavigateUri. En effet, cette action se déclenche automatiquement (de manière asynchrone) lorsque le lien hypertexte est activé et ne peut être annulée à partir du gestionnaire d’événements Click. 

## <a name="hyperlink-underlines"></a>Soulignement de lien hypertexte
Par défaut, les liens hypertexte sont soulignés. Ce trait de soulignement est important, car il permet de répondre aux exigences d’accessibilité. Les utilisateurs daltoniens utilisent le trait de soulignement pour distinguer les liens hypertexte du reste du texte. Si vous désactivez les traits de soulignement, pensez à ajouter un type de format spécial permettant de distinguer les liens hypertexte du reste du texte, tel que FontWeight ou FontStyle.

**Éléments de texte Hyperlink**

Vous pouvez définir la propriété [UnderlineStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.hyperlink.underlinestyle.aspx) pour désactiver le trait de soulignement. Si vous le désactivez, pensez à utiliser [FontWeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontweight.aspx) ou [FontStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontstyle.aspx) pour différencier le texte du lien du reste du texte.

**HyperlinkButton** 

Par défaut, l’élément HyperlinkButton s’affiche sous forme de texte souligné lorsque vous définissez une chaîne comme valeur pour la propriété [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx).

Le texte n’apparaît pas souligné dans les cas suivants:
- Si vous définissez un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) comme valeur pour la propriété Content et définissez la propriété [Text](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.text.aspx) sur TextBlock.
- Si vous redéfinissez le modèle de l’élément HyperlinkButton et modifiez le nom de la partie du modèle [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx).

Si vous avez besoin d’un bouton qui apparaît sous forme de texte non souligné, pensez à utiliser un contrôle Button standard et à appliquer la ressource du système `TextBlockButtonStyle` intégrée à sa propriété Style.

## <a name="notes-for-hyperlink-text-element"></a>Remarques concernant l’élément de texte Hyperlink

Cette section s’applique uniquement à l’élément de texte Hyperlink et non au contrôle HyperlinkButton.

**Événements d’entrée**

Dans la mesure où un élément Hyperlink n’est pas un [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx), il n’a pas l’ensemble des événements d’entrée d’élément d’interface utilisateur tels que Tapped, PointerPressed, etc. À la place, un élément Hyperlink possède son propre événement Click et le comportement implicite du système qui charge tout URI spécifié en tant que valeur pour NavigateUri. Le système gère toutes les actions d’entrée qui doivent appeler les actions Hyperlink et déclenche l’événement Click en réponse.

**Contenu**

L’élément Hyperlink a des restrictions sur le contenu qui peut exister dans sa collection [Inlines](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.inlines.aspx). Plus précisément, un élément Hyperlink autorise uniquement les types [Run](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.run.aspx) et d’autres types [Span]() qui ne sont pas un autre élément Hyperlink. [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.inlineuicontainer.aspx) ne peut pas être dans la collection Inlines d’un élément Hyperlink. Toute tentative d’ajout de contenu restreint lève une exception d’argument invalide ou une exception d’analyse XAML.

**Élément Hyperlink et comportement de thème/style**

L’élément Hyperlink n’hérite pas de [Control](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.aspx), il n’a donc pas de propriété [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.style.aspx) ni de [Template](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.template.aspx). Vous pouvez modifier les propriétés qui sont héritées de [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.aspx), tel que Foreground ou FontFamily, pour changer l’apparence d’un élément Hyperlink. Cependant, vous ne pouvez pas utiliser de style ou de modèle commun pour appliquer des modifications. Au lieu d’utiliser un modèle, pensez à utiliser des ressources communes pour les valeurs des propriétés de l’élément Hyperlink afin de garantir la cohérence. Certaines propriétés de l’élément Hyperlink utilisent par défaut la valeur d’extension de balisage {ThemeResource} fournie par le système. Cela permet à l’apparence de l’élément Hyperlink de s’adapter aux modifications du thème du système apportées par l’utilisateur au moment de l’exécution.

La couleur par défaut du lien hypertexte est la couleur d’accentuation du système. Vous pouvez définir la propriété [Foreground](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.foreground.aspx) pour remplacer celle-ci.

## <a name="recommendations"></a>Recommandations

-   Utilisez uniquement les liens hypertexte pour la navigation. Ne les utilisez pas pour d’autres actions.
-   Utilisez le style Body de la gamme de types pour les liens hypertexte de type texte. Voir [fonts and the Windows 10 type ramp](fonts.md).
-   Gardez les liens hypertexte suffisamment éloignés les uns des autres pour que l’utilisateur puisse les distinguer et les sélectionner aisément.
-   Ajoutez des info-bulles aux liens hypertexte pour indiquer à l’utilisateur l’emplacement vers lequel il va être dirigé. Si l’utilisateur est dirigé vers un site externe, insérez le nom de domaine de niveau supérieur dans l’info-bulle, puis appliquez un style au texte avec une couleur de police secondaire.

## <a name="related-articles"></a>Articles connexes

- [Contrôles de texte](text-controls.md)
- [Recommandations en matière d’info-bulles](tooltips.md)

**Pour les développeurs (XAML)**
- [Classe Windows.UI.Xaml.Documents.Hyperlink](https://msdn.microsoft.com/library/windows/apps/dn279356)
- [Classe Windows.UI.Xaml.Controls.HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739)
