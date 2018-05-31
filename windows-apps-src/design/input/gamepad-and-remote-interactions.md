---
author: mijacobs
Description: TODO
title: Interactions entre le boîtier de commande et la télécommande
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3713d74edd93f437726c04dd68b604cb8a22da8f
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
ms.locfileid: "1392428"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interactions entre le boîtier de commande et la télécommande

Les applications Universal Windows Platform (UWP) prennent désormais en charge les entrées du boîtier de commande et de la télécommande. Les boîtiers de commandes et les télécommandes sont les principaux appareils d’entrée utilisables avec une Xbox et un téléviseur. Les applications UWP doivent être optimisées pour ces types d’appareils d’entrée, comme c’est le cas pour les entrées via le clavier et la souris d’un ordinateur, et pour les entrées tactiles sur un téléphone ou une tablette. Il est essentiel de s’assurer que votre application fonctionne correctement avec ces appareils d’entrée lorsque vous les optimisez pour une Xbox et un téléviseur.
Vous pouvez maintenant brancher le boîtier de commande et l’utiliser avec des applications UWP sur un ordinateur, ce qui facilite la validation du travail.

Pour garantir une expérience utilisateur réussie et agréable avec l’application UWP lors de l’utilisation d’un boîtier de commande ou d’une télécommande, envisagez ce qui suit:

* [Boutons matériels](../devices/designing-for-tv.md#hardware-buttons)-Le boîtier de commande et la télécommande ont des configurations et des boutons très différents.

* [Navigation et interaction en mode focus XY](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction)-La navigation en mode focus XY permet à l’utilisateur de naviguer dans l’interface utilisateur de votre application.

* [Mode souris](../devices/designing-for-tv.md#mouse-mode)-Le mode souris permet à votre application d’émuler l’utilisation de la souris lorsque la navigation en mode focus XY n’est pas suffisante.
