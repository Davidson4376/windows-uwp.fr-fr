---
Description: Répondez à l’entrée de souris dans vos applications en gérant les mêmes événements de pointeur de base que ceux utilisés pour l’entrée tactile et du stylo.
title: Interactions avec la souris
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f81634fdb0f9382b1f660394764e5555189783e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622354"
---
# <a name="mouse-interactions"></a>Interactions avec la souris

Optimisez la conception de votre application de plateforme Windows universelle (UWP) pour l’entrée tactile, et définissez la prise en charge de la souris de base par défaut. 

![souris](images/input-patterns/input-mouse.jpg)

Les entrées de souris conviennent mieux aux interactions utilisateur qui demandent de la précision comme le pointage et le clic. Cette précision inhérente est naturellement prise en charge par l’interface utilisateur de Windows qui permet de gérer la nature imprécise de l’entrée tactile.

Les entrées tactiles et de la souris divergent en raison de la capacité de l’entrée tactile à émuler plus fidèlement la manipulation directe d’éléments d’interface utilisateur par le biais de mouvements physiques effectués directement sur ces objets (comme le balayage, le glissement, la rotation, etc.). Les manipulations avec une souris nécessitent généralement une autre affordance d’interface utilisateur, telle l’utilisation de poignées pour redimensionner ou faire pivoter un objet.

Cette rubrique décrit les considérations relatives à la conception pour les interactions avec la souris.

## <a name="the-uwp-app-mouse-language"></a>Langage de souris d’application UWP

Un ensemble concis d’interactions avec la souris est utilisé de façon uniforme dans l’ensemble du système.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Terme</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Pointer pour apprendre</p></td>
<td align="left"><p>Pointez sur un élément pour afficher des informations détaillées ou des éléments visuels didactiques (tels qu’une info-bulle) ne requérant aucune action.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Cliquer avec le bouton gauche pour effectuer l’action principale</p></td>
<td align="left"><p>Cliquez avec le bouton gauche sur un élément pour appeler son action principale (par exemple, le lancement d’une application ou l’exécution d’une commande).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Faire défiler l’affichage pour changer de vue</p></td>
<td align="left"><p>Affichez des barres de défilement pour monter, descendre, aller à gauche et à droite dans une zone de contenu. Les utilisateurs peuvent faire défiler l’affichage en cliquant sur les barres de défilement ou en actionnant la roulette de la souris. Les barres de défilement peuvent indiquer l’emplacement de la vue actuelle dans la zone de contenu (un mouvement panoramique avec interaction tactile affiche une interface utilisateur similaire).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Cliquer avec le bouton droit pour sélectionner une commande</p></td>
<td align="left"><p>Cliquez avec le bouton droit sur la barre de navigation (si elle est disponible) et la barre de l’application avec des commandes globales. Cliquez avec le bouton droit sur un élément pour le sélectionner et afficher la barre de l’application contenant des commandes contextuelles pour l’élément sélectionné.</p>
<div class="alert">
<strong>Remarque</strong>  avec le bouton droit pour afficher un menu contextuel si l’application ou la sélection de la barre de commandes n’est pas des comportements de l’interface utilisateur appropriées. Toutefois, nous vous recommandons vivement d’utiliser la barre de l’application pour tous les comportements des commandes.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>Commandes d’interface utilisateur pour le zoom</p></td>
<td align="left"><p>Affichez des commandes d’interface utilisateur dans la barre de l’application (telles que + et -) ou appuyez sur Ctrl et actionnez la roulette de la souris pour émuler des mouvements de pincement et d’étirement pour le zoom.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Commandes d’interface utilisateur pour la rotation</p></td>
<td align="left"><p>Affichez des commandes d’interface utilisateur dans la barre de l’application ou appuyez sur Ctrl+Maj et actionnez la roulette de la souris pour émuler un mouvement de rotation. Faites pivoter l’appareil lui-même pour faire pivoter l’écran tout entier.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Cliquer avec le bouton gauche et faire glisser pour réorganiser</p></td>
<td align="left"><p>Cliquez avec le bouton gauche sur un élément et faites-le glisser pour le déplacer.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Cliquer avec le bouton gauche et faire glisser pour sélectionner du texte</p></td>
<td align="left"><p>Cliquez avec le bouton gauche dans du texte sélectionnable et faites glisser le curseur pour sélectionner du texte. Double-cliquez pour sélectionner un mot.</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-input-events"></a>Événements d’entrée de souris

