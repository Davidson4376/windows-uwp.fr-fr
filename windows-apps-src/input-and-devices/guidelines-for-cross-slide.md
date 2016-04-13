---
Description: Utilisez le glisser transversal pour prendre en charge la sélection avec le mouvement de balayage et les interactions de glissement (déplacement) avec le mouvement de glissement.
title: Recommandations en matière de glisser transversal
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
label: Glisser transversal
template: detail.hbs
---

# Recommandations en matière de glisser transversal


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


**API importantes**

-   [**CrossSliding**](https://msdn.microsoft.com/library/windows/apps/br241942)
-   [**CrossSlideThresholds**](https://msdn.microsoft.com/library/windows/apps/br241941)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

Utilisez le glisser transversal pour prendre en charge la sélection avec le mouvement de balayage et les interactions de glissement (déplacement) avec le mouvement de glissement.

## <span id="Dos_and_don_ts"> </span> <span id="dos_and_don_ts"> </span> <span id="DOS_AND_DON_TS"> </span>Pratiques conseillées et déconseillées


-   Utilisez le glisser transversal pour les listes ou les collections qui défilent dans une seule direction.
-   Utilisez le glisser transversal pour la sélection d’éléments lorsque l’interaction d’appui n’est pas possible.
-   N’utilisez pas le glisser transversal pour ajouter des éléments à une file d’attente.

## <span id="Additional_usage_guidance"> </span> <span id="additional_usage_guidance"> </span> <span id="ADDITIONAL_USAGE_GUIDANCE"> </span>Indications d’utilisation supplémentaires


La sélection et le déplacement par glissement sont possibles uniquement à l’intérieur d’une zone de contenu capable de défiler en panoramique dans une direction (verticale or horizontale). Pour que chacune de ces interactions fonctionne, l’une des directions de mouvement panoramique doit être verrouillée. De plus, le mouvement doit être effectué perpendiculairement à la direction du mouvement panoramique.

Nous démontrons ici la sélection et le glissement d’un objet à l’aide d’un glisser transversal. L’image de gauche montre comment s’effectue la sélection d’un élément si le mouvement de balayage ne dépasse aucun seuil de distance avant le relâchement du contact et la libération de l’objet. L’image de droite montre comment un mouvement de glissement dépasse un seuil de distance et aboutit au déplacement de l’objet.

![schéma illustrant les processus de sélection et de glisser-déplacer.](images/crossslide-mechanism.png)

Le schéma suivant indique les distances seuils utilisées par l’interaction de glisser transversal.

![capture d’écran illustrant les processus de sélection et de glisser-déplacer.](images/crossslide-threshold.png)

Pour conserver la fonctionnalité de mouvement panoramique, un petit seuil de 2,7 mm (environ 10 pixels en résolution cible) doit être dépassé avant qu’une interaction de sélection ou de glissement soit activée. Ce petit seuil permet au système non seulement de différencier le glisser transversal du mouvement panoramique, mais également de distinguer un appui du glisser transversal et du mouvement panoramique.

Cette image montre comment un utilisateur appuie sur un élément dans l’interface utilisateur, mais déplace son doigt légèrement vers le bas au contact. Sans seuil, cette interaction serait interprétée comme un glisser transversal en raison du mouvement vertical effectué par l’utilisateur. Grâce au seuil, le mouvement est correctement interprété en tant que mouvement panoramique horizontal.

![capture d’écran illustrant le seuil de levée des ambiguïtés de la sélection ou du glisser-déplacer.](images/crossslide-threshold2.png)

Voici quelques recommandations à prendre en compte quand vous ajoutez la fonctionnalité de glisser transversal dans votre application.

Utilisez le glisser transversal pour les listes ou les collections qui défilent dans une seule direction. Pour plus d’informations, voir la rubrique concernant l’[ajout de contrôles ListView](https://msdn.microsoft.com/library/windows/apps/hh465382).

**Remarque** Dans les cas où la zone de contenu prend en charge le mouvement panoramique dans deux directions (par exemple, des navigateurs Web ou des lecteurs électroniques), l’interaction chronométrée de la séquence Appuyer et maintenir doit être utilisée pour appeler le menu contextuel pour des objets du type image ou lien hypertexte.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![mouvement panoramique horizontal, liste à deux dimensions](images/groupedlistview1.png)                | ![mouvement panoramique vertical, liste à une dimension](images/listviewlistlayout.png)                |
| Un mouvement panoramique horizontal sur une liste à deux dimensions. Glisser vertical pour sélectionner ou déplacer un élément. | Un mouvement panoramique vertical sur une liste à une dimension. Glisser horizontal pour sélectionner ou déplacer un élément. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**Sélection**

La sélection consiste à marquer un ou plusieurs objets sans les lancer ni les activer. Cette action est comparable à celle qui consiste à cliquer avec la souris ou à utiliser la touche Maj + un clic sur un ou plusieurs objets.

La sélection par glisser transversal s’effectue en touchant un élément, puis en le relâchant après l’avoir légèrement fait glisser. Cette méthode de sélection permet de contourner le mode de sélection dédiée et l’interaction chronométrée de la séquence Appuyer et maintenir requis par d’autres interfaces tactiles, et d’éviter le conflit avec l’interaction d’appui pour l’activation.

Outre le seuil de distance, la sélection par glisser transversal est limitée à une zone de seuil de 90°, comme indiqué dans le schéma suivant. Si l’objet est glissé en dehors de cette zone, il n’est pas sélectionné.

![schéma indiquant la zone de seuil de la sélection.](images/crossslide-selection.png)

L’interaction de glisser transversal est complétée par une interaction chronométrée de type appui prolongé, également appelée interaction « auto-révélatrice ». Cette interaction supplémentaire active en effet une animation qui révèle l’action qui peut être effectuée sur l’objet. Pour plus d’informations sur l’interface utilisateur de résolution des ambiguïtés, voir [Recommandations en matière de retour visuel](guidelines-for-visualfeedback.md).

Les captures d’écran suivantes expliquent le fonctionnement de l’animation auto-révélatrice.

1.  Appuyez de manière prolongée pour démarrer l’animation pour l’interaction auto-révélatrice. L’état sélectionné de l’élément affecte ce que l’animation révèle : une coche en cas de non-sélection et aucune coche en cas de sélection.

    ![capture d’écran montrant un état de non-sélection.](images/crossslide-selfreveal1.png)

2.  Sélectionnez l’élément par un mouvement de balayage (vers le haut ou vers le bas).

    ![capture d’écran montrant l’animation d’une action de sélection.](images/crossslide-selfreveal2.png)

3.  L’élément est maintenant sélectionné. Remplacez le comportement de sélection à l’aide du mouvement de glissement pour déplacer l’élément.

    ![capture d’écran montrant l’animation d’une action de glisser-déplacer.](images/crossslide-selfreveal3.png)

Appuyez une fois pour effectuer une sélection dans une application qui ne comporte pas d’autre action principale que celle-ci. L’animation auto-révélatrice du glisser transversal s’affiche pour résoudre les ambiguïtés de cette fonctionnalité par rapport à l’interaction d’appui standard utilisée pour l’activation et la navigation.

**Panier de sélection**

Le panier de sélection est une représentation dynamique et visuellement distincte des éléments qui ont été sélectionnés dans la liste ou la collection principale de l’application. Cette fonctionnalité est utile pour suivre les éléments sélectionnés et doit être utilisée par les applications dans les cas suivants :

-   Les éléments peuvent être sélectionnés dans plusieurs emplacements.
-   De nombreux éléments peuvent être sélectionnés.
-   Une action ou une commande dépend de la liste de sélection.

Le contenu du panier de sélection est conservé quelles que soient les actions et commandes effectuées. Par exemple, si vous sélectionnez une série de photos dans une galerie de photos, appliquez une correction de couleur à chacune d’elles, puis partagez les photos avec la méthode de votre choix, les éléments restent sélectionnés.

Si le panier de sélection n’est pas utilisé dans une application, la sélection active doit être effacée après une action ou une commande. Par exemple, si vous sélectionnez un morceau de musique dans une liste de lecture pour l’évaluer, la sélection doit être effacée.

La sélection active doit également être effacée si le panier de sélection n’est pas utilisé et qu’un autre élément de la liste ou de la collection est activé. Par exemple, si vous sélectionnez un message dans votre boîte de réception, le volet de visualisation est mis à jour. Si, ensuite, vous sélectionnez un deuxième message dans votre boîte de réception, la sélection du précédent message est annulée et le volet de visualisation est mis à jour.

**Files d’attente**

Une file d’attente n’est pas comparable à la liste du panier de sélection et ne doit pas être considérée comme telle. Les principales différences sont les suivantes :

-   La liste des éléments figurant dans le panier de sélection n’est qu’une représentation visuelle. Les éléments figurant dans une file d’attente sont regroupés pour une action spécifique.
-   Les éléments ne peuvent être représentés qu’une seule fois dans le panier de sélection, alors qu’ils peuvent être représentés à plusieurs reprises dans une file d’attente.
-   L’ordre des éléments figurant dans le panier de sélection représente l’ordre de la sélection. L’ordre des éléments figurant dans une file d’attente est directement lié à la fonctionnalité.

C’est la raison pour laquelle l’interaction de sélection par glisser transversal ne doit pas être utilisée pour ajouter des éléments à une file d’attente. Pour effectuer cette opération, il est préférable d’utiliser une action de déplacement par glissement.

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**Glissement**

Utilisez l’action de glissement pour déplacer un ou plusieurs objets d’un emplacement à l’autre.

Si plusieurs objets doivent être déplacés, permettez aux utilisateurs de sélectionner plusieurs éléments, puis de tous les faire glisser en même temps.

## <span id="related_topics"> </span>Articles connexes


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
 

 






<!--HONumber=Mar16_HO1-->


