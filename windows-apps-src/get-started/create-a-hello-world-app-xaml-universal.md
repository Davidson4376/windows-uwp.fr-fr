---
author: GrantMeStrength
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: "Créer une application «Hello World» (XAML)"
description: "Ce didacticiel vous apprend à utiliser le langage XAML (Extensible Application Markup Language) avec C# pour créer une application «Hello World» simple ciblant la plateforme Windows universelle (UWP) sur Windows10."
translationtype: Human Translation
ms.sourcegitcommit: 344ffda398c789f82973b5f08a0e3b791fc5ad10
ms.openlocfilehash: 6cf960781862649588f361b6bfcd87605f3e8d55

---

# Créer une application «Hello World» (XAML)

Ce didacticiel vous explique comment utiliser XAML et C# pour créer une simple application «Hello World» pour la plateforme Windows universelle (UWP) sur Windows10. Dans Microsoft Visual Studio, un seul projet vous permet de générer une application qui s’exécute sur n’importe quel appareil Windows10.

Vous allez apprendre à effectuer les opérations suivantes:

-   créer un projet **Visual Studio2015** qui cible **Windows10** et la **plateforme Windows universelle (UWP)**;
-   écrire du code XAML pour modifier l’interface utilisateur de votre page de démarrage;
-   exécuter le projet sur l’ordinateur local et sur l’émulateur de téléphone dans Visual Studio;
-   utilisez un objet SpeechSynthesizer pour faire parler l’application quand vous appuyez sur un bouton.

## Avant de commencer...

