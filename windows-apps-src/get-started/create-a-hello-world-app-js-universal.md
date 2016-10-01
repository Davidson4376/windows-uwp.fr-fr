---
author: GrantMeStrength
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
title: "Créer une application « Hello World » (JS)"
description: "Ce didacticiel vous expliquer comment utiliser JavaScript et HTML pour créer une simple application &amp;\\#0034;Hello, world&amp;\\#0034; ciblant la plateforme Windows universelle (UWP) sur Windows 10."
translationtype: Human Translation
ms.sourcegitcommit: 2e0965f964f6f2e10b895d99244b66458eb15903
ms.openlocfilehash: 6c81b24f7fa9abe036d4ccd22ee8fa24c011fe77

---
# Créer une application « Hello World » (JS)

Ce didacticiel vous expliquer comment utiliser JavaScript et HTML pour créer une simple application « Hello World » ciblant la plateforme Windows universelle (UWP) sur Windows 10. À l’aide d’un seul projet dans Microsoft Visual Studio, vous pouvez générer une application qui s’exécute sur tout appareil Windows10.

Vous allez apprendre à effectuer les opérations suivantes:

-   créer un projet **Visual Studio2015** qui cible **Windows10** et la **plateforme  Windows universelle (UWP)**;
-   ajouter du contenu HTML à votre page de démarrage;
-   gérer les entrées tactiles, les entrées effectuées à l’aide du stylet et de la souris;
-   exécuter le projet sur l’ordinateur local et sur l’émulateur de téléphone dans Visual Studio;
-   utiliser un contrôle de la bibliothèque Windows pour JavaScript.

## Avant de commencer...

