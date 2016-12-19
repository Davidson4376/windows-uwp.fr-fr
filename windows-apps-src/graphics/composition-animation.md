---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animations de composition
description: "De nombreuses propriétés d’objet et d’effet de composition peuvent être animées à l’aide d’animations par images clés et expressions, ce qui permet aux propriétés d’un élément d’interface utilisateur de changer dans le temps ou en fonction d’un calcul."
translationtype: Human Translation
ms.sourcegitcommit: 9ea05f7ba76c7813b200a4c8cd021613f980355d
ms.openlocfilehash: 72b70dd2ae4de385f2a4711477aebb6d7023158c

---
# <a name="composition-animations"></a>Animations de composition

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

L’API WinRT Windows.UI.Composition vous permet de créer, animer, transformer et manipuler les objets compositeur dans une couche API unifiée. Les animations de composition offrent un moyen puissant et efficace d’exécuter des animations dans l’interface utilisateur de votre application. Elles ont été entièrement conçues pour garantir l’exécution de vos animations à 60 FPS, indépendamment du thread d’interface utilisateur, et pour vous offrir la possibilité de créer des expériences totalement inédites en usant de nombreuses propriétés et entrées pour produire les animations en question.
Cette rubrique offre une vue d’ensemble des fonctionnalités disponibles vous permettant d’animer des propriétés de l’objet de composition.
Ce document suppose que vous êtes familiarisé avec les principes de base de la structure de couche visuelle. Pour plus d’informations, [cliquez ici](./composition-visual-tree.md). Il existe deux types d’animations de composition : les **animations par images clés** et les **animations par expressions**  

![Types d’animation](./images/composition-animation-types.png)  
   
 
## <a name="types-of-composition-animations"></a>Types d’animations de composition
Les **animations par images clés** vous fournissent des expériences d’animation *image par image* et ancrées dans le temps. Les développeurs peuvent définir des *points de contrôle* de manière explicite, décrivant les valeurs que doit avoir une propriété d’animation à des points spécifiques dans la chronologie d’animation. Plus important encore, vous pouvez utiliser des fonctions d’accélération (aussi appelées « interpolateurs ») pour décrire le mode de transition entre ces points de contrôle.  

Les **animations implicites** sont un type d’animation qui permet aux développeurs de définir des animations individuelles réutilisables ou une série d’animations indépendamment de la logique de base de l’application. Les animations implicites permettent aux développeurs de créer des *modèles* d’animation et de les établir avec des déclencheurs. Ces déclencheurs sont des modifications de propriétés qui résultent d’affectations explicites. Les développeurs peuvent définir un modèle en tant qu’animation unique ou groupe d’animations. Les groupes d’animations sont des collections de modèles d’animation qui peut être démarrés ensemble de manière explicite ou avec un déclencheur. Les animations implicites vous évitent de créer des KeyFrameAnimations explicites chaque fois que vous voulez modifier la valeur d’une propriété et la voir s’animer.

Les **animations par expressions** constituent un type d’animation, introduit dans la couche visuelle à l’occasion de la mise à jour Windows 10 de novembre (Build 10586). L’idée derrière les animations par expressions est qu’un développeur peut créer des relations mathématiques entre les propriétés [visuelles](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) et les valeurs discrètes, qui seront évaluées et mises à jour à chaque image. Les développeurs peuvent référencer des propriétés sur les objets de composition ou les jeux de propriétés, utiliser des assistants de fonction mathématique et même référencer des entrées pour établir ces relations mathématiques. Grâce aux expressions, certaines expériences telles que les effets parallaxes et les en-têtes rémanents deviennent possibles sur la plateforme Windows et affichent une grande fluidité.  

## <a name="why-composition-animations"></a>Pourquoi utiliser des animations de composition ?
**Performances**  
 La plupart des développeurs qui créent des d’applications Windows universelles exécutent le code sur le thread d’interface utilisateur. Pour vérifier que les animations s’exécutent sans problème sur l’ensemble des catégories d’appareils, le système effectue les calculs et le travail d’animation sur un thread indépendant afin de conserver les 60 FPS. Les développeurs sont ainsi assurés d’avoir des animations fluides tandis que leurs applications effectuent d’autres opérations complexes pour des expériences utilisateur avancées.    
 
**Possibilités**  
Les animations de composition de la couche visuelle ont pour objectif de faciliter la création d’interfaces utilisateur séduisantes. Notre intention est de proposer aux développeurs différents types d’animations qui facilitent la mise en œuvre de leurs brillantes idées.
 
   

**Création de modèles**  
 Toutes les animations de composition de la couche visuelle sont des modèles ; cela signifie que les développeurs peuvent utiliser une animation sur plusieurs objets sans avoir à créer des animations distinctes. Cela permet aux développeurs d’utiliser une même animation et d’en modifier légèrement les propriétés ou les paramètres pour répondre à d’autres besoins sans craindre de bloquer les animations précédentes.  

Vous pouvez écouter nos discussions //BUILD autour des [animations par expressions](https://channel9.msdn.com/events/Build/2016/P486), des [expériences interactives](https://channel9.msdn.com/Events/Build/2016/P405), des [animations implicites](https://channel9.msdn.com/events/Build/2016/P484) et des [animations connectées](https://channel9.msdn.com/events/Build/2016/P485) pour avoir un aperçu des possibilités.

Vous pouvez aussi consulter la page [Composition GitHub](http://go.microsoft.com/fwlink/?LinkID=789439) pour obtenir des exemples sur la façon d’utiliser les API, ainsi que des exemples qui donnent un aperçu très fidèle des API en action.
 
## <a name="what-can-you-animate-with-composition-animations"></a>Que peut-on animer à l’aide des animations de composition ?
Il est possible d’appliquer des animations de composition à la plupart des propriétés d’objets de composition, telles que [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) et **InsetClip**. Vous pouvez aussi appliquer des animations de composition aux effets de composition et aux jeux de propriétés. **Dès lors que vous avez choisi les éléments à animer, notez-en le type ; vous pourrez ainsi déterminer le type d’animation KeyFrame à créer ou en quel type votre expression doit se résoudre.**  
 
### <a name="visual"></a>Visual
|Propriétés Visual animables|  Type|
|------|------|
|AnchorPoint|   Vector2|
|CenterPoint|   Vector3|
|Offset|    Vector3|
|Opacity|   Scalar|
|Orientation|   Quaternion|
|RotationAngle| Scalar|
|RotationAngleInDegrees|    Scalar|
|RotationAxis|  Vector3|
|Scale| Vector3|
|Size|  Vector2|
|TransformMatrix*|  Matrix4x4|
*Si vous voulez animer l’ensemble de la propriété TransformMatrix en tant que Matrix4x4, vous devez pour cela utiliser un élément ExpressionAnimation. L’autre solution consiste à cibler des cellules individuelles de la matrice et à utiliser un élément KeyFrame ou ExpressionAnimation.  

### <a name="insetclip"></a>InsetClip
|Propriétés InsetClip animables|   Type|
|-------------------------------|-------|
|BottomInset|   Scalar|
|LeftInset| Scalar|
|RightInset|    Scalar|
|TopInset|  Scalar|

## <a name="visual-sub-channel-properties"></a>Propriétés de sous-canal Visual
Outre la possibilité d’animer les propriétés de l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx), vous pouvez aussi cibler les composants de *sous-canal* de ces propriétés pour les animations. Par exemple, supposons que vous voulez simplement animer le décalage (Offset) X d’un élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) plutôt que le décalage entier. L’animation peut cibler la propriété de décalage Vector3 ou le composant Scalar X de la propriété de décalage. Outre la possibilité de cibler un composant de sous-canal individuel d’une propriété, plusieurs composants peuvent être ciblés. Vous pouvez, par exemple, cibler les composants X et Y de Scale

