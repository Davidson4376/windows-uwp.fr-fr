---
author: Jwmsft
Description: "Un contrôle de zoom sémantique permet à l’utilisateur d’effectuer un zoom entre deux affichages sémantiques différents du même jeu de données."
title: "Zoom sémantique"
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 47e39290a63408fc66783617ad2f12345eab2fa3

---

# Zoom sémantique



Le contrôle zoom sémantique permet à l’utilisateur de zoomer entre deux vues du même contenu pour pouvoir rapidement naviguer dans un jeu de données volumineux. La vue avec zoom avant est la vue principale du contenu. Vous affichez le jeu de données complet dans cette vue. La vue avec zoom arrière est une vue plus générale du même contenu. En règle générale, vous affichez les en-têtes de groupe pour un jeu de données groupé dans cette vue. Par exemple, lorsque l’utilisateur affiche son carnet d’adresses, il peut faire un zoom avant sur une lettre pour voir tous les noms associés à cette lettre. 

**API importantes**

-   [**Classe SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601)

**Fonctionnalités** :

-   La taille de la vue avec zoom arrière est restreinte par les limites du contrôle de zoom sémantique.
-   Appuyer sur un en-tête de groupe permet de basculer entre les vues. Il est possible d’activer le pincement pour basculer entre les vues.
-   Les en-têtes actifs basculent entre les vues.

## Exemples

Zoom sémantique utilisé dans l’application Photos.

![Zoom sémantique utilisé dans l’application Photos](images/control-examples/semantic-zoom-photos.png)

Un carnet d’adresses est un exemple de jeu de données dont la navigation peut être considérablement simplifiée grâce à un contrôle de zoom sémantique. L’un des affichages correspond à la vue d’ensemble complète et alphanumérique des personnes figurant dans le carnet d’adresses (image de gauche), tandis que la vue avec zoom avant affiche les données dans l’ordre et avec plus de détails (image de droite).

![Exemple de zoom sémantique utilisé dans une liste de contacts](images/semanticzoom-win10.png)

## Recommandations

-   Lorsque vous utilisez le zoom sémantique dans votre application, veillez à ce que la disposition et le sens du mouvement panoramique de l’élément ne changent pas en fonction du niveau de zoom. Les dispositions et les interactions de mouvement panoramique doivent être cohérentes et prévisibles dans tous les niveaux de zoom.
-   Comme le zoom sémantique permet aux utilisateurs de passer rapidement d’un élément de contenu à un autre, limitez à trois le nombre de pages/d’écrans du mode d’affichage général. Un mouvement panoramique trop important réduit la fonctionnalité du zoom sémantique.
-   Évitez d’utiliser le zoom sémantique pour modifier l’étendue du contenu. Par exemple, il convient de ne pas basculer un album photo vers un affichage des dossiers dans l’Explorateur de fichiers.
-   Utilisez une structure et une sémantique qui sont essentielles à la vue.
-   Utilisez des noms de groupe pour les éléments d’une collection groupée.
-   Utilisez un ordre de tri pour une collection non groupée, mais triée, par exemple l’ordre chronologique pour des dates ou l’ordre alphabétique pour une liste de noms.



## Articles connexes

* [Recommandations en matière d’interactions utilisateur courantes](https://dev.windows.com/design/inputs-devices)


**Exemples (XAML)**
* [Entrée : exemples d’événement d’entrée utilisateur XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Exemple de zoom, de panoramique et de défilement XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)

**Exemples (DirectX)**
* [Exemple d’entrée tactile DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
* [Entrée : exemple de manipulations et de mouvements (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
 

 







<!--HONumber=Jun16_HO4-->


