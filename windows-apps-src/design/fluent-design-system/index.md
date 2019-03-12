---
description: Découvrez Fluent Design et comment l’intégrer à vos applications.
title: Système Fluent Design pour Windows
keywords: 'disposition d’application uwp, plateforme windows universelle, conception d’application, interface, système fluent design'
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Système Fluent Design pour créateurs d’applications Windows

![En-tête Fluent Design](images/fluentdesign-app-header.jpg)

## <a name="introduction"></a>Introduction

Fluent Design est un système de création d’interfaces utilisateur adaptatives, intuitives et esthétiques.

## <a name="principles"></a>Principes

**Adaptive : les expériences Fluent paraissent naturelles sur tous les appareils**

Les expériences Fluent s’adaptent à l’environnement. Une expérience Fluent est agréable aussi bien sur une tablette que sur un PC ou une Xbox. Elle fonctionne même parfaitement sur un casque de réalité mixte. Lorsque vous ajoutez du matériel, comme un nouveau moniteur pour votre PC, l’expérience Fluent en tire parti.

**Intuitive : les expériences Fluent sont intuitives et puissantes**

Les expériences Fluent s’ajustent au comportement et aux intentions. Elles comprennent et anticipent ce dont l’utilisateur a besoin. Elles unissent les personnes et les idées, qu’elles soient géographiquement proches ou éloignées.

**Esthétique : les expériences Fluent sont attractives et immersives**

En intégrant des éléments du monde physique, une expérience Fluent exploite quelque chose de fondamental. Elle utilise la lumière, l’ombre, le mouvement, la profondeur et la texture pour organiser les informations d’une façon qui semble intuitive et instinctive.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>Utilisation de Fluent Design dans votre application avec UWP

![Logo Fluent Design](images/fluentdesign_header.png)

Nos instructions de conception expliquent comment appliquer les principes de Fluent Design à vos applications. Pour quel type d’applications ? Même si la plupart de nos instructions peuvent s’appliquer à n’importe quelle plateforme, nous avons créé la plateforme Windows universelle (UWP) pour prendre en charge Fluent Design.

Les fonctionnalités Fluent Design sont intégrées à UWP. Certaines de ces fonctionnalités (comme les pixels effectifs et le système d’entrée universel) sont automatiques. Il n’est pas nécessaire d’écrire du code supplémentaire pour en bénéficier. D’autres fonctionnalités, telles que l’acrylique, sont facultatives : vous pouvez les ajouter à votre application en écrivant du code.

> Nous mettons à disposition les contrôles UWP pour les postes de travail afin que vous puissiez améliorer l’apparence et les fonctionnalités de vos applications WPF ou Windows existantes avec des fonctionnalités Fluent Design. Pour plus d’informations, consultez [Héberger des contrôles UWP dans les applications WPF et Windows Forms](/windows/uwp/xaml-platform/xaml-host-controls).

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

En plus de fournir des conseils de conception, nos articles Fluent Design vous expliquent comment écrire le code de vos conceptions. UWP utilise le langage de balisage XAML, qui facilite la création d’interfaces utilisateur. Voici un exemple :

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![](images/xaml-example.png)


> Si vous ne connaissez pas bien le développement UWP, consultez la page [Bien démarrer avec UWP](https://developer.microsoft.com/windows/apps/getstarted).

## <a name="find-a-natural-fit"></a>Trouvez une solution naturelle

Comment faire pour qu’une application semble naturelle sur un grand nombre d’appareils ? En donnant l’impression qu’elle a été conçue pour chacun de ces appareils. Une disposition qui s’adapte à différentes tailles d’écran (pour éviter aussi bien les pertes d’espace que l’encombrement de l’écran) rend l’expérience naturelle, comme si elle avait été spécialement conçue pour l’appareil.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Rendez l’utilisation intuitive

Une expérience semble intuitive quand elle se comporte comme l’utilisateur s’y attend. En tirant parti des contrôles et des modèles établis, ainsi que des plateformes d’accessibilité et de globalisation, vous pouvez créer une expérience intuitive qui permet aux utilisateurs d’être plus productifs.

L’expérience intuitive est obtenue lorsque la bonne action est exécutée au bon moment.

Les expériences Fluent utilisent des contrôles et des modèles de façon cohérente, de sorte qu’elles se comportent comme l’utilisateur s’y attend. Les expériences Fluent sont accessibles aux personnes avec un large éventail de capacités physiques et incorporent des fonctionnalités de globalisation de sorte que les personnes du monde entier peuvent les utiliser.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Soyez attrayant et immersif

Fluent Design ne consiste pas à créer des effets tape-à-l’œil. Il intègre des effets physiques qui améliorent véritablement l’expérience utilisateur, car ils émulent des expériences que nos cerveaux sont programmés pour traiter de manière efficace.

## <a name="use-light"></a>Utilisez la lumière

La lumière a le don d’attirer notre attention. Elle crée une ambiance et un sentiment d’appartenance. C’est aussi un outil pratique pour éclairer des informations.

Ajoutez de la lumière à votre application UWP :

:::row:::
    :::column:::
        ![fpo image](../style/images/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](../style/reveal.md) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](../style/images/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](../style/reveal-focus.md) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Créez une profondeur

Nous vivons dans un monde en trois dimensions. En intégrant délibérément de la profondeur dans l’interface utilisateur, nous transformons une interface 2D plate en quelque chose de mieux, quelque chose qui présente efficacement des informations et des concepts en créant une hiérarchie visuelle. Cela réinvente la relation entre les éléments dans un environnement physique stratifié.

Ajoutez de la profondeur à votre application UWP :

:::row:::
    :::column:::
        ![fpo image](../motion/images/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](../motion/parallax.md) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>Incorporez du mouvement

Imaginez la conception du mouvement comme un film. La fluidité des transitions vous permet de vous concentrer sur l’histoire et donne vie aux expériences. Nous pouvons inviter ces sentiments dans nos conceptions, en guidant les utilisateurs d’une tâche à l’autre avec une aisance cinématographique.

Ajoutez du mouvement à votre application UWP :

:::row:::
    :::column:::
        ![continuity gif](images/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](../motion/connected-animation.md) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Générez-la avec la matière appropriée

Les éléments qui nous entourent dans le monde réel sont sensoriels et stimulants. Ils plient, s’étirent, rebondissent, éclatent et glissent. Ces caractéristiques matérielles se traduisent en environnements numériques et donnent envie aux utilisateurs d’interagir avec nos conceptions.

Ajoutez de la matière à votre application UWP :

:::row:::
    :::column:::
        ![fpo image](../style/images/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](../style/acrylic.md) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Kits de ressources et exemples de code

Vous voulez commencer à créer vos propres applications avec Fluent Design ? Nos kits de ressources pour Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer et Sketch vous aideront à accélérer vos conceptions, et nos exemples vous aideront à coder plus rapidement.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









