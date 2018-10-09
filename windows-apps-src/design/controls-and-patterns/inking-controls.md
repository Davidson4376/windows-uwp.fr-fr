---
author: Karl-Bridge-Microsoft
Description: Ink tools described
title: Contrôles pour l’entrée manuscrite
label: Inking Controls
template: detail.hbs
ms.author: kbridge
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 97eae5f3-c16b-4aa5-b4a1-dd892cf32ead
ms.localizationpriority: medium
ms.openlocfilehash: feac752cd076023181247742315fc43d637d0f38
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: Auto
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
ms.locfileid: "1493916"
---
# <a name="inking-controls"></a>Contrôles pour l’entrée manuscrite



Il existe deux contrôles différents qui facilitent l’entrée manuscrite dans les applications de plateforme Windows universelle (UWP): [InkCanvas](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) et [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

Le contrôle InkCanvas permet de restituer une entrée de stylet sous la forme d’un trait d’encre (via les paramètres par défaut de couleur et d’épaisseur) ou d’un trait d’effacement. Ce contrôle est une superposition transparente qui n’inclut pas d’interface utilisateur intégrée permettant de modifier les propriétés de traits d’encre par défaut.

> [!NOTE]
> InkCanvas peut être configuré pour prendre en charge des fonctionnalités similaires pour la souris et les entrées tactiles.

Comme le contrôle InkCanvas n’inclut pas de prise en charge de la modification des paramètres de trait d’encre par défaut, il peut être jumelé à un contrôle InkToolbar. InkToolbar contient une collection extensible et personnalisable de boutons activant des fonctionnalités d’entrée manuscrite dans un InkCanvas associé.

Par défaut, l’élément InkToolbar comprend des boutons pour dessiner, effacer, surligner et afficher une règle. Selon la fonctionnalité, d’autres paramètres et commandes tels que la couleur de l’encre, l’épaisseur du trait, la suppression totale, sont fournis dans un menu volant.

> [!NOTE]
> InkToolbar prend en charge les entrées effectuées à l’aide de la souris et du stylet, et peut être configuré pour reconnaître les entrées tactiles.

<img src="images/ink-tools-invoked-toolbar.png" width="300" alt="InkToolbar palette flyout">

> **API importantes**: [classe InkCanvas](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx), [classe InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx), [classe InkPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx), [Windows.UI.Input.Inking](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utilisez InkCanvas lorsque vous devez activer des fonctionnalités d’entrée manuscrite de base dans votre application sans fournir de paramètres d’entrée manuscrite à l’utilisateur.

Par défaut, les traits sont rendus sous forme d’entrée manuscrite lors de l’utilisation de la pointe du stylet (un stylo à bille noir avec une épaisseur de 2pixels) et de gomme lors de l’utilisation de la gomme. En l’absence de gomme, l’élément InkCanvas peut être configuré pour traiter l’entrée à partir de la pointe du stylet comme un trait d’effacement.

Associez InkCanvas avec un élément InkToolbar pour fournir une interface utilisateur permettant l’activation des fonctionnalités d’entrée manuscrite et la définition de propriétés d’entrée manuscrite de base, telles que la taille du trait, la couleur et la forme de la pointe du stylet.

> [!NOTE] 
> Pour une personnalisation plus complète du rendu des traits d’encre sur un élément InkCanvas, utilisez l’objet [InkPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) sous-jacent.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/InkCanvas">ouvrir l’application et voir l'objet InkCanvas en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Microsoft Edge**

Microsoft Edge utilise InkCanvas et InkToolbar pour **Notes Web**.  
![InkCanvas est utilisé pour effectuer une entrée manuscrite dans MicrosoftEdge](images/ink-tools-edge.png)

**Espace de travail WindowsInk**

Les éléments InkCanvas et InkToolbar sont également utilisés pour **bloc-croquis** et **croquis sur capture d’écran ** dans l’**Espace de travail WindowsInk**.  
![InkToolbar dans l’Espace de travail WindowsInk.](images/ink-tools-ink-workspace.png)

## <a name="create-an-inkcanvas-and-inktoolbar"></a>Créer un élément InkCanvas et InkToolbar

L’ajout d’un InkCanvas à votre application nécessite une seule ligne de balisage:

```xaml
<InkCanvas x:Name=“myInkCanvas”/>
```

> [!NOTE]
> Pour la personnalisation InkCanvas détaillée à l’aide d’InkPresenter, consultez l’article [«Interactions avec le stylo et le stylet dans les applicationsUWP»](http://windowsstyleguide/input/pen-and-stylus-interactions/).

Le contrôle InkToolbar doit être utilisé en association avec un contrôle InkCanvas. L’intégration d’un contrôle InkToolbar (et de tous les outils intégrés) dans votre application nécessite une ligne supplémentaire de balisage:

 ```xaml
<InkToolbar TargetInkCanvas=“{x:Bind myInkCanvas}”/>
 ```

Cette action affiche le contrôle InkToolbar suivant:
<img src="images/ink-tools-uninvoked-toolbar.png" width="250" alt="Basic InkToolbar">

### <a name="built-in-buttons"></a>Boutons intégrés

Le contrôle InkToolbar comprend les boutons intégrés suivants:

**Stylets**

- Stylo à bille: dessine un trait solide, opaque avec une pointe de stylet arrondie. La taille du trait dépend de la pression du stylet détectée.
- Crayon: dessine un trait semi-transparent, texturé et flou (utile pour les effets d’ombrage en couches) avec une pointe de stylet arrondie. La couleur du trait dépend de la pression du stylet détectée.
- Surligneur: dessine un trait semi-transparent avec une pointe de stylet rectangulaire.

Vous pouvez personnaliser la palette de couleurs et les attributs de taille (min, max, par défaut) dans le menu volant pour chaque stylet.

**Outil**

- Gomme: supprime tout trait d’encre touché. Notez que le trait d’encre entier est supprimé et pas seulement la partie située sous le trait de gomme.

**Bascule**

- Règle: affiche ou masque la règle. Le fait de dessiner près du bord de la règle a pour effet d’ancrer le trait d’encre à cette dernière.  
 ![Visuel de la règle associé à InkToolbar](images/inking-tools-ruler.png)

Bien qu’il s’agisse de la configuration par défaut, vous choisissez les boutons intégrés à inclure dans le contrôle InkToolbar pour votre application.

### <a name="custom-buttons"></a>Boutons personnalisés

L’élément InkToolbar se compose de deux groupes de types de boutons distincts:

1. Un groupe de boutons «outil» comprenant les boutons intégrés de dessin, d’effacement et de surlignage. Les outils et stylets personnalisés sont ajoutés ici.
> [!NOTE]
> La sélection de fonctionnalités est mutuellement exclusive.

2. Un groupe de boutons «bascule» contenant le bouton intégré de règle. Les boutons bascule personnalisés sont ajoutés ici.
> [!NOTE]
> Les fonctionnalités ne s’excluent pas mutuellement et peuvent être utilisées simultanément avec d’autres outils actifs.

En fonction de votre application et de la fonctionnalité d’entrée manuscrite requise, vous pouvez ajouter n’importe lequel des boutons suivants (liés à vos fonctionnalités d’entrée manuscrite personnalisées) au contrôle InkToolbar:

- Stylet personnalisé: un stylet dont les propriétés de palette de couleurs de l’encre et de pointe de stylet, telles que la forme, la rotation et la taille, sont définies par l’application hôte.
- Outil personnalisé: un outil autre qu’un stylet, défini par l’application hôte.
- Bascule personnalisée: définit l’état d’une fonctionnalité définie par l’application sur activé ou désactivé. Lorsqu’elle est activée, la fonctionnalité fonctionne conjointement avec l’outil actif.

> [!NOTE]
> Vous ne pouvez pas modifier l’ordre d’affichage des boutons intégrés. L’ordre d’affichage par défaut est le suivant: stylo à bille, crayon, surligneur, gomme et règle. Les stylets personnalisés sont ajoutés au dernier stylet par défaut, les boutons d’outil personnalisé sont ajoutés entre le dernier bouton de stylet et le bouton de gomme, et les boutons de bascule personnalisés sont ajoutés après le bouton de règle. (Les boutons personnalisés sont ajoutés dans l’ordre que vous avez spécifié.)

Bien que l’élément InkToolbar peut être un élément de niveau supérieur, il est généralement exposé par le biais d’un bouton «Entrée manuscrite» ou d’une commande. Nous vous recommandons d’utiliser le glyphe EE56 de la police Segoe MLD2 Assets comme une icône de niveau supérieur.

## <a name="inktoolbar-interaction"></a>Interaction de l’élément InkToolbar

Tous les boutons de stylet et d’outil intégrés incluent un menu volant où il est possible de définir des propriétés d’entrée manuscrite et de taille et de forme de pointe de stylet. «Glyphe d’extension» ![«Glyphe InkToolbar»](images/ink-tools-glyph.png) s’affiche sur le bouton pour indiquer l’existence du menu volant.

Le menu volant s’affiche quand le bouton d’un outil actif est à nouveau sélectionné. Lorsque la couleur ou la taille est modifiée, le menu volant se ferme automatiquement et permet de reprendre l’entrée manuscrite. Les outils et stylets personnalisés peuvent utiliser le menu volant par défaut ou définir un menu volant personnalisé.

La gomme dispose également d’un menu volant proposant la commande **Supprimer toutes les entrées manuscrites**.  
![InkToolbar avec appel du menu volant gomme](images/ink-tools-erase-all-ink.png)

 Pour plus d’informations sur la personnalisation et l’extensibilité, consultez l’[exemple de SimpleInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk).

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- InkCanvas et l’entrée manuscrite en règle générale sont conseillés avec un style actif. Toutefois, nous recommandons la prise en charge de l’entrée manuscrite avec une souris et la fonctionnalité tactile (y compris le stylet passif) si votre application l’impose.
- Utilisez le contrôle InkToolbar avec l’élément InkCanvas pour fournir les paramètres et les fonctionnalités d’entrée manuscrite de base. InkCanvas et InkToolbar peuvent être personnalisés par programme.
- InkToolbar et l’entrée manuscrite en règle générale sont conseillés avec un style actif. Toutefois, l’entrée manuscrite avec une souris et la fonctionnalité tactile peut être prise en charge si votre application l’impose.
- En cas de prise en charge de l’entrée manuscrite avec la fonctionnalité tactile, nous recommandons d’utiliser l’icône ED5F de la police Segoe MLD2 Assets pour le bouton «bascule», avec une info-bulle «écriture tactile».
- Dans le cas d’une sélection du trait, nous recommandons d’utiliser l’icône EF20 de la police Segoe MLD2 Assets pour le bouton «outil», avec une info-bulle «outil de sélection».
- Si vous utilisez plusieurs InkCanvas, nous recommandons l’utilisation d’un seul élément InkToolbar pour contrôler l’entrée manuscrite dans les zones de dessin.
- Pour des performances optimales, nous vous recommandons de modifier le menu volant par défaut plutôt que d’en créer un personnalisé pour les outils personnalisés et par défaut.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple SimpleInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)- Montre 8scénarios autour des fonctionnalités de personnalisation et d’extensibilité des contrôles InkCanvas et InkToolbar. Chaque scénario fournit des indications de base sur les situations courantes d’entrée manuscrite et les implémentations de contrôles.
- [Exemple de ComplexInk](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)- Montre des scénarios d’entrée manuscrite plus avancés.
- [Exemple de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles associés

- [Interactions avec le stylo et le stylet dans les applicationsUWP](http://windowsstyleguide/input/pen-and-stylus-interactions/)
- [Reconnaître les traits d’encre](http://windowsstyleguide/input/convert-ink-to-text/)
- [Stocker et récupérer des traits d’encre](http://windowsstyleguide/input/save-and-load-ink/)
