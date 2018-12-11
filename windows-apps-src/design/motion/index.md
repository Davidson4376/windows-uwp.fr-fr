---
Description: Purposeful, well-designed motion brings your app to life and makes the experience feel crafted and polished. Help users understand context changes, and tie experiences together with visual transitions.
title: Mouvements et animations dans les applications UWP
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2701844ccefdc5a535fa8fc20086c550cb7bc29e
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8888737"
---
# <a name="motion-for-uwp-apps"></a>Mouvements et animations pour les applications UWP

![image hero](images/header-motion2.svg)

Le mouvement Fluent a une fonction spécifique dans votre application. Il fournit des commentaires intelligents basés sur le comportement de l’utilisateur, garde l’interface utilisateur active et guide la navigation de l’utilisateur dans votre application. Le mouvement Fluent suscite un lien émotionnel entre un utilisateur et son expérience numérique. Nous nous appuyons sur les principes du mouvement naturel que l’utilisateur connaît déjà dans le monde physique et nous avons étendu notre système à partir de là.

## <a name="fluent-motion-principles"></a>Principes du mouvement Fluent

### <a name="physical"></a>Physique

Les objets en mouvement présentent les comportements des objets du monde réel. Un mouvement fluide et réactif fait paraître l'expérience naturelle, crée des liens émotionnels et donne de la personnalité.

![Exemple d’interface utilisateur de mouvement physique](images/Physical.gif)
> Lorsque vous interagissez avec l’interface utilisateur via l’interaction tactile, le mouvement de l’interface utilisateur est directement lié à la vitesse de l’interaction. Et parce que l'interaction tactile est une manipulation directe, l’objet avec lequel vous interagissez a une incidence sur les objets qui l’entourent.

### <a name="functional"></a>Fonctionnel

Le mouvement joue un rôle et est incitatif. Il guide l’utilisateur à travers la complexité et aide à établir une hiérarchie. Le mouvement donne l’impression de performances accrues et permet d’optimiser l’expérience utilisateur en masquant la perception de la latence.

![Exemple d’interface utilisateur de mouvement fonctionnel](images/functional.gif)
> Les transitions de page sont spécialement conçues. Elles donnent des indications sur la façon dont les pages sont liées. Leur déplacement est perçu comme rapide même lorsque les performances ne sont pas optimales.

### <a name="continuous"></a>Continu

Un mouvement fluide d'un point à un autre attire naturellement l'œil et guide l’utilisateur. Il fait la liaison de manière élégante avec une tâche de l'utilisateur et la fait paraître plus consommable et conviviale.

![Exemple d’interface utilisateur de mouvement en continu](images/continuous3.gif)
> Les objets peuvent voyager d’une scène à l’autre ou se transformer dans une scène pour établir une continuité et aider l’utilisateur à préserver le contexte.

### <a name="contextual"></a>Contextuel

Un mouvement intelligent fournit des commentaires à l’utilisateur en relation avec la façon dont il a manipulé l’interface utilisateur. L'interaction est centrée sur l’utilisateur. Le mouvement semble appropriée au facteur de forme et conçu en fonction du scénario. Il doit être confortable pour chaque utilisateur.

![Exemple d’interface utilisateur de mouvement contextuel](images/Contextual.gif)
> L'animation doit être relié à l’interaction utilisateur. Un menu contextuel est déployé à partir du point où l’utilisateur l'a activé. 

## <a name="motion-articles"></a>Articles sur le mouvement

:::row:::
    :::column:::
        ### [Timing and easing](timing-and-easing.md)
        Timing and easing are important elements that make motion feel natural for objects entering, exiting, or moving within the UI.
    :::column-end:::
    :::column:::
        ### [Directionality and gravity](directionality-and-gravity.md)
        Directional signals help provide a solid mental model of the journey a user takes across experiences. Directional movement is subject to forces like gravity, which reinforces the natural feel of the movement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ### [Page transitions](page-transitions.md)
        Page transitions navigate users between pages in an app, providing feedback about the relationship between pages. They help users understand where they are in the navigation hierarchy.
    :::column-end:::
    :::column:::
        ### [Connected animation](connected-animation.md)
        Connected animations let you create a dynamic and compelling navigation experience by animating the transition of an element between two different views.
    :::column-end:::
:::row-end:::
