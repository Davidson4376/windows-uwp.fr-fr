---
title: Copie et consultation des données des ressources
description: Les indicateurs d’utilisation spécifient la manière dont l’application va utiliser les données de ressources, de manière à placer les ressources dans la zone de mémoire la plus performante possible. Les données de ressource sont copiées dans les ressources de sorte que l'UC ou le processeur graphique puisse y accéder sans affecter les performances.
ms.assetid: 6A09702D-0FF2-4EA6-A353-0F95A3EE34E2
keywords:
- Copie et consultation des données des ressources
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e7b0f06711b4a908f8990dfb16968400c685c15f
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5874115"
---
# <a name="copying-and-accessing-resource-data"></a>Copie et consultation des données des ressources


Les indicateurs d’utilisation spécifient la manière dont l’application va utiliser les données de ressources, de manière à placer les ressources dans la zone de mémoire la plus performante possible. Les données de ressource sont copiées dans les ressources de sorte que l'UC ou le processeur graphique puisse y accéder sans affecter les performances.

Il n’est pas nécessaire de considérer que les ressources ont été créées dans la mémoire vidéo ou la mémoire système, ou de décider si le runtime doit ou non gérer la mémoire. Avec l’architecture du WDDM (WindowsDisplayDriverModel), les applications créent des ressources Direct3D avec différents indicateurs d’utilisation afin d'indiquer la manière dont l’application va utiliser les données de ressources. Ce modèle de pilote virtualise la mémoire utilisée par les ressources; il incombe au gestionnaire de système d’exploitation/pilotes/mémoire de placer les ressources dans la zone de mémoire la plus performante au vu de l’utilisation prévue.

Le scénario par défaut implique que les ressources soient disponibles pour le processeur graphique. Il arrive que les données de ressources doivent être disponibles pour le processeur graphique. Copier des données de ressources afin que le processeur approprié puisse y accéder sans affecter les performances requiert un minimum de connaissances sur le fonctionnement des méthodes d’API.

## <a name="span-idcopyingspanspan-idcopyingspanspan-idcopyingspancopying-resource-data"></a><span id="Copying"></span><span id="copying"></span><span id="COPYING"></span>Copie des données de ressource


Les ressources sont créées dans la mémoire lorsque Direct3D exécute un appel de création. Elles peuvent être créées dans la mémoire vidéo, la mémoire système ou tout autre type de mémoire. Étant donné que le modèle de pilote WDDM virtualise cette mémoire, les applications n’ont plus besoin de suivre le type de ressources de mémoire qui y sont créées.

Dans l’idéal, toutes les ressources se trouvent dans la mémoire vidéo afin que le processeur graphique puisse y accéder immédiatement. Toutefois, il peut arriver que l'UC doive lire les données de ressources ou que le processeur graphique ait besoin d'accéder aux données de ressource dans lesquelles a écrit l'UC. Direct3D gère ces différents scénarios en demandant à l’application de spécifier une utilisation, et propose plusieurs méthodes permettant de copier les données de ressources lorsque cela est nécessaire.

Selon la manière dont est créée la ressource, il n’est pas toujours possible d’accéder directement aux données sous-jacentes. Cela peut signifier que les données de ressource doivent être copiées de la ressource source vers une autre ressource à laquelle peut accéder le processeur approprié. Pour Direct3D, les ressources par défaut peuvent être accessibles directement par le processeur graphique, tandis que l'UC peut accéder directement aux ressources dynamiques et de transfert.

Une fois qu’une ressource a été créée, son utilisation ne peut pas être modifiée. Au lieu de cela, vous devez copier le contenu d’une ressource dans une autre ressource qui a été créée pour une autre utilisation. Vous devez copier des données de ressource d’une ressource vers une autre, ou copier les données de la mémoire vers une ressource.

Il existe deux principaux types de ressources: configurables et non configurables. Les ressources créées avec des utilisations dynamiques ou de transfert sont configurables, tandis que les ressources créées avec les utilisations immuables ou par défaut sont non configurables.

La copie de données entre des ressources non configurables est extrêmement rapide puisqu'il s'agit du scénario le plus courant qui, à ce titre, a été optimisé pour produire de bonnes performances. Dans la mesure où ces ressources ne sont pas directement accessibles par l’UC, elles sont optimisées pour que le processeur graphique puisse les manipuler rapidement.

La copie de données entre des ressources configurables est plus problématique, car les performances dépendent de l’utilisation avec laquelle la ressource a été créée. Par exemple, le processeur graphique peut lire une ressource dynamique assez rapidement, mais ne peut pas écrire sur cette ressource; de même, le processeur graphique ne peut pas lire ou écrire directement dans des ressources de transfert.

