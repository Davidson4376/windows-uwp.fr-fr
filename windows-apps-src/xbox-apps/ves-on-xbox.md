---
title: Voix activée Shell (VES) sur Xbox
description: Découvrez comment ajouter la prise en charge du contrôle vocal à vos applications UWP sur Xbox.
ms.date: 10/19/2017
ms.topic: article
keywords: Windows 10, uwp, xbox, speech, shell de voix activée
ms.openlocfilehash: ea51216c804754e98c3bac459b79fb75dd9369cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596054"
---
# <a name="using-speech-to-invoke-ui-elements"></a>À l’aide de la reconnaissance vocale pour appeler des éléments d’interface utilisateur

Voix activée Shell (VES) est une extension de la plateforme de reconnaissance vocale Windows qui offre une expérience de première classe vocale à l’intérieur d’applications, en permettant aux utilisateurs d’utiliser la reconnaissance vocale pour l’appel à l’écran des contrôles et pour insérer du texte par le biais de dictée. VES s’efforce de fournir une expérience de voir-it-say-it de bout en bout commune sur tous les interpréteurs de commandes Windows et appareils, avec un effort minimal requis des développeurs d’applications.  Pour ce faire, il tire parti de la plateforme de reconnaissance vocale de Microsoft et de l’infrastructure UI Automation (UIA).

## <a name="user-experience-walkthrough"></a>Procédure pas à pas expérience d’utilisateur ##
Voici une vue d’ensemble de ce que pourrait rencontrer un utilisateur lors de l’utilisation de VES sur Xbox, et il doit permettre de définir le contexte avant de plonger dans les détails du fonctionnement du système VES.

- Utilisateur Active la console Xbox et souhaite parcourir leurs applications trouver quelque chose qui vous intéresse :

        User: "Hey Cortana, open My Games and Apps"

- Utilisateur reste dans Active à l’écoute Mode (ALM), ce qui signifie que la console est maintenant à l’écoute pour l’utilisateur d’appeler un contrôle est visible sur l’écran, sans avoir à dire, « Hey Cortana » chaque fois.  Utilisateur pouvez maintenant basculer pour afficher les applications et faites défiler la liste d’applications :

        User: "applications"

- Pour faire défiler la vue, utilisateur peut simplement dire :

        User: "scroll down"

- Utilisateur voit l’art de la zone de l’application, ils sont intéressés par mais que vous avez oublié le nom.  Pour afficher les étiquettes de Conseil de voix demande utilisateur :

        User: "show labels"

- Maintenant que vous compreniez ce qu’il faut par exemple, l’application peut être lancée :

        User: "movies and TV"

- Pour quitter le mode d’écoute actif, utilisateur dit à Xbox pour cesser d’écouter :

        User: "stop listening"

- Par la suite, une nouvelle session d’écoute active peut être démarrée avec :

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>Dépendance d’automation de l’interface utilisateur ##
VES est un client UI Automation et s’appuie sur les informations exposées par l’application via ses fournisseurs UI Automation. Il s’agit de la même infrastructure déjà utilisée par la fonctionnalité Narrateur sur les plateformes Windows.  UI Automation permet un accès par programme aux éléments d’interface utilisateur, y compris le nom du contrôle, son type et de modèles de quel contrôle qu’il implémente.  L’interface utilisateur que les modifications apportées dans l’application, VES sera réagir aux événements de mise à jour UIA et analyser une nouvelle fois l’arborescence UI Automation mis à jour pour rechercher tous les éléments passibles de poursuites, à l’aide de ces informations pour générer une grammaire de reconnaissance vocale. 

Toutes les applications UWP ont accès à l’infrastructure d’automatisation d’interface utilisateur et peuvent exposer des informations sur l’interface utilisateur indépendamment de l’infrastructure graphics ils reposent sur (XAML, DirectX/Direct3D, Xamarin, etc.).  Dans certains cas, comme XAML, la plupart du gros travail est effectuée par l’infrastructure, en réduisant considérablement le travail requis pour prendre en charge le Narrateur et VES.

Pour plus d’informations sur l’automatisation d’interface utilisateur, consultez [notions de base UI Automation](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "notions de base UI Automation").

