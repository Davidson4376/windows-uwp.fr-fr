---
author: Karl-Bridge-Microsoft
Description: "Complétez la fonctionnalité de base de Cortana avec des commandes vocales qui lancent et exécutent une action unique dans une application externe."
title: Interactions avec Cortana
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: d23416ad3344a39c09078b6ba3acc38fa3ba65a0

---

# Interactions avec Cortana dans les applicationsUWP




Complétez les fonctionnalités de base de **Cortana** avec des commandes vocales qui lancent et exécutent une action unique dans une application externe. 


**Autres fonctions vocales**

-   Voir [Recommandations en matière de conception de fonctions vocales](speech-interactions.md) si vous intégrez la reconnaissance vocale et la conversion de texte par synthèse vocale (également appelée TTS ou synthèse vocale) directement dans votre application.

> **Remarque**  
> Une commande vocale est un énoncé unique avec une intention spécifique, défini dans un fichier VCD (Voice Command Definition), qui est dirigé vers une application installée par le biais de **Cortana**.

> Un fichier VCD définit une ou plusieurs commandes vocales, chacune avec une intention unique.

> La définition de la commande vocale peut varier en complexité. Elle peut prendre en charge un énoncé unique et limité ou une collection d’énoncés plus naturels et flexibles, indiquant tous la même intention.


L’application cible peut être lancée au premier plan (elle prend alors le focus et **Cortana** disparaît) ou activée à l’arrière-plan (**Cortana** conserve le focus, mais fournit les résultats de l’application), selon la complexité de l’interaction. Par exemple, les commandes vocales qui requièrent un contexte supplémentaire ou une entrée utilisateur sont mieux gérées dans une application au premier plan, tandis que les commandes de base peuvent être gérées dans **Cortana** par le biais d’une application en arrière-plan.

 

L’intégration des fonctionnalités de base de votre application et la fourniture d’un point d’entrée central pour que l’utilisateur accomplisse la plupart des tâches sans ouvrir directement votre application permet à **Cortana** de devenir un intermédiaire entre votre application et l’utilisateur. Le fait de fournir ce raccourci vers les fonctionnalités de l’application et de réduire la nécessité de basculer entre les applications peut faire gagner beaucoup de temps et d’énergie à l’utilisateur.


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Recommandations en matière de conception](cortana-design-guidelines.md)</p></td>
<td align="left"><p>Ces recommandations et ces instructions décrivent comment votre application peut utiliser **Cortana** au mieux pour interagir avec l’utilisateur, l’aider à accomplir une tâche et indiquer clairement comment tout cela se passe.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Lancer une application au premier plan à l’aide de commandes vocales](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Vous pouvez utiliser des commandes vocales dans <strong>Cortana</strong> pour accéder aux fonctionnalités système, mais il est également possible d’utiliser ces commandes via <strong>Cortana</strong> pour démarrer une application au premier plan et spécifier une action ou une commande à exécuter au sein de l’application.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Modifier de manière dynamique les listes d’expressions de définition des commandes vocales (VCD)](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>Découvrez comment accéder à la liste des expressions prises en charge (éléments <strong>PhraseList</strong>) d’un fichier VCD et comment la mettre à jour à l’aide du résultat de reconnaissance vocale à l’exécution.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Lancer une application en arrière-plan à l’aide de commandes vocales](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Vous pouvez utiliser des commandes vocales dans <strong>Cortana</strong> pour accéder aux fonctionnalités système. Vous pouvez également compléter <strong>Cortana</strong> avec les fonctionnalités d’une application en arrière-plan à l’aide des commandes vocales qui spécifient une action ou une commande à exécuter au sein de l’application.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Interagir avec une application en arrière-plan](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>Découvrez comment un utilisateur peut interagir avec une application en arrière-plan via les fonctions vocales et le canevas de <strong>Cortana</strong> pendant l’exécution d’une commande vocale.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Lien ciblé vers une application en arrière-plan](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>Indiquez des liens ciblés à partir du service d’application en arrière-plan dans <strong>Cortana</strong> pour lancer l’application au premier plan dans un état ou un contexte spécifique.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Prendre en charge des commandes vocales en langage naturel](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Découvrez comment enrichir <strong>Cortana</strong> avec des commandes vocales plus souples et plus naturelles qui permettent à un utilisateur de prononcer le nom de votre application n’importe où dans la commande.</p></td>
</tr>
</tbody>
</table>

 

## Articles connexes


* [**Éléments et attributs d’un fichier VCDv1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Concepteurs**
* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Recommandations relatives à la conception de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)

**Exemples**
* [Exemple de commande vocale Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO5-->


