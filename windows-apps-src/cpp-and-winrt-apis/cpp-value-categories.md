---
author: stevewhims
description: Cette rubrique décrit les différentes catégories de valeurs qui existent en C++. Vous seront sans doute ont entendu lvalues et rvalues, mais il existe d’autres types, trop.
title: Catégories de valeurs et références à ces
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, déplacement, transfert, catégories de valeurs, sémantique de déplacement, transfert parfait, lvalue, rvalue, glvalue, prvalue, valeur x
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 854bc14c37a70cdb4e7e4113a10ca977582cbb77
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7173862"
---
# <a name="value-categories-and-references-to-them"></a>Catégories de valeurs et références à ces
Cette rubrique décrit les différentes catégories de valeurs (et les références à des valeurs) qui existent en C++. Vous seront sans doute ont entendu *lvalues* et *rvalues*, mais vous ne pouvez pas penser d'entre eux dans les conditions de cette rubrique présente. Et il existe également des autres types de valeurs.

Chaque expression en C++ génère une valeur qui appartient à l’une des catégories décrites dans cette rubrique. Il existe des aspects de langage C++, son facilies et les règles, qui exigent une bonne compréhension de ces catégories de valeurs et des références associées. Par exemple, en prenant l’adresse d’une valeur, la copie d’une valeur, le déplacement d’une valeur et une valeur à une autre fonction de transfert. Cette rubrique ne passe pas dans tous ces aspects en profondeur, mais il fournit des informations notions de base pour une bonne compréhension d'entre eux.

