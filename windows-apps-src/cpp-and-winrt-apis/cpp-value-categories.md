---
description: Cette rubrique décrit les différentes catégories de valeurs qui existent dans C++. Vous aurez sans doute entendu parler de lvalues et rvalues, mais il existe aussi d’autres types.
title: Catégories de valeurs et références à celles-ci
ms.date: 08/11/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, déplacement, transfert, catégories de valeurs, sémantique de déplacement, transfert parfait, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a11d7763c33df6733a8dbf78392d27417e7cf18d
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270206"
---
# <a name="value-categories-and-references-to-them"></a>Catégories de valeurs et références à celles-ci
Cette rubrique décrit les différentes catégories de valeurs (et les références aux valeurs) qui existent en C++. Vous aurez sans doute entendu parler de *lvalues* et de *rvalues*, mais vous ne les considérez peut-être pas dans les mêmes termes que ceux présentés par cette rubrique. Et il existe également d’autres types de valeurs.

Chaque expression en C++ génère une valeur qui appartient à l’une des catégories présentées dans cette rubrique. Certains aspects du langage C++, ses installations et ses règles exigent une bonne compréhension de ces catégories de valeurs et les références à celles-ci. Par exemple, prendre l’adresse d’une valeur, copier une valeur, déplacer une valeur et transférer une valeur à une autre fonction. Cette rubrique n’aborde pas tous ces aspects en détail, mais elle fournit des informations de base permettant de bien les comprendre.