Les applications qui souhaitent copier des données à partir d’une ressource associée à une utilisation par défaut dans une ressource associée à une utilisation de transfert (pour permettre à l'UC de lire les données, qui implique le problème de collationnement du processeur graphique) doivent procéder avec précaution. Voir la rubrique [Accès aux données de ressource](#accessing)ci-dessous.

## <a name="span-idaccessingspanspan-idaccessingspanspan-idaccessingspanaccessing-resource-data"></a><span id="Accessing"></span><span id="accessing"></span><span id="ACCESSING"></span>Accès aux données de ressources


L'accès à une ressource nécessite un mappage de la ressource; un mappage signifie que l’application tente de donner à l'UC un accès à la mémoire. Mapper une ressource afin que le processeur puisse accéder à la mémoire sous-jacente peut provoquer des goulots d’étranglement, c'est pourquoi vous devez faire preuve de prudence quant à la manière d'effectuer cette tâche et à quel moment.

Les performances peuvent être interrompues si l’application tente de mapper une ressource au mauvais moment. Si l’application tente d’accéder aux résultats d’une opération avant la fin de cette opération, le pipeline se bloque.

Une opération de mappage au moment incorrect peut potentiellement provoquer une grave baisse de performances en forçant le processeur graphique à se synchroniser avec l'UC. Cette synchronisation se produira si l’application souhaite accéder à une ressource avant que le processeur graphique ait fini de la copier dans une ressource que l’UC peut mapper.

### <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Considérations relatives aux performances

Le mieux est de considérer un PC comme un ordinateur exécuté en tant qu'architecture parallèle avec deux principaux types de processeurs: une ou plusieurs UC et un ou plusieurs processeurs graphiques. Comme dans n’importe quelle architecture parallèle, on obtient les meilleures performances lorsque chaque processeur est planifié avec suffisamment de tâches pour l’empêcher de passer à l'état inactif et lorsque l'exécution d’un processeur ne dépend pas de celle d’un autre.

Le pire scénario pour le parallélisme entre le processeur graphique et l'UC serait d'avoir à forcer un processeur à attendre les résultats du travail effectué par un autre. Direct3D évite ce coût en utilisant des méthodes de copie asynchrones, qui signifient qu'il n'est pas nécessaire que la copie soit exécutée au moment du retour de la méthode.

L’avantage est que l’application ne subit pas la baisse de performances liée à la copie des données jusqu'à ce que l'UC accède aux données, c'est-à-dire lorsque la méthode Map est appelée. Si la méthode Map est appelée une fois les données effectivement copiées, aucune perte de performances ne se produit. En revanche, si la méthode Map est appelée avant que les données ont été copiées, le pipeline se bloque.

Les appels asynchrones dans Direct3D (qui représentent la grande majorité des méthodes et en particulier les appels de rendu) sont stockés dans ce que l'on appelle une *mémoire tampon de commandes*. Cette mémoire tampon interne au pilote graphique est utilisée pour le traitement par lots des appels sur le matériel sous-jacent afin que le coût lié au passage du mode utilisateur au mode noyau dans MicrosoftWindows se produise le plus rarement possible.

La mémoire tampon de commandes est vidée, ce qui entraîne un basculement de mode utilisateur/noyau dans l'une des quatre situations suivantes.

1.  La méthode présente est appelée.
2.  La méthode de vidage est appelée.
3.  La mémoire tampon de commandes est saturée; sa taille est dynamique et contrôlée par le système d’exploitation et par le pilote graphique.
4.  L'UC a besoin d'accéder aux résultats d’une commande en attente d’exécution dans la mémoire tampon de commandes.

Parmi les quatre situations ci-dessus, la dernière est la plus critique en termes de performances. Si l’application émet un appel pour copier une ressource ou une sous-ressource, cet appel est placé dans la file d'attente de la mémoire tampon de commandes.

Si l’application tente ensuite de mapper la ressource de transfert qui était la cible de l’appel de copie avant le vidage de la mémoire tampon de commandes, le pipeline se bloque, car non seulement l’appel de la méthode de copie doit s’exécuter, mais toutes les autres commandes mises dans la mémoire tampon de commandes doivent s’exécuter également. Cela oblige le processeur graphique et l'UC à se synchroniser, car l'UC attendra pour accéder à la ressource de transfert que le processeur graphique vide la mémoire tampon de commandes et remplisse la ressource dont l’UC a besoin. Une fois que le processeur graphique a terminé la copie, l'UC commencera à accéder à la ressource de transfert; dans l'intervalle, cependant, le processeur graphique restera inactif.

L'exécution fréquence de cette lors de l’exécution a pour effet de dégrader fortement les performances. C'est pourquoi il est recommandé de prendre des précautions lors du mappage de ressources créées avec l’utilisation par défaut. L’application doit attendre suffisamment longtemps pour que la mémoire tampon de commandes soit vidée et, par conséquent, que toutes ces commandes aient fini de s’exécuter avant de tenter de mapper la ressource de transfert correspondante.

Combien de temps l’application doit-elle être mise en attente? Au moins deux trames, car cela permettra d'exploiter au maximum le parallélisme entre l’UC et le processeur graphique. Lorsque l’application traite la trame N en envoyant des appels à la mémoire tampon de commandes, le processeur graphique est occupé à exécuter les appels de la trame précédente, N-1.

Par conséquent, si une application souhaite mapper une ressource qui provient de la mémoire vidéo et copie une ressource au niveau de la trame N, cet appel commence réellement à s'exécuter au niveau de la trame N+1, c'est-à-dire au moment où l’application soumet des appels pour la trame suivante. La copie doit être terminée lorsque l’application traite la trame N+2.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Trame</th>
<th align="left">État processeur graphique/UC</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">N</td>
<td align="left"><ul>
<li>L'UC émet des appels de rendu pour la trame actuelle.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+1</td>
<td align="left"><ul>
<li>Le processeur graphique exécute les appels envoyés par l'UC pendant le traitement de la trame N.</li>
<li>L'UC émet des appels de rendu pour la trame actuelle.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+2</td>
<td align="left"><ul>
<li>Le processeur graphique termine l'exécution des appels envoyés par l'UC pendant le traitement de la trame N (résultats prêts).</li>
<li>Le processeur graphique exécute les appels envoyés par l'UC pendant le traitement de la trame N+1.</li>
<li>L'UC émet des appels de rendu pour la trame actuelle.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+3</td>
<td align="left"><ul>
<li>Le processeur graphique termine l'exécutions des appels envoyés par l'UC pendant le traitement de la trame N+1. Résultats prêts.</li>
<li>L'UC envoie des appels d'exécution du processeur graphique pendant le traitement de la trame N+2.</li>
<li>L'UC émet des appels de rendu pour la trame actuelle.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+4</td>
<td align="left">...</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources](resources.md)

 

 




