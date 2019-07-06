---
Description: Découvrez comment Fluent motion, utilise une direction et la gravité.
title: Direction et gravité - animation dans les applications UWP
label: Directionality and gravity
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f1e36f0febeeaac5a12d408d7be8a717f0ab398
ms.sourcegitcommit: 7c3b88198178d6f6a535f35e1bf8665410d41d92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569124"
---
# <a name="directionality-and-gravity"></a>Direction et gravité

Les signaux directionnels permettent de renforcer le modèle mental du parcours effectué par un utilisateur entre les différentes expériences. Il est important que la direction d'un mouvement prenne en charge à la fois la continuité de l’espace et l’intégrité des objets dans l’espace.

Le mouvement directionnel est soumis à des forces telles que la gravité. L'application de forces au mouvement renforce la sensation de naturel du mouvement.

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/category/Motion">ouvrir l’application et voir Mouvement en action </a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>Direction du mouvement

:::row:::
    :::column:::
Sens du déplacement correspond à physique mouvement. Exactement comme dans la nature, les objets peuvent se déplacer sur n'importe quel axe du monde : X, Y, Z. C'est ainsi que nous envisageons le mouvement des objets à l’écran.
Lorsque vous déplacez des objets, éviter les collisions non naturelles. N’oubliez pas où les objets proviennent et accèdent à et toujours prendre en charge les constructions de niveau supérieur qui peuvent être utilisées dans la scène, tels que de la hiérarchie de direction ou de mise en page de défilement.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Direction de navigation

La direction de navigation entre les scènes de votre application est conceptuelle. Les utilisateurs naviguent vers l’avant et vers l'arrière. Les scènes se déplacent dans et en dehors de la vue. Ces concepts se combinent avec les mouvements physiques pour guider l’utilisateur.

Lorsque la navigation fait passer un objet de la scène précédente à la nouvelle scène, l’objet effectue un simple mouvement de A vers B sur l’écran. Pour faire paraître le mouvement plus physique, l’accélération standard est ajoutée, ainsi que la sensation de gravité.

Pour la navigation vers l’arrière, le déplacement est inversé (B vers A). Lorsque l’utilisateur navigue vers l'arrière, il s'attend à retourner à l’état précédent dès que possible. Le rythme est plus rapide, plus direct et utilise la fonctionnalité de décélération.

Ici, ces principes sont appliqués lorsque l’élément sélectionné reste à l’écran pendant la navigation vers l'avant et vers l'arrière.

![Exemple d’interface utilisateur de mouvement en continu](images/continuous3.gif)

Lorsque la navigation entraîne le remplacement des éléments à l’écran, il est important de montrer où la scène existante est passée et d'où vient la nouvelle scène.

Cela présente plusieurs avantages :

- Cela renforce le modèle mental de l’utilisateur de l’espace.
- La durée de la scène existante donne plus de temps pour préparer le contenu à animer dans la scène à venir.
- Cela améliore la perception que l’utilisateur a des performances de l'application.

4 directions discrètes de navigation sont à prendre en compte.

:::row:::
    :::column:::
**Avant de** Célébrez contenu entrant de la scène d’une manière qui ne sont pas en conflit avec le contenu sortant. Contenu décélère dans la scène.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Avant-Out** contenu se ferme rapidement. Objets accélèrent hors écran.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Vers l’arrière de** identique avant, sorti mais inversée.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**À la sortie vers l’arrière** identique à transférer à la sortie, mais inversée.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Gravité

La gravité fait paraître vos expériences plus naturelles. Les objets qui se déplacent sur l'axe Z et qui ne sont pas ancrés à la scène par une affordance à l’écran sont susceptibles d’être affectés par la gravité. Quand un objet s'échappe de la scène et avant qu’il atteigne sa vitesse d’échappement, la gravité attire l’objet vers le bas, ce qui donne une courbe plus naturelle à la trajectoire de déplacement de l'objet.

La gravité se manifeste généralement lorsqu’un objet doit passer d’une scène à une autre. Pour cette raison, l’animation connectée utilise le concept de gravité.

Ici, un élément dans la rangée supérieure de la grille est affecté par la gravité, ce qui le fait tomber légèrement lorsqu'il quitte son emplacement et se déplace vers l’avant.

![Direction vers l’arrière dans](images/continuity-photos.gif)

## <a name="related-articles"></a>Articles connexes

- [Présentation de Motion](index.md)
- [Accélération et d’échéance](timing-and-easing.md)
