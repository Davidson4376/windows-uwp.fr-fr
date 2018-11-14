---
author: Karl-Bridge-Microsoft
Description: This topic describes Windows zooming and resizing elements and provides user experience guidelines for using these interaction mechanisms in your apps.
title: Recommandations en matière de zoom optique et de redimensionnement
ms.assetid: 51a0007c-8a5d-4c44-ac9f-bbbf092b8a00
label: Optical zoom and resizing
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2087debd758a24b50ac1885cb68d4b97ea2898fd
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/08/2018
ms.locfileid: "6161910"
---
# <a name="optical-zoom-and-resizing"></a>Zoom optique et redimensionnement



Cet article décrit les éléments de zoom et de redimensionnement Windows. Il fournit également des recommandations en matière d’expérience utilisateur en cas d’utilisation de ces mécanismes d’interaction dans vos applications.

> **API importantes**: [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Input (XAML)**](https://msdn.microsoft.com/library/windows/apps/br227994)

Le zoom optique permet à l’utilisateur d’agrandir la vue du contenu au sein d’une zone de contenu (l’interaction s’effectue sur la zone de contenu elle-même), tandis que le redimensionnement permet de modifier la taille relative d’un ou de plusieurs objets sans modifier la vue dans la zone de contenu (dans ce cas, l’interaction s’effectue sur les objets figurant dans la zone de contenu).

Les interactions de zoom optique et de redimensionnement s’effectuent à l’aide des gestes de pincement et d’étirement (resserrez les doigts pour faire un zoom avant et écartez-les pour faire un zoom arrière) ou en maintenant la touche Ctrl enfoncée tout en faisant défiler la roulette de la souris, ou encore en maintenant la touche Ctrl enfoncée (ou la touche Maj s’il n’y a pas de clavier numérique) et en appuyant sur la touche plus (+) ou moins (-).

Les schémas suivants montrent les différences entre le redimensionnement et le zoom optique.

**Zoom optique**: l’utilisateur sélectionne une zone, puis effectue un zoom sur la totalité de la zone.

![resserrez les doigts pour faire un zoom avant et écartez-les pour faire un zoom arrière.](images/areazoom.png)

**Redimensionnement**: l’utilisateur sélectionne un objet au sein d’une zone et le redimensionne.

![resserrer les doigts pour rétrécir un objet et les écarter pour l’agrandir](images/objectresize.png)

**Remarque**  zoom optique ne pas confondre avec [Le Zoom sémantique](../controls-and-patterns/semantic-zoom.md). Même si les mêmes gestes sont utilisés pour les deuxinteractions, le zoom sémantique désigne la présentation et la navigation du contenu organisé au sein d’une seule vue (par exemple, la structure de dossiers d’un ordinateur, une bibliothèque de documents ou un album photo).

 

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


Pour les applications prenant en charge le redimensionnement ou le zoom optique, tenez compte des recommandations suivantes:

-   Si des limites ou des contraintes de tailles maximale et minimale sont définies, utilisez le retour visuel pour indiquer quand l’utilisateur atteint ou dépasse ces limites.
-   Utilisez des points d’ancrage pour influencer le comportement du zoom et du mouvement panoramique en spécifiant des points logiques où arrêter la manipulation et garantir qu’un sous-ensemble spécifique de contenu soit visible dans la fenêtre d’affichage. Prévoyez des points d’ancrage pour les niveaux de zoom ou les affichages logiques courants afin de faciliter leur sélection par l’utilisateur. Par exemple, une application de photographie peut prévoir un point d’ancrage de redimensionnement à 100 % ou, dans le cas d’une application de cartographie, les points d’ancrage peuvent s’avérer utiles dans les vues de la ville, de la province et du pays.

    Les points d’ancrage permettent à l’utilisateur d’exécuter correctement ces opérations, en exigeant moins de précision dans ses mouvements. Si vous utilisez XAML, reportez-vous aux propriétés de points d’ancrage de [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527). Pour JavaScript et HTML, utilisez [**-ms-content-zoom-snap-points**](https://msdn.microsoft.com/library/hh771895).

    Il existe deux types de points d’ancrage:

    -   De proximité : lorsque l’utilisateur met fin au contact, un point d’ancrage est sélectionné si l’inertie s’arrête au sein du seuil de distance du point d’ancrage. Les points d’ancrage de proximité permettent à un zoom ou à un redimensionnement de se terminer entre des points d’ancrage.
    -   Obligatoire : le point d’ancrage sélectionné est celui qui précède ou qui suit immédiatement le dernier point d’ancrage rencontré avant que l’utilisateur ait mis fin au contact (en fonction de la direction et de la vitesse du mouvement). Une manipulation doit prendre fin sur un point d’ancrage obligatoire.
-   Utilisez les principes d’inertie, notamment :
    -   Décélération : se produit dès que l’utilisateur arrête le pincement ou l’étirement. Cette action s’apparente à glisser sur une surface glissante jusqu’à l’arrêt.
    -   Rebond : un léger effet de rebond se produit lorsqu’une limite ou une contrainte de taille est dépassée.
-   Espacez les contrôles conformément aux [Recommandations en matière de ciblage](guidelines-for-targeting.md).
-   Fournissez des poignées de redimensionnement pour le redimensionnement contraint. Le redimensionnement isométrique, ou proportionnel, est l’option par défaut si les poignées ne sont pas spécifiées.
-   N’utilisez pas la fonction de zoom pour parcourir l’interface utilisateur ou exposer des contrôles supplémentaires au sein de votre application ; utilisez plutôt une région de mouvement panoramique. Pour plus d’informations sur le mouvement panoramique, voir [Recommandations en matière de mouvement panoramique](guidelines-for-panning.md).
-   Ne placez pas d’objets redimensionnables dans une zone de contenu redimensionnable. Les exceptions à cette règle sont les suivantes :
    -   Applications de dessin dans lesquelles des éléments redimensionnables peuvent s’afficher sur une zone de dessin ou un carton redimensionnable.
    -   Pages web comportant un objet incorporé, tel qu’une carte.

    **Remarque**  dans tous les cas, la zone de contenu sera redimensionnée, sauf si tous les points tactiles figurent dans l’objet redimensionnable.

     

## <a name="related-articles"></a>Articles connexes


**Exemples**
* [Exemple d’entrée de base](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Entrée: exemple d’événements d’entrée utilisateurXAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrée : exemple de fonctionnalités d’appareils](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple de test de positionnement tactile](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [Exemple de zoom, de panoramique et de défilement XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : exemple d’entrée manuscrite simplifiée](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrée : exemple de mouvements Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrée : exemple de manipulations et de mouvements (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Exemple d’entrée tactile DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




