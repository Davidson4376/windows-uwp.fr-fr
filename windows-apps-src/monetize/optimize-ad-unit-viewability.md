---
author: Xansky
description: Découvrez comment améliorer la visibilité de vos unités publicitaires.
title: Optimiser la visibilité de vos unités publicitaires
ms.author: mhopkins
ms.date: 05/07/2018
ms.topic: article
keywords: windows10, uwp, annonce, publicité, directives, visibilité
ms.localizationpriority: medium
ms.openlocfilehash: ef815dab027f86e5d73f24ae0355f7f41612fae5
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7558846"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>Optimiser la visibilité de vos unités publicitaires

Le [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md) inclut des mesures de visibilité de vos unités publicitaires. La visibilité est un critère important, car le secteur de la publicité tend de plus en plus à privilégier les expositions visibles plutôt que simplement servies. Les annonceurs ont tendance à soumissionner pour des expositions visibles, car ils ont plus de chances que leurs publicités soient vues par les utilisateurs.  

Conformément aux directives de l’Interactive Advertising Bureau (IAB) en matière de visibilité, l'exposition d'une bannière publicitaire est considérée comme visible si elle répond aux critères suivants:

* Exigence de pixels: un pourcentage supérieur ou égal à 50% des pixels de la publicité se trouve sur l’espace visible de l’application.
* Exigence de durée: la durée pendant laquelle l'exigence de pixels est remplie doit être supérieure ou égale à une seconde en continu, à partir du rendu de l'annonce.

Une exposition publicitaire vidéo est considérée comme visible si elle répond aux critères suivants:

* Exigence de pixels: un pourcentage supérieur ou égal à 50% des pixels de la publicité se trouve sur la partie visible de l’application.
* Exigence de durée: la vidéo a répondu à l'exigence de pixels et a été lue pendant deux secondes en continu, à partir du rendu de l'annonce.

La visibilité est calculée à l’aide de la formule suivante:

**Visibilité = [Expositions consultées] * 100 / [Total des expositions publicitaires]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Recommandations pour améliorer la visibilité d'une unité publicitaire

Pour optimiser les expositions visibles de vos unités publicitaires, suivez ces conseils.

1. **Mesurez les performances**&nbsp;&nbsp;Utilisez le [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md) pour passer en revue les mesures de visibilité de chacune de vos unités publicitaires.
2.  **Positionnez votre contrôle de publicité de manière appropriée**&nbsp;&nbsp;En général, la position la plus visible pour une publicité se trouve juste au-dessus de la pliure (autrement dit, en bas de la partie visible de la page que les utilisateurs peuvent voir sans avoir à utiliser le défilement). Les publicités qui s'affichent en haut de la page ne sont pas autant visibles. Envisagez d’examiner chaque élément de votre page pour voir comment optimiser l’espace visible dans votre application. Assurez-vous que vos contrôles de publicité ne sont pas placés dans l’arrière-plan de l’application.
3.  **Gérez la longueur de la page**&nbsp;&nbsp;Une page à contenu court produit une meilleure visibilité. Implémentez le défilement infini si vos pages d’application doivent être plus longues.
4.  **Fixez la position de l'annonce**&nbsp;&nbsp;Assurez-vous que vos annonces restent à une position fixe, même lorsque l’utilisateur utilise le défilement. Vous obtiendrez ainsi un temps d’affichage supérieur et augmenterez votre taux de visibilité.
5.  **Définissez l’opacité**&nbsp;&nbsp;Veillez à ce que le conteneur qui contient le contrôle de publicité soit d'une opacité de 100%.
6.  **Implémentez le chargement différé**&nbsp;&nbsp;Ne chargez pas vos annonces avant que les utilisateurs accèdent à la vue contenant le contrôle de publicité. Vous aurez ainsi la garantie que la publicité s’affichera plus longtemps pour que l’utilisateur la voie.
7.  **Testez différentes tailles de publicité**&nbsp;&nbsp;Sélectionnez plusieurs tailles d'annonce et mesurez la visibilité de chacune d’elles à l’aide du [Rapport sur les performances publicitaires](../publish/advertising-performance-report.md). Sélectionnez celles qui vous convient le mieux.
8.  **Revoyez la conception de l'application**&nbsp;&nbsp;Améliorez la conception de votre page d'application afin de réduire son temps de chargement. Les utilisateurs ont tendance à rester plus longtemps sur des pages qui se chargent rapidement.
