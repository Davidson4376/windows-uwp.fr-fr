---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: Présentation de la conception des applications de plateforme Windows universelle (UWP) (applications Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0527866777c7b5dbbc10697bb313d664f4555fa
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817277"
---
#  <a name="introduction-to-uwp-app-design"></a>Présentation de la conception des applicationsUWP

Le guide de conception pour la plateforme Windows universelle (UWP) est une ressource destinée à vous aider à concevoir des applications belles et élégantes.

Il ne s’agit pas d’une liste de règles normatives, mais plutôt d’un document dynamique, conçu pour s’adapter à l’évolution de notre [système FluentDesign](../fluent-design-system/index.md) ainsi qu’aux besoins de notre communauté de développement d’applications.

Cette nouvelle introduction présente une vue d’ensemble des fonctionnalités de conception universelle qui sont incluses dans chaque applicationUWP. Elle vous aide à créer des interfaces utilisateur (UI) qui s’adaptent parfaitement à toute une gamme d’appareils.

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="effective-pixels-and-scaling"></a>Pixels effectifs et mise à l’échelle

Les applicationsUWP ajustent automatiquement la taille des contrôles, des polices et des autres éléments d’interface utilisateur afin de se rendre accessibles sur [l’ensemble des appareils prenant en charge des applicationsUWP](../devices/index.md).

Lorsque votre application est exécutée sur un appareil, le système utilise un algorithme afin de normaliser l’affichage à l’écran des éléments d’interface utilisateur. Cet algorithme de mise à l’échelle prend en compte la distance d’affichage et la densité de l’écran (en pixels par pouce) pour optimiser la taille perçue (plutôt que la taille physique). L’algorithme de mise à l’échelle garantit qu’une police de 24pixels sur un appareil Surface Hub placé à une distance de 3 mètres est aussi lisible pour l’utilisateur qu’une police de 24pixels sur un téléphone doté d’un écran 5pouces distant de quelques centimètres.

![distances d’affichage des différents appareils](images/1910808-hig-uap-toolkit-03.png)

En raison du mode de fonctionnement du système de mise à l’échelle, lorsque vous concevez une applicationUWP, la conception est effectuée en pixels effectifs et non en pixels physiques réels. Les pixels effectifs (epx) sont une unité de mesure virtuelle qui sert à exprimer les dimensions et l’espacement de la disposition, indépendamment de la densité de l’écran. (Dans nos recommandations, epx ep et px sont utilisées indifféremment.)

Vous pouvez ignorer la densité de pixels et la résolution d’écran réelle lors de la conception. Effectuez plutôt une conception pour la résolution réelle (résolution en pixels effectifs) d’une classe de taille (pour plus d’informations, consultez l’[article Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Lors de la création de maquettes d’écran dans des programmes d’édition d’image, définissez les PPP sur72 et les dimensions d’image sur la résolution réelle pour la classe de taille que vous ciblez. Pour obtenir la liste des catégories de taille et des résolutions réelles, consultez l’[article Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).


## <a name="layout"></a>Disposition

### <a name="margins-spacing-and-positioning"></a>Marges, espacement et positionnement 
![grille](images/4epx.png)

Lorsque le système met à l’échelle l’interface utilisateur d’une application, il le fait par multiples de 4.

Par conséquent, les tailles, les marges et les positions des **éléments de l’interface utilisateur doivent toujours être des multiples de 4epx**. Cela permet d’obtenir le meilleur rendu possible grâce à un alignement avec des pixels entiers. Cette technique permet également de s’assurer que les éléments de l’interface utilisateur ont des bordures accentuées. 

Notez que le texte n’est pas assujetti à cette exigence; il peut en effet avoir n’importe quelle taille et position. Pour savoir comment aligner le texte avec d’autres éléments de l’interface utilisateur, voir le [Guide typographiqueUWP](../style/typography.md).

![mise à l’échelle sur la grille](images/epx-4pixelgood.png)

### <a name="layout-patterns"></a>Modèles de disposition

![Un modèle de disposition courante](images/page-components.png)

L’interface utilisateur est constituée de trois types d’éléments: 
1. Les **éléments de navigation** permettent aux utilisateurs de choisir le contenu à afficher. Voir [Notions de base sur la navigation](navigation-basics.md).
2. Les **éléments de commande** permettent d’initier des actions (de manipulation, d’enregistrement ou de partage de contenu, par exemple). Voir [Notions de base sur les commandes](commanding-basics.md).
3. Les **éléments de contenu** affichent le contenu de l’application. Voir [Notions de base sur le contenu](content-basics.md).

### <a name="adaptive-behavior"></a>Comportement adaptatif
![comportement adaptatif pour téléphone et ordinateur de bureau](../controls-and-patterns/images/patterns_masterdetail.png)

Bien que l’interface utilisateur de votre application soit lisible et utilisable sur tous les périphériques sous Windows, vous pouvez la personnaliser pour des périphériques et des tailles d’écran spécifiques. Pour des conseils plus spécifiques, voir [Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) et [Techniques de conception réactive](../layout/responsive-design.md).

## <a name="type"></a>Type

Par défaut, les applicationsUWP utilisent la police **SegoeUI**. La gamme de caractèresUWP comprend pour sa part septclasses de typographie pour une approche la plus efficace possible pour toutes les tailles d’affichage. 

![Gamme de caractères](images/type-ramp.png)

Pour plus d’informations sur la gamme de caractères, voir le [Guide typographiqueUWP](../style/typography.md). Pour apprendre à utiliser les différents niveaux de la gamme de caractèresUWP dans votre application, voir [Ressources de thème](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp).

## <a name="color"></a>Couleur

La couleur fournit un moyen intuitif de communiquer l’information aux utilisateurs. Elle peut être utilisée pour signaler l’interactivité, donner des commentaires sur les actions des utilisateurs, transmettre des informations d’état et donner à votre interface une impression de continuité visuelle. 

Windows10 fournit une palette universelle partagée de 48couleurs, qui est appliquée dans le shell et les applicationsUWP. 

![palette de couleurs windows universelle](images/colors.png)

Le système applique automatiquement des couleurs à votre applicationUWP avec la couleur d’accentuation système. Lorsque les utilisateurs choisissent une couleur d’accentuation dans la palette; dans leurs paramètres, la couleur s’affiche dans leur thème système. En fonction des préférences utilisateur, la couleur d’accentuation système peut également afficher sur l’écran de démarrage, de démarrage des vignettes, la barre des tâches et les barres de titre. 

![Sélectionnez une couleur d’accentuation dans les paramètres](images/selectcolor.png)

Au sein de votre applicationUWP, des liens hypertexte et certains états de contrôles courants reflèteront la couleur d’accentuation système.

![couleur d’accentuation système sur les contrôles](images/accentcolor.png)

Dans les applicationsUWP, il est possible d’empêcher la couleur d’accentuation système de s’afficher dans les contrôles à l’aide de [styles légers](../controls-and-patterns/xaml-styles.md#lightweight-styling) ou de [contrôles personnalisés](../controls-and-patterns/control-templates.md).

Pour plus d’informations sur l’utilisation de la couleur dans votre applicationUWP, voir l’article [Couleur](../style/color.md).

### <a name="themes"></a>Thèmes

Les utilisateurs peuvent choisir entre un thème à contraste élevé, clair ou sombre. Vous pouvez modifier l’apparence de votre application en fonction des préférences de thème de l’utilisateur ou ne pas intervenir.

Le thème clair convient mieux aux tâches de productivité et de lecture.

Un thème foncé rend plus visibles les contrastes dans les applications multimédias et dans les scénarios riches en vidéos et en images. Dans ces scénarios, la lecture n’est pas forcément prépondérante, contrairement au visionnage d’un film, dans un environnement peu éclairé.

![Calculatrice affichée dans les thèmes clairs et sombres](images/light-dark.png)

Le thème à contraste élevé utilise une petite palette de couleurs contrastées qui facilitent l’affichage de l’interface.
![Calculatrice affichée dans le thème clair et dans le thème noir à contraste élevé](../accessibility/images/high-contrast-calculators.png)


Pour plus d’informations sur l’utilisation des thèmes et de la gamme de caractèresUWP dans votre application, voir [Ressources de thème](../controls-and-patterns/xaml-theme-resources.md).

## <a name="icons"></a>Icônes
![icônes](images/icons.png)

Les icônes sont un type de langage visuel très répandu dans notre quotidien. Elles nous permettent d’exprimer des concepts et des actions d’une manière visuellement attrayante, d’économiser de l’espace à l’écran et de naviguer dans notre expérience numérique. 

Toutes les applicationsUWP ont accès aux icônes dans la [police SegoeMDL2](../style/segoe-ui-symbol-font.md). Ces icônes s’appuient sur les formulaires établis qui sont facilement identifiables et familiers pour tout le monde. Elles ont également été modernisées pour donner le sentiment qu’elles ont été dessinées d’une même main.

Si vous souhaitez créer vos propres icônes, voir [Icônes pour les applicationsUWP](../style/icons.md).

## <a name="tiles"></a>Vignettes
![vignettes dans le menu Démarrer](images/tiles.png)

Les vignettes sont affichées dans le menu Démarrer. Elles offrent un aperçu de ce qui se passe dans votre application. Elles s’appuient sur un solide contenu, et sur l’intelligence et le mode de conception qui sont à leur origine. 

Les applicationsUWP disposent de quatre tailles de vignettes (petite, moyenne, large et grande) qui peuvent être personnalisées avec l’icône et l’identité de l’application. Pour obtenir des conseils sur la conception de vignettes pour votre applicationUWP, voir [Recommandations en matière de ressources de vignette et d’icône](../shell/tiles-and-notifications/app-assets.md).

## <a name="controls"></a>Contrôles
![bouton de contrôle](images/1910808-hig-uap-toolkit-01.png)

La plateformeUWP fournit des contrôles courants dont le fonctionnement est garanti sur tous les appareils fonctionnant sous Windows. Ces contrôles incluent tous les éléments de contrôle, qu’ils soient simples comme les boutons et les éléments de texte, ou plus complexes et capables de générer des listes à partir d’un ensemble de données et d’un modèle.

Les contrôlesUWP reflètent automatiquement la couleur d’accentuation et le thème système, ils sont compatibles avec tous les types de saisie et s’adaptent à tous les appareils. Ils sont aussi hautement personnalisables. Vous pouvez modifier la couleur de premier plan d’un contrôle ou personnaliser complètement son apparence. 

Pour obtenir la liste complète des contrôlesUWP et des modèles que vous pouvez créer à partir de ceux-ci, consultez la [section consacrée aux contrôles et aux modèles](../controls-and-patterns/index.md).

## <a name="input"></a>Saisie
![saisies](../input/images/input-interactions/icons-inputdevices03.png)

Les applicationsUWP s’appuient sur des interactions intelligentes. Vous pouvez concevoir une fonction sur la base d’une interaction de clic sans savoir nécessairement si le clic provient d’un clic de la souris, d’un stylet ou d’une pression du doigt. Vous pouvez également concevoir vos applications pour des [modes de saisie et des appareils spécifiques](../input/input-primer.md).

## <a name="accessibility"></a>Accessibilité
![utilisateurs de la conception inclusive](images/inclusive.png)

Enfin, l’accessibilité consiste à rendre l’expérience de votre application ouverte à tous les utilisateurs. Elle s’adresse à tous, pas uniquement aux personnes souffrant de handicaps. Tout le monde peut bénéficier de l’expérience utilisateur véritablement inclusive. Pour voir comment rendre votre application facile à utiliser par tous, voir [Facilité d’utilisation pour les applicationsUWP](../usability/index.md). Vous souhaiterez peut-être également intégrer des [fonctionnalités d’accessibilité](../accessibility/accessibility-overview.md) pour les utilisateurs présentant des problèmes de vision, d’audition et de mobilité. 

Si vous intégrez l’accessibilité dès le départ dans votre conception, le fait de [rendre votre application accessible](../accessibility/accessibility-in-the-store.md) ne vous demandera que peu de temps et d’efforts supplémentaire.

## <a name="tools-and-design-toolkits"></a>Outils et kits de ressources de conception
Maintenant que vous connaissez les fonctionnalités de création de base, découvrez comment concevoir votre applicationUWP.

Nous proposons divers outils pour vous aider à réaliser votre conception:

* Consultez notre [page des kits de ressources de conception](../downloads/index.md) pour les kits de ressources XD, Illustrator, Photoshop, Framer et Sketch, ainsi que des outils de conception supplémentaires et des téléchargements de police. 

* Pour configurer votre ordinateur afin de pouvoir écrire du code pour les applicationsUWP, consultez notre article [Prise en main &gt; Préparation](../../get-started/get-set-up.md). 

* Pour apprendre à implémenter l’interface utilisateur pour UWP, jetez un coup œil à nos [applicationUWP](https://developer.microsoft.com/windows/samples) de bout en bout proposées en exemple.

## <a name="next-fluent-design-system"></a>Suivant: Système FluentDesign
Si vous souhaitez en savoir plus sur les principes sous-jacents de FluentDesign (système de conception de Microsoft) et voir plus de fonctionnalités pouvant être incorporées dans votre applicationUWP, accédez à [Système FluentDesign](../fluent-design-system/index.md).