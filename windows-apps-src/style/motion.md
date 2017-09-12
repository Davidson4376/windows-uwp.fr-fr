---
author: mijacobs
Description: "Avec des mouvements utiles et bien faits, vos applications prennent vie et donnent l’impression d’un travail soigné. Elles permettent aux utilisateurs de comprendre les changements de contexte et assure l’homogénéité des expériences par des transitions visuelles."
title: Animations dans les applications UWP
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.openlocfilehash: d24edf5eaacb65ca28f2f2727f441cb833141dcf
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2017
---
# <a name="motion-for-uwp-apps"></a>Animations pour les applications UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Avec des mouvements utiles et bien faits, vos applications prennent vie et donnent l’impression d’un travail soigné. Le mouvement permet à vos utilisateurs de comprendre les changements de contexte et où ils se trouvent dans la hiérarchie de navigation de votre application. Il relie les expériences entre elles, grâce à des transitions visuelles. Le mouvement ajoute une dynamique et une dimension à l’expérience.

## <a name="benefits-of-motion"></a>Avantages du mouvement

Un mouvement ne consiste pas simplement à faire bouger des objets. Le mouvement est un outil de création d’un écosystème physique, dans lequel l’utilisateur peut évoluer et agir via une variété de types d’entrée, comme la souris, le clavier, les fonctions tactiles et le stylet. La qualité de l’expérience dépend de la façon dont l’application répond à l’utilisateur et du type de personnalité communiqué par l’interface utilisateur.

Assurez-vous que le mouvement a une fonction spécifique dans votre application. Les meilleures applications de plateforme Windows universelle (UWP) utilisent le mouvement pour donner vie à l’interface utilisateur. Le mouvement doit:

- fournir un retour d’informations basé sur le comportement de l’utilisateur;
- enseigner à l’utilisateur comment interagir avec l’interface utilisateur;
- indiquer comment accéder aux vues précédentes ou suivantes.

Plus un utilisateur passe de temps dans votre application (ou plus les tâches deviennent sophistiquées), plus la qualité du mouvement devient importante: elle permet de modifier la façon dont l’utilisateur perçoit la charge cognitive et la simplicité d’utilisation de votre application. Un mouvement présente de nombreux autres avantages directs:

- **Le mouvement prend en charge l’interaction et l'orientation.**

    Le mouvement est directionnel: il permet de se déplacer vers l’avant ou en arrière, dans ou hors du contenu, laissant une trace mentale quant à la façon dont l’utilisateur est parvenu jusqu’à la vue active. Les transitions peuvent aider les utilisateurs à apprendre comment utiliser de nouvelles applications, par analogie avec des tâches qu’ils connaissent déjà.

- **Le mouvement peut donner l’impression de performances accrues.**

    Quand le réseau est lent ou qu’une interruption résulte du travail du système, des animations peuvent donner à l’utilisateur l’impression que l’attente est plus courte. Les animations peuvent permettre d’indiquer à l’utilisateur que son application n’est pas figée et qu’elle traite actuellement une opération, et elles peuvent exposer de manière passive de nouvelles informations susceptibles d’intéresser l’utilisateur.

- **Le mouvement ajoute de la personnalité.**

    Le mouvement est souvent le fil commun qui véhicule la personnalité de vos applications tandis que l'utilisateur parcourt une expérience.

- **Le mouvement ajoute de l’élégance.**

    Un mouvement fluide et réactif rend les expériences naturelles, et crée des liens émotionnels avec l'expérience.

## <a name="examples-of-motion"></a>Exemples de mouvement

Voici quelques exemples de mouvement dans une application.

Ici, une application utilise une animation connectée pour animer une image qui «perdure» pour faire partie de l’en-tête de la page suivante. L’effet aide à conserver le contexte de l’utilisateur pendant toute la transition.

![Animation connectée](images/connected-animations/example.gif)

Ici, un effet parallaxe visuel déplace les objets à différentes vitesses lorsque vous faites défiler ou que vous développez l'interface utilisateur, afin de créer un effet de profondeur, de perspective et de mouvement.

![Exemple de parallaxe avec une liste et une image d’arrière-plan](images/_Parallax_v2.gif)


## <a name="types-of-motion"></a>Types de mouvement

<table>
    <tr>
        <th align="left">Type de mouvement</th>
        <th align="left">Description</th>
    </tr>
    <tr>
        <td>[Ajouter et supprimer](motion-list.md)
        </td>
        <td>Les animations de liste vous permettent d’insérer ou de supprimer un ou plusieurs éléments dans une collection telle qu’un album photo ou une liste de résultats de recherche.
        </td>
    </tr>
    <tr>
        <td>[Animation connectée](connected-animation.md)
        </td>
        <td>Les animations connectées vous permettent de créer une expérience de navigation dynamique et attrayante en animant la transition d’un élément entre deux affichages. Cela permet à l’utilisateur de conserver son contexte et d'assurer la continuité entre les vues. Dans une animation connectée, un élément «perdure» entre deux affichages lors d'une modification du contenu de l’interface utilisateur, et survole l'écran depuis son emplacement dans l'affichage source à sa destination dans le nouvel affichage. Cela met en évidence le contenu commun des vues et crée un effet dynamique très esthétique dans le cadre d’une transition. 
        </td>
    </tr>
    <tr>
        <td>[Transition de contenu](content-transition-animations.md)
        </td>
        <td>Les animations de transition de contenu vous permettent de modifier le contenu d’une zone de l’écran tout en maintenant le conteneur ou l’arrière-plan constant. Le nouveau contenu apparaît. Si du contenu déjà à l’écran doit être remplacé, il disparaît.
        </td>
    </tr>
    <tr>
        <td>[Exploration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinthemeanimation)
        </td>
        <td>Utilisez une animation d'exploration lorsqu’un utilisateur navigue vers l’avant dans une hiérarchie logique, par exemple, d’une liste maître vers une page de détails. Utilisez une animation d'éloignement lorsqu’un utilisateur navigue vers l’arrière dans une hiérarchie logique, par exemple, d'une page de détails vers une page maître.
        </td>
    </tr>
    <tr>
        <td>[Fondu](motion-fade.md)
        </td>
        <td>Utilisez les animations en fondu pour faire apparaître ou disparaître des éléments. Les deux animations en fondu les plus courantes sont l’apparition en fondu et la disparition en fondu.
        </td>
    </tr>
        <tr>
        <td>[Parallaxe](parallax.md)
        </td>
        <td>Un effet parallaxe visuel permet de créer un effet de profondeur, de perspective et de mouvement. Cet effet est obtenu en déplaçant les objets à différentes vitesses lorsque vous faites défiler ou que vous développez l'interface utilisateur.
        </td>
    </tr> 
    <tr>
        <td>[Retour par pression](motion-pointer.md)
        </td>
        <td>Les animations par pression fournissent un retour visuel lorsqu’un utilisateur appuie sur un élément. L’animation de pointeur vers le bas réduit et incline légèrement l’élément sélectionné. Elle est lue lorsque l’utilisateur appuie pour la première fois sur un élément. L’animation de pointeur vers le haut, qui restaure l’élément à sa position d’origine, est lue quand l’utilisateur relâche le pointeur.
        </td>
    </tr>
</table>