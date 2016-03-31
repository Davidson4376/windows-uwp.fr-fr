---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Composition animations
description: Many composition object and effect properties can be animated using key frame and expression animations allowing properties of a UI element to change over time or based on a calculation.
---
# Composition animations

\[ Updated for UWP apps on Windows 10. For Windows 8.x articles, see the [archive](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Many composition object and effect properties can be animated using key frame and expression animations allowing properties of a UI element to change over time or based on a calculation. There are two types of animations, keyframe animations, represented by the [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830) class, and expression animations represented by the [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) class.

-   [Animatable Properties](./composition-animation.md#animatable_properties)
-   [KeyFrame Animations](./composition-animation.md#keyframe-animations)
    -   [Creating your animation and KeyFrames](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [KeyFrame Properties](./composition-animation.md#keyframe-properties)
    -   [Easing Functions](./composition-animation.md#easing-functions)
    -   [Starting and Stopping KeyFrame Animations](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [Animation Completion Events](./composition-animation.md#animation-completion-events)
-   [Expression Animations](./composition-animation.md#expression-animations)
    -   [Creating your Expression](./composition-animation.md#creating-your-expression)
    -   [Property Sets](./composition-animation.md#property-sets)
    -   [Expression KeyFrames](./composition-animation.md#expression_keyframes)

## Animatable Properties

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

To construct a KeyFrame Animation, use one of the constructor methods of the Compositor class that correlates to the structure type of the property you wish to animate.

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

For example, the following snippet creates a Vector3 keyframe animation:

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

Each KeyFrame is constructed by calling the InsertKeyFrame method of a KeyFrameAnimation and specifying two components and optionally a third component:

-   Time: normalized progress state of the KeyFrame between 0.0 – 1.0
-   Value: specific value of the animating value at the time state
-   (Optional) Easing function: function to describe interpolation between previous and current KeyFrame (discussed later on)

For example, the following snippet inserts a keyframe in a Vector3KeyFrameAnimation that will triggered half way through the animation:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### KeyFrame Properties

Once you've defined your KeyFrame Animation and the individual KeyFrames, you are able to define multiple properties of your animation:

-   DelayTime – time before an animation starts after StartAnimation() is called
-   Duration – duration of the animation
-   IterationBehavior – count or infinite repeat behavior for an animation
-   IterationCount – number of finite times a KeyFrame Animation will repeat
-   KeyFrame Count – number of KeyFrames in a particular KeyFrameAnimation
-   StopBehavior – specifies the behavior of an animating property value when StopAnimation is called.

For example, the following snippet sets the duration of the animation to five seconds:

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### Easing Functions

A key frame easing function, a CompositionEasingFunction, indicates how intermediate values progress from the previous key frame value to the current key frame value. If you do not provide an easing function for the KeyFrame, a default curve will be used.

There are two types of easing functions supported:

-   Linear ([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   Cubic Bezier ([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

To create an easing function, use either the [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) or [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791) method of the [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761) class:

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

Note: A useful tool for generating the points for your Cubic Bezier can be found here.

To add your easing function into your KeyFrame, simply add in the third parameter to the KeyFrame when inserting it into the Animation:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### Starting and Stopping KeyFrame Animations

After you have defined your animation, KeyFrames and properties, you are ready to connect the animation to the property you want to animate. You connect your animation to the property you plan to animate by calling [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840) on the [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) object the property belongs to.

The general syntax and an example is as follows:

```cs
targetVisual.StartAnimation(“Offset”, animation);
```

After starting your animation, you also have the ability to stop and disconnect it as well. This is done by using the [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841) method and specifying the property you want to stop animating.

For example:

```cs
targetVisual.StopAnimation(“Offset”);
```

### Animation Completion Events

KeyFrame animations have a defined end, unlike Expression Animations, which are conditional. Developers can determine when groups or a single KeyFrame animation completes by using batches. Batches can be created or retrieved, depending on the batch object type, and are used to aggregate the end state of animations. Scoped batches are created while Commit batches are retrieved, the use of these different batches are described later.

A batch completion event fires when all animations within the batch have completed. The time it takes for a batch's completion event to fire depends on the longest or most delayed animation in the batch. Aggregating end states is useful when you need to know when groups of select animations complete in order to schedule other work.

### Scoped batches

To aggregate a specific group or a single animation, you first need to create a Scoped batch by calling [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425).

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

After creating a Scoped batch, all started animations aggregate until the batch is explicitly suspended or ended using [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) or [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844).

Calling [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) stops aggregating animation end states until [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847) is called. This allows you to explicitly exclude content from a given batch.

```cs
myScopedBatch.Suspend();
```

In order to complete your batch you must call [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844). Without an End call, the batch will remain open forever-collecting objects.

```cs
myScopedBatch.End();
```

If you want to know when a single animation ends, you need to create a Scoped batch, start the animation and end the batch.

For example:

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation(“Opacity”, myAnimation);
myScopedBatch.End();
```

### Commit batches

A commit batch is a batch that is implicitly created during the commit cycle. The current Commit batch can be retrieved by calling [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430) at any time during the commit cycle. The commit cycle is defined as the time between updates from the compositor. Updates are queued until the system is ready to process the changes and draw bits to the screen. Titles do not control when commits happen. The Commit batch will aggregate all objects within the commit cycle, those before and after GetCommitBatch was called. There is only one Commit batch per commit cycle and you do not need to explicitly call [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) on the commit batch.

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### Batch states

To determine if a batch is open to aggregating animations you can use the **IsActive** property. If the **IsEnded** property return true, you cannot add an animation to that specific batch. Scoped batches are “ended” when they have been explicitly ended by calling the [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) method. Commit batches are ended when the current commit cycle has ended.

## Expression Animations

Expression Animations are animations that use a mathematical expression to specify how the animated value should be calculated for each frame. The expression language itself can reference properties from other composition objects. Unlike KeyFrame Animations, Expressions are not time-based and are processed each frame (if necessary). Expressions can be useful to describe more complex user experiences that can be processed by the Composition engine, for example sticky headers and parallax.

### Creating your Expression

To create your expression, you call [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002) on your Compositor object and specifying the expression you want to use:

```cs
var expression = _compositor.CreateExpressionAnimation(“INSERT_EXPRESSION_STRING”)
```

### Operators, Precedence and Associativity

The Expression language supports the following operators and adheres to operator precedence and associativity as defined in the C# Language Specification:

-   Unary (-)
-   Multiplicative (\* /)
-   Additive (- +)

The Expression language also supports the concept of per-component math operations. These member-access (x.y) operators are only supported on “primitive” types (Vector and Matrices). For example:

```cs
Offset.x + 5.0
```

### Property Parameters

One of the powerful components of the Expression language is being able use parameters as variables in the expression. The expression string can contain parameters which are replaced with either a constant value or are references to another object's property value when calculated. For example:

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

In this example ChildOffset and ParentOffset are parameters that represent the offset property of two visuals. You define parameters using the **Set\*Parameter** methods of the [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708) class:

-   To create a vector parameter you use [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx), [**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx), or [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter).
-   To create a matrix parameter you use [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx), or [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter).
-   to create a scalar parameter you use [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter).
-   To create a color parameter you use [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter).
-   To create a quaternion parameter you use [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter).
-   To create a reference parameter you use [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter).

In the Expression String above, we would need to create two parameters to define the two Visuals:

```cs
Expression.SetReferenceParameter(“ChildVisual”, childVisual);
Expression.SetReferenceParameter(“ParentVisual”, parentVisual);
```

### Expression Helper Functions

In addition to having access to operators and property parameters, you also have access to the full helper function library of the expression language. You can find the full list of helper functions in the remarks section of the [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) class, but here are a few:

-   Max (Scalar value1, Scalar value2)
-   Scale (Vector3 value, Scalar factor)
-   Transform(Vector2 value, Matrix 3x2 matrix)

Here’s a more complex expression string example that uses the Clamp helper function:

```cs
Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)
```

### Starting and stopping your Expression Animation

To start and stop your Expression Animations, you use the same functions ([**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840), [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841)) as you do with KeyFrame Animations. Note: Expression Animations will continue to run until they are stopped using **StopAnimation** – unlike KeyFrame Animations, they do not have a finite duration.

### Property Sets

In addition to being able to reference properties of other Composition objects, you also have the ability to create and store property values of your own that can be used across multiple animations. Property sets are represented by the [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772) class. Property sets allow you to create a property with a value and reference it later in an expression or even hook it up as the target of a KeyFrame Animation.

To create a property set, use the [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802) method of the Compositor class. For example:

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Once you’ve created your property set, you can add a property and value to it using one of the **Insert\*** methods of [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772). For example:

```cs
_sharedProperties.InsertVector3(“NewOffset”, offset);
```

After you've created your expression animation, you can reference properties from the property set in the expression string with the use of a reference parameter. For example:

```cs
var expression = _compositor.CreateExpressionAnimation(“sharedProperties.NewOffset”);
expression.SetReferenceParameter(“sharedProperties”, _sharedProperties);
```

### Expression KeyFrames

Earlier in this document, we described how you create KeyFrame Animations and their respective KeyFrames. In addition to making the traditional KeyFrame with a normalized time and value component, you are also able to define expression KeyFrames.

Instead of defining an explicit value for a particular time in the KeyFrame, we are able to use the expression syntax to describe how the value should be calculated at this particular point in the KeyFrame timeline. You are able to mix and match expression KeyFrames with basic KeyFrames in your KeyFrame Animation.

### Constructing Expression KeyFrames

Similar to traditional KeyFrames, expression KeyFrames are constructed by specifying 2 components:

-   Time: normalized time state of the KeyFrame between 0.0 – 1.0
-   Value: The expression string used to describe how the value should be calculated

For example, the following snippet uses a combination of both regular and expression KeyFrames:

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, “VisualBOffset.X / VisualAOffset.Y”);
animation.InsertKeyFrame(1.00f, 0.8f);
```

### Using current and starting values

In the expression language, it is possible to reference both the current and the starting value of an animation. These values are pre-fixed in the expression language with “this”:

-   this.CurrentValue
-   this.StartingValue

An example of using these in an Expression KeyFrame:

```cs
animation.InsertExpressionKeyFrame(0.0f, “this.CurrentValue + delta”);
```

### Conditional Expressions

In addition to supporting basic mathematical expressions, you can also define a conditional expression using a ternary operator:

```cs
(condition ? first_expression : second_expression)
```

Within the expressions themselves, the following conditional operators are supported in the condition statement:

-   Equals (==)
-   Not Equals (!=)
-   Less than (<)
-   Less than or equal to (<=)
-   Great than (>)
-   Great than or equal to (>=)

Finally, the following conjunctions are supported as operators or functions in the condition statement:

-   Not: ! / Not(bool1)
-   And: && / And(bool1, bool2)
-   Or: || / Or(bool1, bool2)

The following snippet shows an example of using conditionals in an expression:

```cs
var expression = _compositor.CreateExpressionAnimation(“target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f”);
```

 

 




<!--HONumber=Mar16_HO1-->
