---
Description: Créez des applications de plateforme Windows universelle (UWP) offrant des expériences d’interaction utilisateur intuitives et distinctives qui sont optimisées pour l’entrée tactile, mais cohérentes du point de vue du fonctionnement entre les périphériques d’entrée.
title: Recommandations en matière de conception pour l’interface tactile
ms.assetid: 3250F729-4FDD-4AD4-B856-B8BA575C3375
label: Recommandations en matière de conception pour l’interface tactile
template: detail.hbs
---

# Recommandations en matière de conception pour l’interface tactile


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x articles, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Créez des applications de plateforme Windows universelle (UWP) offrant des expériences d’interaction utilisateur intuitives et distinctives qui sont optimisées pour l’entrée tactile, mais cohérentes du point de vue du fonctionnement entre les périphériques d’entrée.

## <span id="Dos_and_don_ts"> </span> <span id="dos_and_don_ts"> </span> <span id="DOS_AND_DON_TS"> </span>Pratiques conseillées et déconseillées


-   Concevez des applications en utilisant l’interaction tactile comme méthode d’entrée principale.
-   Fournissez un retour visuel pour les interactions de tous types (entrée tactile, stylo, stylet, souris, etc.)
-   Optimisez le ciblage en ajustant la taille de la cible tactile, la géométrie de contact, ainsi que les mouvements de frottement et de va-et-vient.
-   Optimisez la précision grâce à l’utilisation de points d’ancrage et de « rails » d’orientation.
-   Fournissez des info-bulles et des poignées pour améliorer la précision tactile quand les éléments d’interface sont serrés entre eux.
-   Évitez dans la mesure du possible d’utiliser des interactions chronométrées (exemple d’utilisation appropriée : maintenir appuyé).
-   Évitez d’utiliser le nombre de doigts servant à distinguer la manipulation.

## <span id="Additional_usage_guidance"> </span> <span id="additional_usage_guidance"> </span> <span id="ADDITIONAL_USAGE_GUIDANCE"> </span>Indications d’utilisation supplémentaires


Avant tout, concevez votre application en ayant comme objectif que l’entrée tactile sera la principale méthode d’entrée de vos utilisateurs. Si vous utilisez les contrôles de la plateforme, la prise en charge du pavé tactile, de la souris et du stylo/stylet ne demande pas plus de programmation car Windows 8 fournit cela gratuitement.

Sachez toutefois qu’une interface utilisateur optimisée pour les entrées tactiles ne s’avère pas toujours supérieure à une interface utilisateur classique. Les deux présentent des avantages et des inconvénients qui sont propres à une technologie et une application. Quand on s’achemine vers une interface utilisateur principalement tactile, il est important de connaître les différences fondamentales qui existent entre les différentes entrées : tactile (y compris le pavé tactile), stylo/stylet, souris et clavier. Ne considérez pas les propriétés et les comportements familiers des périphériques d’entrée comme acquis car, dans Windows 8, la fonction tactile ne se limite pas à une simple émulation de cette fonctionnalité.

Les recommandations suivantes vous permettront de découvrir que l’entrée tactile exige une approche différente de la conception de l’interface utilisateur.

**Comparer les critères de l’interaction tactile**

Le tableau suivant présente certaines différences qui existent entre les périphériques d’entrée dont vous devez tenir compte quand vous concevez des applications du Windows Store optimisées pour l’interaction tactile.

