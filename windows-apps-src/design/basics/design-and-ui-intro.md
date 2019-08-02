---
Description: Les fonctionnalités de conception universelle incluses dans chaque application UWP vous aident à créer des applications qui s’adaptent parfaitement à toute une gamme d’appareils.
title: $$$Présentation de la conception des applications Windows pour la plateforme Windows universelle (UWP)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 05/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2b0f5918b240bf5c28e49f2ede6f10dbeefcbbfc
ms.sourcegitcommit: e13f06042a28a8455a211b8693a009098e150cd1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68522088"
---
# <a name="introduction-to-uwp-app-design"></a>Présentation de la conception des applications UWP

![exemple d’application d'éclairage](images/introUWP-header.jpg)

Le guide de conception pour la plateforme Windows universelle (UWP) est une ressource destinée à vous aider à concevoir et générer de belles applications abouties.

Il ne s’agit pas d’une liste de règles normatives, mais plutôt d’un document dynamique, conçu pour s’adapter à l’évolution de notre [système Fluent Design](/windows/apps/fluent-design-system) ainsi qu’aux besoins de notre communauté de développement d’applications.

Cette nouvelle introduction présente une vue d’ensemble des fonctionnalités de conception universelle qui sont incluses dans chaque application UWP. Elle vous aide à créer des interfaces utilisateur (UI) qui s’adaptent parfaitement à toute une gamme d’appareils.

## <a name="effective-pixels-and-scaling"></a>Pixels effectifs et mise à l’échelle

Les applications UWP s’exécutent sur tous les [appareils Windows 10](../devices/index.md), depuis votre téléviseur, jusqu'à votre tablette ou votre PC. Alors comment concevoir une interface utilisateur qui s’affiche parfaitement sur un large éventail d’appareils et de tailles d’écran ?

![même application sur différents appareils](images/universal-image-1.jpg)

UWP vous aide en ajustant automatiquement les éléments d’interface utilisateur afin de les rendre accessibles sur l’ensemble des appareils et des tailles d'écran et de pouvoir facilement interagir avec eux.

Lorsque votre application est exécutée sur un appareil, le système utilise un algorithme afin de normaliser l’affichage à l’écran des éléments d’interface utilisateur. Cet algorithme de mise à l’échelle prend en compte la distance d’affichage et la densité de l’écran (en pixels par pouce) pour optimiser la taille perçue (plutôt que la taille physique). L’algorithme de mise à l’échelle garantit qu’une police de 24 pixels sur un appareil Surface Hub placé à une distance de 3 mètres est aussi lisible pour l’utilisateur qu’une police de 24 pixels sur un téléphone doté d’un écran 5 pouces distant de quelques centimètres.

![distances d’affichage des différents appareils](images/scaling-chart.png)

En raison du mode de fonctionnement du système de mise à l’échelle, lorsque vous concevez une application UWP, la conception est effectuée en pixels effectifs et non en pixels physiques réels. Les pixels effectifs (epx) sont une unité de mesure virtuelle qui sert à exprimer les dimensions et l’espacement de la disposition, indépendamment de la densité de l’écran. (Dans nos recommandations, epx ep et px sont utilisées indifféremment.)

Vous pouvez ignorer la densité de pixels et la résolution d’écran réelle lors de la conception. Effectuez plutôt une conception pour la résolution réelle (résolution en pixels effectifs) d’une classe de taille (pour plus d’informations, consultez l’[article Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Lors de la création de maquettes d’écran dans des programmes d’édition d’image, définissez les PPP sur 72 et les dimensions d’image sur la résolution réelle pour la classe de taille que vous ciblez. Pour obtenir la liste des catégories de taille et des résolutions réelles, consultez l’[article Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

### <a name="multiples-of-four"></a>Multiples de quatre

:::row:::
    :::column span:::
Les tailles, les marges et les positions des éléments de l’interface utilisateur doivent toujours être des **multiples de 4 epx** dans vos applications UWP.

UWP prend en charge différents appareils avec plateaux d'échelle de 100 %, 125 %, 150 %, 175 %, 200 %, 225 %, 250 %, 300 %, 350 % et 400 %. L’unité de base est 4, car il s’agit du seul entier pouvant être mis à l’échelle par des nombres non entiers (par exemple, 4*1,5 = 6). L’utilisation de multiples de quatre aligne tous les éléments d’interface utilisateur à pixels entiers et leur garantit des bords nets et vifs. (Notez que le texte n’est pas assujetti à cette exigence ; il peut en effet avoir n’importe quelle taille et position.)
    :::column-end:::
    :::column:::
![Grille](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>Disposition

Puisque les applications UWP s'adaptent automatiquement à tous les appareils, la conception d’une application UWP présentera la même structure, quel que soit l'appareil. Commençons par le début de l’interface utilisateur de votre application UWP.

### <a name="windows-frames-and-pages"></a>Fenêtres, cadres et pages

:::row:::
    :::column:::
Lorsqu’une application UWP est lancée sur n’importe quel appareil Windows 10, elle se lance dans une [Fenêtre](/uwp/api/windows.ui.xaml.window) avec un [Cadre](/uwp/api/windows.ui.xaml.controls.frame), qui peut naviguer entre des instances de [Page](/uwp/api/windows.ui.xaml.controls.page).
    :::column-end:::
    :::column:::
![Trame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
Vous pouvez considérer l’interface utilisateur de votre application comme une collection de pages. C’est à vous de choisir les éléments qui vont sur chaque page et les relations entre les pages.

Pour découvrir comment organiser vos pages, consultez [Notions de base sur la navigation](navigation-basics.md).
    :::column-end:::
    :::column:::
![Trame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>Mise en page

Quel aspect doivent avoir ces pages ? La plupart des pages suivent une structure commune pour garantir la cohérence, afin que les utilisateurs puissent facilement naviguer entre les pages et dans chaque page de votre application. Les pages contiennent généralement trois types d’éléments d’interface utilisateur :

- Les éléments de [navigation](navigation-basics.md) permettent aux utilisateurs de choisir le contenu à afficher.
- Les éléments de [commande](commanding-basics.md) permettent d’initier des actions (de manipulation, d’enregistrement ou de partage de contenu, par exemple).
- Les éléments de [contenu](content-basics.md) affichent le contenu de l’application.

![Un modèle de disposition courante](../layout/images/page-components.svg)

Pour en savoir plus sur l’implémentation de modèles d’application UWP courants, consultez l'article [Mise en Page](../layout/page-layout.md).

Vous pouvez également utiliser [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) dans Visual Studio pour commencer une disposition pour votre application.

## <a name="controls"></a>Contrôles

La plateforme de conception UWP fournit un ensemble de contrôles courants qui sont garantis pour fonctionner correctement sur tous les appareils fonctionnant sous Windows, et qui respectent nos principes de [Système Fluent Design](/windows/apps/fluent-design-system). Ces contrôles incluent tous les éléments de contrôle, qu’ils soient simples comme les boutons et les éléments de texte, ou plus complexes et capables de générer des listes à partir d’un ensemble de données et d’un modèle.

![Contrôles UWP](../style/images/color/windows-controls.svg)

Pour obtenir la liste complète des contrôles UWP et des modèles que vous pouvez créer à partir de ceux-ci, consultez la [section consacrée aux contrôles et aux modèles](../controls-and-patterns/index.md).

## <a name="style"></a>Style

Les contrôles courants reflètent automatiquement la couleur d’accentuation et le thème système, ils sont compatibles avec tous les types de saisie et s’adaptent à tous les appareils. Ainsi, ils sont représentatifs du Système Fluent Design : ils sont adaptatifs, empathiques et esthétiques. Les contrôles courants utilisent la lumière, le mouvement et la profondeur dans leur style par défaut, donc en les utilisant, vous incorporez notre Système Fluent Design dans votre application.

Les contrôles courants sont hautement personnalisables. Vous pouvez modifier la couleur de premier plan d’un contrôle ou personnaliser complètement son apparence. Pour remplacer les styles par défaut dans les contrôles, utilisez [styles légers](../controls-and-patterns/xaml-styles.md#lightweight-styling) ou créez des [contrôles personnalisés](../controls-and-patterns/control-templates.md) en XAML.

![Couleur d’accentuation gif](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
Votre application UWP interagira avec l’expérience Windows plus large des vignettes et des notifications dans le Windows [Shell](../shell/tiles-and-notifications/creating-tiles.md).

Les vignettes s'affichent dans le menu Démarrer et au lancement de votre application. Elles offrent un aperçu de ce qui se passe dans votre application. Elles s’appuient sur un solide contenu, et sur l’intelligence et le mode de conception qui sont à leur origine.

Les applications UWP disposent de quatre tailles de vignettes (petite, moyenne, large et grande) qui peuvent être personnalisées avec l’icône et l’identité de l’application. Pour obtenir des conseils sur la conception de vignettes pour votre application UWP, consultez [Recommandations en matière de ressources de vignette et d’icône](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
![vignettes du menu Démarrer](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>Entrées

:::row:::
    :::column:::
Les applications UWP s’appuient sur des interactions intelligentes. Vous pouvez concevoir une fonction sur la base d’une interaction de clic sans savoir nécessairement si le clic provient d’un clic de la souris, d’un stylet ou d’une pression du doigt. Vous pouvez également concevoir vos applications pour des [modes de saisie spécifiques](../input/input-primer.md).
    :::column-end:::
    :::column:::
![entrées](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>Appareils

![appareils](../layout/images/size-classes.svg)

De même, bien qu'UWP adapte automatiquement votre application à différents appareils, vous pouvez également [optimiser votre application UWP pour des appareils spécifiques](../devices/index.md).

## <a name="usability"></a>Facilité d'utilisation

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

Enfin, la facilité d'utilisation consiste à rendre l’expérience de votre application ouverte à tous les utilisateurs. Tout le monde peut bénéficier de l’expérience utilisateur véritablement inclusive. Pour voir comment rendre votre application facile à utiliser par tous, voir [Facilité d’utilisation pour les applications UWP](../usability/index.md).

Si vous concevez pour un public international, vous souhaiterez peut-être consulter la page [Globalisation et localisation](../globalizing/globalizing-portal.md).

Vous souhaiterez peut-être également intégrer des [fonctionnalités d’accessibilité](../accessibility/accessibility-overview.md) pour les utilisateurs présentant des problèmes de vision, d’audition et de mobilité. Si vous intégrez l’accessibilité dès le départ dans votre conception, le fait de [rendre votre application accessible](../accessibility/accessibility-in-the-store.md) ne vous demandera que peu de temps et d’efforts supplémentaires.

## <a name="tools-and-design-toolkits"></a>Outils et kits de ressources de conception

Maintenant que vous connaissez les fonctionnalités de création de base, découvrez comment concevoir votre application UWP.

Nous proposons divers outils pour vous aider à réaliser votre conception :

- Consultez notre [page des kits de ressources de conception](../downloads/index.md) pour les kits de ressources XD, Illustrator, Photoshop, Framer et Sketch, ainsi que des outils de conception supplémentaires et des téléchargements de police.

- Pour configurer votre ordinateur afin de pouvoir écrire du code pour les applications UWP, consultez notre article [Prise en main &gt; Préparation](../../get-started/get-set-up.md).

- Pour apprendre à implémenter l’interface utilisateur pour UWP, jetez un coup œil à nos [exemples d’applications UWP](https://developer.microsoft.com/windows/samples) de bout en bout.

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>Suivant : Système de conception Fluent

Si vous souhaitez en savoir plus sur les principes sous-jacents de Fluent Design (système de conception de Microsoft) et voir plus de fonctionnalités pouvant être incorporées dans votre application UWP, accédez à [Système Fluent Design](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Articles connexes

- [Qu’est-ce qu’une application UWP ?](../../get-started/universal-application-platform-guide.md)
- [Système de conception Fluent](/windows/apps/fluent-design-system)
- [Vue d’ensemble de la plateforme XAML](../../xaml-platform/index.md)
