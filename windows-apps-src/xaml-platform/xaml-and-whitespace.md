---
author: jwmsft
description: "Découvrez les règles de traitement des espaces blancs utilisées par le langage XAML."
title: XAML et espace blanc
ms.assetid: 025F4A8E-9479-4668-8AFD-E20E7262DC24
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3b8b68ae8823ec23105d5f92e533d271ab3dcc88

---

# XAML et espace blanc

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Découvrez les règles de traitement des espaces blancs utilisées par le langage XAML.

## Traitement des espaces blancs

En accord avec le langage XML, les caractères d’espace blanc en langage XAML sont l’espace, le saut de ligne et la tabulation. Ces caractères correspondent respectivement aux valeurs Unicode 0020, 000A et 0009. Par défaut, cette normalisation de l’espace blanc se produit lorsqu’un processeur XAML rencontre le texte interne trouvé entre des éléments dans un fichier XAML :

-   Les caractères de saut de ligne d’Asie orientale situés entre des caractères sont supprimés.
-   Tous les caractères d’espace blanc (espace, saut de ligne, tabulation) sont convertis en espaces.
-   Tous les espaces consécutifs sont supprimés et remplacés par un seul espace.
-   Un espace qui suit immédiatement la balise de début est supprimé.
-   Un espace qui précède immédiatement la balise de fin est supprimé.
-   Les *caractères d’Asie orientale* correspondent à un ensemble de plages de caractères Unicode allant de U+20000 à U+2FFFD et de U+30000 à U+3FFFD. Parfois, ce sous-ensemble est également appelé *idéogrammes CJC*. Pour plus d’informations, voir http://www.unicode.org.

« Par défaut » correspond à l’état indiqué par la valeur par défaut de l’attribut **xml:space**.

### Espace blanc dans le texte interne et primitives de chaîne

Les règles de normalisation ci-dessus s’appliquent au texte interne au sein d’éléments XAML. À l’issue de la normalisation, un processeur XAML convertit le texte interne dans un type approprié comme ceci:

-   Si le type de la propriété n’est pas une collection, mais n’est pas directement un type **Object**, le processeur XAML essaie de le convertir vers ce type à l’aide de son convertisseur de type. Un échec de conversion ici engendre une erreur d’analyse XAML.
-   Si le type de la propriété est une collection et que le texte interne est contigu (sans balises d’éléments intermédiaires), le texte interne est analysé comme un élément **String** unique. Si le type de collection ne peut pas accepter d’élément **String**, cela engendre également une erreur de l’analyseur XAML.
-   Si le type de la propriété est **Object**, alors le texte interne est analysé comme un élément **String** unique. S’il existe des balises d’éléments intermédiaires, cela engendre une erreur de l’analyseur XAML, car le type **Object** implique un objet unique (**String** ou autre).
-   Si le type de la propriété est une collection, et que le texte interne n’est pas contigu, alors la première sous-chaîne est convertie en élément **String** et ajoutée en tant qu’élément de collection, l’élément intermédiaire est ajouté en tant qu’élément de collection et la sous-chaîne finale (le cas échéant) est ajoutée à la collection en tant que troisième élément **String**.

### Espace blanc et modèles de contenu texte

Dans la pratique, la conservation de l’espace blanc concerne uniquement un sous-ensemble de tous les modèles de contenu possibles. Ce sous-ensemble se compose des modèles de contenu pouvant prendre le type **String** d’un singleton dans un formulaire, une collection d’éléments **String** dédiée ou un mélange d’éléments **String** et d’autres types dans des listes, collections ou dictionnaires.

Même pour les modèles de contenu qui peuvent prendre des chaînes, le comportement par défaut au sein de ces modèles de contenu veut que tout espace blanc qui reste ne soit pas traité comme significatif.

### Conservation de l’espace blanc

Plusieurs techniques pour conserver l’espace blanc dans le code XAML source à des fins de présentation finale ne sont pas affectées par la normalisation de l’espace blanc par le processeur XAML.

`xml:space="preserve"`: spécifiez cet attribut au niveau de l’élément quand l’espace blanc doit être conservé. Notez que ceci conserve tous les espaces blancs, y compris ceux qui peuvent être ajoutés par des éditeurs de code ou des aires de conception pour aligner les éléments de balisage en guise d’imbrication visuellement intuitive. Le rendu de ces espaces dépend là encore du modèle de contenu de l’élément contenant. Nous vous déconseillons de spécifier `xml:space="preserve"` au niveau de la racine, car la plupart des modèles objet ne considèrent pas l’espace blanc comme étant significatif. Il est préférable de définir uniquement l’attribut de manière spécifique au niveau des éléments qui rendent l’espace blanc au sein de chaînes ou qui sont des collections significatives d’espace blanc.

Entités et espaces insécables : le code XAML prend en charge le placement d’une entité Unicode au sein d’un modèle objet texte. Vous pouvez utiliser des entités dédiées telles que l’espace insécable (en codage UTF-8). Vous pouvez également utiliser des contrôles de texte enrichi qui prennent en charge les caractères d’espace insécable. Procédez avec caution si vous utilisez des entités pour simuler des caractéristiques de disposition telles que les retraits, car la sortie au moment de l’exécution des entités varie en fonction d’un nombre supérieur de facteurs que celui des fonctionnalités de disposition générales, telles que l’utilisation propre des volets et marges.




<!--HONumber=Aug16_HO3-->