Facteur
Interactions tactiles
Interactions à l’aide de la souris, du clavier, du stylo/stylet
Pavé tactile
Précision
La zone de contact au bout du doigt est plus importante qu’une simple coordonnées x-y, ce qui augmente le risque d’activations involontaires de commandes.
La souris et le stylo/stylet répondent à une coordonnée x-y précise.
Comme la souris.
La forme de la zone de contact change tout au long du mouvement.
Les mouvements de la souris et les traits du stylo/stylet répondent à des coordonnées x-y précises. Le focus du clavier est explicite.
Comme la souris.
Il n’y a pas de curseur de souris pour aider au ciblage.
Le curseur de la souris, le curseur du stylo/stylet et le focus du clavier constituent tous une aide au ciblage.
Comme la souris.
Anatomie humaine
Les mouvements effectués avec le bout du doigt sont imprécis, car le traçage d’une ligne droite avec un ou plusieurs doigts est difficile à réaliser. Cela s’explique par la courbure des articulations de la main et le nombre d’articulations impliquées dans le mouvement.
Il est plus facile de tracer un mouvement de ligne droite avec la souris ou le stylo/stylet, car la main qui les contrôle parcourt une distance plus courte que le curseur sur l’écran.
Comme la souris.
Certaines zones situées sur la surface tactile d’un périphérique d’affichage peuvent être difficiles à atteindre en raison de la posture des doigts et de la prise en main du périphérique par l’utilisateur.
La souris et le stylo/stylet peuvent accéder à toutes les parties de l’écran, et n’importe quel contrôle est accessible par le clavier via l’ordre des onglets.
La posture des doigts et la prise en main peuvent poser problème.
Le bout des doigts ou la main de l’utilisateur peuvent masquer des objets. C’est ce que l’on appelle l’« occlusion ».
Les périphériques d’entrée indirects ne provoquent pas d’occlusion.
Comme la souris.
État de l’objet
L’interaction tactile utilise un modèle à deux états : la surface tactile du périphérique d’affichage est touchée (activée) ou non touchée (désactivée) par l’utilisateur. Il n’existe pas d’état de pointage susceptible de déclencher un retour visuel supplémentaire.
Une souris, un stylo/stylet et un clavier exposent tous un modèle à trois états : soulevé (activé), appuyé (activé) et pointé (focus).

Le pointage permet à l’utilisateur d’explorer et de découvrir les éléments à l’aide d’info-bulles associées aux éléments de l’interface utilisateur. Les effets de pointage et de focus peuvent transmettre les objets qui sont interactifs et aident également au ciblage.

Comme la souris.
Interaction évoluée
Prend en charge l’interaction tactile multipoint : plusieurs points d’entrée (bout des doigts) sur une surface tactile.
Prend en charge un point d’entrée unique.
Comme l’entrée tactile.
Prend en charge la manipulation directe des objets par le biais de gestes tels que l’appui, le glissement, le pincement et la rotation.
Ne prend pas en charge la manipulation directe, car la souris, le stylo/stylet et le clavier sont des périphériques d’entrée indirects.
Comme la souris.
 

**Remarque**  
L’entrée indirecte a bénéficié de plus de 25 ans d’amélioration. Les fonctions comme les info-bulles déclenchées par le pointage ont été conçues pour résoudre les problèmes d’exploration de l’interface utilisateur spécifiques aux entrées à l’aide du pavé tactile, de la souris, du stylo/stylet et du clavier. Les fonctionnalités d’interface utilisateur de ce genre ont été repensées pour enrichir l’expérience de la saisie tactile, sans compromettre l’expérience utilisateur sur les autres appareils.

 

**Utiliser le retour tactile**

Les retours visuels appropriés au cours des interactions avec votre application aident les utilisateurs à reconnaître, à apprendre et à s’adapter à l’interprétation des leurs interactions par l’application et par Windows 8. Le retour visuel peut indiquer les interactions réussies, transmettre l’état du système, améliorer le sentiment de contrôle, réduire les erreurs, aider les utilisateurs à comprendre le système et le périphérique d’entrée et encourager l’interaction.

Le retour visuel est essentiel quand l’utilisateur doit réaliser, avec la fonction tactile, des activités qui demandent de l’exactitude et de la précision selon l’endroit concerné. Affichez le retour, peu importe où et quand l’entrée tactile est détectée, pour aider l’utilisateur à comprendre toutes les méthodes de ciblage personnalisé qui sont définies par votre application et ses contrôles.

**Créer une expérience d’interaction immersive**

Les techniques suivantes améliorent l’expérience immersive des applications du Windows Store.

**Ciblage**

Le ciblage est optimisé par les éléments suivants :

-   Taille des cibles tactiles

    Des instructions claires concernant les tailles garantissent une interface utilisateur confortable contenant des objets et des contrôles que l’utilisateur peut cibler facilement et en toute sécurité.

-   Géométrie de contact

    La totalité de la zone de contact du doigt détermine l’objet cible le plus probable.

-   Frottement

    L’utilisateur peut facilement recibler les éléments au sein d’un groupe en glissant le doigt entre eux (par exemple, des cases d’option). L’élément actif est activé lorsque l’utilisateur relâche le doigt.

-   Va-et-vient

    L’utilisateur peut facilement recibler des éléments compacts (par exemple, des liens hypertexte) en appuyant avec le doigt et, sans le faire glisser, en effectuant un mouvement de va-et-vient sur les éléments. Pour éviter l’occlusion, l’élément est identifié par une info-bulle ou la barre d’état. Il est activé dès que l’utilisateur relâche le doigt.

