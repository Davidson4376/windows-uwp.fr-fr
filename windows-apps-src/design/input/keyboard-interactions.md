---
Description: Apprenez à concevoir et optimiser vos applications UWP afin qu’elles offrent la meilleure expérience possible pour les experts en utilisation du clavier comme pour les utilisateurs atteints de handicaps ou présentant d’autres exigences d’accessibilité.
title: Interactions avec le clavier
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: clavier, accessibilité, navigation, focus, texte, saisie, interactions utilisateur, boîtier de commande, accès à distance
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 20cb1e3162f0c852a60c620be92afd37ad9f8232
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317272"
---
# <a name="keyboard-interactions"></a>Interactions avec le clavier

![image héros du clavier](images/keyboard/keyboard-hero.jpg)

Apprenez à concevoir et optimiser vos applications UWP afin qu’elles offrent la meilleure expérience possible pour les experts en utilisation du clavier comme pour les utilisateurs atteints de handicaps ou présentant d’autres exigences d’accessibilité.

Quel que soit l’appareil, la saisie au clavier constitue une partie importante de l’expérience d’interaction globale sur la plateforme Windows universelle (UWP). Une expérience bien conçue permet aux utilisateurs de claviers de parcourir efficacement l’interface utilisateur de votre application et d’accéder à toutes ses fonctionnalités sans jamais lever les mains du clavier.

![image clavier et boîtier de commande](images/keyboard/keyboard-gamepad.jpg)

***Modèles courants d’interaction sont partagés entre le clavier et gamepad***