|Propriétés animables du sous-canal de l’élément visuel|  Type|
|----------------------------------------|------|
|AnchorPoint.x, y|Scalar|
|AnchorPoint.xy|Vector2|
|CenterPoint.x, y, z|Scalar|
|CenterPoint.xy, xz, yz|Vector2|
|Offset.x, y, z|Scalar|
|Offset.xy, xz, yz|Vector2|
|RotationAxis.x, y, z|Scalar|
|RotationAxis.xy, xz, yz|Vector2|
|Scale.x, y, z|Scalar|
|Scale.xy, xz, yz|Vector2|
|Size.x, y|Scalar|
|Size.xy|Vector2|
|TransformMatrix._11 ... TransformMatrix._NN,|Scalar|
|TransformMatrix._11_12 ... TransformMatrix._NN_NN|Vector2|
|TransformMatrix._11_12_13 ... TransformMatrix._NN_NN_NN|Vector3|
|TransformMatrix._11_12_13_14|Vector4|
|Color*|    Colors (Windows.UI)|

*L’animation d’un sous-canal Color de la propriété Bush est légèrement différente. Vous attachez StartAnimation() à Visual.Brush et déclarez la propriété « Color » à animer dans le paramètre. (L’animation des couleurs est détaillée plus loin dans cet article)

## <a name="property-sets-and-effects"></a>Jeux de propriétés et effets
Outre l’animation des propriétés des éléments [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) et InsetClip, vous pouvez aussi animer les propriétés d’un élément PropertySet ou Effect. Pour les jeux de propriétés, vous devez définir une propriété et la stocker dans un jeu de propriété de composition CompositionPropertySet ; cette propriété peut ensuite être la cible d’une animation (et peut être référencée simultanément dans une autre animation). Ce processus sera décrit plus en détail dans les sections suivantes.  

Quant aux effets, vous pouvez définir les effets graphiques à l’aide des API d’effets de composition (pour la vue d’ensemble des effets, cliquez [ici](./composition-effects.md). Outre la définition des effets, vous pouvez animer les valeurs de propriété de l’effet. Pour ce faire, le composant de propriété de la propriété Brush doit être ciblé sur SpriteVisuals.

## <a name="quick-formula-getting-started-with-composition-animations"></a>Formule rapide : prise en main des animations de composition
Avant de vous plonger dans les détails sur la façon de construire et utiliser les différents types d’animations, voici une formule rapide et générale de création d’animations de composition.  
1.  Décidez quelle propriété, propriété de sous-canal ou effet vous souhaitez animer et notez le type sélectionné.  
2.  Créez un objet pour votre animation (il s’agira d’une animation par images clés ou d’une animation par expressions).  
    *  Pour les animations par images clés, assurez-vous de créer un type d’animation par images clés correspondant au type de propriété que vous souhaitez animer.  
    *  Il existe un seul type d’animation par expressions.  
3.  Définissez le contenu pour l’animation ; insérez vos images clés ou définissez la chaîne d’expression  
    *  Pour les animations par images clés, assurez-vous que la valeur de vos images clés sont du même type que la propriété que vous souhaitez animer.  
    *  Pour les animations par expressions, vérifiez que la chaîne d’expression se résout au même type que la propriété que vous voulez animer.  
4.  Lancez l’animation sur l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) dont vous voulez animer la propriété ; appelez StartAnimation et incluez en tant que paramètres le nom de la propriété que vous voulez animer (sous forme de chaîne) et l’objet de votre animation.  

```cs
// KeyFrame Animation Example to target Opacity property
// Step 2 - Create your animation object
var animation = _compositor.CreateScalarKeyFrameAnimation();
// Step 3 - Define Content
animation.InsertKeyFrameAnimation(1.0f, 0.2f); 
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", animation); 
  
// Expression Animation Example to target Opacity property
// Step 2 - Create your animation object
var expression = _compositor.CreateExpressionAnimation(); 
// Step 3 - Define Content (you can also define the string as part of the expression object
// declaration)
expression.Expression = "targetVisual.Offset.X / windowWidth";
expression.SetReferenceParameter("targetVisual", _target);
expression.SetScalarParameter("windowWidth", _xSizeWindow);
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", expression);

```

## <a name="using-keyframe-animations"></a>Utilisation d’animations par images clés (KeyFrame)
Les animations par images clés sont des animations ancrées dans le temps, utilisant une ou plusieurs images clés pour spécifier la manière dont la valeur animée doit évoluer au fil du temps. Les images représentent des marqueurs ou des points de contrôle vous permettant de définir la valeur animée à un moment précis.  
 
### <a name="creating-your-animation-and-defining-keyframes"></a>Création de votre animation et définition des images clés
Pour construire une animation par images clés, utilisez la méthode Constructor de l’objet Compositor qui est mis en corrélation avec le type de la propriété que vous souhaitez animer. Les différents types d’animation par images clés sont :
*   ColorKeyFrameAnimation
*   QuaternionKeyFrameAnimation
*   ScalarKeyFrameAnimation
*   Vector2KeyFrameAnimation
*   Vector3KeyFrameAnimation
*   Vector4KeyFrameAnimation  

Exemple de création d’une animation Vector3KeyFrameAnimation :     
```cs
var animation = _compositor.CreateVector3KeyFrameAnimation(); 
```

Chaque animation par images clés est créée par l’insertion de segments d’images clés individuels qui définissent deux composants (avec un troisième composant facultatif)  
*   Time: normalized progress state of the KeyFrame between 0.0 – 1.0
*   Value: specific value of the animating value at the time state
*   (Facultatif) Fonction d’accélération : une fonction qui décrit l’interpolation entre l’image clé actuelle et l’image clé précédente (abordée plus tard)  

Exemple d’insertion d’une image clé au niveau du point à mi-chemin de l’animation :
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

**Remarque :** quand vous animez des couleurs à l’aide d’animations par images clés, gardez à l’esprit les points suivants :
1.  Attachez StartAnimation à Visual.Brush, et non à **Visual**, et déclarez le paramètre de propriété [Color](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) en tant que paramètre à animer.
2.  Le composant « value » de KeyFrame est défini par l’objet Colors à partir de l’espace de noms Windows.UI.
3.  Vous pouvez définir l’espace de couleurs par lequel passera l’interpolation en définissant la propriété InterpolationColorSpace. Les valeurs possibles sont :
    *   CompositionColorSpace.Rgb
    *   CompositionColorSpace.Hsl


## <a name="keyframe-animation-properties"></a>Propriétés des animations par images clés
Une fois que vous avez défini votre animation par images clés ainsi que les images clés individuelles, vous pouvez définir plusieurs propriétés de votre animation.
*   DelayTime – time before an animation starts after StartAnimation() is called
*   Duration – duration of the animation
*   IterationBehavior – count or infinite repeat behavior for an animation
*   IterationCount – number of finite times a KeyFrame Animation will repeat
*   KeyFrameCount : indique le nombre d’images clés que contient une animation par images clés
*   StopBehavior : spécifie le comportement d’une valeur de propriété d’animation quand StopAnimation est appelé  
*   Direction : indique le sens de lecture de l’animation  