**Précision**

Pour les interactions imprécises, utilisez :

-   des points d’ancrage qui permettent à l’utilisateur de s’arrêter plus facilement aux emplacements souhaités quand il interagit avec le contenu ;
-   des « rails » d’orientation qui permettent d’aider l’utilisateur à effectuer un mouvement panoramique vertical ou horizontal, même si la main se déplace avec un léger mouvement d’arc. Pour plus d’informations, voir [Recommandations en matière de mouvement panoramique](guidelines-for-panning.md).

**Occlusion**

Pour éviter l’occlusion du doigt et de la main, respectez les recommandations suivantes :

-   Taille et positionnement des éléments d’interface utilisateur

    Créez des éléments d’interface utilisateur suffisamment grands pour qu’ils ne soient pas complètement recouverts par la zone de contact du doigt.

    Positionnez autant que possible les menus et les fenêtres indépendantes au-dessus de la zone de contact.

-   Info-bulles

    Affichez des info-bulles quand un utilisateur maintient son doigt sur un objet. Cela est utile pour décrire la fonctionnalité d’un objet. L’utilisateur peut retirer le bout de son doigt de l’objet pour éviter d’appeler l’info-bulle.

    Pour les petits objets, décalez les info-bulles afin qu’elles ne soient pas recouvertes par la zone de contact du doigt. Cela permet d’améliorer le ciblage.

-   Poignées de précision

    Pour les actions de précision (par exemple, la sélection de texte), insérez des poignées de sélection décalées afin d’augmenter le degré d’exactitude. Pour plus d’informations, voir [Recommandations en matière de sélection de texte et d’images (applications Windows Runtime)](guidelines-for-textselection.md).

**Chronométrage**

Évitez les modifications en mode chronométré au profit de la manipulation directe. Celle-ci simule le maniement direct et en temps réel d’un objet. L’objet réagit directement au mouvement du doigt.

À l’inverse, en mode chronométré, l’interaction se produit après le geste tactile. Généralement, les interactions chronométrées dépendent de seuils invisibles, tels que le temps, la distance ou la vitesse, pour déterminer la commande à effectuer. Elles ne produisent aucun retour visuel tant que le système n’a pas effectué l’action.

La manipulation directe offre un certain nombre d’avantages par rapport aux interactions chronométrées :

-   Le retour visuel instantané au cours de l’interaction permet à l’utilisateur de se sentir davantage impliqué, confiant et en contrôle.
-   Les manipulations directes permettent de sécuriser l’exploration d’un système, car elles sont réversibles, c’est-à-dire que l’utilisateur peut facilement revenir en arrière et annuler ses actions d’une manière logique et intuitive.
-   Les interactions qui affectent directement les objets et qui imitent les gestes réels sont plus intuitives, plus visibles et plus faciles à retenir. Elles ne dépendent pas d’interactions obscures ou abstraites.
-   Les interactions chronométrées peuvent être difficiles à effectuer, étant donné que l’utilisateur doit atteindre des seuils arbitraires et invisibles.

En outre, nous vous encourageons vivement à tenir compte des recommandations suivantes :

-   Ne classez pas les manipulations en fonction du nombre de doigts utilisés.
-   Les interactions doivent prendre en charge les manipulations composées. Par exemple, resserrez les doigts pour zoomer tout en les faisant glisser pour effectuer un mouvement panoramique.
-   Ne classez pas les interactions en fonction du temps. Une même interaction doit avoir le même résultat, quel que soit le temps pris pour l’effectuer. Les activations temporelles impliquent des délais obligatoires à respecter par l’utilisateur. Par ailleurs, elles portent atteinte non seulement à la nature immersive des manipulations directes, mais également à la perception de la réactivité du système.

    **Remarque** Il existe une exception à cette règle : quand vous utilisez des interactions chronométrées à titre d’aide à l’apprentissage et à l’exploration (par exemple, l’appui prolongé).

     

-   Les descriptions appropriées et les signaux visuels influent très favorablement sur l’utilisation des interactions avancées.

## <span id="related_topics"> </span>Articles connexes

**Pour les développeurs (XAML)**
* [Interactions tactiles](https://msdn.microsoft.com/library/windows/apps/mt185617)
* [Interactions utilisateur personnalisées](https://msdn.microsoft.com/library/windows/apps/mt185599)
 

 




<!--HONumber=Mar16_HO1-->
