---
author: stevewhims
description: Cette rubrique décrit les différentes catégories de valeurs qui existent dans C++. Vous serez sans doute avez-vous entendu parler de lvalues et rvalues, mais il existe d’autres types, trop.
title: Catégories de valeur et les références correspondantes
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c ++, RPC, winrt, projection, en déplaçant, transfert, des catégories de valeur, sémantique de déplacement, transfert parfait, lvalue, rvalue, glvalue, prvalue, valeur x
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2881712"
---
# <a name="value-categories-and-references-to-them"></a>Catégories de valeur et les références correspondantes
Cette rubrique décrit les différentes catégories de valeurs (et des références à des valeurs) qui existent dans C++. Vous serez sans doute avez-vous entendu parler de *lvalues* et *rvalues*, mais vous ne pouvez pas penser d'entre eux dans les conditions de cette rubrique présente. Et autres types de valeurs, trop.

Chaque expression en langage C++ génère une valeur qui appartienne à une des catégories décrites dans cette rubrique. Il existe des aspects du langage C++, son facilies et les règles, qui exigent une bonne compréhension de ces catégories de valeur et les références correspondantes. Par exemple, prenez l’adresse d’une valeur, la copie d’une valeur, une valeur de déplacement et de transfert d’une valeur à une autre fonction. Cette rubrique ne prennent tous ces aspects en profondeur, mais il fournit des informations de base pour une bonne compréhension d'entre eux.

Les informations de cette rubrique sont encadrée en termes d’analyse de Stroustrup des catégories de la valeur par les deux propriétés indépendamment de l’identité et movability [Stroustrup 2013].

## <a name="an-lvalue-has-identity"></a>Lvalue a une identité
Que signifie pour une valeur *d’identité*? Si vous avez (ou vous pouvez prendre) à l’adresse de la mémoire d’une valeur et l’utiliser en toute sécurité, puis la valeur a une identité. Ainsi, vous pouvez effectuer plusieurs compare le contenu de valeurs: vous pouvez comparer ou différencier suivant leur identité.

Une *lvalue* a identité. Il est désormais une question d’intérêt historique uniquement que le «l» dans «lvalue» est une abréviation de «left» (comme dans la gauche côté d’une affectation). En langage C++, lvalue peut apparaître sur la gauche *ou* droite d’une affectation. «L» dans «lvalues», puis, ne réellement vous aide à comprendre ni de ce qu’ils sont. Il vous suffit de comprendre que ce que nous appelons lvalue est une valeur qui a une identité.

Exemples d’expressions qui sont lvalues: une variable nommée ou une constante; ou une fonction qui renvoie une référence. Exemples d’expressions qui sont *pas* lvalues: temporaire; ou une fonction qui retourne par valeur.

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

À présent, alors qu’il est une instruction true qu’identité lvalues, cela xvalues. Nous allons passer plus dans quel une *valeur x* est plus loin dans cette rubrique. Pour l’instant, sachez simplement qu’il existe une catégorie valeur appelée glvalue, pour «généralisées lvalue». Le sur-ensemble des glvalues contient lvalues (également appelé *lvalues classique*) et xvalues. Par conséquent, tandis que «lvalue a identité» a la valeur true, l’ensemble complet des éléments dont l’identité est l’ensemble des glvalues, comme indiqué dans cette illustration.

