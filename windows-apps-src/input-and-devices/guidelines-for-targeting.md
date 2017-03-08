---
author: Karl-Bridge-Microsoft
Description: "Cet article décrit l’utilisation de la géométrie de contact pour le ciblage tactile et indique les meilleures pratiques en matière de ciblage dans les applications Windows Runtime."
title: Ciblage
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: fd4ba842a1d6f9eec6012a930a5dda5d7ff7c249
ms.lasthandoff: 02/07/2017

---

# <a name="guidelines-for-targeting"></a>Recommandations en matière de ciblage
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Le ciblage tactile dans Windows utilise la zone de contact entière de chaque doigt détecté par un numériseur tactile. Le jeu de données d’entrée plus grand et plus complexe généré par le numériseur est utilisé pour accroître la précision lors de la détermination de la cible souhaitée (ou la plus probable) de l’utilisateur.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)</li>
<li>[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)</li>
<li>[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)</li>
</ul>
</div>

Cette rubrique décrit l’utilisation de la géométrie de contact pour le ciblage tactile et indique les meilleures pratiques de ciblage dans les applications UWP.

## <a name="measurements-and-scaling"></a>Mesures et mise à l’échelle


Pour conserver la cohérence des différentes tailles d’écran et densités de pixels, toutes les tailles de cibles sont représentées en unités physiques (millimètres). Pour convertir les unités physiques en pixels, utilisez l’équation suivante :

Pixels = densité de pixels × mesure

L’exemple suivant utilise cette formule pour calculer la taille des pixels d’une cible de 9 mm sur un affichage de 135 ppp (pixels par pouce) avec un plateau d’échelle de 1x :

Pixels = 135 ppp × 9 mm

Pixels = 135 ppp × (0,03937 pouces/mm × 9 mm)

Pixels = 135 ppp × 0,35433 pouces

Pixels = 48 pixels

Ce résultat doit être ajusté en fonction de chaque plateau d’échelle défini par le système.

## <a name="thresholds"></a>Seuils


Des seuils de distance et de temps peuvent être utilisés pour déterminer le résultat d’une interaction.

Par exemple, lorsqu’un appui est détecté, il est enregistré si l’objet est glissé de moins de 2,7 mm par rapport au point d’appui et que le doigt est levé dans les 0,1 seconde suivant l’appui. Le fait de déplacer le doigt sur l’écran au-delà de ce seuil de 2,7 mm entraîne un glissement de l’objet et sa sélection ou son déplacement (pour plus d’informations, voir [Recommandations en matière de glisser transversal](guidelines-for-cross-slide.md)). En fonction de votre application, le fait de maintenir appuyé le doigt pendant plus de 0,1 seconde peut provoquer de la part du système une interaction d’auto-révélation (pour plus d’informations, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md)).

## <a name="target-sizes"></a>Taille des cibles


En général, vous devez définir votre cible tactile sur une taille de 9 mm carrés ou plus (48x48 pixels sur un affichage de 135 PPP pour un plateau de mise à l’échelle de 1.0x). Évitez d’utiliser des cibles tactiles inférieures à 7 mm carrés.

Le schéma suivant montre comment la taille de la cible est généralement une combinaison de la cible visuelle, de la taille réelle de la cible et de l’éventuel espacement entre la cible réelle et d’autres cibles potentielles.

![schéma des tailles recommandées pour la cible visuelle, la cible réelle et l’espacement.](images/targeting-size.png)

Le tableau suivant montre les tailles minimales et recommandées pour les composants d’une cible tactile.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Composant de cible</th>
<th align="left">Taille minimale</th>
<th align="left">Taille recommandée</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Espacement</td>
<td align="left">2 mm</td>
<td align="left">Non applicable</td>
</tr>
<tr class="even">
<td align="left">Taille visuelle de la cible</td>
<td align="left">&lt; 60 % de la taille réelle</td>
<td align="left">90 à 100 % de la taille réelle
<p>La plupart des utilisateurs ne se rendront pas compte qu’une cible visuelle est tactile si elle fait moins de 4,2 mm carrés (60 % de la taille de cible minimale recommandée, 7 mm).</p></td>
</tr>
<tr class="odd">
<td align="left">Taille réelle de la cible</td>
<td align="left">7 mm carrés</td>
<td align="left">Supérieure ou égale à 9 mm carrés (48 x 48 px @ 1x)</td>
</tr>
<tr class="even">
<td align="left">Taille totale de la cible</td>
<td align="left">11 x 11 mm (environ 60 px : trois unités de grille de 20 px à 1x)</td>
<td align="left">13,5 x 13,5 mm (72 x 72 px à 1x)
<p>Ceci implique que la taille combinée de la cible réelle et de l’espacement soit supérieure à leurs valeurs minimales respectives.</p></td>
</tr>
</tbody>
</table>

 

