---
description: Cette rubrique décrit les différentes catégories de valeurs qui existent dans C++. Vous serez sans doute avez-vous entendu parler de lvalues et rvalues, mais il existe d’autres types, trop.
title: Catégories de valeur et les références à ceux-ci
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, uwp, standard, c ++, cpp, winrt, projection, déplacement, le transfert, les catégories de valeur, sémantique de déplacement, transfert parfait, lvalue, rvalue, glvalue ayant, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593014"
---
# <a name="value-categories-and-references-to-them"></a>Catégories de valeur et les références à ceux-ci
Cette rubrique décrit les différentes catégories de valeurs (et les références à des valeurs) qui existent dans C++. Vous serez sans doute avez-vous entendu parler de *lvalues* et *rvalues*, mais vous ne pouvez pas imaginer d'entre eux dans cette rubrique présente les termes. Et autres types de valeurs, trop.

Chaque expression en C++ génère une valeur qui appartienne à une des catégories présentées dans cette rubrique. Il existe des aspects de langage C++, ses facilies, règles, qui exigent une bonne compréhension de ces catégories de valeur et les références à ceux-ci. Par exemple, en prenant l’adresse d’une valeur, la copie d’une valeur, le déplacement d’une valeur et une valeur à une autre fonction de transfert. Cette rubrique n’aborde tous ces aspects en détail, mais il fournit des informations fondamentales pour une compréhension solide d'entre eux.