## <a name="control-invocation-name"></a>Nom d’appel de contrôle ##
VES utilise l’heuristique suivante pour déterminer qu’une phrase à inscrire avec le module de reconnaissance vocale en tant que le nom du contrôle (ie. ce que l’utilisateur a besoin de parler pour appeler le contrôle).  Il s’agit également la phrase qui s’affiche dans l’étiquette d’info-bulle de voix.

Source du nom dans l’ordre de priorité :

1. Si l’élément a un `LabeledBy` propriété jointe, VES utilisera le `AutomationProperties.Name` de cette étiquette de texte.
2. `AutomationProperties.Name` de l’élément.  Dans XAML, le contenu de texte du contrôle sera être utilisé en tant que la valeur par défaut `AutomationProperties.Name`.
3. Si le contrôle est un bouton ou ListItem, VES recherchera le premier élément enfant avec valide `AutomationProperties.Name`.

## <a name="actionable-controls"></a>Contrôles actionnables ##
VES considère qu’un contrôle est actionnable si elle implémente un des modèles de contrôle d’Automation suivants :

- **InvokePattern** (par exemple). Bouton)-représente des contrôles qui initialisent ou effectuer une action unique et non équivoque et ne conservent pas l’état quand activé.

- **TogglePattern** (par exemple). Case à cocher) - représente un contrôle qui peut parcourir un ensemble d’états et conserver un état une fois défini.

- **SelectionItemPattern** (par exemple). Zone de liste déroulante) - représente un contrôle qui agit comme un conteneur pour une collection d’éléments enfants sélectionnables.

- **ExpandCollapsePattern** (par exemple). Zone de liste déroulante) - représente les contrôles qui visuellement développés pour afficher le contenu et réduisent pour masquer le contenu.

- **ScrollPattern** (par exemple). Liste) - représente des contrôles qui agissent comme des conteneurs à défilement pour une collection d’éléments enfants.

## <a name="scrollable-containers"></a>Conteneurs à défilement ##
Pour les conteneurs avec défilement qui prennent en charge que le ScrollPattern, VES écoutera les voix des commandes comme « défiler vers la gauche », « défiler vers la droite », etc. et appellera défilement avec les paramètres appropriés quand l’utilisateur déclenche une de ces commandes.  Commandes de défilement sont injectés selon la valeur de la `HorizontalScrollPercent` et `VerticalScrollPercent` propriétés.  Par exemple, si `HorizontalScrollPercent` est supérieure à 0, « défiler vers la gauche » doivent être ajoutées, si elle est inférieure à 100, « défiler vers la droite » sera ajouté et ainsi de suite.

## <a name="narrator-overlap"></a>Chevauchement du Narrateur ##
L’application le Narrateur est également un client UI Automation et qu’il utilise le `AutomationProperties.Name` propriété en tant qu’une des sources pour le texte, il lit pour l’élément d’interface utilisateur sélectionné.  Pour fournir une meilleure expérience d’accessibilité application de nombreux développeurs ont recouru à la surcharge la `Name` propriété avec une longue description dans le but de fournir plus d’informations et contexte lors de la lecture par le Narrateur.  Toutefois, cela entraîne un conflit entre les deux fonctions : VES doit expressions courtes qui correspondent ou sont étroitement liés le texte visible du contrôle, tout en avantages le Narrateur à partir d’expressions plus longues, plus descriptifs afin de donner une meilleure contexte.

Pour résoudre ce problème, en commençant par Windows 10 Creators Update, le Narrateur a été mis à jour pour examiner également le `AutomationProperties.HelpText` propriété.  Si cette propriété n’est pas vide, le Narrateur s’exprime son contenu en plus de `AutomationProperties.Name`.  Si `HelpText` est vide, le Narrateur lit le contenu du nom.  Cela permettra plus descriptifs chaînes à utiliser si nécessaire, mais conserve une expression convivial courte, speech recognition dans le `Name` propriété.

![](images/ves_narrator.jpg)

Pour plus d’informations, consultez [propriétés Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "propriétés Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur").

## <a name="active-listening-mode-alm"></a>Mode d’écoute actif (ALM) ##
### <a name="entering-alm"></a>Entrant ALM ###
Sur la Xbox, VES n'est pas constamment à l’écoute pour la saisie vocale.  L’utilisateur doit entrer en Mode actif à l’écoute explicitement en disant :

