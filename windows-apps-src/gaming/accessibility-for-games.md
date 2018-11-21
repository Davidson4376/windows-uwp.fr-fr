---
author: joannaleecy
title: Proposer des jeux accessibles
description: Découvrez comment assurer l’accessibilité de vos jeux. Appliquez le principe de conception de jeux inclusifs pour garantir l’accessibilité des jeux.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.author: joanlee
ms.date: 11/09/2017
ms.topic: article
keywords: Windows10, uwp, accessibilité, jeux
ms.localizationpriority: medium
ms.openlocfilehash: 79426a302be59af73536081cd13e14cad4facbe3
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7436072"
---
#  <a name="making-games-accessible"></a>Proposer des jeux accessibles

L’accessibilité contribue à accroître les capacités de l’ensemble des individus et des organisations de la planète, et cela se vérifie également dans le domaine des jeux vidéo. Cet article s’adresse aux développeurs, aux concepteurs et aux producteurs de jeux. Il offre une vue d’ensemble des recommandations en matière de conception de jeux accessibles émanant de diverses organisations (répertoriées dans la section de références à la fin de cet article) et présente le principe de conception de jeux inclusifs à appliquer pour améliorer l’accessibilité des jeux.

## <a name="gaming-for-everyone"></a>Jeux tous publics

Chez Microsoft, nous pensons que les jeux doivent permettre à tout le monde de s’amuser. «Nous nous efforçons de rendre les jeux agréables et accessibles à tous. Nous pensons vraiment que ce que nous créons pour nos fans et la manière dont nous le faisons (à l’intérieur et à l’extérieur de Microsoft) reflète ce que nous sommes. Nous avons conçu ce programme afin de refléter nos valeurs fondamentales et nous pensons que le programme pourrait aboutir à un changement positif (non seulement dans notre espace de travail, mais aussi dans les produits que nous développons pour nos joueurs)». ([Billet de Blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone) de Phil Spencer)

Nous voulons créer un environnement accueillant, amusant et varié accessible à tous. «Un virage culturel est indispensable pour qu’il y ait un impact durable et cela ne se fait pas du jour au lendemain. Toutefois, notre équipe s’engage à faire mieux chaque jour, à se concerter et à s’interrompre au cours du processus de prise de décision afin de réfléchir à l’incroyable diversité des besoins, aux capacités et aux centres d’intérêt des joueurs partout dans le monde.» ([Billet de Blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone) de Phil Spencer)