Voici un exemple qui définit la propriété Duration de l’animation à 5 secondes :  
```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

## <a name="easing-functions"></a>Fonctions d’accélération
Les fonctions d’accélération (CompositionEasingFunction) indiquent comment les valeurs intermédiaires progressent de la valeur d’image clé précédente à la valeur d’image clé actuelle. If you do not provide an easing function for the KeyFrame, a default curve will be used.  
Deux types de fonction d’accélération sont pris en charge :
*   Linéaire
*   Courbe de Bézier cubique  
*   Étape  

Les courbes de Bézier cubiques sont des fonctions paramétriques souvent utilisées pour décrire des courbes lisses qui peuvent être mises à l’échelle. Lorsque vous utilisez des animations par images clés de composition, vous définissez deux points de contrôle qui sont des objets Vector2. Ces points de contrôle sont utilisés pour définir la forme de la courbe. Nous vous recommandons d’utiliser des sites similaires (comme [celui-ci](http://cubic-bezier.com/#0,-0.01,.48,.99)) pour visualiser la façon dont les deux points de contrôle créent une courbe de Bézier cubique.

Pour créer une fonction d’accélération, utilisez la méthode Constructor de l’objet Compositor. Voici deux exemples qui créent une fonction d’accélération linéaire et une fonction d’accélération de courbe de Bézier cubique de base.    
```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
var step = _compositor.CreateStepEasingFunction();
```
Pour ajouter la fonction d’accélération à votre image clé, ajoutez simplement le troisième paramètre à l’image clé lors de son insertion dans l’animation.   
Exemple d’ajout d’une fonction d’accélération easeIn avec l’image clé :  
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

## <a name="starting-and-stopping-keyframe-animations"></a>Démarrage et arrêt des animations par images clés
Une fois que vous avez défini l’animation et les images clés, l’animation peut être lancée. Au moment de lancer l’animation, vous devez spécifier l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) et la propriété cible qui doivent être animés, ainsi qu’une référence à l’animation. Pour ce faire, appelez la fonction StartAnimation(). Gardez à l’esprit qu’un appel de StartAnimation() sur une propriété déconnectera et supprimera toutes les animations déjà en cours d’exécution.  
**Remarque :** la référence à la propriété que vous choisissez d’animer se présente sous la forme d’une chaîne.  

Exemple de définition et de lancement d’une animation sur la propriété Offset de l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) :  
```cs
targetVisual.StartAnimation("Offset", animation);
```  

Pour cibler une propriété de sous-canal, ajoutez ce dernier à la chaîne définissant la propriété à animer. Dans les exemples ci-dessus, la syntaxe était changée en StartAnimation("Offset.X", animation2), où animation2 était une animation ScalarKeyFrameAnimation.  

Une fois votre animation lancée, il vous est possible de l’arrêter avant que celle-ci ne termine. Pour ce faire, utilisez la fonction StopAnimation().  
Exemple d’arrêt d’une animation sur la propriété Offset de l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) :    
```cs
targetVisual.StopAnimation("Offset");
```

Vous pouvez aussi définir le comportement de l’animation quand celle-ci est arrêtée explicitement. Pour ce faire, définissez la propriété StopBehavior de votre animation. Il existe trois options possibles :
*   LeaveCurrentValue : l’animation marquera la valeur de la propriété animée comme devant être la dernière valeur calculée de l’animation
*   SetToFinalValue : l’animation marquera la valeur de la propriété animée comme devant être la valeur de la dernière image clé
*   SetToInitialValue : l’animation marquera la valeur de la propriété animée comme devant être la valeur de la première image clé  

Exemple de définition de propriété StopBehavior pour une animation par images clés :  
```cs
animation.StopBehavior = AnimationStopBehavior.LeaveCurrentValue;
```

## <a name="animation-completion-events"></a>Événements d’achèvement d’animation
Grâce aux animations par images clés, les développeurs peuvent utiliser la fonction AnimationBatch pour agréger une ou plusieurs animations lorsque celles-ci sont terminées. Seuls les événements d’achèvement d’animation par images clés peuvent être agrégés en lot. Les expressions n’ont pas de fin déterminée ; elles ne déclenchent donc pas d’événement d’achèvement. Si une animation par expressions est lancée au sein d’un lot, celle-ci s’exécutera comme prévu et ne sera pas concernée par le déclenchement du lot     

Un événement d’achèvement de lot est déclenché lorsque toutes les animations au sein du lot sont terminées. Le temps nécessaire au déclenchement de l’événement d’un lot dépend de l’animation la plus longue ou la plus retardée du lot.
L’agrégation des états de fin est utile lorsque vous devez savoir quand des groupes d’animations sélectionnées se terminent afin de planifier une autre tâche.  

Les lots seront supprimés lors du déclenchement de l’événement d’achèvement. Vous pouvez également appeler Dispose() à tout moment pour libérer les ressources plus tôt. Vous pouvez choisir de supprimer manuellement l’objet lot si une animation par lot se termine tôt et que vous ne souhaitez pas recueillir l’événement d’achèvement. Si une animation est interrompue ou annulée, l’événement d’achèvement se déclenche et sera pris en compte dans son lot. Pour en voir un exemple, consultez le Kit de développement logiciel (SDK) Animation_Batch sur le [GitHub Windows/Composition](http://go.microsoft.com/fwlink/p/?LinkId=789439).  
 
## <a name="scoped-batches"></a>Lots délimités
Pour agréger un groupe d’animations spécifique ou pour cibler l’événement d’achèvement d’une animation unique, créez un lot délimité.    
```cs
CompositionScopedBatch myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
``` 
Après la création d’un lot délimité, toutes les animations démarrées sont agrégées jusqu’à ce que le lot soit explicitement suspendu ou arrêté à l’aide des fonctions Suspend ou End.    

L’appel de la fonction Suspend permet d’arrêter l’agrégation des états de fin de l’animation tant que Resume n’est pas appelée. Cela vous permet d’exclure explicitement du contenu d’un lot donné.  

Dans l’exemple ci-dessous, l’animation ciblant la propriété Offset de VisualA ne sera pas incluse dans le lot :  
```cs
myScopedBatch.Suspend();
VisualA.StartAnimation("Offset", myAnimation);
myScopeBatch.Resume();
```

Pour terminer votre lot, vous devez appeler End(). Sans cet appel, le lot restera ouvert et collectera des objets en permanence.  
 
L’extrait de code et le diagramme ci-dessous montrent un exemple d’agrégation des animations par le lot afin d’effectuer le suivi des états de fin. Notez que, dans cet exemple, les états de fin des animations 1, 3 et 4 feront l’objet d’un suivi dans ce lot, mais pas l’animation 2.  
```cs
myScopedBatch.End();
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
// Start Animation1
[…]
myScopedBatch.Suspend();
// Start Animation2 
[…]
myScopedBatch.Resume();
// Start Animation3
[…]
// Start Animation4
[…]
myScopedBatch.End();
```  
![Le lot délimité contient les animations 1, 3 et 4, mais pas l’animation 2.](./images/composition-scopedbatch.png)
 
## <a name="batching-a-single-animations-completion-event"></a>Traitement par lot d’un événement d’achèvement d’une animation unique
Pour savoir quand une animation unique se termine, créez un lot délimité comprenant uniquement l’animation que vous ciblez. Par exemple :  
```cs
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

## <a name="retrieving-a-batchs-completion-event"></a>Récupération des événements d’achèvement d’un lot

Lors du traitement par lot d’une ou plusieurs animations, la récupération de l’événement d’achèvement du lot se fait de la même manière. Vous inscrivez la méthode de gestion d’événement pour l’événement Terminé du lot ciblé.  

```cs
myScopedBatch.Completed += OnBatchCompleted;
``` 

## <a name="batch-states"></a>États de lot
Vous pouvez utiliser deux propriétés pour déterminer l’état d’un lot existant ; IsActive et IsEnded.  

La propriété IsActive retourne la valeur true si un lot ciblé est ouvert à l’agrégation des animations. IsActive retourne la valeur false lorsqu’un lot est suspendu ou terminé.   

Si la propriété IsEnded retourne la valeur true, vous ne pouvez pas ajouter d’animation à ce lot spécifique. Un lot sera terminé lorsque vous appelez End() explicitement pour un lot spécifique.  
 
## <a name="using-expression-animations"></a>Utilisation des animations par expressions
Les animations par expressions constituent un nouveau type d’animation, introduit par l’équipe de composition lors de la mise à jour Windows 10 de novembre (Build 10586). D’un point de vue général, les animations par expressions sont basées sur des équations mathématiques/des relations entre des valeurs discrètes et des références à d’autres propriétés d’objet de composition. Contrairement aux animations par images clés, qui utilisent une fonction d’interpolateur (courbe de Bézier cubique, Quad, Quintic, etc.) pour décrire la façon dont la valeur change au fil du temps, les animations par expressions utilisent une équation mathématique pour définir la façon dont la valeur animée est calculée pour chaque image. Il est important de souligner que les animations par expressions n’ont pas de durée définie ; une fois démarrées, elles s’exécutent et utilisent des équations mathématiques pour déterminer la valeur de la propriété d’animation jusqu’à ce qu’elles soient explicitement arrêtées.

