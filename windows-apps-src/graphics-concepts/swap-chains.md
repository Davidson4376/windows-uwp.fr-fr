---
title: Chaînes de permutation
description: Une chaîne de permutation est une collection de mémoires tampons utilisées pour la présentation des images à l’utilisateur.
ms.assetid: A38E8BB7-1E77-4D93-B321-D3572A80D5DD
keywords:
- Chaînes de permutation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 486eb4adc1151bac1bf6a04a8f54b67530b426a3
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8801126"
---
# <a name="swap-chains"></a>Chaînes de permutation


Une chaîne d’échange est une collection de mémoires tampons utilisées pour la présentation des images à l’utilisateur. Chaque fois qu’une application présente une nouvelle image à afficher, la première mémoire tampon de la chaîne d’échange prend la place de la mémoire tampon affichée. Ce processus est désigné sous le terme de *permutation* ou d'*inversion*.

Une carte graphique pointe un curseur sur une surface qui représente l'image diffusée à l'écran, appelée un tampon d’affichage. Après avoir actualisé l'écran, la carte graphique envoie le contenu du tampon d'affichage vers l'écran à afficher. Toutefois, cela conduit à un problème de «coupure» lors du rendu des graphiques en temps réel. Le cœur du problème , c'est que les taux de rafraichissement de l'écran sont très lents en comparaison au reste de l'ordinateur. Les taux d'actualisation vont généralement de 60Hz (60fois par seconde) à 100Hz.

Si votre application met à jour le tampon d’affichage pendant que le moniteur est en cours actualisation, l’image qui s’affiche est coupée en deux, la partie supérieure contenant l’ancienne image et la partie inférieure contenant la nouvelle image. Ce problème est appelé *coupure*.

## <a name="span-idavoidingtearingspanspan-idavoidingtearingspanspan-idavoidingtearingspanavoiding-tearing"></a><span id="Avoiding_tearing"></span><span id="avoiding_tearing"></span><span id="AVOIDING_TEARING"></span>Éviter les coupures


Direct3D met en œuvre deux options pour éviter les erreurs:

-   Une option pour autoriser uniquement les mises à jour de l’opération d’analyse sur les opérations de retracé vertical (ou de synchronisation verticale). Un moniteur actualise généralement son image en déplaçant un point lumineux à l'horizontale, en zigzaguant depuis le coin supérieur gauche de l'écran vers le coin inférieur droit. Une fois qu'il a atteint le bas de l'écran, le moniteur réétalonne le point lumineux dans l’angle supérieur gauche afin de pouvoir recommencer le processus.

    Ce réétalonnage est appelé synchronisation verticale. Au cours d’une synchronisation verticale, le moniteur ne dessine rien, que toute mise à jour la mémoire tampon d’affichage n’est pas visibles jusqu'à ce que le moniteur recommence à dessiner. La synchronisation verticale est relativement lente. Toutefois, elle n'est pas suffisamment lente pour rendre une scène complexe pendant l'attente. Pour éviter les erreurs et être en mesure d'assurer le rendu des scènes complexes, vous devez avoir recours à un processus appelé mise en mémoire tampon d'arrière-plan.

-   Une option pour utiliser une technique appelée mise en mémoire tampon d'arrière-plan. La mise en mémoire tampon d'arrière-plan est le processus consistant à dessiner une surface hors écran appelée mémoire tampon d’arrière-plan. Toutes les surfaces autres que le tampon d’affichage sont appelées des surfaces hors écran car elles ne sont jamais directement affichée par le moniteur.

    En utilisant une mémoire tampon d’arrière-plan, une application a la possibilité de rendre une scène chaque fois que le système est inactif (autrement dit, qu'aucun message Windows n’est en attente) sans avoir à prendre en compte la fréquence de rafraîchissement du moniteur. La mise en mémoire tampon d'arrière-plan rend encore plus complexe la manière et le moment de déplacer la mémoire tampon d'arrière-plan vers la mémoire tampon d'affichage.

## <a name="span-idsurfaceflippingspanspan-idsurfaceflippingspanspan-idsurfaceflippingspansurface-flipping"></a><span id="Surface_flipping"></span><span id="surface_flipping"></span><span id="SURFACE_FLIPPING"></span>Inversion de surface


Le processus de déplacement de la mémoire tampon d’arrière-plan vers la mémoire tampon d’affichage est appelé inversion de surface. Comme la carte graphique utilise simplement un pointeur vers une surface pour représenter la mémoire tampon d’affichage, il est simplement nécessaire d'effectuer une simple modification du pointeur pour transférer la mémoire tampon d’arrière-plan vers la mémoire tampon d’affichage. Lorsqu'une application demande à Direct3D de présenter la mémoire tampon d'arrière-plan à la mémoire tampon d'affichage, Direct3D se contente simplement d'«inverser» les deux pointeurs de surface. Par conséquent, la mémoire tampon d'arrière-plan est désormais la nouvelle mémoire tampon d'affichage, et l'ancienne mémoire tampon d'affichage est désormais la nouvelle mémoire tampon d'arrière-plan.

Une inversion de surface est invoquée chaque fois qu'une application demande à l'appareil Direct3D de présenter la mémoire tampon d'affichage. Cependant, Direct3D peut être configuré pour déplacer les demandes vers la file d'attente jusqu'à ce qu'une synchronisation verticale ne survienne. Cette option est appelée l'intervalle de présentation de l'appareil Direct3D. Les données de la mémoire tampon d’arrière-plan peuvent ne pas être réutilisables, en fonction de la manière dont une application spécifie comment Direct3D doit traiter les inversions de surface.

L'inversion de surface est fréquemment utilisée dans les logiciels multimédia, les logiciels d'animation et les jeux vidéo. Elle correspond à la manière dont vous pouvez créer un animation avec un bloc de papier. Sur chaque page, l’artiste modifie légèrement les figures, de manière à ce que lorsque vous faites tourner rapidement les feuilles, le dessin apparaisse comme animé.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Appareils](devices.md)

 

 




