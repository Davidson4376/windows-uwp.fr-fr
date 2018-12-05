---
Description: Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: Présenter des informations d’accessibilité élémentaires
label: Expose basic accessibility information
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e7526ec4f32f641f152709e6968f3dc442c2a06
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8703130"
---
# <a name="expose-basic-accessibility-information"></a>Présenter des informations d’accessibilité élémentaires  



Les informations d’accessibilité élémentaires sont souvent classées en trois catégories : nom, rôle et valeur. Cette rubrique décrit le code qui aide votre application à exposer les informations de base nécessaires aux technologies d’assistance.

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>Nom accessible  
Le nom accessible est une chaîne de texte courte et descriptive qui est utilisée par les lecteurs d’écran pour présenter un élément d’interface utilisateur. Définissez le nom accessible des éléments d’interface utilisateur dont la signification est importante pour comprendre le contenu ou interagir avec l’interface utilisateur. Ces éléments incluent généralement les images, les champs d’entrée, les boutons, les contrôles et les régions.

Le tableau suivant décrit comment définir ou obtenir un nom accessible pour différents types d’éléments dans une interface utilisateur XAML.

| Type d’élément | Description |
|--------------|-------------|
| Texte statique | Pour les éléments [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) et [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), un nom accessible est déterminé automatiquement à partir du texte visible (interne). Tout le texte dans cet élément est utilisé comme nom. Voir [Nom du texte interne](#name_from_inner_text). |
| Images | L’élément [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) XAML n’a pas d’analogue direct pour l’attribut HTML **alt** de type **img** et éléments similaires. Vous devez utiliser [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) pour fournir un nom ou utiliser la technique de légendage. Voir [Noms accessibles pour les images](#images). |
| Éléments de formulaire | Le nom accessible pour un élément de formulaire doit être identique à l’étiquette affichée pour cet élément. Voir [Étiquettes et LabeledBy](#labels). |
| Boutons et liens | Par défaut, le nom accessible d’un bouton ou d’un lien est basé sur le texte visible, les mêmes règles que celles décrites dans [Nom du texte interne](#name_from_inner_text) étant appliquées. Dans les cas où un bouton contient uniquement une image, utilisez [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) pour fournir un équivalent texte-uniquement de l’action prévue du bouton. |

La plupart des éléments de conteneur tels que les panneaux n’effectuent pas la promotion de leur contenu comme nom accessible, car c’est le contenu de l’élément qui doit indiquer un nom et un rôle correspondant, et non son conteneur. L’élément conteneur peut indiquer qu’il s’agit d’un élément ayant des enfants dans une représentation Microsoft UI Automation, de telle sorte que la logique de technologie d’assistance puisse le traverser. Toutefois, les informations sur les conteneurs ne sont généralement pas utiles aux utilisateurs de technologies d’assistance et la plupart des conteneurs ne sont pas nommés.

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>Rôle et valeur  
Les contrôles et autres éléments d’interface utilisateur qui font partie du vocabulaire XAML mettent en œuvre la prise en charge d’UI Automation pour signaler les rôles et les valeurs dans le cadre de leurs définitions. Vous pouvez utiliser des outils UI Automation pour examiner les informations sur les rôles et les valeurs pour les contrôles, ou vous pouvez lire la documentation relative aux implémentations [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) de chaque contrôle. Les rôles disponibles dans une infrastructure UI Automation sont définis dans l’énumération [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182). Les clients UI Automation tels que les technologies d’assistance peuvent obtenir des informations sur les rôles en appelant les méthodes que l’infrastructure UI Automation expose à partir de l’élément **AutomationPeer**du contrôle.

Les contrôles n’ont pas tous une valeur. Ceux qui en ont une signalent ces informations à UI Automation par le biais des homologues et modèles qui sont pris en charge par ce contrôle. Par exemple, un élément de formulaire [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) a bien une valeur. Une technologie d’assistance peut être un client UI Automation et découvrir par conséquent qu’il existe une valeur et quelle est cette valeur. Dans ce cas particulier, **TextBox** prend en charge le modèle [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) via les définitions [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550).

> [!NOTE]
> Dans les cas où vous utilisez [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) ou d’autres techniques pour fournir le nom accessible de manière explicite, n’incluez pas le même texte que celui utilisé par les informations de type ou de rôle de contrôle dans le nom accessible. Par exemple, n’incluez pas des chaînes telles que «bouton» ou «liste» dans le nom. Les informations de type et de rôle proviennent d’une propriété UI Automation différente (**LocalizedControlType**) qui est fournie par la prise en charge du contrôle par défaut pour UI Automation. De nombreuses technologies d’assistance ajoutent l’élément **LocalizedControlType** au nom accessible. Le fait de dupliquer le rôle dans le nom accessible peut donc provoquer une répétition de mots inutile. Par exemple, si vous attribuez à un contrôle [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) le nom accessible « bouton » ou incluez « bouton » à la fin du nom, les lecteurs d’écran risquent de lire « bouton bouton ». Nous vous conseillons de tester cet aspect de votre accessibilité à l’aide du Narrateur.

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>Influence sur les vues de l’arborescence UI Automation  
La conception de l’infrastructure UI Automation repose sur des arborescences, où les clients UIAutomation peuvent récupérer les relations entre les éléments d’une interface utilisateur à l’aide de trois affichages possibles: brut, de contrôle et de contenu. L’affichage de contrôle est souvent utilisé par les clients UI Automation, car il fournit une bonne représentation et une organisation efficace des éléments interactifs d’une interface utilisateur. Les outils de test vous permettent généralement de choisir l’arborescence à utiliser quand l’outil présente l’organisation des éléments.

Par défaut, toute classe dérivée [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) et quelques autres éléments apparaissent dans l’affichage de contrôle lorsque l’infrastructure UI Automation représente l’interface utilisateur pour une application UWP. Cependant, il se peut que vous ne souhaitiez pas qu’un élément apparaisse dans l’affichage de contrôle en raison de la composition de l’interface utilisateur, où cet élément duplique des informations ou présente des informations qui ne sont pas importantes pour les scénarios d’accessibilité. Utilisez la propriété jointe [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/Dn251788) pour modifier la façon dont les éléments sont exposés dans les arborescences. Si vous placez un élément dans l’arborescence **Raw**, la plupart des technologies d’assistance n’indiquent pas cet élément dans le cadre de leurs affichages. Pour voir quelques exemples de ce fonctionnement dans des contrôles existants, ouvrez le fichier XAML de référence de la conception generic.xaml dans un éditeur de texte et recherchez **AutomationProperties.AccessibilityView** dans les modèles.

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>Nom du texte interne  
Pour simplifier l’utilisation de chaînes qui existent déjà dans l’interface utilisateur visible pour les valeurs de noms accessibles, une grande partie des contrôles et autres éléments d’interface utilisateur permettent de déterminer automatiquement un nom accessible par défaut, en fonction du texte interne de l’élément ou de valeurs de chaînes de propriétés de contenu.

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652), [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) et **RichTextBlock** effectuent chacun la promotion de la valeur de la propriété **Text** comme nom accessible par défaut.
* Toute sous-classe [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) utilise une technique «ToString» itérative pour rechercher les chaînes dans sa valeur [**Content**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content) et effectue la promotion de ces chaînes comme nom accessible par défaut.

> [!NOTE]
> Comme l’impose UIAutomation, la longueur du nom accessible ne doit pas dépasser 2048caractères. Si une chaîne utilisée pour la détermination automatique du nom accessible dépasse cette limite, le nom accessible est tronqué à ce niveau.

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>Noms accessibles pour les images
Pour prendre en charge les lecteurs d’écran et pour fournir les informations d’identification de base pour chaque élément dans l’interface utilisateur, vous devez parfois spécifier des alternatives textuelles à des informations non textuelles telles que des images et des graphiques (à l’exclusion des éléments purement décoratifs ou structurels). Ces éléments n’ayant pas de texte interne, aucune valeur n’est calculée pour le nom accessible. Vous pouvez définir directement le nom accessible à l’aide de la propriété attachée [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770), comme Illustré dans l’exemple.

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

En guise d’alternative, vous pouvez inclure une légende qui s’affiche dans l’interface utilisateur visible et qui sert également d’informations d’accessibilité associées à l’étiquette pour le contenu de l’image. Voici un exemple:

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>Étiquettes et LabeledBy.  
Le meilleur moyen d’associer une étiquette à un élément de formulaire consiste à utiliser un objet [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) avec un **x:Name** comme texte d’étiquette, puis à définir la propriété jointe [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) sur l’élément de formulaire pour faire référence à l’étiquetage **TextBlock** par son nom XAML. Si vous utilisez ce modèle, quand l’utilisateur clique sur une étiquette, le focus bascule vers le contrôle associé et les technologies d’assistance peuvent utiliser le texte d’étiquette comme nom accessible pour le champ de formulaire. Voici un exemple qui illustre cette technique.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_FirstName">First name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_FirstName}"
      Name="tbFirstName" Width="100"/>
   </StackPanel>
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_LastName">Last name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_LastName}"
      Name="tbLastName" Width="100"/>
   </StackPanel>
 </StackPanel>
```

<span id="accessible_description"/>
<span id="ACCESSIBLE_DESCRIPTION"/>

## <a name="accessible-description-optional"></a>Description accessible (facultative)  
Une description accessible fournit des informations d’accessibilité supplémentaires sur un élément d’interface utilisateur particulier. Vous fournissez généralement une description accessible lorsqu’un nom accessible seul ne transmet pas de façon adéquate l’objectif d’un élément.

Le lecteur d’écran Narrateur lit la description accessible d’un élément uniquement lorsque l’utilisateur demande davantage d’informations sur l’élément en appuyant sur Verr. maj+F.

Le nom accessible est censé identifier le contrôle plutôt que documenter son comportement de manière exhaustive. Si une brève description n’est pas suffisante pour expliquer le contrôle, vous pouvez définir la propriété jointe [**AutomationProperties.HelpText**](https://msdn.microsoft.com/library/windows/apps/Hh759765) en plus de la propriété [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770).

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>Test anticipé et régulier de l’accessibilité  
Pour finir, la meilleure approche de la prise en charge des lecteurs d’écran consiste à tester vous-même votre application à l’aide d’un lecteur d’écran. Cela vous permettra de savoir comment le lecteur d’écran se comporte et d’identifier les informations d’accessibilité éventuellement manquantes dans l’application. Vous pourrez alors ajuster les valeurs de propriétés d’interface utilisateur ou UI Automation en conséquence. Pour plus d’informations, voir [Tests d’accessibilité](accessibility-testing.md).

L’un des outils que vous pouvez utiliser pour tester l’accessibilité s’appelle **AccScope**. L’outil **AccScope** est particulièrement utile car il vous permet d’afficher des représentations visuelles de votre interface utilisateur qui indiquent comment les technologies d’assistance peuvent voir votre application comme une arborescence d’automatisation. Il existe en particulier un mode Narrateur qui donne une vue de la façon dont le Narrateur obtient du texte de votre application et dont il organise les éléments dans l’interface utilisateur. AccScope est conçu de façon à pouvoir être utilisé et utile tout au long du cycle de développement d’une application, même pendant la phase de conception préliminaire. Pour plus d’informations, voir [AccScope](https://msdn.microsoft.com/library/windows/desktop/Dn433239).

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>Noms accessibles tirés de données dynamiques  
Windows prend en charge de nombreux contrôles qui peuvent servir à afficher des valeurs provenant d’une source de données associée, grâce à une fonctionnalité appelée *liaison de données*. Lors du remplissage de listes avec des éléments de données, vous devrez peut-être appliquer une technique qui définit les noms accessibles des éléments de listes liés aux données une fois la liste initiale remplie. Pour plus d’informations, voir « Scénario 4 » dans l’[Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570).

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>Noms accessibles et localisation  
Pour vous assurer que le nom accessible est également un élément localisé, vous devez appliquer des techniques correctes pour le stockage de chaînes localisables telles que les ressources, puis faire référence aux connexions de ressources avec des valeurs [directive x:Uid](https://msdn.microsoft.com/library/windows/apps/Mt204791). Si le nom accessible provient d’une utilisation d’[**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) définie de manière explicite, assurez-vous que la chaîne est aussi localisable.

Notez que les propriétés jointes telles les propriétés [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) utilisent une syntaxe de qualification spéciale pour le nom de la ressource, afin que la ressource fasse référence à la propriété jointe telle qu’elle a été appliquée à un élément spécifique. Par exemple, le nom de la ressource pour [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) telle qu’elle a été appliquée à un élément d’interface utilisateur nommé `MediumButton` est : `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)
* [Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Test de l’accessibilité](accessibility-testing.md)