Les informations fournies dans cette rubrique sont encadrées en termes d’analyse de catégories de valeur de Stroustrup par les deux propriétés indépendantes d’identité et de mobilité [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Une lvalue a une identité
Que signifie, pour une valeur, d’avoir une *identité* ? Si vous avez (ou que vous pouvez prendre) l’adresse mémoire d’une valeur et l’utiliser de manière sécurisée, la valeur a une identité. Ainsi, vous pouvez effectuer bien plus que comparer le contenu de valeurs : vous pouvez les comparer ou les différencier par identité.

Une *lvalue* a une identité. Le fait que le « l » dans « lvalue » soit l’abréviation de « left » (gauche) (comme dans le côté gauche d’une assignation) ne présente désormais qu’un intérêt purement historique. En C++, une lvalue peut apparaître à gauche *ou* à droite d’une assignation. Le « l » dans « lvalues » ne vous permet pas vraiment de comprendre ni de définir ce qu’elles sont. Vous devez seulement comprendre que ce que nous appelons une lvalue est une valeur qui a une identité.

Une variable ou une constante nommée, ou encore une fonction qui retourne une référence constituent des exemples d’expressions qui sont des lvalues. Un élément temporaire ou une fonction qui produit un retour par valeur constituent des exemples d’expressions qui en sont *pas* des lvalues.

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

Maintenant, même s’il est vrai que les lvalues ont une identité, c’est également le cas des xvalues. Nous examinerons plus en détail ce qu’est une *xvalue* plus loin dans cette rubrique. Pour le moment, sachez simplement qu’il existe une catégorie de valeurs appelée glvalue, pour « generalized lvalue » (lvalue généralisée). Le sur-ensemble des glvalues contient à la fois les lvalues (également appelées *lvalues classiques*) et les xvalues. Par conséquent, même s’il est vrai qu’« une lvalue a une identité », l’ensemble complet des éléments ayant une identité est l’ensemble des glvalues, comme indiqué dans cette illustration.

![Une lvalue a une identité](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Une rvalue est déplaçable ; une lvalue ne l’est pas
Il y a toutefois des valeurs qui ne sont pas des glvalues. Par conséquent, il existe des valeurs pour lesquelles vous *ne pouvez pas* obtenir une adresse de mémoire (ou sur la validité desquelles vous ne pouvez pas compter). Nous avons vu de telles valeurs dans l’exemple de code ci-dessus. Cela peut paraître un inconvénient. Mais, en réalité, l’avantage d’une valeur comme celle-ci est que vous pouvez *vous déplacer* à partir de celle-ci (ce qui est généralement économique), au lieu de copier à partir de celle-ci (ce qui est généralement coûteux). Un déplacement à partir d’une valeur signifie qu’elle n’est plus à l’emplacement où elle se trouvait avant. Par conséquent, il faut éviter de tenter d’y accéder à cet emplacement. Quand et *comment* déplacer une valeur n’est pas abordé dans cette rubrique. Dans cette rubrique, nous devons juste savoir qu’une valeur qui peut être déplacée est appelée *rvalue* (ou *rvalue classique*).

Le « r » dans « rvalue » est l’abréviation de « right » (droite) comme dans le côté droit d’une assignation). Mais vous pouvez utiliser des rvalues et des références à des rvalues en dehors des affectations. Le « r » dans « rvalues » n’est donc pas le point sur lequel il faut se concentrer. Vous devez seulement comprendre que ce que nous appelons une rvalue est une valeur qui est déplaçable.

À l’inverse, une lvalue n’est pas déplaçable, comme le montre cette illustration. Une lvalue déplacée défierait la définition d’une *lvalue*, ce qui constituerait un problème inattendu pour le code qui, de façon très raisonnable, était supposé pouvoir continuer à accéder à la lvalue.

![Une rvalue est déplaçable ; une lvalue ne l’est pas](images/is-movable.png)

Vous ne pouvez pas déplacer une lvalue. Mais il *existe* une sorte de glvalue (l’ensemble d’éléments ayant une identité) que vous pouvez déplacer, si vous savez ce que vous faites (en veillant notamment à ne pas y accéder après le déplacement) : il s’agit de la xvalue. Nous reviendrons à ce concept une fois ci-dessous, quand nous examinerons la vue complète des catégories de valeurs.

## <a name="rvalue-references-and-reference-binding-rules"></a>Références rvalue et règles de liaison des références
Cette section présente la syntaxe d’une référence à une rvalue. Nous allons devoir attendre une autre rubrique pour examiner un traitement substantiel de déplacement et de transfert, mais ce sont des problèmes qui sont résolus par les références rvalue. Toutefois, avant d’étudier les références rvalue, nous devons d’abord clarifier ce qu’est `T&`, l’élément que nous avons auparavant simplement appelé « référence ». Il s’agit réellement d’« une référence lvalue (non-const) », qui fait référence à une valeur à laquelle l’utilisateur de la référence peut écrire.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Une référence lvalue peut établir une liaison à une lvalue, mais pas à une rvalue.

Ensuite, il existe des références const lvalue (`T const&`), qui font référence à des objets auxquels l’utilisateur de la référence *ne peut pas* écrire (par exemple, une constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Une référence const lvalue peut établir une liaison à une lvalue ou à une rvalue.

La syntaxe d’une référence à une rvalue de type `T` est écrite sous la forme `T&&`. Une référence rvalue fait référence à une valeur déplaçable, c’est-à-dire une valeur dont que nous n’avons pas besoin de conserver le contenu une fois que nous l’avons utilisée (par exemple, un élément temporaire). Étant donné que l’objectif principal est de se déplacer à partir la valeur liée à une référence rvalue (en la modifiant donc), les qualificateurs `const` et `volatile` (également appelés « qualificateurs cv ») ne s’appliquent pas aux références rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Une référence rvalue établit une liaison à une rvalue. En réalité, en termes de résolution de surcharge, une rvalue *préfère* être liée à une référence rvalue plutôt qu’à une référence const lvalue. Mais une référence rvalue ne peut pas établir une liaison à une lvalue car, comme nous l’avons dit, une référence rvalue fait référence à une valeur dont on présume que nous n’avons pas besoin de conserver le contenu (par exemple, le paramètre d’un constructeur de déplacement).

Vous pouvez également transmettre une rvalue où un argument par valeur est attendu, par le biais d’une construction de copie (ou par le biais d’une construction de déplacement, si la rvalue est une xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Une glvalue a une identité ; une prvalue n’en a pas
À ce stade, nous savons ce qu’est une identité. Et nous savons ce qui est déplaçable et ce qui ne l’est pas. Mais nous n’avons pas encore nommé l’ensemble des valeurs qui n’ont*pas* d’identité. Cet ensemble est appelé *prvalue*, ou *rvalue pure*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Une lvalue a une identité ; une prvalue n’en a pas](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>Vue complète des catégories de valeurs
Il reste uniquement à regrouper les informations et les illustrations ci-dessus en une seule vue complète.

![Vue complète des catégories de valeurs](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Une glvalue (lvalue généralisée) a une identité.

### <a name="lvalue-im"></a>lvalue (i\&\!m)
Une lvalue (sorte de glvalue) a une identité, mais ne peut pas être déplacée. Il s’agit de valeurs généralement en lecture-écriture que vous passez par référence ou par référence const, ou encore par valeur si la copie est économique. Une lvalue ne peut pas être liée à une référence rvalue.

### <a name="xvalue-im"></a>xvalue (i\&m)
Une xvalue (sorte de glvalue, mais également sorte de rvalue) a une identité et peut aussi être déplacée. Ce peut être une ancienne lvalue que vous avez décidé de déplacer car la copie est coûteuse, et vous veillerez à ne pas y accéder par la suite. Voici comment vous pouvez transformer une lvalue en xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

Dans l’exemple de code ci-dessus, nous n’avons encore rien déplacé. Nous avons juste créé une xvalue en effectuant un cast d’une lvalue en référence rvalue sans nom. Elle peut toujours être identifiée par son nom d’lvalue ; mais en tant que xvalue, elle est désormais *capable* d’être déplacée. Les raisons de le faire et ce à quoi ressemble un déplacement réellement feront l’objet d’une autre rubrique. Mais si cela vous aide, vous pouvez considérer que le « x » de « xvalue » signifie « expert seulement ». En effectuant un cast d’une lvalue en xvalue (sorte de rvalue), la valeur devient capable d’être liée à une référence rvalue.

Voici deux autres exemples de xvalues : un appelant une fonction qui retourne une référence rvalue sans nom et un accédant à un membre d’une xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\&m)
Une prvalue (rvalue pure ; sorte de rvalue) n’a pas d’identité, mais est déplaçable. Il s’agit généralement d’éléments temporaires, le résultat de l’appel d’une fonction qui retourne par valeur ou le résultat de l’évaluation de toute autre expression qui n’est pas une glvalue.

### <a name="rvalue-m"></a>rvalue (m)
Une rvalue est déplaçable. Une *référence* rvalue fait toujours référence à une rvalue (valeur dont on présume que nous n’avons pas besoin de conserver le contenu).

Mais une référence rvalue est-elle elle-même une rvalue ? Un référence rvalue *sans nom* (comme celles indiquées dans les exemples de code xvalue ci-dessus) est une xvalue. Donc, oui, il s’agit d’une rvalue. Elle préfère être liée à un paramètre de fonction de référence rvalue, comme celle d’un constructeur de déplacement. À l’inverse (et peut-être contre toute attente), si une référence rvalue a un nom, l’expression composée de ce nom est une lvalue. Par conséquent, elle *ne peut pas* être liée à un paramètre de référence rvalue. Mais il est facile de faire en sorte qu’elle le soit : il vous suffit d’en effectuer à nouveau un cast en une référence rvalue sans nom (une xvalue).

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
Le type de valeur qui n’a pas identité et qui n’est pas déplaçable est la combinaison que nous n’avons pas encore abordée. Mais nous pouvons l’ignorer, car cette catégorie n’est pas un concept utile en langage C++.

## <a name="reference-collapsing-rules"></a>Règles de réduction de référence
Plusieurs références similaires dans une expression (une référence lvalue à une référence lvalue, ou une référence rvalue à une référence rvalue) s’annulent mutuellement.

- `A& &` est réduite en `A&`.
- `A&& &&` est réduite en `A&&`.

Plusieurs références différentes dans une expression sont réduites en une référence lvalue.

- `A& &&` est réduite en `A&`.
- `A&& &` est réduite en `A&`.

## <a name="forwarding-references"></a>Références de transfert
Cette dernière section compare des références rvalue, que nous avons déjà présentées, avec le nouveau concept de *référence de transfert*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` est une référence rvalue, comme nous l’avons vu. Const et volatile ne s’appliquent pas aux références rvalue.
- `foo` accepte uniquement des rvalues de type **A**.
- La raison de l’existence de références rvalue (comme `A&&`) est de vous permettre de créer une autre surcharge qui est optimisée en cas de passage d’un élément temporaire (ou d’une autre rvalue).

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` est une *référence de transfert*. En fonction de ce que vous passez à `bar`, le type  **_Ty** peut être const/non-const indépendamment de volatile/non-volatile.
- `bar` accepte n’importe quelle lvalue ou rvalue de type  **_Ty**.
- Si une lvalue est passée, la référence de transfert devient `_Ty& &&`, qui est réduite en référence lvalue `_Ty&`.
- Si une rvalue est passée, la référence de transfert devient `_Ty&& &&`, qui est réduite en référence rvalue `_Ty&&`.
- La raison de l’existence de références de transfert (comme `_Ty&&`) n’est *pas* l’optimisation, mais de prendre ce que vous leur passez et de le transférer de façon transparente et efficace. Vous rencontrerez probablement une référence de transfert uniquement si vous écrivez (ou étudiez attentivement) le code de la bibliothèque ; par exemple, une fonction d’usine qui effectue un transfert sur des arguments de constructeur.

## <a name="sources"></a>Sources
* \[Stroustrup, 2013\] B. Stroustrup: The C++ Programming Language, Fourth Edition. Addison-Wesley. 2013.