Les informations dans cette rubrique sont encadrée en termes d’analyse de Stroustrup de catégories de valeur par les deux propriétés indépendantes de l’identité et movability [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Une lvalue possède une identité
Que signifie pour une valeur d’avoir *identité*? Si vous avez (ou vous pouvez tirer) à l’adresse mémoire d’une valeur et l’utiliser en toute sécurité, identité n’a la valeur. De cette façon, vous pouvez effectuer plus de comparer le contenu de valeurs : vous pouvez comparer ou les distinguer par identité.

Un *lvalue* possède une identité. Il est désormais une question d’intérêt historique uniquement que la lettre « l » dans « lvalue » est l’abréviation de « left » (par exemple, le-gauche d’une assignation). En C++, une lvalue peut apparaître sur la gauche *ou* sur la droite d’une assignation. La lettre « l » dans « lvalues », puis, n’en fait vous aide à comprendre ni définir ce qu’ils sont. Vous devez uniquement comprendre que ce que nous appelons une lvalue est une valeur qui a une identité.

Exemples d’expressions qui sont des lvalues : une variable nommée ou une constante ; ou une fonction qui retourne une référence. Exemples d’expressions qui sont *pas* lvalues incluent : une table temporaire ; ou une fonction qui retourne par valeur.

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

Maintenant, bien qu’il soit une instruction vraie que lvalues ont identité, cela valeurs x. Nous allons passer plus ce qui un *xvalue* est plus loin dans cette rubrique. Pour le moment, seulement à l’esprit qu’il existe une catégorie de valeur appelée glvalue ayant, pour « généralisée lvalue ». Le sur-ensemble de glvalues contient les deux lvalues (également appelé *lvalues classique*) et les valeurs x. Par conséquent, tandis que « une lvalue a identité » a la valeur true, l’ensemble complet des éléments ayant une identité est l’ensemble de glvalues, comme indiqué dans cette illustration.

![Une lvalue possède une identité](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Une rvalue est mobile ; n’est pas une lvalue
Mais il existe des valeurs qui ne sont pas glvalues. Par conséquent, il existe des valeurs que vous avez *ne peut pas* obtenir une adresse de mémoire pour (ou vous ne pouvez pas y recourir pour être valide). Nous avons vu certaines de ces valeurs dans l’exemple de code ci-dessus. Cela ressemble à un inconvénient. Mais en réalité l’avantage d’une valeur qui est que vous pouvez *déplacer* à partir de (qui est généralement bon marché), plutôt que de copie à partir de celle-ci (qui est généralement coûteux). Déplacement d’une valeur signifie qu’il n’est plus à la place, qu'il est utilisé pour être. Tente d’y accéder à la place, que ce n’est donc quelque chose à être évitée. Une discussion sur quand et *comment* pour déplacer une valeur est hors de portée pour cette rubrique. Pour cette rubrique, nous devons savoir qu’une valeur qui peut être déplacé est appelée un *rvalue* (ou *rvalue classique*).

« R » dans « rvalue » est l’abréviation de « droite » (par exemple, le droit côté d’une assignation). Mais vous pouvez utiliser des rvalues et des références à des rvalues, en dehors des affectations. « R » dans « rvalues », puis, n'est pas la chose pour vous concentrer sur. Vous devez uniquement comprendre que ce que nous appelons une rvalue est une valeur qui peut être déplacé.

Une lvalue, à l’inverse, n’est pas déplaçable, comme indiqué dans cette illustration. Une lvalue déplacé serait Relevez la définition de *lvalue*, et il serait un problème inattendu pour le code qui très raisonnablement être en mesure de continuer à accéder à la valeur lvalue.

![Une rvalue est mobile ; n’est pas une lvalue](images/is-movable.png)

Vous ne pouvez pas déplacer une lvalue. Mais il y a *est* une sorte de glvalue ayant (l’ensemble des objets avec l’identité) que vous pouvez déplacer&mdash;si vous savez ce que vous faites (y compris en veillant à ne pas y accéder après le déplacement)&mdash;et qui est la valeur de x. Nous y reviendrons cette idée recliquez ci-dessous, lorsque nous examinons l’image complète des catégories de valeur.

## <a name="rvalue-references-and-reference-binding-rules"></a>Références rvalue et les règles de liaison de référence
Cette section présente la syntaxe pour une référence à une valeur rvalue. Nous allons devoir attendre une autre rubrique dans un traitement substantiels de déplacement et de transfert, mais ce sont des problèmes sont résolus par des références rvalue. Avant d’examiner les références rvalue, cependant, nous devons d’abord être plus clair sur `T&` &mdash;la chose que nous avons anciennement appelé simplement « une référence ». C’est « une lvalue (non-const) référence », qui fait référence à une valeur à laquelle l’utilisateur de la référence peut écrire.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Une référence lvalue peut lier à une lvalue, mais pas à une valeur rvalue.

Il existe des références constantes lvalue (`T const&`), qui font référence aux objets auxquels l’utilisateur de la référence *ne peut pas* écriture (par exemple, une constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Une référence const lvalue peut lier à une lvalue ou rvalue.

La syntaxe pour une référence à une rvalue du type `T` est écrite sous la forme `T&&`. Une référence rvalue fait référence à une valeur peut être déplacée&mdash;une valeur dont le contenu nous n’avez pas besoin de conserver une fois que nous avons utilisé (par exemple, une variable temporaire). Depuis le point entière est à déplacer à partir de (modification ainsi) la valeur liée à une référence rvalue, `const` et `volatile` qualificateurs (également appelés des qualificateurs cv) ne s’appliquent pas aux références rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Une référence rvalue lie à une valeur rvalue. En fait, en termes de résolution de surcharge, une rvalue *préfère* doit être lié à une référence rvalue à une référence const lvalue. Mais une référence rvalue ne peut pas lier à une lvalue car, comme nous l’avons dit, une référence rvalue fait référence à une valeur dont le contenu est supposé nous n’avez pas besoin de conserver (par exemple, le paramètre pour un constructeur de déplacement).

Vous pouvez également transmettre une rvalue où un argument par valeur est attendu, via la construction de copie (ou par le biais de construction de déplacement, si la valeur rvalue est une valeur de x).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>A un glvalue ayant une identité ; n’est pas le cas d’une prvalue
À ce stade, nous savons que ce qui a une identité. Et nous savons ce qu’est mobile et ce qui n’est pas. Mais nous n’avons pas encore le jeu nommé des valeurs qui *ne* ont identité. Que l’ensemble est appelé le *prvalue*, ou *rvalue pure*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Une lvalue possède une identité ; n’est pas le cas d’une prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>L’image complète des catégories de valeur
Il reste uniquement pour combiner les informations et les illustrations ci-dessus dans une seule image d’ensemble.

![L’image complète des catégories de valeur](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue ayant (i)
Un glvalue ayant (lvalue généralisé) a l’identité.

### <a name="lvalue-im"></a>lvalue (j’ai\&\!m)
Une lvalue (il s’agit d’un type de glvalue ayant) a une identité, mais ne peut être déplacée. Ce sont les valeurs généralement en lecture-écriture qui vous passez par référence ou par référence const ou par valeur si la copie est bon marché. Impossible de lier une lvalue en référence rvalue.

### <a name="xvalue-im"></a>valeur x (j’ai\&m)
Une valeur de x (une sorte de glvalue ayant, mais également un type de rvalue) a une identité et peut également être déplacé. Cela peut être une lvalue espièglerie que vous avez décidé de déplacer, car la copie est coûteuse, et vous serez veiller à ne pas y accéder par la suite. Voici comment vous pouvez activer une lvalue en une valeur de x.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

Dans l’exemple de code ci-dessus, nous n’avons pas quoi que ce soit encore transféré. Nous avons créé une xvalue en effectuant un cast d’une lvalue en référence rvalue sans nom. Il peut toujours être identifié par son nom lvalue ; mais, comme une valeur de x, il est désormais *compatible* d’en cours de déplacement. Les raisons pour ce faire, et les déplacer réellement l’aspect, devront attendre une autre rubrique. Mais vous pouvez considérer le « x » dans « xvalue « signification » expert seule » si qui vous aide à. En convertissant une lvalue dans une xvalue (il s’agit d’un type de rvalue), la valeur devient alors capable d’être lié à une référence rvalue.

Voici deux autres exemples de valeurs x&mdash;appelant une fonction qui retourne une référence rvalue sans nom et l’accès à un membre d’une valeur de x.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!je\&m)
Une prvalue (rvalue pure ; un type de rvalue) dépourvu d’identité, mais peut être déplacé. Il s’agit généralement des objets temporaires, le résultat de l’appel d’une fonction qui retourne par valeur ou le résultat de l’évaluation de toute autre expression qui n’est pas un glvalue ayant,

### <a name="rvalue-m"></a>rvalue (m)
Une rvalue est mobile. Une rvalue *référence* fait toujours référence à une rvalue (une valeur dont le contenu est supposé nous n’avez pas besoin de conserver).

Mais, une référence rvalue lui-même une rvalue ? Un *sans nom* référence rvalue (comme celles indiquées dans les exemples de code xvalue ci-dessus) est une valeur de x, Oui, il est donc une rvalue. Il préfère être lié à un paramètre de fonction de référence rvalue, telle que celle d’un constructeur de déplacement. À l’inverse (et peut-être counter-intuitively), si une référence rvalue a un nom, l’expression composée de ce nom est une lvalue. Par conséquent, il *ne peut pas* être lié à un paramètre de référence rvalue. Mais il est facile de le faire&mdash;simplement effectuer un cast en une référence rvalue sans nom (une xvalue) à nouveau.

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

### <a name="im"></a>\!i\&\!m
Le type de valeur qui n’a pas identité et n’est pas mobile est l’un combinaison que nous n’avons pas encore abordé. Mais nous pouvons ignorer, car cette catégorie n’est pas une idée utile dans le langage C++.

## <a name="reference-collapsing-rules"></a>Règles de réduction de référence
Plusieurs références like dans une expression (une référence lvalue à une référence lvalue ou une référence rvalue à une référence rvalue) annuler un hors d’un autre.

- `A& &` est réduite à `A&`.
- `A&& &&` est réduite à `A&&`.

Plusieurs références dans une expression à la différence se réduire à une référence lvalue.

- `A& &&` est réduite à `A&`.
- `A&& &` est réduite à `A&`.

## <a name="forwarding-references"></a>Références de transfert
Cette dernière section compare les références rvalue, dont nous avons déjà vu, avec le concept de différents d’un *transfert référence*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` est une référence rvalue, comme nous l’avons vu. Const et volatile ne s’appliquent pas aux références rvalue.
- `foo` accepte uniquement les rvalues de type **A**.
- Références rvalue de la raison (tel que `A&&`) existe est afin que vous pouvez créer une surcharge qui est optimisée pour le cas d’une table temporaire (ou autres rvalue) est passé.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` est un *transfert référence*. Selon ce que vous transmettez à `bar`, type **_Ty** pourrait être const/non const indépendamment volatile/non volatile.
- `bar` accepte n’importe quel lvalue ou une rvalue du type **_Ty**.
- Le passage d’une lvalue entraîne la référence de transfert devenir `_Ty& &&`, ce qui réduit à la référence lvalue `_Ty&`.
- Le passage d’une rvalue entraîne la référence de transfert devenir `_Ty&& &&`, ce qui réduit à la référence rvalue `_Ty&&`.
- La raison pour laquelle les références de transfert (tel que `_Ty&&`) existe est *pas* pour l’optimisation, mais pour tirer ce que vous passez pour eux et transférer de façon transparente et efficace. Vous êtes susceptible de rencontrer une référence de transfert uniquement si vous écrivez (ou étudiez attentivement) code de bibliothèque&mdash;, par exemple, une fonction de fabrique qui transmet les arguments de constructeur.

## <a name="sources"></a>Sources
* \[Stroustrup, 2013\] Stroustrup de B. : Le langage de programmation C++, quatrième édition. Addison-Wesley. 2013.
