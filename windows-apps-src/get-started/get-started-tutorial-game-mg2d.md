---
title: Créer un jeu UWP dans MonoGame2D
description: Un jeu UWP simple pour le Microsoft Store, écrit en c# et MonoGame
author: muhsinking
ms.author: mukin
ms.date: 03/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: d38465ce02e0aedf854094ede75fc33701b226a6
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5469632"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>Créer un jeu UWP dans MonoGame2D

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>Un jeu UWP simple en2D pour le Microsoft Store, écrit en C# et MonoGame


![Feuille de sprite Dino en marche](images/JS2D_0.png)

## <a name="introduction"></a>Introduction

MonoGame est une infrastructure légère de développement de jeux. Ce didacticiel vous explique les notions de base du développement de jeux dans MonoGame, notamment comment charger du contenu, dessiner des sprites, les animer et gérer les entrées utilisateur. Vous trouverez également des concepts plus avancés, tels que la détection des collisions et la prise en charge des écrans haute résolution. Ce didacticiel dure30 à 60minutes.

## <a name="prerequisites"></a>Éléments prérequis
+   Windows10 et Microsoft Visual Studio2017.  [Cliquez ici pour savoir comment prendre en main VisualStudio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ L’infrastructure de développement de bureau .NET. Si vous n’avez pas encore cela installé, vous pouvez l’obtenir en réexécuter le programme d’installation de Visual Studio et en modifiant votre installation de Visual Studio 2017.
+   Connaissances élémentaires de C# ou d'un langage de programmation similaire orienté objet. [Cliquez ici pour savoir comment commencer en C#](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).
+   La connaissance de concepts informatiques élémentaires tels que les classes, les méthodes ou les variables sera un plus.

## <a name="why-monogame"></a>Pourquoi MonoGame?
Le choix ne manque pas en matière d'environnements de développement de jeux. Entre les moteurs complets comme Unity et les API multimédias exhaustives et complexes telles que DirectX, savoir par où commencer peut s'avérer difficile. MonoGame est un ensemble d’outils dont le niveau de complexité se situe entre un moteur de jeu et une API plus fruste comme DirectX. Il fournit un pipeline de contenu facile à utiliser, ainsi que toutes les fonctionnalités requises pour créer des jeux légers qui s’exécutent sur un large éventail de plateformes. De plus, les applications MonoGame sont écrites en c# pur et vous pouvez les distribuer rapidement via le Microsoft Store ou d’autres plateformes de distribution similaires.

## <a name="get-the-code"></a>Obtenir le code
Si vous ne souhaitez pas suivre le didacticiel pas à pas et préférez juste voir MonoGame en action, [cliquez ici pour obtenir l’application terminée](https://github.com/Microsoft/Windows-appsample-get-started-mg2d).

Ouvrez le projet dans Visual Studio 2017 et appuyez sur **F5** pour exécuter l’exemple. Cela peut prendre un certain temps la première fois, puisque Visual Studio doit extraire les packages NuGet qui peuvent manquer dans votre installation.

Si cela est déjà fait, ignorez la section suivante sur la configuration de MonoGame pour passer directement à une explication pas à pas du code.

**Remarque:** le jeu créé dans cet exemple ne vise pas à être complet (ni même amusant). Son seul objectif est d’illustrer l’ensemble des concepts principaux du développement 2D dans MonoGame. N’hésitez pas à utiliser ce code pour créer quelque chose de plus intéressant ou même repartir de zéro lorsque vous maîtriserez les concepts de base!

## <a name="set-up-monogame-project"></a>Configuration du projet MonoGame
1. Installez **MonoGame3.6** pour Visual Studio à partir de [MonoGame.net](http://www.monogame.net/)

2. Démarrez Visual Studio 2017.

3. Accédez à **Fichier -> Nouveau -> Projet**

4. Sous les modèles de projet Visual C#, sélectionnez **MonoGame** et **Projet MonoGame Universal Windows10**

5. Nommez votre projet «MonoGame2D» et sélectionnez OK. Une fois créé, le projet aura probablement l'air truffé d'erreurs. Celles-ci devraient disparaître lorsque vous aurez exécuté le projet pour la première fois et que tous les packages NuGet manquants auront été installés.

6. Assurez-vous que **x86** et **Local Machine** sont définis comme plateforme cible, puis appuyez sur **F5** pour générer et exécuter le projet vide. Si vous avez suivi les étapes ci-dessus, vous devez voir apparaître une fenêtre bleue vide une fois que le projet a achevé la génération.

## <a name="method-overview"></a>Vue d’ensemble de la méthode
Maintenant que vous avez créé le projet, ouvrez le fichier **Game1.cs** depuis **Explorateur de solutions**. C'est là qu'ira la majeure partie de la logique du jeu. De nombreuses méthodes cruciales sont générées automatiquement ici lorsque vous créez un nouveau projet MonoGame. Passons-les rapidement en revue:

**public Game1()** Le constructeur. Nous ne modifierons pas du tout cette méthode dans ce didacticiel.

**protected override void Initialize()** Ici, nous initialisons toutes les variables de classe utilisées. Cette méthode est appelée une fois au début du jeu.

**protected override void LoadContent()** Cette méthode charge le contenu (par ex. textures, sons, polices) dans la mémoire avant le démarrage du jeu. Comme Initialize, elle est appelée une fois lorsque l’application démarre.

**protected override void UnloadContent()** Cette méthode est utilisée pour décharger le contenu autre que celui du Gestionnaire de contenu. Nous ne l’utilisons pas du tout.

**protected override void Update(GameTime gameTIme)** Cette méthode est appelée une fois à chaque cycle de la boucle de jeu. Ici, nous mettons à jour les états de n’importe quel objet ou de n’importe quelle variable utilisée dans le jeu. Cela inclut des éléments tels que la position d’un objet, sa vitesse ou sa couleur. C'est également là que les entrées de l’utilisateur sont gérées. Bref, cette méthode traite chaque partie de la logique du jeu à l'exception du dessin des objets à l’écran.
**protected override void Draw(GameTime gameTime)** C'est là que les objets sont dessinés sur l’écran à l’aide des positions données par la méthode Update.

## <a name="draw-a-sprite"></a>Dessin d'un sprite
Vous avez donc exécuté votre projet MonoGame tout neuf et trouvé un joli ciel bleu. Ajoutons-y un sol.
Dans MonoGame, les graphismes2D sont ajoutés à l’application sous forme de «sprites». Un sprite est tout simplement un élément d'infographie que l'on manipule comme s'il s'agissait d'une seule entité. Les sprites peuvent être déplacés, mis à l’échelle, déformés, animés et combinés pour créer tout ce que vous pouvez imaginer dans l’espace2D.

### <a name="1-download-a-texture"></a>1. Téléchargement d'une texture
Dans notre cas, ce premier sprite va être parfaitement ennuyeux. [Cliquez ici pour télécharger ce rectangle vert indifférencié](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png).

### <a name="2-add-the-texture-to-the-content-folder"></a>2. Ajout de la texture au dossier Content
- Ouvrez l'**Explorateur de solutions**
- Cliquez avec le bouton droit sur **Content.mgcb** dans le dossier **Contenu** et sélectionnez **Ouvrir avec**. Dans le menu contextuel, sélectionnez **Pipeline Monogame**, puis **OK**.
- Dans la nouvelle fenêtre, cliquez avec le bouton droit sur l’élément **Contenu** et sélectionnez **Ajouter -> Élément existant**.
- Recherchez et sélectionnez le rectangle vert dans l’Explorateur de fichiers
- Nommez l’élément «grass.png» et sélectionnez **Ajouter**.

### <a name="3-add-class-variables"></a>3. Ajout des variables de classe
Pour charger cette image en tant que texture de sprite, ouvrez **Game1.cs** et ajoutez les variables de classe suivantes.

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

La variable SKYRATIO nous indique quelle part de la scène nous voulons accorder au ciel par rapport à l’herbe: ici, les deux tiers. **screenWidth** et **screenHeight** assureront le suivi de la taille de la fenêtre de l’application, et **grass** est l'endroit où nous stockerons notre rectangle vert.

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. Initialisation des variables de classe et définition de la taille de la fenêtre
Les variables **screenWidth** et **screenHeight** doivent encore être initialisées. Ajoutez ce code à la méthode **Initialize**:

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

Outre la hauteur et la largeur de l’écran, nous réglons également le mode de fenêtrage de l’application sur **Fullscreen**et nous rendons la souris invisible.

### <a name="5-load-the-texture"></a>5. Chargement de la texture
Pour charger la texture dans la variable d’herbe, ajoutez le code suivant à la méthode **LoadContent**:

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. Dessin du sprite
Pour dessiner le rectangle, ajoutez les lignes suivantes à la méthode **Draw**:

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

Ici, nous utilisons la méthode **spriteBatch.Draw** pour placer la texture donnée dans les limites d’un objet Rectangle. Un **Rectangle** est défini par les coordonnées x et y de ses angles supérieur gauche et inférieur droit. À l’aide des variables **screenWidth**, **screenHeight** et **SKYRATIO** que nous avons définies précédemment, nous dessinons la texture du rectangle sur le tiers inférieur de l’écran. Si vous exécutez le programme maintenant, vous devriez voir l’arrière-plan bleu précédent partiellement recouvert par le rectangle vert.

![Rectangle vert](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>Adaptation à des écrans haute résolution
Si vous utilisez Visual Studio sur un moniteur à haute densité de pixels, comme celui des Surface Pro ou Surface Studio, vous pouvez constater que le rectangle vert des étapes ci-dessus ne recouvre pas tout à fait le tiers inférieur de l’écran. Il flotte probablement au-dessus de l’angle inférieur gauche de l’écran. Pour résoudre ce problème et unifier l’apparence de notre jeu sur tous les appareils, nous allons devoir créer une méthode qui fait évoluer certaines valeurs en fonction de la densité de pixels de l’écran:

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

Remplacez ensuite les initialisations de **screenHeight** et **screenWidth** dans la méthode **Initialize** par ce qui suit:

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

Si vous utilisez un écran à fort taux de PPP et que vous essayez d’exécuter l’application maintenant, vous devriez voir le rectangle vert recouvrir le tiers inférieur de l’écran, comme prévu.

## <a name="build-the-spriteclass"></a>Générer la SpriteClass
Avant de commencer à animer les sprites, nous allons créer une nouvelle classe appelée «SpriteClass» qui nous permettra de simplifier la manipulation des sprites au niveau de la surface.

### <a name="1-create-a-new-class"></a>1. Création d'une nouvelle classe
Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur **MonoGame2D (Windows universel)** et sélectionnez **Ajouter -> Classe**. Nommez la classe «SpriteClass.cs», puis sélectionnez **Ajouter**.

### <a name="2-add-class-variables"></a>2. Ajout de variables de classe
Ajoutez ce code à la classe que vous venez de créer:

```CSharp
public Texture2D texture
{
  get;
}

public float x
{
  get;
  set;
}

public float y
{
  get;
  set;
}

public float angle
{
  get;
  set;
}

public float dX
{
  get;
  set;
}

public float dY
{
  get;
  set;
}

public float dA
{
  get;
  set;
}

public float scale
{
  get;
  set;
}
```

Ici, nous allons configurer les variables de classe dont nous avons besoin pour dessiner et animer un sprite. Les variables **x** et **y** représentent la position actuelle du sprite sur le plan, tandis que la variable **angle** est l'angle actuel du sprite en degrés (0étant vertical et 90représentant une inclinaison de 90degrés vers la droite). Il est important de noter que, pour cette classe, **x** et **y** représentent les coordonnées du **centre** du sprite (l’origine par défaut est le coin supérieur gauche). Cela facilite la rotation des sprites, lesquels pivoteront autour de l’origine indiquée et sachant qu'une rotation autour du centre nous donne un mouvement de rotation uniforme.

Après cela, nous avons **dX**, **dY** et **dA**, qui sont les taux respectifs de changements par seconde des variables **x**, **y** et **angle**.

### <a name="3-create-a-constructor"></a>3. Création d’un constructeur
Lors de la création d’une instance de **SpriteClass**, nous fournissons le constructeur avec le périphérique graphique de **Game1.cs**, le chemin d’accès à la texture par rapport au dossier du projet et l’échelle désirée de la texture par rapport à sa taille d’origine. Nous définirons le reste des variables de classe une fois que nous aurons démarré le jeu, dans la méthode de mise à jour.

```CSharp
public SpriteClass (GraphicsDevice graphicsDevice, string textureName, float scale)
{
  this.scale = scale;
  if (texture == null)
  {
    using (var stream = TitleContainer.OpenStream(textureName))
    {
      texture = Texture2D.FromStream(graphicsDevice, stream);
    }
  }
}
```

### <a name="4-update-and-draw"></a>4. Mise à jour et dessin
Nous devons encore ajouter une ou deux méthodes à la déclaration SpriteClass:

```CSharp
public void Update (float elapsedTime)
{
  this.x += this.dX * elapsedTime;
  this.y += this.dY * elapsedTime;
  this.angle += this.dA * elapsedTime;
}

public void Draw (SpriteBatch spriteBatch)
{
  Vector2 spritePosition = new Vector2(this.x, this.y);
  spriteBatch.Draw(texture, spritePosition, null, Color.White, this.angle, new Vector2(texture.Width/2, texture.Height/2), new Vector2(scale, scale), SpriteEffects.None, 0f);
}
```

La méthode SpriteClass **Update** est appelée dans la méthode **Update** de Game1.cs et sert à mettre à jour les valeurs de sprites **x**, **y** et **angle** en fonction de leurs taux de modification respectifs.

La méthode **Draw** est appelée dans la méthode **Draw** de Game1.cs et sert à dessiner le sprite dans la fenêtre de jeu.

## <a name="user-input-and-animation"></a>Entrées de l’utilisateur et animation
Maintenant que la SpriteClass est créée, nous allons l’utiliser pour créer deux nouveaux objets du jeu. Le premier est un avatar que le joueur peut contrôler avec les touches de direction et la barre d’espace. La seconde est un objet que le joueur doit éviter.

### <a name="1-get-the-textures"></a>1. Obtention des textures
Pour l’avatar du joueur, nous allons utiliser le Chat ninja de Microsoft lui-même, chevauchant son fidèle T-Rex. [Cliquez ici pour télécharger l’image](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png).

Occupons-nous maintenant de l’obstacle que le joueur doit éviter. Qu'est-ce que les chats ninjas et les dinosaures carnivores détestent plus que tout? Manger des légumes! [Cliquez ici pour télécharger l’image](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png).

Exactement comme pour le rectangle vert, ajoutez ces images à **Content.mgcb** via le **Pipeline MonoGame** et appelez-les respectivement «ninja-cat-dino.png» et «broccoli.png».

### <a name="2-add-class-variables"></a>2. Ajout de variables de classe
Ajoutez le code suivant à la liste des variables de classe dans **Game1.cs**:

```CSharp
SpriteClass dino;
SpriteClass broccoli;

bool spaceDown;
bool gameStarted;

float broccoliSpeedMultiplier;
float gravitySpeed;
float dinoSpeedX;
float dinoJumpY;
float score;

Random random;
```

**dino** et **broccoli** sont les variables de notre SpriteClass. **dino** contiendra l’avatar du joueur, tandis que **broccoli** contiendra l’obstacle brocoli.

**spaceDown** suit la position de la barre d’espace pour déterminer si elle maintenue enfoncée ou enfoncée, puis relâchée.

**gameStarted** nous prévient si c'est la première fois que l’utilisateur a démarré le jeu.

**broccoliSpeedMultiplier** détermine la vitesse de déplacement de l’obstacle brocoli sur l’écran.

**gravitySpeed** détermine la vitesse à laquelle l’avatar du joueur accélère vers le bas après un saut.

**dinoSpeedX** et **dinoJumpY** déterminent la vitesse avec laquelle l’avatar du joueur se déplace et saute.
score compte les obstacles que le joueur a correctement évités.

Enfin, **random** servira à ajouter une dose de hasard au comportement de l’obstacle brocoli.

### <a name="3-initialize-variables"></a>3. Initialisation des variables
Nous devons ensuite initialiser ces variables. Ajoutez le code suivant à la méthode:

```CSharp
broccoliSpeedMultiplier = 0.5f;
spaceDown = false;
gameStarted = false;
score = 0;
random = new Random();
dinoSpeedX = ScaleToHighDPI(1000f);
dinoJumpY = ScaleToHighDPI(-1200f);
gravitySpeed = ScaleToHighDPI(30f);
```

Notez que les trois dernières variables doivent être mises à l’échelle pour les périphériques haute résolution, car ils spécifient un taux de changement en pixels.

### <a name="4-construct-spriteclasses"></a>4. Construction de SpriteClasses
Nous allons construire des objets SpriteClass dans la méthode **LoadContent**. Ajoutez ce code à ce que vous avez déjà ici:

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

La taille de l’image du brocoli est nettement plus grande que celle que nous voulons lui donner dans le jeu. Nous allons donc la ramener à 0,2fois sa taille d’origine.

### <a name="5-program-obstacle-behavior"></a>5. Programmation du comportement d'un obstacle
Nous voulons que le brocoli soit généré quelque part hors de l'écran et se dirige dans la direction de l’avatar du joueur, qui doit alors les éviter. Pour cela, ajoutez cette méthode à la classe **Game1.cs**:

```CSharp
public void SpawnBroccoli()
{
  int direction = random.Next(1, 5);
  switch (direction)
  {
    case 1:
      broccoli.x = -100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 2:
      broccoli.y = -100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
    case 3:
      broccoli.x = screenWidth + 100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 4:
      broccoli.y = screenHeight + 100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
  }

  if (score % 5 == 0) broccoliSpeedMultiplier += 0.2f;

  broccoli.dX = (dino.x - broccoli.x) * broccoliSpeedMultiplier;
  broccoli.dY = (dino.y - broccoli.y) * broccoliSpeedMultiplier;
  broccoli.dA = 7f;
}
```

La première partie de la méthode détermine le point de l'écran où sera généré l’objet brocoli, en utilisant pour cela deux nombres aléatoires.

La deuxième partie détermine la vitesse à laquelle le brocoli se déplace, ce qui est déterminé par le score du moment. Il accélérera chaque fois que le joueur aura réussi à en éviter cinq.

La troisième partie définit la direction du mouvement du sprite brocoli. Il pointe dans la direction de l’avatar du joueur (dino) lorsque le brocoli est généré. Nous lui attribuons également une valeur **dA** de 7f qui fait tournoyer le brocoli dans les airs en poursuivant le joueur.

### <a name="6-program-game-starting-state"></a>6. Programmation de l'état du jeu au démarrage
Avant de pouvoir passer à la gestion des entrées clavier, nous avons besoin d’une méthode qui définit l’état de jeu initial des deux objets que nous avons créés. Plutôt que le jeu démarre dès que l’application s’exécute, nous voulons que l’utilisateur le démarre manuellement en appuyant sur la barre d’espace. Ajoutez le code suivant qui définit l’état initial des objets animés et réinitialise le score:

```CSharp
public void StartGame()
{
  dino.x = screenWidth / 2;
  dino.y = screenHeight * SKYRATIO;
  broccoliSpeedMultiplier = 0.5f;
  SpawnBroccoli();  
  score = 0;
}
```

### <a name="7-handle-keyboard-input"></a>7. gérer les entrées du clavier
Nous devons ensuite une nouvelle méthode pour gérer les entrées utilisateur par le biais du clavier. Ajoutez cette méthode à **Game1.cs**:

```CSharp
void KeyboardHandler()
{
  KeyboardState state = Keyboard.GetState();

  // Quit the game if Escape is pressed.
  if (state.IsKeyDown(Keys.Escape))
  {
    Exit();
  }

  // Start the game if Space is pressed.
  if (!gameStarted)
  {
    if (state.IsKeyDown(Keys.Space))
    {
      StartGame();
      gameStarted = true;
      spaceDown = true;
      gameOver = false;
    }
    return;
  }            
  // Jump if Space is pressed
  if (state.IsKeyDown(Keys.Space) || state.IsKeyDown(Keys.Up))
  {
    // Jump if the Space is pressed but not held and the dino is on the floor
    if (!spaceDown && dino.y >= screenHeight * SKYRATIO - 1) dino.dY = dinoJumpY;

    spaceDown = true;
  }
  else spaceDown = false;

  // Handle left and right
  if (state.IsKeyDown(Keys.Left)) dino.dX = dinoSpeedX * -1;

  else if (state.IsKeyDown(Keys.Right)) dino.dX = dinoSpeedX;
  else dino.dX = 0;
}
```

Au-dessus, nous avons une série de quatre instructions si:

La première fait quitter le jeu si la touche **Échappement** est enfoncée.

La seconde démarre le jeu si la touche **Espace** est enfoncée et si le jeu n’a pas déjà démarré.

La troisième fait sauter l’avatar dino si **Espace** est enfoncé, en modifiant sa propriété **dY**. Notez que le joueur ne peut sauter que s’il se trouve sur le «sol» (dino.y = screenHeight * SKYRATIO). Il ne peut pas non plus sauter si la touche espace est maintenue enfoncée au lieu d'être enfoncée une seule fois. Cela empêche le dino de sauter dès que le jeu démarre, puisqu'il utilise la même touche que celle qui démarre le jeu.

Enfin, la dernière clause si/alors vérifie si les flèches directionnelles gauche ou droite sont enfoncées. Si tel est le cas, elle modifie la propriété **dX** du dino en conséquence.

**Défi:** pouvez-vous faire fonctionner la méthode ci-dessus de gestion par le clavier avec le schéma d’entrée WASD, ainsi qu'avec les touches fléchées?

### <a name="8-add-logic-to-the-update-method"></a>8. Ajout de logique à la méthode Update
Nous devons ensuite ajouter une logique pour toutes ces parties à la méthode **Update** dans **Game1.cs**:

```CSharp
float elapsedTime = (float)gameTime.ElapsedGameTime.TotalSeconds;
KeyboardHandler(); // Handle keyboard input
// Update animated SpriteClass objects based on their current rates of change
dino.Update(elapsedTime);
broccoli.Update(elapsedTime);

// Accelerate the dino downward each frame to simulate gravity.
dino.dY += gravitySpeed;

// Set game floor so the player does not fall through it
if (dino.y > screenHeight * SKYRATIO)
{
  dino.dY = 0;
  dino.y = screenHeight * SKYRATIO;
}

// Set game edges to prevent the player from moving offscreen
if (dino.x > screenWidth - dino.texture.Width/2)
{
  dino.x = screenWidth - dino.texture.Width/2;
  dino.dX = 0;
}
if (dino.x < 0 + dino.texture.Width/2)
{
  dino.x = 0 + dino.texture.Width/2;
  dino.dX = 0;
}

// If the broccoli goes offscreen, spawn a new one and iterate the score
if (broccoli.y > screenHeight+100 || broccoli.y < -100 || broccoli.x > screenWidth+100 || broccoli.x < -100)
{
  SpawnBroccoli();
  score++;
}
```

### <a name="9-draw-spriteclass-objects"></a>9. Dessin d'objets SpriteClass
Enfin, ajoutez le code suivant à la méthode **Draw** de **Game1.cs**, juste après le dernier appel de **spriteBatch.Draw**:

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

Dans MonoGame, les nouveaux appels à **spriteBatch.Draw** dessineront par-dessus tous les appels antérieurs. Cela signifie que les sprites brocoli et dino s'afficheront tous deux par-dessus le sprite herbe existant, cela afin qu’ils ne puissent jamais être masqués par l'herbe, quelle que soit leur position.

Essayez maintenant d’exécuter le jeu et de déplacer le dino avec les touches de direction et la barre d’espace. Si vous avez suivi les étapes ci-dessus, vous devez être en mesure de faire bouger votre avatar dans la fenêtre de jeu et le brocoli doit se déplacer de plus en plus vite.

![Obstacle et avatar du joueur](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>Rendu de texte avec SpriteFont
En utilisant le code ci-dessus, nous suivons le score du joueur en arrière-plan, mais nous ne l'indiquons en réalité pas au joueur. Nous avons également une introduction relativement peu intuitive au démarrage de l’application: le joueur voit une fenêtre bleue et verte, mais n’a aucun moyen de savoir qu'il doit appuyer sur espace pour démarrer le jeu.

Pour résoudre ces deux problèmes, nous allons utiliser un nouveau type d’objet MonoGame appelé **SpriteFonts**.

### <a name="1-create-spritefont-description-files"></a>1. Création de fichiers de description SpriteFont
Dans l'**Explorateur de solutions**, trouvez le dossier **Contenu**. Dans ce dossier, cliquez avec le bouton droit sur le fichier **Content.mgcb** et sélectionnez **Ouvrir avec**. Dans le menu contextuel, sélectionnez **Pipeline MonoGame**, puis appuyez sur **OK**. Dans la nouvelle fenêtre, cliquez avec le bouton droit sur l’élément **Contenu** et sélectionnez **Ajouter -> Nouvel élément**. Sélectionnez **Description SpriteFont**, nommez-le «Score» et appuyez sur **OK**. Ajoutez ensuite une autre description SpriteFont nommée «GameState» à l’aide de la même procédure.

### <a name="2-edit-descriptions"></a>2. Modification des descriptions
Cliquez avec le bouton droit sur le dossier **Contenu** dans le **Pipeline MonoGame** et sélectionnez **Ouvrir l’emplacement du fichier**. Vous devriez voir un dossier contenant les fichiers de description SpriteFont que vous venez de créer, ainsi que toutes les images que vous avez ajoutées jusqu'ici au dossier Contenu. Vous pouvez maintenant fermer et enregistrer la fenêtre Pipeline MonoGame. Depuis l'**Explorateur de fichiers**, ouvrez les deux fichiers de description dans un éditeur de texte (Visual Studio, NotePad ++, Atom, etc.).

Chaque description contient un certain nombre de valeurs qui décrivent le SpriteFont. Nous allons apporter quelques modifications:

Dans **Score.spritefont**, modifiez la valeur **<Size>** de12 à36.

Dans **GameState.spritefont**, modifiez la valeur **<Size>** de12 à72 et la valeur **<FontName>** d'Arial à Agency. Agency est une autre police livrée en standard avec les ordinateurs Windows10. Elle ajoute une certaine élégance à notre écran de présentation.

### <a name="3-load-spritefonts"></a>3. Chargement de SpriteFonts
De retour dans Visual Studio, nous allons tout d’abord ajouter une nouvelle texture pour l’écran de présentation de Microsoft au démarrage. [Cliquez ici pour télécharger l’image](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png).

Comme précédemment, ajoutez la texture au projet en cliquant avec le bouton droit sur le Contenu et en sélectionnant **Ajouter -> Élément existant**. Nommez le nouvel élément «start-splash.png».

Ensuite, ajoutez les variables de classe suivantes à **Game1.cs**:

```CSharp
Texture2D startGameSplash;
SpriteFont scoreFont;
SpriteFont stateFont;
```

Ajoutez ensuite ces lignes à la méthode **LoadContent**:

```CSharp
startGameSplash = Content.Load<Texture2D>("start-splash");
scoreFont = Content.Load<SpriteFont>("Score");
stateFont = Content.Load<SpriteFont>("GameState");
```

### <a name="4-draw-the-score"></a>4. Dessin du score
Accédez à la méthode **Draw** de **Game1.cs** et ajoutez le code suivant juste avant **spriteBatch.End();**

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

Le code ci-dessus utilise la description de sprite que nous avons créée (Arial taille36) pour tirer le score actuel du joueur vers le coin supérieur droit de l’écran.

### <a name="5-draw-horizontally-centered-text"></a>5. Dessin du texte centré horizontalement
Lorsque vous créez un jeu, vous souhaitez souvent dessiner un texte centré, soit horizontalement, soit verticalement. Pour centrer horizontalement le texte d’introduction, ajoutez ce code à la méthode **Draw** juste avant **spriteBatch.End();**

```CSharp
if (!gameStarted)
{
  // Fill the screen with black before the game starts
  spriteBatch.Draw(startGameSplash, new Rectangle(0, 0,
  (int)screenWidth, (int)screenHeight), Color.White);

  String title = "VEGGIE JUMP";
  String pressSpace = "Press Space to start";

  // Measure the size of text in the given font
  Vector2 titleSize = stateFont.MeasureString(title);
  Vector2 pressSpaceSize = stateFont.MeasureString(pressSpace);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, title,
  new Vector2(screenWidth / 2 - titleSize.X / 2, screenHeight / 3),
  Color.ForestGreen);
  spriteBatch.DrawString(stateFont, pressSpace,
  new Vector2(screenWidth / 2 - pressSpaceSize.X / 2,
  screenHeight / 2), Color.White);
  }
```

Tout d’abord, nous créons deux chaînes, une pour chaque ligne de texte à dessiner. Ensuite, nous mesurons la largeur et la hauteur de chaque ligne une fois imprimée, à l’aide de la méthode **SpriteFont.MeasureString(String)**. Cela nous donne la taille en tant qu'objet **Vector2**, avec la propriété **X** contenant sa largeur, et **Y** sa hauteur.

Enfin, nous traçons chaque ligne. Pour centrer le texte horizontalement, nous rendons la valeur **X** de son vecteur de position égale à **screenWidth / 2 - textSize.X / 2**

**Défi:** comment modifier la procédure ci-dessus afin de centrer le texte verticalement et horizontalement?

Essayez d’exécuter le jeu! Voyez-vous l’écran de présentation au démarrage? Le score augmente-t-il chaque fois que le brocoli se régénère?

![Écran de présentation](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>Détection des collisions
Nous avons un brocoli qui vous suit partout, ainsi qu'un score qui augmente chaque fois qu’un nouveau brocoli est créé, mais pour l'instant, il n’existe aucun moyen de perdre réellement à ce jeu. Il nous faut un moyen de savoir si les sprites dino et brocolis entrent en collision et, quand c'est le cas, de déclarer que le jeu est fini.

### <a name="1-rectangular-collision"></a>1. Collision rectangulaire
Lorsque des collisions sont détectées dans un jeu, les objets sont souvent simplifiés pour réduire la complexité des calculs nécessaires. Nous allons traiter à la fois l'avatar du joueur et l'obstacle brocoli comme des rectangles afin de détecter leurs collisions.

Ouvrez **SpriteClass.cs** et ajoutez une nouvelle variable de classe:

```CSharp
const float HITBOXSCALE = .5f;
```

Cette valeur représente le degré de «tolérance» de la détection des collisions pour le joueur. Avec la valeur .5f, les bords du rectangle dans lequel le dino peut entrer en collision avec les brocolis (souvent appelée la «hitbox) auront la moitié de la taille totale de la texture. Il n'y aura donc que peu de cas dans lesquels les coins des deux textures entreront en collision, sans qu'aucune partie des images aient l'air de se toucher réellement. N’hésitez pas à modifier cette valeur à votre convenance.

Ajoutez ensuite une nouvelle méthode pour **SpriteClass.cs**:

```CSharp
public bool RectangleCollision(SpriteClass otherSprite)
{
  if (this.x + this.texture.Width * this.scale * HITBOXSCALE / 2 < otherSprite.x - otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y + this.texture.Height * this.scale * HITBOXSCALE / 2 < otherSprite.y - otherSprite.texture.Height * otherSprite.scale / 2) return false;
  if (this.x - this.texture.Width * this.scale * HITBOXSCALE / 2 > otherSprite.x + otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y - this.texture.Height * this.scale * HITBOXSCALE / 2 > otherSprite.y + otherSprite.texture.Height * otherSprite.scale / 2) return false;
  return true;
}
```

Cette méthode détecte si les deux objets rectangulaires se sont heurtés. L’algorithme cherche à voir s’il existe un espace entre deux des côtés des rectangles. Si c'est le cas, c'est qu'il n'y a aucune collision. Inversement, s'il n'y a aucun espace, c'est qu'une collision s'est produite.

### <a name="2-load-new-textures"></a>2. Chargement de nouvelles textures

Ouvrez ensuite **Game1.cs** et ajoutez deux nouvelles variables de classe, l'une pour stocker la texture du sprite signifiant la fin du jeu et une valeur booléenne pour suivre l’état du jeu:

```CSharp
Texture2D gameOverTexture;
bool gameOver;
```

Initialisez ensuite **gameOver** dans la méthode **initialiser**:

```CSharp
gameOver = false;
```

Enfin, chargez la texture dans **gameOverTexture** dans la méthode **LoadContent**:

```CSharp
gameOverTexture = Content.Load<Texture2D>("game-over");
```

### <a name="3-implement-game-over-logic"></a>3. Implémentation de la logique «jeu terminé»
Ajoutez ce code à la méthode **Update**, juste après l'appel de la méthode **KeyboardHandler**:

```CSharp
if (gameOver)
{
  dino.dX = 0;
  dino.dY = 0;
  broccoli.dX = 0;
  broccoli.dY = 0;
  broccoli.dA = 0;
}
```

Cela stoppera toutes les animations lorsque le jeu sera terminé, figeant les sprites dino et brocoli sur place.

Ensuite, à la fin de la méthode **Update**, juste avant **base. Update(gameTime)**, ajoutez la ligne suivante:

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

Cela appelle la méthode **RectangleCollision** que nous avons créée dans **SpriteClass**et marque le jeu comme étant terminé si elle retourne true.

### <a name="4-add-user-input-for-resetting-the-game"></a>4. Ajout de l’entrée utilisateur permettant de réinitialiser le jeu
Ajoutez ce code à la méthode **KeyboardHandler** pour permettre à l’utilisateur de réinitialiser le jeu en appuyant sur ENTRÉE:

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="5-draw-game-over-splash-and-text"></a>5. Dessin de l’écran et du texte de fin de jeu
Pour finir, ajoutez ce code à la méthode Draw, juste après le premier appel de **spriteBatch.Draw** (il doit s’agir de l’appel qui dessine la texture herbe).

```CSharp
if (gameOver)
{
  // Draw game over texture
  spriteBatch.Draw(gameOverTexture, new Vector2(screenWidth / 2 - gameOverTexture.Width / 2, screenHeight / 4 - gameOverTexture.Width / 2), Color.White);

  String pressEnter = "Press Enter to restart!";

  // Measure the size of text in the given font
  Vector2 pressEnterSize = stateFont.MeasureString(pressEnter);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, pressEnter, new Vector2(screenWidth / 2 - pressEnterSize.X / 2, screenHeight - 200), Color.White);
}
```

Nous utilisons ici la même méthode que celle nous avons employée pour tracer le texte centré à l’horizontale (en réutilisant la police que nous avons utilisée pour la présentation de démarrage) et pour centrer **gameOverTexture** dans la partie supérieure de la fenêtre.

Et c'est fini! Essayez de réexécuter le jeu. Si vous avez suivi les étapes ci-dessus, le jeu doit maintenant se terminer lorsque le dino entre en collision avec le brocoli et le lecteur doit être invité à redémarrer le jeu en appuyant sur la touche ENTRÉE.

![Fin du jeu](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>Publier dans le Microsoft Store
Étant donné que nous avons créé ce jeu comme une application UWP, il est possible de publier le projet dans le Microsoft Store. La procédure comprend quelques étapes.

Vous devez être [enregistré](https://developer.microsoft.com/en-us/store/register) en tant que développeur Windows.

Vous devez utiliser la [liste de vérification de soumission d’applications](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions).

L’application doit être soumise pour [certification](https://docs.microsoft.com/en-us/windows/uwp/publish/the-app-certification-process).

Pour plus d’informations, consultez [la publication de votre application UWP](https://developer.microsoft.com/en-us/store/publish-apps).
