---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Étalonner les capteurs
description: Dans un appareil basé sur le magnétomètre (la boussole, l’inclinomètre et le capteur d’orientation), il peut s’avérer nécessaire d’étalonner les capteurs en raison de facteurs environnementaux.
---
# Étalonner les capteurs

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

** API importantes **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Dans un appareil basé sur le magnétomètre (la boussole, l’inclinomètre et le capteur d’orientation), il peut s’avérer nécessaire d’étalonner les capteurs en raison de facteurs environnementaux. L’énumération [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) peut aider à déterminer la marche à suivre quand votre appareil doit être étalonné.

## Quand étalonner le magnétomètre

L’énumération [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) comporte quatre valeurs qui vous permettent de déterminer si l’appareil sur lequel votre application s’exécute doit être étalonné. Si un appareil doit être étalonné, vous devez faire savoir à l’utilisateur que cette opération est nécessaire. Toutefois, vous ne devez pas demander trop souvent à l’utilisateur d’effectuer un étalonnage. Nous recommandons une fréquence maximale d’une fois toutes les 10 minutes.

| Valeur | Description |-----------------|-------------------| | **Unknown** | Le pilote du capteur n’a pas pu indiquer la précision actuelle. Cela ne signifie pas nécessairement que l’appareil n’est pas étalonné. Votre application doit décider de la meilleure marche à suivre si la valeur **Unknown** est retournée. Si votre application dépend d’une lecture précise des capteurs, vous pouvez demander à l’utilisateur d’étalonner l’appareil. |
| **Unreliable** | Il y a actuellement un niveau élevé d’imprécision dans le magnétomètre. Les applications doivent toujours demander un étalonnage par l’utilisateur quand cette valeur est retournée pour la première fois. |
| **Approximate** | Les données sont suffisamment précises pour certaines applications. Une application de réalité virtuelle, qui doit uniquement savoir si l’utilisateur a déplacé l’appareil vers le haut/bas ou vers la gauche/droite, peut continuer sans étalonnage. Les applications qui ont besoin d’un en-tête absolu, comme une application de navigation qui doit connaître la direction dans laquelle vous conduisez pour vous indiquer des directions, doivent demander un étalonnage. |
| **High** | Les données sont précises. Aucun étalonnage n’est nécessaire, même pour les applications qui doivent connaître un en-tête absolu, telles que la réalité augmentée ou les applications de navigation. |

## Comment étalonner le magnétomètre

Cette brève vidéo donne une vue d’ensemble de la façon d’étalonner le magnétomètre.<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/727bd0e3-9116-49c3-8af6-0b4339324b71/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">One Dev Minute - Étalonnage des capteurs</iframe>





<!--HONumber=Mar16_HO1-->


