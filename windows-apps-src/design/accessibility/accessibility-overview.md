---
Description: This article is an overview of the concepts and technologies related to accessibility scenarios for Universal Windows Platform (UWP) apps.
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
title: Vue d’ensemble de l’accessibilité
label: Accessibility overview
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50e6c68841440120b783713ef0a591e39a7c7eec
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8798373"
---
# <a name="accessibility-overview"></a>Vue d’ensemble de l’accessibilité  




Cet article est une vue d’ensemble des concepts et technologies associés aux scénarios d’accessibilité des applications de plateforme Windows universelle (UWP).

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]

<span id="Accessibility_and_your_app"/>
<span id="accessibility_and_your_app"/>
<span id="ACCESSIBILITY_AND_YOUR_APP"/>

## <a name="accessibility-and-your-app"></a>Accessibilité et votre application  
Il existe de nombreuses invalidités et handicaps, notamment des limitations relatives à la mobilité, la vision, la perception des couleurs, l’audition, la parole, la cognition et l’alphabétisation. Toutefois, vous pouvez répondre à la plupart des besoins en suivant les recommandations fournies dans cette rubrique. Vous devez notamment offrir :

* la prise en charge d’interactions au clavier et de lecteurs d’écran ;
* la prise en charge de la personnalisation utilisateur, y compris les paramètres de police, de zoom (loupe), de couleur et de contraste élevé ;
* des alternatives ou suppléments pour des parties de votre interface utilisateur.

Les contrôles XAML fournissent une prise en charge intégrée du clavier et prennent en charge les lecteurs d’écran, qui tirent parti d’infrastructures d’accessibilité qui prennent déjà en charge les applications UWP, HTML et d’autres technologies d’interface utilisateur. Cette prise en charge intégrée offre un niveau d’accessibilité de base que vous pouvez personnaliser avec très peu de travail, en définissant uniquement quelques propriétés. Si vous créez vos propres contrôles et composants XAML personnalisés, vous pouvez également ajouter une prise en charge similaire à ces contrôles en faisant appel au concept d’*homologue d’automation*.

De plus, les fonctionnalités de liaison de données, de style et de modèle simplifient l’implémentation de la prise en charge des modifications dynamiques des paramètres d’affichage et du texte pour d’autres interfaces utilisateur.

<span id="UI_Automation"/>
<span id="ui_automation"/>
<span id="UI_AUTOMATION"/>

