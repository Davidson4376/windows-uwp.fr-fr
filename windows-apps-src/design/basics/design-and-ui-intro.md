---
author: mijacobs
Description: The universal design features included in every UWP app help you build apps that scale beautifully across a range of devices.
title: Présentation de la conception des applications de plateforme Windows universelle (UWP) (applications Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: mijacobs
ms.date: 05/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 952db87d0dabdb927a472de17f0c0d7b345bde4e
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2018
ms.locfileid: "3823743"
---
# <a name="introduction-to-uwp-app-design"></a>Présentation de la conception des applicationsUWP

![exemple d’application d'éclairage](images/introUWP-header.jpg)

Le guide de conception pour la plateforme Windows universelle (UWP) est une ressource destinée à vous aider à concevoir des applications belles et élégantes.

Il ne s’agit pas d’une liste de règles normatives, mais plutôt d’un document dynamique, conçu pour s’adapter à l’évolution de notre [système FluentDesign](../fluent-design-system/index.md) ainsi qu’aux besoins de notre communauté de développement d’applications.

Cette nouvelle introduction présente une vue d’ensemble des fonctionnalités de conception universelle qui sont incluses dans chaque applicationUWP. Elle vous aide à créer des interfaces utilisateur (UI) qui s’adaptent parfaitement à toute une gamme d’appareils.

## <a name="effective-pixels-and-scaling"></a>Pixels effectifs et mise à l’échelle

Tout d’abord, les applications UWP s’exécutent sur tous les [appareils Windows10](../devices/index.md), depuis votre téléviseur, jusqu'à votre tablette ou votre PC. Comment cela affecte-t-il l’interface utilisateur de votre application?

![même application sur différents appareils](images/universal-image-1.jpg)

Heureusement pour vous, les applicationsUWP ajustent automatiquement la taille des éléments d’interface utilisateur afin de se rendre accessibles sur l’ensemble des appareils et des tailles d'écran et de pouvoir facilement interagir avec eux!

Lorsque votre application est exécutée sur un appareil, le système utilise un algorithme afin de normaliser l’affichage à l’écran des éléments d’interface utilisateur. Cet algorithme de mise à l’échelle prend en compte la distance d’affichage et la densité de l’écran (en pixels par pouce) pour optimiser la taille perçue (plutôt que la taille physique). L’algorithme de mise à l’échelle garantit qu’une police de 24pixels sur un appareil Surface Hub placé à une distance de 3 mètres est aussi lisible pour l’utilisateur qu’une police de 24pixels sur un téléphone doté d’un écran 5pouces distant de quelques centimètres.

![distances d’affichage des différents appareils](images/scaling-chart.png)

En raison du mode de fonctionnement du système de mise à l’échelle, lorsque vous concevez une applicationUWP, la conception est effectuée en pixels effectifs et non en pixels physiques réels. Les pixels effectifs (epx) sont une unité de mesure virtuelle qui sert à exprimer les dimensions et l’espacement de la disposition, indépendamment de la densité de l’écran. (Dans nos recommandations, epx ep et px sont utilisées indifféremment.)

Vous pouvez ignorer la densité de pixels et la résolution d’écran réelle lors de la conception. Effectuez plutôt une conception pour la résolution réelle (résolution en pixels effectifs) d’une classe de taille (pour plus d’informations, consultez l’[article Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Lors de la création de maquettes d’écran dans des programmes d’édition d’image, définissez les PPP sur72 et les dimensions d’image sur la résolution réelle pour la classe de taille que vous ciblez. Pour obtenir la liste des catégories de taille et des résolutions réelles, consultez l’[article Tailles d’écran et points d’arrêt](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

### <a name="multiples-of-four"></a>Multiples de quatre

:::row:::
    ::: column span::: les tailles, les marges et les positions des éléments d’interface utilisateur doivent toujours être des **multiples de 4 epx** dans vos applications UWP.

        UWP scales across a range of devices with scaling plateaus of 100%, 125%, 150%, 175%, 200%, 225%, 250%, 300%, 350%, and 400%. The base unit is 4 because it's the only integer that can be scaled by non-whole numbers (e.g. 4*1.5 = 6). Using multiples of four aligns all UI elements with whole pixels and ensures UI elements have crisp, sharp edges. (Note that text doesn't have this requirement; text can have any size and position.)
    :::column-end:::
    :::column:::
        ![grid](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>Disposition

Puisque les applications UWP s'adaptent automatiquement à tous les appareils, la conception d’une application UWP présentera la même structure quel que soit l'appareil. Commençons par le début de l’interface utilisateur de votre application UWP.

### <a name="windows-frames-and-pages"></a>Fenêtres, cadres et pages

:::row:::
    :::column:::
        Lorsqu’une application UWP est lancée sur n’importe quel appareil Windows 10, elle se lance dans une [fenêtre](/uwp/api/Windows.UI.Xaml.Controls.Window) avec une [image](/uwp/api/Windows.UI.Xaml.Controls.Frame), ce qui peut naviguer entre les instances de la [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) .
    :::column-end:::
    :::column:::
        ![Trame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        Vous pouvez considérer l’interface utilisateur de votre application comme une collection de pages. C’est à vous de choisir les éléments qui vont sur chaque page et les relations entre les pages.

        To learn how you can organize your pages, see [Navigation basics](navigation-basics.md).
    :::column-end:::
    :::column:::
        ![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>Mise en page

Quel aspect doivent avoir ces pages? La plupart des pages suivent une structure commune pour garantir la cohérence, afin que les utilisateurs puissent facilement naviguer entre les pages et dans chaque page de votre application. Les pages contiennent généralement trois types d’éléments d’interface utilisateur:

- Les éléments de [Navigation](navigation-basics.md) permettent aux utilisateurs de choisir le contenu à afficher.
- Les éléments de [Commande](commanding-basics.md) permettent d’initier des actions (de manipulation, d’enregistrement ou de partage de contenu, par exemple).
- Les éléments de [Contenu](content-basics.md) affichent le contenu de l’application.

![Un modèle de disposition courante](../layout/images/page-components.svg)

Pour en savoir plus sur l’implémentation de modèles d’application UWP courants, consultez l'article [Mise en Page](../layout/page-layout.md).

Vous pouvez également utiliser [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) dans Visual Studio pour commencer une disposition pour votre application.

## <a name="controls"></a>Contrôles

La plateforme de conception UWP fournit un ensemble de contrôles courants qui sont garantis pour fonctionner correctement sur tous les appareils fonctionnant sous Windows, et qui respectent nos principes de [Système Fluent Design](../fluent-design-system/index.md). Ces contrôles incluent tous les éléments de contrôle, qu’ils soient simples comme les boutons et les éléments de texte, ou plus complexes et capables de générer des listes à partir d’un ensemble de données et d’un modèle.

![Contrôles UWP](../style/images/color/windows-controls.svg)

Pour obtenir la liste complète des contrôlesUWP et des modèles que vous pouvez créer à partir de ceux-ci, consultez la [section consacrée aux contrôles et aux modèles](../controls-and-patterns/index.md).

## <a name="style"></a>Style

Les contrôles courants reflètent automatiquement la couleur d’accentuation et le thème système, ils sont compatibles avec tous les types de saisie et s’adaptent à tous les appareils. Ainsi, ils sont représentatifs du Système Fluent Design: ils sont adaptatifs, empathiques et esthétiques. Les contrôles courants utilisent la lumière, le mouvement et la profondeur dans leur style par défaut, donc en les utilisant, vous incorporez notre Système Fluent Design dans votre application.

Les contrôles courants sont hautement personnalisables. Vous pouvez modifier la couleur de premier plan d’un contrôle ou personnaliser complètement son apparence. Pour remplacer les styles par défaut dans les contrôles, utilisez [styles légers](../controls-and-patterns/xaml-styles.md#lightweight-styling) ou créez des [contrôles personnalisés](../controls-and-patterns/control-templates.md) en XAML.

![Couleur d’accentuation gif](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
        Votre application UWP interagira avec l’expérience Windows plus large de vignettes et notifications dans le Windows [Shell](../shell/tiles-and-notifications/creating-tiles.md).

        Tiles are displayed in the Start menu and when your app launches, and they provide a glimpse of what's going on in your app. Their power comes from the content behind them, and the intelligence and craft with which they're offered up.

        UWP apps have four tile sizes (small, medium, wide, and large) that can be customized with the app's icon and identity. For guidance on designing tiles for your UWP app, see [Guidelines for tile and icon assets](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
        ![tiles on start menu](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>Entrées

:::row:::
    :::column:::
        Les applicationsUWP s’appuient sur des interactions intelligentes. Vous pouvez concevoir une fonction sur la base d’une interaction de clic sans savoir nécessairement si le clic provient d’un clic de la souris, d’un stylet ou d’une pression du doigt. Vous pouvez également concevoir vos applications pour des [modes de saisie spécifiques](../input/input-primer.md).
    :::column-end:::
    :::column:::
        ![saisies](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>Appareils

![appareils](../layout/images/size-classes.svg)

De même, bien qu'UWP adapte automatiquement votre application à différents appareils, vous pouvez également [optimiser votre application UWP pour des appareils spécifiques](../devices/index.md).

## <a name="usability"></a>Facilité d'utilisation

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

Enfin, la facilité d'utilisation consiste à rendre l’expérience de votre application ouverte à tous les utilisateurs. Tout le monde peut bénéficier de l’expérience utilisateur véritablement inclusive. Pour voir comment rendre votre application facile à utiliser par tous, voir [Facilité d’utilisation pour les applicationsUWP](../usability/index.md).

Si vous concevez pour un public international, vous souhaiterez peut-être consulter la page [Globalisation et localisation](../globalizing/globalizing-portal.md).

Vous souhaiterez peut-être également intégrer des [fonctionnalités d’accessibilité](../accessibility/accessibility-overview.md) pour les utilisateurs présentant des problèmes de vision, d’audition et de mobilité. Si vous intégrez l’accessibilité dès le départ dans votre conception, le fait de [rendre votre application accessible](../accessibility/accessibility-in-the-store.md) ne vous demandera que peu de temps et d’efforts supplémentaire.

## <a name="tools-and-design-toolkits"></a>Outils et kits de ressources de conception

Maintenant que vous connaissez les fonctionnalités de création de base, découvrez comment concevoir votre applicationUWP.

Nous proposons divers outils pour vous aider à réaliser votre conception:

- Consultez notre [page des kits de ressources de conception](../downloads/index.md) pour les kits de ressources XD, Illustrator, Photoshop, Framer et Sketch, ainsi que des outils de conception supplémentaires et des téléchargements de police.

- Pour configurer votre ordinateur afin de pouvoir écrire du code pour les applicationsUWP, consultez notre article [Prise en main &gt; Préparation](../../get-started/get-set-up.md).

- Pour apprendre à implémenter l’interface utilisateur pour UWP, jetez un coup œil à nos [exemples d’applications UWP](https://developer.microsoft.com/windows/samples) de bout en bout.

## <a name="video-summary"></a>Résumé de la vidéo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>Suivant: Système FluentDesign

Si vous souhaitez en savoir plus sur les principes sous-jacents de FluentDesign (système de conception de Microsoft) et voir plus de fonctionnalités pouvant être incorporées dans votre applicationUWP, accédez à [Système FluentDesign](../fluent-design-system/index.md).

## <a name="related-articles"></a>Articles associés

- [Qu’est-ce qu’une application UWP?](../../get-started/universal-application-platform-guide.md)
- [Système Fluent Design](../fluent-design-system/index.md)
- [Vue d’ensemble de la plateforme XAML](../../xaml-platform/index.md)