**En quoi les animations par expressions sont-elles utiles ?** La puissance réelle des animations par expressions provient de leur aptitude à créer une relation mathématique qui inclut des références à des paramètres ou des propriétés sur d’autres objets. Cela signifie qu’une équation peut faire référence à des valeurs de propriétés sur d’autres objets de composition, à des variables locales, voire à des valeurs partagées dans les jeux de propriétés de composition. En raison de ce modèle de référence et du fait que l’équation est évaluée à chaque image, si les valeurs qui définissent une équation changent, la sortie de cette dernière changera également. Cela crée de nouvelles perspectives au-delà des animations par images clés traditionnelles, dans lesquelles les valeurs doivent être discrètes et prédéfinies. Par exemple, des expériences comme les en-têtes rémanents et l’effet parallaxe peuvent être décrites facilement à l’aide des animations par expressions.

**Remarque :** nous employons les termes « Expression » ou « Chaîne d’expression » en tant que référence à l’équation mathématique qui définit votre objet d’animation par expressions.

## <a name="creating-and-attaching-your-expression-animation"></a>Création et attachement de l’animation par expressions
Avant de nous plonger dans la syntaxe de création d’animations par expressions, passons en revue les principes fondamentaux suivants :  
*   Les animations par expressions utilisent une équation mathématique définie pour déterminer la valeur de la propriété d’animation à chaque image.
*   L’équation mathématique est entrée dans l’Expression en tant que chaîne.
*   La sortie de l’équation mathématique doit se résoudre vers le même type que la propriété que vous souhaitez animer. Si les types ne sont pas identiques, une erreur apparaît lorsque l’Expression est calculée. Si l’équation se résout en Nan (nombre/0), le système utilisera la dernière valeur calculée précédemment.
*   Les animations par expressions ont une *durée de vie infinie* ; elles s’exécuteront jusqu’à leur arrêt manuel.  

Pour créer votre animation par expressions, utilisez simplement le constructeur de votre objet de composition, dans lequel vous définissez votre expression mathématique.  
 
Voici un exemple de constructeur dans lequel une expression très basique combine deux valeurs Scalar. (Nous aborderons des expressions plus complexes dans la section suivante) :  
```cs
var expression = _compositor.CreateExpressionAnimation("0.2 + 0.3");
```
À l’instar des animations par images clés, une fois que vous avez défini votre animation par expressions, vous devez l’attacher à l’élément Visual et déclarer la propriété qui doit être animée par l’animation. L’exemple qui suit constitue la suite de l’exemple ci-dessus ; nous attachons notre animation par expressions à la propriété Opacity de l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) (de type Scalar) :  
```cs
targetVisual.StartAnimation("Opacity", expression);
```

## <a name="components-of-your-expression-string"></a>Composants de la chaîne d’expression
Dans l’exemple de la section précédente, deux valeurs Scalar simples ont été ajoutées ensemble. Bien qu’il s’agisse d’un exemple valable d’Expression, il n’illustre pas l’ensemble des manipulations que permettent les expressions. Il est important de souligner que les valeurs de l’exemple ci-dessus sont des valeurs discrètes ; de ce fait, chaque image de l’équation se résout en 0.5, ce qui ne changera pas pendant toute la durée de vie de l’animation. Le potentiel réel des Expressions provient de la définition d’une relation mathématique dans laquelle les valeurs peuvent changer régulièrement, voire tout le temps.  
 
Passons en revue les différentes parties qui composent ces types d’Expressions.  

### <a name="operators-precedence-and-associativity"></a>Opérateurs, priorité et associativité
La chaîne d’expression prend en charge l’utilisation des opérateurs standard décrivant les relations mathématiques entre les différents composants de l’équation :  

|Catégorie|  Opérateurs|
|--------|-----------|
|Unaire| -|
|Multiplicatif|    * /|
|Additif|  + -|
|Mod| %|  

De même, quand l’Expression est évaluée, elle respecte la priorité et l’associativité des opérateurs telles qu’elles sont définies dans la spécification du langage C#. Autrement dit, elle respecte un ordre basique des opérations.  

Lorsque l’exemple ci-dessous sera évalué, les parenthèses seront résolues en premier, puis le reste de l’équation en fonction de l’ordre des opérations :  
```cs
"(5.0 * (72.4 – 36.0) + 5.0" // (5.0 * 36.4 + 5) -> (182 + 5) -> 187
```

### <a name="property-parameters"></a>Paramètres de propriété
Les paramètres de propriété sont l’un des composants les plus puissants des animations par expressions. Dans la chaîne d’expression, vous pouvez référencer les valeurs des propriétés à partir d’autres objets, tels que [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx), CompositionPropertySet et d’autres objets C#.   

Pour utiliser ces éléments dans une chaîne d’expression, définissez simplement les références en tant que paramètres de l’animation par expressions. Pour cela, vous devez mapper la chaîne utilisée dans l’Expression vers l’objet réel. Cela permet au système de savoir ce qu’il doit inspecter pour calculer la valeur lors de l’évaluation de l’équation. Il existe différents types de paramètres en corrélation avec le type de l’objet que vous souhaitez inclure dans l’équation :  

|Type|  Fonction permettant de créer le paramètre|
|----|------------------------------|
|Scalar|    SetScalarParameter(String ref, Scalar obj)|
|Vector|    SetVector2Parameter(String ref, Vector2 obj)<br/>SetVector3Parameter(String ref, Vector3 obj)<br/>SetVector4Parameter(String ref, Vector4 obj)|
|Matrix|    SetMatrix3x2Parameter(String ref, Matrix3x2 obj)<br/>SetMatrix4x4Parameter(String ref, Matrix4x4 obj)|
|Quaternion|    SetQuaternionParameter(String ref, Quaternion obj)|
|Color| SetColorParameter(String ref, Color obj)|
|CompositionObject| SetReferenceParameter(String ref, Composition object obj)|
|Boolean| SetBooleanParameter(String ref, Boolean obj)|  

Dans l’exemple ci-dessous, nous créons une animation par expressions qui référencera la propriété Offset des deux autres éléments [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) de composition, ainsi qu’un objet Vector3 System.Numerics de base.  
```cs
var commonOffset = new Vector3(25.0, 17.0, 10.0);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + additionalOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetVector3Parameter("additionalOffset", commonOffset);
```

En outre, vous pouvez référencer une valeur dans un jeu de propriétés à partir d’une expression utilisant le modèle décrit ci-dessus. Les jeux de propriétés de composition sont un moyen utile pour stocker les données utilisées par les animations. Elles sont également utiles pour créer des données pouvant être partagées et réutilisées, et qui ne dépendent pas de la durée de vie d’un objet de composition tiers. Les valeurs des jeux de propriétés peuvent être référencées dans une expression semblable aux autres références de propriétés. (Les jeux de propriétés sont davantage décrits plus loin dans cet article)  

