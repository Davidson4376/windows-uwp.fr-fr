---
Description: Nous lisons du texte en permanence dans notre vie quotidienne : nos e-mails, un livre, un panneau routier, les prix sur un menu, la pression des pneus ou des affiches sur des poteaux.
title: Contrôles de texte
ms.assetid: 43DC68BF-FA86-43D2-8807-70A359453048
label: Contrôles de texte
template: detail.hbs
---
# Contrôles de texte
Les contrôles de texte comprennent les zones de saisie de texte, les zones de mot de passe, les zones de suggestion automatique et les blocs de texte. L’infrastructure XAML propose plusieurs contrôles de rendu, de saisie et de modification de texte, ainsi qu’un jeu de propriétés de mise en forme du texte.

- Les contrôles d’affichage de texte en lecture seule sont [TextBlock](text-block.md) et [RichTextBlock](rich-text-block.md).
- Les contrôles de saisie et de modification de texte sont : [TextBox](text-block.md), [AutoSuggestBox](auto-suggest-box.md), [PasswordBox](password-box.md) et [RichEditBox](rich-edit-box.md). 


<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Classe AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)
-   [**Classe PasswordBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)
-   [**Classe RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)
-   [**Classe RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)
-   [**Classe TextBlock**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)
-   [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx)

## Est-ce le contrôle approprié ?

Le contrôle de texte utilisé dépend du scénario. Utilisez les informations fournies dans cet article pour sélectionner le contrôle de texte adapté à votre application.

### Afficher un texte en lecture seule

Pour afficher la majeure partie de votre texte en lecture seule dans votre application, utilisez un contrôle **TextBlock**. Ce contrôle vous permet d’afficher une ou plusieurs lignes de texte, des liens hypertexte inclus et du texte avec mise en forme de type gras, italique ou souligné.

Le contrôle TextBlock est généralement plus facile à utiliser et offre de meilleures performances en termes de rendu de texte que le contrôle RichTextBlock. C’est la raison pour laquelle il est recommandé pour la plupart des textes d’interface utilisateur d’application. Vous pouvez facilement visualiser et utiliser le texte d’un contrôle TextBlock dans votre application en obtenant la valeur de la propriété [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx). 

Ce contrôle propose également de nombreuses options de mise en forme similaires pour personnaliser le mode de rendu de votre texte. Même si vous pouvez insérer des sauts de ligne dans le texte, TextBlock est conçu pour n’afficher qu’un seul paragraphe et ne prend pas en charge le retrait du texte.

Utilisez un contrôle **RichTextBlock** si vous devez prendre en charge plusieurs paragraphes, du texte sur plusieurs colonnes, d’autres dispositions de texte complexes ou des éléments d’interface utilisateur inclus, tels que des images. RichTextBlock propose plusieurs fonctionnalités avancées de disposition du texte.

La propriété de contenu de RichTextBlock est la propriété [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx), qui prend en charge du texte basé sur un paragraphe par le biais de l’élément [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx). Elle ne présente pas de propriété **Text** vous permettant d’accéder facilement au contenu de texte du contrôle dans votre application.  

### Saisie de texte

Utilisez un contrôle **TextBox** si vous souhaitez permettre à un utilisateur de saisir et de modifier du texte sans mise en forme, dans un formulaire par exemple. Vous pouvez utiliser la propriété Text pour obtenir et définir le texte d’un contrôle TextBox.

Vous avez la possibilité de définir un contrôle TextBox en lecture seule, mais il doit s’agir d’un état conditionnel temporaire. Si le texte ne doit jamais être modifiable, envisagez plutôt d’utiliser un contrôle TextBlock.

Si vous souhaitez recueillir un mot de passe ou d’autres données confidentielles, telles qu’un numéro de sécurité sociale, utilisez un contrôle **PasswordBox**. Une zone de mot de passe est une zone de saisie de texte qui masque les caractères entrés par souci de confidentialité. Une zone de mot de passe ressemble à une zone de saisie de texte, à la différence qu’elle affiche des puces à la place du texte entré. Le caractère puce est personnalisable.

Pour présenter à l’utilisateur une liste de suggestions dans laquelle il peut effectuer un choix pendant qu’il saisit du texte, utilisez un contrôle **AutoSuggestBox**. Une zone de suggestion automatique est une zone de texte qui déclenche une liste de suggestions de recherche de base. Les termes suggérés peuvent provenir d’une combinaison de termes de recherche courants et de termes entrés précédemment par l’utilisateur.

Vous devez également utiliser un contrôle AutoSuggestBox pour implémenter une zone de recherche.