-   [Qu’est-ce qu’une application Windows universelle?](whats-a-uwp.md)
-   [Nouveautés de Windows10](https://dev.windows.com/whats-new-windows-10-dev-preview)
-   Pour suivre ce didacticiel, vous avez besoin de Windows10 et de Visual Studio2015. [Se préparer](get-set-up.md).
-   Nous partons également du principe que vous utilisez la disposition de fenêtre par défaut de Visual Studio. Si vous modifiez la disposition par défaut, vous pouvez la réinitialiser dans le menu **Fenêtre** en choisissant la commande **Rétablir la disposition de fenêtre**.

## Étape1: créer un projet dans Visual Studio

Créons une application intitulée `HelloWorld`. Voici comment procéder:
1.  Lancez Visual Studio2015.

2.  À partir du menu **Fichier**, sélectionnez **Nouveau &gt; Projet...** pour ouvrir la boîte de dialogue *Nouveau projet*.

3.  Dans la liste de modèles de gauche, développez **Installés &gt; Modèles &gt; JavaScript &gt; Windows**, puis choisissez **Universel** pour afficher la liste des modèles de projet UWP. Choisissez **Application WinJS (Windows universel)**.

    ![La fenêtre Nouveau projet ](images/winjs-tut-newproject.png)

    Pour ce didacticiel, nous utilisons le modèle **Application WinJS**. Ce modèle crée une application UWP dépouillée qui peut être compilée et exécutée, mais qui ne contient ni contrôles d’interface utilisateur, ni données. Dans le cadre de ce didacticiel, vous allez ajoutez des contrôles et des données à l’application.

   (Si vous ne voyez pas ces options, assurez-vous que vous avez installé les outils de développement d’applications Windows universelles. Pour plus d’informations, consultez [Se préparer](get-set-up.md).)

4.  Dans la zone de texte **Nom**, tapez «HelloWorld».
5.  Cliquez sur **OK** pour créer le projet.
6.  Vous serez invité à sélectionner une **version cible** et une **version minimale** de Windows pour la prise en charge. Les paramètres par défaut sont corrects, donc cliquez sur **OK**.

    Visual Studio crée votre projet et l’affiche dans **l’Explorateur de solutions**.

    ![Explorateur de solutions Visual Studio pour le projet HelloWorld](images/winjs-tut-helloworld.png)

Même si le modèle **Application WinJS** est dépouillé, il contient quelques fichiers:

-   un fichier manifeste (package.appxmanifest) qui décrit votre application (nom, description, vignette, page de démarrage, écran de démarrage, etc.) et répertorie les fichiers contenus dans votre application;
-   un ensemble d’images de logo (images/Square150x150Logo.scale-200.png, images/Square44x44Logo.scale-200.png, et images/Wide310x150Logo.scale-200.png) à afficher dans le menu Démarrer;
-   une image (images/StoreLogo.png) pour représenter votre application dans le Windows Store;
-   un écran de démarrage (images/SplashScreen.scale-200.png) pour indiquer quand votre application démarre;
-   une page de démarrage (index.html) et un fichier JavaScript associé (main.js) qui s’exécute au démarrage de l’application.

Pour afficher et modifier les fichiers, double-cliquez dessus dans **l’Explorateur de solutions**.

Ces fichiers sont indispensables pour toutes les applications UWP en JavaScript. Ils figurent dans tous les projets que vous créez dans Visual Studio.

## Étape 2 : lancer l’application


À ce stade, vous avez créé une application très simple. Le moment est bien choisi pour générer, déployer et lancer votre application et voir à quoi elle ressemble. Vous pouvez déboguer votre application sur l’ordinateur local, dans un simulateur ou un émulateur, ou sur un appareil distant. Voici le menu des périphériques cibles dans Visual Studio.

![Liste déroulante des périphériques cibles pour le débogage de votre application](images/uap-debug.png)

### Démarrer l’application sur un ordinateur de bureau

Par défaut, l’application s’exécute sur l’ordinateur local. Le menu des périphériques cibles vous offre plusieurs options pour le débogage de votre application sur des périphériques de la famille des ordinateurs de bureau.

-   **Simulateur**
-   **Ordinateur local**
-   **Ordinateur distant**

**Pour démarrer le débogage sur l’ordinateur local**

1.  Dans le menu des appareils cibles (![Menu Démarrer le débogage](images/startdebug-full.png)) figurant sur la barre d’outils **Standard**, assurez-vous que l’option **Ordinateur local** est sélectionnée. (Il s’agit de la sélection par défaut.)
2.  Cliquez sur le bouton **Démarrer le débogage** (![Bouton Démarrer le débogage](images/startdebug-sm.png)) de la barre d’outils.

   –ou–

   Dans le menu **Déboguer**, cliquez sur **Démarrer le débogage**.

   –ou–

   Appuyez sur F5.

L’application s’ouvre dans une fenêtre, et un écran de démarrage par défaut s’affiche en premier. Cet écran est défini par une image (SplashScreen.png) et par une couleur d’arrière-plan (spécifiées dans le fichier manifeste de votre application).

L’écran de démarrage disparaît pour céder la place à votre application. Elle contient un écran noir avec la mention «Le contenu s’affiche ici».

![Application HelloWorld sur un PC](images/helloworld-1-winjs.png)

Appuyez sur la touche Windows pour ouvrir le menu **Démarrer**, puis affichez toutes les applications. Notez que le déploiement de l’application entraîne l’ajout local de sa vignette au menu **Démarrer**. Pour exécuter de nouveau l’application (non en mode débogage), appuyez ou cliquez sur sa vignette dans le menu **Démarrer**.

Félicitations! Vous venez de générer votre première application UWP, même si celle-ci ne propose pas (encore) beaucoup de fonctions.

**Pour arrêter le débogage**

-   Cliquez sur le bouton **Arrêter le débogage** (![Bouton Arrêter le débogage](images/stopdebug.png)) dans la barre d’outils.

   –ou–

   Dans le menu **Déboguer**, cliquez sur **Arrêter le débogage**.

   –ou–

   Fermez la fenêtre de l’application.

### Démarrer l’application sur un émulateur d’appareil mobile

Votre application s’exécute sur n’importe quel appareil Windows 10. Examinons donc son aspect sur un Windows Phone.

Outre les options de débogage sur un ordinateur de bureau, Visual Studio offre des options de déploiement et de débogage de votre application sur un appareil mobile physique connecté à l’ordinateur ou sur un émulateur d’appareil mobile. Vous pouvez choisir parmi plusieurs émulateurs d’appareil correspondant à différentes configurations de mémoire et d’affichage.

-   **Appareil**
-   **Émulateur <SDK version> WVGA 4pouces 512Mo**
-   **Émulateur <SDK version> WVGA 4pouces 1Go**
-   etc. (Divers émulateurs associés à d’autres configurations)

(Si vous ne voyez pas les émulateurs, assurez-vous que vous avez installé les outils de développement d’applications Windows universelles. Pour plus d’informations, consultez [Se préparer](get-set-up.md).)

Il est judicieux de tester votre application sur un appareil doté d’un petit écran et d’une mémoire limitée. Par conséquent, choisissez l’option **Émulateur 10.0.14393.0 WVGA 4pouces 512Mo**.

**Pour démarrer le débogage sur un émulateur d’appareil mobile**

1.  Dans le menu des appareils cibles (![Menu Démarrer le débogage](images/startdebug-full.png)) situé sur la barre d’outils **Standard**, choisissez **Émulateur 10.0.14393.0 WVGA 4 pouces 512Mo**.
2.  Cliquez sur le bouton **Démarrer le débogage** (![Bouton Démarrer le débogage](images/startdebug-sm.png)) dans la barre d’outils.

   –ou–

   Dans le menu **Déboguer**, cliquez sur **Démarrer le débogage**.


Visual Studio démarre l’émulateur sélectionné, puis déploie et démarre votre application. Lors du lancement initial, l’émulateur peut prendre un peu de temps pour démarrer. Si vous rencontrez une erreur concernant HyperV, cliquer sur **Réessayer** devrait résoudre ce problème. Sur l’émulateur d’appareil mobile, l’application se présente comme suit.

![Écran initial de l’application sur un appareil mobile](images/helloworld-1-winjs-phone.png)

## Étape3: modifier votre page de démarrage

**index.html** est l’un des fichiers créés automatiquement par Visual Studio. Il s’agit de la page de démarrage de votre application. Quand l’application s’exécute, elle affiche le contenu de sa page de démarrage. La page de démarrage contient également des références aux fichiers de code et aux feuilles de style de l’application. Voici la page d’accueil automatiquement créée par Visual Studio pour vous:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/main.js"></script>
</head>
<body class="win-type-body">
    <div>Content goes here!</div>
</body>
</html>
```

Ajoutons du contenu au fichier default.html. Vous devez ajouter votre contenu à l’intérieur de l’élément [body](https://msdn.microsoft.com/library/windows/apps/Hh453011), comme vous le feriez pour n’importe quel autre fichier HTML. Vous pouvez utiliser des éléments HTML5 pour créer votre application (à [quelques exceptions](https://msdn.microsoft.com/library/windows/apps/Hh465380) près). En d’autres termes, vous pouvez utiliser des éléments HTML5 comme [h1](https://msdn.microsoft.com/library/windows/apps/Hh441078), [p](https://msdn.microsoft.com/library/windows/apps/Hh453431), [button](https://msdn.microsoft.com/library/windows/apps/Hh453017), [div](https://msdn.microsoft.com/library/windows/apps/Hh453133) et [img](https://msdn.microsoft.com/library/windows/apps/Hh466114).

**Modifier votre page de démarrage**

1.  Remplacez le contenu existant de l’élément **body** par le titre de premier niveau «Hello, world!», du texte qui demande le nom de l’utilisateur, un élément **input** pour accepter le nom de l’utilisateur, un élément **button** et un élément **div**. Assignez des ID aux éléments **input**, **button** et **div**.

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  Exécutez l’application sur l’ordinateur local. Elle se présente comme suit :

![L’application HelloWorld avec le nouveau contenu](images/helloworld-2-winjs.png)

   Vous pouvez écrire dans l’élément **input**, mais rien ne se produit pour l’instant si vous cliquez sur l’élément **button**. Des objets tels que l’élément **button** peuvent envoyer des messages lorsque certains événements se produisent. Ces messages d’événement vous permettent de réagir en réponse à l’événement. Insérez le code de réponse à l’événement dans une méthode de gestionnaire d’événements.

   Dans les étapes suivantes, nous allons créer un gestionnaire d’événements pour l’élément **button** qui affiche une formule de salutation personnalisée. Nous ajouterons notre code de gestionnaire d’événements au fichier main.js.

## Étape4: créer un gestionnaire d’événements

Lorsque nous avons créé notre projet, Visual Studio a créé un fichier /js/main.js. Ce fichier contient du code permettant de gérer le cycle de vie de votre application. C’est également ici que vous écrivez le code supplémentaire assurant l’interactivité du fichier index.html.

Ouvrez le fichier main.js.

Avant d’ajouter notre propre code, examinons les premières et dernières lignes de code du fichier:

```javascript
(function () {
    "use strict";

     // Omitted code

 })();
```

Vous vous demandez peut-être ce à quoi tout ceci correspond. Ces lignes de code encapsulent le reste du code main.js dans une fonction anonyme qui s’exécute automatiquement. Une fonction anonyme s’exécutant automatiquement permet d’éviter plus facilement des conflits de noms ou des situations dans lesquelles vous modifiez accidentellement une valeur. De même, cela maintient les identificateurs inutiles à l’extérieur de l’espace de noms global, ce qui favorise les performances. Même si cela paraît un peu étrange, il s’agit d’une bonne pratique de programmation.

La ligne de code suivante active le [mode strict](https://msdn.microsoft.com/library/windows/apps/br230269.aspx) pour votre code JavaScript. Le mode strict assure une vérification supplémentaire des erreurs au niveau de votre code. Par exemple, il vous empêche d’utiliser des variables déclarées implicitement ou d’assigner une valeur à une propriété en lecture seule.

Examinez le reste du code dans main.js. Il gère les événements [activated](https://msdn.microsoft.com/library/windows/apps/BR212679) et [checkpoint](https://msdn.microsoft.com/library/windows/apps/BR229839) de votre application. Nous nous pencherons plus en détail sur ces événements ultérieurement. Pour l’heure, sachez simplement que l’événement **activated** se déclenche au démarrage de votre application.

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
          if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
          else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
              if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
              }
              else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);
            args.setPromise(WinJS.UI.processAll());
        }

        isFirstActivation = false;
    };
```

Définissons un gestionnaire d’événements pour votre élément [button](https://msdn.microsoft.com/library/windows/apps/Hh453017). Notre nouveau gestionnaire d’événements obtient le nom de l’utilisateur auprès du contrôle [input](https://msdn.microsoft.com/library/windows/apps/Hh453271) `nameInput` et l’utilise pour générer une salutation à destination de l’élément **div** `greetingOutput` que vous avez créé dans la section précédente.

### Utilisation d’événements qui fonctionnent pour les entrées tactiles et effectuées à l’aide de la souris et du stylet

Dans une application UWP, vous n’avez pas besoin de vous inquiéter des différences entre les entrées tactiles, les entrées effectuées à l’aide de la souris et les autres formes d’entrée de pointeur. Vous pouvez simplement utiliser les événements que vous connaissez, comme [click](https://msdn.microsoft.com/library/windows/apps/Hh441312), qui fonctionnent pour toutes les formes d’entrée.

**Conseil** Votre application peut également utiliser les nouveaux événements *MSPointer\** et *MSGesture\**, qui fonctionnent pour les entrées tactiles et les entrées effectuées à l’aide de la souris et du stylet. Ils peuvent également fournir des informations supplémentaires sur l’appareil qui a déclenché l’événement. Pour plus d’informations, voir [Réponse à l’interaction utilisateur](https://msdn.microsoft.com/library/windows/apps/Hh700412) et [Mouvements, manipulations et interactions](https://msdn.microsoft.com/library/windows/apps/Hh761498).

Poursuivons en créant le gestionnaire d’événements.

**Créer le gestionnaire d’événements**

1.  Dans main.js, après le gestionnaire d’événements [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) et avant l’appel à [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705), créez une fonction de gestionnaire d’événements [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) nommée `buttonClickHandler` qui prend un paramètre unique nommé `eventInfo`.
```javascript
    function buttonClickHandler(eventInfo) {

        }
```

2.  À l’intérieur de notre gestionnaire d’événements, récupérez le nom de l’utilisateur à partir du contrôle [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) `nameInput` et servez-vous en pour créer une salutation. Utilisez le contrôle [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) `greetingOutput` pour afficher le résultat.
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString;
        }
 ```

Vous avez ajouté votre gestionnaire d’événements à main.js. Vous devez à présent l’inscrire.

## Étape 5 : inscrire le gestionnaire d’événements au lancement de votre application


Il ne vous reste plus qu’à inscrire le gestionnaire d’événements avec le bouton. Pour inscrire un gestionnaire d’événements, nous vous recommandons d’appeler [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) à partir de notre code. Il est tout indiqué de le faire lors de l’activation de l’application. Heureusement, Visual Studio a généré du code automatiquement dans notre fichier main.js chargé de gérer l’activation de notre application.


À l’intérieur du gestionnaire [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679), le code vérifie le type d’activation qui s’est produit. Il existe différents types d’activation. Par exemple, votre application est activée lorsque l’utilisateur lance votre application et lorsqu’il cherche à ouvrir un fichier associé à votre application. (Pour plus d’informations, consultez [Cycle de vie de l’application](https://msdn.microsoft.com/library/windows/apps/Mt243287).)

Celui qui nous intéresse est l’activation [launch](https://msdn.microsoft.com/library/windows/apps/BR224693). Le *lancement* d’une application intervient chaque fois qu’un utilisateur active l’application alors qu’elle n’est pas en cours d’exécution. L’appel à [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975) se produit systématiquement, que l’application ait été arrêtée antérieurement ou qu’il s’agisse de son tout premier lancement. **WinJS.UI.processAll** est incorporé dans un appel à la méthode [setPromise](https://msdn.microsoft.com/library/windows/apps/JJ215609) chargée de maintenir l’écran de démarrage affiché jusqu’à ce que la page de l’application soit prête.

**Conseil** La fonction **WinJS.UI.processAll** analyse le fichier default.html à la recherche de contrôles WinJS et les initialise. Jusqu’ici, nous n’avons ajouté aucun de ces contrôles, mais il est judicieux de conserver ce code au cas où vous voudriez en ajouter par la suite.

L’inscription de gestionnaires d’événements pour des contrôles autres que les contrôles WinJS se fera, de préférence, juste après l’appel à **WinJS.UI.processAll**.

**Inscrire votre gestionnaire d’événements**

-   Dans le gestionnaire d’événements [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) du fichier main.js, récupérez `helloButton` et utilisez [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) pour inscrire le gestionnaire d’événements pour l’événement [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312). Ajoutez ce code après l’appel à [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975).

```javascript
   app.onactivated = function (args) {
           // Omitted code
           if (isFirstActivation) {
              document.addEventListener("visibilitychange", onVisibilityChanged);
              args.setPromise(WinJS.UI.processAll());

              // Add your code to retrieve the button and register the event handler.
              var helloButton = document.getElementById("helloButton");
              helloButton.addEventListener("click", buttonClickHandler, false);
            }

```    



Exécutez l’application. Lorsque vous entrez votre nom dans la zone de texte et que vous cliquez sur le bouton, l’application affiche alors un message de salutations personnalisé.

**Remarque** Si vous êtes curieux de savoir pourquoi nous utilisons [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) pour inscrire l’événement dans le code au lieu de définir l’événement [onclick](https://msdn.microsoft.com/library/windows/apps/Hh441312) dans le code HTML, consultez [Codage d’applications de base](https://msdn.microsoft.com/library/windows/apps/Hh780660) pour une explication détaillée.

## Étape6: ajouter un contrôle de bibliothèque Windows pour JavaScript


En plus des contrôles HTML standard, votre application peut utiliser tout contrôle de la [bibliothèque Windows pour JavaScript](https://msdn.microsoft.com/library/windows/apps/BR229782), tels que [WinJS.UI.DatePicker](https://msdn.microsoft.com/library/windows/apps/BR211681), [WinJS.UI.FlipView](https://msdn.microsoft.com/library/windows/apps/BR211711), [WinjS.UI.ListView](https://msdn.microsoft.com/library/windows/apps/BR211837) et [WinJS.UI.Rating](https://msdn.microsoft.com/library/windows/apps/BR211895).

À la différence des contrôles HTML, les contrôles WinJS ne possèdent pas d’éléments de balisage dédiés: vous ne pouvez pas créer de contrôle [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) en ajoutant un élément `<rating />`, par exemple. Pour ajouter un contrôle WinJS, créez un élément **div** et utilisez l’attribut [data-win-control](https://msdn.microsoft.com/library/windows/apps/Hh440969) pour spécifier le type de contrôle souhaité. Pour ajouter un contrôle **Rating**, affectez à l’attribut la valeur «WinJS.UI.Rating».

**Ajoutons un contrôle Rating à votre application.**

1.  Dans votre fichier index.html, ajoutez un élément [label](https://msdn.microsoft.com/library/windows/apps/Hh453321) et un contrôle [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) après l’élément **div** `greetingOutput`.

    ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
    </body>
    ```

2.  Exécutez l’application sur l’ordinateur local. Notez le nouveau contrôle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895).

   ![L’application Hello world! avec un contrôle de la bibliothèque Windows pour JavaScript](images/helloworld-4-winjs.png)

> Pour le chargement du contrôle **Rating**, votre page doit appeler [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975). Étant donné que notre application utilise l’un des modèles Visual Studio, votre main.js contient déjà un appel à **WinJS.UI.processAll**, comme décrit plus tôt, et vous n’avez donc pas à ajouter du code.

Pour l’instant, un clic sur le contrôle **Rating** ne fait que modifier l’évaluation, rien de plus. Utilisons un gestionnaire d’événements pour effectuer une action quand l’utilisateur modifie l’évaluation.

## Étape 7 : inscrire un gestionnaire d’événements pour un contrôle de la bibliothèque Windows pour JavaScript


L’inscription d’un gestionnaire d’événements pour un contrôle WinJS diffère quelque peu de l’inscription d’un gestionnaire d’événements pour un contrôle HTML standard. Nous avons mentionné plus tôt que le gestionnaire d’événements **onactivated** appelle la méthode **WinJS.UI.processAll** pour initialiser la bibliothèque WinJS dans votre balisage. L’appel à **WinJS.UI.processAll** est incorporé dans un appel à la méthode **setPromise**.

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

Si **Rating** était un contrôle HTML standard, vous pourriez ajouter votre gestionnaire d’événements après cet appel à **WinJS.UI.processAll**. Mais cela est un peu plus compliqué pour un contrôle WinJS comme **Rating**. Comme **WinJS.UI.processAll** crée le contrôle **Rating**, nous ne pouvons pas ajouter le gestionnaire d’événements à **Rating** tant que **WinJS.UI.processAll** n’a pas fini son traitement.

Si **WinJS.UI.processAll** était une méthode classique, nous pourrions inscrire le gestionnaire d’événements **Rating** juste après l’avoir appelée. La méthode **WinJS.UI.processAll** étant asynchrone, tout code qui la suit peut s’exécuter avant la fin de l’exécution de **WinJS.UI.processAll**. Alors, que faire? Nous utilisons un objet [Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) pour recevoir une notification à la fin de l’exécution de **WinJS.UI.processAll**.

Comme toutes les méthodes WinJS asynchrones, **WinJS.UI.processAll** retourne un objet **Promise**. Un objet **Promise** est une «promesse» qu’un événement va se produire à l’avenir; lorsqu’il se produit, l’objet **Promise** est considéré comme ayant terminé.

Les objets [Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) incluent une méthode [then](https://msdn.microsoft.com/library/windows/apps/BR229728) qui accepte une fonction «completed» comme paramètre. L’objet **Promise** appelle cette fonction lorsqu’il a terminé.

En ajoutant votre code à une fonction «completed» et en la passant à la méthode **then** de l’objet **Promise**, vous pouvez être sûr que votre code est exécuté à la fin de **WinJS.UI.processAll**.

**Générer la valeur d’évaluation que l’utilisateur sélectionne**

1.  Dans votre fichier index.html, créez un élément [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) pour afficher la valeur d’évaluation et affectez-lui **l’id** «ratingOutput».

    ```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
        <div id="ratingOutput"></div>
    </body>
```

2.  Dans le fichier main.js, créez un gestionnaire d’événements pour l’événement [change](https://msdn.microsoft.com/library/windows/apps/BR211891) du contrôle **Rating** nommé `ratingChanged`. Le paramètre [eventInfo](https://msdn.microsoft.com/library/windows/apps/Hh465776) contient une propriété **detail.tentativeRating** qui fournit la nouvelle évaluation de l’utilisateur. Récupérez cette valeur et affichez-la dans l’élément **div** de sortie.

    ```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating;
        }
```

3.  Mettez à jour le code dans le gestionnaire d’événements [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679) qui appelle [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975) en ajoutant un appel à la méthode [then](https://msdn.microsoft.com/library/windows/apps/BR229728) et en lui passant une fonction `completed`. Dans la fonction `completed`, récupérez l’élément `ratingControlDiv` qui héberge le contrôle [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895). Utilisez ensuite la propriété [winControl](https://msdn.microsoft.com/library/windows/apps/Hh770814) pour récupérer le contrôle **Rating** réel. (Cet exemple définit la fonction `completed` en ligne.)

    ```javascript
           args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler.
                    ratingControl.addEventListener("change", ratingChanged, false);

                }));
```

4.  Même s’il est bien d’inscrire des gestionnaires d’événements pour les contrôles HTML après l’appel à [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975), il est également judicieux de les inscrire à l’intérieur de votre fonction `completed`. Pour simplifier, poursuivons en déplaçant toutes les inscriptions de gestionnaires d’événements à l’intérieur du gestionnaire d’événements [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728).

    Voici le gestionnaire d’événements [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) mis à jour:

    ```javascript
    (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
        else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
            if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
            }
            else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);

            args.setPromise(WinJS.UI.processAll().then(function completed() {
                var ratingControlDiv = document.getElementById("ratingControlDiv");
                var ratingControl = ratingControlDiv.winControl;
                ratingControl.addEventListener("change",ratingChanged, false);
            }));

            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);

        }

        isFirstActivation = false;
    };

    ```        

    Exécutez l’application. Lorsque vous sélectionnez une valeur d’évaluation, la valeur numérique est générée sous le contrôle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895).

![L’application Hello World terminée sur un PC](images/helloworld-5-winjs.png)

## Résumé

Félicitations, vous avez créé votre première application pour Windows 10 et l’UWP en JavaScript et HTML.



<!--HONumber=Aug16_HO3-->


