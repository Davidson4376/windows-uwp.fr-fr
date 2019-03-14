---
title: Création d'une application de plateforme Windows universelle (UWP) complexe
description: 'Les équipes de conception Microsoft suivent un processus de création d’applications en cinq étapes distinctes : concept, structure, dynamique, visuels et prototype. Nous vous encourageons à adopter un processus similaire en prenant plaisir à créer des expériences novatrices dont tout le monde pourra profiter.'
ms.assetid: 9A5189CD-3B97-4967-8E7D-36D25F04F244
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4f4c80ca1d031c69f5f70cdf85e90ea1d5887830
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605664"
---
#  <a name="building-a-complex-universal-windows-platform-uwp-app"></a>Création d'une application de plateforme Windows universelle (UWP) complexe

Les équipes de conception Microsoft suivent un processus de création d’applications en cinq étapes distinctes : concept, structure, dynamique, visuels et prototype. Nous vous encourageons à adopter un processus similaire en prenant plaisir à créer des expériences novatrices dont tout le monde pourra profiter.

## <a name="concept"></a>Concept

**Concentrer votre application**

Durant la planification de votre application de plateforme Windows universelle (UWP), vous devez déterminer sa finalité et sa cible, mais aussi ses points forts. Au cœur de chaque application réussie se trouve un concept puissant, qui offre une base solide.

Supposons que vous souhaitiez créer une application de photographie. En cherchant les raisons qui pousseraient quelqu’un à utiliser votre application, à l’enregistrer et à partager ses photos, vous arrivez à la conclusion que ses objectifs seraient de faire revivre des souvenirs, de rester en contact avec des proches grâce aux photos et de conserver celles-ci en lieu sûr. Voilà en quoi votre application doit exceller. Ces objectifs doivent vous guider tout au long du processus de conception.

**Quel est votre application ?** Commencez avec un concept élargi et faites l’inventaire des tâches que votre application peut aider à effectuer.

Par exemple, imaginons que vous souhaitiez créer une application qui aide les utilisateurs à planifier leurs voyages. Voici quelques idées que vous pourriez commencer par noter :

-   Trouver des cartes de tous les lieux d’un itinéraire et les emporter pour le voyage.
-   Rechercher tous les événements qui se produisent dans une ville donnée.
-   Pouvoir créer des listes individuelles, et partageables avec ses compagnons de voyage, des activités et des attractions incontournables.
-   Permettre à des compagnons de voyage de rassembler toutes leurs photos pour les partager avec leurs amis et leur famille.
-   Trouver des destinations conseillées en fonction du prix des vols.
-   Trouver une liste d’offres de restaurants, de boutiques et d’activités regroupées sur le lieu de voyage.

![Conception d’une application de voyage](images/ux-triptracker-tab-phone-700.png)

**Quel est votre application très utiles pour ?** Prenez du recul et analysez votre liste d’idées pour voir s’il en ressort un scénario particulier. Obligez-vous à réduire cette liste pour arriver à un seul scénario sur lequel vous concentrer. En cours de processus, vous serez peut-être amené à supprimer de nombreuses bonnes idées, mais, pour aboutir à un excellent scénario unique, vous devez être rigoureux.

Lorsque vous avez choisi un scénario unique, décidez comment vous expliqueriez les capacités de votre application à une personne lambda, par écrit, en une phrase. Exemple :

-   Mon application de voyage est excellente pour aider des amis à créer ensemble des itinéraires pour des voyages en groupe.
-   Mon application d’entraînement est excellente pour permettre à des amis de suivre leurs progrès et de partager leurs résultats.
-   Mon application de courses est excellente pour aider les familles à organiser leurs courses hebdomadaires et pour leur éviter d’oublier certains achats ou d’en faire en double.

![Conception d’un outil de collaboration](images/ux-collaboration-tabphone-700.png)

Cette liste de points forts peut orienter un grand nombre de décisions et d’arbitrage de conception à prendre durant la création de votre application. Concentrez-vous sur une liste de scénarios d’utilisation de votre application, et veillez à ne pas transformer cette liste en une énumération de fonctionnalités. Il s’agit de répertorier ce que vos utilisateurs pourront faire, pas votre application.

**L’entonnoir de conception**

