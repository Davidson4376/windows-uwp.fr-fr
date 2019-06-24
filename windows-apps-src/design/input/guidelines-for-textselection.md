---
Description: Cette rubrique décrit la nouvelle interface utilisateur de Windows pour la sélection et manipulation de texte, des images et des contrôles et fournit des recommandations pour l’expérience utilisateur doivent être prises en compte lors de l’utilisation de ces mécanismes de manipulation et un nouvelle sélection dans votre application UWP.
title: Sélection de texte et d’images
ms.assetid: d973ffd8-602e-47b5-ab0b-4b2a964ec53d
label: Selecting text and images
template: detail.hbs
keywords: clavier, texte, entrées, interactions avec l’utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8dab8d26436d312601b749bed7e97048ed5805bb
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317286"
---
# <a name="selecting-text-and-images"></a>Sélection de texte et d’images


Cet article décrit la sélection et la manipulation de texte, d’images et de contrôles, et fournit des recommandations en matière d’expérience utilisateur à prendre en compte lors de l’utilisation de ces mécanismes dans vos applications.

> **API importantes** : [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)
 


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez des glyphes de police lors de l’implémentation de votre propre interface utilisateur de barre de redimensionnement. Le symbole de sélection est une combinaison de deux polices Segoe UI disponibles à l’échelle du système. L’utilisation de ressources de police simplifie les problèmes de rendu pour différentes valeurs de ppp et fonctionne bien avec les divers plateaux d’échelle d’interface utilisateur. Lorsque vous implémentez vos propres barres de redimensionnement, elles doivent partager les caractéristiques d’interface utilisateur suivantes :

    -   forme circulaire,
    -   visible par rapport à tout arrière-plan,
    -   taille cohérente.
-   Fournissez une marge autour du contenu sélectionnable pour prendre en compte l’interface utilisateur de la barre de redimensionnement. Si votre application permet la sélection de texte dans une région qui n’effectue pas de panoramique/défilement, prévoyez une marge large d’une demi barre de redimensionnement sur les côtés gauche et droit de la zone de texte et une marge haute d’une barre de redimensionnement en haut et en bas de la zone de texte (comme illustré dans les images suivantes). Cela garantit que l’interface utilisateur complète de barre de redimensionnement soit exposée à l’utilisateur et réduit les interactions involontaires avec d’autres éléments d’interface utilisateur latéraux.

    ![marges de redimensionnement du texte sélectionné](images/textselection-gripper-margins.png)

-   Masquez l’interface utilisateur de redimensionnement au cours d’une interaction. Élimine l’occlusion par les symboles de sélection/redimensionnement au cours de l’interaction. Cela est utile lorsqu’une barre de redimensionnement n’est pas complètement masquée par le doigt ou dans le cas de plusieurs barres de redimensionnement de sélection de texte. Cela permet d’éliminer les artefacts visuels durant l’affichage de fenêtres enfants.

-   N’autorisez pas la sélection d’éléments d’interface utilisateur tels que des contrôles, des étiquettes, des images, des contenus propriétaires, etc. En règle générale, les applications Windows permettent cette sélection uniquement au sein de contrôles spécifiques. Les contrôles comme les boutons, les étiquettes et les logos ne peuvent pas être sélectionnés. Déterminez si la sélection représente un problème pour votre application et, le cas échéant, identifiez les zones de l’interface utilisateur dans lesquelles la sélection doit être interdite. 

## <a name="additional-usage-guidance"></a>Indications d’utilisation supplémentaires


La sélection et la manipulation de texte représentent des difficultés particulières pour l’utilisateur en termes d’interactions tactiles. Les entrées effectuées par le biais de la souris, du stylo/stylet et du clavier sont des opérations extrêmement granulaires : un clic de souris ou un contact du stylo/stylet est généralement associé à un pixel unique, et une touche est appuyée ou non. Une entrée tactile n’est pas granulaire ; il est difficile d’associer la totalité de la surface de la pointe du doigt à un emplacement x-y spécifique sur l’écran pour placer avec précision un curseur de texte.

**Considérations et recommandations**

Utilisez les contrôles intégrés, exposées via les infrastructures de langage dans Windows pour créer des applications qui fournissent l’expérience d’interaction de l’utilisateur une plateforme complète, y compris les comportements de sélection et la manipulation. La fonctionnalité d’interaction des contrôles intégrés se révèle suffisante pour la plupart des applications UWP.

Lorsque vous utilisez des contrôles de texte UWP standard, vous ne pouvez pas personnaliser les comportements et les éléments visuels de sélection décrits dans cette rubrique.

