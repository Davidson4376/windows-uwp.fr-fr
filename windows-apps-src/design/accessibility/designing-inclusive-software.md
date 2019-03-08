---
Description: En savoir plus sur l’évolution de la conception inclusive avec les applications de la plateforme Windows universelle (UWP) pour Windows 10.  Concevez et développez un logiciel inclusif en tenant compte de l’accessibilité.
ms.assetid: A6393A57-53F2-4F06-89AF-0D806FD76DB0
title: Conception de logiciels inclusifs dans Windows 10
label: Designing inclusive software
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 88d2377f95efbe79853b1416213a25e22ed7d5f0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639694"
---
# <a name="designing-inclusive-software-for-windows-10"></a>Conception de logiciels inclusifs pour Windows 10  

En savoir plus sur l’évolution de la conception inclusive avec les applications de la plateforme Windows universelle (UWP) pour Windows 10.  Concevez et développez un logiciel inclusif en tenant compte de l’accessibilité.

Chez Microsoft, nous faisons évoluer nos principes et pratiques de conception. Ces derniers dictent l’aspect, la fonction et le comportement de nos expériences. Nous enrichissons notre perspective.

Cette nouvelle philosophie de conception est appelée conception inclusive. L’idée consiste à concevoir un logiciel qui s’adresse à tous dès le départ. Cela diffère du processus qui consiste à intégrer l’élément d’accessibilité à la toute fin du processus de développement, ce qui convient uniquement à un groupe d’utilisateurs restreint.

