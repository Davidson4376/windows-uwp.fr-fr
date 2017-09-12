---
author: Karl-Bridge-Microsoft
Description: "Apprenez à concevoir et optimiser vos applications UWP afin qu’elles offrent la meilleure expérience possible pour les experts en utilisation du clavier comme pour les utilisateurs atteints de handicaps ou présentant d’autres exigences d’accessibilité."
title: Interactions avec le clavier
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: "clavier, accessibilité, navigation, focus, texte, saisie, interactions utilisateur, boîtier de commande, accès à distance"
ms.author: kbridge
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 22a95cfb77740d7521c62f0b7153130ccdc1b552
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2017
---
# <a name="keyboard-interactions"></a>Interactions avec le clavier
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

![image héros du clavier](images/keyboard/keyboard-hero.jpg)

Apprenez à concevoir et optimiser vos applications UWP afin qu’elles offrent la meilleure expérience possible pour les experts en utilisation du clavier comme pour les utilisateurs atteints de handicaps ou présentant d’autres exigences d’accessibilité.

Quel que soit l’appareil, la saisie au clavier constitue une partie importante de l’expérience d’interaction globale sur la plateforme Windows universelle (UWP). Une expérience bien conçue permet aux utilisateurs de claviers de parcourir efficacement l’interface utilisateur de votre application et d’accéder à toutes ses fonctionnalités sans jamais lever les mains du clavier.

![image clavier et boîtier de commande](images/keyboard/keyboard-gamepad.jpg)

***Les modèles d’interaction communs sont partagés entre le clavier et le boîtier de commande***