Nous pouvons modifier l’exemple ci-dessus de manière à ce qu’un jeu de propriétés soit utilisé pour définir la variable commonOffset au lieu d’une variable locale :
```cs
_sharedProperties = _compositor.CreatePropertySet();
_sharedProperties.InsertVector3("commonOffset", offset);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + sharedProperties.commonOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

Enfin, quand vous référencez les propriétés d’autres objets, il est également possible de faire référence aux propriétés du sous-canal dans la chaîne d’expression ou dans le paramètre de référence.  
 
Dans l’exemple ci-dessous, nous faisons référence au sous-canal x des propriétés Offset à partir de deux éléments [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) : le premier dans la chaîne d’expression et l’autre pendant la création de la référence de paramètre.
Notez qu’en faisant référence au composant X d’Offset, nous changeons le type du paramètre qui passe de Vector3 à Scalar, comme dans l’exemple précédent :  
```cs
var expression = _compositor.CreateExpressionAnimation("xOffset/ ParentOffset.X");
expression.SetScalarParameter("xOffset", childVisual.Offset.X);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
```

### <a name="expression-helper-functions-and-constructors"></a>Expression : Fonctions d’assistance et constructeurs
Outre l’accès à des opérateurs et des paramètres de propriété, vous pouvez exploiter une liste de fonctions mathématiques à utiliser dans ces expressions. Ces fonctions sont fournies pour effectuer des calculs et des opérations sur différents types, que vous ferez également sur des objets System.Numerics.  

Voici un exemple de création d’une Expression ciblant des paramètres Scalar, qui tire parti de la fonction d’assistance Clamp :  
```cs
var expression = _compositor.CreateExpressionAnimation("Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)"
```

Outre les fonctions d’assistance, vous pouvez utiliser les méthodes Constructor intégrées au sein d’une chaîne d’expression qui généreront une instance de ce type en fonction des paramètres fournis.  

Voici un exemple de création d’une Expression qui définit un nouveau paramètre Vector3 dans la chaîne d’expression :  
```cs
var expression = _compositor.CreateExpressionAnimation("Offset / Vector3(targetX, targetY, targetZ");
```

Vous trouverez la liste complète des fonctions d’assistance et des constructeurs dans l’annexe ; sinon, pour chaque type, consultez la liste ci-après :  
*   [Scalar](#scalar)
*   [Vector2](#vector2)
*   [Vector3](#vector3)
*   [Matrix3x2](#matrix3x2)
*   [Matrix4x4](#matrix4x4)
*   [Quaternion](#quaternion)
*   [Color](#color)  

### <a name="expression-keywords"></a>Mots-clés d’expression
Vous pouvez tirer parti des « mots-clés » spéciaux qui sont traités différemment lorsque la chaîne d’expression est évaluée. Étant donné leur nature de « mots-clés », ces derniers ne peuvent pas être utilisés en tant que paramètres de chaîne de leurs références de propriété.  
 
|Mot-clé|   Description|
|-------|--------------|
|This.StartingValue| Fournit une référence à la valeur de départ d’origine de la propriété animée.|
|This.CurrentValue| Fournit une référence à la valeur actuelle « connue » de la propriété|
|Pi| Fournit une référence de mot-clé à la valeur de PI|

Voici un exemple d’utilisation du mot-clé this.StartingValue :  
```cs
var expression = _compositor.CreateExpressionAnimation("this.StartingValue + delta");
```

### <a name="expressions-with-conditionals"></a>Expressions comportant des instructions conditionnelles
Outre la prise en charge des relations mathématiques à l’aide des opérateurs, des références de propriétés et des fonctions et constructeurs, vous pouvez également créer une expression contenant un opérateur ternaire :  
```
(condition ? ifTrue_expression : ifFalse_expression)
```

Des instructions conditionnelles permettent d’écrire des expressions basées sur une certaine condition ; différentes relations mathématiques seront utilisées par le système pour calculer la valeur de la propriété d’animation. Les opérateurs ternaires peuvent être imbriqués en tant qu’expressions pour les instructions true ou false.  

Les opérateurs conditionnels suivants sont pris en charge dans l’instruction de condition : 
*   Equals (==)
*   Not Equals (!=)
*   Less than (&lt;)
*   Less than or equal to (&lt;=)
*   Great than (&gt;)
*   Great than or equal to (&gt;=)  

Les conjonctions suivantes sont prises en charge en tant qu’opérateurs ou fonctions dans l’instruction de condition :
*   Not: ! / Not(bool1)
*   And: &amp;&amp; / And(bool1, bool2)
*   Or: || / Or(bool1, bool2)  

Voici un exemple d’animation par expressions utilisant une instruction conditionnelle.  
```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f + (target.Offset.x / parent.Offset.x) : 1.0f");
```

## <a name="expression-keyframes"></a>Images clés d’expression
Précédemment dans ce document, nous avons décrit comment créer des animations par images clés et vous avons présenté les animations par expressions ainsi que les composants nécessaires à la création d’une chaîne d’expression. Comment tirer parti de la puissance des animations par expressions, tout en exploitant l’interpolation du temps que procurent les animations par images clés ? La réponse réside dans les images clés d’expression !  

Au lieu de définir une valeur discrète pour chaque point de contrôle dans l’animation par images clés, cette valeur peut prendre la forme d’une chaîne d’expression. Dans ce cas, le système utilisera la chaîne d’expression pour calculer la valeur de la propriété d’animation à un moment donné dans la chronologie. Le système sera simplement interpolé vers cette valeur, comme dans une animation par images clés normale.    

Vous n’avez pas besoin de créer des animations spéciales pour utiliser les images clés d’expression ; insérez simplement une ExpressionKeyFrame dans votre animation par images clés standard, puis renseignez le temps et la chaîne d’expression en tant que valeur. Voici un exemple illustratif, utilisant une chaîne d’expression en tant que valeur pour l’une des images clés :   
```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

## <a name="expression-sample"></a>Exemple d’expression
Le code ci-dessous montre un exemple de configuration d’une animation par expressions pour une expérience de parallaxe de base, qui collecte des valeurs d’entrée à partir de la visionneuse à défilement.
```cs
// Get scrollviewer
ScrollViewer myScrollViewer = ThumbnailList.GetFirstDescendantOfType<ScrollViewer>();
_scrollProperties = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);

// Setup the expression
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 *  ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5   * ItemHeight))";
```

## <a name="animating-with-property-sets"></a>Animation avec des jeux de propriétés
Les jeux de propriétés de composition vous permettent de stocker des valeurs pouvant être partagées entre plusieurs animations et qui ne sont pas liées à la durée de vie d’un objet de composition tiers. Les jeux de propriétés sont extrêmement utiles pour stocker des valeurs courantes, puis les référencer ultérieurement (et facilement) dans les animations. Vous pouvez également utiliser les jeux de propriétés pour stocker des données basées sur une logique d’application pour l’exécution d’une expression.  

Pour créer un jeu de propriété, utilisez la méthode Constructor de l’objet Compositor :  
```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Une fois que vous avez créé votre jeu de propriétés, vous pouvez ajouter une propriété et une valeur à ce dernier :  
```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

À l’instar de ce que nous avons pu voir précédemment, nous pouvons faire référence à cette valeur de jeu de propriété dans une animation par expressions :  
```cs
var expression = _compositor.CreateExpressionAnimation("this.target.Offset + sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
targetVisual.StartAnimation("Offset", expression);
```

Les valeurs des jeux de propriétés peuvent également être animées. Pour ce faire, attachez l’animation à un objet PropertySet, puis référencez le nom de la propriété dans la chaîne. Ci-dessous, nous animons la propriété NewOffset dans le jeu de propriété à l’aide d’une animation par images clés.  
```cs
var keyFrameAnimation = _compositor.CreateVector3KeyFrameAnimation()
keyFrameAnimation.InsertKeyFrame(0.5f, new Vector3(25.0f, 68.0f, 0.0f);
keyFrameAnimation.InsertKeyFrame(1.0f, new Vector3(89.0f, 125.0f, 0.0f);
_sharedProperties.StartAnimation("NewOffset", keyFrameAnimation);
```


Vous vous demandez peut-être ce qui se passerait pour la valeur de la propriété animée à laquelle est attachée l’animation par expressions si ce code était exécuté dans une application. Dans ce cas, l’expression sort d’abord en tant que valeur. Cependant, dès que l’animation par images clés anime la propriété du jeu de propriétés, la valeur de l’expression sera également mise à jour, étant donné que l’équation est calculée à chaque image. C’est toute la magie des jeux de propriété avec des animations par images clés et par expressions !  
 
## <a name="manipulationpropertyset"></a>ManipulationPropertySet
Outre l’utilisation de jeux de propriétés de composition, un développeur peut également accéder à des ManipulationPropertySet, qui permettent d’accéder à des propriétés sur XAML ScrollViewer. Ces propriétés peuvent ensuite être utilisées, puis référencées dans une animation par expressions afin de propulser des expériences telles que l’effet parallaxe et les en-têtes rémanents. Remarque : vous pouvez saisir le ScrollViewer d’un contrôle XAML quelconque (ListView, GridView, etc.) comportant un contenu avec défilement, puis utiliser ce ScrollViewer afin d’obtenir ManipulationPropertySet pour les contrôles de défilement en question.  

Dans votre Expression, vous pouvez référencer les propriétés suivantes de la visionneuse à défilement :  

|Propriété| Type|  
|--------|-----|  
|Translation| Vector3|  
|Pan| Vector3|  
|Scale| Vector3|  
|CenterPoint| Vector3|  
|Matrix| Matrix4x4|  

Pour obtenir une référence à ManipulationPropertySet, utilisez la méthode GetScrollViewerManipulationPropertySet de ElementCompositionPreview.  
```csharp
CompositionPropertySet manipPropSet = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScroller);
```

Une fois que vous avez une référence à ce jeu de propriétés, vous pouvez référencer les propriétés de la visionneuse à défilement qui se trouvent dans le jeu de propriétés. Commencez par créer le paramètre de référence.  
```csharp
ExpressionAnimation exp = compositor.CreateExpressionAnimation();
exp.SetReferenceParameter("ScrollManipulation", manipPropSet);
```

Après avoir configuré le paramètre de référence, vous pouvez faire référence aux propriétés ManipulationPropertySet dans l’expression.  
```csharp
exp.Expression = “ScrollManipulation.Translation.Y / ScrollBounds”;
_target.StartAnimation(“Opacity”, exp);
```

## <a name="using-implicit-animations"></a>Utilisation d’animations implicites  
Les animations vous permettent de décrire de manière efficace un comportement à vos utilisateurs. Vous pouvez animer votre contenu de plusieurs façons, mais toutes les méthodes décrites jusqu’ici vous contraignent à *Démarrer* votre animation de manière explicite. Même si cela vous laisse tout le loisir de lancer l’animation quand vous le voulez, il devient plus difficile de déterminer à quel moment une animation est nécessaire chaque fois qu’une valeur de propriété est modifiée. Cela se produit assez souvent quand une application a séparé sa « personnalité » qui définit les animations de sa « logique » qui définit ses composants et son infrastructure de base. Les animations implicites offrent un moyen plus simple et plus transparent de définir l’animation indépendamment de la logique de base de l’application. Vous pouvez faire en sorte que ces animations s’exécutent avec des déclencheurs de modifications de propriétés spécifiques.

### <a name="setting-up-your-implicitanimationcollection"></a>Configuration de ImplicitAnimationCollection  
Les animations implicites sont définies par d’autres objets **CompositionAnimation** (**KeyFrameAnimation** ou **ExpressionAnimation**). **ImplicitAnimationCollection** représente l’ensemble d’objets **CompositionAnimation** qui sont lancés quand le *déclencheur* de modification de propriété s’actionne. Remarque : au moment de définir des animations, veillez à définir la propriété **Target**. Celle-ci définit la propriété [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) qui est ciblée par l’animation au moment où elle démarre. La propriété **Target** ne peut être qu’une propriété [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) animable.
Dans l’extrait de code ci-dessous, un seul élément **Vector3KeyFrameAnimation** est créé et défini comme partie intégrante d’**ImplicitAnimationCollection**. **ImplicitAnimationCollection** est ensuite attaché à la propriété **ImplicitAnimation** de l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx), si bien que lorsque le déclencheur entre en action, l’animation démarre.  
```csharp
Vector3KeyFrameAnimation animation = _compositor.CreateVector3KeyFrameAnimation();
animation.DelayTime =  TimeSpan.FromMilliseconds(index);
animation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");
animation.Target = "Offset";
ImplicitAnimationCollection implicitAnimationCollection = compositor.CreateImplicitAnimationCollection();

visual.ImplicitAnimations = implicitAnimationCollection;
```