Pour afficher et modifier des fichiers de texte, utilisez un contrôle **RichEditBox**. Vous n’utilisez pas un contrôle RichEditBox pour obtenir les données entrées dans votre application par l’utilisateur de la même façon que vous utilisez d’autres zones de saisie de texte standard. En fait, ce contrôle vous permet de travailler sur des fichiers de texte séparés de votre application. Le texte entré dans un contrôle RichEditBox est généralement enregistré dans un fichier .rtf.

**La saisie de texte est-elle la meilleure option ?**

Vous disposez de nombreuses méthodes pour obtenir les entrées des utilisateurs de votre application. Les questions suivantes vous aideront à déterminer si le meilleur moyen d’obtenir les entrées utilisateur consiste à utiliser l’une des zones de saisie de texte standard ou un autre contrôle.

-   **Toutes les valeurs valides peuvent-elles être efficacement énumérées ?** Si tel est le cas, envisagez d’utiliser les contrôles de sélection, par exemple une [case à cocher](checkbox.md), une [liste déroulante](lists.md), une zone de liste, une [case d’option](radio-button.md), un [curseur](slider.md), un [bouton bascule](toggles.md), un [sélecteur de dates](date-and-time.md) ou un sélecteur d’heure.
-   **L’ensemble de valeurs valides est-il restreint ?** Si c’est le cas, utilisez plutôt une [liste déroulante](lists.md) ou une zone de liste, en particulier si les valeurs contiennent un grand nombre de caractères.
-   **Les données valides sont-elles totalement sans contraintes ? Ou sont-elles uniquement contraintes par le format (contraintes de longueur ou de types de caractères) ?** Si tel est le cas, utilisez un contrôle de saisie de texte. Vous pouvez limiter le nombre de caractères pouvant être entrés, et valider le format dans le code de votre application.
-   **La valeur représente-t-elle un type de données auquel correspond un contrôle courant spécialisé ?** Si c’est le cas, utilisez le contrôle approprié plutôt que le contrôle de saisie de texte. Utilisez, par exemple, un contrôle [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681) plutôt qu’un contrôle de saisie de texte pour les saisies de date.
-   Si les données sont strictement numériques :
    -   **La valeur à entrer est-elle approximative et/ou relative à une autre quantité spécifiée sur la même page ?** Si c’est le cas, utilisez un [curseur](slider.md).
    -   **L’utilisateur profitera-t-il du résultat instantané des modifications de ses paramètres ?** Si c’est le cas, utilisez un [curseur](slider.md) accompagné éventuellement d’un contrôle correspondant.
    -   **La valeur entrée risque-t-elle d’être ajustée en fonction du résultat obtenu (par exemple, le volume ou la luminosité) ?** Dans ce cas, utilisez un [curseur](slider.md).
    
## Exemples

Zone de texte

![Zone de texte](images/text-box.png)

Zone de suggestion automatique

![Exemple de contrôle de suggestion automatique développé](images/controls_autosuggest_expanded01.png)

Zone de mot de passe

![État de focus de la zone de mot de passe pendant la saisie](images/passwordbox-focus-typing.png)

## Créer un contrôle de texte

Pour plus d’informations et d’exemples propres à chaque contrôle de texte, voir les articles suivants.

-   [**AutoSuggestBox**](auto-suggest-box.md)
-   [**PasswordBox**](password-box.md)
-   [**RichEditBox**](rich-edit-box.md)
-   [**RichTextBlock**](rich-text-block.md)
-   [**TextBlock**](text-block.md)
-   [**TextBox**](text-box.md)

## Recommandations en matière de polices et de styles
Pour découvrir les recommandations concernant les polices, voir les articles suivants :

- [**Recommandations en matière de polices**](fonts.md)
- [**Recommandations en matière d’icônes Segoe MDL2**](segoe-ui-symbol-font.md)


## Choisir le clavier adapté à votre contrôle de texte

**S’applique aux contrôles :** TextBox, PasswordBox, RichEditBox

Pour faciliter la saisie de données par les utilisateurs au moyen du clavier tactile, ou clavier virtuel, définissez l’étendue des entrées du contrôle de texte de façon qu’elle corresponde au type de données attendu par l’utilisateur.

>Conseil 
>Ces informations s’appliquent uniquement au clavier virtuel. Elles ne concernent pas les claviers matériels ou visuels figurant dans les Options d’ergonomie de Windows.