- « Hey Cortana, sélectionnez », ou
- « Hey Cortana, vérifiez une sélection »

Il existe plusieurs autres commandes Cortana également laisser l’utilisateur à l’écoute active à la fin, par exemple « Hey Cortana, de connexion » ou « Hey Cortana, accéder à l’accueil ». 

Entrant ALM aura l’effet suivant :

- La superposition de Cortana s’affichera dans le coin supérieur droit, indiquant qu’ils peuvent dire ce qu’ils voient à l’utilisateur.  Tandis que l’utilisateur est en parlant, les fragments de phrase qui sont reconnus par le module de reconnaissance vocale apparaît également dans cet emplacement.
- VES analyse l’arborescence UIA, recherche tous les contrôles utilisables, inscrit leur texte dans la grammaire de reconnaissance vocale et démarre une session d’écoute continue.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Quitter ALM ###
Le système restera dans ALM lorsque l’utilisateur interagit avec l’interface utilisateur à l’aide de la voix.  Il existe deux façons de quitter ALM :

- Utilisateur dit explicitement, « arrêter l’écoute », ou
- Un délai d’expiration se produit s’il n’est pas une reconnaissance positive 17 secondes de saisie ALM ou depuis la dernière reconnaissance positive

## <a name="invoking-controls"></a>Appeler des contrôles ##
En ALM l’utilisateur peut interagir avec l’interface utilisateur à l’aide de la voix.  Si l’interface utilisateur est configuré correctement (avec les propriétés du nom correspondant au texte visible), à l’aide de la voix pour effectuer des actions doit être à une expérience naturelle en toute transparence.  L’utilisateur doit être en mesure de vous dire ce qu’ils voient à l’écran.

## <a name="overlay-ui-on-xbox"></a>Superposition de l’interface utilisateur sur Xbox ##
Le nom VES dérive pour un contrôle peut être différent dans le texte visible dans l’interface utilisateur.  Cela peut être dû à la `Name` propriété du contrôle ou le fichier joint `LabeledBy` élément définie explicitement à une autre chaîne.  Ou bien, le contrôle n’a pas de texte de l’interface graphique utilisateur, mais uniquement un élément icône ou image.

Dans ce cas, les utilisateurs ont besoin d’un moyen de voir ce qui doit être désignée pour appeler ce type de contrôle.  Par conséquent, une seule fois dans écoute active, les conseils de voix peuvent être affichés en disant « afficher les étiquettes ».  Cela entraîne des étiquettes d’info-bulle s’affichent au-dessus de chaque contrôle actionnable voix.

Il existe une limite de 100 étiquettes, si l’interface utilisateur de l’application offre des contrôles plus exploitables que 100 il y aura certains qui n’aura pas d’étiquettes d’info-bulle de voix indiqués.  Les légendes sont choisies dans ce cas n’est pas déterministe, car elle dépend de la structure et la composition de l’interface utilisateur actuelle comme tout d’abord énumérés dans l’arborescence UIA.

Une fois que la voix info-bulle des étiquettes il n’existe aucune commande pour les masquer, elles restent visibles jusqu'à ce qu’un des événements suivants se produisent :

- l’utilisateur appelle un contrôle
- utilisateur navigue en dehors de la scène actuelle
- utilisateur dit, « arrêter l’écoute »
- mode d’écoute actif arrive à expiration

## <a name="location-of-voice-tip-labels"></a>Emplacement des étiquettes d’info-bulle de voix ##
Étiquettes d’info-bulle de voix sont centrés horizontalement et verticalement au sein BoundingRectangle du contrôle.  Lorsque les contrôles sont petites et étroitement groupées, les étiquettes peuvent se chevauchent/obscurci par d’autres et VES tente de pousser ces étiquettes les unes des autres pour les séparer et à s’assurer qu’ils sont visibles.  Toutefois, cela n’est pas garanti pour utiliser 100 % du temps.  S’il existe une interface utilisateur très encombrée, il sera probablement entraîner certaines étiquettes ne soit masqués par d’autres utilisateurs. Passez en revue votre interface utilisateur avec « afficher les étiquettes » afin de garantir un espace supplémentaire pour la visibilité d’info-bulle de voix.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Zones de liste déroulante ##
Lorsqu’une zone de liste déroulante est développée de chaque élément dans la zone de liste déroulante obtient son propre étiquette d’info-bulle de voix et souvent ils seront au-dessus des contrôles existants derrière la zone de liste déroulante.  Pour éviter de présenter un muddle soit ni encombré ni déroutant des étiquettes (où les étiquettes d’élément de zone de liste déroulante sont mélangées avec les étiquettes des contrôles derrière la zone de liste déroulante) lorsqu’une zone de liste déroulante est développée uniquement les étiquettes pour ses éléments enfants sont affichées ;  tous les autres étiquettes d’info-bulle de voix seront masquées.  L’utilisateur peut puis sélectionnez un des éléments de liste déroulante ou zone de liste déroulante « fermer ».

