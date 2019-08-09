---
Description: Découvrez les techniques de conception réactives pour adapter votre application à des appareils spécifiques
title: Techniques de conception réactive
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867415"
---
# <a name="responsive-design-techniques"></a>Techniques de conception réactive

Les applications UWP utilisent des pixels effectifs pour garantir que votre interface utilisateur sera lisible et utilisable sur tous les appareils Windows. Dès lors, pourquoi souhaiteriez-vous personnaliser l’interface utilisateur de votre application pour une famille d’appareils spécifique ?

- **Pour tirer le meilleur parti de l’espace et réduire le besoin de naviguer dans**

    Si vous concevez une application pour qu’elle s’affiche correctement sur un appareil qui a un petit écran, tel qu’une tablette, l’application sera utilisable sur un PC avec un affichage bien plus grand, mais il y aura probablement un gaspillage d’espace. Vous pouvez personnaliser l’application pour afficher davantage de contenu lorsque la taille de l’écran est supérieure à une certaine valeur. Par exemple, une application d’achat peut afficher une catégorie de marchandises à la fois sur une tablette, mais afficher plusieurs catégories et produits simultanément sur un PC ou un ordinateur portable.

    En plaçant plus de contenu à l’écran, vous réduisez les étapes de navigation que l’utilisateur doit effectuer.

- **Pour tirer parti des fonctionnalités des appareils**

    Certains appareils sont plus susceptibles d’être dotés de fonctionnalités particulières. Par exemple, les ordinateurs portables sont susceptibles d’avoir un capteur d’emplacement et un appareil photo, alors qu’un téléviseur n’en a peut-être pas. Votre application peut détecter les fonctionnalités qui sont disponibles et les activer.

- **Pour optimiser l’entrée**

    La bibliothèque de contrôles universels fonctionne avec tous les types d’entrée (tactile, stylet, clavier, souris), mais vous pouvez toujours optimiser certains types d’entrée en réorganisant vos éléments d’interface utilisateur. Par exemple, si vous placez des éléments de navigation en bas de l’écran, ils seront plus facilement accessibles aux utilisateurs de téléphone, mais la plupart des utilisateurs de PC s’attendent à voir des éléments de navigation en haut de l’écran.

Lorsque vous optimisez l’interface utilisateur de votre application pour des largeurs d’écran spécifiques, nous disons que vous créez une conception réactive. Voici six techniques de conception réactive que vous pouvez utiliser pour personnaliser l’interface utilisateur de votre application.

>[!TIP]
> De nombreux contrôles UWP implémentent automatiquement ces comportements réactifs. Pour créer une interface utilisateur réactive, nous vous recommandons d’extraire les [contrôles UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Repositionner

Vous pouvez modifier l’emplacement et la position des éléments de l’interface utilisateur pour tirer le meilleur parti de la taille de la fenêtre. Dans cet exemple, la fenêtre plus petite empile les éléments verticalement. Lorsque l’application se traduit par une fenêtre plus grande, les éléments peuvent tirer parti de la largeur de fenêtre plus large.

![Repositionner](images/rsp-design/rspd-reposition2.gif)

Dans cet exemple de conception pour une application de photos, l’application repositionne son contenu sur des écrans plus grands.

## <a name="resize"></a>Redimensionner

Vous pouvez optimiser la taille de la fenêtre en ajustant les marges et la taille des éléments de l’interface utilisateur. Par exemple, cela peut augmenter l’expérience de lecture sur un écran plus grand en développant simplement le cadre du contenu.

![Redimensionnement des éléments de conception](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Ajuster dynamiquement

En modifiant le flux des éléments de l’interface utilisateur en fonction de l’appareil et de l’orientation, votre application peut offrir un affichage de contenu optimal. Par exemple, lorsque vous accédez à un écran plus grand, il peut être judicieux d’ajouter des colonnes, d’utiliser des conteneurs plus volumineux ou de générer des éléments de liste d’une manière différente.

Cet exemple montre comment une seule colonne de contenu à défilement vertical sur un écran plus petit qui peut être redistribué sur un écran plus grand pour afficher deux colonnes de texte.

![Ajustement dynamique des éléments de conception](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Masquer/Afficher

Vous pouvez afficher ou masquer des éléments d’interface utilisateur en fonction de l’espace disponible à l’écran, ou lorsque l’appareil prend en charge des fonctionnalités supplémentaires, des situations spécifiques ou des orientations d’écran favorites.

![Masquage des éléments de conception](images/rsp-design/rspd-revealhide.gif)

Par exemple, les contrôles du lecteur multimédia réduisent le bouton défini sur des écrans plus petits et s’étendent sur des écrans plus grands. Le lecteur multimédia sur une fenêtre plus grande peut gérer beaucoup plus de fonctionnalités à l’écran qu’il ne le peut sur une petite fenêtre.

Une partie de la technique de révélation ou de masquage comprend le choix de l’affichage des métadonnées. Avec les fenêtres plus petites, il est préférable d’afficher une quantité minime de métadonnées. Avec les fenêtres plus grandes, une quantité importante de métadonnées peut être exposée. Voici quelques exemples d’affichage ou de masquage des métadonnées:

- Dans une application de messagerie, vous pouvez afficher l’avatar de l’utilisateur.
- Dans une application de musique, vous pouvez afficher davantage d’informations sur un album ou un artiste.
- Dans une application vidéo, vous pouvez afficher plus d’informations sur un film ou une émission, en présentant par exemple des détails sur la distribution et l’équipe technique.
- Dans n’importe quelle application, vous pouvez décomposer les colonnes et révéler plus de détails.
- Dans n’importe quelle application, vous pouvez disposer un élément à l’horizontal alors qu’il était empilé verticalement. Lorsque vous passez d’un téléphone ou d’une phablette à un appareil plus grand, les éléments de liste empilés peuvent changer pour faire apparaître des lignes d’éléments de liste et des colonnes de métadonnées.

## <a name="replace"></a>Remplacer

Cette technique vous permet de basculer l’interface utilisateur pour un point d’arrêt spécifique. Dans cet exemple, le volet de navigation et son interface utilisateur temporaire compacte fonctionnent bien pour un écran plus petit, mais sur un écran plus grand, les onglets peuvent être un meilleur choix.

![Remplacement des éléments de conception](images/rsp-design/rspd-replace.gif)

Le contrôle [NavigationView](../controls-and-patterns/navigationview.md) prend en charge cette technique réactive, en permettant aux utilisateurs de définir la position du volet en haut ou à gauche.

## <a name="re-architect"></a>Remodéliser

Vous pouvez réduire ou répliquer l’architecture de votre application pour mieux cibler des appareils spécifiques. Dans cet exemple, le développement de la fenêtre affiche l’intégralité du modèle maître/détails.

![exemple de réorganisation d’une interface utilisateur](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Rubriques connexes

- [Tailles d’écran et points d’arrêt](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Dispositions réactives avec XAML](layouts-with-xaml.md)
- [Modèles et contrôles UWP](../controls-and-patterns/index.md)
