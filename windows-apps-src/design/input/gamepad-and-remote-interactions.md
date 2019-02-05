---
author: Karl-Bridge-Microsoft
Description: Optimize your app for input from Xbox gamepad and remote control.
title: Interactions entre le boîtier de commande et la télécommande
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 022724064ad1e7f5551b6676bf256ca5cf6e4b8e
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048556"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interactions entre le boîtier de commande et la télécommande

![image clavier et boîtier de commande](images/keyboard/keyboard-gamepad.jpg)

***Modèles d’interaction communs sont partagés entre le boîtier de commande, télécommande et le clavier***

Le bon fonctionnement de votre application avec un boîtier de commande et une télécommande représente l’étape la plus importante de l’optimisation des expériences «10-foot». Vous pouvez cependant apporter des améliorations relatives aux boîtiers de commande et aux télécommandes pour optimiser l’expérience d’interaction utilisateur sur un appareil où leurs actions sont relativement limitées.

Les applications Universal Windows Platform (UWP) prennent désormais en charge les entrées du boîtier de commande et de la télécommande. 

Les boîtiers de commandes et les télécommandes sont les principaux appareils d’entrée utilisables avec une Xbox et un téléviseur. 

Les applications UWP doivent être optimisées pour ces types d’appareils d’entrée, comme c’est le cas pour les entrées via le clavier et la souris d’un ordinateur, et pour les entrées tactiles sur un téléphone ou une tablette. 

Il est essentiel de s’assurer que votre application fonctionne correctement avec ces appareils d’entrée lorsque vous les optimisez pour une Xbox et un téléviseur.

Vous pouvez maintenant brancher le boîtier de commande et l’utiliser avec des applications UWP sur un ordinateur, ce qui facilite la validation du travail.

Pour garantir une expérience utilisateur réussie et agréable avec l’application UWP lors de l’utilisation d’un boîtier de commande ou d’une télécommande, envisagez ce qui suit:

* [Boutons matériels](../devices/designing-for-tv.md#hardware-buttons)-Le boîtier de commande et la télécommande ont des configurations et des boutons très différents.

* [Navigation et interaction en mode focus XY](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction)-La navigation en mode focus XY permet à l’utilisateur de naviguer dans l’interface utilisateur de votre application.

* [Mode souris](../devices/designing-for-tv.md#mouse-mode)-Le mode souris permet à votre application d’émuler l’utilisation de la souris lorsque la navigation en mode focus XY n’est pas suffisante.