-   [Qu’est-ce qu’une application Windows universelle?](whats-a-uwp.md)
-   [Nouveautés de Windows10](https://dev.windows.com/whats-new-windows-10-dev-preview)
-   Pour suivre ce didacticiel, vous avez besoin de Windows10 et de Visual Studio2015. [Se préparer](get-set-up.md).
-   Nous partons également du principe que vous utilisez la disposition de fenêtre par défaut de Visual Studio. Si vous modifiez la disposition par défaut, vous pouvez la réinitialiser dans le menu **Fenêtre** en choisissant la commande **Rétablir la disposition de fenêtre**.


## Si vous préférez visionner une vidéo...

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Writing-Your-First-Windows-10-App/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

Si vous préférez une approche visuelle plutôt qu’un guide pas à pas, cette vidéo aborde les mêmes sujets avec en prime une bande son séduisante.

## Étape1: Créer un projet dans Visual Studio

1.  Lancez Visual Studio2015.

2.  Dans le menu **Fichier**, sélectionnez **Nouveau &gt; Projet...** pour ouvrir la boîte de dialogue *Nouveau projet*.

3.  Dans la liste de modèles de gauche, ouvrez **Installé &gt; Modèles &gt; Visual C# &gt; Windows**, puis choisissez **Universel** pour afficher la liste des modèles de projet UWP.

    (Si aucun modèle universel n’apparaît, c’est que vous n’avez pas Visual Studio2015 ou qu’il vous manque les composants permettant de créer des applications UWP. Voir [Préparation](get-set-up.md) pour réparer vos outils.)

4.  Choisissez le modèle **Application vide (Windows universel)**, puis entrez «HelloWorld» comme **Nom**. Sélectionnez **OK**.

    ![Fenêtre Nouveau projet](images/win10-cs-01.png)

5.  La boîte de dialogue Version cible/Version minimale s’affiche. Les paramètres par défaut étant corrects, sélectionnez **OK** pour créer le projet.

    ![Fenêtre Explorateur de solutions](images/win10-cs-02.png)

6.  Votre nouveau projet s’ouvre en affichant ses fichiers dans le volet **Explorateur de solutions**, à droite. Vous devrez peut-être choisir l’onglet **Explorateur de solutions** à la place de l’onglet **Propriétés** pour voir vos fichiers.

    ![Fenêtre Explorateur de solutions](images/win10-cs-03.png)

Même si le modèle **Application vide (Windows universel)** est dépouillé, il contient cependant de nombreux fichiers. Ces fichiers sont indispensables pour toutes les applications UWP en C#. Ils se trouvent dans tous les projets que vous créez dans Visual Studio.


### Que contiennent les fichiers?

Pour afficher et modifier un fichier de votre projet, double-cliquez dessus dans l’**Explorateur de solutions**. Développez un fichier XAML à la manière d’un dossier pour afficher le fichier de code qui lui est associé. Les fichiers XAML s’ouvrent en mode Fractionné avec l’aire de conception et l’éditeur XAML tous deux affichés.
> [!NOTE]
> Qu’est-ce que le XAML? XAML (Extensible Application Markup Language) est le langage utilisé pour définir l’interface utilisateur de votre application. Vous pouvez entrer son code manuellement ou le créer avec les outils de conception VisualStudio. Un fichier .xaml s’accompagne d’un fichier code-behind .xaml.cs qui contient la logique. Ensemble, les fichiers XAML et code-behind forment une classe à part entière. Pour plus d’informations, voir [Vue d’ensemble du langage XAML](https://msdn.microsoft.com/library/windows/apps/Mt185595).

*App.xaml et App.xaml.cs*

-   App.xaml est le fichier dans lequel vous déclarez les ressources utilisées dans l’application.
-   App.xaml.cs est le fichier code-behind d’App.xaml. Comme toutes les pages code-behind, il contient un constructeur qui appelle la méthode `InitializeComponent`. Ce n’est pas vous qui écrivez la méthode `InitializeComponent`. Elle est générée par Visual Studio et vise essentiellement à initialiser les éléments déclarés dans le fichier XAML.
-   App.xaml.cs est le point d’entrée de votre application.
-   App.xaml.cs contient par ailleurs des méthodes destinées à gérer l’activation et la suspension de l’application.

*MainPage.xaml*

-   MainPage.xaml est le fichier dans lequel vous définissez l’interface utilisateur de votre application. Vous pouvez y ajouter directement des éléments en utilisant du balisage XAML ou vous pouvez utiliser les outils de conception fournis avec Visual Studio.
-   MainPage.xaml.cs est la page code-behind de MainPage.xaml. Cette page vous permet d’ajouter la logique de votre application et les gestionnaires d’événements.
-   Ces deux fichiers définissent ensemble une nouvelle classe appelée `MainPage`, qui hérite de l’élément [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503), dans l’espace de noms `HelloWorld`.

*Package.appxmanifest*
-   Fichier manifeste qui décrit votre application: nom, description, vignette, page de démarrage, etc..
-   Comprend la liste des fichiers contenus dans votre application.

*Ensemble d’images de logo*
-   Assets/Square150x150Logo.scale-200.png représente votre application dans le menu Démarrer.
-   Assets/StoreLogo.png représente votre application dans le WindowsStore.
-   Assets/SplashScreen.scale-200.png est l’écran de démarrage qui s’affiche quand votre application démarre.

## Étape2: Ajouter un bouton

### En utilisant le mode concepteur

Ajoutons un bouton à la page. Dans ce didacticiel, vous n’utiliserez qu’une partie des fichiers mentionnés précédemment: App.xaml, MainPage.xaml et MainPage.xaml.cs.

1.  Double-cliquez sur **MainPage.xaml** pour l’ouvrir en mode Création.

    Vous remarquerez dans la partie supérieure de l’écran la présence d’un affichage graphique et en dessous celle d’un affichage de code XAML. Même si les deux peuvent être modifiés, nous n’utiliserons pour le moment que l’affichage graphique.

    ![Fenêtre Explorateur de solutions](images/win10-cs-04.png)

2.  Cliquez sur l’onglet vertical **Boîte à outils** à gauche pour ouvrir la liste des contrôles d’interface utilisateur. (Vous pouvez cliquer sur l’épingle dans sa barre de titre pour qu’elle reste visible.)

    ![Fenêtre Explorateur de solutions](images/win10-cs-05.png)

3.  Développez **Contrôles XAML communs** et faites glisser le contrôle **Button** jusqu’au milieu de l’aire de conception.

    ![Fenêtre Explorateur de solutions](images/win10-cs-06.png)

    Si vous regardez dans la fenêtre de code XAML, vous constaterez que le contrôle Button y a été aussi ajouté:

 ```XAML
<Button x:name="button" Content="Button" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
 ```

4.  Modifiez le texte du bouton.

    Cliquez dans l’affichage de code XAML et modifiez la valeur de Content en remplaçant «Button» par «Hello World!».

```XAML
<Button x:name="button" Content="Hello, world!" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
```

Le bouton figurant dans l’aire de conception est alors mis à jour pour présenter le nouveau texte.

![Fenêtre Explorateur de solutions](images/win10-cs-07.png)

## Étape 3: Démarrer l’application


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

L’écran de démarrage disparaît pour céder la place à votre application. Cette dernière se présente comme suit.

![Écran initial de l’application](images/win10-cs-08.png)

Appuyez sur la touche Windows pour ouvrir le menu **Démarrer**, puis affichez toutes les applications. Notez que le déploiement de l’application entraîne l’ajout local de sa vignette au menu **Démarrer**. Pour exécuter de nouveau l’application à un moment ultérieur (pas en mode débogage), appuyez ou cliquez sur sa vignette dans le menu **Démarrer**.

Félicitations! Vous venez de créer votre première application UWP, même si celle-ci ne propose pas (encore) beaucoup de fonctions.

**Pour arrêter le débogage**

   Cliquez sur le bouton **Arrêter le débogage** (![Bouton Arrêter le débogage](images/stopdebug.png)) dans la barre d’outils.

   –ou–

   Dans le menu **Déboguer**, cliquez sur **Arrêter le débogage**.

   –ou–

   Fermez la fenêtre de l’application.

### Démarrer l’application sur un émulateur d’appareil mobile

Votre application s’exécute sur n’importe quel appareil Windows10. Examinons donc son aspect sur un Windows Phone.

Outre les options de débogage sur un ordinateur de bureau, Visual Studio offre des options de déploiement et de débogage de votre application sur un appareil mobile physique connecté à l’ordinateur ou sur un émulateur d’appareil mobile. Vous pouvez choisir parmi plusieurs émulateurs d’appareil correspondant à différentes configurations de mémoire et d’affichage.

-   **Appareil**
-   **Émulateur <SDK version> WVGA 4pouces 512Mo**
-   **Émulateur <SDK version> WVGA 4pouces 1Go**
-   etc. (Divers émulateurs associés à d’autres configurations)

(Vous ne voyez pas les émulateurs? Consultez [Préparation](get-set-up.md) pour vérifier que vous avez bien installé les outils de développement d’applications Windows universelles.)

**Pour démarrer le débogage sur un émulateur d’appareil mobile**

1.  En guise de bonne pratique, nous vous conseillons de tester votre application sur un appareil équipé d’un petit écran et d’une mémoire limitée. Pour cela, dans le menu de l’appareil cible (![menu Démarrer le débogage](images/startdebug-full.png)), dans la barre d’outils **Standard**, sélectionnez **Emulator 10.0.14393.0 WVGA 4pouces 512 Mo**.

2.  Cliquez sur le bouton **Démarrer le débogage** (![Bouton Démarrer le débogage](images/startdebug-sm.png)) dans la barre d’outils.

   –ou–

   Dans le menu **Déboguer**, cliquez sur **Démarrer le débogage**.

   –ou–

   Appuyez sur F5.

Visual Studio démarre l’émulateur sélectionné, puis déploie et démarre votre application. Cette opération peut prendre un peu de temps au premier démarrage de l’émulateur. Sur l’émulateur d’appareil mobile, l’application se présente comme suit.

![Écran initial de l’application sur un appareil mobile](images/win10-cs-09.png)

Si vous possédez un Windows Phone exécutant Windows10, vous pouvez le connecter à l’ordinateur pour y déployer l’application et l’exécuter directement (vous devez au préalable [activer le mode développeur](enable-your-device-for-development.md)).


## Étape3: Gestionnaires d’événements

Si le terme «gestionnaire d’événements» vous paraît compliqué, il s’agit simplement d’un autre nom pour désigner le code qui est appelé quand un événement se produit (par exemple, quand l’utilisateur clique sur votre bouton).

1.  Arrêtez l’exécution de l’application, si ce n’est déjà fait.

2.  Double-cliquez sur le contrôle de bouton dans l’aire de conception pour que Visual Studio crée un gestionnaire d’événements pour votre bouton.

  Bien entendu, vous pouvez créer l’intégralité du code manuellement. Vous pouvez aussi sélectionner le bouton en cliquant dessus et consulter le volet **Propriétés** en bas à droite. Si vous basculez dans **Événements** (le petit boulon clignotant), vous pouvez ajouter le nom de votre gestionnaire d’événements.

3.  Modifiez le code du gestionnaire d’événements dans *MainPage.xaml.cs*, la page code-behind. C’est là où les choses deviennent intéressantes. Le gestionnaire d’événements par défaut se présente ceci:

```C#
private void button_Click(object sender, RouteEventArgs e)
{

}
```

  Modifions-le de sorte qu’il se présente comme ceci:

```C#
private async void button_Click(object sender, RoutedEventArgs e)
        {
            MediaElement mediaElement = new MediaElement();
            var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
            Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream = await synth.SynthesizeTextToStreamAsync("Hello, World!");
            mediaElement.SetSource(stream, stream.ContentType);
            mediaElement.Play();
        }
```

Veillez aussi à inclure le mot clé **async** car à défaut, vous obtiendrez une erreur en essayant d’exécuter l’application.

### Que venons-nous de faire?

Ce code utilise certaines API Windows pour créer un objet de synthèse vocale et lui donne du texte à prononcer. (Pour plus d’informations sur l’utilisation de SpeechSynthesis, voir la documentation [Espace de noms SpeechSynthesis](https://msdn.microsoft.com/library/windows/apps/windows.media.speechsynthesis.aspx).)

Quand vous exécutez l’application et que vous cliquez sur le bouton, votre ordinateur (ou téléphone) prononce «Hello World!».


## Récapitulatif


Félicitations! Vous venez de créer votre première application pour Windows10 et UWP. Prêt pour [l’étape suivante](learn-more.md)?



<!--HONumber=Nov16_HO1-->


