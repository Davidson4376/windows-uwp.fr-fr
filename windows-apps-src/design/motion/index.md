---
Description: Avec des mouvements utiles et bien faits, vos applications prennent vie et donnent l’impression d’un travail soigné. Elles permettent aux utilisateurs de comprendre les changements de contexte et assure l’homogénéité des expériences par des transitions visuelles.
title: Animations dans les applications UWP
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 31cf2134fb8f77809b75a5abf3e6980443452059
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867403"
---
# <a name="motion-for-uwp-apps"></a>Animations pour les applications UWP

![Icône de mouvement](../images/motion-2x.png)

Le mouvement Fluent a une fonction spécifique dans votre application. Il fournit des commentaires intelligents basés sur le comportement de l’utilisateur, garde l’interface utilisateur active et guide la navigation de l’utilisateur dans votre application. Le mouvement Fluent suscite un lien émotionnel entre un utilisateur et son expérience numérique. Nous nous appuyons sur les principes du mouvement naturel que l’utilisateur connaît déjà dans le monde physique et nous avons étendu notre système à partir de là.

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/category/Motion">ouvrir l’application et voir Mouvement en action </a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-motion-principles"></a>Principes du mouvement Fluent

### <a name="physical"></a>Physique

Les objets en mouvement présentent les comportements des objets du monde réel. Un mouvement fluide et réactif fait paraître l’expérience naturelle, crée des liens émotionnels et donne de la personnalité.

![Exemple d’interface utilisateur de mouvement physique](images/Physical.gif)
> Lorsque vous interagissez avec l’interface utilisateur via l’interaction tactile, le mouvement de l’interface utilisateur est directement lié à la vitesse de l’interaction. Et parce que l’interaction tactile est une manipulation directe, l’objet avec lequel vous interagissez a une incidence sur les objets qui l’entourent.

### <a name="functional"></a>Fonctionnel

Le mouvement joue un rôle et est incitatif. Il guide l’utilisateur à travers la complexité et aide à établir une hiérarchie. Le mouvement donne l’impression de performances accrues et permet d’optimiser l’expérience utilisateur en masquant la perception de la latence.

![Exemple d’interface utilisateur de mouvement fonctionnel](images/functional.gif)
> Les transitions de page sont spécialement conçues. Elles donnent des indications sur la façon dont les pages sont liées. Leur déplacement est perçu comme rapide même lorsque les performances ne sont pas optimales.

### <a name="continuous"></a>Continu

Un mouvement fluide d’un point à un autre attire naturellement l’œil et guide l’utilisateur. Il fait la liaison de manière élégante avec une tâche de l’utilisateur et la fait paraître plus consommable et conviviale.

![Exemple d’interface utilisateur de mouvement en continu](images/continuous3.gif)
> Les objets peuvent voyager d’une scène à l’autre ou se transformer dans une scène pour établir une continuité et aider l’utilisateur à préserver le contexte.

### <a name="contextual"></a>Contextuel

Un mouvement intelligent fournit des commentaires à l’utilisateur en relation avec la façon dont il a manipulé l’interface utilisateur. L’interaction est centrée sur l’utilisateur. Le mouvement semble approprié au facteur de forme et conçu en fonction du scénario. Il doit être confortable pour chaque utilisateur.

![Exemple d’interface utilisateur de mouvement contextuel](images/Contextual.gif)
> L’animation doit être reliée à l’interaction utilisateur. Un menu contextuel est déployé là où l’utilisateur l’a activé.

## <a name="motion-articles"></a>Articles sur le mouvement

:::row:::
    :::column:::
### <a name="timing-and-easingtiming-and-easingmd"></a>[Minutage et accélération](timing-and-easing.md)
Le minutage et l’accélération sont des éléments importants pour que le mouvement des objets entrant, sortant ou bougeant dans l’interface utilisateur paraisse naturel.
    :::column-end:::
    :::column:::
### <a name="directionality-and-gravitydirectionality-and-gravitymd"></a>[Direction et gravité](directionality-and-gravity.md)
Les signaux directionnels permettent de fournir un bon modèle mental du parcours que prend un utilisateur à travers les différentes expériences. Le mouvement directionnel est soumis à des forces comme la gravité, ce qui renforce l’impression naturel du mouvement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
### <a name="page-transitionspage-transitionsmd"></a>[Transitions de page](page-transitions.md)
Les transitions de page dirigent les utilisateurs entre les pages d’une application, en fournissant des commentaires sur la relation entre les pages. Elles aident les utilisateurs à comprendre où ils se trouvent dans la hiérarchie de navigation.
    :::column-end:::
    :::column:::
### <a name="connected-animationconnected-animationmd"></a>[Animation connectée](connected-animation.md)
Les animations connectées vous permettent de créer une expérience de navigation dynamique et intéressante en animant la transition d’un élément entre deux vues.
    :::column-end:::
:::row-end:::
