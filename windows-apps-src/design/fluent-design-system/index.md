---
description: Découvrez Fluent Design et comment l'intégrer à vos applications.
title: Système Fluent Design pour les applications UWP
author: mijacobs
keywords: disposition d’application uwp, plateforme windows universelle, conception d’application, interface, système fluent design
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5b57dc2ddae4c6e260df663097db5649866b96aa
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
ms.locfileid: "1638787"
---
# <a name="the-fluent-design-system-for-uwp-apps"></a>Système Fluent Design pour les applications UWP

## <a name="introduction"></a>Introduction

<img src="images/fluentdesign-app-header.jpg" alt=" " />

L’interface utilisateur évolue. Elle s’étend pour inclure de nouvelles dimensions et interfaces, de la 2D à la 3D et au-delà, du clavier et de la souris au pointage du regard, au stylet et à la fonction tactile.  

Le système Fluent Design est un ensemble de fonctionnalités UWP innovantes, combinées avec les meilleures pratiques pour créer des applications qui s'exécutent parfaitement sur tous les types d’appareils fonctionnant sous Windows.

C’est notre système pour la création d’interfaces utilisateur adaptatives, empathiques et esthétiques. 

**Adaptatives: les expériences Fluent paraissent naturelles sur chaque appareil.**

Les expériences Fluent s’adaptent à l’environnement. Une expérience Fluent est agréable sur une tablette, un PC de bureau et une Xbox&mdash;elle fonctionne même parfaitement sur un casque de réalité mixte. Et lorsque vous ajoutez du matériel supplémentaire, comme un nouveau moniteur pour votre PC, une expérience Fluent en tire profit. 

**Empathiques: les expériences Fluent sont intuitives et puissantes**

Les expériences Fluent s’ajustent au comportement et aux intentions&mdash;elles comprennent et anticipent ce qui est nécessaire. Elles unissent les personnes et les idées, qu’elles se trouvent aux extrémités opposées de la planète ou juste les unes à côté des autres. 

Montrer de l'empathie c’est exécuter la bonne action au bon moment. 

Les expériences Fluent utilisent des contrôles et des modèles de façon cohérente, de sorte qu’elles se comportent comme l’utilisateur s’y attend. Les expériences Fluent sont accessibles aux personnes avec un large éventail de capacités physiques et incorporent des fonctionnalités de globalisation de sorte que les personnes du monde entier peuvent les utiliser. 

**Esthétiques: les expériences Fluent sont attractives et immersives** 

En intégrant des éléments du monde physique, une expérience Fluent exploite quelque chose de fondamental. Elle utilise la lumière, l’ombre, le mouvement, la profondeur et la texture pour organiser les informations d’une façon qui semble intuitive et instinctive. 

Fluent Design ne consiste pas à créer des effets tape-à-l'œil. Il intègre des effets physiques qui améliorent véritablement l’expérience utilisateur, car ils émulent des expériences que nos cerveaux sont programmés pour traiter de manière efficace. 

## <a name="applying-fluent-design-to-your-app"></a>Utilisation de Fluent Design dans votre application

Les fonctionnalités Fluent Design sont intégrées dans UWP. Certaines de ces fonctionnalités&mdash;comme les pixels effectifs et le système d’entrée universel&mdash; sont automatiques. Il n’est pas nécessaire d’écrire du code supplémentaire pour en tirer parti. D'autres fonctionnalités, telles qu’acrylique, sont facultatives: vous les ajoutez à votre application en écrivant du code pour les inclure. 

