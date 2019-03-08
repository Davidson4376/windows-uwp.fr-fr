---
Description: Découvrez les techniques de conception réactive pour adapter votre application pour des appareils spécifiques
label: Responsive design techniques
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9e06131da5d7dd56354e1871867f9956ad13aa2c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624814"
---
# <a name="responsive-design-techniques"></a>Techniques de conception réactive

Les applications UWP utilisent des pixels efficaces pour garantir que votre interface utilisateur sera lisible et utilisable sur tous les périphériques fonctionnant sous Windows. Dès lors, pourquoi souhaiteriez-vous personnaliser l’interface utilisateur de votre application pour une famille d’appareils spécifique ?

- **Pour rendre l’utilisation plus efficace de l’espace et de réduire le besoin d’accéder**

    Si vous concevez une application à une apparence agréable sur un appareil qui a un petit écran, par exemple un Tablet PC, l’application sera utilisable sur un PC avec un affichage de plus grand quantité, mais il y aura probablement un espace gaspillé. Vous pouvez personnaliser l’application pour afficher davantage de contenu lorsque la taille de l’écran est supérieure à une certaine valeur. Par exemple, une application d’achat peut afficher une catégorie de marchandises à la fois sur une tablette, mais afficher plusieurs catégories et produits simultanément sur un PC ou un ordinateur portable.

    En plaçant plus de contenu à l’écran, vous réduisez les étapes de navigation que l’utilisateur doit effectuer.

- **Pour tirer parti des capacités des appareils**

    Certains appareils sont plus susceptibles d’être dotés de fonctionnalités particulières. Par exemple, les ordinateurs portables sont susceptibles de bénéficier d’un capteur d’emplacement et un appareil photo, tout un téléviseur ne soit peut-être pas. Votre application peut détecter les fonctionnalités qui sont disponibles et les activer.

- **Pour optimiser pour l’entrée**

    La bibliothèque de contrôles universels fonctionne avec tous les types d’entrée (tactile, stylet, clavier, souris), mais vous pouvez toujours optimiser certains types d’entrée en réorganisant vos éléments d’interface utilisateur. Par exemple, si vous placez des éléments de navigation en bas de l’écran, ils seront plus facilement accessibles aux utilisateurs de téléphone, mais la plupart des utilisateurs de PC s’attendent à voir des éléments de navigation en haut de l’écran.

Lorsque vous optimisez l’interface utilisateur de votre application pour des largeurs d’écran spécifiques, nous disons que vous créez une conception réactive. Voici six techniques de conception réactive que vous pouvez utiliser pour personnaliser l’interface utilisateur de votre application.

>[!TIP]
> De nombreux contrôles UWP implémentent automatiquement ces comportements réactives. Pour créer une interface utilisateur réactive, nous vous recommande de lire le [les contrôles UWP](../controls-and-patterns/index.md).

## <a name="reposition"></a>Repositionner

Vous pouvez modifier l’emplacement et la position des éléments d’interface à tirer le meilleur parti de la taille de fenêtre. Dans cet exemple, la fenêtre plus petite empile les éléments verticalement. Lorsque l’application se traduit par une plus grande fenêtre, les éléments peuvent tirer parti de la largeur de fenêtre plus large.

![Repositionner](images/rsp-design/rspd-reposition2.gif)

Dans cet exemple de conception pour une application de photos, l’application repositionne son contenu sur des écrans plus grands.

## <a name="resize"></a>Redimensionner

Vous pouvez optimiser la taille de fenêtre en ajustant les marges et la taille des éléments d’interface utilisateur. Par exemple, cela peut augmenter l’expérience de lecture sur un écran plus grand par croissante simplement le frame de contenu.

![Redimensionnement des éléments de conception](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Ajuster dynamiquement

En modifiant le flux des éléments de l’interface utilisateur en fonction de l’appareil et de l’orientation, votre application peut offrir un affichage de contenu optimal. Par exemple, lorsque vous passez à un écran plus grand, il peut être judicieux d’ajouter des colonnes, utiliser des conteneurs plus volumineux ou générer des éléments de liste d’une manière différente.

Cet exemple montre comment une seule colonne de contenu sur un écran plus petit qui peut être réorganisé sur un écran plus grand pour afficher les deux colonnes de texte à défilement vertical.

![Ajustement dynamique des éléments de conception](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Masquer/Afficher

Vous pouvez afficher ou masquer des éléments d’interface utilisateur en fonction de l’espace disponible à l’écran, ou lorsque l’appareil prend en charge des fonctionnalités supplémentaires, des situations spécifiques ou des orientations d’écran favorites.

![Masquage des éléments de conception](images/rsp-design/rspd-revealhide.gif)

Par exemple, les contrôles du lecteur multimédia réduisent l’ensemble de boutons sur les petits écrans et développent sur les écrans plus grands. Le lecteur de média dans une fenêtre plus grande peut gérer beaucoup plus à l’écran de fonctionnalités qu’il peuvent dans une fenêtre plus petite.

Une partie de la technique de révélation ou de masquage comprend le choix de l’affichage des métadonnées. Avec fenêtres plus petites, il est préférable d’afficher une quantité minimale de métadonnées. Avec windows plus volumineux, une quantité importante de métadonnées peut être exposée. Voici quelques exemples du moment afficher ou masquer des métadonnées :

- Dans une application de messagerie, vous pouvez afficher l’avatar de l’utilisateur.
- Dans une application de musique, vous pouvez afficher davantage d’informations sur un album ou un artiste.
- Dans une application vidéo, vous pouvez afficher plus d’informations sur un film ou une émission, en présentant par exemple des détails sur la distribution et l’équipe technique.
- Dans n’importe quelle application, vous pouvez décomposer les colonnes et révéler plus de détails.
- Dans n’importe quelle application, vous pouvez disposer un élément à l’horizontal alors qu’il était empilé verticalement. Lorsque vous passez d’un téléphone ou d’une phablette à un appareil plus grand, les éléments de liste empilés peuvent changer pour faire apparaître des lignes d’éléments de liste et des colonnes de métadonnées.

## <a name="replace"></a>Remplacer

Cette technique vous permet de basculer l’interface utilisateur pour un points d’arrêt spécifique. Dans cet exemple, le volet de navigation et son compact, l’interface utilisateur temporaire fonctionne bien pour un écran plus petit, mais sur un écran plus grand, onglets peuvent être un meilleur choix.

![Remplacement des éléments de conception](images/rsp-design/rspd-replace.gif)

Le [NavigationView](../controls-and-patterns/navigationview.md) contrôle prend en charge cette technique réactive, en permettant aux utilisateurs de la position de la valeur est supérieure ou gauche.

## <a name="re-architect"></a>Remodéliser

Vous pouvez réduire ou répliquer l’architecture de votre application pour mieux cibler des appareils spécifiques. Dans cet exemple, la fenêtre en développant le modèle maître/détails entier.

![exemple de réorganisation d’une interface utilisateur](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Rubriques connexes

- [Tailles d’écran et points d’arrêt](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Dispositions réactives avec XAML](layouts-with-xaml.md)
- [Les contrôles UWP et des modèles](../controls-and-patterns/index.md)
