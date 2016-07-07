---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animations de composition
description: "De nombreuses propriétés d’objet et d’effet de composition peuvent être animées à l’aide d’animations par images clés et expressions, ce qui permet aux propriétés d’un élément d’interface utilisateur de changer dans le temps ou en fonction d’un calcul."
ms.sourcegitcommit: 62f0ea80940ff862d26feaa063414d95b048f685
ms.openlocfilehash: e0088692b9de10c188f15b85b1f20b98cc113517

---
# Animations de composition

\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

L’API WinRT Windows.UI.Composition vous permet de créer, animer, transformer et manipuler les objets compositeur dans une couche API unifiée. Les animations de composition offrent un moyen puissant et efficace d’exécuter des animations dans l’interface utilisateur de votre application. Elles ont été entièrement conçues pour garantir l’exécution de vos animations à 60FPS, indépendamment du thread d’interface utilisateur, et pour vous offrir la possibilité de créer des expériences totalement inédites en usant de nombreuses propriétés et entrées pour produire les animations en question.
Cette rubrique fournit une vue d’ensemble des fonctionnalités disponibles vous permettant d’animer des propriétés de l’objet de composition.
Ce document suppose que vous êtes familiarisé avec les principes de base de la structure de couche visuelle. Pour plus d’informations, [cliquez ici](./composition-visual-tree.md). Il existe deux types d’animations de composition: les **animations par images clés**, et les **animations par expressions**  

![](./images/composition-animation-types.png)  
   
 
##Types d’animations de composition
Les **animations par images clés** vous fournissent des expériences d’animation *image par image* et ancrées dans le temps. Les développeurs peuvent définir des *points de contrôle* de manière explicite, décrivant les valeurs que doit avoir une propriété d’animation à des points spécifiques dans la chronologie d’animation. Plus important encore, vous pouvez utiliser des fonctions d’accélération (aussi appelées «interpolateurs») pour décrire comment effectuer la transition entre ces points de contrôle.  

Les **animations par expressions** constituent un nouveau type d’animation, introduit dans la couche visuelle lors de la mise à jour Windows10 de novembre (Build10586). L’idée derrière les animations par expressions est qu’un développeur puisse créer des relations mathématiques entre les propriétés visuelles et les valeurs discrètes, qui seront évaluées et mises à jour à chaque image. Les développeurs peuvent référencer des propriétés sur les objets de composition ou les jeux de propriétés, utiliser des assistants de fonction mathématique, voire référencer des entrées pour dériver ces relations mathématiques. Les expressions rendent possibles et fluides des expériences telles que les effets parallaxes et les en-têtes rémanents sur la plateforme Windows.  

##Pourquoi utiliser des animations de composition?
**Performances**  
 Lors de la création d’applications universelles Windows, la plupart du code de l’application s’exécute sur le thread d’interface utilisateur. Par conséquent, pour vous assurer que les animations s’exécutent sans problème sur l’ensemble des catégories d’appareils, le système effectue les calculs et le travail d’animation sur un thread indépendant afin de conserver les 60FPS. Les développeurs sont ainsi assurés d’avoir des animations fluides, tandis que leurs applications effectuent d’autres opérations complexes pour des expériences utilisateur avancées.    
 
