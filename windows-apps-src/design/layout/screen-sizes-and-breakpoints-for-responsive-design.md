---
title: Tailles d’écran et points d’arrêt pour la conception réactive
description: Au lieu d’optimiser votre interface utilisateur pour les nombreux appareils de l’écosystème Windows10, nous vous recommandons de concevoir une application pour plusieurs largeurs principales appelées «points d’arrêt».
ms.date: 08/30/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0959c9bc09782538cdb15a68c46b0797d4b7d230
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335081"
---
#  <a name="screen-sizes-and-breakpoints"></a>Tailles d’écran et points d’arrêt

Les applications UWP peuvent s’exécuter sur n’importe quel appareil exécutant Windows10, y compris les téléphones, tablettes, ordinateurs de bureau, téléviseurs et plus encore. Avec un nombre considérable d’appareils cibles et de tailles d’écran dans l’écosystème Windows 10, au lieu d’optimiser votre interface utilisateur pour chaque appareil, nous vous recommandons de concevoir une application pour plusieurs largeurs principales (également appelées «points d’arrêt»): 
- Petite (inférieure à 640pixels)
- Moyenne (641px à 1007px)
- Grande (supérieure ou égale à 1008px)

> [!TIP]
> Lors de la conception pour des points d’arrêt spécifiques, pensez à la quantité d’espace d’écran disponible pour votre application (fenêtre de l’application), et non à la taille d’écran. Lorsque l’application s’exécute plein écran, la fenêtre d’application est de la même taille que l’écran, mais lorsque l’application n’est pas plein écran, la fenêtre est inférieure à l’écran.

## <a name="breakpoints"></a>Points d’arrêt
Ce tableau décrit les différentes classes de taille et les points d’arrêt.

![Points d’arrêt de la conception réactive](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">Classe de taille</th>
<th align="left">Points d’arrêt</th>
<th align="left">Taille d’écran standard (diagonale)</th>
<th align="left">Périphériques</th>
<th align="left">Tailles de fenêtre</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Petite</td>
<td style="vertical-align:top;">640px maximum</td>
<td style="vertical-align:top;">4&quot; à 6&quot;; 20&quot; à 65&quot;</td>
<td style="vertical-align:top;">Téléphones, téléviseurs</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Moyenne</td>
<td style="vertical-align:top;">641px à 1007px</td>
<td style="vertical-align:top;">7&quot; à12&quot;</td>
<td style="vertical-align:top;">Phablettes, tablettes</td>
<td style="vertical-align:top;">960x540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Grande</td>
<td style="vertical-align:top;">1008px minimum</td>
<td style="vertical-align:top;">13&quot; et supérieur</td>
<td style="vertical-align:top;">PC, ordinateurs portables, Surface Hub</td>
<td style="vertical-align:top;">1024x640, 1366x768, 1920x1080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>Pourquoi les téléviseurs sont-ils considérés comme de «petite» taille? 

Bien que la plupart des écrans de télévision soient physiquement très grands (une taille de 40 à 65pouces est courante) et offrent des résolutions élevées (HD ou 4K), la conception d'un téléviseur en 1080P que vous regardez à 3mètres (10pieds) de distance est différente de celle d'un moniteur en 1080p placé à 30cm de distance sur votre bureau. Lorsque vous tenez compte de la distance, les téléviseurs 1080pixels s'apparentent plus à un moniteur 540pixels qui est beaucoup plus proche.

Le système de pixels effectifs d'UWP prend automatiquement en compte la distance d’affichage. Lorsque vous spécifiez une taille pour un contrôle ou une plage de point d’arrêt, vous utilisez en fait des pixels «effectifs». Par exemple, si vous créez un code réactif pour 1080pixels et versions ultérieures, un moniteur de 1080 utilisera ce code, mais pas un téléviseur en 1080p, car même si un téléviseur en 1080p possède 1080pixels physiques, il a uniquement 540pixels effectifs. Cela rend la conception d'un téléviseur similaire à celle d'un téléphone.

## <a name="effective-pixels-and-scale-factor"></a>Pixels effectifs et facteur d’échelle

Les applications UWP mettent automatiquement à l’échelle votre interface utilisateur afin de garantir que votre application pourra être lisible sur tous les appareils Windows10. Windows met automatiquement à l’échelle chaque affichage en fonction de son nombre de PPP (points par pouce) et de la distance de visualisation de l’appareil. Les utilisateurs peuvent remplacer la valeur par défaut en accédant à page de paramètres**Paramètres** > **Affichage** > **Échelle et disposition**. 


## <a name="general-recommendations"></a>Recommandations générales

### <a name="small"></a>Petite
- Définissez une marge de 12px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.
- Ancrez les [barres de l’application](../controls-and-patterns/app-bars.md) en bas de la fenêtre pour faciliter l’accessibilité.
- Utilisez une colonne/zone à la fois.
- Utilisez une icône pour représenter la recherche (n’affichez pas de zone de recherche).
- Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode Superposition pour économiser l’espace à l’écran.
- Si vous utilisez le [modèle Maître/Détails](../controls-and-patterns/master-details.md), utilisez le mode présentation empilée pour économiser l’espace d’écran.

### <a name="medium"></a>Moyenne
- Définissez une marge de 24px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.
- Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.
- Utilisez jusqu’à deuxcolonnes/zones.
- Affichez la zone de recherche.
- Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode partiel de manière à ce qu’une bande étroite d’icônes soit toujours affichée.
- Pensez à personnaliser davantage votre application pour les [expériences télévisuelles](http://go.microsoft.com/fwlink/?LinkId=760736).

### <a name="large"></a>Grande
- Définissez une marge de 24px de part et d’autre de la fenêtre afin de créer une séparation visuelle au niveau des bords gauche et droit de la fenêtre de l’application.
- Placez les éléments de commande comme les [barres de l’application](../controls-and-patterns/app-bars.md) en haut de la fenêtre de l’application.
- Utilisez jusqu’à troiscolonnes/zones.
- Affichez la zone de recherche.
- Placez le [volet de navigation](../controls-and-patterns/navigationview.md) en mode ancré pour qu’il soit toujours affiché.

>[!TIP] 
> Avec [**Continuum pour téléphones**](http://go.microsoft.com/fwlink/p/?LinkID=699431), les utilisateurs peuvent connecter des appareils mobiles Windows 10 compatibles à un moniteur, souris et clavier à apporter leurs téléphones fonctionnent comme des ordinateurs portables. N’oubliez pas cette nouvelle fonctionnalité lorsque vous concevez des points d’arrêt spécifiques: un téléphone mobile ne reste pas toujours dans la classe de taille.