> Pour en savoir plus sur les fonctionnalités de base qui sont automatiquement incluses dans chaque application UWP, voir l’article [Introduction à la conception d’applications UWP](../basics/design-and-ui-intro.md). Si vous débutez dans le développement UWP, vous souhaiterez peut-être découvrir en premier notre page [Prise en main de UWP](https://developer.microsoft.com/windows/apps/getstarted). 

Pour en savoir plus sur les nouvelles fonctionnalités qui vous permettent d'intégrer Fluent Design dans votre application, poursuivez la lecture.

## <a name="find-a-natural-fit"></a>Trouvez une solution naturelle

Comment faire qu’une application semble naturelle sur un grand nombre d’appareils? En donnant l’impression qu’elle a été conçue pour chaque appareil spécifique. Une disposition de l’interface utilisateur qui s’adapte à différentes tailles d’écran&mdash;comme cela aucun espace n'est perdu (ni encombré non plus)&mdash;rend une expérience naturelle, comme si elle avait été spécialement conçue pour cet appareil. 

*  **Concevez pour les points d’arrêt appropriés**

    Au lieu de concevoir une application pour chaque taille d’écran, le fait de se concentrer sur plusieurs largeurs principales (également appelées «points d’arrêt») peut considérablement simplifier vos conceptions et votre code, tout en donnant à votre application une apparence remarquable sur les petits comme sur les grands écrans.

    [En savoir plus sur les tailles d’écran et les points d’arrêt](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)

*  **Créez une disposition dynamique**

    Pour qu’une application semble naturelle, elle doit remplir l’espace d’affichage disponible sans paraître trop encombrée. UWP fournit des panneaux qui organisent le contenu en grilles, piles et flux, que vous pouvez imbriquer les uns dans les autres.

    [En savoir plus sur les panneaux de disposition UWP](/windows/uwp/design/layout/layout-panels)

* **Concevez pour un large éventail d’appareils**

    Les applications UWP peuvent s’exécuter sur un large éventail d’appareils fonctionnant sous Windows. Il est utile de comprendre quels appareils sont disponibles, à quoi ils servent et comment les utilisateurs interagissent avec.

    [En savoir plus sur les appareils UWP](/windows/uwp/design/devices/)

* **Optimisez pour l’entrée appropriée**

    Les applications UWP prennent automatiquement en charge les interactions souris, clavier, stylet et tactile courantes&mdash;vous n’avez rien de spécial à faire. Toutefois, vous pouvez améliorer votre application avec une prise en charge optimisée pour des entrées spécifiques, comme le stylet et Surface Dial.

    [En savoir plus sur les entrées et les interactions](/windows/uwp/design/input/input-primer)


## <a name="make-it-intuitive-and-powerful"></a>Rendez-la intuitive et puissante

Une expérience semble intuitive quand elle se comporte comme l’utilisateur s’y attend. À l’aide des contrôles et des modèles établis, et en tirant profit de la prise en charge de plates-formes pour l’accessibilité et la globalisation, vous pouvez créer une expérience intuitive qui permet aux utilisateurs d’être plus productifs. 

* **Utilisez les contrôles appropriés pour la tâche**

    Les contrôles sont des blocs de construction de l’interface utilisateur; l’utilisation du contrôle approprié vous permet de créer une interface utilisateur qui se comporte comme les utilisateurs s’y attendent.  UWP fournit plus de 45contrôles, des simples boutons aux contrôles de données puissants. 

    [En savoir plus sur les contrôles UWP](/windows/uwp/design/controls-and-patterns/)

* **Soyez complet** 

    Une application bien conçue est accessible aux personnes souffrant de handicaps ou d’invalidités. Avec du codage supplémentaire, vous pouvez partager votre application avec des personnes dans le monde entier.

    [En savoir plus sur la facilité d'utilisation](/windows/uwp/design/usability/)


## <a name="be-engaging-and-immersive"></a>Soyez attrayant et immersif 

Rendez votre application attrayante en incorporant des éléments physiques, tels que de la lumière et du mouvement. 

## <a name="use-light"></a>Utilisez la lumière

La lumière a le don d'attirer notre attention. Elle crée une ambiance et un sentiment d’appartenance. C’est aussi un outil pratique pour éclairer des informations.
        
Ajoutez de la lumière à votre application UWP:
        
* L’effet [Révéler](../style/reveal.md) utilise la lumière pour faire ressortir des éléments interactifs. La lumière illumine les éléments avec lesquels l’utilisateur peut interagir, en révélant des bordures masquées. Révéler est automatiquement activé sur certains contrôles, tels que l’affichage Liste et l’affichage Grille. Vous pouvez l’activer sur d’autres contrôles en appliquant nos styles Révéler prédéfinis. 

* L’effet [Révéler focus](../style/reveal-focus.md) utilise la lumière pour attirer l’attention sur l’élément sur lequel le focus est positionné.  

## <a name="create-a-sense-of-depth"></a>Créez une idée de profondeur

Nous vivons dans un monde en trois dimensions. En intégrant délibérément de la profondeur dans l’interface utilisateur, nous transformons une interface2D plate en quelque chose de mieux&mdash;quelque chose qui présente efficacement des informations et des concepts en créant une hiérarchie visuelle. Cela réinvente la relation entre les éléments dans un environnement physique stratifié.

Ajoutez de la profondeur à votre application UWP:

* [Acrylique](../style/acrylic.md) est un matériau translucide qui permet à l’utilisateur de voir des couches de contenu, en établissant une hiérarchie d’éléments d’interface utilisateur.

* [Parallaxe](../motion/parallax.md) crée l’illusion de profondeur en donnant l’impression que les éléments situés au premier plan se déplacent plus rapidement que ceux de l’arrière-plan.

## <a name="incorporate-motion"></a>Incorporez du mouvement

Imaginez la conception du mouvement comme un film. Les transitions transparentes vous laissent concentrés sur l’histoire et donnent vie aux expériences. Nous pouvons inviter ces sentiments dans nos conceptions, en guidant les utilisateurs d’une tâche à l’autre avec une aisance cinématographique.

Ajoutez du mouvement à votre application UWP:

* Les [animations connectées](../motion/connected-animation.md) aident l’utilisateur à préserver le contexte en créant une transition transparente entre les scènes. 

## <a name="build-it-with-the-right-material"></a>Générez-la avec la matière appropriée

Les éléments qui nous entourent dans le monde réel sont sensoriels et stimulants. Ils plient, s'étirent, rebondissent, éclatent et glissent. Ces caractéristiques matérielles se traduisent en environnements numériques et donnent envie aux utilisateurs d'attraper et de toucher nos conceptions.

Ajoutez de la matière à votre application UWP: 
        
* [Acrylique](../style/acrylic.md) est un matériau translucide qui permet à l’utilisateur de voir des couches de contenu, en établissant une hiérarchie d’éléments d’interface utilisateur. 

## <a name="design-toolkits-and-code-samples"></a>Kits de ressources et exemples de code

Vous voulez commencer à créer vos propres applications avec Fluent Design? Nos kits de ressources pour AdobeXD, Adobe Illustrator, Adobe Photoshop, Framer et Sketch vous aideront à accélérer vos conceptions, et nos exemples vous aideront à coder plus rapidement.

* Découvrez notre page [Kits de ressources et exemples de conception](/windows/uwp/design/downloads/)

<img src="images/fluentdesign_header.png" alt=" " />








