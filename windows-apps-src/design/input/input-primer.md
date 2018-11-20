---
author: Karl-Bridge-Microsoft
Description: User interactions in the Universal Windows Platform (UWP) are a combination of input and output sources (such as mouse, keyboard, pen, touch, touchpad, speech, Cortana, controller, gesture, gaze, and so on), along with various modes or modifiers that enable extended experiences (including mouse wheel and buttons, pen eraser and barrel buttons, touch keyboard, and background app services).
title: Notions fondamentales sur les interactions
ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8
label: Interaction primer
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9babc1f96b83123cef4bf103f4d13696697cc897
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7287566"
---
# <a name="interaction-primer"></a>Notions fondamentales sur les interactions

![types d’entrée Windows](images/input-interactions/icons-inputdevices03.png)

Dans la plateforme Windows universelle (UWP), les interactions utilisateur reposent sur une combinaison de sources d’entrée et de sortie (souris, clavier, stylet, interaction tactile, pavé tactile, fonctions vocales, **Cortana**, contrôleur, mouvement, regard, etc.), ainsi que sur différents modes ou modificateurs qui permettent des expériences étendues (roulette et boutons de souris, bouton et gomme du stylet, clavier tactile, services d’application en arrière-plan, etc.).

La plateforme UWP utilise un système d’interactions contextuelles intelligent qui, dans la plupart des cas, élimine la nécessité de traiter individuellement les types d’entrées uniques reçus par votre application. Cela inclut la gestion des entrées effectuées par le biais d’une interface tactile, d’un pavé tactile, d’une souris et d’un stylet en tant que type de pointeur générique pour prendre en charge les actions statiques (comme l’appui ou l’appui long), les actions de manipulation (comme le glissement pour le mouvement panoramique) ou le rendu d’entrée manuscrite.

Familiarisez-vous avec chaque type de périphérique d’entrée, ses comportements, ses fonctionnalités et ses limites, selon son association à certains facteurs de forme. Cela vous aidera à déterminer si les contrôles et l’intuitivité de la plateforme sont suffisants pour votre application ou requièrent une personnalisation de l’expérience d’interaction utilisateur.

## <a name="gaze"></a>Pointage du regard

Pour la **Mise à jour d'avril2018 de Windows10**, nous avons introduit la prise en charge de l'entrée du pointage de regard à l’aide de périphériques d’entrée par suivi oculaire et suivi de la tête. 

> [!NOTE]
> La prise en charge du matériel de suivi oculaire a été introduite dans **Windows10 Fall Creators Update** avec le [contrôle visuel](https://support.microsoft.com/en-us/help/4043921/windows-10-get-started-eye-control), une fonctionnalité intégrée qui vous permet d’utiliser vos yeux pour contrôler le pointeur à l'écran, taper avec le clavier visuel et communiquer avec des personnes à l’aide de la synthèse vocale.

### <a name="device-support"></a>Périphériques pris en charge

- Tablette
- PC et portables

### <a name="typical-usage"></a>Utilisation type

Effectuez le suivi du pointage du regard, de l'attention et de la présence de l'utilisateur en fonction de l’emplacement et des mouvements de ses yeux. Ce nouveau moyen puissant d'utiliser vos applications UWP et d'interagir avec elles est particulièrement utile comme technologie d’assistance pour les utilisateurs souffrant de maladies neuro-musculaires (comme la SLA) ou d’autres handicaps impliquant des troubles des fonctions musculaires ou neurologiques. L'entrée du pointage de regard offre également de remarquables opportunités pour les jeux (notamment pour l'acquisition de cible et le suivi), les applications de productivité traditionnelles, les bornes et autres scénarios interactifs dans lesquels les périphériques classiques (clavier, souris entrée tactile) ne sont pas disponibles ou il peut être utile de libérer des mains de l’utilisateur pour d’autres tâches (par exemple, pour tenir des sacs).

### <a name="more-info"></a>Informations supplémentaires

[Interactions avec le pointage du regard et le suivi oculaire](gaze-interactions.md)