« Nous définissons le handicap/invalidité comme l’incompatibilité entre les besoins de l’individu et le service, le produit ou l’environnement proposés. Tout le monde peut rencontrer un handicap/invalidité. L’exclusion est très courante dans notre société humaine. »  \- à partir de la [inclusif](https://www.microsoft.com/design/inclusive) vidéo  

La conception inclusive crée de meilleurs produits pour tout le monde. Il s’agit de créations pouvant s’appliquer à l’ensemble de la diversité humaine. Prenons l’exemple des abaissements de trottoir présents sur la plupart des trottoirs. À l’origine, ils étaient destinés aux personnes en fauteuil roulant. Mais presque tout le monde s’en sert à présent, y compris les personnes avec des poussettes, les cyclistes et les utilisateurs de skate-board. Même les piétons profitent souvent de ces aménagements qui facilitent la vie. La télécommande d’une TV peut être considérée comme une technologie d’assistance pour une personne atteinte d’un handicap moteur. Et pourtant, presque chaque télévision est accompagnée d’une télécommande. Avant que les enfants n’apprennent à lacer leurs chaussures, ils peuvent porter des chaussures simples à enfiler ou à fermeture facile. Les chaussures simples à enfiler et à enlever sont souvent préférées dans les cultures où l’on se déchausse avant d’entrer dans une maison. Elles sont également mieux adaptées aux personnes ayant des problèmes de dextérité et souffrant par exemple d’arthrite ou d’une fracture du poignet.

## <a name="inclusive-design-principles"></a>Principes de conception inclusive.  
Les 4 principes suivants guident la transition de Microsoft vers la conception inclusive :

**Pensez universelle**: Nous nous concentrons sur ce qui unifie les personnes, des motivations humaines, les relations et les capacités. Cela nous oblige également de prendre en compte l’impact social élargi de notre travail. Le résultat obtenu constitue une expérience à laquelle tout le monde peut participer de manière différente.

**Vérifiez personnelle**: Ensuite, nous défis pour créer des connexions émotionnels. Les interactions humaines peuvent inspirer une meilleure interaction entre la technologie et les hommes. La situation unique d’une personne peut améliorer la conception pour tout le monde. Le résultat obtenu constitue une expérience donnant l’impression d’avoir été créée sur mesure pour tous.

**Simplicité**: La simplicité constitue le fondement de notre stratégie. Avec un affichage plus clair, nos utilisateurs se sentent plus à l’aise. Les espaces épurés, clairs et ouverts renforcent leur confiance. Nous offrons une expérience honnête, intemporelle.

**Créer qui raviront**: Expériences plaisante évoquer étonnant et découverte. Parfois, cela relève de la magie. Parfois, il s’agit d’un détail parfait. Nous concevons ces moments pour proposer à nos utilisateurs un changement bienvenu. Le résultat obtenu constitue une expérience fluide et continue.

## <a name="inclusive-design-users"></a>Utilisateurs de la conception inclusive  
On distingue deux catégories d’utilisateurs de la technologie d’assistance :

1. Ceux qui en dépendent du fait d’un handicap ou d’une invalidité, de leur âge avancé ou d’une situation temporaire (par exemple, mobilité réduite à cause d’un plâtre).  
2. Ceux qui l’utilisent pour une expérience informatique plus confortable ou pratique.

La majorité (54 %) des utilisateurs d’ordinateur connaît une technologie d’assistance quelconque, et 44 % des utilisateurs en utilisent une, mais bon nombre d’entre eux n’utilisent pas une technologie d’assistance qui leur serait bénéfique (Forrester 2004).  

Une étude commandée par Microsoft et menée entre 2003 et 2004 par l’institut Forrester Research a permis d’établir que plus de la moitié &mdash;57 %&mdash; des utilisateurs d’ordinateur aux États-Unis entre 18 et 64 ans pouvaient améliorer leur expérience à l’aide d’une technologie d’assistance. La plupart de ces utilisateurs ne s’identifient pas comme étant handicapés ou invalides, mais ils expriment certaines difficultés ou troubles quant à certaines tâches liées à l’utilisation d’un ordinateur. Forrester (2003) trouve également le nombre d’utilisateurs avec ces difficultés spécifiques suivant : Un dans quatre rencontre une difficulté visual. Un utilisateur sur quatre ressent une douleur au niveau des poignets ou des mains. Un utilisateur sur cinq souffre de troubles auditifs.  

En plus des handicaps permanents, la gravité et les types de difficulté que rencontrent les individus peuvent varier tout au long de leur vie. L’« individu normal » n’existe pas. Nos capacités sont en constante évolution. Margaret Meade a déclaré : « Nous sommes tous uniques. En étant tous uniques, nous sommes tous pareils. »  

Microsoft s’est engagé à mener des recherches en matière d’informatique et de génie logiciel afin d’améliorer l’expérience informatique et d’inventer des technologies informatiques innovantes. Consultez les [projets actuels de recherche et développement Microsoft](https://www.microsoft.com/enable/microsoft/research.aspx), visant à rendre l’ordinateur plus accessible, plus visible et audible, ainsi que de faciliter l’interaction avec celui-ci.  

## <a name="practical-design-steps"></a>Étapes pratiques de la conception  
Si vous êtes en pleine phase de conception, cette section est faite pour vous. Elle décrit les étapes pratiques de conception à prendre en compte lors de l’implémentation de la conception inclusive dans votre application.  

### <a name="describe-the-target-audience"></a>Décrire le public visé  
Déterminez les utilisateurs potentiels de votre application. Réfléchissez à toutes leurs aptitudes et caractéristiques différentes. Par exemple : l’âge, le sexe, la langue, les utilisateurs sourds, malentendants ou atteints de déficiences visuelles, les capacités cognitives, le style d’apprentissage, les restrictions de mobilité, etc. Votre conception répond-elle à leurs besoins individuels ?  

### <a name="talk-to-actual-humans-with-specific-needs"></a>Discuter avec des personnes ayant des besoins spécifiques  
Rencontrez des utilisateurs potentiels aux caractéristiques diverses. Assurez-vous de prendre en compte tous leurs besoins lors de la conception de votre application. Par exemple, Microsoft a découvert que les utilisateurs sourds désactivaient les notifications toast sur leurs consoles Xbox. Lorsque nous les avons interrogés à ce sujet, nous avons appris que les notifications toast masquaient une partie des sous-titres. Nous avons corrigé cela en déplaçant le toast légèrement plus haut sur l’écran. Il s’agissait d’une solution simple qui n’était pas nécessairement évidente en se basant sur les données de télémétrie ayant révélé ce comportement.  

### <a name="choose-a-development-framework-wisely"></a>Choisir judicieusement une infrastructure de développement  
Lors de la phase de conception, l’infrastructure de développement que vous utiliserez (c’est-à-dire UWP, Win32, le web) est essentielle pour le développement de votre produit. Si vous avez la chance de pouvoir choisir votre infrastructure, réfléchissez à l’effort nécessaire pour la création de vos contrôles au sein de celle-ci. Quelles sont les propriétés d’accessibilité par défaut ou intégrées comprises dans l’infrastructure ? Quels contrôles devez-vous personnaliser ? Lorsque vous choisissez l’infrastructure, il s’agit en fait de choisir quels contrôles d’accessibilité vous obtiendrez « gratuitement » (autrement dit, combien d’entre eux sont déjà intégrés) et combien d’entre eux nécessiteront des coûts de développement supplémentaires en raison des personnalisations de contrôles.   

Utilisez les contrôles Windows standard lorsque c’est possible. Ces contrôles sont déjà activés avec la technologie nécessaire pour établir un lien avec les technologies d’assistance.

### <a name="design-a-logical-hierarchy-for-your-controls"></a>Concevoir une hiérarchie logique pour vos contrôles  
Une fois que vous avez votre infrastructure, concevez une hiérarchie logique pour mapper vos contrôles. La hiérarchie logique de votre application inclut la disposition et l’ordre de tabulation des contrôles. La présentation visuelle de l’interface utilisateur n’est pas suffisante lorsque les technologies d’assistance (telles que les lecteurs d’écran) lisent votre interface utilisateur. Vous devez fournir une alternative programmatique pertinente du point de vue structurel pour vos utilisateurs. Une hiérarchie logique peut vous aider pour cette fin. C’est un moyen d’étudier la disposition de votre interface utilisateur et de structurer chaque élément pour que les utilisateurs puissent la comprendre. Une hiérarchie logique est surtout utilisée :  

1.  pour fournir aux programmes un contexte pour l’ordre logique de lecture des éléments dans l’interface utilisateur ;  
2.  pour identifier les frontières claires entre les contrôles personnalisés et les contrôles standard dans l’interface utilisateur ;  
3.  pour déterminer la façon dont les éléments de l’interface utilisateur interagissent.  

Une hiérarchie logique est un excellent moyen de résoudre les problèmes potentiels liés à l’utilisation. Si vous ne pouvez pas structurer l’interface utilisateur d’une manière relativement simple, vous pouvez avoir des problèmes liés à l’utilisation. Une représentation logique d’une boîte de dialogue simple ne doit pas se traduire par plusieurs pages de diagrammes. Si votre hiérarchie logique devient trop longue ou trop large, vous devriez peut-être refaire votre interface utilisateur. Pour en savoir plus, téléchargez le livre électronique [Engineering Software for Accessibility (Conception de logiciels accessibles)](https://www.microsoft.com/download/details.aspx?id=19262).  

### <a name="design-appropriate-visual-ui-settings"></a>Concevoir des paramètres appropriés de l’interface utilisateur visuelle  
Lors de la conception de l’interface utilisateur visuelle, assurez-vous que votre produit dispose d’un paramètre de contraste élevé, utilise les polices système et les options de lissage par défaut et se met correctement à l’échelle par rapport aux paramètres PPP (points par pouce) de l’écran. Veillez également à ce que le coefficient de contraste du texte par défaut soit d’au moins 5:1 par rapport à l’arrière-plan et à ce que les combinaisons de couleurs soient faciles à distinguer pour les utilisateurs daltoniens.  

#### <a name="high-contrast-setting"></a>Paramètre de contraste élevé  
L’une des fonctionnalités d’accessibilité intégrées dans Windows est le mode Contraste élevé, qui améliore le contraste de couleur du texte et des images. Pour certaines personnes, l’augmentation du contraste des couleurs permet de réduire la fatigue visuelle et de faciliter la lecture. Lorsque vous vérifiez votre interface utilisateur en mode de contraste élevé, vous souhaitez vous assurer que les contrôles, tels que les liens, ont été codés de manière cohérente et avec les couleurs système (pas à l’aide de couleurs codées en dur) pour vous assurer qu’ils seront en mesure de voir tous les contrôles de l’écran qu’un utilisateur n’utilisant pas le contraste élevé pourrait voir.  

#### <a name="system-font-settings"></a>Paramètres de police système  
Pour garantir la lisibilité et éviter toute déformation inattendue du texte, assurez-vous de la conformité de votre produit par rapport à la police système par défaut et veillez à ce qu’il active les options d’anticrénelage et de lissage. Si votre produit utilise des polices personnalisées, les utilisateurs peuvent rencontrer des problèmes de lisibilité lorsqu’ils personnalisent la présentation de leur interface utilisateur (via des lecteurs d’écran ou en utilisant des styles de police différents pour l’afficher, par exemple).  

#### <a name="high-dpi-resolutions"></a>Résolutions PPP élevées  
Pour les utilisateurs malvoyants, il est important d’avoir une interface utilisateur évolutive. Les interfaces utilisateur qui ne se mettent pas correctement à l’échelle lorsque les résolutions PPP (point par pouce) sont élevées peuvent provoquer le chevauchement de certains composants importants, voire masquer et rendre inaccessibles d’autres composants.  

#### <a name="color-contrast-ratio"></a>Coefficient de contraste des couleurs  
La section 508 mise à jour de l’Americans with Disability Act (ADA), ainsi que d’autres législations, exigent que le contraste de couleur par défaut entre le texte et son arrière-plan soit de 5:1. Pour le texte de grande taille (tailles de police de 18 points, ou 14 points et en gras), le contraste requis par défaut est de 3:1.  

#### <a name="color-combinations"></a>Combinaisons de couleurs  
Environ 7 % des hommes (et moins de 1 % des femmes) souffrent de problèmes de perception des couleurs. Les utilisateurs daltoniens ont des difficultés à distinguer certaines couleurs, il est donc important de ne pas se servir uniquement de la couleur pour communiquer un état ou une idée. Comme pour les images décoratives (par exemple, les icônes ou les arrière-plans), les combinaisons de couleurs doivent être choisies de manière à optimiser la perception de l’image par les utilisateurs daltoniens. Si vous vous appuyez sur ces recommandations en matière de couleurs dès le début de la conception de votre application, cette dernière sera déjà inclusive en partie.  

## <a name="summary-mdash-seven-steps-for-inclusive-design"></a>Récapitulatif : &mdash;sept étapes pour la conception inclusive  
En résumé, suivez ces sept étapes pour vous assurer que votre logiciel est inclusif.  
1.  Décidez si la conception inclusive revêt une importance quelconque pour votre logiciel. Si c’est le cas, découvrez comment celle-ci permet aux utilisateurs réels de vivre, de travailler et de jouer afin de vous guider dans votre conception.  
2.  Lorsque vous concevez des solutions à vos besoins, utilisez les contrôles fournis par votre infrastructure (contrôles standard) autant que possible ; évitez les efforts et les coûts inutiles liés aux contrôles personnalisés.  
3.  Concevez une hiérarchie logique pour votre produit, en notant où se trouvent les contrôles standard, les contrôles personnalisés et le focus clavier dans l’interface utilisateur.  
4.  Concevez des paramètres système utiles (par exemple, la navigation au clavier, le contraste élevé et la haute résolution) pour votre produit.  
5.  Implémentez votre conception en utilisant le [Hub de développeurs axés sur l’accessibilité Microsoft](https://developer.microsoft.com/windows/accessible-apps) et la spécification d’accessibilité de votre infrastructure comme point de référence.  
6.  Testez votre produit avec des utilisateurs ayant des besoins spécifiques pour vous assurer qu’ils pourront tirer parti des techniques de conception inclusive que vous avez implémentées.  
7.  Livrez votre produit fini et documentez votre implémentation pour ceux qui travailleront sur le projet ultérieurement.  

## <a name="related-topics"></a>Rubriques connexes  
* [Conception inclusive](https://design.microsoft.com/inclusive)
* [Logiciel d’ingénierie pour l’accessibilité](https://www.microsoft.com/download/details.aspx?id=19262)
* [Hub de développeur d’accessibilité de Microsoft](https://developer.microsoft.com/windows/accessible-apps)
* [Développement d’applications Windows inclusives](developing-inclusive-windows-apps.md) 
* [Accessibilité](accessibility.md)