## <a name="ui-automation"></a>UIAutomation  
La prise en charge de l’accessibilité provient principalement de la prise en charge intégrée de l’infrastructure MicrosoftUIAutomation. Cette prise en charge est fournie via des classes de base et le comportement intégré de l’implémentation de classe pour les types de contrôle, ainsi que via une représentation d’interface de l’API du fournisseur UI Automation. Chaque classe de contrôle utilise les concepts UI Automation des homologues d’automation et des modèles d’automation pour signaler le rôle et le contenu des contrôles aux clients UI Automation. L’application est traitée en tant que fenêtre de niveau supérieur par UI Automation. Via l’infrastructure UI Automation, tout le contenu relatif à l’accessibilité présent dans cette fenêtre d’application est disponible pour un client UI Automation. Pour plus d’informations sur UI Automation, voir [Vue d’ensemble d’UI Automation](https://msdn.microsoft.com/library/windows/desktop/Ee684076).

<span id="Assistive_technology"/>
<span id="assistive_technology"/>
<span id="ASSISTIVE_TECHNOLOGY"/>

## <a name="assistive-technology"></a>Technologie d’assistance  
De nombreux besoins d’accessibilité des utilisateurs sont satisfaits par des produits de technologies d’assistance installés par l’utilisateur ou par des outils et paramètres fournis par le système d’exploitation. Parmi les fonctionnalités proposées, citons les lecteurs d’écran, le grossissement de l’écran et les paramètres de contraste élevé.

Les produits de technologie d’assistance englobent une large gamme de logiciels et de matériel. Ces produits fonctionnent par le biais des infrastructures d’accessibilité et d’interface de clavier standard qui signalent les informations relatives au contenu et à la structure d’une interface utilisateur aux lecteurs d’écran et autres technologies d’assistance. Voici quelques exemples de produits de technologie d’assistance :

* clavier visuel, qui permet d’utiliser un pointeur à la place d’un clavier pour la saisie d’un texte ;
* logiciels de reconnaissance vocale, qui convertissent la parole en texte tapé ;
* lecteurs d’écran, qui convertissent le texte en parole ou autres formes telles que le Braille ;
* lecteur d’écran du Narrateur, qui fait spécifiquement partie de Windows (le Narrateur dispose d’un mode tactile, qui permet de lire sur l’écran en exécutant des mouvements tactiles quand aucun clavier n’est disponible) ;
* programmes ou paramètres qui ajustent l’affichage ou certaines de ses zones, par exemple les thèmes à contraste élevé, les valeurs ppp de l’affichage ou l’outil Loupe.

Les applications qui disposent d’une bonne prise en charge des claviers et des lecteurs d’écran fonctionnent généralement bien avec différents produits de technologies d’assistance. Dans de nombreux cas, une application UWP fonctionne avec ces produits sans modification supplémentaire d’informations ou de structure. Toutefois, vous souhaiterez peut-être modifier certains paramètres pour une expérience d’accessibilité optimale ou une prise en charge supplémentaire.

Certaines des options à votre disposition pour tester des scénarios d’accessibilité de base avec les technologies d’assistance sont répertoriées dans [Test d’accessibilité](accessibility-testing.md).

<span id="Screen_reader_support_and_basic_accessibility_information"/>
<span id="screen_reader_support_and_basic_accessibility_information"/>
<span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"/>

## <a name="screen-reader-support-and-basic-accessibility-information"></a>Prise en charge de lecteurs d’écran et informations d’accessibilité de base  
Les lecteurs d’écran permettent d’accéder au texte d’une application en effectuant un rendu de celui-ci sous une autre forme, telle qu’une sortie en langue parlée ou en Braille. Le comportement exact d’un lecteur d’écran dépend du logiciel et de la façon dont l’utilisateur l’a configuré.

Par exemple, certains lecteurs d’écran lisent la totalité de l’interface utilisateur de l’application quand l’utilisateur démarre ou bascule vers l’application affichée. L’utilisateur peut ainsi recevoir tout le contenu d’information disponible avant de commencer la navigation. Certains lecteurs d’écran lisent aussi le texte associé à un contrôle individuel lorsqu’il reçoit le focus pendant la navigation par onglets. Cela permet aux utilisateurs de s’orienter à mesure qu’ils naviguent parmi les contrôles d’entrée d’une application. Le Narrateur est un exemple de lecteur d’écran qui fournit ces deux comportements, en fonction du choix de l’utilisateur.

L’information la plus importante dont un lecteur d’écran ou une autre technologie d’assistance a besoin pour aider les utilisateurs à comprendre une application ou à naviguer dans une application est un *nom accessible* pour les parties d’élément de l’application. Dans de nombreux cas, un contrôle ou élément possède déjà un nom accessible calculé à partir d’autres valeurs de propriété que vous avez fournies par un autre moyen. Le cas le plus courant dans lequel vous pouvez utiliser un nom déjà calculé concerne un élément qui prend en charge et affiche du texte interne. Pour les autres éléments, vous devez parfois prendre en compte d’autres manières de fournir un nom accessible en appliquant les meilleures pratiques en matière de structure d’élément. Parfois, vous devez fournir un nom spécifié de manière explicite comme nom accessible aux fins d’accessibilité de l’application. Pour obtenir une liste des valeurs calculées qui fonctionnent dans les éléments d’interface utilisateur courants et pour plus d’informations sur les noms accessibles en général, voir [Informations d’accessibilité de base](basic-accessibility-information.md).

Plusieurs autres propriétés d’automation sont disponibles (notamment les propriétés de clavier décrites dans la section suivante). Toutefois, tous les lecteurs d’écran ne prennent pas en charge toutes les propriétés d’automation. Engénéral, vous devez définir toutes les propriétés d’automatisation appropriées et les tester pour fournir la prise en charge la plus large possible pour les lecteurs d’écran.

<span id="Keyboard_support"/>
<span id="keyboard_support"/>
<span id="KEYBOARD_SUPPORT"/>

## <a name="keyboard-support"></a>Prise en charge du clavier  
Pour fournir une bonne prise en charge du clavier, vous devez vous assurer que chaque partie de votre application peut être utilisée avec un clavier. Si votre application utilise principalement les contrôles standard et n’utilise aucun contrôle personnalisé, vous n’aurez pas beaucoup de tâches supplémentaires à effectuer. Le modèle de contrôle de base XAML fournit une prise en charge intégrée du clavier incluant la navigation par tabulation, la saisie de texte et la prise en charge spécifique aux contrôles. Les éléments qui servent de conteneurs de disposition (tels que les panneaux) utilisent l’ordre de disposition pour établir un ordre de tabulation par défaut. Cet ordre est souvent l’ordre de tabulation correct à utiliser pour une représentation accessible de l’interface utilisateur. Si vous utilisez les contrôles [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) et [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) pour afficher des données, ils fournissent une navigation avec les touches de direction intégrée. Si vous utilisez un contrôle [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265), il gère déjà les touches Espace ou Entrée pour l’activation du bouton.

Pour plus d’informations sur tous les aspects de la prise en charge du clavier, y compris sur l’ordre de tabulation et la navigation ou l’activation basée sur les touches, voir [Accessibilité du clavier](keyboard-accessibility.md).

<span id="Media_and_captioning"/>
<span id="media_and_captioning"/>
<span id="MEDIA_AND_CAPTIONING"/>

## <a name="media-and-captioning"></a>Média et sous-titrage  
Vous affichez généralement le contenu multimédia audiovisuel par l’intermédiaire d’un objet [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926). Vous pouvez utiliser les API **MediaElement** pour contrôler la lecture de média. À des fins d’accessibilité, fournissez des contrôles qui permettent aux utilisateurs de lire, de mettre en pause et d’arrêter le contenu multimédia selon leurs besoins. Parfois, le contenu multimédia inclut des composants supplémentaires destinés à l’accessibilité, tels que les sous-titres ou les pistes audio de substitution qui comportent des descriptions narratives.

<span id="Accessible_text"/>
<span id="accessible_text"/>
<span id="ACCESSIBLE_TEXT"/>

## <a name="accessible-text"></a>Texte accessible  
Trois principaux aspects du texte sont pertinents en ce qui concerne l’accessibilité :

* Des outils doivent déterminer si le texte doit être lu dans le cadre d’une traversée de séquence de tabulation ou uniquement dans le cadre d’une représentation de document globale. Vous pouvez aider à contrôler cette détermination en choisissant l’élément approprié pour l’affichage du texte ou en ajustant les propriétés de ces éléments de texte. Chaque élément de texte disponible remplit une fonction donnée, souvent associée à un rôle UI Automation particulier. L’utilisation de l’élément incorrect peut entraîner le signalement du rôle incorrect à UI Automation et la création d’une expérience confuse pour un utilisateur de technologie d’assistance.
* De nombreux utilisateurs souffrent de limitations visuelles qui font que le texte est difficile à lire en cas d’insuffisance du contraste par rapport à l’arrière-plan. L’impact sur l’utilisateur est délicat à évaluer pour un concepteur d’application qui ne souffre pas de la même limitation visuelle. Par exemple, un choix des couleurs inapproprié lors de la conception peut empêcher certains utilisateurs daltoniens de pouvoir lire le texte. Les recommandations en matière d’accessibilité effectuées initialement pour le contenu Web définissent des normes de contraste qui peuvent éviter ces problèmes également dans les applications. Pour plus d’informations, consultez la rubrique [Exigences de texte accessible](accessible-text-requirements.md).
* De nombreux utilisateurs éprouvent des difficultés à lire du texte trop petit. Vous pouvez éviter que ce problème ne se produise en faisant en sorte que le texte de l’interface utilisateur de votre application soit suffisamment grand en premier lieu. Toutefois, ceci est difficile avec les applications qui affichent une grande quantité de texte ou du texte combiné à d’autres éléments visuels. Dans les cas de ce type, assurez-vous que l’application interagit correctement avec les fonctionnalités système qui peuvent agrandir l’affichage ainsi que le texte qu’elle contient. (Certains utilisateurs modifient les valeurs ppp comme option d’accessibilité.) (Cette option est disponible dans la zone **Agrandir les éléments affichés à l’écran** de la fenêtre **Options d’ergonomie**, qui redirige l’utilisateur vers une interface utilisateur du **Panneau de configuration** pour les options **Apparence et personnalisation** / **Affichage**.)

<span id="Supporting_high-contrast_themes"/>
<span id="supporting_high-contrast_themes"/>
<span id="SUPPORTING_HIGH-CONTRAST_THEMES"/>

## <a name="supporting-high-contrast-themes"></a>Prise en charge des thèmes à contraste élevé  
Les contrôles d’interface utilisateur utilisent une représentation visuelle définie dans le cadre d’un dictionnaire de ressources XAML de thèmes. Un ou plusieurs de ces thèmes sont spécifiquement utilisés lorsque le système est configuré en mode de contraste élevé. Lorsque l’utilisateur passe en mode de contraste élevé en recherchant dynamiquement le thème approprié dans un dictionnaire de ressources, tous vos contrôles d’interface utilisateur utilisent aussi un thème à contraste élevé approprié. Assurez-vous simplement de ne pas avoir désactivé les thèmes en spécifiant un style explicite ou en utilisant une autre technique de style qui empêche les thèmes à contraste élevé de se charger et de remplacer vos modifications de style. Pour plus d’informations, voir [Thèmes à contraste élevé](high-contrast-themes.md).

<span id="Design_for_alternative_UI"/>
<span id="design_for_alternative_ui"/>
<span id="DESIGN_FOR_ALTERNATIVE_UI"/>

## <a name="design-for-alternative-ui"></a>Conception d’une autre interface utilisateur  
Quand vous concevez vos applications, demandez-vous comment elles peuvent être utilisées par les personnes à mobilité réduite, malvoyantes ou malentendantes. Étant donné que les produits de technologies d’assistance utilisent fréquemment l’interface utilisateur standard, il est particulièrement important de fournir une bonne prise en charge des claviers et des lecteurs d’écran, même si vous n’effectuez aucun autre réglage relatif à l’accessibilité.

Dans de nombreux cas, plusieurs techniques permettent de transmettre les informations essentielles afin d’élargir votre audience. Par exemple, vous pouvez mettre en surbrillance des informations à l’aide d’informations sur les icônes et sur les couleurs pour aider les utilisateurs qui ne distinguent pas les couleurs, et vous pouvez afficher des alertes visuelles avec des effets sonores pour aider les utilisateurs qui sont malentendants.

Si nécessaire, vous pouvez fournir d’autres éléments d’interface utilisateur accessibles qui suppriment totalement les éléments et les animations non essentiels, et fournir d’autres simplifications pour rationaliser l’expérience utilisateur. L’exemple de code suivant montre comment afficher une instance de l’élément [**UserControl**](https://msdn.microsoft.com/library/windows/apps/BR227647) à la place d’une autre en fonction d’un paramètre utilisateur.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">

  <CheckBox x:Name="ShowAccessibleUICheckBox" Click="ShowAccessibleUICheckBox_Click">
    Show Accessible UI
  </CheckBox>

  <UserControl x:Name="ContentBlock">
    <local:ContentPage/>
  </UserControl>

</StackPanel>
```

VisualBasic
```vb
Private Sub ShowAccessibleUICheckBox_Click(ByVal sender As Object,
    ByVal e As RoutedEventArgs)

    If (ShowAccessibleUICheckBox.IsChecked.Value) Then
        ContentBlock.Content = New AccessibleContentPage()
    Else
        ContentBlock.Content = New ContentPage()
    End If
End Sub
```

C#
```csharp
private void ShowAccessibleUICheckBox_Click(object sender, RoutedEventArgs e)
{
    if ((sender as CheckBox).IsChecked.Value)
    {
        ContentBlock.Content = new AccessibleContentPage();
    }
    else
    {
        ContentBlock.Content = new ContentPage();
    }
}
```

<span id="Verification_and_publishing"/>
<span id="verification_and_publishing"/>
<span id="VERIFICATION_AND_PUBLISHING"/>

## <a name="verification-and-publishing"></a>Vérification et publication  
Pour plus d’informations sur les déclarations d’accessibilité et la publication de votre application, consultez la rubrique [Accessibilité dans le Windows Store](accessibility-in-the-store.md).

> [!NOTE]
> Déclarer l’application comme accessible n’est pertinent que pour le MicrosoftStore.

<span id="Assistive_technology_support_in_custom_controls"/>
<span id="assistive_technology_support_in_custom_controls"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"/>

## <a name="assistive-technology-support-in-custom-controls"></a>Prise en charge de la technologie d’assistance dans des contrôles personnalisés  
Lorsque vous créez un contrôle personnalisé, nous vous recommandons de mettre en œuvre ou de développer également une ou plusieurs sous-classes [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) pour fournir une prise en charge de l’accessibilité. Dans certains cas, tant que vous utilisez la même classe homologue que celle utilisée par la classe de contrôle de base, la prise en charge de l’automation pour votre classe dérivée est adéquate à un niveau de base. Cependant, nous vous conseillons de tester cette configuration. L’implémentation d’un homologue est toujours recommandée comme meilleure pratique, l’homologue pouvant correctement signaler le nom de la classe de votre nouveau contrôle. Plusieurs étapes sont nécessaires pour implémenter un homologue d’automatisation personnalisé. Pour plus d’informations, voir [Homologues d’automatisation personnalisés](custom-automation-peers.md).

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"/>
<span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"/>

## <a name="assistive-technology-support-in-apps-that-support-xaml--microsoft-directx-interop"></a>Prise en charge de la technologie d’assistance dans les applications qui gèrent l’interopérabilité entre XAML et Microsoft DirectX  
Par défaut, le contenu Microsoft DirectX hébergé dans une interface utilisateur XAML (à l’aide de la classe [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/Dn252834) ou [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/Hh702041)) n’est pas accessible. L’[exemple XAML SwapChainPanel DirectX interop](http://go.microsoft.com/fwlink/p/?LinkID=309155) montre comment créer des homologues UI Automation pour le contenu DirectX hébergé. Cette technique permet de rendre le contenu hébergé accessible via UI Automation.

## <a name="related-topics"></a>Rubriques connexes  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179)
* [Concevoir des applications pour l’accessibilité](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Exemple d’accessibilité XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Accessibilité](accessibility.md)
* [Prise en main du Narrateur](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)
