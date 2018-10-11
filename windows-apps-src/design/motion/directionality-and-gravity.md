---
author: jwmsft
Description: Learn how Fluent motion uses directionality and gravity.
title: Direction et gravité - animation dans les applications UWP
label: Directionality and gravity
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: b61abf00d5ab8820457742f16feb9b496b7d7d1c
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532840"
---
# <a name="directionality-and-gravity"></a>Direction et gravité

Les signaux directionnels permettent de renforcer le modèle mental du parcours effectué par un utilisateur entre les différentes expériences. Il est important que la direction d'un mouvement prenne en charge à la fois la continuité de l’espace et l’intégrité des objets dans l’espace.

Le mouvement directionnel est soumis à des forces telles que la gravité. L'application de forces au mouvement renforce la sensation de naturel du mouvement.

## <a name="direction-of-movement"></a>Direction du mouvement

:::row:::
    :::column:::
        Direction of movement corresponds to physical motion. Just like in nature, objects can move in any world axis - X,Y,Z. This is how we think of the movement of objects on the screen.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Direction de navigation

La direction de navigation entre les scènes de votre application est conceptuelle. Les utilisateurs naviguent vers l’avant et vers l'arrière. Les scènes se déplacent dans et en dehors de la vue. Ces concepts se combinent avec les mouvements physiques pour guider l’utilisateur.

Lorsque la navigation fait passer un objet de la scène précédente à la nouvelle scène, l’objet effectue un simple mouvement de A vers B sur l’écran. Pour faire paraître le mouvement plus physique, l’accélération standard est ajoutée, ainsi que la sensation de gravité.

Pour la navigation vers l’arrière, le déplacement est inversé (B vers A). Lorsque l’utilisateur navigue vers l'arrière, il s'attend à retourner à l’état précédent dès que possible. Le rythme est plus rapide, plus direct et utilise la fonctionnalité de décélération.

Ici, ces principes sont appliqués lorsque l’élément sélectionné reste à l’écran pendant la navigation vers l'avant et vers l'arrière.

![Exemple d’interface utilisateur de mouvement en continu](images/continuous3.gif)

Lorsque la navigation entraîne le remplacement des éléments à l’écran, il est important de montrer où la scène existante est passée et d'où vient la nouvelle scène.

Cela présente plusieurs avantages:

- Cela renforce le modèle mental de l’utilisateur de l’espace.
- La durée de la scène existante donne plus de temps pour préparer le contenu à animer dans la scène à venir.
- Cela améliore la perception que l’utilisateur a des performances de l'application.

4directions discrètes de navigation sont à prendre en compte.

:::row:::
    :::column:::
        **Forward-In**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Forward-Out**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-In**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-Out**

        Same as Forward-Out, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Gravité

La gravité fait paraître vos expériences plus naturelles. Les objets qui se déplacent sur l'axe Z et qui ne sont pas ancrés à la scène par une affordance à l’écran sont susceptibles d’être affectés par la gravité. Quand un objet s'échappe de la scène et avant qu’il atteigne sa vitesse d’échappement, la gravité attire l’objet vers le bas, ce qui donne une courbe plus naturelle à la trajectoire de déplacement de l'objet.

La gravité se manifeste généralement lorsqu’un objet doit passer d’une scène à une autre. Pour cette raison, l’animation connectée utilise le concept de gravité.

Ici, un élément dans la rangée supérieure de la grille est affecté par la gravité, ce qui le fait tomber légèrement lorsqu'il quitte son emplacement et se déplace vers l’avant.

![Direction vers l’arrière dans](images/continuity-photos.gif)

## <a name="related-articles"></a>Articles associés

- [Présentation du mouvement](index.md)
- [Minutage et accélération](timing-and-easing.md)