Il est très tentant (après avoir eu une idée intéressante) de se lancer et de la développer, et peut-être même de l’amener à un stade avancé en matière de production. Mais admettons que les choses se passent ainsi, quand soudain une autre idée intéressante se présente. Il est naturel que vous soyez tenté de vous en tenir à l’idée dans laquelle vous vous êtes investi, indépendamment des mérites que peuvent avoir ces deux idées. Si seulement vous aviez pensé plus tôt à cette autre idée au cours du processus ! Justement, l’entonnoir de conception est une technique qui vous aide à découvrir vos meilleures idées le plus tôt possible.

Le terme « entonnoir » provient de la forme de cette technique. À l’extrémité large de l’entonnoir, de nombreuses idées sont injectées, chacune étant représentée par un artefact de conception peu précis (via un croquis, peut-être, ou un paragraphe de texte). Au fur et à mesure que cet ensemble d’idées circule jusqu’à l’extrémité étroite de l’entonnoir, le nombre d’idées baisse alors que la précision des artefacts qui représentent ces idées augmente. Chaque artefact doit cibler uniquement les informations nécessaires pour comparer les idées ou pour répondre à une question particulière telle que « est-ce utilisable ? » ou « est-ce intuitif ? ». *N’y consacrez pas plus de temps et d’efforts que nécessaire*. Vous abandonnerez certaines idées en les testant, ce qui est une bonne chose, car vous n’aurez pas à vous investir davantage dans ces dernières. Les idées qui continuent leur chemin dans l’entonnoir seront prises en considération de manière plus fine. Au final, il restera un seul artefact de conception qui représentera l’idée gagnante. Il s’agit de l’idée qui gagne en raison de ses mérites, et pas simplement parce qu’elle vous est venue en premier. Ainsi, vous concevrez la meilleure application possible.

## <a name="structure"></a>Structure


**Organisation transforme tout plus facile**

![L’organisation rend tout plus facile](images/ux-vision-and-process-organization.png)

Quand vous êtes satisfait de votre concept, vous êtes prêt pour la prochaine étape : la création du plan de votre application. L’architecture de l’information donne à votre contenu l’intégrité structurelle dont il a besoin. Il permet de définir le modèle de navigation de votre application et, en fin de compte, son identité. En planifiant l’organisation de votre contenu et la façon dont vos utilisateurs vont le découvrir, vous pouvez vous faire une meilleure idée du ressenti des utilisateurs face à votre application.