### <a name="triggering-when-the-implicitanimation-starts"></a>Déclenchement lors du démarrage d’une animation implicite  
Le terme déclencheur décrit l’instant où les animations démarrent de manière implicite. Pour l’heure, les déclencheurs sont définis comme étant des modifications apportées aux propriétés animables d’un élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) (ces modifications se produisent par le biais d’un jeu explicite au niveau de la propriété). Par exemple, en plaçant un déclencheur **Offset** sur un élément **ImplicitAnimationCollection** et en y associant une animation, les mises à jour apportées à la propriété **Offset** de l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) ciblé ont pour effet de l’animer avec sa nouvelle valeur en utilisant l’animation de la collection.  
En partant de l’exemple ci-dessus, nous ajoutons cette ligne supplémentaires pour définir le déclencheur sur la propriété **Offset** de la cible [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx).  
```csharp
implicitAnimationCollection["Offset"] = animation;
```  
Notez qu’un élément **ImplicitAnimationCollection** peut avoir plusieurs déclencheurs. Cela signifie que l’animation implicite ou le groupe d’animations peut être démarré à la suite de modifications apportées à des propriétés différentes. Dans l’exemple ci-dessus, le développeur peut éventuellement ajouter un déclencheur pour d’autres propriétés, par exemple Opacity.  
###<a name="thisfinalvalue"></a>this.FinalValue     
Dans le premier exemple implicite, nous avons utilisé un ExpressionKeyFrame pour le KeyFrame « 1.0 » et lui avons affecté l’expression **this.FinalValue**. **this.FinalValue** est un mot clé réservé dans le langage des expressions qui fournit un comportement de différenciation pour les animations implicites. **this.FinalValue** lie l’ensemble de valeurs de la propriété API à l’animation. Cela facilite la création de vrais modèles. **this.FinalValue** n’est pas utile dans le cas des animations explicites dans la mesure où la définition de la propriété API est instantanée, contrairement aux animations implicites, où elle est différée.  
 
## <a name="using-animation-groups"></a>Utilisation de groupes d’animations  
**CompositionAnimationGroup** offre aux développeurs un moyen simple de regrouper une liste d’animations pouvant être utilisée avec des animations implicites ou explicites.   
### <a name="creating-and-populating-animation-groups"></a>Création et remplissage de groupes d’animations  
La méthode **CreateAnimationGroup** de l’objet Compositor permet aux développeurs de créer un groupe d’animations :  
```sharp
CompositionAnimationGroup animationGroup = _compositor.CreateAnimationGroup();
animationGroup.Add(animationA);
animationGroup.Add(animationB);
```   
Une fois que le groupe d’animations est créé, il est possible d’y ajouter des animations individuelles. Pour rappel, vous n’avez pas besoin de démarrer de façon explicite les animations individuelles : elles démarrent dès lors que **StartAnimationGroup** est appelé pour le scénario explicite ou que le déclencheur s’actionne pour le scénario implicite.  
Remarque : vérifiez que la propriété **Target** est définie pour les animations qui sont ajoutées au groupe. Elle définit la propriété de l’élément [Visual](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.visual.aspx) cible qui sera animée.

### <a name="using-animation-groups-with-implicit-animations"></a>Utilisation de groupes d’animations avec des animations implicites  
Les développeurs peuvent créer des animations implicites de sorte que lorsqu’un déclencheur s’actionne, un ensemble d’animations démarre sous la forme d’un groupe d’animations. Dans ce cas, définissez le groupe d’animation comme l’ensemble d’animations qui démarre quand le déclencheur s’actionne.  
```csharp
implicitAnimationCollection["Offset"] = animationGroup;
```   
### <a name="using-animation-groups-with-explicit-animations"></a>Utilisation de groupes d’animations avec des animations explicites  
Les développeurs peuvent créer des animations explicites de telle sorte que les animations individuelles ajoutées démarrent quand **StartAnimationGroup** est appelé. Notez que dans cet appel **StartAnimation**, aucune propriété n’est ciblée pour le groupe, car les animations individuelles peuvent cibler des propriétés différentes. Vérifiez que la propriété cible de chaque animation est définie.  
```csharp
visual.StartAnimationGourp(AnimationGroup);
```  

