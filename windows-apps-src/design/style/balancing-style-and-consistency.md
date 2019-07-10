---
description: Conseils pour la création d’applications cohérentes et conviviales, qui dénotent également une certaine originalité et créativité.
title: Trouver un juste équilibre entre le style et la cohérence (conception des applications UWP)
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb511fad1aa4e1605d83090a5e4e8d98efff1be
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63790653"
---
# <a name="balancing-style-and-consistency"></a>Trouver un juste équilibre entre le style et la cohérence

 

> Remarque: Cet article est une version préliminaire pour Windows 10 RS2. Les noms des fonctionnalités, la terminologie et les fonctionnalités sont sujets à des modifications.

Quand vous concevez un produit, vous êtes le représentant du client. Nous souhaitons tous concevoir un modèle parfait, qui corresponde parfaitement à ce que nous avions imaginé. Cet article se penche sur l’équilibre entre les conventions suivantes, pour créer à la fois une expérience utilisateur cohérente et des fonctionnalités et expériences uniques, qui différencieront votre application. 

 
## <a name="the-importance-of-consistency"></a>L’importance de la cohérence
Pourquoi la cohérence est-elle importante ? La cohérence peut rendre une application plus facile à utiliser. L’un des aspects essentiels d’une bonne conception est la simplicité d’apprentissage ; une conception cohérente entre les applications réduit le nombre de fois où l’utilisateur final doit « réapprendre » à les utiliser. Pensez à des exemples de la vie de tous les jours : une pédale d’accélération dans une voiture se trouve toujours à droite, un bouton de porte se tourne toujours dans le sens contraire des aiguilles d’une montre pour s’ouvrir, un signe Stop est toujours rouge. 

Toutefois, assurer la cohérence de votre application ne signifie pas que toutes les applications doivent se présenter et fonctionner de la même manière. Pour revenir aux exemples du quotidien, il existe un nombre quasiment illimité de modèles de chaise, mais très peu nécessitent d’apprendre une nouvelle manière de s’asseoir. En effet, cela s’explique par le fait que les éléments importants, comme la surface où l’on peut s’asseoir, sont assez cohérents pour que l’utilisateur puisse les identifier. 

L’une des difficultés auxquelles nous sommes confrontés quand nous créons une application de qualité consiste à comprendre dans quels domaines il est important de rester cohérents, et dans quels domaines il faut faire preuve d’originalité. 

## <a name="the-consistency-spectrum"></a>L’éventail des cohérences
 Il convient d’envisager la cohérence comme un éventail, doté de deux extrémités opposées :


![L’éventail des cohérences](images/consistency/consistency-spectrum.png)

Les éléments familiers d’une expérience utilisateur incluent :
-   Les modèles d’interface utilisateur établis (le comportement correspondant à un clic de la souris, à un appui prolongé sur un élément, la présentation similaire des icônes)
-   Les éléments de votre marque que vous souhaitez appliquer sur l’ensemble de vos produits (typographie, couleurs)

Les facteurs de différenciation sont les suivants :
-   Les éléments qui forment « l’âme » unique des produits
-   Les éléments qui vous aident à adapter l’expérience utilisateur au facteur de forme prévu

Nous allons créer un modèle de conception en utilisant cet éventail et en l’appliquant aux éléments principaux d’une application. 

![Le modèle de cohérence du modèle](images/consistency/design-consistency-model.png)

Dans ce modèle, les couches inférieures fournissent une base éprouvée en termes de cohérence, tandis que les couches supérieures sont axées sur la flexibilité et la personnalisation.  

1. Les notions de base de l’application constituent la première couche de notre modèle : la grille de disposition, la [palette de couleurs](color.md), les [conventions typographiques](typography.md) et les [styles des icônes](icons.md). Ces fonctionnalités de base doivent être appliquées de manière cohérente. 