Une bonne architecture de l’information facilite les scénarios d’utilisation et vous aide à envisager les écrans principaux. L’application [Audible](https://go.microsoft.com/fwlink/p/?LinkID=268089), par exemple, se lance directement dans une page Hub, qui permet d’accéder à la bibliothèque, au magasin, aux informations et aux statistiques de l’utilisateur. L’expérience est ciblée, pour permettre aux utilisateurs d’obtenir rapidement des livres audio et d’en profiter. À des niveaux plus profonds, l’application se concentre sur des tâches plus spécifiques.

Pour obtenir des recommandations à ce sujet, voir [Informations de base relatives à la conception de la navigation](../design/basics/navigation-basics.md).

## <a name="dynamics"></a>Dynamics

**Exécuter votre concept**

Si l’étape de concept permet de définir la finalité de votre application, l’étape dynamique consiste à atteindre cette finalité. Il existe de nombreuses façons d’y parvenir, par exemple en utilisant des maquettes de fil de fer pour esquisser vos flux de page (comment aller d’un point à un autre dans l’application pour atteindre les objectifs), ainsi qu’en réfléchissant au ton et aux mots utilisés dans l’ensemble de l’interface utilisateur. La maquette de fil de fer est un outil de représentation rapide et globale, qui vous aide à prendre des décisions essentielles sur le flux d’utilisation de votre application.

Le flux de votre application doit être étroitement lié à votre liste de points forts et doit aider les utilisateurs à réaliser le scénario unique que vous souhaitez valoriser. Les applications réussies ont des flux faciles à assimiler qui nécessitent un effort minimal. Commencez à penser à un niveau écran à écran, et imaginez votre application comme si vous l’utilisiez pour la première fois. Quand vous définissez les situations d’utilisation des pages que vous créez, donnez aux utilisateurs exactement ce qu’ils veulent sans touches d’écran inutiles. La dynamique est également liée au mouvement. Les capacités de mouvement appropriées vont déterminer la fluidité et la facilité d’utilisation d’une page à l’autre.

Techniques courantes :

-   Décrit le flux : Ce qui se présente en premier, ce qui vient ensuite ?
-   Table de montage séquentiel le flux : Doivent déplacements des utilisateurs dans votre interface utilisateur pour terminer le flux ?
-   Prototype : Tester le flux avec un prototype rapide.

**Quelles doivent les utilisateurs pourront faire ?** Par exemple, l’application de voyage est « excellente pour aider des amis à créer ensemble des itinéraires pour des voyages en groupe ». Établissons la liste des flux à activer :

-   Créer un voyage en utilisant des informations générales.
-   Inviter des amis à se joindre à un voyage.
-   Participer à un voyage avec des amis.
-   Voir les itinéraires recommandés par d’autres voyageurs.
-   Ajouter des destinations et des activités au voyage.
-   Modifier et commenter les destinations et les activités ajoutées par les amis.
-   Permettre à des amis et à la famille de suivre les itinéraires partagés.

## <a name="visual"></a>Éléments visuels

**Parlez sans mots**

![Conception d’une application de création de cocktails](images/ux-cocktailcreator-tab-phone.png)

Une fois que vous avez créé la dynamique de votre application, vous pouvez mettre celle-ci en valeur en soignant son aspect visuel de façon appropriée. Des éléments visuels réussis définissent non seulement l’apparence de votre application, mais également la façon dont elle est perçue et la manière dont elle fonctionne via l’animation et le mouvement. Vos choix de palette de couleurs, d’icônes et d’illustrations ne sont que quelques exemples de ce langage visuel.

Chaque application possède sa propre identité, explorez donc les directions visuelles que vous pouvez prendre avec votre application. Laissez le contenu guider l’apparence de votre application, et non l’inverse.

## <a name="prototype"></a>Prototype

**Affiner votre chef-d'œuvre**

Le prototypage est une étape de l’*entonnoir de conception* (technique dont nous avons parlé un peu plus tôt), où l’artefact qui représente votre idée se développe au-delà de l’esquisse, mais à un niveau moins complexe que celui d’une application complète. Un prototype peut être un flux d’écrans dessinés à la main et présentés à un utilisateur. La personne qui effectue le test peut répondre aux indications de l’utilisateur en plaçant différents écrans vers le bas, ou en collant/décollant de petites parties de l’interface utilisateur sur les pages, pour simuler une application en cours d’exécution. Un prototype peut également être une application très simple qui simule certains flux de travail, à condition que l’opérateur respecte un script établi et appuie sur les boutons appropriés. À ce stade, votre application prend vie et tout votre dur travail est mis à l’épreuve. Durant le prototypage des domaines de votre application, prenez le temps de façonner et d’affiner les éléments qui en ont le plus besoin.

Pour les nouveaux développeurs, nous ne saurait trop insister suffisamment : Créer d’excellentes applications est un processus itératif. Nous vous recommandons de prototyper le plus tôt possible et souvent. Comme toute activité créatrice, les meilleures applications sont le fruit d’un processus intensif d’essais et d’erreurs.

## <a name="decide-what-features-to-include"></a>Identifier les fonctionnalités à inclure

Une fois que vous savez ce que veulent vos utilisateurs et comment les aider à y parvenir, vous pouvez étudier les outils spécifiques de votre boîte à outils. Passez la plateforme Windows universelle (UWP) en revue et associez des fonctionnalités pour répondre aux besoins de votre application. Veillez à suivre les [recommandations relatives à l’expérience utilisateur](https://developer.microsoft.com/windows/apps/design) pour chaque fonctionnalité.
<!--need URL for landing page -->

Techniques courantes :

-   Recherche de la plateforme : Découvrez les fonctionnalités que les offres de plate-forme et comment vous pouvez les utiliser.
-   Diagrammes d’association : Se connecter à vos flux avec des fonctionnalités.
-   Prototype : Testez les fonctionnalités pour vous assurer qu’elles le fassent ce dont vous avez besoin.

**Contrats d’application**  votre application peut participer à des contrats d’application qui permettent de flux large, entre applications, entre fonctionnalités utilisateur.

-   **Partager**  permettent aux utilisateurs de partager du contenu à partir de votre application avec d’autres personnes via d’autres applications et recevoir du contenu partageable d’autres personnes et les applications, trop.
-   **Play To**  permettent à vos utilisateurs profitent d’audio, vidéo ou images diffusées à partir de votre application avec d’autres appareils dans leur réseau domestique.
-   **Sélecteur de fichiers et extensions de sélecteur de fichier**   permettent de chargeront et enregistrement leurs fichiers à partir du système de fichiers local, périphériques de stockage connectés, groupe résidentiel ou même d’autres applications de vos utilisateurs. Vous pouvez également fournir une extension du sélecteur de fichiers pour permettre à d’autres applications de charger le contenu de votre application.

Pour plus d’informations, voir [Contrats et extensions d’application](https://msdn.microsoft.com/library/windows/apps/hh464906).
<!-- Win 8 page. Should have replacement. -->

**Différentes vues, les facteurs de forme et les configurations matérielles**  Windows met les utilisateurs dans votre application au premier plan et les frais. Vous voulez que l’interface utilisateur de votre application ressorte sur tous les appareils, à l’aide de n’importe quel mode d’entrée, avec toutes les orientations et configurations matérielles et dans toutes les circonstances d’utilisation.

**Tactile d’abord**  Windows offre une expérience tactile unique et distinctive n’émule pas simplement les fonctionnalités de la souris.

Par exemple, le zoom sémantique permet de naviguer dans un grand volume de contenu en mode tactile. Les utilisateurs peuvent survoler ou faire défiler les catégories de contenu, puis zoomer pour afficher des informations de façon de plus en plus détaillée. Vous pouvez ainsi présenter votre contenu de façon plus tactile, visuelle et instructive qu’avec la navigation traditionnelle et les modèles de disposition tels que les onglets.

Vous pouvez évidemment tirer parti d’un certain nombre d’interactions tactiles permettant de faire pivoter, d’effectuer un mouvement panoramique, un balayage, etc. En savoir plus sur les [interactions tactiles et les autres interactions avec l’utilisateur](../design/input/input-primer.md).

**Nouvelles et attrayantes**  Vérifiez que votre application semble fraîche et implique les utilisateurs avec ces expériences standards :

-   **Animations**  utiliser notre bibliothèque d’animations pour rendre votre application rapide et fluide pour vos utilisateurs. Permet aux utilisateurs de comprendre les changements de contexte et assure l’homogénéité des expériences par des transitions visuelles. Découvrez tout ce qu’il faut savoir sur l’[animation de votre interface utilisateur](../graphics/animations-overview.md).
-   **Notifications toast**  permettent à vos utilisateurs connaissent sur le contenu de contrainte de temps ou personnellement approprié via les notifications de toast et invitez-les à votre application même quand votre application est fermée. En savoir plus sur les [vignettes, badges et notifications toast](../design/shell/tiles-and-notifications/index.md).
-   **Vignettes d’applications**  fournir des mises à jour fraîches et pertinentes pour inciter les utilisateurs dans votre application. Pour plus d’informations à ce sujet, voir la section suivante. En savoir plus sur les [vignettes d’application](../design/shell/tiles-and-notifications/creating-tiles.md).

**Personnalisation**

-   **Paramètres**  permettent à vos utilisateurs à créer l’expérience, ils veulent en enregistrant les paramètres de l’application. Regroupe tous vos paramètres sur un seul écran, ce qui permet aux utilisateurs de configurer votre application via un mécanisme courant qu’ils maîtrisent déjà. En savoir plus sur l’[ajout de paramètres d’application](../design/app-settings/app-settings-and-data.md).
-   **Itinérance**  créer une expérience continue sur des appareils en itinérance des données qui permet aux utilisateurs de récupérer une tâche exactement où il s’était arrêté et conserve l’expérience utilisateur elles plus importantes, quel que soit l’appareil qu’ils utilisent. Votre application doit être facile à utiliser partout (du PC familial dans la cuisine au PC professionnel, en passant par la tablette personnelle et d’autres facteurs de forme), et conserver les paramètres et les états en itinérance. Pour en savoir plus, voir [Gestion des données d’application](../design/app-settings/store-and-retrieve-app-data.md) et [Recommandations en matière de données d’application itinérantes](https://msdn.microsoft.com/library/windows/apps/hh465094).
-   **Vignettes utilisateur**    rendre votre application plus personnelle à vos utilisateurs en chargeant leur mosaïque utilisateur, ou permettre aux utilisateurs de définir le contenu à partir de votre application en tant que sa vignette personnel tout au long de Windows.

**Fonctionnalités des appareils**  Vérifiez que votre application tire pleinement parti des fonctionnalités des appareils d’aujourd'hui.

-   **Les mouvements de proximité**  permettent aux utilisateurs de connecter des appareils avec des utilisateurs qui sont physiquement à proximité, par physiquement en appuyant sur les appareils ensemble (jeux multijoueurs). En savoir plus sur [les mouvements de proximité et le contact](https://msdn.microsoft.com/library/windows/apps/hh465229).
-   **Appareils photo et périphériques de stockage externe**  connecter vos utilisateurs à leurs appareils intégrés ou branché pour discuter et de conférence, l’enregistrement activée.%12%n%nPour, en prenant des pics de profil, documenter le monde autour d’elles, ou toute activité pour votre application est très utile au. Découvrez tout ce qu’il faut savoir sur l’[accès à du contenu de stockage amovible](https://msdn.microsoft.com/library/windows/apps/hh465189).
-   **Accéléromètres et autres capteurs**     appareils sont fournis avec un nombre de capteurs aujourd'hui. Votre application peut assombrir ou éclaircir l’affichage en fonction de la lumière ambiante, réorganiser l’interface utilisateur si l’utilisateur fait pivoter l’affichage ou réagir à un mouvement. En savoir plus sur les [capteurs](../devices-sensors/sensors.md).
-   **GÉOLOCALISATION**  utilisez les informations de géolocalisation à partir de données web standard ou à partir des capteurs de géolocalisation pour aider vos utilisateurs à contourner, trouver leur position sur une carte ou obtenir des notifications à proximité des personnes, des activités et des destinations. En savoir plus sur la [géolocalisation](https://msdn.microsoft.com/library/windows/apps/hh465139).

Reprenons l’exemple de l’application de voyage. Pour réellement aider des amis à créer ensemble des itinéraires pour les voyages en groupe, vous pouvez utiliser quelques fonctionnalités, comme les suivantes pour n’en citer que certaines :

-   Partager : Utilisateurs partagent des trajets à venir et leurs itinéraires à plusieurs réseaux sociaux pour partager l’enthousiasme préliminaire voyage avec leurs amis et les familles.
-   Rechercher : Les utilisateurs recherchent et trouver des activités ou des destinations de celles des autres partagé ou public itinéraires qu’ils peuvent inclure dans leurs propres des déplacements.
-   Notifications : Les utilisateurs sont avertis lorsque Compagnons de voyage mettez à jour leurs itinéraires.
-   Paramètres : Utilisateurs configurent l’application à leur convenance, tels que les courses doit mettre des notifications ou les groupes de réseaux sociaux sont autorisés à rechercher des itinéraires d’utilisateurs.
-   Zoom sémantique : Les utilisateurs naviguer dans la chronologie de leur feuille de route et faire un zoom pour afficher des détails supplémentaires de la longue liste d’activités qu’ils avez planifié.
-   Vignettes de l’utilisateur : Les utilisateurs choisir l’image que qui doivent s’afficher quand ils partagent leur voyage avec vos amis.

## <a name="decide-how-to-monetize-your-app"></a>Choisir le mode de monétisation de votre application

Il existe un grand nombre d’options vous permettant de gagner de l’argent avec votre application. Si vous décidez d’utiliser des publicités ou des ventes intégrées à l’application, vous devez concevoir une interface utilisateur adaptée. Pour plus d’informations, voir [Planifier la monétisation](../monetize/index.md).

## <a name="design-the-ux-for-your-app"></a>Concevoir l’expérience utilisateur de votre application

Il s’agit de partir sur la bonne base. Maintenant que vous connaissez les points forts de votre application et les flux à prendre en charge, vous pouvez réfléchir aux principes de base de la conception de l’expérience utilisateur.

**Comment doit organiser le contenu de l’interface utilisateur ?**   La plupart des contenus d’application peuvent être organisés en une forme de regroupements ou des hiérarchies. Ce que vous choisissez comme groupe de premier niveau pour votre contenu doit correspondre à votre liste de points forts.

Pour reprendre l’exemple de l’application de voyage, il existe de nombreuses façons de regrouper des itinéraires. Si l’objet principal de l’application est de découvrir des destinations intéressantes, vous pouvez les regrouper par centre d’intérêt : aventure, plaisirs ensoleillés ou destinations romantiques. Si votre application sert cependant à planifier des voyages avec des amis, il est plus sensé d’organiser des itinéraires en fonction des cercles sociaux, comme la famille, les amis ou les collègues.

Choisir le mode de regroupement de votre contenu vous permet de décider des pages ou des vues nécessaires dans votre application. Reportez-vous aux notions de base sur l’interface utilisateur pour plus d’informations.

**Comment doit présenter le contenu de l’interface utilisateur?** Une fois que vous avez décidé de l’organisation de votre interface utilisateur, vous pouvez définir les objectifs de l’expérience utilisateur qui spécifient la création et la présentation de cette interface à vos utilisateurs. Quel que soit le scénario, assurez-vous que vos utilisateurs peuvent continuer à profiter de votre application aussi rapidement que possible. Pour ce faire, déterminez les parties de votre interface utilisateur qui doivent être présentées en premier, puis vérifiez qu’elles sont complètes avant de passer du temps à générer les parties non essentielles.

Dans l’application de voyage, l’utilisateur souhaitera probablement trouver en premier un itinéraire de voyage. Pour présenter ces informations le plus rapidement possible, affichez la liste des voyages en premier à l’aide d’un contrôle **ListView**.

![Conception du sélecteur d’itinéraires d’une application de voyage](images/ux-app-travel-cc-a-1-180.png)

Une fois la liste des voyages affichée, vous pouvez charger d’autres fonctionnalités, par exemple un flux d’actualités relatif aux voyages des amis.

**Les surfaces d’interface utilisateur et les commandes avez-vous besoin?**   Passez en revue les flux que vous avez identifiée précédemment. Pour chaque flux, créez une ébauche des étapes entreprises par les utilisateurs.

Analysons le flux « Permettre à des amis et à la famille de suivre les itinéraires ». Supposons que l’utilisateur a déjà créé un voyage. Pour partager l’itinéraire d’un voyage, les étapes suivantes sont nécessaires :

1.  L’utilisateur ouvre l’application et voit la liste des voyages qu’il a créés.
2.  L’utilisateur appuie sur le voyage à partager.
3.  Les détails du voyage s’affichent à l’écran.
4.  L’utilisateur accède à certaines zones de l’interface utilisateur pour lancer le partage.
5.  L’utilisateur sélectionne ou entre l’adresse e-mail ou le nom de l’ami avec lequel il veut partager le voyage.
6.  L’utilisateur accède à certaines zones de l’interface utilisateur pour finaliser le partage.
7.  Votre application met à jour les informations du voyage avec la liste des personnes avec qui il a partagé son voyage.

Au cours de ce processus, vous entrevoyez le type d’interface utilisateur que vous devez créer et les informations supplémentaires à trouver (comme rédiger un modèle d’e-mail standard pour les amis qui n’utilisent pas encore votre application). Vous pouvez également commencer à supprimer certaines étapes inutiles. L’utilisateur n’a peut-être pas besoin de voir tous les détails du voyage avant de le partager, par exemple. Plus le flux est simple, plus il est facile à utiliser.

Pour en savoir plus sur l’utilisation des différentes surfaces, voir <!--[Command design basics](../design/basics/commanding-basics.md)-->.

**Ce qui doit le flux de l’impression ?** Une fois les étapes de l’utilisateur définies, vous pouvez transformer ce flux en objectifs de performances. Pour plus d’informations, voir [Planifier les performances](../debug-test-perf/planning-and-measuring-performance.md).

**Comment organiser les commandes ?**   Votre présentation des étapes de flux permet d’identifier des commandes potentielles dont vous avez besoin pour concevoir de. Cherchez ensuite où ces commandes doivent être utilisées dans votre application.

-   **Toujours essayer d’utiliser le contenu.**   Autant que possible, permettre aux utilisateurs manipuler directement le contenu sur le canevas de l’application, plutôt qu’Ajout de commandes qui agissent sur le contenu. Par exemple, dans l’application de voyage, permettez aux utilisateurs de réorganiser leur itinéraire en faisant glisser et en déplaçant des activités d’une liste de la zone de dessin, plutôt qu’en sélectionnant l’activité et en utilisant les boutons haut et bas de la barre de commandes.
-   **Si vous ne pouvez pas utiliser le contenu.** Placez les commandes sur l’une des surfaces suivantes de l’interface utilisateur :

    -   Dans le [barre de commandes](https://msdn.microsoft.com/library/windows/apps/hh465302): Vous devez placer la plupart des commandes sur la barre de commandes, qui est généralement masquée jusqu'à ce que l’utilisateur appuie pour le rendre visible.
    -   Sur le canevas de l’application : Si l’utilisateur se trouve sur une page ou une vue qui a un seul objectif, vous pouvez fournir des commandes à cet effet directement sur le canevas. Il doit y avoir très peu de commandes de ce type.
    -   Dans un [menu contextuel](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/menus): Vous pouvez utiliser les menus contextuels pour les actions de Presse-papiers (par exemple, couper, copier et coller), ou pour les commandes qui s’appliquent pour le contenu qui ne peut pas être sélectionné (comme l’ajout d’une punaise à un emplacement sur une carte).

**Décider comment organiser votre application dans chaque vue.**    Windows prend en charge les orientations portrait et paysage et prend en charge le redimensionnement des applications à toute la largeur, de plein écran sur une largeur minimale. Vous devez faire en sorte que votre application fonctionne correctement et soit visuellement plaisante dans n’importe quelle taille, sur n’importe quel écran et dans n’importe quelle orientation. Cela signifie que vous devez planifier la disposition des éléments de votre interface utilisateur pour différentes tailles et affichages. Ainsi, l’interface utilisateur de l’application s’adapte de façon fluide pour répondre aux besoins et aux préférences de l’utilisateur.

![Conception d’une application pour PC ou appareils mobiles](images/ux-budgettracker1-md-notablet.png)

Pour plus d’informations sur la conception pour différentes tailles d’écran, voir [Tailles d’écran et points d’arrêt pour la conception réactive](https://docs.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

## <a name="make-a-good-first-impression"></a>Faire une bonne première impression

Gardez à l’esprit ce que vous voulez que vos utilisateurs pensent, ressentent ou fassent au moment où ils lancent votre application pour la première fois. Reportez-vous à votre liste de points forts. Même si vous n’aurez pas l’occasion de dire personnellement à vos utilisateurs quels sont les points forts de votre application, vous pouvez leur faire passer le message en donnant une bonne première impression. Utilisez les possibilités suivantes :

**Vignettes et les notifications**    la vignette est la face de votre application. Pour quelles raisons les utilisateurs choisiront-ils de lancer votre application, présentée parmi d’autres sur leur écran de démarrage ? Votre vignette doit mettre en valeur la marque et les capacités de votre application. Utilisez les notifications par vignette pour garder votre application à jour et d’actualité, et pour ramener constamment l’utilisateur à votre application.

**Écran de démarrage**  l’écran de démarrage doit charger aussi rapidement que possible et restent sur l’écran, que vous devez initialiser l’état de votre application. Ce que vous affichez sur cet écran de démarrage doit exprimer la personnalité de votre application.

**Tout d’abord lancer**  avant les utilisateurs s’inscrire à votre service, de se connecter à leur compte ou ajouter leur propre contenu, ce qu’ils verra ? Essayez de montrer la valeur de votre application avant de demander des informations aux utilisateurs. Pensez à donner des exemples de contenu pour permettre aux utilisateurs de les manipuler et de comprendre l’objet de votre application avant de leur demander de s’engager.

**Page d’accueil**  la page d’accueil est la solution idéale incluait les utilisateurs chaque fois qu’il lance votre application. Son contenu doit être évident et doit indiquer immédiatement ce pour votre application est conçue. Faites une excellente page et comptez sur les gens pour explorer le reste de votre application. Éliminez à tout prix les distractions sur la page d’accueil au lieu de miser sur la découverte.

## <a name="validate-your-design"></a>Valider votre conception

Pour éviter d’avoir à effectuer trop de changements plus tard, validez votre conception ou votre prototype par rapport aux recommandations, impressions des utilisateurs et exigences avant de pousser le développement trop loin. Chaque fonctionnalité possède un ensemble d’instructions de l’expérience utilisateur pour vous aider à peaufiner votre application et un ensemble d’exigences de Store qui doivent être réunies pour publier votre application dans le Microsoft Store. Vous pouvez utiliser le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) pour tester la conformité technique aux exigences du Windows Store. Vous pouvez également utiliser les outils d’analyse des performances de Microsoft Visual Studio pour proposer à vos utilisateurs la meilleure expérience possible, quel que soit le scénario.

Utilisez la liste de [recommandations détaillées relatives à l’expérience utilisateur pour les applications UWP](https://msdn.microsoft.com/library/windows/apps/hh465424) afin de ne pas oublier de fonctionnalités importantes. Utilisez les [outils de performances Visual Studio](https://msdn.microsoft.com/library/windows/apps/hh696636.aspx) pour analyser les performances de chacun des scénarios de votre application.