- Étiquettes sur des zones de liste déroulante réduit :

    ![](images/ves_combo_closed.png)

- Étiquettes de zone de liste déroulante étendue :

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Contrôles à défilement ##
Pour les contrôles à défilement, les conseils de la voix pour les commandes de défilement seront centrés sur chacun des bords du contrôle.  Conseils de voix ne s’affichera que pour les sens de défilement qui sont exploitables, par exemple si le défilement vertical n’est pas disponible, « défiler vers le haut » et « défiler vers le bas » n’apparaît pas.  Lorsque plusieurs régions déroulantes dans lesquelles figurent VES utilisera les ordinaux pour différencier (par ex. « Faites défiler vers le droite 1 », « Faites défiler vers le droite 2 », etc..).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Levée d’ambiguïté ##
Lorsque plusieurs éléments d’interface utilisateur ont le même nom ou le module de reconnaissance vocale mis en correspondance plusieurs candidats, VES bascule en mode de levée d’ambiguïté.  Dans cette info-bulle de la voix mode étiquettes seront affichera pour les éléments impliqués afin que l’utilisateur peut sélectionner celui qui convient. L’utilisateur peut annuler hors du mode de levée d’ambiguïté en disant « Annuler ».

Exemple :

- En Mode actif à l’écoute, avant la levée d’ambiguïté ; utilisateur dit « Suis I ambigu » :

    ![](images/ves_disambig1.png) 

- Les deux boutons mis en correspondance ; levée d’ambiguïté en route :

    ![](images/ves_disambig2.png) 

- Affichage cliquez sur l’action lorsque « Sélectionner 2 » a été choisie :

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Exemple d’interface utilisateur ##
Voici un exemple d’un XAML en fonction de l’interface utilisateur, en définissant AutomationProperties.Name la valeur de différentes manières :

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


À l’aide de l’exemple ci-dessus ici est l’aspect de l’interface utilisateur avec et sans les étiquettes d’info-bulle de voix.
 
- Mode Active à l’écoute, sans les étiquettes affichées :

    ![](images/ves_alm_nolabels.png) 

- En écoute Mode actif, une fois que l’utilisateur dit « afficher les étiquettes » :

    ![](images/ves_alm_labels.png) 

Dans le cas de `button1`, automatique XAML remplit le `AutomationProperties.Name` propriété à l’aide de texte à partir du contenu de texte visible du contrôle.  C’est pourquoi il existe une étiquette d’info-bulle de voix, même s’il n’est pas explicite `AutomationProperties.Name` définie.

Avec `button2`, nous définissons explicitement le `AutomationProperties.Name` sur autre chose que le texte du contrôle.

Avec `comboBox`, nous avons utilisé le `LabeledBy` propriété à référencer `label1` comme source de l’automatisation `Name`et dans `label1` nous définissons le `AutomationProperties.Name` à une expression plus naturelle que ce qui est rendu à l’écran (« Day of Week » au lieu de cela « Sélectionnez par jour de semaine »).

Enfin, avec `button3`, VES s’empare du `Name` du premier élément enfant depuis `button3` lui-même n’a pas une `AutomationProperties.Name` définie.

## <a name="see-also"></a>Voir également
- [Notions de base UI Automation](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "notions de base UI Automation")
- [Propriétés de l’Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "propriétés Automation pour la prise en charge de l’accessibilité dans l’interface utilisateur")
- [Forum aux questions](frequently-asked-questions.md)
- [UWP sur Xbox One](index.md)