## <a name="surface-dial"></a>SurfaceDial

À l’occasion de la **Mise à jour anniversaire Windows10**, nous avons introduit la catégorie de périphériques de saisie Windows Wheel. Surface Dial est le premier de cette classe de périphérique.

### <a name="device-support"></a>Prise en charge des appareils

- Tablette
- PC et portables

### <a name="typical-usage"></a>Utilisation type

Avec un format appelant à une action de rotation (ou un mouvement), Surface Dial est conçu à la manière d’un périphérique de saisie secondaire multimode venant compléter ou modifier la saisie à partir d’un périphérique principal. Dans la plupart des cas, l’utilisateur manipule le périphérique avec la main qu’il utilise le moins tout en effectuant une tâche avec sa main dominante (par exemple, la saisie manuscrite avec un stylet).

### <a name="more-info"></a>Informations supplémentaires

[Recommandations de conception de Surface Dial](windows-wheel-interactions.md)

## <a name="cortana"></a>Cortana

Dans Windows 10, l’extensibilité **de Cortana** vous permet de gérer les commandes vocales à partir d’un utilisateur et lancer votre application pour effectuer une action unique.

### <a name="device-support"></a>Prise en charge des appareils

-   Téléphones et phablettes
-   Tablette
-   PC et portables
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![cortana](images/input-interactions/icons-cortana01.png)

### <a name="typical-usage"></a>Utilisation type

Une commande vocale est un énoncé, défini dans un fichier VCD (Voice Command Definition), qui est dirigé vers une application installée via **Cortana**. L’application peut être lancée au premier plan ou en arrière-plan en fonction du niveau et de la complexité de l’interaction. Par exemple, les commandes vocales qui requièrent un contexte ou une entrée utilisateur supplémentaire sont mieux gérées au premier plan, tandis que les commandes de base peuvent être gérées en arrière-plan.

L’intégration des fonctionnalités de base de votre application et la fourniture d’un point d’entrée central pour que l’utilisateur accomplisse la plupart des tâches sans ouvrir directement votre application, permettent à **Cortana** de devenir une liaison entre votre application et l’utilisateur. Dans de nombreux cas, cela permet à l’utilisateur de gagner beaucoup de temps et d’énergie. Pour plus d’informations, voir [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233).

### <a name="more-info"></a>Informations supplémentaires

[Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
 

## <a name="speech"></a>Fonctions vocales

Les fonctions vocales offrent un moyen efficace et naturel d’interagir avec les applications. Elles assurent une communication simple et précise avec les applications et permettent aux utilisateurs d’être productifs et de se tenir informés dans diverses situations.

Les fonctions vocales peuvent compléter ou, souvent, constituer le type d’entrée principal, selon l’appareil de l’utilisateur. Par exemple, les appareils tels que HoloLens et la Xbox ne prennent pas en charge les types d’entrée traditionnels (hormis un clavier logiciel dans certains cas). Au lieu de cela, ils s’appuient sur une entrée et une sortie vocales (souvent combinées à d’autres types d’entrée non traditionnels comme le regard et le mouvement) pour la plupart des interactions utilisateur.

La conversion de texte par synthèse vocale (ou TTS) est utilisée pour informer ou guider l’utilisateur.

### <a name="device-support"></a>Prise en charge des appareils

-   Téléphones et phablettes
-   Tablette
-   PC et portables
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![fonctions vocales](images/input-interactions/icons-speech01.png)

### <a name="typical-usage"></a>Utilisation type

Il existe trois modes d’interaction vocale:

**Langage naturel**

Le langage naturel est la façon dont nous interagissons verbalement avec d’autres personnes de façon régulière. Notre discours varie d’une personne à l’autre et d’une situation à l’autre, et il est généralement compris. Lorsque ce n’est pas le cas, nous utilisons souvent d’autres mots et séquences de mots pour formuler la même idée.

Les interactions en langage naturel avec une application sont similaires: nous parlons à l’application par le biais de notre appareil comme s’il s’agissait d’une personne et attendons de l’application qu’elle comprenne et réagisse en conséquence.

Le langage naturel est le mode d’interaction vocale le plus avancé. Il peut être implémenté et exposé par le biais de **Cortana**.

**Commande et contrôle**

La commande et le contrôle représentent l’utilisation de commandes verbales pour activer des contrôles et fonctionnalités comme un clic sur un bouton ou la sélection d’un élément de menu.

Ce sont des aspects essentiels d’une expérience utilisateur réussie. Par conséquent, il n’est pas recommandé de prévoir un seul type d’entrée. Les fonctions vocales sont généralement proposées en complément d’autres options d’entrée, selon les préférences de l’utilisateur ou les fonctionnalités matérielles.

**Dictée**

La méthode d’entrée vocale la plus simple. Chaque énoncé est converti en texte.

La dictée est généralement utilisée lorsqu’une application n’a pas besoin de comprendre la signification ou l’intention des phrases.

### <a name="more-info"></a>Informations supplémentaires

[Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)
 

## <a name="pen"></a>Stylet

Un stylet peut servir de dispositif de pointage précis au niveau des pixels, tel que la souris. Il constitue également l’appareil d’entrée manuscrite numérique optimal.

**Remarque**il existe deux types de stylet: actif et passif.
  -   Les stylets passifs ne contiennent pas d’éléments électroniques et peuvent émuler efficacement des entrées tactiles au doigt. Ils requièrent un écran de base reconnaissant les entrées en fonction de la pression de contact. Dans la mesure où les utilisateurs posent souvent leur main lorsqu’ils écrivent sur la surface d’entrée, les données d’entrée peuvent être altérées en raison d’une mauvaise élimination des interférences de la paume.
  -   Les stylets actifs contiennent des éléments électroniques et peuvent fonctionner avec des écrans d’appareil complexes. Ils peuvent ainsi fournir des données d’entrée beaucoup plus étendues (pointage ou données de proximité, par exemple) au système et à votre application. L’élimination des interférences de la paume est beaucoup plus robuste.

Lorsque nous parlons de stylets, nous faisons référence à des stylets actifs qui fournissent des données d’entrée riches et qui sont utilisés principalement dans les interactions de pointage et d’écriture précises.

### <a name="device-support"></a>Prise en charge des appareils

-   Téléphones et phablettes
-   Tablette
-   PC et portables
-   Surface Hub
-   IoT

![stylet](images/input-interactions/icons-pen01.png)

### <a name="typical-usage"></a>Utilisation type

La plateforme d’entrée manuscrite de Windows, associée à un stylet, fournit un moyen naturel de créer des notes manuscrites, des dessins et des annotations. Cette plateforme prend en charge la capture de données d’entrée manuscrite à partir de l’entrée du numériseur, la génération de données d’entrée manuscrite, le rendu de ces données sous forme de traits d’encre sur le périphérique de sortie, la gestion des données d’entrée manuscrite et la reconnaissance de l’écriture manuscrite. En plus de capturer les mouvements spatiaux du stylet à mesure que l’utilisateur écrit ou dessine, votre application peut également recueillir des informations telles que la pression, la forme, la couleur et l’opacité, de manière à offrir des expériences utilisateur ressemblant étroitement au dessin réel à l’aide d’un stylo, d’un crayon ou d’un pinceau.

Les entrées tactiles et du stylet divergent en raison de la capacité de l’entrée tactile à émuler la manipulation directe des éléments de l’interface utilisateur sur l’écran par le biais de mouvements physiques effectués sur ces objets (comme le balayage, le glissement, la rotation, etc.).

Il est recommandé de fournir des commandes d’interface utilisateur spécifiques au stylet (ou un élément incitatif) pour prendre en charge ces interactions. Par exemple, utilisez les boutons Précédent et Suivant (ou + et -) pour permettre aux utilisateurs de tourner les pages de contenu ou de faire pivoter, de redimensionner et d’agrandir les objets.

### <a name="more-info"></a>Informations supplémentaires

[Recommandations en matière de conception pour le stylet](https://msdn.microsoft.com/library/windows/apps/dn456352)
 

## <a name="touch"></a>Interface tactile

Avec une interface tactile, les mouvements physiques avec un ou plusieurs doigts peuvent servir à émuler la manipulation directe d’éléments d’interface utilisateur (par exemple, le mouvement panoramique, la rotation, le redimensionnement ou le déplacement), ou faire office de méthode d’entrée alternative (semblable à la souris ou au stylet) ou complémentaire (pour modifier l’aspect d’autres entrées, comme par exemple maculer un trait d’encre dessiné avec un stylet). Les expériences tactiles peuvent fournir des sensations plus naturelles et proches de celles du monde réel pour les utilisateurs lorsqu’ils interagissent avec des éléments sur un écran.

### <a name="device-support"></a>Prise en charge des appareils

-   Téléphones et phablettes
-   Tablette
-   PC et portables
-   Surface Hub
-   IoT

![interface tactile](images/input-interactions/icons-touch01.png)

### <a name="typical-usage"></a>Utilisation type

La prise en charge des entrées tactiles peut varier de manière significative selon l’appareil.

Certains appareils ne prennent en charge aucune fonction tactile. Certains prennent en charge un contact tactile unique et d’autres l’entrée tactile multipoint (deux contacts ou plus).

La plupart des appareils qui prennent en charge l’entrée tactile multipoint reconnaissent généralement dix contacts uniques simultanés.

Les appareils Surface Hub reconnaissent 100 contacts tactiles uniques simultanés.

En règle générale, une interface tactile répond aux critères suivants :

-   Un seul utilisateur, sauf si elle est utilisée avec un appareil de l’équipe Microsoft comme Surface Hub, où la collaboration est mise en avant.
-   Pas de contrainte quant à l’orientation de l’appareil
-   Utilisée pour toutes les interactions, y compris les entrées de texte (clavier tactile) et les entrées manuscrites (application configurée).

### <a name="more-info"></a>Informations supplémentaires

[Recommandations en matière de conception pour l’interface tactile](https://msdn.microsoft.com/library/windows/apps/hh465370)
 

## <a name="touchpad"></a>Pavé tactile

Un pavé tactile combine l’entrée tactile multipoint indirecte et l’entrée de précision d’un dispositif de pointage comme la souris. Fort de cette combinaison, le pavé tactile est adapté à l’interface utilisateur optimisée pour l’interaction tactile et aux cibles d’applications de productivité plus petites.

### <a name="device-support"></a>Prise en charge des appareils

-   PC et portables
-   IoT

![pavé tactile](images/input-interactions/icons-touchpad01.png)

### <a name="typical-usage"></a>Utilisation type

Les pavés tactiles prennent généralement en charge un ensemble de mouvements tactiles similaires à la manipulation directe d’objets et de l’interface utilisateur.

Outre la prise en charge de l’entrée tactile, nous vous recommandons également de fournir des commandes d’interface utilisateur (ou un élément incitatif) accessibles à l’aide de la souris grâce à la convergence des expériences d’interaction prises en charge par les pavés tactiles. Fournissez des commandes d’interface utilisateur spécifiques au pavé tactile (ou un élément incitatif) pour prendre en charge ces interactions.

Il est recommandé de fournir des commandes d’interface utilisateur spécifiques à la souris (ou un élément incitatif) pour prendre en charge ces interactions. Par exemple, utilisez les boutons Précédent et Suivant (ou + et -) pour permettre aux utilisateurs de tourner les pages de contenu ou de faire pivoter, de redimensionner et d’agrandir les objets.

### <a name="more-info"></a>Informations supplémentaires

[Recommandations en matière de conception pour le pavé tactile](https://msdn.microsoft.com/library/windows/apps/dn456353)
 

## <a name="keyboard"></a>Clavier

Le clavier, principal périphérique d’entrée de texte, est indispensable pour les personnes souffrant de certains handicaps et les utilisateurs qui le considèrent simplement comme un mode d’interaction plus rapide et plus efficace avec une application.

Avec [Continuum pour téléphones](http://go.microsoft.com/fwlink/p/?LinkID=699431), une nouvelle expérience pour les appareils mobiles Windows 10 compatibles, les utilisateurs peuvent connecter leurs téléphones à une souris et un clavier pour leurs téléphones utiliser comme un ordinateur portable.

### <a name="device-support"></a>Prise en charge des appareils

-   Téléphones et phablettes
-   Tablette
-   PC et portables
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![clavier](images/input-interactions/icons-keyboard01.png)

### <a name="typical-usage"></a>Utilisation type

Les utilisateurs peuvent interagir avec les applications Windows universelles via un clavier matériel et deux claviers logiciels : le clavier visuel et le clavier tactile.

Le clavier visuel est un clavier logiciel visuel que vous pouvez utiliser à la place du clavier physique pour entrer des données à l’aide de la fonction tactile, de la souris, du stylo/stylet ou d’un autre dispositif de pointage (un écran tactile n’est pas nécessaire). Le clavier visuel est fourni pour les systèmes qui ne possèdent pas de clavier physique ou pour les utilisateurs qui connaissent des problèmes de mobilité les empêchant d’utiliser les périphériques d’entrée physiques classiques. Le clavier visuel émule la plupart, sinon la totalité, des fonctionnalités d’un clavier matériel.

Le clavier tactile est un clavier logiciel visuel permettant d’entrer du texte à l’aide de la fonction tactile. Le clavier tactile ne se substitue pas au clavier visuel car il n’est utilisé que pour la saisie de texte (il n’émule pas le clavier matériel) et apparaît seulement quand un champ textuel ou un autre contrôle textuel modifiable reçoit le focus. Le clavier tactile ne gère pas les commandes système ou de l’application.

**Remarque**le clavier visuel est prioritaire sur le clavier tactile, lequel n’est pas visible si le clavier visuel est présent.

En règle générale, un clavier répond aux critères suivants:

-   Utilisateur unique
-   Pas de contrainte quant à l’orientation de l’appareil
-   Utilisé pour l’entrée de texte, la navigation, le jeu et l’accessibilité
-   Toujours disponible, de façon proactive ou réactive

### <a name="more-info"></a>Informations supplémentaires

[Recommandations en matière de conception pour le clavier](https://msdn.microsoft.com/library/windows/apps/hh972345)
 

## <a name="mouse"></a>Souris

La souris est particulièrement adaptée aux applications de productivité et interfaces utilisateur haute densité, lorsque les interactions utilisateur nécessitent une précision au niveau des pixels pour le ciblage et la commande.

### <a name="device-support"></a>Prise en charge des appareils

-   Téléphones et phablettes
-   Tablette
-   PC et portables
-   Surface Hub
-   IoT

![souris](images/input-interactions/icons-mouse01.png)

### <a name="typical-usage"></a>Utilisation type

Les entrées de souris peuvent être modifiées avec l’ajout de plusieurs touches du clavier (Ctrl, Maj, Alt, etc.). Ces touches peuvent être combinées avec le bouton gauche de la souris, le bouton droit de la souris, la roulette et les boutons X pour un ensemble de commandes étendues optimisées pour la souris. (Certaines souris Microsoft possèdent deux boutons supplémentaires, nommés boutons X, qui permettent en général de naviguer vers l’arrière et vers l’avant dans les navigateurs Web.)

Comme pour le stylet, les entrées tactiles et de la souris divergent en raison de la capacité de l’entrée tactile à émuler la manipulation directe des éléments de l’interface utilisateur sur l’écran par le biais de mouvements physiques effectués sur ces objets (comme le balayage, le glissement, la rotation, etc.).

Il est recommandé de fournir des commandes d’interface utilisateur spécifiques à la souris (ou un élément incitatif) pour prendre en charge ces interactions. Par exemple, utilisez les boutons Précédent et Suivant (ou + et -) pour permettre aux utilisateurs de tourner les pages de contenu ou de faire pivoter, de redimensionner et d’agrandir les objets.

### <a name="more-info"></a>Informations supplémentaires

[Recommandations en matière de conception pour la souris](https://msdn.microsoft.com/library/windows/apps/dn456351)
 

## <a name="gesture"></a>Mouvement

Un mouvement désigne tous les gestes de l’utilisateur reconnus comme entrée pour contrôler ou interagir avec une application. Les mouvements prennent différentes formes : mouvements simples de la main pour cibler quelque chose à l’écran, mouvements appris dans un but spécifique ou longs mouvements continus effectués avec la totalité du corps. Soyez prudent lors de la conception des mouvements personnalisés, car leur signification peut varier en fonction des paramètres régionaux et de la culture.

### <a name="device-support"></a>Prise en charge des appareils

-   PC et portables
-   IoT
-   Xbox
-   HoloLens

![mouvement](images/input-interactions/icons-gesture01.png)

### <a name="typical-usage"></a>Utilisation type

Les événements d’action statique sont déclenchés une fois qu’une interaction se termine.

- Les événements d’action statique incluent Tapped, DoubleTapped, RightTapped et Holding.

Les événements d’action de manipulation indiquent une interaction en cours. L’utilisateur les déclenche en touchant un élément. Ils se poursuivent jusqu’à ce que l’utilisateur mette fin au contact ou que la manipulation soit annulée.

- Les événements de manipulation comprennent les interactions tactiles multipoints, telles que le zoom, le mouvement panoramique ou la rotation, et les interactions qui utilisent des données d’inertie et de vitesse, telles que le glissement. (Les informations fournies par les événements de manipulation ne reflètent pas l’interaction, mais communiquent des données telles que la position, le delta de translation et la vitesse.)

- Les événements de pointeur tels que PointerPressed et PointerMoved fournissent des détails de bas niveau pour chaque contact tactile, y compris le mouvement du pointeur et la capacité à distinguer les événements liés à l’appui ou au relâchement.

Outre la prise en charge de l’entrée tactile, nous vous recommandons également de fournir des commandes d’interface utilisateur (ou un élément incitatif) accessibles à l’aide de la souris grâce à la convergence des expériences d’interaction prises en charge par Windows. Par exemple, utilisez les boutons Précédent et Suivant (ou + et -) pour permettre aux utilisateurs de tourner les pages de contenu ou de faire pivoter, redimensionner et agrandir les objets.


## <a name="gamepadcontroller"></a>Boîtier de commande/contrôleur

Le boîtier de commande/contrôleur est un périphérique spécialisé généralement dédié aux jeux. Toutefois, il est également utilisé pour émuler les entrées de clavier de base et pour offrir une expérience de navigation de l’interface utilisateur très similaire au clavier.

### <a name="device-support"></a>Prise en charge des appareils

-   PC et portables
-   IoT
-   Xbox

![contrôleur](images/input-interactions/icons-controller01.png)

### <a name="typical-usage"></a>Utilisation type

Jouer à des jeux et interagir avec une console spécialisée.


## <a name="multiple-inputs"></a>Entrées multiples

En vous adaptant au plus grand nombre d’utilisateurs et d’appareils et en concevant vos applications de manière à ce qu’elles fonctionnent avec le plus grand nombre de types d’entrée (mouvement, commande vocale, entrée tactile, pavé tactile, souris et clavier), vous optimisez la flexibilité, la facilité d’utilisation et l’accessibilité.

### <a name="device-support"></a>Prise en charge des appareils

-   Téléphones et phablettes
-   Tablette
-   PC et portables
-   Surface Hub
-   IoT
-   Xbox
-   HoloLens

![entrées multiples](images/input-interactions/icons-inputdevices03-vertical.png)

### <a name="typical-usage"></a>Utilisation type

Tout comme les personnes ont recours à une combinaison de voix et de mouvement pour communiquer entre elles, plusieurs types et modes d’entrée peuvent également être utilisés lors de l’interaction avec une application. Toutefois, ces interactions combinées doivent être aussi intuitives et naturelles que possible pour ne pas perturber l’expérience de l’utilisateur.





 

 