Venez participer à cette aventure afin de faire du concept de [Jeux tous publics](https://news.microsoft.com/gamingforeveryone) une réalité. 

##  <a name="why-make-games-accessible"></a>Pourquoi rendre les jeux accessibles?

### <a name="increased-gamer-base"></a>Élargissement de la communauté des joueurs

À la base, la justification économique de l’accessibilité repose sur une formule d’une grande simplicité:

Nombre d’utilisateurs pouvant jouer à votre jeu x niveau d’excellence du jeu = augmentation des ventes du jeu

Si vous créez un jeu spectaculaire, mais si complexe ou alambiqué que seule une poignée de joueurs peuvent en profiter, vous limiterez votre nombre de ventes. De même, si vous concevez un jeu inutilisable par des personnes présentant des troubles physiques, sensoriels ou cognitifs, vous raterez des opportunités de ventes. Si l’on considère par exemple que [19% de la population américaine souffre d’une forme quelconque de handicap ou d’invalidité](http://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), [que 14% environ des adultes américains éprouvent des difficultés à lire](https://nces.ed.gov/naal/estimates/overview.aspx), et [qu’environ 10% des hommes souffrent d’une forme de trouble de la vision des couleurs](https://www.aao.org/eye-health/diseases/color-blindness-risk), cet aspect peut avoir une incidence considérable sur les revenus générés par votre jeu. 

Pour découvrir d’autres justifications commerciales, voir [Proposer des jeux vidéo accessibles](https://msdn.microsoft.com/library/windows/desktop/ee415219).

### <a name="better-games"></a>Amélioration des jeux

La création d’un jeu plus accessible permet d’offrir un produit plus abouti au final. 

Les jeux qui proposent des sous-titres en constituent un bon exemple. Par le passé, les jeux prenaient rarement en charge le sous-titrage des dialogues. Aujourd’hui, il est d’usage de proposer des jeux incluant des sous-titres et des légendes. Cette évolution n’a pas été inspirée par des joueurs souffrant de handicaps ou d’invalidités. Elle s’est effectuée sous l’impulsion de la localisation, et s’est étendue à un large éventail de joueurs qui préfèrent jouer avec des sous-titres, tout simplement car cela leur offre une meilleure expérience de jeu. Les joueurs activent les sous-titres et les légendes lorsqu’ils jouent dans un environnement présentant un bruit de fond important, lorsqu’ils éprouvent des difficultés d’audition des voix accompagnées de divers effets sonores ou bruits ambiants, ou simplement lorsqu’ils doivent maintenir le volume à bas niveau pour ne pas déranger leur entourage. L’affichage de sous-titres et de légendes a non seulement contribué à améliorer l’expérience de jeu des utilisateurs, mais a également étendu cette expérience aux personnes souffrant de handicaps ou d’invalidités.

Pour les mêmes raisons, la reconfiguration des manettes est une autre fonctionnalité progressivement adoptée en standard par l’industrie des jeux électroniques. Elle est très souvent présentée comme un avantage pour tous les joueurs. Certains joueurs aiment pouvoir personnaliser leur expérience de jeu, d’autres préfèrent simplement quelque chose de différent de ce que les concepteurs avaient à l’esprit. La plupart des gens ignorent que la possibilité de reconfigurer les boutons d’un périphérique d’entrée constitue en réalité une fonctionnalité d’accessibilité initialement conçue pour les utilisateurs présentant différents handicaps psychomoteurs et pour qui il était physiquement difficile, voir impossible de faire fonctionner certaines zones de la manette.

Au final, la réflexion que vous aurez menée pour améliorer l’accessibilité de votre jeu se traduira généralement par l’obtention d’un jeu plus abouti, car vous offrirez à vos utilisateurs une expérience plus conviviale et personnalisable.

### <a name="social-space-and-quality-of-life"></a>Espace social et qualité de vie

Les jeux vidéo sont l’une des formes de divertissement les plus largement répandues et peuvent offrir du bonheur pendant des heures. Toutefois, pour certaines personnes, le jeu permet non seulement de se divertir, mais également d’échapper à un lit d’hôpital, à une douleur chronique ou à une anxiété sociale invalidante. Les joueurs sont transportés au sein d’un univers qui leur offre la possibilité de devenir les personnages principaux du jeu vidéo. Grâce aux jeux, ils peuvent donner vie et prendre part à un espace social qui leur est destiné et qui leur fait oublier agréablement leur combat quotidien contre le handicap dont ils souffrent tout en leur offrant l’opportunité de communiquer avec des personnes avec lesquelles ils n’auraient peut-être pas la possibilité d’échanger dans d’autres circonstances. 

Il s’agit également d’une culture. Le fait de pouvoir participer à la même activité que tous ses amis peut se révéler très valorisant pour une personne.

##  <a name="is-the-game-you-are-making-today-accessible"></a>Le jeu que vous proposez aujourd’hui est-il accessible?

Si vous envisagez pour la première fois de rendre votre jeu accessible, voici quelques questions que vous devez vous poser:

* Pouvez-vous exécuter le jeu avec une seule main? 
* La prise en main de votre jeu peut-elle être effectuée par une personne lambda?
* Pouvez-vous jouer facilement au jeu sur un moniteur ou un téléviseur de petite taille à une certaine distance?
* Prenez-vous en charge plusieurs types de périphériques d’entrée utilisables tout au long du jeu?
* Pouvez-vous jouer au jeu avec le son désactivé?
* Pouvez-vous jouer avec votre moniteur configuré en noir et blanc?
* Lorsque vous rechargez le dernier jeu enregistré au bout d’un mois, pouvez-vous savoir facilement où vous êtes arrivé et ce que vous devez faire afin de progresser?

Si vous répondez non à la plupart de ces questions ou que vous n’en connaissez pas les réponses, il est temps de passer à la vitesse supérieure et de garantir l’accessibilité de votre jeu.

## <a name="defining-disability"></a>Définition de la notion de handicap/invalidité

Le terme «handicap/invalidité» désigne une incompatibilité entre les besoins de l’individu et le service, le produit ou l’environnement proposés. ([Vidéo Inclusive](https://www.microsoft.com/design/inclusive), Microsoft.com.) Cela signifie que tout le monde peut souffrir d’un handicap ou d’une invalidité et qu’il peut s’agir d’une situation à court terme ou circonstancielle. Envisagez toutes les difficultés que les joueurs atteints de tels handicaps risquent de rencontrer en utilisant votre jeu, et réfléchissez à la façon dont vous pouvez améliorer la conception de votre jeu à leur intention. Voici quelques-uns des handicaps ou invalidités que vous devez prendre en compte:

### <a name="vision"></a>Vision

*   Affections médicales à long terme telles que le glaucome, la cataracte, le daltonisme, la myopie et la rétinopathie diabétique
*   Situations circonstancielles à court terme comme un moniteur ou un écran de petite taille, un écran basse résolution ou la présence de reflets sur l’écran en raison de l’exposition du moniteur ou de l’écran de portable au soleil
        
### <a name="hearing"></a>Audition

* Affections médicales à long terme telles qu’une surdité totale ou une perte d’audition partielle découlant de maladies ou de facteurs génétiques
* Situations circonstancielles à court terme comme un bruit de fond excessif, une qualité audio médiocre ou une baisse du volume pour ne pas gêner l’entourage
        
### <a name="motor"></a>Motricité

* Affections médicales à long terme telles que la maladie de Parkinson, la sclérose latérale amyotrophique (SLA), l’arthrite et la dystrophie musculaire
* Situations circonstancielles à court terme comme une blessure à la main ou le fait de tenir une boisson ou de porter un enfant dans ses bras
  
### <a name="cognitive"></a>Capacités cognitives

* Affections médicales à long terme telles que la dyslexie, l’épilepsie, le trouble déficitaire de l’attention avec hyperactivité (TDAH), la démence et l’amnésie
* Situations circonstancielles à court terme comme la consommation d’alcool, le manque de sommeil ou les distractions temporaires telles que la sirène d’une ambulance passant à proximité

### <a name="speech"></a>Parole

* Affections médicales à long terme telles qu’une lésion des cordes vocales, une dysarthrie ou une apraxie
* Situations circonstancielles à court terme comme des soins dentaires ou le fait de manger et de boire


## <a name="how-to-make-games-more-accessible"></a>Comment proposer des jeux plus accessibles?

### <a name="design-shift-inclusive-game-design-approach"></a>Évolution des pratiques de conception: adoptez une approche de conception de jeux inclusive

La conception inclusive est axée sur la création de produits et de services plus accessibles à un plus vaste éventail de clients, y compris aux personnes souffrant de handicaps ou d’invalidités.

Pour atteindre cet objectif, les concepteurs de jeux doivent désormais viser davantage que la simple création de jeux distrayants. Il leur faut tenir compte de l’impact de leur décisions de conception sur l’accessibilité globale du jeu, autrement dit sur la facilité de manipulation du jeu par l’ensemble des utilisateurs auxquels ils s’adressent, y compris ceux atteints de handicaps ou d’invalidités.

Les paradigmes de conception de jeux traditionnels doivent donc évoluer afin de prendre en compte le concept de conception de jeux inclusive. La conception de jeux inclusive implique de dépasser le processus élémentaire de développement de jeux uniquement axé sur le divertissement du public cible en créant des personnages supplémentaires ou modifiés afin d’englober un plus large éventail de joueurs. Vous devez être parfaitement au courant des difficultés dans votre jeu et vous assurer qu’elles ne constituent pas des obstacles inutiles qui enlèvent le plaisir de jouer.

En identifiant ces lacunes, vous pouvez optimiser, itérer sur le concept de conception d’origine et améliorer ce dernier, ce qui permet à d’autres personnes de partager votre vision. Lorsque vous prenez le temps de mettre en place un processus de conception de jeux plus inclusif, vous améliorez l’accessibilité de votre jeu au final. Aucun jeu ne peut convenir à tout le monde. La définition d’un jeu nécessite certains degrés de difficulté, mais en tenant compte de l’accessibilité vous avez la certitude de n’exclure personne inutilement.

### <a name="empower-gamers-give-gamers-options"></a>Accroissement des capacités des joueurs: élargissez les options des utilisateurs

Presque toutes les solutions d’accessibilité se résument à deux principes. Le premier principe consiste à offrir aux joueurs une multitude d’options pour personnaliser leur expérience de jeu. Si vos jeux connaissent déjà une immense popularité, il est possible qu’une part non négligeable de vos fans refuse toute modification de l’expérience de jeu. Cela ne pose aucun problème. Offrez à vos joueurs la possibilité d’activer ou de désactiver les fonctionnalités d’accessibilité et de configurer ces dernières individuellement. Vous devez permettre aux joueurs de profiter au mieux du jeu en fonction de leurs besoins et de leurs préférences.

### <a name="reinforce-communicate-information-in-more-than-one-way"></a>Renforcer: communiquez les informations de plusieurs façons

Le deuxième principe consiste à offrir une approche unique qui non seulement permet de rassembler plus de joueurs mais améliore également l’expérience dans son ensemble et c’est là qu’intervient le concept de conception universelle. Par exemple une image ainsi que du texte, un symbole ainsi que de la couleur. Une carte basée sur différents marqueurs colorés n’est pas seulement impossible à déchiffrer pour les joueurs daltoniens, elle constitue également une difficulté pour tout le monde car il faut savoir à quoi correspond chaque marqueur. L’ajout de symboles permet d’offrir une meilleure expérience à tous les utilisateurs.

### <a name="innovate-be-creative"></a>Innovation: faites preuve de créativité

Vous disposez de nombreux moyens pour améliorer l’accessibilité de votre jeu. Soyez inventif et inspirez-vous des autres jeux accessibles existants. Si vous avez déjà créé un jeu, efforcez-vous d’en identifier les fonctionnalités que vous pourriez améliorer tout en conservant les mécanismes clés du jeu et l’expérience de jeu initiale. Comme indiqué précédemment, l’accessibilité en matière de jeux repose entièrement sur l’offre de possibilités de personnalisation de l’expérience de jeu. Cela peut se faire en renforçant ou en communiquant les informations de plusieurs manières. 

Le fait de réfléchir à l’accessibilité vous permet d’aborder la conception sous un nouvel angle et éventuellement de trouver des idées auxquelles vous n’auriez pas pensé sinon. Cette approche de la conception aboutit non seulement à des concepts intéressants, mais permet aussi de créer des produits adoptés massivement ou qui rencontrent un grand succès commercial. Parmi les exemples on trouve la saisie de texte prédictive personnalisée, la reconnaissance vocale, les entrées charretières, le haut-parleur, la machine à écrire et la reconnaissance optique de caractères (OCR). Ces produits ont été au départ imaginés par des personnes qui réfléchissaient à des solutions pour l’accessibilité.

### <a name="adopt-quality-means-accessible-features"></a>Adopter: qualité signifie fonctionnalités accessibles

L’accessibilité est un critère de qualité. Elle devrait être obligatoire et non facultative. Par exemple, «L’adaptation d’une mini-carte pour les personnes daltoniennes» ne doit pas être considérée comme une tâche secondaire, de faible priorité que vous faites s’il vous reste du temps. Si cette tâche n’est pas effectuée, la fonctionnalité de minicarte est incomplète et ne peut pas être livrée.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Sensibilisation: mettez l’accent sur l’accessibilité dans votre studio de jeu

Le développement de jeux étant fréquemment soumis à des délais serrés, votre capacité à hisser l’accessibilité au rang de vos priorités contribuera à simplifier ce processus. Un bon moyen de procéder consiste à concevoir vos jeux à partir de zéro en gardant l’accessibilité à l’esprit. Plus l’accessibilité est prise en compte tôt et plus il est facile et économique de la mettre en place. 

Partagez vos connaissances sur l’accessibilité avec les membres de votre équipe, présentez-leur les justifications commerciales de cette approche, et dissipez les idées reçues, à savoir que l’accessibilité profite à peu de monde et qu’elle est compliquée et onéreuse à mettre en place.

### <a name="review-constantly-evaluate-your-game"></a>Vérification: évaluez continuellement votre jeu

Au cours du développement, vous pouvez introduire un processus de vérification destiné à vous assurer que chaque étape de la conception est axée sur l’accessibilité. Établissez une liste de contrôle semblable à celle ci-dessous pour aider votre équipe à déterminer continuellement si le jeu que vous créez est ou non accessible.

| Liste de contrôle                                         | Fonctionnalités d’accessibilité                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Séquences animées au sein du jeu                                | Présentation de sous-titres et de légendes, test de photosensibilité des animations                                                                           |
| Conception graphique globale (graphismes 2D et 3D)              | Couleurs et options compatibles avec le daltonisme, possibilités d’identification non entièrement basées sur les couleurs et tirant également parti des formes et des motifs|
| Écran d’accueil, menu de paramètres et autres menus       | Possibilité de lecture des options à voix haute, possibilité de mémorisation des paramètres, méthode d’entrée de commande alternative, possibilité de réglage de la taille des polices de l’interface utilisateur  |
| Expérience de jeu                                          | Vaste choix de niveaux de difficulté, affichage de sous-titres et de légendes, retour visuel et audio pertinent pour le joueur                           |
| Affichage tête haute                                       | Position d’écran réglable, taille de police ajustable, option destinée aux personnes daltoniennes                                                  |        
| Entrée de contrôle                                     | Possibilité de reconfigurer des contrôles sur le périphérique d’entrée, prise en charge de manettes personnalisées, autorisation d’entrées simplifiées pour le jeu                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Test du jeu et itération: recueillez les commentaires des joueurs

Lorsque vous organisez des sessions de test du jeu, invitez des personnes souffrant de handicaps ou d’invalidités à y participer afin de vérifier l’accessibilité. N’oubliez pas d’inclure des questions d’accessibilité dans les questionnaires de bêta-test. Les groupes de personnes handicapées sont une excellente source de participants. Observez leur façon de jouer avec votre jeu et recueillez leurs commentaires. Déterminez les changements à apporter afin d’améliorer votre jeu.

Utilisez les médias sociaux et le forum de votre jeu pour recueillir des commentaires et savoir quelles sont les fonctionnalités d’accessibilité les plus importantes et comment elles doivent être implémentées. 

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Promotion: signalez l’accessibilité de votre jeu au monde entier

Les utilisateurs ont besoin de savoir si votre jeu est manipulable par des personnes souffrant de handicaps ou d’invalidités. Signalez lisiblement l’accessibilité de votre jeu sur le site Web, les communiqués de presse et sur l’emballage du jeu pour vous assurer que les clients comprennent exactement les possibilités que leur offrira votre jeu. Pensez également à assurer l’accessibilité de votre site web et de tous les circuits de vente de votre jeu. Plus important encore, faites la promotion de votre jeu auprès de la communauté de joueurs concernés par l’accessibilité.

## <a name="game-accessibility-features"></a>Fonctionnalités d’accessibilité des jeux

Cette section décrit certaines des fonctionnalités vous permettant d’améliorer l’accessibilité de votre jeu. Ces fonctionnalités émanent des [recommandations en matière de conception de jeux accessibles](http://gameaccessibilityguidelines.com/) (en anglais), qui présentent les conclusions d’un groupe de concertation réunissant différents studios, spécialistes et universitaires. Pour plus d’informations, voir l’article [Game accessibility guidelines](http://gameaccessibilityguidelines.com/) (en anglais). 

### <a name="colorblind-friendly-graphics-and-user-interface"></a>Graphismes et interface utilisateur compatibles avec le daltonisme

La rétine comporte deux types de cellules photoréceptrices: les cônes qui permettent de percevoir la lumière, et les bâtonnets qui facilitent la vision dans des conditions de faible luminosité. 

Il existe trois types de cônes (rouges, verts et bleus) qui nous aident à visualiser correctement les couleurs. Le daltonisme survient lorsque l’un ou plusieurs de ces trois types de cônes ne fonctionnent pas comme prévu. Il existe différents degrés de daltonisme, depuis une perception des couleurs quasi-normale avec réduction de la sensibilité à la lumière rouge, verte ou bleue jusqu’à une totale incapacité à percevoir les couleurs. 

Du fait de la rareté des cas de sensibilité réduite à la lumière bleue, lorsque vous concevez un jeu pour des personnes daltoniennes, choisissez les couleurs en fonction des individus incapables de percevoir la couleur rouge ou verte:
 
  + Utilisez des combinaisons de couleurs pouvant être distinguées par les personnes daltoniennes incapables de percevoir le rouge et le vert:
  
    * couleurs apparaissant comme similaires: toutes les nuances de rouge et de vert, y compris le marron et l’orange;
    * couleurs qui ressortent: le bleu et le jaune.
    
  + Ne comptez pas uniquement sur les couleurs pour communiquer ou distinguer les objets du jeu. Utilisez également des formes et des modèles.
  + Si vous devez compter uniquement sur les couleurs, combinez des paramètres prédéfinis avec un libre choix de couleurs, afin que les joueurs qui en ont besoin puissent les personnaliser à leur guise et que cela n'engendre pas d'efforts supplémentaires pour les joueurs qui n’en ont pas besoin.
  + Utilisez un simulateur de daltonisme pour tester vos conceptions afin de pouvoir les visualiser à travers les yeux d’une personne daltonienne. Cela va vous aider à éviter les problèmes de contraste élevé courants. [Color Oracle](http://www.colororacle.org) est un simulateur de daltonisme gratuit qui permet de simuler les trois types de daltonisme les plus courants: la deutéranopie, la protanopie et la tritanopie.
  
### <a name="closed-captioning-and-subtitles"></a>Sous-titres et légendes

Lorsque vous concevez les sous-titres et les légendes de votre jeu, vous devez offrir la possibilité d’activer des sous-titres lisibles pour permettre aux utilisateurs de jouer à votre jeu sans le son. Vous devez faire en sorte que certains composants du jeu, tels que les dialogues et les animations et effets sonores, soient affichables sous forme de texte à l’écran.

Voici quelques recommandations de base à prendre en compte lors de la conception des sous-titres et des légendes:

*   Sélectionnez une police lisible simple.
*   Choisissez une police suffisamment grande ou envisagez de proposer une option de taille de police ajustable afin d’améliorer la flexibilité. (La taille de police idéale dépend de la taille de l’écran, de l’éloignement de l’écran, etc.)
*   Créez un contraste élevé entre la couleur d’arrière-plan et la couleur de police. Appliquez un contour et une ombre au texte. Utilisez un arrière-plan sombre pour les légendes et n’oubliez pas d’y associer des options permettant de l’activer ou de le désactiver. (Pour plus d’informations, voir [Coefficients de contraste](https://msdn.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Affichez des phrases courtes à l’écran (38caractères au maximum par ligne et 2 à 3lignes maximum à la fois). (Veillez à ne pas gâcher le suspense de votre jeu en affichant le texte avant que l’événement correspondant ne se produise.)
*   Indiquez clairement la provenance de l’effet sonore ou le nom du personnage en train de parler. (Par exemple: «Daniel: Bonjour!»)
*   Offrez la possibilité d’activer et de désactiver les sous-titres et les légendes. (Fonctionnalité supplémentaire: possibilité de sélection de la quantité d’informations sonores affichées en fonction de leur importance.)

### <a name="game-chat-transcription"></a>Transcription de tchat en cours de jeu

Si le jeu permet aux joueurs de communiquer verbalement et de s’envoyer des SMS, les fonctionnalités de reconnaissance vocale et de synthèse vocale doivent être disponibles en tant qu’option.

Les personnes qui n’ont pas de microphones connectés à leur périphérique de jeu peuvent toujours avoir une conversation avec une personne qui parle. Elles peuvent saisir du texte dans la fenêtre de conversation et le convertir en messages vocaux. Cela permet aussi aux personnes qui n’entendent pas très bien de lire les messages texte retranscrits de la personne avec qu’ils ont une conversation vocale.

Pour les partenaires et les développeurs gérés dans le programme ID@Xbox, les fonctionnalités de reconnaissance vocale et de synthèse vocale sont disponibles en tant que [fonctionnalités d’accessibilité Game Chat 2](../xbox-live/multiplayer/chat/using-game-chat-2.md#accessibility) dans le service Xbox Live. Pour plus d’informations, voir [Game Chat 2 Overview](../xbox-live/multiplayer/chat/game-chat-2-overview.md).

### <a name="sound-feedback"></a>Retour audio

Le son fournit un retour au joueur en complément du retour visuel. La conception d’une structure audio efficace peut améliorer l’accessibilité de votre jeu pour les joueurs souffrant d’une déficience visuelle. Voici quelques recommandations à prendre en compte:

*   Utilisez des signaux audio 3D pour fournir des informations spatiales complémentaires.
* Séparez les contrôles de volume de la musique, de la voix et des effets sonores.
*   Choisissez des formulations qui fournissent des informations pertinentes pour les joueurs. (Par exemple: «Les ennemis sont en train d’entrer par la porte de derrière» plutôt que «Les ennemis sont en approche».)
*   Assurez-vous que la vitesse d’élocution est correcte et proposez un contrôle permettant d’ajuster cette vitesse pour améliorer l’accessibilité.

### <a name="fully-mappable-controls"></a>Contrôles entièrement configurables

Certaines entreprises et organisations, telles que [Special Effect](http://www.specialeffect.org.uk/), conçoivent des manettes de jeu personnalisées qui sont utilisables avec différents systèmes de jeu, comme Windows et XboxOne. Cette personnalisation permet à des personnes atteintes de différentes formes de handicap ou d’invalidité de jouer à des jeux qu’ils n’auraient pas pu expérimenter sans cela. Pour découvrir des exemples d’utilisateurs qui sont désormais en mesure de jouer à des jeux de façon autonome grâce aux manettes personnalisées, consultez la page présentant [les personnes ayant bénéficié de l’aide de SpecialEffect](http://www.specialeffect.org.uk/who-we-helped).

En tant que développeur de jeux, vous pouvez améliorer l’accessibilité de votre jeu en autorisant les contrôles entièrement configurables afin d’offrir aux joueurs la possibilité de brancher leurs manettes personnalisées et de reconfigurer les touches selon leurs besoins.

Les contrôles entièrement configurables sont également bénéfiques pour les personnes qui utilisent des contrôleurs standard. Les joueurs peuvent concevoir une disposition adaptée à leurs besoins individuels.

Les manettes XboxOne standard et XboxElite sont personnalisables pour les jeux de précision. Pour profiter pleinement des fonctionnalités de remappage, __il est recommandé que les développeurs incluent le remappage directement dans le jeu__. Pour plus d’informations, voir [XboxOne](http://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) et [XboxElite](http://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Large éventail de niveaux de difficulté

Le rôle des jeux vidéo est de divertir les utilisateurs. La difficulté pour les développeurs consiste à ajuster le niveau de difficulté afin de proposer au joueur le défi le mieux adapté. Pour commencer, l’habilité et les capacités des joueurs diffèrent selon les individus. L’élargissement de l’éventail de niveaux de difficulté est donc plus susceptible de satisfaire aux attentes précises des joueurs en matière de défi à relever. Parallèlement, le fait d’offrir un choix de niveaux de difficulté plus étendu contribue également à optimiser l’accessibilité de votre jeu en permettant à un plus grand nombre de personnes handicapées d’utiliser votre jeu. N’oubliez pas que les joueurs sont motivés par la perspective de relever les différents défis d’un jeu et d’en être récompensés. Ils ne veulent pas d’un jeu impossible à gagner.

La mise au point du niveau de difficulté de votre jeu constitue un processus délicat. Si votre jeu est trop simple, les joueurs risquent de s’en lasser. S’il est trop difficile, il est possible que les utilisateurs finissent par baisser les bras et par abandonner votre jeu. Trouver le juste équilibre entre les deux relève à la fois de l’art et de la science. Il existe de nombreuses façons de concevoir un jeu offrant le niveau de difficulté approprié. Certains jeux proposent des entrées simplifiées, comme la possibilité de jouer par une simple pression sur un bouton, une option de rembobinage et de recommencement de la partie pour obtenir une nouvelle chance de gagner, ou une possibilité de diminuer le nombre ou la force des adversaires afin de faciliter la progression dans le jeu après plusieurs tentatives.

### <a name="photosensitivity-epilepsy-testing"></a>Test contre les risques d’épilepsie photosensible

L’épilepsie photosensible désigne le déclenchement de crises d’épilepsie par des stimuli visuels, notamment l’exposition à des lumières clignotantes ou à certains types de formes et de motifs visuels en mouvement. Ce type de trouble touche près de trois pour cent de la population et survient plus fréquemment chez les enfants et les adolescents. En termes de chiffres, cela touche environ [1 personne sur 4000âgées de 5 à 24ans](http://www.epilepsy.com/information/professionals/about-epilepsy-seizures/reflex-seizures-and-related-epileptic-syndromes-3).

De nombreux facteurs peuvent entraîner une réaction photosensible lors de l’utilisation d’un jeu vidéo, comme la durée de la partie, la fréquence des clignotements, l’intensité lumineuse, le contraste de l’arrière-plan et des motifs lumineux, la distance entre l’écran et le joueur, ainsi que la longueur d’onde de la lumière.

De nombreuses personnes découvrent qu’elles sont épileptiques lors de leur première crise. Les joueurs risquent de faire leur première crise à cause de jeux vidéo, et cela peut entraîner des blessures. Si vous développez des jeux, voici quelques conseils à suivre afin de réduire le risque de crises associées à l’épilepsie photosensible.

Évitez ce qui suit:
* Introduire des lumières clignotantes dont la fréquence est comprise entre 5 et 30clignotements par seconde (Hertz), car cette fréquence de clignotements est la plus susceptible de déclencher des crises.
* Des séquences d’images clignotantes qui durent plus de 5secondes.
* Plus de trois clignotements par seconde couvrant plus de 25% de l’écran.
* Déplacement de motifs répétés ou de texte uniforme, couvrant plus de 25% de l’écran.
* Motifs répétés statiques ou texte uniforme, couvrant plus de 40% de l’écran.
* Un important changement instantané dans la luminosité/le contraste (y compris des coupures rapides), ou vers ou depuis la couleur rouge.
* Plus de cinq rayures à contraste élevé, espacées de manière égale (des lignes ou des colonnes telles que des grilles et des damiers) et composées de petits éléments réguliers.
* Plus de cinq lignes de texte mises en forme en majuscules uniquement, avec peu d’espacement entre les lettres et un espacement de la même hauteur que les lignes elles-mêmes.

Utilisez un système automatisé pour rechercher dans votre jeu la présence éventuelle de stimuli risquant de provoquer une épilepsie photosensible. (Par exemple, utilisez les outils [Harding Test](http://www.hardingtest.com/index.php?page=test) et [Harding Flash and Pattern Analyzer (FPA) G2](http://www.hardingfpa.com/harding-fpa-for-games/) conçus par Cambridge Research System Ltd et le professeur GrahamHarding.) 

Incluez **Flashing On/Off** comme option de paramétrage et définissez **Flashing** sur **Off** par défaut. Vous protégez ainsi les joueurs qui ne savent pas encore qu’ils peuvent être sujets à des crises.

Introduisez des pauses entre les niveaux de jeu afin d’inciter les joueurs à s’arrêter de temps en temps au lieu de jouer pendant plusieurs heures d’affilée.

## <a name="other-accessibility-resources"></a>Autres ressources en matière d’accessibilité

Vous trouverez ci-après quelques sites externes fournissant des informations supplémentaires sur l’accessibilité des jeux.

### <a name="game-accessibility-guidelines"></a>Recommandations en matière de conception de jeux accessibles
* [Game accessibility guidelines (en anglais)](http://gameaccessibilityguidelines.com/)
* [Recommandations d’AbleGamers Foundation (en anglais)](http://www.includification.com/)
* [Concevoir des jeux universellement accessibles (en anglais)](http://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Manettes de jeu personnalisées
* [Special Effect (en anglais)](http://www.specialeffect.org.uk/)
* [Warfighter Engaged (en anglais)](http://www.warfighterengaged.org/)

## <a name="references-used"></a>Références utilisées
* [Game accessibility guidelines (en anglais)](http://gameaccessibilityguidelines.com/)
* [Recommandations d’AbleGamers Foundation (en anglais)](http://www.includification.com/)
* [Color Blind Awareness, entreprise d’intérêt communautaire (en anglais)](http://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [How to do subtitles well (en anglais) - Article de blog sur Gamasutra par IanHamilton](http://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Innovation for All Programme (en anglais)](http://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)
* [Epilepsy Foundation](http://www.epilepsy.com/)

## <a name="related-links"></a>Liens connexes
* [Conception inclusive](https://www.microsoft.com/design/inclusive)
* [Hub de développeurs axés sur l’accessibilité Microsoft](https://developer.microsoft.com/windows/accessible-apps)
* [Développement d’applications UWP accessibles](https://msdn.microsoft.com/windows/uwp/accessibility/accessibility)
* [Livre électronique sur la conception de logiciels accessibles](https://www.microsoft.com/download/details.aspx?id=19262)