Dans cette rubrique, nous mettons l’accent sur la conception des applications UWP pour la saisie au clavier sur PC. Toutefois, une expérience clavier bien conçue est importante pour assurer la prise en charge des outils d’accessibilité tels que le Narrateur Windows avec des claviers logiciels tels que le [clavier tactile](#touch-keyboard) et le [clavier visuel](#osk), ainsi que pour la gestion des autres types de dispositifs de saisie tels que le boîtier de commande Xbox et la télécommande.

Nombre de recommandations abordées ici, y compris en matière de [visuels du focus](#focus-visual), [touches d’accès rapide](#access-keys), et [navigation dans l’interface utilisateur](#navigation), sont également applicables à ces autres scénarios.

**REMARQUE**: bien que les claviers matériels et logiciels soient utilisés pour la saisie de texte, cette rubrique se concentre sur la navigation et l’interaction.

## <a name="built-in-support"></a>Prise en charge intégrée

Avec la souris, le clavier est le périphérique le plus largement utilisé sur les PC et, par conséquent, il constitue une partie essentielle de l’expérience PC. Les utilisateurs de PC attendent une expérience complète et cohérente du système comme des applications individuelles en réponse à la saisie au clavier.

Tous les contrôles UWP intègrent une prise en charge renforçant la richesse des expériences clavier et des interactions utilisateur, tandis que la plateforme proprement dite fournit une base complète pour la création d’expériences clavier mieux adaptée à la personnalisation de vos contrôles et applications.

![image de clavier avec téléphone](images/keyboard/keyboard-phone.jpg)

***UWP prend en charge le clavier avec tous les appareils***

## <a name="basic-experiences"></a>Expériences de base
![Périphériques basés sur le focus](images/keyboard/focus-based-devices.jpg)

Comme mentionné précédemment, les périphériques d’entrée tels que le boîtier de commande Xbox et la télécommande, ainsi que les outils d’accessibilité tels que Narrateur, partagent la plus grande partie de l’expérience de saisie au clavier pour la navigation et les commandes. Cette expérience commune aux types d’entrée et outils limite les tâches supplémentaires pour vous et contribue à l’objectif «générer une seule fois, exécuter partout» de la plateforme Windows universelle.

Le cas échéant, nous allons identifier les principales différences que vous devez connaître et décrire les mesures d’atténuation que vous devez envisager.

Voici les périphériques et outils décrits dans cette rubrique:

| Appareil/outil                       | Description     |
|-----------------------------------|-----------------|
|Clavier (matériel et logiciel)   |Outre le clavier physique standard, les applications UWP prennent en charge deux claviers logiciels: le [tactile (ou clavier logiciel)](#touch-keyboard) et le [clavier visuel](#osk).|
|Boîtier de commande et télécommande         |Le boîtier de commande Xbox et la télécommande sont des périphériques d’entrée fondamentaux pour l’[expérience «10-foot»](designing-for-tv.md).
Pour plus d’informations sur la prise en charge UWP pour le boîtier de commande et la télécommande, voir [Interactions entre le boîtier de commande et la télécommande](gamepad-and-remote-interactions.md).|
|Lecteurs d’écran (Narrateur)          |Narrateur est un lecteur d’écran intégré pour Windows; il fournit des fonctionnalités et expériences d’interaction uniques, mais s’appuie toujours sur la saisie et la navigation de base au clavier.
Pour plus d’informations sur le Narrateur, voir [Prise en main du Narrateur](https://support.microsoft.com/help/22798/windows-10-narrator-get-started).|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Expériences personnalisées et efficacité de la saisie
Comme indiqué, la prise en charge du clavier permet de garantir que vos applications fonctionnent parfaitement pour les utilisateurs aux compétences, capacités et attentes différentes. Nous recommandons de donner la priorité aux actions suivantes.
- Prendre en charge la navigation et de l’interaction avec le clavier
    - Vérifier que les éléments exploitables sont identifiés comme des taquets de tabulation (et que les éléments exploitables ne le sont pas), et que l’ordre de navigation est logique et prévisible (voir [Taquets de tabulation](#tab-stops))
    - Définir le focus initial sur l’élément le plus logique (voir [Focus initial](#initial-focus))
    - Proposer une navigation à l’aide des touches de direction pour les «navigations internes» (voir [Navigation](#navigation))
- Prendre en charge les raccourcis clavier
    - Proposer des touches d’accélération pour les actions rapides (voir [Accélérateurs](#accelerators))
    - Propooser des touches d’accès pour naviguer dans l’interface utilisateur de votre application (voir [Touches d’accès](access-keys.md))

### <a name="focus-visuals-a-namefocus-visual"></a>Visuels du focus <a name="focus-visual">

La plateforme UWP prend en charge un concept unique de visuel du focus qui fonctionne bien pour tous les types d’entrées et d’expériences.
![Visuel du focus](images/keyboard/focus-visual.png)

Un visuel du focus:
-   S’affiche dès qu’un élément d’interface utilisateur reçoit le focus à partir d’un clavier, d’un boîtier de commande et/ou d’une télécommande
-   Est restitué sous forme d’une bordure en surbrillance entourant l’élément d’interface utilisateur pour indiquer qu’une action peut être effectuée
-   Permet à un utilisateur de naviguer dans une interface utilisateur d’application sans se perdre
-   Peut être personnalisé pour votre application (voir [visuels du focus à haute visibilité](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

**Remarque**: le visuel du focus de l’UWP n’est pas identique au rectangle de focus du Narrateur.

### <a name="tab-stops-a-nametab-stops"></a>Taquets de tabulation <a name="tab-stops">

Pour utiliser un contrôle (y compris des éléments de navigation) avec le clavier, il faut que le focus de celui-ci soit positionné sur le contrôle. L’une des méthodes par lesquelles un contrôle peut recevoir le focus par clavier consiste à le rendre accessible par le biais de la navigation par tabulation en l’identifiant comme taquet dans l’ordre de tabulation de votre application.

Pour inclure un contrôle dans l’ordre de tabulation, la propriété [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) doit avoir la valeur**true** (vrai) et la valeur de la propriété [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) doit également être **true.**

Pour exclure spécifiquement un contrôle de l’ordre de tabulation, attribuez à la propriété [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) la valeur **false** (faux).

Par défaut, l’ordre de tabulation reflète l’ordre de création des éléments dans l’interface utilisateur. Par exemple, si un `StackPanel` contient un `Button`, un `Checkbox` et un `TextBox`, l’ordre de tabulation est `Button`, `Checkbox` et `TextBox`.

Vous pouvez remplacer l’ordre de tabulation par défaut en réglant la propriété [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461).

#### <a name="tab-order-should-be-logical-and-predictable"></a>L’ordre de tabulation doit être logique et prévisible

Un modèle bien conçu de navigation au clavier, disposant d’un ordre de tabulation logique et prévisible, rend votre application plus intuitive et aide les utilisateurs à explorer, détecter et accéder aux fonctionnalités plus efficacement.

Contrairement aux contrôles non interactifs, tels que les `labels`, tous les contrôles interactifs doivent comprendre des taquets de tabulation (sauf s’ils figurent dans un [groupe](#control-group)).

Évitez les ordres des tabulations qui font sauter le focus dans votre application. Par exemple, une liste des contrôles placée dans une interface utilisateur de type formulaire doit présenter un ordre de tabulation de gauche à droite et haut en bas.

Voir la page [Accessibilité au clavier](../accessibility/keyboard-accessibility.md) pour plus d’informations sur la personnalisation des taquets de tabulation.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Essayez de coordonner l’ordre de tabulation et l’ordre visuel

La coordination de l’ordre de tabulation et de l’ordre visuel (également appelés ordre de lecture ou d’affichage) permet de réduire la confusion pour les utilisateurs au cours de la navigation dans l’interface utilisateur de votre application.

Essayez de classer et de présenter les commandes, contrôles et éléments de contenu les plus importants en premier, tant dans l’ordre de tabulation que dans l’ordre visuel. Toutefois, la position d’affichage réelle peut dépendre du conteneur de disposition parent et de certaines propriétés des éléments enfants qui influencent la disposition. Spécifiquement, des dispositions qui utilisent une métaphore visuelle de grille ou de tableau peuvent avoir un ordre visuel très différent de l’ordre de tabulation.

**REMARQUE**: l’ordre visuel dépend également des paramètres régionaux et linguistiques.

### <a name="initial-focus-a-nameinitial-focus"></a>Focus initial <a name="initial-focus">

Le focus initial spécifie l’élément d’interface utilisateur qui reçoit le focus lors du premier lancement ou de la première activation d’une application ou d’une page. En cas d’utilisation d’un clavier, c’est à partir de cet élément qu’un utilisateur démarre l’interaction avec l’interface de votre application.

Pour les applications UWP, le focus initial est défini sur l’élément qui peut recevoir le focus (celui dont la valeur [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) est la plus élevée). Les éléments enfants des contrôles des conteneurs sont ignorés. En cas d’égalité, le premier élément de l’arborescence visuelle reçoit le focus.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Définissez le focus initial sur l’élément le plus logique

Définissez le focus initial sur l’élément d’interface utilisateur correspondant à la première action, ou à l’action principale, que les utilisateurs sont susceptibles d’entreprendre lors du lancement de votre application ou pour accéder à une page. Certains exemples comprennent notamment:
-   Une application photographique dans laquelle le focus est défini sur le premier élément d’une galerie
-   Une application de musique dans laquelle le focus est défini sur le bouton lecture

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Ne définissez pas le focus initial sur un élément exposant à un résultat potentiellement négatif, voire désastreux

Ce niveau de fonctionnalité doit être le choix d’un utilisateur. Le fait de définir le focus initial sur un élément entraînant un résultat significatif peut entraîner un accès système ou une perte intempestive de données. Par exemple, ne placez pas le focus sur le bouton de suppression lors de l’accès à un message électronique.

Voir la page [Accessibilité du clavier](../accessibility/keyboard-accessibility.md) pour plus d’informations sur le remplacement de l’ordre de tabulation.

### <a name="navigation-a-namenavigation"></a>Navigation <a name="navigation">

La navigation au clavier est généralement prise en charge via les touches de tabulation et les touches de direction.

![tabulation et touches de direction](images/keyboard/tab-and-arrow.png)

Par défaut, les contrôles UWP suivent ces comportements de base du clavier:
-   **Les touches de tabulation** donnent accès aux contrôles exploitables/actifs dans l’ordre de tabulation.
-   **Maj + Tab** permet d’accéder aux contrôles dans l’ordre inverse de la tabulation. Si l’utilisateur accède au contrôle à l’aide d’une touche de direction, le focus est défini sur la dernière valeur connue à l’intérieur du contrôle.
-   **Les touches de direction** exploitent la «navigation interne» spécifique au contrôle lorsque l’utilisateur accède à ce mode, les touches de direction ne permettent pas d’accéder à l’extérieur d’un contrôle. Certains exemples comprennent notamment:
    -   La touche haut/bas déplace le focus à l’intérieur de `ListView` et `MenuFlyout`
    -   Modifie les valeurs actuellement sélectionnées pour `Slider` et `RatingsControl`
    -   Déplace le point d’insertion à l’intérieur `TextBox`
    -   Développe/réduit des éléments à l’intérieur `TreeView`

Utilisez ces comportements par défaut pour optimiser la navigation au clavier dans votre application.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Utilisez la «navigation interne» avec les jeux de contrôles associés

Le fait de proposer une navigation avec les touches de direction dans un ensemble de contrôles connexes renforce leur relation au sein de l’organisation globale de l’interface utilisateur de votre application.

Par exemple, le contrôle `ContentDialog` illustré ici propose une navigation interne par défaut pour une ligne horizontale de boutons (pour les contrôles personnalisés, voir la section [Groupe de contrôles](#control-group)).

![exemple de boîte de dialogue](images/keyboard/dialog.png)

***L’interaction avec une collection de boutons associés facilite la navigation avec les touches de direction***

Si les éléments sont affichés dans une seule colonne, ils sont accessibles à l’aide des flèches haut/bas. Si les éléments sont affichés sur une seule ligne, ils sont accessibles à l’aide des flèches gauche/droite. Si les éléments sont répartis sur plusieurs colonnes, ils sont accessibles à l’aide des 4 touches de direction.

#### <a name="make-a-set-of-related-controls-a-single-tab-stop"></a>Associer un ensemble de contrôles à un taquet de tabulation

Le fait d’associer un ensemble de contrôles, qu’ils soient liés ou complémentaires, dans un taquet de tabulation unique vous permet de réduire le nombre global de taquets au sein de votre application.

Par exemple, les images suivantes montrent deux contrôles `ListView` empilés. L’image de gauche montre la navigation à l’aide des touches de direction, utilisée avec un taquet de tabulation pour le déplacement entre les contrôles `ListView`, tandis que l’image de droite montre comment simplifier et rationaliser la navigation entre les éléments enfants en éliminant toute nécessité pour parcourir les contrôles parents avec une touche de tabulation.


<table>
  <td>![flèche et tabulation](images/keyboard/arrow-and-tab.png)</td>
  <td>![flèche uniquement](images/keyboard/arrow-only.png)</td>
</table>

***L’interaction avec les deux contrôles ListView empilés peut être simplifiée et rationalisée par l’élimination du taquet de tabulation et en naviguant simplement à l’aide des touches de direction.***

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

La touche **Entrée** peut accomplir des interactions utilisateur courantes et variées, selon le contrôle qui a le focus:
-   Active les contrôles de la commande tels qu’un `Button` ou `Hyperlink`. Pour éviter toute confusion de l’utilisateur final, la touche **Entrée** active également les contrôles ressemblant à des contrôles de commandes tels que `ToggleButton` ou `AppBarToggleButton`.
-   Affiche l’interface utilisateur du sélecteur pour les contrôles tels que `ComboBox` et `DatePicker`. La touche **Entrée** valide également et ferme l’interface utilisateur du sélecteur.
-   Active les contrôles de liste comme `ListView`, `GridView` et `ComboBox`.
    -   La touche **Entrée** exécute l’action de sélection en tant que touche **Espace** pour les éléments de liste et de grille, sauf si une action supplémentaire est associée à ces éléments (en ouvrant une nouvelle fenêtre).
    -   Si une action supplémentaire est associée au contrôle, la touche **Entrée** effectue l’action supplémentaire et la touche **Espace** exécute l’action de sélection.

**REMARQUE**: les touches **Entrée** et **Espace** n’accomplissent pas toujours la même action, bien que ce soit souvent le cas.

![touche Entrée](images/keyboard/enter-key.png)

La touche Échap permet à un utilisateur d’annuler l’interface utilisateur temporaire (ainsi que toutes les actions en cours dans cette interface).

Voici quelques exemples de cette expérience:
-   Un utilisateur ouvre une `ComboBox` avec une valeur sélectionnée et utilise les flèches pour déplacer la sélection du focus vers une nouvelle valeur. L’appui sur la touche Échap ferme le `ComboBox` et rétablit la valeur d’origine de la valeur sélectionnée.
-   L’utilisateur appelle une action de suppression définitive d’un message électronique et est invité à confirmer l’action via un `ContentDialog`. L’utilisateur décide que ce n’est pas l’action prévue et appuie sur la touche **Échap** pour fermer la boîte de dialogue. Comme la touche **Échap** est associée au bouton **Annuler**, la boîte de dialogue se referme et l’action est annulée. La touche **Échap** affecte uniquement l’interface utilisateur temporaire, elle ne ferme pas l’interface utilisateur de l’application et ne permet pas la navigation à rebours.

![Touche Échap](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Touches Début et Fin

Les touches **Début** et **Fin** permettent à un utilisateur d’accéder au début ou la fin d’une zone de l’interface utilisateur.

Voici quelques exemples de cette expérience:
-   Pour les contrôles `ListView` et `GridView`, la touche **Début** déplace le focus vers le premier élément et le fait défiler, tandis que **Fin** déplace le focus vers le dernier élément et le fait défiler.
-   Pour un contrôle `ScrollView`, la touche **Début** provoque un défilement vers le haut de la zone, tandis que **Fin** déclenche un défilement vers le bas (le focus n’est pas modifié).

![touches début et fin](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Touches Page haut et Page bas

Les touches **Page** permettent à un utilisateur de faire défiler une zone de l’interface utilisateur par incréments discrets.

Par exemple, pour les contrôles `ListView` et `GridView`, la touche **Page haut** fait remonter la zone d’une «page» (en règle générale, la hauteur de fenêtre d’affichage) et déplace le focus vers le haut de la zone. Par ailleurs, la touche **Page bas** fait descendre la zone de la valeur d’une page et déplace le focus vers le bas de la zone.

![touches page haut et page bas](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Raccourcis clavier

Les raccourcis clavier peuvent rendre l’utilisation de votre application plus facile et plus efficace.

Outre l’implémentation de la navigation au clavier et l’activation du clavier pour votre application, il est recommandé de mettre en œuvre des raccourcis pour les fonctionnalités de votre application. La navigation par tabulation fournit un bon niveau de prise en charge de base du clavier, mais avec des formulaires complexes vous souhaiterez peut-être ajouter une prise en charge de raccourcis clavier. Cela peut rendre l’utilisation de votre application plus efficace, même pour les personnes qui utilisent à la fois un clavier et des appareils de pointage.

Un raccourci est une combinaison de touches qui améliore la productivité en fournissant à l’utilisateur un moyen efficace d’accéder aux fonctionnalités de l’application. Il existe deux types de raccourcis:
-   Une [touche de raccourci](#accelerators) est un raccourci vers une commande d’application. Votre application peut avoir ou non une interface utilisateur qui correspond exactement à la commande. Les touches de raccourci associent la touche Ctrl et une lettre.
-   Une [touche d’accès rapide](#access-keys) est un raccourci vers un élément de l’interface utilisateur de votre application. Les touches d’accès rapide associent la touche Alt et une lettre.

Consultez cette page où vous trouverez une liste exhaustive des [raccourcis clavier pour Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts) ainsi que des [raccourcis clavier spécifiques aux applications](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps) utilisés par les applications développées par Microsoft.

#### <a name="accelerators-a-nameaccelerators"></a>Accélérateurs <a name="accelerators">

Les accélérateurs permettent aux utilisateurs d’effectuer rapidement des actions courantes de l’application. Pour que les accélérateurs soient utiles mais aussi puissants, il est très important de fournir des touches d’accès rapide cohérentes que les utilisateurs pourront facilement mémoriser et utiliser entre les applications qui accomplissent des tâches similaires.

Exemples d’accélérateurs:
-   L’appui sur les touches Ctrl+N dans une application de messagerie lance un nouvel élément de messagerie.
-   L’appui sur les touches Ctrl+E dans Edge et dans l’application Windows Store permet à utilisateur de saisir rapidement du texte dans la zone de recherche.

Les accélérateurs présentent les caractéristiques suivantes:
-   Ils utilisent principalement des combinaisons basées sur la touche Ctrl et une touche de fonction (les touches de raccourci système de Windows emploient également des combinaisons basées sur la touche Alt et une touche non alphanumérique, ainsi que la touche de logo Windows).
-   Elles visent principalement à renforcer l’efficacité pour les utilisateurs expérimentés.
-   Elles sont attribuées uniquement aux commandes les plus couramment utilisées.
-   Elles sont destinées à être mémorisées et sont indiquées uniquement dans les menus, les info-bulles et l’aide.
-   Elles sont disponibles dans l’ensemble du programme, mais n’ont aucun effet là où elles ne s’appliquent pas.
-   Elles doivent être attribuées de façon cohérente, car elles sont mémorisées et non directement indiquées.

#### <a name="access-keys-a-nameaccess-keys"></a>Touches d’accès <a name="access-keys">

Les touches d’accès rapide constituent pour les utilisateurs ayant des exigences en termes d’accessibilité ainsi que les utilisateurs de clavier avancés un moyen efficace pour naviguer dans l’interface utilisateur de votre application.

Consultez la page [touches d’accès rapide](access-keys.md) où vous trouverez des informations approfondies sur la prise en charge des touches d’accès rapide avec UWP.

Les touches d’accès rapide permettent aux utilisateurs présentant un handicap moteur fonctionnel d’appuyer sur une touche à la fois pour agir sur un élément spécifique de l’interface utilisateur. En outre, les touches d’accès rapide peuvent faire office de touches de raccourci supplémentaires pour aider les utilisateurs avancés à effectuer des actions rapidement.

Les touches d’accès rapide présentent les caractéristiques suivantes:
-   Elles utilisent la touche Alt associée à une touche alphanumérique.
-   Elles visent principalement l’accessibilité.
-   Elles sont documentées directement dans l’interface utilisateur, en regard du contrôle, par le biais de [conseils relatifs aux touches](access-keys.md).
-   Leur incidence se limite à la fenêtre active et elles permettent d’accéder à l’option de menu ou au contrôle correspondant.
-   Elles ne sont pas attribuées de façon cohérente, car ce n’est pas toujours possible. Toutefois, il convient de les attribuer de façon cohérente pour les commandes fréquemment utilisées, en particulier les boutons de validation.
-   Elles sont localisées.

#### <a name="common-keyboard-shortcuts"></a>Raccourcis clavier courants

Le tableau suivant constitue un petit échantillon des commandes au clavier fréquemment utilisées. Pour obtenir la liste complète des commandes au clavier, voir [Touches de raccourci clavier Windows](https://support.microsoft.com/kb/126449).

| Action                               | Touche                                      |
|--------------------------------------|--------------------------------------------------|
| Tout sélectionner                           | Ctrl+A                                           |
| Sélection continue                  | Maj+touche de direction                                  |
| Enregistrer                                 | Ctrl+S                                           |
| Rechercher                                 | Ctrl+F                                           |
| Imprimer                                | Ctrl+P                                           |
| Copier                                 | Ctrl+C                                           |
| Couper                                  | Ctrl+X                                           |
| Coller                                | Ctrl+V                                           |
| Annuler                                 | Ctrl+Z                                           |
| Onglet suivant                             | Ctrl+Tab                                         |
| Fermer l’onglet                            | Ctrl+F4 ou Ctrl+W                                |
| Zoom sémantique                        | Ctrl++ ou Ctrl+-                                 |

## <a name="advanced-experiences"></a>Expériences avancées

Dans cette section, nous aborderons certaines des expériences d’interaction avec le clavier les plus complexes supportées par les applications UWP, ainsi que certains comportements dont vous devez être conscient lorsque votre application est utilisée sur des appareils et avec des outils différents.

### <a name="control-group-a-namecontrol-group"></a>Groupe de contrôle <a name="control-group">

Vous pouvez regrouper un ensemble de contrôles associés ou complémentaires, dans un «groupe de contrôle» (ou une zone directionnelle), ce qui permet la «navigation interne» à l’aide des touches de direction. Le groupe de contrôle peut être un taquet de tabulation, mais vous pouvez aussi spécifier plusieurs taquets de tabulation au sein du groupe de contrôle.

#### <a name="arrow-key-navigation"></a>Navigation avec les touches de direction

Les utilisateurs s’attendent à la prise en charge de la navigation à l’aide des touches de direction lorsqu’il existe un groupe de contrôles similaires dans une région de l’interface utilisateur:
-   `AppBarButtons` dans un `CommandBar`
-   `ListItems` ou `GridItems` à l’intérieur `ListView` ou `GridView`
-   `Buttons` à l’intérieur `ContentDialog`

Par défaut, les contrôles d’UWP prennent en charge la navigation avec les touches de direction. Pour les dispositions personnalisées et les groupes de contrôle, utilisez `XYFocusKeyboardNavigation="Enabled"` pour fournir un comportement similaire.

Envisagez d’ajouter la prise en charge de la navigation avec les touches de direction lorsque vous disposez des contrôles suivants:

<table>
  <tr>
    <td>
      <p>![boîte de dialogue](images/keyboard/dialog.png)</p>
      <p>**Boutons**</p>
      <p>![radiobutton](images/keyboard/radiobutton.png)</p>
      <p>**RadioButtons**</p>     
    </td>
    <td>
      <p>![appbar](images/keyboard/appbar.png)</p>
      <p>**AppBarButtons**</p>
      <p>![éléments de liste et grille](images/keyboard/list-and-grid-items.png)</p>
      <p>**Éléments ListItems et GridItems**</p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Taquets de tabulation

En fonction des fonctionnalités et de l’agencement de votre application, la meilleure option de navigation pour un groupe de contrôle peut être un taquet de tabulation unique avec accès vers les éléments enfants par les touches de direction, plusieurs taquets de tabulation ou une combinaison.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Utilisez plusieurs taquets de tabulation et les touches de direction pour les boutons

Les utilisateurs d’accessibilité s’appuient sur des règles bien établies de navigation au clavier, qui n’utilisent généralement pas les touches de direction pour naviguer dans une collection de boutons. Toutefois, les utilisateurs sans déficience visuelle peuvent penser que le comportement est naturel.

Un exemple de comportement par défaut d’UWP dans ce cas est le `ContentDialog`. Même si les touches de direction peuvent être utilisées pour naviguer entre les boutons, chaque bouton est également un taquet de tabulation.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Affecter un taquet de tabulation unique aux modèles familiers de l’interface utilisateur

Dans les cas où votre agencement suit un modèle bien connu d’interface utilisateur pour les groupes de contrôle, l’affectation au groupe d’un taquet de tabulation unique peut améliorer l’efficacité de la navigation pour les utilisateurs.

Par exemple:
-   `RadioButtons`
-   Plusieurs `ListViews` qui ont l’aspect d’une seule et se comportent comme tel `ListView`
-   Toute interface utilisateur conçue pour ressembler et se comporter comme une grille de vignettes (par exemple, les vignettes du menu Démarrer)

#### <a name="specifying-control-group-behavior"></a>Spécification du comportement de groupe de contrôles

Utilisez les API suivantes pour prendre en charge le comportement de groupes de contrôle personnalisés (toutes sont détaillées plus loin dans cette rubrique):

-   [XYFocusKeyboardNavigation](custom-keyboard-interactions.md#xyfocuskeyboardnavigation) permet une navigation par touches de direction entre les contrôles
-   [TabFocusNavigation](custom-keyboard-interactions.md#tab-navigation) indique s’il existe plusieurs taquets de tabulation ou un taquet unique
-   [FindFirstFocusableElement et FindLastFocusableElement](managing-focus-navigation.md#findfirstfocusableelement) définissent le focus sur le premier élément avec la touche **Début** et sur le dernier élément avec la touche **Fin**

L’image suivante montre un comportement de navigation intuitive au clavier pour un groupe de contrôles composé de cases d’options associées. Dans ce cas, nous recommandons un taquet de tabulation unique pour le groupe de contrôles, une navigation interne entre les cases d’option à l’aide des touches de direction, la liaison de **Début** avec la première case d’option et de **Fin** avec la dernière.

![synthèse de tous les éléments](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Clavier et Narrateur

Le Narrateur est un outil d’accessibilité de l’interface utilisateur orienté vers les utilisateurs de claviers (les autres types d’entrées sont également pris en charge). Toutefois, la fonctionnalité du Narrateur va au-delà des interactions avec le clavier prises en charge par les applications UWP et une attention supplémentaire est requise lors de la conception de votre application UWP pour le Narrateur. (La [page des notions de base du Narrateur](https://support.microsoft.com/help/22808/windows-10-narrator-learning-basics) vous guide tout au long de l’expérience utilisateur du Narrateur.)

Voici quelques-unes des différences entre les comportements de clavier avec UWP et ceux pris en charge par le Narrateur:
-   Des combinaisons de touches supplémentaires sont prévues pour accéder aux éléments d’interface utilisateur qui ne sont pas exposés par la navigation standard au clavier, tels que la lecture des étiquettes de contrôle avec Ver Maj + touches de direction.
-   Navigation vers les éléments désactivés. Par défaut, les éléments désactivés ne sont pas exposés par la navigation standard au clavier.
    -   Contrôlez les «affichages» pour une navigation plus rapide basée sur la granularité de l’interface utilisateur. Les utilisateurs peuvent accéder à des éléments, caractères, mots, lignes, paragraphes, liens, titres, tableaux, repères et suggestions. La navigation standard au clavier expose ces objets sous forme de liste plate, ce qui peut rendre la navigation fastidieuse si vous ne fournissez pas de touches de raccourci.

#### <a name="case-study--autosuggestbox-control"></a>Étude de cas – le contrôle AutoSuggestBox

Le bouton Rechercher de `AutoSuggestBox` n’est pas accessible par la navigation standard au clavier avec les touches de direction et de tabulation, car l’utilisateur peut appuyer sur la touche **Entrée** pour soumettre la requête de recherche. Toutefois, il est accessible via le Narrateur lorsque l’utilisateur appuie sur Ver Maj + une touche de direction.

![suggestion automatique du focus du clavier](images/keyboard/auto-suggest-keyboard.png)

***Avec le clavier***, *les utilisateurs utilisent* ***la touche Entrée*** *pour envoyer la requête de recherche*

<table>
  <tr>
    <td>
      <p>![suggestion automatique du focus du narrateur](images/keyboard/auto-suggest-narrator-1.png)</p>
      <p>**Le Narrateur** *permet aux utilisateurs d’utiliser la touche entrée pour envoyer la requête de recherche*</P>
    </td>
    <td>
      <p>![suggestion automatique du focus du narrateur pendant la recherche](images/keyboard/auto-suggest-narrator-2.png)</p>
      <p>*L’utilisateur peut également accéder au bouton de recherche par Verr Maj. + flèche droite, puis appuyer sur la touche espace*</p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>Le clavier, la télécommande et le boîtier de commande Xbox

Les télécommandes et boîtiers de commande Xbox prennent en charge de nombreux comportements et expériences de clavier UWP. Toutefois, en raison de l’absence de diverses options de touches disponibles par rapport à un clavier, le boîtier de commande et la télécommande ne disposent pas des nombreuses optimisations propres au clavier (la télécommande est encore plus limitée que le boîtier de commande).

Voir [Conception pour Xbox et télévision](designing-for-tv.md#gamepad-and-remote-control) pour plus d’informations sur la prise en charge par UWP du boîtier de commande et des télécommandes.

Voici quelques correspondances de touches entre le clavier, le boîtier de commande et la télécommande.

| **Clavier**  | **Boîtier de commande**                         | **Télécommande**  |
|---------------|-------------------------------------|---------------------|
| Espace         | BoutonA                            | Bouton Sélection       |
| Entrée         | BoutonA                            | Bouton Sélection       |
| Échap        | BoutonB                            | Bouton Précédent         |
| Début/Fin      | Non applicable                                 | Non applicable                 |
| Page haut/bas  | Bouton de déclenchement du défilement vertical, bouton pare-chocs du défilement horizontal   | Non applicable                 |

Voici certaines différences clés, dont vous devez être conscient lors de la conception de votre application UWP pour une utilisation avec le boîtier de commande et la télécommande:
-   La saisie de texte exige de l’utilisateur qu’il appuie sur A pour activer un contrôle de texte.
-   La navigation en mode focus ne se limite pas au contrôle des groupes, les utilisateurs peuvent naviguer librement vers tous les éléments de l’interface utilisateur sélectionnables dans l’application.

    **REMARQUE**: le focus peut se déplacer vers tout élément d’interface utilisateur sélectionnable dans le sens indiqué par la touche pressée, sauf s’il se trouve dans un segment de chevauchement de l’interface ou si une [activation du focus](designing-for-tv.md#focus-engagement) est spécifiée, ce qui empêche le focus d’entrer/sortir d’une zone tant qu’il n’y a pas d’enclenchement/activation avec le bouton A. Pour plus d’informations, voir la section [navigation directionnelle](#directional-navigation).
-   Le bouton multidirectionnel et la touche gauche du stick analogique sont utilisés pour déplacer le focus entre les contrôles ainsi que pour la navigation interne.

    **REMARQUE**: le boîtier de commande et la télécommande ne permettent d’accéder qu’aux éléments qui se trouvent dans le même ordre visuel lorsque la touche de direction est enfoncée. La navigation est désactivée dans cette direction lorsqu’aucun élément suivant ne peut recevoir le focus. Selon la situation, les utilisateurs du clavier ne sont pas toujours confrontés à cette contrainte. Consultez la section [Optimisation de clavier intégré](#built-in-keyboard-optimization) pour en savoir plus.

#### <a name="directional-navigation-a-namedirectional-navigation"></a>Navigation directionnelle <a name="directional-navigation">

La navigation directionnelle est gérée par une classe d’assistance UWP [Gestionnaire de Focus](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) qui enregistre la touche directionnelle (touche de direction, bouton multidirectionnel) et tente de déplacer le focus dans la direction visuelle correspondante.

Contrairement au mode clavier, lorsqu’une application choisit le [Mode souris](designing-for-tv.md#mouse-mode), la navigation directionnelle est activée dans toute l’application pour le boîtier de commande et la télécommande. Consultez l’article [Interaction et navigation en mode focusXY](designing-for-tv.md#xy-focus-navigation-and-interaction) pour en savoir plus sur les optimisations de la navigation directionnelle pour le boîtier de commande et la télécommande.

**REMARQUE**: la navigation à l’aide de la touche de tabulation du clavier n’est pas considérée comme une navigation directionnelle. Pour en savoir plus, voir la section [Taquets de tabulation](#tab-stops).

<table>
  <tr>
    <td>
      <p>![navigation directionnelle](images/keyboard/directional-navigation.png)</p>
      <p>***Navigation directionnelle prise en charge*** </br>*L’utilisateur peut naviguer entre les différents contrôles à l’aide des touches de direction (flèches du clavier, boîtier de commande et bouton multidirectionnel de la télécommande), .*</p>
    </td>
    <td>
      <p>![pas de navigation directionnelle](images/keyboard/no-directional-navigation.png)</p>
      <p>***Navigation directionnelle non prise en charge*** </br>*L’utilisateur ne peut pas naviguer entre les différents contrôles à l’aide des touches de direction. Les autres méthodes de navigation entre les contrôles (touche de tabulation) ne sont pas affectées.*</p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization-a-namebuilt-in-keyboard-optimization"></a>Optimisation de clavier intégré <a name="built-in-keyboard-optimization">

En fonction de la disposition et des contrôles utilisés, les applications UWP peuvent être optimisées spécifiquement pour la saisie au clavier.

L’exemple qui suit montre un groupe d’éléments de liste, d’éléments de grille et d’éléments de menu qui ont été affectés à un taquet de tabulation (voir la section [Taquets de tabulation](#tab-stops)). Lorsque le groupe a le focus, la navigation interne s’effectue à l’aide des touches de direction dans l’ordre visuel correspondant (voir la section [Navigation](#navigation)).

![navigation avec les touches de direction dans une colonne unique](images/keyboard/single-column-arrow.png)

***Navigation avec les touches de direction dans une colonne unique***

![navigation avec les touches de direction dans une ligne unique](images/keyboard/single-row-arrow.png)

***Navigation avec les touches de direction dans une ligne unique***

![navigation avec les touches de direction dans plusieurs colonnes et lignes](images/keyboard/multiple-column-and-row-navigation.png)

***Navigation avec les touches de direction dans plusieurs colonnes et lignes***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Emballage d’éléments homogènes d’affichage de liste et de grille

La navigation directionnelle n’est pas toujours le moyen le plus efficace pour parcourir plusieurs lignes et colonnes d’éléments de liste et de type GridView.

**REMARQUE**: les éléments de menu sont généralement des listes à une seule colonne, mais des règles spéciales de focus peuvent s’appliquer dans certains cas (voir [Interface utilisateur contextuelle](#popup-ui)).

Les objets de liste et de grille peuvent être créés avec plusieurs lignes et colonnes. Ils sont généralement ordonnés en mode «row-major» (où les éléments remplissent d’abord toute la ligne avant de passer à la ligne suivante) ou en mode «column-major» (où les éléments remplissent d’abord toute la colonne avant de passer à la colonne suivante). L’ordre par ligne (row-major) ou par colonne (column-major) dépend du sens de défilement, et vous devez vous assurer que l’ordre des éléments n’est pas en conflit avec ce sens.

Dans le cas d’un ordonnancement en mode «row-major» (où les éléments sont renseignés de gauche à droite et de haut en bas), lorsque le focus est sur le dernier élément d’une ligne et que la touche flèche droite est enfoncée, le focus se déplace vers le premier élément de la ligne suivante. Ce comportement concerne également l’ordre inverse: lorsque le focus est placé sur le premier élément d’une ligne et que la touche flèche gauche est enfoncée, le focus se déplace vers le dernier élément de la ligne précédente.

Dans le cas d’un ordonnancement en mode «column-major» (où les éléments sont renseignés de haut en bas et de gauche à droite), lorsque le focus est sur le dernier élément d’une colonne et que l’utilisateur appuie sur la touche flèche bas, le focus se déplace vers le premier élément de la colonne suivante. Ce comportement concerne également l’ordre inverse: lorsque le focus est placé sur le premier élément d’une colonne et que la touche flèche haut est enfoncée, le focus se déplace vers le dernier élément de la colonne précédente.

<table>
  <tr>
    <td>
      <p>![navigation clavier par ordre row-major](images/keyboard/row-major-keyboard.png)</p>
      <p>***Navigation clavier par ordre row-major***</p>
    </td>
    <td>
      <p>![navigation clavier par ordre column-major](images/keyboard/column-major-keyboard.png)</p>
      <p>***Navigation clavier par ordre column-major***</p>
    </td>
  </tr>
</table>

#### <a name="popup-ui-a-namepopup-ui"></a>Interface utilisateur contextuelle <a name="popup-ui">

Comme indiqué, vous devez essayer de garantir une navigation directionnelle correspondant à l’ordre visuel des contrôles de l’interface utilisateur de votre application.

Certains contrôles, tels que `ContextMenu`, `AppBarOverflowMenu`, et `AutoSuggest`, comportent un menu contextuel qui s’affiche dans un emplacement et une direction relative au contrôle principal (en fonction de l’espace d’écran disponible). Par exemple, lorsque l’espace est insuffisant pour que le menu s’ouvre vers le bas (la direction par défaut), il s’ouvre vers le haut. Il n’existe aucune garantie que le menu s’ouvre à chaque fois dans le même sens.

<table>
  <td>![la barre de commandes s’ouvre vers le bas avec la touche flèche bas](images/keyboard/command-bar-open-down.png)</td>
  <td>![la barre de commandes s’ouvre vers le haut avec la touche flèche bas](images/keyboard/command-bar-open-up.png)</td>
</table>

Pour ces contrôles, lors de la première ouverture du menu (et si aucun élément n’a été sélectionné par l’utilisateur), la touche flèche bas place toujours le focus sur le premier élément et la touche flèche haut le place toujours sur le dernier élément du menu. De même, lorsque le dernier élément est sélectionné et lorsque la touche flèche bas est activée, le focus se déplace sur le premier élément du menu et lorsque le premier élément est sélectionné et que l’utilisateur appuie sur la touche flèche haut, le focus se déplace vers le dernier élément du menu.

Vous devez tenter d’émuler ces mêmes comportements dans vos contrôles personnalisés. Un exemple de code montrant comment implémenter ce comportement se trouve dans la documentation [Gestion de la navigation en mode focus](managing-focus-navigation.md#popup-ui-code-sample).

## <a name="test-your-app"></a>Tester votre application

Testez votre application avec tous les périphériques d’entrée pris en charge pour garantir l’utilisation cohérente et intuitive des éléments d’interface utilisateur et vous assurer qu’aucun élément inattendu n’interfère avec l’ordre de tabulation souhaité.

## <a name="related-articles"></a>Articles connexes
* [Événements de clavier](keyboard-events.md)
* [Identifier des périphériques d’entrée](identify-input-devices.md)
* [Répondre à la présence du clavier tactile](respond-to-the-presence-of-the-touch-keyboard.md)
* [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)

## <a name="appendix"></a>Annexe

### <a name="software-keyboard-a-nametouch-keyboard"></a>Clavier logiciel <a name="touch-keyboard">

Le clavier logiciel est un clavier qui s’affiche sur l’écran et que l’utilisateur peut utiliser à la place du clavier physique pour entrer des données à l’aide de la fonction tactile, de la souris, du stylo/stylet ou d’un autre périphérique de pointage (un écran tactile n’est pas nécessaire). Sur l’écran tactile, il suffit de toucher directement ces claviers pour saisir du texte. Sur les appareils XboxOne, des clés individuelles doivent être sélectionnées par déplacement du visuel du focus ou à l’aide des raccourcis produits avec le boîtier de commande ou la télécommande.

![Clavier tactile de Windows10](images/keyboard/kbdpcdefault.png)

***Clavier tactile de Windows10***

![Clavier tactile de Windows10Phone](images/keyboard/kbdwpdefault.png)

***Clavier tactile de Windows10Phone***

![Clavier visuel XboxOne](images/keyboard/xbox-onscreen-keyboard.png)

***Clavier visuel XboxOne***

Selon l’appareil, le clavier logiciel s’affiche quand un champ de texte ou un autre contrôle de texte modifiable reçoit le focus, ou lorsque l’utilisateur l’active manuellement via le **Centre de notifications**:

![icône du clavier tactile dans le Centre de notifications](images/keyboard/touch-keyboard-notificationcenter.png)

Si votre application définit le focus par programme sur un contrôle d’entrée de texte, le clavier tactile n’est pas appelé. Cela permet d’éliminer les comportements inattendus non provoqués directement par l’utilisateur. Toutefois, le clavier est automatiquement masqué lorsque le focus est déplacé par programme vers un contrôle autre que d’entrée de texte.

En général, le clavier tactile ne disparaît pas automatiquement tant que l’utilisateur parcourt les contrôles dans un formulaire. Ce comportement peut varier en fonction des autres types de contrôle du formulaire.

Voici la liste des contrôles autres que d’édition qui peuvent recevoir le focus pendant une session d’entrée de texte à l’aide du clavier tactile sans que cela ait pour effet de masquer celui-ci. Au lieu de perturber inutilement l’interface utilisateur au risque de désorienter l’utilisateur, le clavier tactile reste bien en vue, car l’utilisateur va vraisemblablement passer des contrôles à l’entrée de texte à l’aide du clavier tactile.

-   Check box
-   Combo box
-   Radio button
-   Scroll bar
-   Tree
-   Tree item
-   Menu
-   Menu bar
-   Menu item
-   ToolBar
-   List
-   List item

Voici quelques exemples des différents modes disponibles pour le clavier tactile. La première image représente la disposition classique, la seconde représente la disposition ergonomique (qui n’est pas forcément disponible dans toutes les langues).

![clavier tactile en mode disposition classique](images/keyboard/touchkeyboard-standard.png)

***Clavier tactile en mode disposition classique***

![clavier tactile en mode disposition étendue](images/keyboard/touchkeyboard-expanded.png)

***Clavier tactile en mode disposition étendue***

![clavier tactile en mode disposition ergonomique](images/keyboard/touchkeyboard-thumb.png)

***Clavier tactile en mode disposition ergonomique classique***

![clavier tactile en mode disposition ergonomique numérique](images/keyboard/touchkeyboard-numeric-thumb.png)

***Clavier tactile en mode disposition ergonomique numérique***

Des interactions réussies avec le clavier permettent aux utilisateurs d’accomplir des scénarios d’application de base uniquement à l’aide du clavier. Autrement dit, les utilisateurs peuvent atteindre tous les éléments interactifs et activer leur fonctionnalité par défaut. Plusieurs facteurs peuvent affecter le degré de réussite, tels que la navigation à l’aide du clavier, les touches d’accès pour l’accessibilité et les touches d’accès rapide (ou de raccourci) pour les utilisateurs expérimentés.

**REMARQUE**: Le clavier tactile ne prend pas en charge le basculement ni la plupart des commandes système.

#### <a name="on-screen-keyboard-a-nameosk"></a>Clavier visuel <a name="osk">
Comme le clavier logiciel, le clavier visuel est un clavier logiciel visuel que vous pouvez utiliser à la place du clavier physique pour entrer des données à l’aide de la fonction tactile, de la souris, du stylo/stylet ou d’un autre périphérique de pointage (un écran tactile n’est pas nécessaire). Le Clavier visuel est fourni pour les systèmes qui ne possèdent pas de clavier physique ou pour les utilisateurs qui connaissent des problèmes de mobilité les empêchant d’utiliser les périphériques d’entrée physiques classiques. Le clavier visuel émule la plupart, sinon la totalité, des fonctionnalités d’un clavier matériel.

Il peut être activé depuis la page Clavier dans Paramètres &gt; Options d’ergonomie.

**REMARQUE**: Le clavier visuel est prioritaire sur le clavier tactile, lequel n’est pas visible si le clavier visuel est présent.

![clavier visuel](images/keyboard/osk.png)

***Clavier visuel***

Consultez la [page du clavier visuel](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard) pour plus d’informations sur le clavier visuel.