### <a name="e2e-sample"></a>Exemple E2E 
Cet exemple illustre l’animation implicite de la propriété Offset quand une nouvelle valeur est définie.  
```csharp 
class PropertyAnimation
{
    PropertyAnimation(Compositor compositor, SpriteVisual heroVisual, SpriteVisual listVisual)
    {
        // Define ImplicitAnimationCollection
        ImplicitAnimationCollection implicitAnimations = 
        compositor.CreateImplicitAnimationCollection();

        // Trigger animation when the “Offset” property changes.
        implicitAnimations["Offset"] = CreateAnimation(compositor);

        // Assign ImplicitAnimations to a visual. Unlike Visual.Children,    
        // ImplicitAnimations can be shared by multiple visuals so that they 
        // share the same implicit animation behavior (same as Visual.Clip).
        heroVisual.ImplicitAnimations = implicitAnimations;

        // ImplicitAnimations can be shared among visuals 
        listVisual.ImplicitAnimations = implicitAnimations;

        listVisual.Offset = new Vector3(20f, 20f, 20f);
    }

    Vector3KeyFrameAnimation CreateAnimation(Compositor compositor)
    {
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        animation.InsertExpressionKeyFrame(0f, "this.StartingValue");
        animation.InsertExpressionKeyFrame(1f, "this.FinalValue");
        animation.Target = “Offset”;
        animation.Duration = TimeSpan.FromSeconds(0.25);
        return animation;
    }
}
```   

 
 
## <a name="appendix"></a>Annexe
### <a name="expression-functions-by-structure-type"></a>Fonctions d’expression par type de structure
### <a name="scalar"></a>Scalar  

|Fonction et opérations de constructeur| Description|  
|-----------------------------------|--------------|  
|Abs(valeur Float)| Retourne une valeur Float représentant la valeur absolue du paramètre Float|  
|Clamp (valeur Float, Float min, Float max)| Retourne une valeur Float qui est supérieure à la valeur minimale et inférieure à la valeur maximale ou minimale si la valeur est inférieure à min ou max si la valeur est supérieure à la valeur maximale|  
|Max (valeur1 de Float, valeur2 de Float)| Retourne la valeur Float supérieure entre la valeur1 et la valeur2.|  
|Min (valeur1 de Float, valeur2 de Float)| Retourne la valeur Float inférieure entre la valeur1 et la valeur2.|  
|Lerp(valeur1 de Float, valeur2 de Float, progression de Float)| Retourne une valeur Float qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Scalar en fonction de la progression (Remarque : la progression se situe entre 0.0 et 1.0)|  
|Slerp(valeur1 de Float, valeur2 de Float, progression de Float)| Retourne une valeur Float qui représente l’interpolation sphérique calculée entre les deux valeurs Float en fonction de la progression (Remarque : la progression se situe entre 0.0 et 1.0)|  
|Mod(valeur1 de Float, valeur2 de Float)| Retourne le reste de la valeur Float résultant de la division des valeurs1 et 2|  
|Ceil(valeur Float)| Retourne le paramètre Float arrondi au nombre entier supérieur suivant|  
|Floor(valeur Float)| Retourne le paramètre Float arrondi au nombre entier inférieur suivant|  
|Sqrt(valeur Float)| Retourne la racine carrée du paramètre Float|  
|Square(valeur Float)| Retourne le carré du paramètre Float|  
|Sin(valeur1 de Float)| Retourne le Sin du paramètre Float|
|Asin(valeur2 de Float)| Retourne l’ArcSin du paramètre Float|
|Cos(valeur1 de Float)| Retourne le Cos du paramètre Float|
|ACos(valeur2 de Float)| Retourne l’ArcCos du paramètre Float|
|Tan(valeur1 de Float)| Retourne la Tan du paramètre Float|
|ATan(valeur2 de Float)| Retourne l’ArcTan du paramètre Float|
|Round(valeur Float)| Retourne le paramètre Float arrondi au nombre entier le plus proche|
|Log10(valeur Float)| Retourne le résultat Log (base 10) du paramètre Float|
|Ln(valeur Float)| Retourne le résultat Natural Log du paramètre Float|
|Pow (valeur Float, puissance Float)| Retourne le résultat du paramètre Float élevé à une puissance spécifique|
|ToDegrees(valeurs Float en radians)| Retourne le paramètre Float converti en degrés|
|ToRadians (degrés Float)| Retourne le paramètre Float converti en radians|

### <a name="vector2"></a>Vector2  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Abs (valeur Vector2)|   Retourne une valeur Vector2 avec une valeur absolue appliquée à chacun des composants|
|Clamp (valeur1 de Vector2, Vector2 min, Vector2 max)|  Retourne un Vector2 contenant les valeurs limitées pour chaque composant respectif|
|Max (valeur1 de Vector2 , valeur2 de Vector2)|  Retourne une valeur Vector2 ayant atteint une valeur Max sur chacun des composants correspondants de valeur1 et valeur2|
|Min (valeur1 de Vector2 , valeur2 de Vector2)|  Retourne une valeur Vector2 ayant atteint une valeur Min sur chacun des composants correspondants de valeur1 et valeur2|
|Scale(valeur Vector2, facteur Float)|    Retourne un Vector2 avec chaque composant du vecteur multiplié par le facteur d’échelle.|
|Transform(valeur Vector2, matrice Matrix3x2)|    Retourne une valeur Vector2 résultant de la transformation linéaire entre Vector2 et Matrix3x2 (c’est-à-dire la multiplication d’un vecteur par une matrice).|
|Lerp(valeur1 de Vector2, valeur2 de Vector2, progression de Float)|  Retourne une valeur Vector2 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Vector2 en fonction de la progression (Remarque : la progression se situe entre 0.0 et 1.0)|
|Length(valeur Vector2)| Retourne une valeur Float qui représente la longueur/la magnitude de Vector2|
|LengthSquared(Vector2)|    Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’une valeur Vector2|
|Distance(valeur1 de Vector2 , valeur2 de Vector2)|  Retourne une valeur Float qui représente la distance entre deux valeurs Vector2|
|DistanceSquared(valeur1 de Vector2 , valeur2 de Vector2)|   Retourne une valeur Float qui représente le carré de la distance entre deux valeurs Vector2|
|Normalize(valeur Vector2)|  Retourne une valeur Vector2 représentant le vecteur unitaire du paramètre dans lequel tous les composants ont été normalisés|
|Vector2(Float x et Float y)| Crée un Vector2 à l’aide de deux paramètres Float|

### <a name="vector3"></a>Vector3  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Abs (valeur Vector3)|   Retourne une valeur Vector3 avec une valeur absolue appliquée à chacun des composants|
|Clamp (valeur1 de Vector3, Vector3 min, Vector3 max)|  Retourne une valeur Vector3 contenant les valeurs limitées pour chaque composant respectif|
|Max (valeur1 de Vector3, valeur2 de Vector3)|  Retourne une valeur Vector3 ayant atteint une valeur Max sur chacun des composants correspondants de valeur1 et valeur2|
|Min (valeur1 de Vector3, valeur2 de Vector3)|  Retourne une valeur Vector3 ayant atteint une valeur Min sur chacun des composants correspondants de valeur1 et valeur2|
|Scale(valeur Vector3, facteur Float)|    Retourne un Vector3 avec chaque composant du vecteur multiplié par le facteur d’échelle.|
|Lerp(valeur1 de Vector3, valeur2 de Vector3, progression de Float)|  Retourne une valeur Vector3 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Vector3 en fonction de la progression (Remarque : la progression se situe entre 0.0 et 1.0)|
|Length(valeur de Vector3)| Retourne une valeur Float qui représente la longueur/la magnitude de Vector3|
|LengthSquared(Vector3)|    Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’une valeur Vector3|
|Distance(valeur1 de Vector3, valeur2 de Vector3)|  Retourne une valeur Float qui représente la distance entre deux valeurs Vector3|
|DistanceSquared(valeur1 de Vector3, valeur2 de Vector3)|   Retourne une valeur Float qui représente le carré de la distance entre deux valeurs Vector3|
|Normalize(valeur Vector3)|  Retourne une valeur Vector3 représentant le vecteur unitaire du paramètre dans lequel tous les composants ont été normalisés|
|Vector3(Float x, Float y, Float z)|    Crée un Vector3 à l’aide de trois paramètres Float|