2. Concernant la seconde couche, UWP fournit un ensemble de [contrôles communs](../controls-and-patterns/index.md) qui permettent de trouver un juste équilibre entre efficacité et flexibilité. Nous avons également élaboré des directives, afin de vous aider à établir un style et un ton cohérents, en reprenant les termes qui décrivent et guident les utilisateurs à travers les expériences utilisateur des applications. Nous avons créé un ensemble de modèles qui peuvent être utilisés lors de la conception des applications, afin de s’assurer que les modèles peuvent s’adapter aux différentes tailles et saisies des appareils. 
3. La troisième couche permet de personnaliser la navigation aux différents appareils et contextes. Par exemple, votre navigation avec une interaction tactile sur un téléphone sera probablement différente de celle sur un moniteur 32 pouces avec clavier et souris ou de celle sur un HoloLens avec mouvements et 100 points tactiles sur une surface de 84 pouces.
4. La quatrième couche vous permet de définir la personnalité de votre marque. Quels sont les éléments caractéristiques de votre marque qui la renforcent et la différencient de la concurrence ? Cette couche vous permet également d’adapter votre application aux différents utilisateurs finaux. Votre application s’adresse-t-elle à des adeptes de jeux vidéos, à des informaticiens, à des écoliers ou collégiens, ou à des enseignants ? Quels sont les besoins spécifiques pour ces différents clients et est-il possible de rendre notre modèle encore plus efficace en ce sens ? Ne reproduisez pas le même schéma, n’ayez jamais de cesse d’explorer les nouvelles manières de créer de la valeur pour vos différents clients.  


## <a name="design-principles"></a>Principes de conception
Pour utiliser efficacement ce modèle, nous avons besoin d’un ensemble de principes de conception qui nous aidera à faire les bons choix. Nos principes de cohérence au niveau de la conception sont les suivants :

**Si la présentation est la même, le comportement doit être le même**
-   Quand l’utilisateur voit une zone de texte ou la commande de type Hamburger, il est probable qu’il s’attende à ce que cet élément fonctionne de la même manière que sur d’autres appareils. Si vous avez une bonne raison de modifier un comportement établi, définissez les attentes des utilisateurs en modifiant également la présentation.

**Si un élément est très similaire à un élément existant ou à une convention, envisagez de l’imiter**
-   Vous avez besoin d’une icône « nouveau document ». Pourquoi en créer une qui sera simplement légèrement différente, alors que vous pouvez en utiliser une qui sera reconnue par l’utilisateur.

**La facilité d’utilisation l’emporte sur la cohérence**
-   Il est préférable qu’un élément soit simple à utiliser plutôt que cohérent. Dans certains cas, vous devrez peut-être développer de nouveaux contrôles ou comportements pour faciliter l’utilisation. Le fait d’utiliser votre téléphone avec une seule main présente des difficultés bien spécifiques. De même que de travailler avec un écran de 80 pouces. Une bonne conception donnera l’impression à l’utilisateur qu’il gère la situation comme un expert. 

**L’interaction est importante**
-   Ne soyez pas ennuyeux. Si tout est plat avec une couleur unie et des carrés, est-ce que cela donnera envie à nos clients de saisir notre produit et de l’utiliser ? Le plaisir avant tout. Introduisez de nouveaux éléments qui surprendront l’utilisateur, sans nuire à la facilité d’apprentissage. 

**Les comportements évoluent**
-   Toute la difficulté est là : à mesure que le secteur évolue, de nouvelles conventions sont établies. Il est possible que les comportements actuels soient abandonnés et nos comportements cohérents peuvent devoir adopter de nouvelles normes. Prenons l’exemple du pincement et du zoom. Auparavant, on attendait d’une interface utilisateur que les fonctions +/- nous permettent d’effectuer un zoom avant ou arrière. Désormais, dans les nouvelles interfaces utilisateur, nous nous attendons à pouvoir pincer et zoomer. Observez les nouveaux modèles d’expérience utilisateur et évoluez en fonction. 