Les informations fournies dans cette rubrique sont encadré en termes d’analyse de Stroustrup de catégories de valeurs par les deux propriétés indépendantes d’identité et movability [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Lvalue possède une identité
Que signifie pour une valeur *d’identité*? Si vous avez (ou vous pouvez prendre) à l’adresse de mémoire d’une valeur et l’utiliser en toute sécurité, alors la valeur n’a identité. De cette façon, vous pouvez faire plus que comparer le contenu de valeurs: vous pouvez comparer ou de les distinguer par identité.

Une *lvalue* dispose d’identité. Il est désormais une question d’intérêt uniquement historique que le «l» en «lvalue» est l’abréviation de «gauche» (comme dans la gauche partie d’une affectation). En C++, lvalue peut apparaître sur la gauche *ou* à droite d’une affectation. Le «l» en «lvalues», puis, ne prend pas réellement vous aident à comprendre ni de ce qu’ils sont. Vous devez uniquement comprendre que ce que nous appelons lvalue est une valeur qui a une identité.

Exemples d’expressions qui sont lvalues: une variable nommée ou une constante; ou une fonction qui renvoie une référence. Exemples d’expressions qui se trouvent *pas* lvalues: une variable temporaire; ou une fonction qui retourne par valeur.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

Maintenant, alors qu’elle est une instruction true que lvalues ont identité, cela valeurs x. Nous aborderons plus quelles une *valeur x* est plus loin dans cette rubrique. Pour l’instant, sachez simplement qu’il existe une catégorie de valeur appelée glvalue, pour «généralisées lvalue». Le sur-ensemble de glvalues contient à la fois lvalues (également connue sous le *lvalues classique*) et les valeurs x. Par conséquent, tandis que «lvalue a identité» est true, l’ensemble complet des choses que vous avez identité est l’ensemble des glvalues, comme illustré dans l’illustration suivante.

![Lvalue possède une identité](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue est mobile; lvalue n’est pas
Mais il existe des valeurs qui ne sont pas glvalues. Par conséquent, il existe des valeurs que vous *ne pouvez pas* obtenir une adresse de mémoire pour (ou si vous ne pouvez pas compter sur qu’elle soit valide). Nous avons vu certaines de ces valeurs dans l’exemple de code ci-dessus. Cela ressemble un inconvénient. Mais en fait l’avantage d’une valeur comme cela signifie que vous pouvez *déplacer* à partir de celui-ci (qui est généralement peu coûteux), plutôt que copier à partir de celui-ci (qui est généralement coûteux). Passer d’une valeur signifie qu’il n’est plus à la place de que pouvoir. Lors de la tentative d’y accéder à la place de que pouvoir est donc quelque chose à être évitées. Explication des quand et *comment* pour déplacer qu'une valeur n’est pas dans le cadre de cette rubrique. Dans cette rubrique, nous devons juste savoir qu’une valeur qui peut être déplacée est appelée une *rvalue* (ou *rvalue classique*).

Le «r» dans «rvalue» est l’abréviation de «droite» (comme dans le bouton droit partie d’une affectation). Mais vous pouvez utiliser rvalues et des références aux valeurs r, en dehors des affectations. Le «r» dans «valeurs r», puis, n'est pas la chose à se concentrer sur. Vous devez uniquement comprendre que ce que nous appelons rvalue est une valeur qui peut être déplacée.

Lvalue, à l’inverse, n’est pas mobile, comme illustré dans l’illustration suivante. Lvalue déplacé serait Relevez la définition de *lvalue*, et il serait un problème inattendu pour le code qui très raisonnablement s’attendre à être en mesure de continuer à accéder à la valeur lvalue.

![Rvalue est mobile; lvalue n’est pas](images/is-movable.png)

Vous ne pouvez pas déplacer lvalue. Mais il *est* un type de glvalue (l’ensemble des choses avec l’identité) que vous pouvez déplacer&mdash;si vous savez ce que vous êtes en train de faire (y compris en veillant à ne pas y accéder après le déplacement)&mdash;et c’est la valeur x. Nous y reviendrons cette idée une nouvelle fois ci-dessous, lorsque nous allons examiner l’image complète des catégories de valeur.

## <a name="rvalue-references-and-reference-binding-rules"></a>Références rvalue et les règles de liaison de référence
Cette section présente la syntaxe pour une référence à une valeur rvalue. Nous allons devoir attendre une autre rubrique accéder à un traitement substantiel de déplacement et de transfert, mais ce sont des problèmes sont résolus par des références de rvalue. Avant de nous allons examiner les références rvalue, cependant, nous devons d’abord être réorganisées sur `T&` &mdash;la chose que nous avons précédemment été appelant simplement «une référence». Il s’agit vraiment «une lvalue (non const) référence», qui fait référence à une valeur sur laquelle l’utilisateur de la référence peut écrire.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Une référence lvalue peut lier à lvalue, mais pas à une valeur rvalue.

Ensuite, il existe des références const lvalue (`T const&`), qui font référence aux objets auxquels l’utilisateur de la référence *ne peut pas* écrire (par exemple, il s’agit d’une constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Une référence const lvalue peut lier à lvalue ou à une valeur rvalue.

La syntaxe pour une référence à une valeur rvalue de type `T` est écrit en tant que `T&&`. Une référence rvalue fait référence à une valeur mobile&mdash;une valeur dont nous n’avez pas besoin de conserver une fois que nous avons utilisé (par exemple, une variable temporaire) le contenu. Depuis le but principal est pour déplacer de (ainsi modification) la valeur liée à une référence rvalue, `const` et `volatile` qualificateurs (également connue sous le nom VC-qualificateurs) ne s’appliquent pas aux références rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Une référence rvalue est lié à une valeur rvalue. En fait, en termes de résolution de la surcharge, une rvalue *préfère* être liée à une référence rvalue que pour une référence const lvalue. Mais une référence rvalue ne pouvez pas lier à lvalue car, comme nous l’avons dit, une référence rvalue fait référence à une valeur dont le contenu qu’il est supposé que nous n’avez pas besoin de conserver (par exemple, le paramètre pour un constructeur de déplacement).

Vous pouvez également passer rvalue où un argument par valeur est attendu, via la construction de copie (ou construction de déplacement, si la valeur rvalue est une valeur x).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Un glvalue a une identité; n’est pas le cas d’un prvalue
À ce stade, nous savons que ce qui a une identité. Et nous savons ce qui peut être déplacé et ce qui n’est pas. Mais nous ne l’avez pas encore nommé l’ensemble de valeurs qui ** n’ont identité. Ce jeu est appelé le *prvalue*ou *rvalue pure*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Lvalue possède une identité; n’est pas le cas d’un prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>L’image complète des catégories de valeur
Il reste uniquement pour combiner les informations et les illustrations ci-dessus dans une image unique et grande.

![L’image complète des catégories de valeur](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Un glvalue (lvalue généralisé) dispose d’identité.

### <a name="lvalue-im"></a>lvalue (i\ & \!m)
Lvalue (il s’agit d’un type de glvalue) a une identité, mais n’est pas mobile. Il s’agit généralement en lecture-écriture les valeurs que vous passez autour de référence par référence const ou par valeur si la copie est bon marché. Lvalue ne peut pas être liée à une référence rvalue.

### <a name="xvalue-im"></a>valeur x (i\ & m)
Une valeur x (un type de glvalue, mais également un type de valeur rvalue) a une identité et peut également être déplacé. Cela peut être lvalue premier que vous avez décidé de déplacer, car la copie est coûteuse, et vous serez veiller à ne pas y accéder par la suite. Voici comment vous pouvez transformer lvalue en une valeur x.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

Dans l’exemple de code ci-dessus, nous n’avons pas déplacé quoi que ce soit encore. Nous avons simplement créé une valeur x en effectuant un cast de lvalue pour une référence rvalue sans nom. Elle peut toujours être identifiée par son nom lvalue; Toutefois, comme une valeur x, il est désormais *en mesure* de déplacé. Les raisons pour ce faire, et le déplacement ressemble, aura attendre une autre rubrique. Toutefois, vous pouvez considérer le «x» dans la valeur de «x» en tant que la signification «expert uniquement» si qui vous aide à. En effectuant un cast lvalue dans une valeur x (il s’agit d’un type de valeur rvalue), la valeur est alors capable d’une référence rvalue en cours de liaison.

Voici deux autres exemples de valeurs x&mdash;appel d’une fonction qui renvoie une référence rvalue sans nom et l’accès à un membre d’une valeur x.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Un prvalue (rvalue pure; un type de valeur rvalue) n’a pas identité, mais peut être déplacé. Il s’agit généralement temporaires, le résultat de l’appel d’une fonction qui retourne par valeur, ou le résultat de l’évaluation de toute autre expression qui n’est pas un glvalue,

### <a name="rvalue-m"></a>rvalue (m)
Rvalue peut être déplacée. Une rvalue *référence* correspond toujours à rvalue (une valeur dont le contenu qu’il est supposé que nous n’avez pas besoin de conserver).

Toutefois, est une référence rvalue lui-même rvalue? Une référence rvalue *sans nom* (comme celles indiquées dans les exemples de code de valeur x ci-dessus) est une valeur x Oui, il est donc une valeur rvalue. Il préfère être liée à un paramètre de fonction référence rvalue, telles que celle d’un constructeur de déplacement. À l’inverse (et éventuellement counter-intuitively), si une référence rvalue a un nom, l’expression consistant à ce nom est lvalue. Par conséquent, il *ne peut pas* être liée à un paramètre de référence rvalue. Mais il est facile afin de pouvoir faire&mdash;simplement un cast vers une référence de rvalue sans nom (une valeur x) à nouveau.

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\ & \!m
Le type de valeur qui n’a pas identité et n’est pas mobile est la combinaison d’one que nous n’avons pas encore traité. Toutefois, nous pouvons ignorer, car cette catégorie n’est pas une idée utile dans le langage C++.

## <a name="reference-collapsing-rules"></a>Règles de réduction de référence
Plusieurs références similaires dans une expression (une référence lvalue à une référence lvalue, ou une référence rvalue à une référence rvalue) annuler l’une sortie d’une autre.

- `A& &` se transforme en `A&`.
- `A&& &&` se transforme en `A&&`.

Plusieurs Contrairement aux références dans une expression réduire à une référence lvalue.

- `A& &&` se transforme en `A&`.
- `A&& &` se transforme en `A&`.

## <a name="forwarding-references"></a>Transfert des références
Cette section finale contraste des références rvalue, nous avons déjà abordés, le concept de différent d’une *référence de transfert*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` est une référence rvalue, comme nous l’avons vu. Const et volatile ne s’appliquent pas aux références rvalue.
- `foo` accepte uniquement les rvalues de type **A**.
- Fait référence à la valeur rvalue raison (tel que `A&&`) existe est de sorte que vous pouvez créer une surcharge qui est optimisée pour le cas d’une variable temporaire (ou d’autres rvalue) passés.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` est une *référence de transfert*. En fonction de ce que vous passez à `bar`, de type **_Ty** pourrait être const/non const indépendamment volatile/non volatile.
- `bar` accepte n’importe quel lvalue ou les rvalue de type **_Ty**.
- Le passage d’une valeur lvalue entraîne la référence de transfert à devenir `_Ty& &&`, ce qui réduit la référence lvalue `_Ty&`.
- Le passage d’une valeur rvalue entraîne la référence de transfert à devenir `_Ty&& &&`, ce qui réduit la référence rvalue `_Ty&&`.
- La raison pour laquelle les références de transfert (telles que `_Ty&&`) existe est *pas* d’optimisation, mais à prendre ce que vous transmettez leur et la transférer sur efficacement et de manière transparente. Vous êtes susceptible de rencontrer une référence de transfert uniquement si vous écrivez (ou étudiez attentivement) code de la bibliothèque&mdash;par exemple, une fonction factory qui transfère les arguments de constructeur.

## <a name="sources"></a>Sources
* \[Stroustrup, 2013\] B. Stroustrup: le langage de programmation C++, la quatrième édition. Addison-Wesley. 2013.