La plupart des souris d’entrée peuvent être gérés via les événements routés d’entrée communs pris en charge par tous les [ **UIElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) objets. Par exemple :

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**DROP**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**Réception focus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Maintenant**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**L’appui sur**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

Toutefois, vous pouvez tirer parti des fonctionnalités spécifiques de chaque appareil (par exemple, les événements de roulette de souris) en utilisant les événements de pointeur, de mouvement et de manipulation dans [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input).

**Exemples :** Consultez notre [BasicInput exemple](https://go.microsoft.com/fwlink/p/?LinkID=620302), pour.

## <a name="guidelines-for-visual-feedback"></a>Recommandations en matière de retour visuel

- Quand des événements de déplacement ou de pointage permettent de détecter une souris, affichez une interface utilisateur propre à la souris pour indiquer les fonctionnalités exposées par l’élément. Si la souris ne bouge pas pendant un certain temps ou si l’utilisateur commence une interaction tactile, estompez progressivement l’interface utilisateur de la souris. Cela maintient l’interface utilisateur propre et aérée.
- N’utilisez pas le curseur pour le retour de pointage, car le retour fourni par l’élément est suffisant (voir la section Curseurs, ci-dessous).
- N’affichez pas de retour visuel si un élément ne prend pas en charge l’interaction (tel que le texte statique).
- N’utilisez pas de rectangles de sélection avec les interactions avec la souris. Réservez ceux-ci aux interactions avec le clavier.
- Affichez un retour visuel simultanément pour tous les éléments qui représentent la même cible d’entrée.
- Fournissez des boutons (tels que + et -) pour émuler des manipulations tactiles, telles que le mouvement panoramique, la rotation, le zoom, etc.

Pour obtenir des recommandations plus générales sur le retour visuel, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md).

## <a name="cursors"></a>Curseurs

Un ensemble de curseurs standard est disponible pour servir de pointeurs de souris. Ces derniers sont utilisés pour indiquer l’action principale d’un élément.

Chaque curseur standard possède une image par défaut correspondante qui lui est associée. L’utilisateur ou une application peut remplacer à tout moment l’image par défaut associée à n’importe quel curseur standard. Spécifiez une image de curseur par le biais de la fonction [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273).

Si vous avez besoin de personnaliser le curseur de la souris :

- Utilisez toujours le curseur en forme de flèche (![Curseur en forme de flèche](images/cursor-arrow.png)) pour les éléments interactifs. N’utilisez pas le curseur en forme de main (![Curseur en forme de main](images/cursor-pointinghand.png)) pour les liens ou pour d’autres éléments interactifs. À la place, utilisez les effets de pointage (décrits précédemment).
- Utilisez le curseur texte (![Curseur texte](images/cursor-text.png)) pour le texte sélectionnable.
- Utilisez le curseur de déplacement (![Curseur de déplacement](images/cursor-move.png)) lorsque l’action principale correspond à un déplacement (par exemple, un glisser-déplacer ou un rognage). N’utilisez pas le curseur de déplacement pour des éléments lorsque l’action principale correspond à une navigation (tels que les vignettes de l’écran de démarrage).
- Utilisez les curseurs de redimensionnement horizontal, vertical et diagonal (![Curseur de redimensionnement vertical](images/cursor-vertical.png), ![Curseur de redimensionnement horizontal](images/cursor-horizontal.png), ![Curseur de redimensionnement diagonal (du coin inférieur gauche au coin supérieur droit)](images/cursor-diagonal2.png), ![Curseur de redimensionnement diagonal (du coin supérieur gauche au coin inférieur droit)](images/cursor-diagonal1.png)) lorsqu’un objet est redimensionnable.
- Utilisez les curseurs en forme de main de saisie (![Curseur en forme de main de saisie (ouverte)](images/cursor-pan1.png), ![Curseur en forme de main de saisie (fermée)](images/cursor-pan2.png)) lors d’un mouvement panoramique de contenu au sein d’une zone de dessin fixe (telle qu’une carte).

## <a name="related-articles"></a>Articles connexes

- [Gestion des entrées du pointeur](handle-pointer-input.md)
- [Identification des périphériques d’entrée](identify-input-devices.md)
- [Vue d’ensemble des événements et des événements routés](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

### <a name="samples"></a>Exemples

- [Exemple d’entrée de base](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [Exemple d’entrée à faible latence](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [Exemple de mode d’interaction utilisateur](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [Exemple d’éléments visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)