Le clavier tactile permet d’entrer du texte lorsque l’application est exécutée sur un appareil disposant d’un écran tactile. Le clavier tactile est appelé lorsque l’utilisateur appuie sur un champ d’entrée modifiable, tel qu’un contrôle TextBox ou RichEditBox. Vous pouvez nettement faciliter et accélérer la saisie de données par les utilisateurs dans votre application en définissant l’étendue des entrées du contrôle de texte afin qu’elle corresponde au type de données attendu de la part de l’utilisateur. L’étendue des entrées fournit au système une indication sur le type d’entrée de texte attendu par le contrôle, afin que le système puisse fournir une disposition de clavier tactile spécialisée pour le type d’entrée.

Par exemple, si une zone de texte est utilisée uniquement pour la saisie d’un code confidentiel à 4 chiffres, définissez la propriété [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) sur **Number**. Cela indique au système qu’il doit afficher la disposition du pavé numérique, ce qui facilite la saisie d’un code confidentiel par l’utilisateur.

>Important  
>L’étendue des entrées n’entraîne l’exécution d’aucune validation des entrées et n’empêche pas l’utilisateur de saisir des données par le biais d’un clavier matériel ou d’un autre dispositif du même ordre. Vous restez responsable de la validation d’une entrée dans votre code, si nécessaire.

Pour plus d’informations, voir l’article Utiliser l’étendue des entrées pour modifier le clavier tactile.

## Polices en couleur

**S’applique aux contrôles :** TextBlock, RichTextBlock, TextBox, RichEditBox

Les polices Windows peuvent inclure plusieurs couches colorées pour chaque glyphe. Par exemple, la police Segoe UI Emoji définit les versions de couleur des émoticônes et des autres caractères Emoji. 

Les contrôles de texte standard et enrichi prennent en charge l’affichage des polices en couleur. Par défaut, la propriété **IsColorFontEnabled** est définie sur **true**, et les polices dotées de ces couches supplémentaires s’affichent donc en couleur. La police en couleur par défaut sur le système est Segoe UI Emoji, et les contrôles utiliseront cette police pour afficher les glyphes en couleur. 

```xaml
<TextBlock FontSize="30">Hello ☺⛄☂♨⛅</TextBlock>
```

Le texte affiché ressemble à ceci :

![Bloc de texte avec police en couleur](images/text-block-color-fonts.png)

Pour plus d’informations, voir la propriété [**IsColorFontEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.iscolorfontenabled.aspx).

## Recommandations en matière de séparateurs de lignes et de paragraphes

**S’applique aux contrôles :** TextBlock, RichTextBlock, TextBox multiligne, RichEditBox

Pour diviser un texte brut, utilisez le caractère séparateur de lignes (0x2028) et le caractère séparateur de paragraphes (0x2029). Une ligne est créée après chaque séparateur de lignes. Un paragraphe est créé après chaque séparateur de paragraphes.

Il n’est pas nécessaire d’insérer ces caractères au début de la première ligne ou du premier paragraphe d’un fichier, ni à la fin de la dernière ligne ou du dernier paragraphe ; cette opération signalerait l’existence d’une ligne vide ou d’un paragraphe vide à cet emplacement.

Votre application peut utiliser le séparateur de lignes pour indiquer une fin de ligne inconditionnelle. Toutefois, les séparateurs de lignes ne correspondent pas aux caractères distincts de retour chariot et de saut de ligne, ni à une combinaison de ces caractères. Les séparateurs de lignes doivent être traités séparément des caractères de saut de ligne et de retour chariot.

Votre application peut insérer un séparateur de paragraphes entre les paragraphes du texte. L’utilisation de ce séparateur vous permet de créer des fichiers de texte brut qui peuvent être mis en forme avec des largeurs de ligne distinctes sur différents systèmes d’exploitation. Le système cible peut ignorer les séparateurs de lignes et interrompre les paragraphes uniquement au niveau des séparateurs de paragraphes.

\[Cet article contient des informations propres aux applications de plateforme Windows universelle (UWP) et à Windows 10. Pour obtenir de l’aide concernant Windows 8.1, téléchargez le [document PDF de recommandations pour Windows 8.1](https://go.microsoft.com/fwlink/p/?linkid=258743) (en anglais).\]

## Articles connexes

**Pour les concepteurs**
- [**Recommandations en matière de polices**](fonts.md)
- [**Recommandations en matière d’icônes Segoe MDL2**](segoe-ui-symbol-font.md)
- [Recommandations en matière de vérification orthographique](spell-checking-and-prediction.md)
- [Ajout de la fonctionnalité de recherche](https://msdn.microsoft.com/library/windows/apps/hh465231)

**Pour les développeurs (XAML)**
- [**Classe TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Classe PasswordBox Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propriété String.Length](https://msdn.microsoft.com/library/system.string.length.aspx)
<!--HONumber=Mar16_HO1-->
