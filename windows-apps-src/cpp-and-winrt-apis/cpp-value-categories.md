---
author: stevewhims
description: Cette rubrique décrit les différentes catégories de valeurs qui existent dans C++. Vous serez sans doute avez-vous entendu parler de lvalues et rvalues, mais il existe d’autres types, trop.
title: Catégories de valeur
ms.author: stwhi
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c ++, RPC, winrt, projection, en déplaçant, transfert, des catégories de valeur, sémantique de déplacement, transfert parfait, lvalue, rvalue, glvalue, prvalue, valeur x
ms.localizationpriority: medium
ms.openlocfilehash: 75176334909e0e6bcf81763b543a19b70a4cba73
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/10/2018
ms.locfileid: "2739080"
---
# <a name="value-categories"></a>Catégories de valeur
Cette rubrique décrit les différentes catégories de valeurs qui existent dans C++. Vous serez sans doute avez-vous entendu parler de *lvalues* et *rvalues*, mais il existe d’autres types, trop. Chaque expression en langage C++ génère une valeur qui appartienne à une des catégories décrites dans cette rubrique. Il existe des aspects de langage C++, son facilies et les règles, une bonne compréhension des catégories de la valeur de la demande.

## <a name="an-lvalue-has-identity"></a>Lvalue a une identité
Que signifie pour une valeur *d’identité*? Si vous avez (ou vous pouvez prendre) l’adresse de la mémoire d’une valeur, la valeur a identité. Ainsi, vous pouvez effectuer plusieurs compare le contenu de valeurs: vous pouvez comparer ou différencier suivant leur identité.

Une *lvalue* a identité. Il est désormais une question d’intérêt historique uniquement que le «l» dans «lvalue» est une abréviation de «left» (comme dans la gauche côté d’une affectation). En langage C++, lvalue peut apparaître sur la gauche *ou* droite d’une affectation. «L» dans «lvalues», puis, ne réellement vous aide à comprendre ni de ce qu’ils sont. Il vous suffit de comprendre que ce que nous appelons lvalue est une valeur qui a une identité.

À présent, alors qu’il est une instruction true qu’identité lvalues, cela xvalues. Nous allons le voir un peu plus dans quel une *valeur x* est plus loin dans cette rubrique (bien que, une description complète de leur n’est pas dans la portée ici). Pour l’instant, sachez simplement qu’il existe une catégorie valeur appelée glvalue, pour «généralisées lvalue». Le sur-ensemble des glvalues contient lvalues (également appelé «lvalues classique») et xvalues. Par conséquent, tandis que «lvalue a identité» a la valeur true, l’ensemble complet des éléments dont l’identité est l’ensemble des glvalues, comme indiqué dans cette illustration.

![Lvalue a une identité](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue est mobile; lvalue n’est pas
Mais il existe des valeurs qui ne sont pas glvalues. Par conséquent, il existe des valeurs que vous *ne pouvez pas* obtenir une adresse de mémoire pour (ou si vous ne pouvez pas compter sur qu’elle soit valide). Les sons comme un inconvénient. Mais en fait l’avantage d’une valeur qui est que vous ne pouvez *déplacer* (qui est généralement peu coûteux), plutôt que de copie it (qui est généralement coûteux). Déplacement d’une valeur signifie qu’il n’est plus place qu'auparavant. Essaie d’accéder aux place que ce n’est donc quelque chose à éviter. Une description des quand et *comment* déplacer qu'une valeur est hors de portée de cette rubrique. Pour cette rubrique, nous devons simplement savoir qu’une valeur qui est mobile est appelée une *rvalue*.

«R» dans «rvalue» est l’abréviation de «droite» (comme dans le droit côté d’une affectation). Mais vous pouvez utiliser rvalues et des références aux valeurs r, en dehors des affectations. «R» dans les valeurs «r», puis, n'est pas la chose à consacrer. Il vous suffit de comprendre que ce que nous appelons rvalue est une valeur qui peut être déplacée.

Lvalue, à l’inverse, n’est pas mobile, comme indiqué dans cette illustration. Impossible de déplacer une lvalue parce que, si vous pouviez, puis il le serait non sécurisés (ou même désastreux) pour continuer à y accéder par la suite. N’oubliez pas, vous avez son identité.

![Rvalue est mobile; lvalue n’est pas](images/is-movable.png)

Vous ne pouvez pas déplacer lvalue. Mais il *est* un type de glvalue (l’ensemble des éléments avec l’identité) que vous pouvez déplacer&mdash;si vous savez ce que vous faites&mdash;et qui est la valeur x. Nous y reviendrons cette idée une nouvelle fois ci-dessous, lorsque nous examinons l’image complète des catégories de valeur.

## <a name="an-lvalue-has-identity-a-prvalue-does-not"></a>Lvalue a une identité; n’est pas le cas d’un prvalue
À ce stade, nous savons que ce qui a une identité. Et nous savons What ' s mobile et ce qui n’est pas. Mais nous n’avez pas encore nommé l’ensemble de valeurs qui *ne* possèdent identité. Cet ensemble est appelé *prvalue*, ou «rvalue pur».

![Lvalue a une identité; n’est pas le cas d’un prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>L’image complète des catégories de valeur
Il reste uniquement pour combiner les informations et les illustrations ci-dessus dans une image unique, grande.

![L’image complète des catégories de valeur](images/value-categories.png)

- Un glvalue (lvalue généralisé) a l’identité.
- Lvalue (un type de glvalue) a une identité, mais n’est pas mobile. Il s’agit généralement en lecture-écriture des valeurs que vous passez autour par référence par référence const ou par valeur si la copie est peu coûteux.
- Une valeur x (un type de glvalue, mais également un type de valeur rvalue) a une identité et peut également être déplacé. Cela peut être une lvalue premier que vous avez décidé de déplacer, car la copie est coûteuse, et vous pouvez veiller à ne pas y accéder par la suite. La façon dont vous transformez lvalue en une valeur x et les motifs de déplacement, doivent attendre une autre rubrique. Mais vous pouvez considérer la «x» dans la valeur «x» en ce qui signifie «expert» si cela permet d’afficher.
- Un prvalue (rvalue pur; un type de valeur rvalue) n’a pas d’identité, mais peut être déplacé. Il s’agit généralement de littéraux, temporaires, renvoyer les valeurs&mdash;tout ce qui est le résultat de l’évaluation d’expression (expression qui n’est pas un glvalue), ou tout élément renvoyé par la valeur d’une fonction.
- Rvalue peut être déplacée.
