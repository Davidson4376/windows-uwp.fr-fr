---
description: ''
title: Contenu en tant qu’objets
template: detail.hbs
ms.localizationpriority: medium
ms.date: 04/18/2019
ms.openlocfilehash: b73e595454121fa34b536f3672c75a5c554ee0f3
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714030"
---
# <a name="content-as-objects"></a>Contenu en tant qu’objets

 

Vous pouvez manipuler la profondeur, ou ordre de plan, des éléments pour créer une hiérarchie visuelle qui vous aide à rendre votre application plus simple à utiliser.  

> Remarque: Cet article est une première ébauche d’une nouvelle fonctionnalité de Windows 10 RS2. Les noms des fonctionnalités, la terminologie et les fonctionnalités sont sujets à des modifications 

## <a name="why-visual-hierarchy-is-important"></a>Pourquoi la hiérarchie visuelle est importante

Les utilisateurs sont en permanence bombardés de demandes visant à attirer leur attention. Ils sont constamment sollicités pour regarder chaque élément de l’écran, lire chaque chaîne de texte, cliquer sur chaque bouton. À mesure que l’environnement visuel devient plus confus et chaotique, il faut davantage d’efforts pour analyser la scène et comprendre ce qui se passe.  

C’est pourquoi il est important de sélectionner avec soin les éléments de votre interface utilisateur et de créer une disposition qui établit une hiérarchie visuelle claire entre les éléments de votre interface utilisateur. <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

Une hiérarchie visuelle claire indique aux utilisateurs quels sont les éléments les plus importants et crée des relations entre chaque élément. Avec une bonne hiérarchie visuelle, les utilisateurs comprennent la disposition de la page d’un simple coup d’œil et peuvent se concentrer sur la tâche qu'ils essayent d’exécuter. 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>Par conséquent, comment créer une hiérarchie visuelle claire ? Avec les versions antérieures de Windows 10, vous pouviez utiliser les espaces, la position et la typographie pour définir une hiérarchie visuelle. </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/flat-layout.png">Une disposition plate</a>
    
  </div>
</div>
</div>

Avec Windows 10 RS2, nous avons ajouté littéralement une autre dimension : la profondeur. 

<a href="images/content-as-objects/depth-in-layout2.png">Profondeur de la mise en page</a>


## <a name="use-depth-to-establish-a-hierarchy"></a>Utiliser la profondeur pour établir une hiérarchie 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>Vous pouvez utiliser la profondeur (ordre de plan) avec vos autres outils de conception (espace, position, typographie) pour établir une hiérarchie. Faites passer vos éléments les plus importants vers la couche supérieure ; utilisez les couches inférieures pour afficher l’interface utilisateur moins critique. 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Profondeur de la mise en page</a> 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>Comment cela fonctionne-t-il ?
> TODO : Brève description de la façon dont vous pouvez contrôler l’ordre de plan des éléments. Codez-vous explicitement en dur l’ordre de plan, ou existe-t-il un système de classement sémantique ? Comment les éléments se déplacent-ils d’une couche vers une autre ? Quelles actions le système fait-il automatiquement, et de quels éléments les concepteurs/développeurs doivent-ils se soucier ? 

## <a name="the-four-layers-of-a-typical-app-layers"></a>Les quatre couches d’une application standard

<p>Une application standard possède quatre couches.</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Au-delà d’arrière-plan</b> cette couche se trouve derrière l’application.  Lorsque des éléments sont déplacés vers cette couche, nous recommandons de les rendre non interactifs. Les éléments de cette couche ont l’effet de parallaxe le plus lent et sont fixés à la fenêtre d’application. TODO : Cette couche évolue ? 

<p>Éléments de l’arrière-plan exemple incluent image derrière le contenu, TODO : Exemple, TODO : Exemple.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">Au-delà de la couche d’une application d’arrière-plan</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Couche passif</b> il s’agit de l’application, la couche de base où se trouvent des éléments d’interface utilisateur par défaut.  Les éléments se déplacent en temps réel sur cette couche (sans effet de parallaxe), ils sont fixés à la fenêtre d’application et sont rendus à l’échelle 100 %. 

<p>Éléments de l’exemple : L’application en arrière-plan, texte, l’interface utilisateur secondaire, par exemple la navigation de l’application l’interface utilisateur.</p>
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">La couche passive d’une application</a>
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Appels à l’action</b> cette couche est pour les éléments interactifs que vous donnez la priorité au-dessus des éléments de couche passif. Les éléments sur cette couche ont un effet de parallaxe moyen et sont fixés à la fenêtre d’application. TODO : Éléments à cette échelle de la couche ou une ombre portée ?

<p>Éléments de l’exemple : listes, les grilles, les commandes principales (TODO : Such as...).</p> 
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">La couche de l’appel à l’action d’une application</a>
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>Couche de héros</b> cette couche est pour l’élément de priorité le plus élevé sur l’écran en temps.  Les éléments de cette couche peuvent dépasser les limites de la fenêtre d’application et être mis à l'échelle. Ils ont automatiquement une ombre portée.

<p>Exemples d’éléments : éléments photographiques, élément sélectionné.</p>  
  </div>
  <div class="side-by-side-content-right">
    <a href="images/content-as-objects/elements-forward-backward.png">La couche de héros d’une application</a>
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>Exemple : À déterminer
> TODO : Montrer comment adapter un modèle courant de l’interface utilisateur pour utiliser l’ordre de plan. Afficher des illustrations et du code. 

## <a name="download-the-code-samples"></a>Télécharger des exemples de code
>TODO : Lien vers des exemples qui illustrent cette fonctionnalité. 


## <a name="related-articles"></a>Articles connexes
* [Notions de base sur le contenu](../basics/content-basics.md)