Vous pouvez modifier ces recommandations en matière de tailles de cibles en fonction du scénario particulier de votre application. Certaines des remarques suivantes liées à ces recommandations doivent être prises en compte :

-   Fréquences des interactions tactiles : il est recommandé que la taille des cibles utilisées de manière répétée ou fréquente soit supérieure à la taille minimale.
-   Conséquences des erreurs : les cibles dont l’utilisation par erreur peut être lourde de conséquences doivent être affectées d’un espacement supplémentaire et placées à une distance plus éloignée du bord de la zone de contenu. Ceci concerne tout particulièrement les cibles fréquemment utilisées.
-   Position dans la zone de contenu
-   Facteur de forme et taille d’écran
-   Position des doigts
-   Visualisations tactiles
-   Digitaliseurs matériel et tactile

## <a name="targeting-assistance"></a>Aide au ciblage


Windows offre une aide au ciblage pour prendre en charge les scénarios dans lesquels les recommandations en matière de taille ou d’espacement minimal présentées ici ne peuvent pas s’appliquer ; par exemple, des liens hypertexte sur une page web, des contrôles de calendrier, des listes déroulantes et des zones de liste modifiable ou une sélection de texte.

Ces améliorations de la plateforme de ciblage et les comportement de l’interface utilisateur fonctionnent ensemble avec le retour visuel (interface utilisateur de résolution des ambiguïtés) afin d’améliorer la précision et la confiance de l’utilisateur. Pour plus d’informations, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md).

Si la taille d’un élément tactile doit être inférieure à la taille de cible minimale recommandée, vous pouvez utiliser les techniques suivantes pour réduire les problèmes de ciblage qui en découlent.

## <a name="tethering"></a>Connexion


La connexion est un signal visuel (un connecteur situé entre un point de contact et le rectangle englobant d’un objet) qui sert à indiquer à l’utilisateur qu’il est connecté à un objet et qu’il interagit avec celui-ci, même si le contact d’entrée ne touche pas directement l’objet. Ceci peut se produire dans les cas suivants :

-   Un contact tactile a été détecté pour la première fois dans un certain seuil de proximité d’un objet et cet objet a été identifié comme la cible la plus probable du contact.
-   Un contact tactile a été éloigné d’un objet, mais il reste encore dans un seuil de proximité.

Cette fonctionnalité n’est pas exposée aux développeurs d’applications du Windows Store en JavaScript.

## <a name="scrubbing"></a>Frottement


Le frottement consiste à toucher n’importe où dans un champ de cibles et de laisser glisser le doigt pour sélectionner la cible souhaitée sans le lever tant qu’il n’est pas sur la cible souhaitée. Avec cette technique, également appelée « activation par décollage », l’objet qui est activé, est celui qui a été touché en dernier lorsque l’utilisateur a levé le doigt de l’écran.

Pour concevoir des interactions de frottement, tenez compte des recommandations suivantes :

-   Le frottement est utilisé conjointement avec l’interface utilisateur de résolution des ambiguïtés. Pour plus d’informations, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md).
-   La taille minimale recommandée pour une cible tactile de frottement est de 20 px (3,75 mm à 1x).
-   Le frottement est prioritaire lorsqu’il est effectué sur une surface de mouvement panoramique, telle qu’une page Web.
-   Les cibles de frottement doivent être placées à proximité les unes des autres.
-   L’action est annulée si l’utilisateur cesse de faire glisser son doigt sur la cible de frottement.
-   La connexion à une cible de frottement est spécifiée si les actions effectuées par la cible sont sans danger ; par exemple, passer d’une date à une autre dans un calendrier.
-   La connexion est spécifiée dans une seule direction, horizontalement ou verticalement.

## <a name="related-articles"></a>Articles connexes


**Exemples**
* [Exemple d’entrée de base](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple de visuels de focus](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Entrée : exemple d’événements d’entrée utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrée : exemple de fonctionnalités d’appareils](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : exemple de test de positionnement tactile](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : exemple d’entrée manuscrite simplifiée](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrée : exemple de mouvements Windows 8](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrée : exemple de manipulations et de mouvements (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Exemple d’entrée tactile DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 





