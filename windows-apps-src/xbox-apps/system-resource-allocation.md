---
author: Mtoepke
title: Ressources système pour les applications UWP et les jeux sur Xbox One
description: UWP sur les ressources système Xbox
area: Xbox
---

# Ressources système pour les applications UWP et les jeux sur Xbox One

Les applications et les jeux UWP s’exécutant sur Xbox One partagent les ressources avec le système et d’autres applications. 
Par conséquent, les applications et les jeux UWP ont accès aux ressources suivantes :

* Dans cette version d’évaluation, la mémoire maximale disponible est de 448 Mo.
    * Dans les versions ultérieures, la mémoire maximale disponible est de 1 Go.
    * Lorsque vous exécutez votre application ou votre jeu à partir du débogueur Visual Studio, ces limites de mémoire ne s’appliquent pas. Cette limite s’applique uniquement quand l’exécution n’a pas lieu en mode débogage.

* Partage de 2 à 4 cœurs de processeur en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

* Partage de 45 % du GPU en fonction du nombre d’applications et de jeux en cours d’exécution sur le système.

* UWP sur Xbox One prend en charge la fonctionnalité DirectX 11 niveau 10. DirectX 12 n’est pas pris en charge pour l’instant. 

Pour le **développement d’applications**, il est important de garder à l’esprit que les ressources disponibles peuvent être limitées par rapport à un ordinateur standard.

Pour le **développement de jeux**, il est important de garder à l’esprit que Xbox One, comme les autres consoles de jeu, est un élément matériel spécialisé qui nécessite un kit de développement matériel spécifique pour offrir tout son potentiel. 
Si vous travaillez sur un jeu sollicitant les capacités maximales du matériel Xbox One, vous pouvez vous inscrire auprès du programme [ID@XBOX](http://www.xbox.com/en-us/Developers/id) pour accéder aux kits de développement Xbox One, qui incluent la prise en charge de DirectX 12.


<!--HONumber=May16_HO2-->


