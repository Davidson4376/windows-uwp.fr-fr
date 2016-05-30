---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animations de composition
description: De nombreuses propriétés d’objet et d’effet de composition peuvent être animées à l’aide d’animations par images clés et expressions, ce qui permet aux propriétés d’un élément d’interface utilisateur de changer dans le temps ou en fonction d’un calcul.
---
# Animations de composition

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

De nombreuses propriétés d’objet et d’effet de composition peuvent être animées à l’aide d’animations par images clés et expressions, ce qui permet aux propriétés d’un élément d’interface utilisateur de changer dans le temps ou en fonction d’un calcul. Il existe deux types d’animation, les animations par images clés, représentées par la classe [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830), et les animations par expressions, représentées par la classe [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817).

-   [Propriétés animables](./composition-animation.md#animatable_properties)
-   [Animations par images clés](./composition-animation.md#keyframe-animations)
    -   [Création de l’animation et des images clés](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [Propriétés d’image clé](./composition-animation.md#keyframe-properties)
    -   [Fonctions d’accélération](./composition-animation.md#easing-functions)
    -   [Démarrage et arrêt des animations par images clés](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [Événements d’achèvement d’animation](./composition-animation.md#animation-completion-events)
-   [Animations par expressions](./composition-animation.md#expression-animations)
    -   [Création de votre expression](./composition-animation.md#creating-your-expression)
    -   [Jeux de propriétés](./composition-animation.md#property-sets)
    -   [Images clés d’expression](./composition-animation.md#expression_keyframes)

## Propriétés animables

The following properties are animatable by associating them with a KeyFrame or Expression Animation:

-   CompositionColorBrush.Color
-   InsetClip.BottomInset
-   InsetClip.LeftInset
-   InsetClip.RightInset
-   InsetClip.TopInset
-   Visual.AnchorPoint
-   Visual.CenterPoint
-   Visual.Offset
-   Visual.Opacity
-   Visual.Orientation
-   Visual.RotationAngle
-   Visual.RotationAxis
-   Visual.Size
-   Visual.TransformMatrix
-   CompositionPropertySet

## KeyFrame Animations

KeyFrame Animations are time-based animations that use one or more key frames to specify how the animated value should change over time. The frames represent markers, allowing you to define what the animated value should be at a specific time.

### Creating your animation and KeyFrames

Pour construire une animation par images clés, utilisez l’une des méthodes Constructor de la classe Compositor qui est mise en corrélation avec le type de structure de la propriété que vous souhaitez animer.

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

Par exemple, l’extrait de code suivant crée une animation Vector3 par images clés :

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

Chaque image clé est construite en appelant la méthode InsertKeyFrame d’une KeyFrameAnimation et en spécifiant deux composants et éventuellement un troisième :

-   Time: normalized progress state of the KeyFrame between 0.0 – 1.0
-   Value: specific value of the animating value at the time state
-   (Optional) Easing function: function to describe interpolation between previous and current KeyFrame (discussed later on)

Par exemple, l’extrait de code suivant insère une image clé dans une Vector3KeyFrameAnimation qui sera déclenchée à mi-chemin de l’animation :

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### Propriétés d’image clé

Once you've defined your KeyFrame Animation and the individual KeyFrames, you are able to define multiple properties of your animation:

-   DelayTime – time before an animation starts after StartAnimation() is called
-   Duration – duration of the animation
-   IterationBehavior – count or infinite repeat behavior for an animation
-   IterationCount – number of finite times a KeyFrame Animation will repeat
-   KeyFrame Count – number of KeyFrames in a particular KeyFrameAnimation
-   StopBehavior – specifies the behavior of an animating property value when StopAnimation is called.

Par exemple, l’extrait de code suivant définit la durée de l’animation sur cinq secondes :

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### Fonctions d’accélération

A key frame easing function, a CompositionEasingFunction, indicates how intermediate values progress from the previous key frame value to the current key frame value. If you do not provide an easing function for the KeyFrame, a default curve will be used.

Deux types de fonction d’accélération sont pris en charge :

-   Linéaire ([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   Courbe de Bézier cubique ([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

Pour créer une fonction d’accélération, utilisez la méthode [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) ou [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791) de la classe [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761) :

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

Remarque : vous trouverez ici un outil utile permettant de générer les points de votre courbe de Bézier cubique.

Pour ajouter la fonction d’accélération à votre image clé, ajoutez simplement le troisième paramètre à l’image clé lors de son insertion dans l’animation :

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### Démarrage et arrêt des animations par images clés

Une fois que vous avez défini votre animation, les images clés et les propriétés, vous êtes prêt à connecter l’animation à la propriété que vous voulez animer. Vous connectez votre animation à la propriété que vous souhaitez animer en appelant la méthode [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840) dans l’objet [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) auquel appartient la propriété.

Syntaxe générale et exemple :

```cs
targetVisual.StartAnimation("Offset", animation);
```

Après le démarrage de votre animation, vous pouvez également l’arrêter et la déconnecter. Pour ce faire, utilisez la méthode [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841) et spécifiez la propriété souhaitée pour arrêter l’animation.

Par exemple :

```cs
targetVisual.StopAnimation("Offset");
```

### Événements d’achèvement d’animation

KeyFrame animations have a defined end, unlike Expression Animations, which are conditional. Developers can determine when groups or a single KeyFrame animation completes by using batches. Batches can be created or retrieved, depending on the batch object type, and are used to aggregate the end state of animations. Scoped batches are created while Commit batches are retrieved, the use of these different batches are described later.

Un événement d’achèvement de lot est déclenché lorsque toutes les animations au sein du lot sont terminées. Le temps nécessaire au déclenchement de l’événement d’achèvement d’un lot dépend de l’animation la plus longue ou la plus retardée du lot. L’agrégation des états de fin est utile lorsque vous devez savoir quand des groupes d’animations sélectionnées se terminent afin de planifier une autre tâche.

### Lots délimités

Pour agréger un groupe spécifique ou une animation unique, vous devez tout d’abord créer un lot délimité en appelant [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425).

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

Après la création d’un lot délimité, toutes les animations démarrées sont agrégées jusqu’à ce que le lot soit explicitement suspendu ou arrêté à l’aide de [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) ou [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844).

L’appel de [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) permet d’arrêter l’agrégation des états de fin de l’animation tant que [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847) n’est pas appelée. Cela vous permet d’exclure explicitement du contenu d’un lot donné.

```cs
myScopedBatch.Suspend();
```

Pour terminer votre lot, vous devez appeler [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844). Sans cet appel, le lot correspond à des objets ouverts en collecte permanente.

```cs
myScopedBatch.End();
```

Pour savoir quand se termine une animation unique, créez un lot délimité, démarrez l’animation, puis terminez le lot.

Par exemple :

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

### Lots de validation

Un lot de validation est un lot implicitement créé au cours du cycle de validation. Le lot de validation actuel peut être récupéré en appelant [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430) à tout moment du cycle de validation. Le cycle de validation est défini comme la durée comprise entre les mises à jour du compositeur. Updates are queued until the system is ready to process the changes and draw bits to the screen. Titles do not control when commits happen. Le lot de validation agrège tous les objets au sein du cycle de validation, ceux qui précèdent et suivent l’appel de GetCommitBatch. Il n’existe qu’un lot de validation par cycle de validation, et vous n’avez pas besoin d’appeler explicitement [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) dans le lot de validation.

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### États de lot

Pour déterminer si un lot est ouvert à l’agrégation des animations, vous pouvez utiliser la propriété **IsActive**. Si la propriété **IsEnded** retourne la valeur true, vous ne pouvez pas ajouter d’animation à ce lot spécifique. Les lots délimités sont « terminés » si vous les avez arrêtés explicitement en appelant la méthode [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844). Les lots de validation sont terminés lorsque le cycle de validation actuel prend fin.

## Expression Animations

Expression Animations are animations that use a mathematical expression to specify how the animated value should be calculated for each frame. The expression language itself can reference properties from other composition objects. Unlike KeyFrame Animations, Expressions are not time-based and are processed each frame (if necessary). Expressions can be useful to describe more complex user experiences that can be processed by the Composition engine, for example sticky headers and parallax.

### Création de votre expression

Pour créer votre expression, vous devez appeler [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002) dans votre objet compositeur en spécifiant l’expression que vous souhaitez utiliser :

```cs
var expression = _compositor.CreateExpressionAnimation("INSERT_EXPRESSION_STRING")
```

### Opérateurs, priorité et associativité

The Expression language supports the following operators and adheres to operator precedence and associativity as defined in the C# Language Specification:

-   Unaire (-)
-   Multiplicatif (\* /)
-   Additif (- +)

The Expression language also supports the concept of per-component math operations. These member-access (x.y) operators are only supported on “primitive” types (Vector and Matrices). Par exemple :

```cs
Offset.x + 5.0
```

### Paramètres de propriété

One of the powerful components of the Expression language is being able use parameters as variables in the expression. The expression string can contain parameters which are replaced with either a constant value or are references to another object's property value when calculated. Par exemple :

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

Dans cet exemple, ChildOffset et ParentOffset sont des paramètres qui représentent la propriété Offset de deux éléments visuels. Vous définissez les paramètres en utilisant les méthodes **Set\*Parameter** de la classe [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708) :

-   Pour créer un paramètre de vecteur, vous utilisez [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx), [**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx) ou [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter).
-   Pour créer un paramètre de matrice, vous utilisez [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx) ou [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter).
-   Pour créer un paramètre scalaire, vous utilisez [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter).
-   Pour créer un paramètre de couleur, vous utilisez [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter).
-   Pour créer un paramètre de quaternion, vous utilisez [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter).
-   Pour créer un paramètre de référence, vous utilisez [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter).

Dans la chaîne d’expression ci-dessus, nous devons créer deux paramètres pour définir les deux éléments visuels :

```cs
Expression.SetReferenceParameter("ChildVisual", childVisual);
Expression.SetReferenceParameter("ParentVisual", parentVisual);
```

### Expression : Fonctions d’assistance

Outre l’accès aux opérateurs et aux paramètres de propriété, vous avez accès à la bibliothèque complète des fonctions d’assistance du langage Expression. Vous trouverez la liste complète des fonctions d’assistance dans la section Remarques de la classe [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817), mais en voici quelques-unes :

-   Max (valeur scalaire1, valeur scalaire2)
-   Scale (Vector3 value, Scalar factor)
-   Transform(Vector2 value, Matrix 3x2 matrix)

Voici un exemple de chaîne d’expression plus complexe qui utilise la fonction d’assistance Clamp :

```cs
Clamp((scroller.Offset.y * -1.0) - container.Offset.y, 0, container.Size.y - header.Size.y)
```

### Démarrage et arrêt de l’animation par expressions

Pour démarrer et arrêter les animations par expressions, vous utilisez les fonctions ([**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840), [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841)) que vous utilisez avec les animations par images clés. Remarque : les animations par expressions continuent de s’exécuter tant qu’elles ne sont pas arrêtées à l’aide de **StopAnimation**, contrairement aux animations par images clés qui ont une durée limitée.

### Jeux de propriétés

En plus d’être en mesure de référencer les propriétés des autres objets de Composition, vous avez également la possibilité de créer et de stocker des valeurs de propriété qui peuvent être utilisées dans plusieurs animations. Les jeux de propriétés sont représentés par la classe [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772). Les jeux de propriétés vous permettent de créer une propriété dotée d’une valeur et de la référencer plus loin dans une expression, voire de l’accrocher comme cible d’une animation par images clés.

Pour créer un jeu de propriétés, utilisez la méthode [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802) de la classe Compositor. Par exemple :

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Une fois que vous avez créé votre jeu de propriétés, vous pouvez ajouter une propriété et la valeur correspondante en utilisant l’une des méthodes **Insert\*** de [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772). Par exemple :

```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

Après avoir créé votre animation par expressions, vous pouvez référencer des propriétés du jeu de propriétés dans la chaîne d’expression en utilisant un paramètre de référence. Par exemple :

```cs
var expression = _compositor.CreateExpressionAnimation("sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

### Images clés d’expression

Earlier in this document, we described how you create KeyFrame Animations and their respective KeyFrames. En plus d’associer l’image clé traditionnelle à un composant de temps et de valeur normalisé, vous pouvez également définir des images clés d’expression.

Au lieu de définir une valeur explicite pour une heure particulière dans l’image clé, nous pouvons utiliser la syntaxe d’expression pour décrire la façon dont la valeur doit être calculée à ce stade particulier de la chronologie de l’image clé. Vous pouvez combiner et assortir des images clés d’expression avec des images clés de base dans votre animation par images clés.

### Constructing Expression KeyFrames

Similar to traditional KeyFrames, expression KeyFrames are constructed by specifying 2 components:

-   Time: normalized time state of the KeyFrame between 0.0 – 1.0
-   Value: The expression string used to describe how the value should be calculated

Par exemple, l’extrait de code suivant utilise une combinaison d’images clés traditionnelles et d’expression :

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

### Utilisation des valeurs de départ et actuelles

In the expression language, it is possible to reference both the current and the starting value of an animation. These values are pre-fixed in the expression language with “this”:

-   this.CurrentValue
-   this.StartingValue

Exemple d’utilisation de ces valeurs dans une image clé d’expression :

```cs
animation.InsertExpressionKeyFrame(0.0f, "this.CurrentValue + delta");
```

### Expressions conditionnelles

Outre la prise en charge des expressions mathématiques de base, vous pouvez également définir une expression conditionnelle à l’aide d’un opérateur ternaire :

```cs
(condition ? first_expression : second_expression)
```

Dans les expressions proprement dites, les opérateurs conditionnels suivants sont pris en charge dans l’instruction de condition :

-   Equals (==)
-   Not Equals (!=)
-   Less than (&lt;)
-   Less than or equal to (&lt;=)
-   Great than (&gt;)
-   Great than or equal to (&gt;=)

Finally, the following conjunctions are supported as operators or functions in the condition statement:

-   Not: ! / Not(bool1)
-   And: &amp;&amp; / And(bool1, bool2)
-   Or: || / Or(bool1, bool2)

L’extrait de code suivant illustre un exemple d’utilisation des instructions conditionnelles dans une expression :

```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f");
```

 

 






<!--HONumber=May16_HO2-->