Dans cette rubrique, nous mettons l’accent sur la conception des applications UWP pour la saisie au clavier sur PC. Toutefois, une expérience de clavier bien conçue est importante pour prendre en charge des outils d’accessibilité tels que le Narrateur Windows, à l’aide de [claviers logiciels](#software-keyboard) tels que le clavier tactile et le clavier visuel (clavier visuel) et pour la gestion des autres types de périphérique d’entrée, tels que la Xbox gamepad et le contrôle à distance.

Nombre de recommandations abordées ici, y compris en matière de [visuels du focus](#focus-visuals), [touches d’accès rapide](#access-keys), et [navigation dans l’interface utilisateur](#navigation), sont également applicables à ces autres scénarios.

**REMARQUE** : bien que les claviers matériels et logiciels soient utilisés pour la saisie de texte, cette rubrique se concentre sur la navigation et l’interaction.

## <a name="built-in-support"></a>Prise en charge intégrée

Avec la souris, le clavier est le périphérique le plus largement utilisé sur les PC et, par conséquent, il constitue une partie essentielle de l’expérience PC. Les utilisateurs de PC attendent une expérience complète et cohérente du système comme des applications individuelles en réponse à la saisie au clavier.

Tous les contrôles UWP intègrent une prise en charge renforçant la richesse des expériences clavier et des interactions utilisateur, tandis que la plateforme proprement dite fournit une base complète pour la création d’expériences clavier mieux adaptée à la personnalisation de vos contrôles et applications.

![image de clavier avec téléphone](images/keyboard/keyboard-phone.jpg)

***UWP prend en charge le clavier avec n’importe quel appareil***

## <a name="basic-experiences"></a>Expériences de base
![Périphériques basés sur le focus](images/keyboard/focus-based-devices.jpg)

Comme mentionné précédemment, les périphériques d’entrée tels que le boîtier de commande Xbox et la télécommande, ainsi que les outils d’accessibilité tels que Narrateur, partagent la plus grande partie de l’expérience de saisie au clavier pour la navigation et les commandes. Cette expérience commune aux types d’entrée et outils limite les tâches supplémentaires pour vous et contribue à l’objectif « générer une seule fois, exécuter partout » de la plateforme Windows universelle.

Le cas échéant, nous les identifierons principales différences, vous devez être conscient et décrivent les limitations de risques que vous devez envisager.

Voici les périphériques et outils décrits dans cette rubrique :

| Appareil/outil                       | Description     |
|-----------------------------------|-----------------|
|Clavier (matériel et logiciel)   |Outre le clavier matériel standard, les applications UWP prennent en charge les deux claviers logiciels : le [clavier tactile (ou logiciel)](#software-keyboard) et [clavier visuel](#on-screen-keyboard).|
|Boîtier de commande et télécommande         |Le boîtier de commande Xbox et la télécommande sont des périphériques d’entrée fondamentaux pour l’[expérience « 10-foot »](../devices/designing-for-tv.md). Pour plus d’informations sur la prise en charge UWP pour le boîtier de commande et la télécommande, voir [Interactions entre le boîtier de commande et la télécommande](gamepad-and-remote-interactions.md).|
|Lecteurs d’écran (Narrateur)          |Narrateur est un lecteur d’écran intégré pour Windows ; il fournit des fonctionnalités et expériences d’interaction uniques, mais s’appuie toujours sur la saisie et la navigation de base au clavier. Pour plus d’informations sur le Narrateur, voir [Prise en main du Narrateur](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator).|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Expériences personnalisées et efficacité de la saisie
Comme indiqué, la prise en charge du clavier permet de garantir que vos applications fonctionnent parfaitement pour les utilisateurs aux compétences, capacités et attentes différentes. Nous recommandons de donner la priorité aux actions suivantes.
- Prendre en charge la navigation et de l’interaction avec le clavier
    - Vérifier que les éléments exploitables sont identifiés comme des taquets de tabulation (et que les éléments exploitables ne le sont pas), et que l’ordre de navigation est logique et prévisible (voir [Taquets de tabulation](#tab-stops))
    - Définir le focus initial sur l’élément le plus logique (voir [Focus initial](#initial-focus))
    - Proposer une navigation à l’aide des touches de direction pour les « navigations internes » (voir [Navigation](#navigation))
- Prendre en charge les raccourcis clavier
    - Proposer des touches d’accélération pour les actions rapides (voir [Accélérateurs](#accelerators))
    - Fournir des clés d’accès pour naviguer dans l’interface utilisateur de votre application (consultez [clés d’accès](access-keys.md))

### <a name="focus-visuals"></a>Visuels du focus

La plateforme UWP prend en charge un concept unique de visuel du focus qui fonctionne bien pour tous les types d’entrées et d’expériences.
![Visuel de focus](images/keyboard/focus-visual.png)

Un visuel du focus :

- S’affiche dès qu’un élément d’interface utilisateur reçoit le focus à partir d’un clavier, d’un boîtier de commande et/ou d’une télécommande
- Est restitué sous forme d’une bordure en surbrillance entourant l’élément d’interface utilisateur pour indiquer qu’une action peut être effectuée
- Permet à un utilisateur de naviguer dans une interface utilisateur d’application sans se perdre
- Peut être personnalisé pour votre application (voir [visuels du focus à haute visibilité](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**Remarque** : le visuel du focus de l’UWP n’est pas identique au rectangle de focus du Narrateur.

### <a name="tab-stops"></a>Taquets de tabulation

Pour utiliser un contrôle (y compris des éléments de navigation) avec le clavier, il faut que le focus de celui-ci soit positionné sur le contrôle. La première consiste pour un contrôle devant recevoir le focus clavier pour le rendre accessible via la navigation par onglets par identifiant sous forme d’onglet arrêt dans l’ordre de tabulation de votre application.

Pour un contrôle à inclure dans l’ordre de tabulation, le [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) propriété doit être définie sur **true** et [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) propriété doit être définie sur **true**.

Pour exclure spécifiquement un contrôle à partir de l’ordre de tabulation, définissez le [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) propriété **false**.

Par défaut, l’ordre de tabulation reflète l’ordre de création des éléments dans l’interface utilisateur. Par exemple, si un `StackPanel` contient un `Button`, un `Checkbox` et un `TextBox`, l’ordre de tabulation est `Button`, `Checkbox` et `TextBox`.

Vous pouvez remplacer l’ordre de tabulation par défaut en définissant le [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) propriété.

#### <a name="tab-order-should-be-logical-and-predictable"></a>L’ordre de tabulation doit être logique et prévisible

Un modèle bien conçu de navigation au clavier, disposant d’un ordre de tabulation logique et prévisible, rend votre application plus intuitive et aide les utilisateurs à explorer, détecter et accéder aux fonctionnalités plus efficacement.

Tous les contrôles interactifs doivent avoir des taquets de tabulation (sauf si elles se trouvent dans un [groupe](#control-group)), lors des contrôles non interactif, tels que des étiquettes, ne doit pas.

Évitez un ordre de tabulation personnalisé qui fait sauter le focus dans toute votre application. Par exemple, une liste des contrôles placée dans un formulaire doit présenter un ordre de tabulation de gauche à droite et de haut en bas (selon les paramètres régionaux).

Consultez [accessibilité du clavier](../accessibility/keyboard-accessibility.md) pour plus d’informations sur la personnalisation des taquets de tabulation.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Essayez de coordonner l’ordre de tabulation et l’ordre visuel

Coordination de l’ordre de tabulation et ordre visuel (également appelée en tant que la lecture de commande ou ordre d’affichage) permet de réduire la source de confusion pour les utilisateurs quand ils naviguent via l’interface utilisateur de votre application.

Essayez de classer et de présenter les commandes, contrôles et éléments de contenu les plus importants en premier, tant dans l’ordre de tabulation que dans l’ordre visuel. Toutefois, la position d’affichage réelle peut dépendre du conteneur de disposition parent et de certaines propriétés des éléments enfants qui influencent la disposition. Spécifiquement, des dispositions qui utilisent une métaphore visuelle de grille ou de tableau peuvent avoir un ordre visuel très différent de l’ordre de tabulation.

**REMARQUE** : l’ordre visuel dépend également des paramètres régionaux et linguistiques.

### <a name="initial-focus"></a>Focus initial

Le focus initial spécifie l’élément d’interface utilisateur qui reçoit le focus lors du premier lancement ou de la première activation d’une application ou d’une page. Lorsque vous utilisez un clavier, il est à partir de cet élément un utilisateur commence à interagir avec l’interface utilisateur de votre application.

Pour les applications UWP, le focus initial est défini à l’élément le plus élevé [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) qui peut recevoir le focus. Les éléments enfants des contrôles des conteneurs sont ignorés. En cas d’égalité, le premier élément de l’arborescence visuelle reçoit le focus.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Définissez le focus initial sur l’élément le plus logique

Définissez le focus initial sur l’élément d’interface utilisateur correspondant à la première action, ou à l’action principale, que les utilisateurs sont susceptibles d’entreprendre lors du lancement de votre application ou pour accéder à une page. Voici quelques exemples :
-   Une application photographique dans laquelle le focus est défini sur le premier élément d’une galerie
-   Une application de musique dans laquelle le focus est défini sur le bouton lecture

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Ne définissez pas le focus initial sur un élément qui expose un résultat négatif, ou même désastreux,

Ce niveau de fonctionnalité doit être le choix de l’utilisateur. Le fait de définir le focus initial sur un élément entraînant un résultat significatif peut entraîner un accès système ou une perte intempestive de données. Par exemple, ne définissez pas le focus sur le bouton de suppression lors de la navigation à un message électronique.

Consultez [concentrer navigation](focus-navigation.md) pour plus d’informations sur la substitution de l’ordre de tabulation.

### <a name="navigation"></a>Navigation

La navigation au clavier est généralement prise en charge via les touches de tabulation et les touches de direction.

![tabulation et touches de direction](images/keyboard/tab-and-arrow.png)

Par défaut, les contrôles UWP suivent ces comportements de base du clavier :
-   **Les touches de tabulation** donnent accès aux contrôles exploitables/actifs dans l’ordre de tabulation.
-   **Maj + Tab** permet d’accéder aux contrôles dans l’ordre inverse de la tabulation. Si l’utilisateur accède au contrôle à l’aide d’une touche de direction, le focus est défini sur la dernière valeur connue à l’intérieur du contrôle.
-   **Les touches de direction** exploitent la « navigation interne » spécifique au contrôle lorsque l’utilisateur accède à ce mode, les touches de direction ne permettent pas d’accéder à l’extérieur d’un contrôle. Voici quelques exemples :
    -   Flèche haut/bas touche déplace le focus à l’intérieur de `ListView` et `MenuFlyout`
    -   Modifier les valeurs sélectionnées pour `Slider` et `RatingsControl`
    -   Déplacer le signe insertion à l’intérieur `TextBox`
    -   Développer/réduire des éléments à l’intérieur `TreeView`

Utilisez ces comportements par défaut pour optimiser la navigation au clavier de votre application.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Utilisez la « navigation interne » avec les jeux de contrôles associés

Fournir une flèche navigation clés en un ensemble de contrôles connexes renforce leur relation au sein de l’organisation générale de l’interface utilisateur de votre application.

Par exemple, le contrôle `ContentDialog` illustré ici propose une navigation interne par défaut pour une ligne horizontale de boutons (pour les contrôles personnalisés, voir la section [Groupe de contrôles](#control-group)).

![exemple de boîte de dialogue](images/keyboard/dialog.png)

***Interaction avec une collection de boutons associés qui facilitent la navigation de clé flèche***

Si les éléments sont affichés dans une seule colonne, ils sont accessibles à l’aide des flèches haut/bas. Si les éléments sont affichés sur une seule ligne, ils sont accessibles à l’aide des flèches gauche/droite. Si les éléments sont répartis sur plusieurs colonnes, ils sont accessibles à l’aide des 4 touches de direction.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Définir un taquet de tabulation pour une collection de contrôles connexes

En définissant un taquet de tabulation pour une collection de contrôles connexes, ou complémentaires, vous pouvez réduire le nombre global des taquets de tabulation dans votre application.

Par exemple, les images suivantes montrent deux contrôles `ListView` empilés. L’image de gauche montre la navigation à l’aide des touches de direction, utilisée avec un taquet de tabulation pour le déplacement entre les contrôles `ListView`, tandis que l’image de droite montre comment simplifier et rationaliser la navigation entre les éléments enfants en éliminant toute nécessité pour parcourir les contrôles parents avec une touche de tabulation.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***Interaction avec les deux contrôles ListView empilées peut être effectuée plus facile et plus efficace en éliminant le taquet de tabulation et de navigation avec simplement les touches de direction.***

Consultez la section [Groupe de contrôles](#control-group) pour savoir comment appliquer les exemples de l’optimisation à l’interface utilisateur de votre application.

### <a name="interaction-and-commanding"></a>Interaction et commandes

Dès qu’un contrôle a le focus, un utilisateur peut interagir avec lui et appeler toutes les fonctionnalités associées à l’aide d’une commande clavier spécifique.

#### <a name="text-entry"></a>Saisie de texte

Pour les contrôles spécialement conçus pour la saisie de texte tels que `TextBox` et `RichEditBox`, toutes les saisies au clavier sont utilisées pour la frappe ou la navigation dans le texte, ce qui est prioritaire sur les autres commandes de clavier. Par exemple, le menu déroulant d’un contrôle `AutoSuggestBox` ne reconnaît pas la touche **espace** comme une commande de sélection.

![saisie de texte](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Touche espace

Lorsque vous n’êtes pas en mode saisie de texte, la touche **espace** appelle l’action ou la commande associée au contrôle qui a le focus (comme un appui sur une touche ou un clic avec une souris).

![touche espace](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Touche Entrée

La touche **Entrée** peut accomplir des interactions utilisateur courantes et variées, selon le contrôle qui a le focus :
-   Active les contrôles de la commande tels qu’un `Button` ou `Hyperlink`. Pour éviter toute confusion de l’utilisateur final, la touche **Entrée** active également les contrôles ressemblant à des contrôles de commandes tels que `ToggleButton` ou `AppBarToggleButton`.
-   Affiche l’interface utilisateur du sélecteur pour les contrôles tels que `ComboBox` et `DatePicker`. La touche **Entrée** valide également et ferme l’interface utilisateur du sélecteur.
-   Active les contrôles de liste comme `ListView`, `GridView` et `ComboBox`.
    -   La touche **Entrée** exécute l’action de sélection en tant que touche **Espace** pour les éléments de liste et de grille, sauf si une action supplémentaire est associée à ces éléments (en ouvrant une nouvelle fenêtre).
    -   Si une action supplémentaire est associée au contrôle, la touche **Entrée** effectue l’action supplémentaire et la touche **Espace** exécute l’action de sélection.

**REMARQUE** : les touches **Entrée** et **Espace** n’accomplissent pas toujours la même action, bien que ce soit souvent le cas.

![touche Entrée](images/keyboard/enter-key.png)

La touche Échap permet à un utilisateur d’annuler l’interface utilisateur temporaire (ainsi que toutes les actions en cours dans cette interface).

Voici quelques exemples de cette expérience :
-   Un utilisateur ouvre une `ComboBox` avec une valeur sélectionnée et utilise les flèches pour déplacer la sélection du focus vers une nouvelle valeur. L’appui sur la touche Échap ferme le `ComboBox` et rétablit la valeur d’origine de la valeur sélectionnée.
-   L’utilisateur appelle une action de suppression définitive d’un message électronique et est invité à confirmer l’action via un `ContentDialog`. L’utilisateur décide que ce n’est pas l’action prévue et appuie sur la touche **Échap** pour fermer la boîte de dialogue. Comme la touche **Échap** est associée au bouton **Annuler**, la boîte de dialogue se referme et l’action est annulée. La touche **Échap** affecte uniquement l’interface utilisateur temporaire, elle ne ferme pas l’interface utilisateur de l’application et ne permet pas la navigation à rebours.

![Touche Échap](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Touches Début et Fin

Les touches **Début** et **Fin** permettent à un utilisateur d’accéder au début ou la fin d’une zone de l’interface utilisateur.

Voici quelques exemples de cette expérience :
-   Pour les contrôles `ListView` et `GridView`, la touche **Début** déplace le focus vers le premier élément et le fait défiler, tandis que **Fin** déplace le focus vers le dernier élément et le fait défiler.
-   Pour un contrôle `ScrollView`, la touche **Début** provoque un défilement vers le haut de la zone, tandis que **Fin** déclenche un défilement vers le bas (le focus n’est pas modifié).

![touches début et fin](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Touches Page haut et Page bas

Les touches **Page** permettent à un utilisateur de faire défiler une zone de l’interface utilisateur par incréments discrets.

Par exemple, pour les contrôles `ListView` et `GridView`, la touche **Page haut** fait remonter la zone d’une « page » (en règle générale, la hauteur de fenêtre d’affichage) et déplace le focus vers le haut de la zone. Par ailleurs, la touche **Page bas** fait descendre la zone de la valeur d’une page et déplace le focus vers le bas de la zone.

![touches page haut et page bas](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Raccourcis clavier

Raccourcis clavier peuvent rendre votre application plus facile à utiliser en fournissant à la fois meilleure prise en charge pour l’accessibilité et amélioration de l’efficacité des utilisateurs du clavier.

La prise en charge la navigation au clavier et activation dans votre application, il est également conseillé de fournir des raccourcis pour les fonctionnalités de votre application. Navigation par onglets fournit un niveau de bonne, base de prise en charge du clavier, mais avec l’interface utilisateur plus complexe, vous souhaiterez peut-être ajouter la prise en charge des touches de raccourci également. 

Un raccourci est une combinaison de touches qui améliore la productivité en fournissant à l’utilisateur un moyen efficace d’accéder aux fonctionnalités de l’application. Il existe deux types de raccourcis :
-   [Accélérateurs](#accelerators) sont des raccourcis qui appellent une commande de l’application. Votre application peut ou ne peut pas fournir de l’interface utilisateur spécifique qui correspond à la commande. Accélérateurs se composent généralement de la touche Ctrl enfoncée et une clé de la lettre.
-   [Clés d’accès](#access-keys) les raccourcis que définir le focus sur l’interface utilisateur spécifique dans votre application. Typicaly de clés d’accès se composent de la touche Alt et une clé de la lettre.

Raccourcis clavier cohérente qui prennent en charge des tâches similaires entre les applications les rend beaucoup plus utile et puissante et permet aux utilisateurs les rappeler.

#### <a name="accelerators"></a>Accélérateurs

Accélérateurs aider les utilisateurs à effectuer des actions courantes dans une application beaucoup plus rapidement et efficacement. 

Exemples d’accélérateurs :
-   Si vous appuyez sur Ctrl + N de clé de n’importe où dans le **Mail** application démarre un nouvel élément de messagerie.
-   En appuyant sur Ctrl + E clé n’importe où dans Microsoft Edge (et nombreuses applications Microsoft Store) lance la recherche.

Les accélérateurs présentent les caractéristiques suivantes :
-   Ils utilisent principalement Ctrl et la fonction de la clé de séquences (touches de raccourci Windows system également utilisent Alt + touches non alphanumériques et la touche du logo Windows).
-   Elles sont attribuées uniquement aux commandes les plus couramment utilisées.
-   Elles sont destinées à être mémorisées et sont indiquées uniquement dans les menus, les info-bulles et l’aide.
-   Elles n’ont effet dans toute l’application, lors de la prise en charge.
-   Ils doivent être assignées constamment tels qu’ils sont mémorisés non documentées directement.

#### <a name="access-keys"></a>Touches d’accès rapide

Consultez la page [touches d’accès rapide](access-keys.md) où vous trouverez des informations approfondies sur la prise en charge des touches d’accès rapide avec UWP.

Les touches d’accès rapide permettent aux utilisateurs présentant un handicap moteur fonctionnel d’appuyer sur une touche à la fois pour agir sur un élément spécifique de l’interface utilisateur. En outre, les touches d’accès rapide peuvent faire office de touches de raccourci supplémentaires pour aider les utilisateurs avancés à effectuer des actions rapidement.

Les touches d’accès rapide présentent les caractéristiques suivantes :
-   Elles utilisent la touche Alt associée à une touche alphanumérique.
-   Elles visent principalement l’accessibilité.
-   Ils sont documentés directement dans l’interface utilisateur, adjacent au contrôle, via [touches accélératrices](access-keys.md).
-   Leur incidence se limite à la fenêtre active et elles permettent d’accéder à l’option de menu ou au contrôle correspondant.
-   Clés d’accès doivent être affectées régulièrement des commandes couramment utilisées (en particulier les boutons de validation), chaque fois que possible.
-   Elles sont localisées.

#### <a name="common-keyboard-shortcuts"></a>Raccourcis clavier courants

Le tableau suivant est un petit échantillon des raccourcis clavier fréquemment utilisées. 

| Action                               | Touche                                      |
|--------------------------------------|--------------------------------------------------|
| Tout sélectionner                           | Ctrl+A                                           |
| Sélection continue                  | Maj+touche de direction                                  |
| Save                                 | Ctrl+S                                           |
| Rechercher                                 | Ctrl+F                                           |
| Imprimer                                | Ctrl+P                                           |
| Copier                                 | Ctrl+C                                           |
| Couper                                  | Ctrl+X                                           |
| Coller                                | Ctrl+V                                           |
| Annuler                                 | Ctrl+Z                                           |
| Onglet suivant                             | Ctrl+Tab                                         |
| Fermer l’onglet                            | Ctrl+F4 ou Ctrl+W                                |
| Zoom sémantique                        | Ctrl++ ou Ctrl+-                                 |

Pour obtenir la liste complète des raccourcis de système de Windows, consultez [raccourcis clavier pour Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). Pour les raccourcis d’application courants, consultez [raccourcis clavier pour les applications Microsoft](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>Expériences avancées

Dans cette section, nous aborderons certaines des expériences d’interaction avec le clavier les plus complexes supportées par les applications UWP, ainsi que certains comportements dont vous devez être conscient lorsque votre application est utilisée sur des appareils et avec des outils différents.

### <a name="control-group"></a>Groupe de contrôles

Vous pouvez regrouper un ensemble de contrôles associés ou complémentaires, dans un « groupe de contrôle » (ou une zone directionnelle), ce qui permet la « navigation interne » à l’aide des touches de direction. Le groupe de contrôle peut être un taquet de tabulation, mais vous pouvez aussi spécifier plusieurs taquets de tabulation au sein du groupe de contrôle.

#### <a name="arrow-key-navigation"></a>Navigation avec les touches de direction

Les utilisateurs s’attendent à la prise en charge de la navigation à l’aide des touches de direction lorsqu’il existe un groupe de contrôles similaires dans une région de l’interface utilisateur :
-   `AppBarButtons` dans un `CommandBar`
-   `ListItems` ou `GridItems` à l’intérieur `ListView` ou `GridView`
-   `Buttons` à l’intérieur `ContentDialog`

Par défaut, les contrôles d’UWP prennent en charge la navigation avec les touches de direction. Pour les dispositions personnalisées et les groupes de contrôle, utilisez `XYFocusKeyboardNavigation="Enabled"` pour fournir un comportement similaire.

Vous pouvez envisager la prise en charge pour la navigation de clé flèche lorsque vous utilisez les contrôles suivants :

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Boutons de la boîte de dialogue</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems et GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Taquets de tabulation

Selon les fonctionnalités et la disposition de votre application, la meilleure option de navigation pour un groupe de contrôle peut être un taquet de tabulation avec navigation avec les flèches pour les éléments enfants, plusieurs taquets de tabulation ou une combinaison des deux.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Utilisez plusieurs taquets de tabulation et les touches de direction pour les boutons

Les utilisateurs d’accessibilité s’appuient sur des règles bien établies de navigation au clavier, qui n’utilisent généralement pas les touches de direction pour naviguer dans une collection de boutons. Toutefois, les utilisateurs sans déficience visuelle peuvent penser que le comportement est naturel.

Un exemple de comportement par défaut d’UWP dans ce cas est le `ContentDialog`. Même si les touches de direction peuvent être utilisées pour naviguer entre les boutons, chaque bouton est également un taquet de tabulation.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Affecter un taquet de tabulation unique aux modèles familiers de l’interface utilisateur

Dans les cas où votre agencement suit un modèle bien connu d’interface utilisateur pour les groupes de contrôle, l’affectation au groupe d’un taquet de tabulation unique peut améliorer l’efficacité de la navigation pour les utilisateurs.

Par exemple :
-   `RadioButtons`
-   Plusieurs `ListViews` qui ressemblent et se comportent comme un seul `ListView`
-   Toute interface utilisateur conçue pour ressembler et se comporter comme une grille de vignettes (par exemple, les vignettes du menu Démarrer)

#### <a name="specifying-control-group-behavior"></a>Spécification du comportement de groupe de contrôles

Utilisez les API suivantes pour prendre en charge le comportement de groupes de contrôle personnalisés (toutes sont détaillées plus loin dans cette rubrique) :

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) permet une navigation par touches de direction entre les contrôles
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) indique s’il existe plusieurs taquets de tabulation ou un taquet unique
-   [FindFirstFocusableElement et FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) définissent le focus sur le premier élément avec la touche **Début** et sur le dernier élément avec la touche **Fin**

L’image suivante montre un comportement de navigation intuitive au clavier pour un groupe de contrôles composé de cases d’options associées. Dans ce cas, nous recommandons un taquet de tabulation unique pour le groupe de contrôles, une navigation interne entre les cases d’option à l’aide des touches de direction, la liaison de **Début** avec la première case d’option et de **Fin** avec la dernière.

![synthèse de tous les éléments](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Clavier et Narrateur

Le Narrateur est un outil d’accessibilité de l’interface utilisateur orienté vers les utilisateurs de claviers (les autres types d’entrées sont également pris en charge). Toutefois, la fonctionnalité du Narrateur va au-delà des interactions avec le clavier prises en charge par les applications UWP et une attention supplémentaire est requise lors de la conception de votre application UWP pour le Narrateur. (La [page des notions de base du Narrateur](https://support.microsoft.com/help/22808/windows-10-narrator-basics) vous guide tout au long de l’expérience utilisateur du Narrateur.)

Voici quelques-unes des différences entre les comportements de clavier avec UWP et ceux pris en charge par le Narrateur :
-   Des combinaisons de touches supplémentaires sont prévues pour accéder aux éléments d’interface utilisateur qui ne sont pas exposés par la navigation standard au clavier, tels que la lecture des étiquettes de contrôle avec Ver Maj + touches de direction.
-   Navigation vers les éléments désactivés. Par défaut, les éléments désactivés ne sont pas exposés par la navigation standard au clavier.
    -   Contrôlez les « affichages » pour une navigation plus rapide basée sur la granularité de l’interface utilisateur. Les utilisateurs peuvent accéder à des éléments, caractères, mots, lignes, paragraphes, liens, titres, tableaux, repères et suggestions. La navigation standard au clavier expose ces objets sous forme de liste plate, ce qui peut rendre la navigation fastidieuse si vous ne fournissez pas de touches de raccourci.

#### <a name="case-study--autosuggestbox-control"></a>Étude de cas – le contrôle AutoSuggestBox

Le bouton Rechercher de `AutoSuggestBox` n’est pas accessible par la navigation standard au clavier avec les touches de direction et de tabulation, car l’utilisateur peut appuyer sur la touche **Entrée** pour soumettre la requête de recherche. Toutefois, il est accessible via le Narrateur lorsque l’utilisateur appuie sur Ver Maj + une touche de direction.

![suggestion automatique du focus du clavier](images/keyboard/auto-suggest-keyboard.png)

*Avec le clavier, les utilisateurs appuient sur la* ***entrée*** *clé pour envoyer la requête de recherche*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>Le Narrateur, les utilisateurs d’appuyer sur le <strong>entrée</strong> clé pour envoyer la requête de recherche</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>Avec le Narrateur, les utilisateurs peuvent également accéder au bouton de recherche en utilisant le <strong>VERR. MAJ + touche de direction droite</strong>, puis en appuyant sur <strong>espace</strong> clé</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>Le clavier, la télécommande et le boîtier de commande Xbox

Les télécommandes et boîtiers de commande Xbox prennent en charge de nombreux comportements et expériences de clavier UWP. Toutefois, en raison de l’absence de diverses options de touches disponibles par rapport à un clavier, le boîtier de commande et la télécommande ne disposent pas des nombreuses optimisations propres au clavier (la télécommande est encore plus limitée que le boîtier de commande).

Consultez [Gamepad et contrôle à distance des interactions](gamepad-and-remote-interactions.md) pour plus d’informations sur la prise en charge UWP gamepad et entrée de contrôle à distance.

Voici quelques correspondances de touches entre le clavier, le boîtier de commande et la télécommande.

| **Clavier**  | **Boîtier de commande**                         | **Contrôle à distance**  |
|---------------|-------------------------------------|---------------------|
| Espace         | Bouton A                            | Bouton Sélection       |
| Entrée         | Bouton A                            | Bouton Sélection       |
| Échap        | Bouton B                            | Bouton Précédent         |
| Début/Fin      | N/A                                 | N/A                 |
| Page haut/bas  | Bouton de déclenchement du défilement vertical, bouton pare-chocs du défilement horizontal   | N/A                 |

Voici certaines différences clés, dont vous devez être conscient lors de la conception de votre application UWP pour une utilisation avec le boîtier de commande et la télécommande :
-   La saisie de texte exige de l’utilisateur qu’il appuie sur A pour activer un contrôle de texte.
-   La navigation en mode focus ne se limite pas au contrôle des groupes, les utilisateurs peuvent naviguer librement vers tous les éléments de l’interface utilisateur sélectionnables dans l’application.

    **REMARQUE** : le focus peut se déplacer vers tout élément d’interface utilisateur sélectionnable dans le sens indiqué par la touche pressée, sauf s’il se trouve dans un segment de chevauchement de l’interface ou si une [activation du focus](gamepad-and-remote-interactions.md#focus-engagement) est spécifiée, ce qui empêche le focus d’entrer/sortir d’une zone tant qu’il n’y a pas d’enclenchement/activation avec le bouton A. Pour plus d’informations, voir la section [navigation directionnelle](#directional-navigation).
-   Le bouton multidirectionnel et la touche gauche du stick analogique sont utilisés pour déplacer le focus entre les contrôles ainsi que pour la navigation interne.

    **REMARQUE** : le boîtier de commande et la télécommande ne permettent d’accéder qu’aux éléments qui se trouvent dans le même ordre visuel lorsque la touche de direction est enfoncée. La navigation est désactivée dans cette direction lorsqu’aucun élément suivant ne peut recevoir le focus. Selon la situation, les utilisateurs du clavier ne sont pas toujours confrontés à cette contrainte. Consultez la section [Optimisation de clavier intégré](#built-in-keyboard-optimization) pour en savoir plus.

#### <a name="directional-navigation"></a>Navigation directionnelle

La navigation directionnelle est gérée par une classe d’assistance UWP [Gestionnaire de Focus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) qui enregistre la touche directionnelle (touche de direction, bouton multidirectionnel) et tente de déplacer le focus dans la direction visuelle correspondante.

À la différence du clavier, quand une application opts hors [Mode de la souris](gamepad-and-remote-interactions.md#mouse-mode), navigation directionnelle est appliquée dans toute l’application pour gamepad et de contrôle à distance. Consultez [Gamepad et contrôle à distance des interactions](gamepad-and-remote-interactions.md) pour plus d’informations sur l’optimisation de la navigation directionnelle.

**REMARQUE** : la navigation à l’aide de la touche de tabulation du clavier n’est pas considérée comme une navigation directionnelle. Pour en savoir plus, voir la section [Taquets de tabulation](#tab-stops).

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Navigation directionnelle pris en charge</strong></br>À l’aide des touches de direction (touches de direction, gamepad et le contrôle à distance pavé directionnel), l’utilisateur peut naviguer entre les différents contrôles.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>Navigation directionnelle ne pas pris en charge</strong> </br>L’utilisateur ne peut pas naviguer entre les différents contrôles à l’aide des touches de direction. Autres méthodes de navigation entre les contrôles (touche de tabulation) ne sont pas affectées.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Intégré dans l’optimisation du clavier

En fonction de la disposition et des contrôles utilisés, les applications UWP peuvent être optimisées spécifiquement pour la saisie au clavier.

L’exemple qui suit montre un groupe d’éléments de liste, d’éléments de grille et d’éléments de menu qui ont été affectés à un taquet de tabulation (voir la section [Taquets de tabulation](#tab-stops)). Lorsque le groupe a le focus, la navigation interne s’effectue à l’aide des touches de direction dans l’ordre visuel correspondant (voir la section [Navigation](#navigation)).

![navigation avec les touches de direction dans une colonne unique](images/keyboard/single-column-arrow.png)

***Navigation de clé unique colonne flèche***

![navigation avec les touches de direction dans une ligne unique](images/keyboard/single-row-arrow.png)

***Navigation de clé flèche ligne unique***

![navigation avec les touches de direction dans plusieurs colonnes et lignes](images/keyboard/multiple-column-and-row-navigation.png)

***Plusieurs colonnes/lignes la Navigation flèche de clé***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Emballage d’éléments homogènes d’affichage de liste et de grille

La navigation directionnelle n’est pas toujours le moyen le plus efficace pour parcourir plusieurs lignes et colonnes d’éléments de liste et de type GridView.

**REMARQUE** : les éléments de menu sont généralement des listes à une seule colonne, mais des règles spéciales de focus peuvent s’appliquer dans certains cas (voir [Interface utilisateur contextuelle](#popup-ui)).

Les objets de liste et de grille peuvent être créés avec plusieurs lignes et colonnes. Ils sont généralement ordonnés en mode « row-major » (où les éléments remplissent d’abord toute la ligne avant de passer à la ligne suivante) ou en mode « column-major » (où les éléments remplissent d’abord toute la colonne avant de passer à la colonne suivante). L’ordre par ligne (row-major) ou par colonne (column-major) dépend du sens de défilement, et vous devez vous assurer que l’ordre des éléments n’est pas en conflit avec ce sens.

Dans le cas d’un ordonnancement en mode « row-major » (où les éléments sont renseignés de gauche à droite et de haut en bas), lorsque le focus est sur le dernier élément d’une ligne et que la touche flèche droite est enfoncée, le focus se déplace vers le premier élément de la ligne suivante. Ce comportement se produit dans l’ordre inverse : Lorsque le focus est défini sur le premier élément dans une ligne et la touche de direction gauche est enfoncée, le focus est déplacé au dernier élément dans la ligne précédente.

Dans le cas d’un ordonnancement en mode « column-major » (où les éléments sont renseignés de haut en bas et de gauche à droite), lorsque le focus est sur le dernier élément d’une colonne et que l’utilisateur appuie sur la touche flèche bas, le focus se déplace vers le premier élément de la colonne suivante. Ce comportement se produit dans l’ordre inverse : Lorsque le focus est défini sur le premier élément dans une colonne et la touche est enfoncée, le focus est déplacé au dernier élément dans la colonne précédente.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Navigation au clavier majeure ligne</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Navigation de clavier principales de colonne</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Menu contextuel de l’interface utilisateur

Comme mentionné, vous devez essayer de vous assurer de la navigation directionnelle correspond à l’ordre visuel des contrôles dans l’interface utilisateur de votre application.

Certains contrôles, tels que ContextMenu, AppBarOverflowMenu et suggestion automatique, incluent un menu contextuel qui s’affiche dans un emplacement et la direction par rapport au contrôle principal (basé sur l’espace à l’écran disponible). Par exemple, lorsque l’espace est insuffisant pour que le menu s’ouvre vers le bas (la direction par défaut), il s’ouvre vers le haut. Il n’existe aucune garantie que le menu s’ouvre à chaque fois dans le même sens.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

Pour ces contrôles, lors de la première ouverture du menu (et si aucun élément n’a été sélectionné par l’utilisateur), la touche flèche bas place toujours le focus sur le premier élément et la touche flèche haut le place toujours sur le dernier élément du menu. De même, lorsque le dernier élément est sélectionné et lorsque la touche flèche bas est activée, le focus se déplace sur le premier élément du menu et lorsque le premier élément est sélectionné et que l’utilisateur appuie sur la touche flèche haut, le focus se déplace vers le dernier élément du menu.

Vous devez tenter d’émuler ces mêmes comportements dans vos contrôles personnalisés. Vous trouverez des exemples de code sur la façon d’implémenter ce comportement dans [navigation du focus par programmation](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) documentation.

## <a name="test-your-app"></a>Tester votre application

Testez votre application avec tous les périphériques d’entrée pris en charge pour garantir l’utilisation cohérente et intuitive des éléments d’interface utilisateur et vous assurer qu’aucun élément inattendu n’interfère avec l’ordre de tabulation souhaité.

## <a name="related-articles"></a>Articles associés
* [Événements de clavier](keyboard-events.md)
* [Identification des périphériques d’entrée](identify-input-devices.md)
* [Répondre à la présence du clavier tactile](respond-to-the-presence-of-the-touch-keyboard.md)
* [Exemple d’éléments visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)

## <a name="appendix"></a>Annexe

### <a name="software-keyboard"></a>Clavier logiciel

Le clavier logiciel est un clavier qui s’affiche sur l’écran et que l’utilisateur peut utiliser à la place du clavier physique pour entrer des données à l’aide de la fonction tactile, de la souris, du stylo/stylet ou d’un autre périphérique de pointage (un écran tactile n’est pas nécessaire). Sur l’écran tactile, il suffit de toucher directement ces claviers pour saisir du texte. Sur les appareils Xbox One, des clés individuelles doivent être sélectionnées par déplacement du visuel du focus ou à l’aide des raccourcis produits avec le boîtier de commande ou la télécommande.

![Clavier tactile de Windows 10](images/keyboard/default.png)

***Windows 10 touche clavier***

![Clavier visuel Xbox One](images/keyboard/xbox-onscreen-keyboard.png)

***Clavier à l’écran de Xbox One***

Selon l’appareil, le clavier logiciel s’affiche quand un champ de texte ou un autre contrôle de texte modifiable reçoit le focus, ou lorsque l’utilisateur l’active manuellement via le **Centre de notifications** :

![icône du clavier tactile dans le Centre de notifications](images/keyboard/touch-keyboard-notificationcenter.png)

Si votre application définit le focus par programme sur un contrôle d’entrée de texte, le clavier tactile n’est pas appelé. Cela permet d’éliminer les comportements inattendus non provoqués directement par l’utilisateur. Toutefois, le clavier est automatiquement masqué lorsque le focus est déplacé par programme vers un contrôle autre que d’entrée de texte.

En général, le clavier tactile ne disparaît pas automatiquement tant que l’utilisateur parcourt les contrôles dans un formulaire. Ce comportement peut varier en fonction des autres types de contrôle du formulaire.

Voici la liste des contrôles autres que d’édition qui peuvent recevoir le focus pendant une session d’entrée de texte à l’aide du clavier tactile sans que cela ait pour effet de masquer celui-ci. Au lieu de perturber inutilement l’interface utilisateur au risque de désorienter l’utilisateur, le clavier tactile reste bien en vue, car l’utilisateur va vraisemblablement passer des contrôles à l’entrée de texte à l’aide du clavier tactile.

-   Case à cocher
-   Zone de liste modifiable
-   Case d’option
-   Barre de défilement
-   Tree
-   Tree item
-   Menu
-   Menu bar
-   Menu item
-   ToolBar
-   List
-   List item

Voici quelques exemples des différents modes disponibles pour le clavier tactile. La première image représente la disposition classique, la seconde représente la disposition ergonomique (qui n’est pas forcément disponible dans toutes les langues).

![clavier tactile en mode disposition classique](images/keyboard/default.png)

***La touche du clavier par défaut en mode de disposition***

![clavier tactile en mode disposition étendue](images/keyboard/extendedview.png)

***Le clavier tactile en mode de disposition développée***

Des interactions réussies avec le clavier permettent aux utilisateurs d’accomplir des scénarios d’application de base uniquement à l’aide du clavier. Autrement dit, les utilisateurs peuvent atteindre tous les éléments interactifs et activer leur fonctionnalité par défaut. Plusieurs facteurs peuvent affecter le degré de réussite, tels que la navigation à l’aide du clavier, les touches d’accès pour l’accessibilité et les touches d’accès rapide (ou de raccourci) pour les utilisateurs expérimentés.

**Remarque**  le clavier tactile ne prend pas en charge le bouton bascule et la plupart des commandes du système.

#### <a name="on-screen-keyboard"></a>Clavier visuel
Comme le clavier logiciel, le clavier visuel est un clavier logiciel visuel que vous pouvez utiliser à la place du clavier physique pour entrer des données à l’aide de la fonction tactile, de la souris, du stylo/stylet ou d’un autre périphérique de pointage (un écran tactile n’est pas nécessaire). Le Clavier visuel est fourni pour les systèmes qui ne possèdent pas de clavier physique ou pour les utilisateurs qui connaissent des problèmes de mobilité les empêchant d’utiliser les périphériques d’entrée physiques classiques. Le clavier visuel émule la plupart, sinon la totalité, des fonctionnalités d’un clavier matériel.

Il peut être activé depuis la page Clavier dans Paramètres &gt; Options d’ergonomie.

**REMARQUE** : Le clavier visuel est prioritaire sur le clavier tactile, lequel n’est pas visible si le clavier visuel est présent.

![clavier visuel](images/keyboard/osk.png)

***Clavier visuel***

Consultez la [page du clavier visuel](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard) pour plus d’informations sur le clavier visuel.

## <a name="related-articles"></a>Articles associés

- [Accessibilité du clavier](../accessibility/keyboard-accessibility.md)
