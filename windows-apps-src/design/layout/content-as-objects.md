---
description: ''
title: Contenu en tant qu’objets
template: detail.hbs
ms.localizationpriority: medium
ms.openlocfilehash: 37ba5093f2d7cfe268be40413b889801daf00967
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8898677"
---
# <a name="content-as-objects"></a>Contenu en tant qu’objets

 

Vous pouvez manipuler la profondeur, ou ordre de plan, des éléments pour créer une hiérarchie visuelle qui vous aide à rendre votre application plus simple à utiliser.  

> Remarque: Cet article est une version préliminaire pour une nouvelle fonctionnalité de Windows10 RS2. Les noms des fonctionnalités, la terminologie et les fonctionnalités sont sujets à des modifications 

## <a name="why-visual-hierarchy-is-important"></a>Pourquoi la hiérarchie visuelle est importante

Les utilisateurs sont en permanence bombardés de demandes visant à attirer leur attention. Ils sont constamment sollicités pour regarder chaque élément de l’écran, lire chaque chaîne de texte, cliquer sur chaque bouton. À mesure que l’environnement visuel devient plus confus et chaotique, il faut davantage d’efforts pour analyser la scène et comprendre ce qui se passe.  

C’est pourquoi il est important de sélectionner avec soin les éléments de votre interface utilisateur et de créer une disposition qui établit une hiérarchie visuelle claire entre les éléments de votre interface utilisateur. <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

Une hiérarchie visuelle claire indique aux utilisateurs quels sont les éléments les plus importants et crée des relations entre chaque élément. Avec une bonne hiérarchie visuelle, les utilisateurs comprennent la disposition de la page d’un simple coup d’œil et peuvent se concentrer sur la tâche qu'ils essayent d’exécuter. 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>Par conséquent, comment créer une hiérarchie visuelle claire? Avec les versions antérieures de Windows10, vous pouviez utiliser les espaces, la position et la typographie pour définir une hiérarchie visuelle. </p>
  </div>
  <div class="side-by-side-content-right">
    ![Disposition plate](images/content-as-objects/flat-layout.png)
    
  </div>
</div>
</div>

Avec Windows10 RS2, nous avons ajouté littéralement une autre dimension: la profondeur. 

![Profondeur de disposition](images/content-as-objects/depth-in-layout2.png)


## <a name="use-depth-to-establish-a-hierarchy"></a>Utiliser la profondeur pour établir une hiérarchie 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>Vous pouvez utiliser la profondeur (ordre de plan) avec vos autres outils de conception (espace, position, typographie) pour établir une hiérarchie. Faites passer vos éléments les plus importants vers la couche supérieure; utilisez les couches inférieures pour afficher l’interface utilisateur moins critique. 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    ![Profondeur de disposition](images/content-as-objects/elements-forward-backward.png) 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>Comment cela fonctionne-t-il?
> TODO: Brève description de la façon dont vous pouvez contrôler l’ordre de plan des éléments. Codez-vous explicitement en dur l’ordre de plan, ou existe-t-il un système de classement sémantique? Comment les éléments se déplacent-ils d’une couche vers une autre? Quelles actions le système fait-il automatiquement, et de quels éléments les concepteurs/développeurs doivent-ils se soucier? 

## <a name="the-four-layers-of-a-typical-app-layers"></a>Les quatre couches d’une application standard

<p>Une application standard possède quatrecouches.</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Au-delà de l’arrière-plan** <br/>
Cette couche se trouve derrière l’application.  Lorsque des éléments sont déplacés vers cette couche, nous recommandons de les rendre non interactifs. Les éléments de cette couche ont l’effet de parallaxe le plus lent et sont fixés à la fenêtre d’application. TODO: Cette couche est-elle mise à l’échelle? 

<p>Des exemples d’éléments d’arrière-plan incluent une image derrière du contenu. TODO: Exemple, TODO: Exemple.</p>
  </div>
  <div class="side-by-side-content-right">
    ![Couche Au-delà de l’arrière-plan d’une application](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Couche passive** <br/>
Il s’agit de la couche de base de l’application, où les éléments d’interface utilisateur se trouvent par défaut.  Les éléments se déplacent en temps réel sur cette couche (sans effet de parallaxe), ils sont fixés à la fenêtre d’application et sont rendus à l’échelle 100%. 

<p>Exemples d’éléments: arrière-plan d’application, texte, interface utilisateur secondaire, telle que l’interface utilisateur de navigation de l’application.</p>
  </div>
  <div class="side-by-side-content-right">
    ![Couche passive d’une application](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Appels à l’action** <br/>
Cette couche est destinée aux éléments interactifs auxquels vous donnez la priorité par rapport aux éléments de la couche passive. Les éléments sur cette couche ont un effet de parallaxe moyen et sont fixés à la fenêtre d’application. TODO: Les éléments de cette couche sont-ils mis à l’échelle ou ont-ils une ombre portée?

<p>Exemples d'éléments: listes, grilles, commandes principales (TODO: comme...).</p> 
  </div>
  <div class="side-by-side-content-right">
    ![Couche Appel à l'action d’une application](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Couche principale** <br/>
Cette couche est réservée à l’élément ayant la priorité la plus élevé sur l’écran à un moment donné.  Les éléments de cette couche peuvent dépasser les limites de la fenêtre d’application et être mis à l'échelle. Ils ont automatiquement une ombre portée.

<p>Exemples d’éléments: éléments photographiques, élément sélectionné.</p>  
  </div>
  <div class="side-by-side-content-right">
    ![Couche principale d’une application](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>Exemple: TBD
> TODO: Indiquer comment adapter un modèle d’interface utilisateur courant pour utiliser l’ordre de plan. Afficher des illustrations et du code. 

## <a name="download-the-code-samples"></a>Télécharger des exemples de code
>TODO: Lien vers des exemples qui illustrent cette fonctionnalité. 


## <a name="related-articles"></a>Articles associés
* [Notions de base sur le contenu](../basics/content-basics.md)
