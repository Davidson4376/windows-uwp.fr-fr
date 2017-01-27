---
author: mithom
title: "Entrées pour les jeux"
description: "Cette section indique comment utiliser des boîtiers de commande et d’autres périphériques d’entrée pour les jeux de plateforme Windows universelle (UWP)."
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
translationtype: Human Translation
ms.sourcegitcommit: 858bf6a0862d6459b2bac22d8ab9431b51332fef
ms.openlocfilehash: 8d1cfa840e359ad1f6a890ed7e7425d387393e15

---

# <a name="input-for-games"></a>Entrées pour les jeux

Cette section décrit les différents types de périphériques d’entrée qui peuvent être utilisés dans les jeux UWP sur Windows 10 et Xbox One. Elle illustre également leur utilisation de base, et recommande les modèles et techniques à appliquer pour obtenir une programmation des entrées efficace dans les jeux.

> **Remarque**    D’autres types de périphériques d’entrée existent et peuvent être utilisés dans les jeux UWP, par exemple des périphériques d’entrée personnalisés qui peuvent être propres à un genre ou à un jeu. Cette section ne couvre pas ces périphériques ni leur programmation. Pour plus d’informations sur les interfaces utilisées pour faciliter les périphériques d’entrée personnalisés, voir l’espace de noms [Windows.Gaming.Input.Custom][].

## <a name="gaming-input-devices"></a>Périphériques d’entrée de jeu

Les périphériques d’entrée de jeu sont pris en charge dans les jeux et applications UWP pour Windows 10 et Xbox One, via l’espace de noms [Windows.Gaming.Input][].

### <a name="gamepads"></a>Boîtiers de commande

Les boîtiers de commande représentent le périphérique d’entrée standard sur Xbox One et ils sont couramment choisis par les joueurs Windows s’ils ne préfèrent pas utiliser un clavier et une souris. Ils fournissent de nombreux contrôles numériques et analogiques qui conviennent à pratiquement tous les types de jeu, ainsi que des retours tactiles via des moteurs de vibration incorporés.

Pour plus d’informations sur l’utilisation des boîtiers de commande dans votre jeu UWP, consultez [Boîtier de commande et vibrations](gamepad-and-vibration.md).

### <a name="arcade-sticks"></a>Sticks arcade

Les sticks arcade sont des périphériques d’entrée entièrement numériques qui sont estimés pour reproduire la sensation des machines d’arcade autonomes et qui constituent le périphérique d’entrée parfait pour les combats tête à tête ou d’autres jeux de type arcade.

Pour plus d’informations sur l’utilisation des sticks arcade dans votre jeu UWP, consultez [Stick arcade](arcade-stick.md).

### <a name="racing-wheels"></a>Volants de course

Les volants de course sont des périphériques d’entrée dont l’utilisation rappelle celle du volant d’une voiture de course réelle. Ils sont idéaux pour les jeux de course qui mettent en scène des voitures ou des camions. De nombreux volants de course sont équipés d’un retour de force réel ; ils peuvent appliquer des forces réelles sur un axe de contrôle, comme le volant, et non une simple vibration.

Pour plus d’informations sur l’utilisation des volants de course dans votre jeu UWP, consultez [Volant de course et retour de force](racing-wheel-and-force-feedback.md).

### <a name="ui-navigation-controller"></a>Contrôleur de navigation d’interface utilisateur

Les contrôleurs de navigation d’interface utilisateur sont des périphériques d’entrée logiques qui fournissent un vocabulaire commun pour les commandes de navigation d’interface utilisateur, qui promeut une expérience utilisateur homogène entre les différents jeux et les périphériques d’entrée physiques. L’interface utilisateur d’un jeu doit utiliser les interfaces UINavigationController et non les interfaces propres à un périphérique.

Pour plus d’informations sur l’utilisation des contrôleurs de navigation d’interface utilisateur dans votre jeu UWP, consultez [Contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md).

### <a name="headsets"></a>Casques

Les casques sont des périphériques de capture et de lecture audio, qui sont associés à des utilisateurs spécifiques lorsqu’ils sont connectés via leur périphérique d’entrée. Ils sont couramment utilisés pour les conversations vocales des jeux en ligne, mais ils permettent également d’améliorer l’immersion des joueurs ou de fournir des fonctionnalités de séquence de jeu dans les jeux en ligne et hors connexion.

Pour plus d’informations sur l’utilisation des casques dans votre jeu UWP, consultez [Casque](headset.md).

### <a name="users"></a>Utilisateurs

Chaque périphérique d’entrée et son casque connecté peuvent être associés à un utilisateur spécifique pour lier l’identité de celui-ci à sa séquence de jeu. L’identité de l’utilisateur est également le moyen par le biais duquel les entrées d’un périphérique d’entrée physique sont mises en corrélation avec les entrées de son contrôleur de navigation d’interface utilisateur logique.

Pour en savoir plus sur la gestion des utilisateurs et de leurs périphériques d’entrée, consultez [Suivi des utilisateurs et de leurs périphériques](input-practices-for-games.md#tracking-users-and-their-devices).

## <a name="see-also"></a>Voir aussi
[Windows.Gaming.Input.Custom][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Custom]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.gaming.input.custom.aspx



<!--HONumber=Dec16_HO3-->