![Lvalue a une identité](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Rvalue est mobile; lvalue n’est pas
Mais il existe des valeurs qui ne sont pas glvalues. Par conséquent, il existe des valeurs que vous *ne pouvez pas* obtenir une adresse de mémoire pour (ou si vous ne pouvez pas compter sur qu’elle soit valide). Nous avons constaté certaines de ces valeurs dans l’exemple de code ci-dessus. Cela représente un inconvénient. Mais en fait l’avantage d’une valeur qui est que vous pouvez *déplacer* à partir de celui-ci (qui est généralement peu coûteux), plutôt que copie à partir de celui-ci (qui est généralement coûteux). Passage d’une valeur signifie qu’il n’est plus place qu'auparavant. Essaie d’accéder aux place que ce n’est donc quelque chose à éviter. Une description des quand et *comment* déplacer qu'une valeur est hors de portée de cette rubrique. Pour cette rubrique, nous devons simplement savoir qu’une valeur qui est mobile est appelée une *rvalue* (ou *rvalue classique*).

«R» dans «rvalue» est l’abréviation de «droite» (comme dans le droit côté d’une affectation). Mais vous pouvez utiliser rvalues et des références aux valeurs r, en dehors des affectations. «R» dans les valeurs «r», puis, n'est pas la chose à consacrer. Il vous suffit de comprendre que ce que nous appelons rvalue est une valeur qui peut être déplacée.

Lvalue, à l’inverse, n’est pas mobile, comme indiqué dans cette illustration. Lvalue déplacé serait Relevez la définition de *lvalue*, et il serait un problème inattendu pour le code qui très raisonnablement puissent continuer à accéder à la valeur lvalue.

![Rvalue est mobile; lvalue n’est pas](images/is-movable.png)

Vous ne pouvez pas déplacer lvalue. Mais il *est* un type de glvalue (l’ensemble des éléments avec l’identité) que vous pouvez déplacer&mdash;si vous savez ce que vous faites (y compris en veillant à ne pas y accéder après le déplacement)&mdash;et qui est la valeur x. Nous y reviendrons cette idée une nouvelle fois ci-dessous, lorsque nous examinons l’image complète des catégories de valeur.

## <a name="rvalue-references-and-reference-binding-rules"></a>Références rvalue et les règles de liaison de référence
Cette section présente la syntaxe d’une référence à une valeur rvalue. Nous aurons d’attente d’une autre rubrique dans un traitement substantiel de déplacement et de transfert, mais il s’agit des problèmes sont résolus par références rvalue. Avant de consulter les références de rvalue, cependant, nous devons d’abord être plus clair sur `T&` &mdash;la chose que nous avons auparavant été appelant simplement «une référence». C’est vraiment «une lvalue (non const) référence», qui fait référence à une valeur à laquelle l’utilisateur de la référence peut écrire.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Une référence lvalue peut lier à lvalue, mais pas à une valeur rvalue.

Puis il existe des références const lvalue (`T const&`), qui font référence aux objets auxquels l’utilisateur de la référence *ne peut pas* écrire (par exemple, une constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Une référence const lvalue peut lier à lvalue ou à une valeur rvalue.

La syntaxe d’une référence à une valeur rvalue de type `T` est écrit en tant que `T&&`. Une référence rvalue fait référence à une valeur mobile&mdash;une valeur dont le contenu nous n’avez pas besoin de conserver une fois que nous avons utilisé (par exemple, une variable temporaire). Depuis l’objectif est de transition de (modification ainsi) la valeur liée à une référence rvalue, `const` et `volatile` qualificateurs (également appelé qualificateurs cv) ne s’appliquent pas aux références rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Une référence rvalue lie à rvalue. En fait, en termes de résolution de surcharge, une rvalue *préfère* être lié à une référence rvalue à une référence const lvalue. Mais une référence rvalue ne peut pas lier à lvalue car, comme nous l’avons dit, une référence rvalue fait référence à une valeur dont il est supposé n’est pas nécessaire (par exemple, le paramètre pour un constructeur de déplacement) de conserver le contenu.

Vous pouvez également passer rvalue où un argument par valeur est attendu, par le biais de copier la construction (ou par le biais de construction de déplacement, si la valeur rvalue est une valeur x).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Un glvalue a une identité; n’est pas le cas d’un prvalue
À ce stade, nous savons que ce qui a une identité. Et nous savons What ' s mobile et ce qui n’est pas. Mais nous n’avez pas encore nommé l’ensemble de valeurs qui *ne* possèdent identité. Cet ensemble est appelé *prvalue*, ou *rvalue pur*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Lvalue a une identité; n’est pas le cas d’un prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>L’image complète des catégories de valeur
Il reste uniquement pour combiner les informations et les illustrations ci-dessus dans une image unique, grande.

![L’image complète des catégories de valeur](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Un glvalue (lvalue généralisé) a l’identité.

### <a name="lvalue-im"></a>lvalue (i\ et \!m)
Lvalue (un type de glvalue) a une identité, mais n’est pas mobile. Il s’agit généralement en lecture-écriture des valeurs que vous passez autour par référence par référence const ou par valeur si la copie est peu coûteux. Lvalue ne peut pas être liée à une référence rvalue.

### <a name="xvalue-im"></a>valeur x (i\ & m)
Une valeur x (un type de glvalue, mais également un type de valeur rvalue) a une identité et peut également être déplacé. Cela peut être une lvalue premier que vous avez décidé de déplacer, car la copie est coûteuse, et vous pouvez veiller à ne pas y accéder par la suite. Voici comment vous pouvez transformer une lvalue en une valeur x.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

Dans l’exemple ci-dessus, nous n’avons pas déplacé rien encore. Nous venons de créer une valeur x en effectuant un cast lvalue à une référence rvalue sans nom. Il peut toujours être identifié par son nom lvalue; mais, comme une valeur x, il est désormais *capable* d’en cours de déplacement. Les raisons pour ce faire, et le déplacement réellement l’aspect, doivent attendre pour une autre rubrique. Mais vous pouvez considérer la «x» dans la valeur «x» en tant que le sens «expert uniquement» si cela permet d’afficher. En effectuant un cast lvalue en une valeur x (il s’agit d’un type de valeur rvalue), la valeur devient alors capable de lié à une référence rvalue.

Voici deux autres exemples de xvalues&mdash;appel d’une fonction qui renvoie une référence rvalue sans nom et l’accès à un membre d’une valeur x.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Un prvalue (rvalue pur; un type de valeur rvalue) ne possède pas identité, mais peut être déplacé. Il s’agit généralement temporaires, le résultat de l’appel à une fonction qui renvoie à la valeur, ou le résultat de l’évaluation de toute autre expression n’est pas un glvalue,

### <a name="rvalue-m"></a>rvalue (m)
Rvalue peut être déplacée. Une rvalue *référence* correspond toujours à rvalue (une valeur dont le contenu qu’il est supposé que nous n’êtes pas obligé de conserver).

Mais, est une référence rvalue lui-même rvalue? Une référence rvalue *sans nom* (comme celles indiquées dans les exemples de code valeur x ci-dessus) est une valeur x, Oui, il est donc rvalue. Il souhaite être lié à un paramètre de fonction référence rvalue, par exemple celui d’un constructeur de déplacement. À l’inverse (et éventuellement counter-intuitively), si une référence rvalue a un nom, puis l’expression constituée de ce nom est lvalue. Ainsi, il *ne peut pas* être lié à un paramètre de référence rvalue. Mais il est facile de rendre le faire&mdash;simplement convertissez-la dans une référence rvalue sans nom (une valeur x) à nouveau.

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
Le type de valeur identity ne possède pas et n’est pas mobile est la combinaison d’un, nous n’avons pas encore traité. Mais nous pouvons ignorer, car cette catégorie n’est pas une idée utile en langage C++.

## <a name="reference-collapsing-rules"></a>Réduction d’une référence de règles
Plusieurs références like dans une expression (une référence lvalue à une référence lvalue, ou une référence rvalue à une référence rvalue) annuler une sortie d’une autre.

- `A& &` réduire dans `A&`.
- `A&& &&` réduire dans `A&&`.

Plusieurs Contrairement aux références dans une expression réduire à une référence lvalue.

- `A& &&` réduire dans `A&`.
- `A&& &` réduire dans `A&`.

## <a name="forwarding-references"></a>Références de transfert
Cette dernière section compare les références de rvalue, nous avons déjà vu, avec le concept d’une *référence de transfert*d’autre.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` est une référence rvalue, comme nous l’avons vu. Const et volatiles ne s’appliquent pas aux références rvalue.
- `foo` accepte uniquement rvalues de type **A**.
- Fait référence à la valeur rvalue raison (tel que `A&&`) existe afin que vous pouvez créer une surcharge qui est optimisée pour le cas d’une variable temporaire (ou autres rvalue) transmises.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` est une *référence de transfert*. Selon ce que vous passez à `bar`, type **_Ty** pourrait être const/non const indépendamment volatiles/non volatiles.
- `bar` accepte rvalue de type **_Ty**ni lvalue.
- Le passage d’une lvalue entraîne la référence de transfert de devenir `_Ty& &&`, ce qui réduit la référence lvalue `_Ty&`.
- Le passage d’une valeur rvalue entraîne la référence de transfert de devenir `_Ty&& &&`, ce qui réduit la référence rvalue `_Ty&&`.
- La raison pour laquelle les références de transfert (tel que `_Ty&&`) existent est *pas* d’optimisation, mais à prendre que vous passez à et transférer sur efficacement et en toute transparence. Vous êtes susceptible de rencontrer une référence de transfert uniquement si vous écrivez (ou mieux étudiez) code de bibliothèque&mdash;, par exemple, une fonction d’usine qui transmet les arguments de constructeur.

## <a name="sources"></a>Sources
* \[Stroustrup, 2013\] Stroustrup b: le langage de programmation C++, quatrième édition. Addison-Wesley. 2013.