### <a name="vector4"></a>Vector4  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Abs (valeur Vector4)|   Retourne une valeur Vector3 avec une valeur absolue appliquée à chacun des composants|
|Clamp (valeur1 de Vector4, Vector4 min, Vector4 max)|  Retourne une valeur Vector4 contenant les valeurs limitées pour chaque composant respectif|
|Max (valeur1 de Vector4, valeur2 de Vector4)|   Retourne une valeur Vector4 ayant atteint une valeur Max sur chacun des composants correspondants de valeur1 et valeur2|
|Min (valeur1 de Vector4, valeur2 de Vector4)|   Retourne une valeur Vector4 ayant atteint une valeur Min sur chacun des composants correspondants de valeur1 et valeur2|
|Scale(valeur Vector3, facteur Float)|    Retourne un Vector3 avec chaque composant du vecteur multiplié par le facteur d’échelle.|
|Transform(valeur Vector4, matrice Matrix4x4)|    Retourne une valeur Vector4 résultant de la transformation linéaire entre Vector4 et Matrix4x4 (c’est-à-dire la multiplication d’un vecteur par une matrice).|
|Lerp(valeur1 de Vector4, valeur2 de Vector4, progression de Float)|  Retourne une valeur Vector4 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Vector4 en fonction de la progression (Remarque : la progression se situe entre 0.0 et 1.0)|
|Length(valeur Vector4)| Retourne une valeur Float qui représente la longueur/la magnitude de Vector4|
|LengthSquared(Vector4)|    Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’une valeur Vector4|
|Distance(valeur1 de Vector4, valeur2 de Vector4)|  Retourne une valeur Float qui représente la distance entre deux valeurs Vector4|
|DistanceSquared(valeur1 de Vector4, valeur2 de Vector4)|   Retourne une valeur Float qui représente le carré de la distance entre deux valeurs Vector4|
|Normalize(valeur de Vector4)|  Retourne une valeur Vector4 représentant le vecteur unitaire du paramètre dans lequel tous les composants ont été normalisés|
|Vector4(Float x, Float y, Float w)|   Crée un Vector4 à l’aide de quatre paramètres Float|

### <a name="matrix3x2"></a>Matrix3x2  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Scale(valeur Matrix3x2, facteur Float)|  Retourne une Matrix3x2 avec chaque composant de la matrice multiplié par le facteur d’échelle.|
|Inverse(valeur de Matrix3x2)| Renvoie un objet Matrix3x2 représentant la matrice réciproque.|
|Matrix3x2(Float M11, Float M12, Float M21, Float M22, Float M31, Float M32)|   Crée une matrice Matrix3x2 à l’aide de six paramètres Float|
|Matrix3x2.CreateFromScale(échelle Vector2)|  Crée une matrice Matrix3x2 à partir d’un Vector2 représentant une échelle<br/>\[scale.X, 0.0<br/> 0.0, scale.Y<br/> 0.0, 0.0 \]|
|Matrix3x2.CreateFromTranslation(translation de Vector2)|  Crée une matrice Matrix3x2 à partir d’un Vector2 représentant une translation<br/>\[1.0, 0.0,<br/> 0.0, 1.0,<br/> translation.X, translation.Y\]|  
|Matrix3x2.CreateSkew (Float x, Float y, point central Vector2)| Crée une matrice Matrix3x2 à partir de deux valeurs Float et d’une valeur Vector2 représentant une inclinaison<br/>\[1.0, Tan(y),<br/>Tan(x), 1.0,<br/>-centerpoint.Y * Tan(x), -centerpoint.X * Tan(y)\]|  
|Matrix3x2.CreateRotation (radians Float)| Crée une matrice Matrix3x2 à partir d’une rotation en radians<br/>\[COS(radians), Sin(radians),<br/>-Sin(radians), Cos(radians),<br/>0.0, 0.0 \]|   
|Matrix3x2.CreateTranslation(translation de Vector2)| Identique à CreateFromTranslation|      
|Matrix3x2.CreateScale(échelle Vector2)| Identique à CreateFromScale|    

    
### <a name="matrix4x4"></a>Matrix4x4  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Scale(valeur Matrix4x4, facteur Float)|  Retourne une matrice Matrix4x4 avec chaque composant de la matrice multiplié par le facteur d’échelle.|
|Inverse(Matrix4x4)|    Renvoie un objet Matrix4x4 représentant la matrice réciproque.|
|Matrix4x4(Float M11, Float M12, Float M13, Float M14,<br/>Float M21, Float M22, Float M23, Float M24,<br/>    Float M31, Float M32, Float M33, Float M34,<br/>    Float M41, Float M42, Float M43, Float M44)| Crée une matrice Matrix4x4 à l’aide de 16 paramètres Float|
|Matrix4x4.CreateFromScale(échelle Vector3)|  Crée une matrice Matrix4x4 à partir d’un Vector3 représentant une échelle<br/>\[scale.X, 0.0, 0.0, 0.0,<br/> 0.0, scale.Y, 0.0, 0.0,<br/> 0.0, 0.0, scale.Z, 0.0,<br/> 0.0, 0.0, 0.0, 1.0\]|
|Matrix4x4.CreateFromTranslation(translation de Vector3)|  Crée une matrice Matrix4x4 à partir d’un Vector3 représentant une translation<br/>\[1.0, 0.0, 0.0, 0.0,<br/> 0.0, 1.0, 0.0, 0.0,<br/> 0.0, 0.0, 1.0, 0.0,<br/> translation.X, translation.Y, translation.Z, 1.0\]|
|Matrix4x4.CreateFromAxisAngle(axe Vector3, angle Float)|  Crée une matrice Matrix4x4 à partir d’un axe Vector3 et d’une valeur Float représentant un angle|
|Matrix4x4(matrice Matrix3x2)| Crée une matrice Matrix4x4 à partir d’une matrice Matrix3x2<br/>\[matrix.11, matrix.12, 0, 0,<br/>matrix.21, matrix.22, 0, 0,<br/>0, 0, 1, 0,<br/>matrix.31, matrix.32, 0, 1\]|  
|Matrix4x4.CreateTranslation(translation de Vector3)| Identique à CreateFromTranslation|  
|Matrix4x4.CreateScale(échelle Vector3)| Identique à CreateFromScale|  


### <a name="quaternion"></a>Quaternion  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Slerp(valeur1 de Quaternion, valeur2 de Quaternion, progression de Float)|   Retourne une valeur Quaternion qui représente l’interpolation sphérique calculée entre les deux valeurs Quaternion en fonction de la progression (Remarque : la progression se situe entre 0.0 et 1.0)|
|Concatenate(valeur1 de Quaternion, valeur2 de Quaternion)|  Retourne une valeur Quaternion qui représente la concaténation de deux Quaternions (c’est-à-dire, un Quaternion qui représente deux rotations individuelles combinées)|
|Length(valeur Quaternion)|  Retourne une valeur Float qui représente la longueur/la magnitude du Quaternion.|
|LengthSquared(Quaternion)| Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’un Quaternion|
|Normalize(valeur Quaternion)|   Retourne un Quaternion dont les composants ont été normalisés|
|Quaternion.CreateFromAxisAngle(axe Vector3, angle Scalar)|    Crée un Quaternion à partir d’un axe Vector3 et d’une valeur Scalar représentant un angle|
|Quaternion(Float x, Float y, Float z, Float w)|    Crée un Quaternion à partir de quatre valeurs Float|

### <a name="color"></a>Color

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|ColorLerp(colorTo de Color, colorFrom de Color, progression de Float)| Retourne un objet Color représentant la valeur calculée d’une interpolation linéaire entre deux objets Color, en fonction d’une progression donnée. (Remarque : la progression se situe entre 0.0 et 1.0)|
|ColorLerpRGB(colorTo de Color, colorFrom de Color, progression de Float)|  Retourne un objet Color représentant la valeur calculée d’une interpolation linéaire entre deux objets, en fonction d’une progression donnée dans l’espace de couleur RVB.|
|ColorLerpHSL(colorTo de Color, colorFrom de Color, progression de Float)|  Retourne un objet Color représentant la valeur calculée d’une interpolation linéaire entre deux objets, en fonction d’une progression donnée dans l’espace de couleur HSL.|
|ColorArgb(Float a, Float r, Float g, Float b)| Crée un objet représentant Color, défini par des composants ARGB|
|ColorHsl(Float h, Float s, Float l)|   Crée un objet représentant Color, défini par les composants HSL (Remarque : la teinte Hue est définie entre 0 et 2pi)|







<!--HONumber=Dec16_HO1-->