**Possibilités**  
Les animations de composition de la couche visuelle ont pour objectif d’améliorer l’esthétique de l’interface utilisateur. Nous voulons fournir aux développeurs la flexibilité et les différents types d’animations nécessaires à la mise en œuvre de leurs idées incroyables et repousser davantage les limites de l’UWP
 
 (Vous pouvez également consulter [GitHub Composition](http://go.microsoft.com/fwlink/?LinkID=789439) pour obtenir des exemples sur la façon d’utiliser les API ainsi que des exemples de fidélité supérieure des API en action)  

**Création de modèles**  
 Toutes les animations de composition de la couche visuelle sont des modèles; cela signifie que les développeurs peuvent utiliser une animation sur plusieurs objets sans avoir à créer des animations distinctes. Cela permet aux développeurs d’utiliser la même animation et de modifier ses propriétés ou ses paramètres pour répondre à d’autres besoins sans perturber les animations précédentes.  
 
##Que peut-on animer à l’aide des animations de composition?
Vous pouvez appliquer les animations de composition à la plupart des propriétés des objets de composition, telles que les éléments visuels et InsetClip. Vous pouvez également appliquer des animations de composition aux effets de composition et aux jeux de propriétés. **Lorsque vous choisissez les éléments à animer, notez leur type; cela permet de déterminer le type d’animation par images clés à créer ou en quel type votre expression doit se résoudre.**  
 
###Éléments visuels
|Propriétés des éléments visuels animables|  Type|
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
*Si vous souhaitez animer la propriété TransformMatrix entière en tant que Matrix4x4, vous devrez utiliser l’animation par expressions. Dans le cas contraire, vous pouvez cibler des cellules individuelles de la matrice et utiliser des animations par images clés ou par expressions.  

###InsetClip
|Propriétés InsetClip animables|   Type|
|-------------------------------|-------|
|BottomInset|   Scalar|
|LeftInset| Scalar|
|RightInset|    Scalar|
|TopInset|  Scalar|

##Propriétés du sous-canal de l’élément visuel
Outre la possibilité d’animer les propriétés de l’élément visuel, vous pouvez également cibler les composants du *sous-canal* de ces propriétés d’animation. Par exemple, supposons que vous voulez simplement animer le décalage (Offset) X d’un élément visuel plutôt que le décalage entier. L’animation peut cibler la propriété de décalage Vector3 ou le composant Scalar X de la propriété de décalage. Outre la possibilité de cibler un composant de sous-canal individuel d’une propriété, plusieurs composants peuvent être ciblés. Vous pouvez, par exemple, cibler les composants X et Y de Scale

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

*L’animation d’un sous-canal Color de la propriété Bush est légèrement différente. Vous attachez StartAnimation() à Visual.Brush et déclarez la propriété «Color» à animer dans le paramètre. (L’animation des couleurs est détaillée plus loin dans cet article)

##Jeux de propriétés et effets
Outre l’animation des propriétés des éléments visuels de composition et d’InsetClip, vous pouvez animer des propriétés dans un PropertySet ou un effet. Pour les jeux de propriétés, vous définissez une propriété et la conservez dans un jeu de propriété de composition CompositionPropertySet; cette propriété peut ensuite être la cible d’une animation (et peut être référencée dans une autre animation en même temps). Ce processus sera décrit plus en détail dans les sections suivantes.  

Quant aux effets, vous pouvez définir les effets graphiques à l’aide des API d’effets de composition (pour la vue d’ensemble des effets, cliquez [ici](./composition-effects.md). Outre la définition des effets, vous pouvez animer les valeurs de propriété de l’effet. Pour ce faire, le composant de propriété de la propriété Brush doit être ciblé sur SpriteVisuals.

##Formule rapide: prise en main des animations de composition
Avant de vous plonger dans les détails sur la façon de construire et utiliser les différents types d’animations, voici une formule rapide et générale de création d’animations de composition.  
1.  Décidez quelle propriété, propriété de sous-canal ou effet vous souhaitez animer et notez le type sélectionné.  
2.  Créez un objet pour votre animation (il s’agira d’une animation par images clés ou d’une animation par expressions).  
    *  Pour les animations par images clés, assurez-vous de créer un type d’animation par images clés correspondant au type de propriété que vous souhaitez animer.  
    *  Il existe un seul type d’animation par expressions.  
3.  Définissez le contenu pour l’animation; insérez vos images clés ou définissez la chaîne d’expression  
    *  Pour les animations par images clés, assurez-vous que la valeur de vos images clés sont du même type que la propriété que vous souhaitez animer.  
    *  Pour les animations par expressions, assurez-vous que la chaîne d’expression se résout vers le même type que la propriété que vous souhaitez animer.  
4.  Lancez l’animation sur l’élément visuel dont vous souhaitez animer la propriété; appelez StartAnimation et fournissez les paramètres suivants: le nom de la propriété que vous souhaitez animer (sous forme de chaîne) et l’objet pour votre animation.  

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

##Utilisation des animations par images clés
Les animations par images clés sont des animations ancrées dans le temps, utilisant une ou plusieurs images clés pour spécifier la manière dont la valeur animée doit évoluer au fil du temps. Les images représentent des marqueurs ou des points de contrôle vous permettant de définir la valeur animée à un moment précis.  
 
###Création de votre animation et définition des images clés
Pour construire une animation par images clés, utilisez la méthode Constructor de l’objet Compositor qui est mis en corrélation avec le type de la propriété que vous souhaitez animer. Les différents types d’animation par images clés sont:
*   ColorKeyFrameAnimation
*   QuaternionKeyFrameAnimation
*   ScalarKeyFrameAnimation
*   Vector2KeyFrameAnimation
*   Vector3KeyFrameAnimation
*   Vector4KeyFrameAnimation  

Exemple de création d’une animation Vector3KeyFrameAnimation:     
```cs
var animation = _compositor.CreateVector3KeyFrameAnimation(); 
```

Chaque animation par images clés est créée par l’insertion de segments d’images clés individuels qui définissent deux composants (avec un troisième composant facultatif)  
*   Time: normalized progress state of the KeyFrame between 0.0 – 1.0
*   Value: specific value of the animating value at the time state
*   (Facultatif) Fonction d’accélération: une fonction qui décrit l’interpolation entre l’image clé actuelle et l’image clé précédente (abordée plus tard)  

Exemple d’insertion d’une image clé au niveau du point à mi-chemin de l’animation:
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

**Remarque:** gardez les éléments suivants à l’esprit lorsque vous animez des couleurs à l’aide d’animations par images clés:
1.  Vous attachez StartAnimation à Visual.Brush, au lieu de Visual, et déclarez le paramètre de propriété **Color** en tant que paramètre à animer.
2.  Le composant «value» de l’image clé est défini par l’objet Colors à partir de l’espace de noms Windows.UI.
3.  Vous pouvez définir l’espace de couleurs par lequel passera l’interpolation en définissant la propriété InterpolationColorSpace. Valeurs possibles: a.  CompositionColorSpace.Rgb b.  CompositionColorSpace.Hsl


##Propriétés des animations par images clés
Une fois que vous avez défini votre animation par images clés ainsi que les images clés individuelles, vous pouvez définir plusieurs propriétés de votre animation.
*   DelayTime – time before an animation starts after StartAnimation() is called
*   Duration – duration of the animation
*   IterationBehavior – count or infinite repeat behavior for an animation
*   IterationCount – number of finite times a KeyFrame Animation will repeat
*   KeyFrameCount: indique le nombre d’images clés que contient une animation par images clés
*   StopBehavior: spécifie le comportement d’une valeur de propriété d’animation lorsque StopAnimation est appelé.  

Exemple de définition de la durée d’animation sur 5secondes:  
```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

##Fonctions d’accélération
Les fonctions d’accélération (CompositionEasingFunction) indiquent comment les valeurs intermédiaires progressent de la valeur d’image clé précédente à la valeur d’image clé actuelle. If you do not provide an easing function for the KeyFrame, a default curve will be used.  
Deux types de fonction d’accélération sont pris en charge :
*   Linéaire
*   Courbe de Bézier cubique  

Les courbes de Bézier cubiques sont des fonctions paramétriques, souvent utilisées pour décrire des courbes lisses pouvant être mises à l’échelle. Lorsque vous utilisez des animations par images clés de composition, vous définissez deux points de contrôle qui sont des objets Vector2. Ces points de contrôle sont utilisés pour définir la forme de la courbe. Nous vous recommandons d’utiliser des sites similaires (comme [celui-ci](http://cubic-bezier.com/#0,-0.01,.48,.99)) pour visualiser la façon dont les deux points de contrôle créent une courbe de Bézier cubique.

Pour créer une fonction d’accélération, utilisez la méthode Constructor de l’objet Compositor. Voici deux exemples de création d’une fonction d’accélération linéaire et de courbe de Bézier cubique easeIn de base.  
```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```
Pour ajouter la fonction d’accélération à votre image clé, ajoutez simplement le troisième paramètre à l’image clé lors de son insertion dans l’animation:   
Exemple d’ajout d’une fonction d’accélération easeIn avec l’image clé:  
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

##Démarrage et arrêt des animations par images clés
Une fois votre animation et images clés définies, l’animation peut être lancée. Lorsque vous lancez l’animation, vous spécifiez l’élément visuel et la propriété cible devant être animés et créez une référence à l’animation. Pour ce faire, appelez la fonction StartAnimation(). Gardez à l’esprit qu’un appel de StartAnimation() sur une propriété déconnectera et supprimera toutes les animations déjà en cours d’exécution.  
**Remarque:** la référence à la propriété que vous souhaitez animer est sous forme de chaîne.  

Exemple de définition et de lancement d’une animation sur la propriété Offset du Visual:  
```cs
targetVisual.StartAnimation("Offset", animation);
```  

Pour cibler une propriété de sous-canal, ajoutez ce dernier à la chaîne en définissant la propriété que vous souhaitez animer. Dans les exemples ci-dessus, la syntaxe était changée en StartAnimation("Offset.X", animation2), où animation2 était une animation ScalarKeyFrameAnimation.  

Une fois votre animation lancée, il vous est possible de l’arrêter avant que celle-ci ne termine. Pour ce faire, utilisez la fonction StopAnimation().  
Exemple d’arrêt d’une animation sur la propriété Offset de l’élément visuel:    
```cs
targetVisual.StopAnimation("Offset");
```

Vous avez également la possibilité de définir le comportement de l’animation lorsque celle-ci est arrêtée explicitement. Pour ce faire, définissez la propriété StopBehavior de votre animation. Il existe trois options possibles:
*   LeaveCurrentValue: l’animation marquera la valeur de la propriété animée comme devant être la dernière valeur calculée de l’animation
*   SetToFinalValue: l’animation marquera la valeur de la propriété animée comme devant être la valeur de la dernière image clé
*   SetToInitialValue: l’animation marquera la valeur de la propriété animée comme devant être la valeur de la première image clé  

Exemple de définition de propriété StopBehavior pour une animation par images clés:  
```cs
animation.StopBehavior = AnimationStopBehavior.LeaveCurrentValue;
```

##Événements d’achèvement d’animation
Grâce aux animations par images clés, les développeurs peuvent utiliser la fonction AnimationBatch pour agréger une ou plusieurs animations lorsque celles-ci sont terminées. Seuls les événements d’achèvement d’animation par images clés peuvent être agrégés en lot. Les expressions n’ont pas de fin déterminée; elles ne déclenchent donc pas d’événement d’achèvement. Si une animation par expressions est lancée au sein d’un lot, celle-ci s’exécutera comme prévu et ne sera pas concernée par le déclenchement du lot     

Un événement d’achèvement de lot est déclenché lorsque toutes les animations au sein du lot sont terminées. Le temps nécessaire au déclenchement de l’événement d’un lot dépend de l’animation la plus longue ou la plus retardée du lot.
L’agrégation des états de fin est utile lorsque vous devez savoir quand des groupes d’animations sélectionnées se terminent afin de planifier une autre tâche.  

Les lots seront supprimés lors du déclenchement de l’événement d’achèvement. Vous pouvez également appeler Dispose() à tout moment pour libérer les ressources plus tôt. Vous pouvez choisir de supprimer manuellement l’objet lot si une animation par lot se termine tôt et que vous ne souhaitez pas recueillir l’événement d’achèvement. Si une animation est interrompue ou annulée, l’événement d’achèvement se déclenche et sera pris en compte dans son lot. Pour en voir un exemple, consultez le Kit de développement logiciel (SDK) Animation_Batch sur le [GitHub Windows/Composition](http://go.microsoft.com/fwlink/p/?LinkId=789439).  
 
##Lots délimités
Pour agréger un groupe d’animations spécifique ou pour cibler l’événement d’achèvement d’une animation unique, créez un lot délimité.    
```cs
CompositionScopedBatch myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
``` 
Après la création d’un lot délimité, toutes les animations démarrées sont agrégées jusqu’à ce que le lot soit explicitement suspendu ou arrêté à l’aide des fonctions Suspend ou End.    

L’appel de la fonction Suspend permet d’arrêter l’agrégation des états de fin de l’animation tant que Resume n’est pas appelée. Cela vous permet d’exclure explicitement du contenu d’un lot donné.  

Dans l’exemple ci-dessous, l’animation ciblant la propriété Offset de VisualA ne sera pas incluse dans le lot:  
```cs
myScopedBatch.Suspend();
VisualA.StartAnimation("Offset", myAnimation);
myScopeBatch.Resume();
```

Pour terminer votre lot, vous devez appeler End(). Sans cet appel, le lot restera ouvert et collectera des objets en permanence.  
 
L’extrait de code et le diagramme ci-dessous montrent un exemple d’agrégation des animations par le lot afin d’effectuer le suivi des états de fin. Notez que dans cet exemple, les états de fin des Animations1, 3 et 4 feront l’objet d’un suivi par ce lot, mais pas l’Animation2.  
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
![](./images/composition-scopedbatch.png)
 
##Traitement par lot d’un événement d’achèvement d’une animation unique
Pour savoir quand une animation unique se termine, créez un lot délimité comprenant uniquement l’animation que vous ciblez. Par exemple:  
```cs
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

##Récupération des événements d’achèvement d’un lot

Lors du traitement par lot d’une ou plusieurs animations, la récupération de l’événement d’achèvement du lot se fait de la même manière. Vous inscrivez la méthode de gestion d’événement pour l’événement Terminé du lot ciblé.  

```cs
myScopedBatch.Completed += OnBatchCompleted;
``` 

##États de lot
Vous pouvez utiliser deux propriétés pour déterminer l’état d’un lot existant; IsActive et IsEnded.  

La propriété IsActive retourne la valeur true si un lot ciblé est ouvert à l’agrégation des animations. IsActive retourne la valeur false lorsqu’un lot est suspendu ou terminé.   

Si la propriété IsEnded retourne la valeur true, vous ne pouvez pas ajouter d’animation à ce lot spécifique. Un lot sera terminé lorsque vous appelez End() explicitement pour un lot spécifique.  
 
##Utilisation des animations par expressions
Les animations par expressions constituent un nouveau type d’animation, introduit par l’équipe de composition lors de la mise à jour Windows10 de novembre (Build10586). D’un point de vue général, les animations par expressions sont basées sur des équations mathématiques/des relations entre des valeurs discrètes et des références à d’autres propriétés d’objet de composition. Contrairement aux animations par images clés, qui utilisent une fonction d’interpolateur (courbe de Bézier cubique, Quad, Quintic, etc.) pour décrire la façon dont la valeur change au fil du temps, les animations par expressions utilisent une équation mathématique pour définir la façon dont la valeur animée est calculée pour chaque image. Il est important de souligner que les animations par expressions n’ont pas de durée définie; une fois démarrées, elles s’exécutent et utilisent des équations mathématiques pour déterminer la valeur de la propriété d’animation jusqu’à ce qu’elles soient explicitement arrêtées.

**En quoi les animations par expressions sont-elles utiles?** La puissance réelle des animations par expressions provient de leur aptitude à créer une relation mathématique qui inclut des références à des paramètres ou des propriétés sur d’autres objets. Cela signifie qu’une équation peut faire référence à des valeurs de propriétés sur d’autres objets de composition, à des variables locales, voire à des valeurs partagées dans les jeux de propriétés de composition. En raison de ce modèle de référence et du fait que l’équation est évaluée à chaque image, si les valeurs qui définissent une équation changent, la sortie de cette dernière changera également. Cela crée de nouvelles perspectives au-delà des animations par images clés traditionnelles, dans lesquelles les valeurs doivent être discrètes et prédéfinies. Par exemple, des expériences comme les en-têtes rémanents et l’effet parallaxe peuvent être décrites facilement à l’aide des animations par expressions.

**Remarque:** nous employons les termes «Expression» ou «Chaîne d’expression» en tant que référence à l’équation mathématique qui définit votre objet d’animation par expressions.

##Création et attachement de l’animation par expressions
Avant de nous plonger dans la syntaxe de création d’animations par expressions, passons en revue les principes fondamentaux suivants:  
*   Les animations par expressions utilisent une équation mathématique définie pour déterminer la valeur de la propriété d’animation à chaque image.
*   L’équation mathématique est entrée dans l’Expression en tant que chaîne.
*   La sortie de l’équation mathématique doit se résoudre vers le même type que la propriété que vous souhaitez animer. Si les types ne sont pas identiques, une erreur apparaît lorsque l’Expression est calculée. Si l’équation se résout en Nan (nombre/0), le système utilisera la dernière valeur calculée précédemment.
*   Les animations par expressions ont une *durée de vie infinie*; elles s’exécuteront jusqu’à leur arrêt manuel.  

Pour créer votre animation par expressions, utilisez simplement le constructeur de votre objet de composition, dans lequel vous définissez votre expression mathématique.  
 
Voici un exemple de constructeur dans lequel une expression très basique combine deux valeurs Scalar. (Nous aborderons des expressions plus complexes dans la section suivante):  
```cs
var expression = _compositor.CreateExpressionAnimation("0.2 + 0.3");
```
À l’instar des animations par images clés, une fois que vous avez défini votre animation par expressions, vous devez l’attacher à l’élément visuel et déclarer la propriété devant être animée par l’animation. L’exemple qui suit constitue la suite de l’exemple ci-dessus; nous attachons notre animation par expressions à la propriété Opacity de l’élément visuel (de type Scalar):  
```cs
targetVisual.StartAnimation("Opacity", expression);
```

##Composants de la chaîne d’expression
Dans l’exemple de la section précédente, deux valeurs Scalar simples ont été ajoutées ensemble. Bien qu’il s’agisse d’un exemple valable d’Expression, il n’illustre pas l’ensemble des manipulations que permettent les expressions. Il est important de souligner que les valeurs de l’exemple ci-dessus sont des valeurs discrètes; de ce fait, chaque image de l’équation se résout en 0.5, ce qui ne changera pas pendant toute la durée de vie de l’animation. Le potentiel réel des Expressions provient de la définition d’une relation mathématique dans laquelle les valeurs peuvent changer régulièrement, voire tout le temps.  
 
Passons en revue les différentes parties qui composent ces types d’Expressions.  

###Opérateurs, priorité et associativité
La chaîne d’expression prend en charge l’utilisation des opérateurs standard décrivant les relations mathématiques entre les différents composants de l’équation:  

|Catégorie|  Opérateurs|
|--------|-----------|
|Unaire| -|
|Multiplicatif|    * /|
|Additif|  + -|

De même, lorsque l’Expression est évaluée, elle respecte la priorité et l’associativité des opérateurs telles qu’elles sont définies dans la spécification du langageC#. Autrement dit, elle respecte un ordre basique des opérations.  

Lorsque l’exemple ci-dessous sera évalué, les parenthèses seront résolues en premier, puis le reste de l’équation en fonction de l’ordre des opérations:  
```cs
"(5.0 * (72.4 – 36.0) + 5.0" // (5.0 * 36.4 + 5) -> (182 + 5) -> 187
```

###Paramètres de propriété
Les paramètres de propriété sont l’un des composants les plus puissants des animations par expressions. Dans la chaîne d’expression, vous pouvez référencer les valeurs des propriétés à partir d’autres objets, tels que CompositionVisual, CompositionPropertySet et d’autres objets C#.   

Pour utiliser ces éléments dans une chaîne d’Expression, définissez simplement les références en tant que paramètres de l’animation par expressions. Pour cela, vous devez mapper la chaîne utilisée dans l’Expression vers l’objet réel. Cela permet au système de savoir ce qu’il doit inspecter pour calculer la valeur lors de l’évaluation de l’équation. Il existe différents types de paramètres en corrélation avec le type de l’objet que vous souhaitez inclure dans l’équation:  

|Type|  Fonction permettant de créer le paramètre|
|----|------------------------------|
|Scalar|    SetScalarParameter(String ref, Scalar obj)|
|Vector|    SetVector2Parameter(String ref, Vector2 obj)<br/>SetVector3Parameter(String ref, Vector3 obj)<br/>SetVector4Parameter(String ref, Vector4 obj)|
|Matrix|    SetMatrix3x2Parameter(String ref, Matrix3x2 obj)<br/>SetMatrix4x4Parameter(String ref, Matrix4x4 obj)|
|Quaternion|    SetQuaternionParameter(String ref, Quaternion obj)|
|Color| SetColorParameter(String ref, Color obj)|
|CompositionObject| SetReferenceParameter(String ref, Composition object obj)|

Dans l’exemple ci-dessous, nous créons une animation par expressions qui référencera la propriété Offset des deux autres éléments visuels de composition ainsi qu’un objet Vector3System.Numerics de base.  
```cs
var commonOffset = new Vector3(25.0, 17.0, 10.0);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + additionalOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetVector3Parameter("additionalOffset", commonOffset);
```

En outre, vous pouvez référencer une valeur dans un jeu de propriété à partir d’une expression en utilisant le modèle décrit ci-dessus. Les jeux de propriétés de composition sont un moyen utile pour stocker les données utilisées par les animations. Elles sont également utiles pour créer des données pouvant être partagées et réutilisées, et qui ne dépendent pas de la durée de vie d’un objet de composition tiers. Les valeurs des jeux de propriétés peuvent être référencées dans une expression semblable aux autres références de propriétés. (Les jeux de propriétés sont davantage décrits plus loin dans cet article)  

Nous pouvons modifier l’exemple ci-dessus de manière à ce qu’un jeu de propriétés soit utilisé pour définir la variable commonOffset au lieu d’une variable locale:
```cs
_sharedProperties = _compositor.CreatePropertySet();
_sharedProperties.InsertVector3("commonOffset", offset);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + sharedProperties.commonOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

Enfin, lorsque vous référencez les propriétés d’autres objets, il est également possible de faire référence aux propriétés du sous-canal, dans la chaîne d’expression ou dans le cadre du paramètre de référence.  
 
Dans l’exemple ci-dessous, nous faisons référence au sous-canal x des propriétés Offset à partir de deux éléments visuels: le premier au sein de la chaîne d’expression et l’autre lors de la création de la référence de paramètre.
Notez que, lorsque vous référencez le composant X d’Offset, nous changeons notre type de paramètre en paramètre Scalar au lieu de Vector3, comme dans l’exemple précédent:  
```cs
var expression = _compositor.CreateExpressionAnimation("xOffset/ ParentOffset.X");
expression.SetScalarParameter("xOffset", childVisual.Offset.X);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
```

###Expression: Fonctions d’assistance et constructeurs
Outre l’accès à des opérateurs et des paramètres de propriété, vous pouvez exploiter une liste de fonctions mathématiques à utiliser dans ces expressions. Ces fonctions sont fournies pour effectuer des calculs et des opérations sur différents types, que vous ferez également sur des objets System.Numerics.  

Voici un exemple de création d’une Expression ciblant des paramètres Scalar, qui tire parti de la fonction d’assistance Clamp:  
```cs
var expression = _compositor.CreateExpressionAnimation("Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)"
```

Outre les fonctions d’assistance, vous pouvez utiliser les méthodes Constructor intégrées au sein d’une chaîne d’expression qui généreront une instance de ce type en fonction des paramètres fournis.  

Voici un exemple de création d’une Expression qui définit un nouveau paramètre Vector3 dans la chaîne d’expression :  
```cs
var expression = _compositor.CreateExpressionAnimation("Offset / Vector3(targetX, targetY, targetZ");
```

Vous trouverez la liste complète des fonctions d’assistance et des constructeurs dans l’annexe; sinon, pour chaque type, consultez la liste ci-après:  
*   [Scalar](#scalar)
*   [Vector2](#vector2)
*   [Vector3](#vector3)
*   [Matrix3x2](#matrix3x2)
*   [Matrix4x4](#matrix4x4)
*   [Quaternion](#quaternion)
*   [Color](#color)  

###Mots-clés d’expression
Vous pouvez tirer parti des «mots-clés» spéciaux qui sont traités différemment lorsque la chaîne d’expression est évaluée. Étant donné leur nature de «mots-clés», ces derniers ne peuvent pas être utilisés en tant que paramètres de chaîne de leurs références de propriété.  
 
|Mot-clé|   Description|
|-------|--------------|
|This.StartingValue| Fournit une référence à la valeur de départ d’origine de la propriété animée.|
|This.CurrentValue| Fournit une référence à la valeur actuelle «connue» de la propriété|
|Pi| Fournit une référence de mot-clé à la valeur de PI|

Voici un exemple d’utilisation du mot-clé this.StartingValue:  
```cs
var expression = _compositor.CreateExpressionAnimation("this.StartingValue + delta");
```

###Expressions comportant des instructions conditionnelles
Outre la prise en charge des relations mathématiques à l’aide des opérateurs, des références de propriétés et des fonctions et constructeurs, vous pouvez également créer une expression contenant un opérateur ternaire:  
```
(condition ? ifTrue_expression : ifFalse_expression)
```

Des instructions conditionnelles permettent d’écrire des expressions basées sur une certaine condition; différentes relations mathématiques seront utilisées par le système pour calculer la valeur de la propriété d’animation. Les opérateurs ternaires peuvent être imbriqués en tant qu’expressions pour les instructions true ou false.  

Les opérateurs conditionnels suivants sont pris en charge dans l’instruction de condition: 
*   Equals (==)
*   Not Equals (!=)
*   Less than (&lt;)
*   Less than or equal to (&lt;=)
*   Great than (&gt;)
*   Great than or equal to (&gt;=)  

Les conjonctions suivantes sont prises en charge en tant qu’opérateurs ou fonctions dans l’instruction de condition:
*   Not: ! / Not(bool1)
*   And: &amp;&amp; / And(bool1, bool2)
*   Or: || / Or(bool1, bool2)  

Voici un exemple d’animation par expressions utilisant une instruction conditionnelle.  
```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f + (target.Offset.x / parent.Offset.x) : 1.0f");
```

##Images clés d’expression
Précédemment dans ce document, nous avons décrit comment créer des animations par images clés et vous avons présenté les animations par expressions ainsi que les composants nécessaires à la création d’une chaîne d’expression. Comment tirer parti de la puissance des animations par expressions, tout en exploitant l’interpolation du temps que procurent les animations par images clés? La réponse réside dans les images clés d’expression!  

Au lieu de définir une valeur discrète pour chaque point de contrôle dans l’animation par images clés, cette valeur peut prendre la forme d’une chaîne d’expression. Dans ce cas, le système utilisera la chaîne d’expression pour calculer la valeur de la propriété d’animation à un moment donné dans la chronologie. Le système sera simplement interpolé vers cette valeur, comme dans une animation par images clés normale.    

Vous n’avez pas besoin de créer des animations spéciales pour utiliser les images clés d’expression; insérez simplement une ExpressionKeyFrame dans votre animation par images clés standard, puis renseignez le temps et la chaîne d’expression en tant que valeur. Voici un exemple illustratif, utilisant une chaîne d’expression en tant que valeur pour l’une des images clés:   
```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

##Exemple d’expression
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

##Animation avec des jeux de propriétés
Les jeux de propriétés de composition vous permettent de stocker des valeurs pouvant être partagées entre plusieurs animations et qui ne sont pas liées à la durée de vie d’un objet de composition tiers. Les jeux de propriétés sont extrêmement utiles pour stocker des valeurs courantes, puis les référencer ultérieurement (et facilement) dans les animations. Vous pouvez également utiliser les jeux de propriétés pour stocker des données basées sur une logique d’application pour l’exécution d’une expression.  

Pour créer un jeu de propriété, utilisez la méthode Constructor de l’objet Compositor:  
```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Une fois que vous avez créé votre jeu de propriétés, vous pouvez ajouter une propriété et une valeur à ce dernier:  
```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

À l’instar de ce que nous avons pu voir précédemment, nous pouvons faire référence à cette valeur de jeu de propriété dans une animation par expressions:  
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


Vous vous demandez peut-être ce qui se passerait pour la valeur de la propriété animée à laquelle est attachée l’animation par expressions si ce code était exécuté dans une application. Dans ce cas, l’expression sort d’abord en tant que valeur. Cependant, dès que l’animation par images clés anime la propriété du jeu de propriétés, la valeur de l’expression sera également mise à jour, étant donné que l’équation est calculée à chaque image. C’est toute la magie des jeux de propriété avec des animations par images clés et par expressions!  
 
##ManipulationPropertySet
Outre l’utilisation de jeux de propriétés de composition, un développeur peut également accéder à des ManipulationPropertySet, qui permettent d’accéder à des propriétés sur XAML ScrollViewer. Ces propriétés peuvent ensuite être utilisées, puis référencées dans une animation par expressions afin de propulser des expériences telles que l’effet parallaxe et les en-têtes rémanents. Remarque: vous pouvez saisir le ScrollViewer d’un contrôle XAML quelconque (ListView, GridView, etc.) comportant un contenu avec défilement, puis utiliser ce ScrollViewer afin d’obtenir ManipulationPropertySet pour les contrôles de défilement en question.  

Dans votre Expression, vous pouvez référencer les propriétés suivantes de la visionneuse à défilement:  

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

Après avoir configuré le paramètre de référence, vous pouvez faire référence aux propriétés ManipulationPropertySet dans l’Expression.  
```csharp
exp.Expression = “ScrollManipulation.Translation.Y / ScrollBounds”;
_target.StartAnimation(“Opacity”, exp);
```



 
 
##Annexe
###Fonctions d’expression par type de structure
###Scalar  

|Fonction et opérations de constructeur| Description|  
|-----------------------------------|--------------|  
|Abs(valeur Float)|  Retourne une valeur Float représentant la valeur absolue du paramètre Float|  
|Clamp (valeur Float, Float min, Float max)|  Retourne une valeur Float qui est supérieure à la valeur minimale et inférieure à la valeur maximale ou minimale si la valeur est inférieure à min ou max si la valeur est supérieure à la valeur maximale|  
|Max (valeur1 de Float, valeur2 de Float)|  Retourne la valeur Float supérieure entre la valeur1 et la valeur2.|  
|Min (valeur1 de Float, valeur2 de Float)|  Retourne la valeur Float inférieure entre la valeur1 et la valeur2.|  
|Lerp(valeur1 de Float, valeur2 de Float, progression de Float)|  Retourne une valeur Float qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Scalar en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|  
|Slerp(valeur1 de Float, valeur2 de Float, progression de Float)| Retourne une valeur Float qui représente l’interpolation sphérique calculée entre les deux valeurs Float en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|  
|Mod(valeur1 de Float, valeur2 de Float)|   Retourne le reste de la valeur Float résultant de la division des valeurs1 et 2|  
|Ceil(valeur Float)|     Retourne le paramètre Float arrondi au nombre entier supérieur suivant|  
|Floor(valeur Float)|    Retourne le paramètre Float arrondi au nombre entier inférieur suivant|  
|Sqrt(valeur Float)| Retourne la racine carrée du paramètre Float|  
|Square(valeur Float)|   Retourne le carré du paramètre Float|  
|Sin(valeur1 de Float)||
|Asin(valeur2 de Float)|    Retourne le Sin ou l’ArcSin du paramètre Float|
|Cos(valeur1 de Float)||
|ACos(valeur2 de Float)|    Retourne le Cos ou l’ArcSin du paramètre Float|
|Tan(valeur1 de Float)||
|ATan(valeur2 de Float)|    Retourne le Tan ou l’ArcTan du paramètre Float|
|Round(valeur Float)|    Retourne le paramètre Float arrondi au nombre entier le plus proche|
|Log10(valeur Float)|    Retourne le résultat Log (base 10) du paramètre Float|
|Ln(valeur Float)|   Retourne le résultat NaturalLog du paramètre Float|
|Pow (valeur Float, puissance Float)| Retourne le résultat du paramètre Float élevé à une puissance spécifique|
|ToDegrees(valeurs Float en radians)|  Retourne le paramètre Float converti en degrés|
|ToRadians (degrés Float)|  Retourne le paramètre Float converti en radians|

###Vector2  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Abs (valeur Vector2)|   Retourne une valeur Vector2 avec une valeur absolue appliquée à chacun des composants|
|Clamp (valeur1 de Vector2, Vector2 min, Vector2 max)|  Retourne un Vector2 contenant les valeurs limitées pour chaque composant respectif|
|Max (valeur1 de Vector2 , valeur2 de Vector2)|  Retourne une valeur Vector2 ayant atteint une valeur Max sur chacun des composants correspondants de valeur1 et valeur2|
|Min (valeur1 de Vector2 , valeur2 de Vector2)|  Retourne une valeur Vector2 ayant atteint une valeur Min sur chacun des composants correspondants de valeur1 et valeur2|
|Scale(valeur Vector2, facteur Float)|    Retourne un Vector2 avec chaque composant du vecteur multiplié par le facteur d’échelle.|
|Transform(valeur Vector2, matrice Matrix3x2)|    Retourne une valeur Vector2 résultant de la transformation linéaire entre Vector2 et Matrix3x2 (c’est-à-dire la multiplication d’un vecteur par une matrice).|
|Lerp(valeur1 de Vector2, valeur2 de Vector2, progression de Float)|  Retourne une valeur Vector2 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Vector2 en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|
|Length(valeur Vector2)| Retourne une valeur Float qui représente la longueur/la magnitude de Vector2|
|LengthSquared(Vector2)|    Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’une valeur Vector2|
|Distance(valeur1 de Vector2 , valeur2 de Vector2)|  Retourne une valeur Float qui représente la distance entre deux valeurs Vector2|
|DistanceSquared(valeur1 de Vector2 , valeur2 de Vector2)|   Retourne une valeur Float qui représente le carré de la distance entre deux valeurs Vector2|
|Normalize(valeur Vector2)|  Retourne une valeur Vector2 représentant le vecteur unitaire du paramètre dans lequel tous les composants ont été normalisés|
|Vector2(Float x et Float y)| Crée un Vector2 à l’aide de deux paramètres Float|

###Vector3  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Abs (valeur Vector3)|   Retourne une valeur Vector3 avec une valeur absolue appliquée à chacun des composants|
|Clamp (valeur1 de Vector3, Vector3 min, Vector3 max)|  Retourne une valeur Vector3 contenant les valeurs limitées pour chaque composant respectif|
|Max (valeur1 de Vector3, valeur2 de Vector3)|  Retourne une valeur Vector3 ayant atteint une valeur Max sur chacun des composants correspondants de valeur1 et valeur2|
|Min (valeur1 de Vector3, valeur2 de Vector3)|  Retourne une valeur Vector3 ayant atteint une valeur Min sur chacun des composants correspondants de valeur1 et valeur2|
|Scale(valeur Vector3, facteur Float)|    Retourne un Vector3 avec chaque composant du vecteur multiplié par le facteur d’échelle.|
|Lerp(valeur1 de Vector3, valeur2 de Vector3, progression de Float)|  Retourne une valeur Vector3 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Vector3 en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|
|Length(valeur de Vector3)| Retourne une valeur Float qui représente la longueur/la magnitude de Vector3|
|LengthSquared(Vector3)|    Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’une valeur Vector3|
|Distance(valeur1 de Vector3, valeur2 de Vector3)|  Retourne une valeur Float qui représente la distance entre deux valeurs Vector3|
|DistanceSquared(valeur1 de Vector3, valeur2 de Vector3)|   Retourne une valeur Float qui représente le carré de la distance entre deux valeurs Vector3|
|Normalize(valeur Vector3)|  Retourne une valeur Vector3 représentant le vecteur unitaire du paramètre dans lequel tous les composants ont été normalisés|
|Vector3(Float x, Float y, Float z)|    Crée un Vector3 à l’aide de trois paramètres Float|

###Vector4  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Abs (valeur Vector4)|   Retourne une valeur Vector3 avec une valeur absolue appliquée à chacun des composants|
|Clamp (valeur1 de Vector4, Vector4 min, Vector4 max)|  Retourne une valeur Vector4 contenant les valeurs limitées pour chaque composant respectif|
|Max (valeur1 de Vector4, valeur2 de Vector4)|   Retourne une valeur Vector4 ayant atteint une valeur Max sur chacun des composants correspondants de valeur1 et valeur2|
|Min (valeur1 de Vector4, valeur2 de Vector4)|   Retourne une valeur Vector4 ayant atteint une valeur Min sur chacun des composants correspondants de valeur1 et valeur2|
|Scale(valeur Vector3, facteur Float)|    Retourne un Vector3 avec chaque composant du vecteur multiplié par le facteur d’échelle.|
|Transform(valeur Vector4, matrice Matrix4x4)|    Retourne une valeur Vector4 résultant de la transformation linéaire entre Vector4 et Matrix4x4 (c’est-à-dire la multiplication d’un vecteur par une matrice).|
|Lerp(valeur1 de Vector4, valeur2 de Vector4, progression de Float)|  Retourne une valeur Vector4 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Vector4 en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|
|Length(valeur Vector4)| Retourne une valeur Float qui représente la longueur/la magnitude de Vector4|
|LengthSquared(Vector4)|    Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’une valeur Vector4|
|Distance(valeur1 de Vector4, valeur2 de Vector4)|  Retourne une valeur Float qui représente la distance entre deux valeurs Vector4|
|DistanceSquared(valeur1 de Vector4, valeur2 de Vector4)|   Retourne une valeur Float qui représente le carré de la distance entre deux valeurs Vector4|
|Normalize(valeur de Vector4)|  Retourne une valeur Vector4 représentant le vecteur unitaire du paramètre dans lequel tous les composants ont été normalisés|
|Vector4(Float x, Float y, Float w)|   Crée un Vector4 à l’aide de quatre paramètres Float|

###Matrix3x2  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Scale(valeur Matrix3x2, facteur Float)|  Retourne une Matrix3x2 avec chaque composant de la matrice multiplié par le facteur d’échelle.|
|Inverse(valeur de Matrix3x2)| Retourne un objet Matrix3x2 qui représente la matrice réciproque|
|Lerp(valeur1 de Matrix3x2, valeur2 de Matrix3x2, progression de Float)|  Retourne une valeur Matrix3x2 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Matrix3x2 en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|
|Matrix3x2(Float M11, Float M12, Float M21, Float M22, Float M31, Float M32)|   Crée une matrice Matrix3x2 à l’aide de six paramètres Float|
|Matrix3x2.CreateFromScale(échelle Vector2)|  Crée une matrice Matrix3x2 à partir d’un Vector2 représentant une échelle<br/>\[scale.X, 0.0<br/> 0.0, scale.Y<br/> 0.0, 0.0 \]|
|Matrix3x2.CreateFromTranslation(translation de Vector2)|  Crée une matrice Matrix3x2 à partir d’un Vector2 représentant une translation<br/>\[1.0, 0.0,<br/> 0.0, 1.0,<br/> translation.X, translation.Y\]|
    
###Matrix4x4  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Scale(valeur Matrix4x4, facteur Float)|  Retourne une matrice Matrix4x4 avec chaque composant de la matrice multiplié par le facteur d’échelle.|
|Inverse(Matrix4x4)|    Retourne un objet Matrix4x4 qui représente la matrice réciproque|
|Lerp(valeur1 de Matrix4x4 , valeur2 de Matrix4x4, progression de Float)|  Retourne une valeur Matrix4x4 qui représente le calcul d’une interpolation linéaire calculée entre les deux valeurs Matrix4x4 en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|
|Matrix4x4(Float M11, Float M12, Float M13, Float M14,<br/>Float M21, Float M22, Float M23, Float M24,<br/>    Float M31, Float M32, Float M33, Float M34,<br/>    Float M41, Float M42, Float M43, Float M44)| Crée une matrice Matrix4x4 à l’aide de 16paramètres Float|
|Matrix4x4.CreateFromScale(échelle Vector3)|  Crée une matrice Matrix4x4 à partir d’un Vector3 représentant une échelle<br/>\[scale.X, 0.0, 0.0, 0.0,<br/> 0.0, scale.Y, 0.0, 0.0,<br/> 0.0, 0.0, scale.Z, 0.0,<br/> 0.0, 0.0, 0.0, 1.0\]|
|Matrix4x4.CreateFromTranslation(translation de Vector3)|  Crée une matrice Matrix4x4 à partir d’un Vector3 représentant une translation<br/>\[1.0, 0.0, 0.0, 0.0,<br/> 0.0, 1.0, 0.0, 0.0,<br/> 0.0, 0.0, 1.0, 0.0,<br/> translation.X, translation.Y, translation.Z, 1.0\]|
|Matrix4x4.CreateFromAxisAngle(axe Vector3, angle Float)|  Crée une matrice Matrix4x4 à partir d’un axe Vector3 et d’une valeur Float représentant un angle|

###Quaternion  

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|Slerp(valeur1 de Quaternion, valeur2 de Quaternion, progression de Float)|   Retourne une valeur Quaternion qui représente l’interpolation sphérique calculée entre les deux valeurs Quaternion en fonction de la progression (Remarque: la progression se situe entre 0.0 et 1.0)|
|Concatenate(valeur1 de Quaternion, valeur2 de Quaternion)|  Retourne une valeur Quaternion qui représente la concaténation de deux Quaternions (c’est-à-dire, un Quaternion qui représente deux rotations individuelles combinées)|
|Length(valeur Quaternion)|  Retourne une valeur Float qui représente la longueur/la magnitude du Quaternion.|
|LengthSquared(Quaternion)| Retourne une valeur Float qui représente le carré de la longueur/la magnitude d’un Quaternion|
|Normalize(valeur Quaternion)|   Retourne un Quaternion dont les composants ont été normalisés|
|Quaternion.CreateFromAxisAngle(axe Vector3, angle Scalar)|    Crée un Quaternion à partir d’un axe Vector3 et d’une valeur Scalar représentant un angle|
|Quaternion(Float x, Float y, Float z, Float w)|    Crée un Quaternion à partir de quatre valeurs Float|

###Color

|Fonction et opérations de constructeur|   Description|
|-----------------------------------|--------------|
|ColorLerp(colorTo de Color, colorFrom de Color, progression de Float)| Retourne un objet Color représentant la valeur calculée d’une interpolation linéaire entre deux objets Color, en fonction d’une progression donnée. (Remarque: la progression se situe entre 0.0 et 1.0)|
|ColorLerpRGB(colorTo de Color, colorFrom de Color, progression de Float)|  Retourne un objet Color représentant la valeur calculée d’une interpolation linéaire entre deux objets, en fonction d’une progression donnée dans l’espace de couleur RVB.|
|ColorLerpHSL(colorTo de Color, colorFrom de Color, progression de Float)|  Retourne un objet Color représentant la valeur calculée d’une interpolation linéaire entre deux objets, en fonction d’une progression donnée dans l’espace de couleur HSL.|
|ColorArgb(Float a, Float r, Float g, Float b)| Crée un objet représentant Color, défini par des composants ARGB|
|ColorHsl(Float h, Float s, Float l)|   Crée un objet représentant Color, défini par les composants HSL (Remarque: la teinte Hue est définie entre 0 et 2pi)|







<!--HONumber=Jun16_HO4-->