**Sélection de texte**

Si votre application nécessite une interface utilisateur personnalisée qui prend en charge la sélection de texte, nous vous recommandons de suivre les comportements de sélection de Windows décrits ici.

**Contenu modifiable et non modifiables**


Avec l’entrée tactile, les interactions de sélection sont principalement effectuées par des gestes (par exemple, un appui pour placer un curseur d’insertion ou sélectionner un mot, et un glissement pour modifier une sélection). Comme avec d’autres Windows les interactions tactiles, chronométrées interactions sont limitées à la presse et maintenez le mouvement à afficher l’interface utilisateur d’information. Pour plus d’informations, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md).

Windows reconnaît deux états possibles pour les interactions de sélection, modifiables et non modifiables et ajuste la sélection de l’interface utilisateur, les commentaires et les fonctionnalités en conséquence.

**Contenu modifiable**

Un appui dans la moitié gauche d’un mot place le curseur immédiatement à gauche du mot, tandis qu’un appui dans la moitié droite le placement immédiatement à droite du mot.

L’image suivante montre comment placer un curseur d’insertion initial avec une barre de redimensionnement en appuyant à côté du début ou de la fin d’un mot.

![Appuyez (ou effectuez un appui prolongé) sur le côté gauche du mot pour placer un curseur et une barre de redimensionnement au début du mot. Appuyez (ou effectuez un appui prolongé) sur le côté droit du mot pour placer un curseur et une barre de redimensionnement à la fin du mot.](images/textselection-place-caret.png)

L’image suivante montre comment ajuster une sélection en faisant glisser la barre de redimensionnement.

![Faites glisser la barre de redimensionnement dans n’importe quelle direction pour ajuster la sélection (la première barre de redimensionnement reste ancrée et une deuxième barre de redimensionnement s’affiche). Faites glisser l’une des deux barres pour effectuer d’autres ajustements.](images/adjust-selection.png)

Les images suivantes montrent comment appeler le menu contextuel en appuyant dans la sélection ou sur une barre de redimensionnement (l’appui prolongé peut aussi être utilisé).

![Appuyez (ou effectuez un appui prolongé) dans la sélection ou sur un symbole de sélection/redimensionnement pour appeler le menu contextuel.](images/textselection-show-context.png)

**Remarque**  ces interactions diffèrent légèrement dans le cas d’un mot mal orthographié. L’appui sur un mot qui est marqué comme incorrectement orthographié entraîne la mise en surbrillance de la totalité du mot et l’appel du menu contextuel d’orthographe suggérée.

 

**Contenu non modifiables**

L’image suivante montre comment sélectionner un mot en appuyant au sein de celui-ci (aucun espace n’est inclus dans la sélection initiale).

![Appuyez dans un mot pour le sélectionner (aucun espace n’est inclus dans la sélection initiale).](images/select-word.png)

Suivez les mêmes procédures que pour le texte modifiable pour ajuster la sélection et afficher le menu contextuel.

**Manipulation des objets**

Chaque fois que cela est possible, utilisez les mêmes ressources de symbole de redimensionnement (ou des ressources similaires) que la sélection de texte lors de l’implémentation d’une manipulation d’objet personnalisée dans une application UWP. Cela permet d’offrir une expérience d’interaction cohérente sur toute la plateforme.

Par exemple, il est possible également d’utiliser des barres de redimensionnement dans des applications de traitement d’image prenant en charge le redimensionnement et le rognage ou dans des applications de lecteur multimédia fournissant des barres de progression réglables, comme illustré dans les images suivantes.

![lecteur multimédia avec barre de progression](images/gripper-mediaplayer.png)

*Lecteur multimédia avec barre de progression réglable.*

![image avec poignées de rognage](images/gripper-imagemanip.png)

*Éditeur d’images avec les barres de défilement de rognage.*

## <a name="related-articles"></a>Articles associés



**pour les développeurs**
* [Interactions de l’utilisateur personnalisé](https://docs.microsoft.com/windows/uwp/design/layout/index)

**Exemples**
* [Exemple d’entrée de base](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemple d’entrée à faible latence](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemple de mode d’interaction utilisateur](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Exemple d’éléments visuels de focus](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemples d’archive**
* [Entrée : Exemple d’événements d’entrée utilisateur XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrée : Exemples de fonctionnalités d’appareil](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrée : Exemple de test d’atteinte tactile](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML de défilement, panoramique et zoom d’exemple](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrée : Exemple d’entrée manuscrite simplifiée](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrée : Exemple de mouvements de Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrée : Manipulations et exemple de mouvements (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Exemple d’entrée tactile de DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




