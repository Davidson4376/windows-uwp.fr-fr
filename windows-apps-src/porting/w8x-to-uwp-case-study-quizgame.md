---
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: Cette rubrique présente une étude de cas de portage d’un questionnaire de pair à pair jeu WinRT 8.1 exemple d’application pour une application de plateforme Windows universelle Windows 10 (UWP).
title: 'Étude de cas de portage d’application Windows Runtime8.x vers UWP: exemple d’application d’homologue à homologue QuizGame'
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9ce39e87f3c5c9e11f3e9ddb1424d606356ee3c8
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8466788"
---
# <a name="windows-runtime-8x-to-uwp-case-study-quizgame-sample-app"></a>Windows Runtime 8.x vers l’étude de cas UWP: exemple d’application QuizGame




Cette rubrique présente une étude de cas de portage d’un questionnaire de pair à pair jeu WinRT 8.1 exemple d’application pour une application de plateforme Windows universelle Windows10Universal (UWP).

Une application 8.1 universelle est une application qui génère deux versions de la même application: un package d’application pour Windows8.1 et une autre pour Windows Phone 8.1. La version WinRT 8.1 de l’application QuizGame utilise une disposition de projet d’application Windows universelle, mais adopte une approche différente et génère une application fonctionnellement distincte pour les deux plates-formes. Le package d’application Windows8.1 joue l’hôte pour une session de jeu-questionnaire, alors que le package d’application Windows Phone 8.1 joue le rôle du client de l’hôte. Les deux composantes de la session de jeu-questionnaire communiquent via un réseau homologue à homologue.

Une adaptation personnalisée de ces deux composantes pour un PC et un téléphone (respectivement) semble appropriée. Toutefois, ne serait-il pas préférable de pouvoir exécuter le client et l’hôte sur n’importe quel appareil ? Dans ce cas étude, nous allons porter les deux applications vers Windows 10 dans laquelle elles seront générées dans un package d’application unique que les utilisateurs peuvent installer sur un large éventail d’appareils.

L’application utilise des modèles qui exploitent des affichages et des modèles d’affichage. Grâce à cette séparation nette, le processus de portage de cette application est très direct, comme vous allez le constater.

**Remarque**cet exemple suppose que votre réseau est configuré pour envoyer et recevoir des UDP personnalisés (la plupart des réseaux domestiques le sont, bien que votre réseau d’entreprise ne soient pas) les paquets de multidiffusion de groupe. Cet exemple envoie et reçoit également des paquets TCP.

 

**Remarque**  lorsque vous ouvrez QuizGame10 dans Visual Studio, si vous voyez apparaître le message «Visual Studio mise à jour requise», puis suivez les étapes décrites dans [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

 

## <a name="downloads"></a>Téléchargements

[Téléchargez l’application 8.1 universelle QuizGame](http://go.microsoft.com/fwlink/?linkid=532953). Il s’agit de l’état initial de l’application avant le portage. 

[Téléchargez le QuizGame10 Windows 10 application](http://go.microsoft.com/fwlink/?linkid=532954). Il s’agit de l’état de l’application juste après le portage. 

[Voir la dernière version de cet exemple sur GitHub](https://github.com/Microsoft/Windows-appsample-quizgame).

## <a name="the-winrt-81-solution"></a>Solution WinRT8.1


Voici à quoi ressemble QuizGame, l’application que nous allons porter.

![Application QuizGame hôte s’exécutant sur Windows](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

Application QuizGame hôte s’exécutant sur Windows

 

![Application QuizGame cliente s’exécutant sur WindowsPhone](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

Application QuizGame cliente s’exécutant sur WindowsPhone

## <a name="a-walkthrough-of-quizgame-in-use"></a>Procédure pas à pas de l’application QuizGame en cours d’utilisation

Il s’agit d’un compte-rendu hypothétique de l’application en cours d’utilisation, qui fournit cependant des informations utiles si vous souhaitez tester l’application vous-même sur votre réseau sans fil.

Un jeu-questionnaire amusant est diffusé dans un bar. Un immense écran de télévision est installé ; tous les clients peuvent le voir. L’animateur dispose d’un PC, dont la sortie est affichée sur l’écran de télévision. Sur ce PC s’exécute « l’application hôte ». Toute personne qui souhaite participer à ce questionnaire doit simplement installer « l’application cliente » sur son téléphone ou sur sa tablette Surface.

L’application hôte est en mode d’introduction ; l’écran de télévision indique qu’elle est prête et attend la connexion des applications clientes. Joanna lance l’application cliente sur son appareil mobile. Elle tape son nom dans la zone de texte **Nom du joueur**, puis appuie sur **Rejoindre le jeu**. L’application hôte reconnaît la connexion de Joanna en affichant son nom. Quant à l’application cliente de Joanna, elle indique qu’elle attend le début de la partie. Ensuite, Maxwell effectue la même procédure sur son appareil mobile.

L’animateur clique sur **Démarrer le jeu** et l’application hôte affiche une question, ainsi que les différentes réponses possibles (elle affiche également une liste des joueurs ayant rejoint la partie, en utilisant une police normale grise). Simultanément, les réponses s’affichent sur les boutons des appareils clients connectés. Joanna appuie sur le bouton indiquant la réponse « 1975 ». À ce moment, tous les boutons sont désactivés. Sur l’application hôte, le nom de Joanna s’affiche en vert (et en gras), ce qui indique que sa réponse a bien été reçue. Les réponses de Maxwell s’affichent de la même manière. L’animateur remarque que tous les noms de joueurs sont affichés en vert. Il clique alors sur **Question suivante**.

Le jeu se poursuit. Une question est posée et reçoit une réponse; l’animateur pose la suivante, et ainsi de suite. Une fois la dernière question affichée sur l’application hôte, le bouton indique **Afficher les résultats**, et non plus **Question suivante**. Lorsque l’animateur clique sur **Afficher les résultats**, les résultats apparaissent. Un clic sur **Revenir à la page d’introduction** vous ramène au début du cycle du jeu, sauf que les joueurs restent connectés. Toutefois, le retour à la page d’introduction permet à de nouveaux joueurs de participer et aux joueurs déjà connectés, de quitter la partie (même s’ils peuvent la quitter à tout moment en appuyant sur **Quitter la partie**).

## <a name="local-test-mode"></a>Mode test local

Pour tester l’application et ses interactions sur un seul PC, et non sur des appareils distribués, vous pouvez générer l’application hôte en mode test local. Ce mode ne tient pas compte de l’utilisation du réseau. Au lieu de cela, l’interface utilisateur de l’application hôte affiche la partie hôte à gauche de la fenêtre et, à droite, deux copies de l’interface utilisateur d’application cliente empilées verticalement (dans cette version, l’interface utilisateur de mode test local est fixe pour un affichage PC ; il ne s’adapte pas aux appareils de petite taille). Dans la même application, ces segments de l’interface utilisateur communiquent entre eux par le biais d’une fonction Communicator de client fictive, qui simule des interactions survenant sur le réseau.

Pour activer le mode test local, définissez l’élément **LOCALTESTMODEON** (dans les propriétés du projet) en tant que symbole de compilation conditionnelle, puis relancez la génération.

## <a name="porting-to-a-windows10-project"></a>Portage d’applications vers un projet Windows 10

L’application QuizGame comporte les éléments suivants :

-   P2PHelper. Il s’agit d’une bibliothèque de classes portable, qui contient la logique du réseau homologue à homologue.
-   QuizGame.Windows. Il s’agit du projet qui crée le package d’application pour l’application hôte, qui cible Windows8.1.
-   QuizGame.WindowsPhone. Il s’agit du projet qui génère le package d’application pour l’application client, qui cible Windows Phone 8.1.
-   QuizGame.Shared. Il s’agit du projet qui contient le code source, les fichiers de balisage et d’autres actifs et ressources qui sont utilisés par les deux autres projets.

Pour cette étude de cas, nous disposons des options habituelles décrites dans la section [Si vous disposez d’une application 8.1 universelle](w8x-to-uwp-root.md), relative aux appareils à prendre en charge.

En fonction de ces options, nous allons porter l’élément QuizGame.Windows vers un nouveau projet Windows 10, appelé QuizGameHost. Et, nous allons porter l’élément QuizGame.WindowsPhone vers un nouveau projet Windows 10, appelé QuizGameClient. Ces projets ciblent la famille d’appareils universels ; ainsi, ils peuvent s’exécuter sur n’importe quel appareil. Nous allons laisser les fichiers sources de l’élément QuizGame.Shared, entre autres, dans leur dossier, et lier les fichiers partagés dans les deux nouveaux projets. Comme auparavant, nous allons conserver tous les éléments en une seule solution, que nous appellerons QuizGame10.

**Solution QuizGame10**

-   Créez une solution (**Nouveau projet** &gt; **Autres types de projets** &gt; **Solutions Visual Studio**), puis appelez-la «QuizGame10».

**P2PHelper**

-   Dans la solution, créez un nouveau projet de bibliothèque de classes Windows 10 (**Nouveau projet** &gt; **Windows universel** &gt; **Bibliothèque de classes (Windows universel)**) et nommez-le P2PHelper.
-   Dans le nouveau projet, supprimez le fichier Class1.cs.
-   Copiez les fichiers P2PSession.cs, P2PSessionClient.cs et P2PSessionHost.cs dans le dossier du nouveau projet, puis insérez les fichiers copiés dans le nouveau projet.
-   Le projet est généré, aucune autre modification n’était nécessaire.

**Fichiers partagés**

-   Copiez les dossiers Common, Model, View et ViewModel à partir de \\QuizGame.Shared\\ vers \\QuizGame10\\.
-   Lorsque nous évoquons les dossiers partagés sur le disque, c’est à ces dossiers (Common, Model, View et ViewModel) que nous faisons allusion.

**QuizGameHost**

-   Créez un projet d’application Windows 10 (**Ajouter** &gt; **Nouveau projet** &gt; **Windows universel** &gt; **Application vide (Windows universel)**) et appelez-le «quizgamehost».
-   Ajoutez une référence à l’élément P2PHelper (**Ajouter une référence** &gt; **Projets** &gt; **Solution** &gt; **P2PHelper**).
-   Dans l’**Explorateur de solutions**, créez un dossier pour chacun des dossiers partagés sur le disque. Ensuite, cliquez avec le bouton droit sur chaque dossier que vous venez de créer et sélectionnez **Ajouter** &gt; **Élément existant**, puis passez au dossier au-dessus. Ouvrez le dossier partagé approprié, sélectionnez tous les fichiers, puis cliquez sur **Ajouter en tant que lien**.
-   Copiez le fichier MainPage.xaml de l’emplacement \\QuizGame.Windows\\ vers \\QuizGameHost\\ et remplacez l’espace de noms par QuizGameHost.
-   Copiez le fichier App.xaml de l’emplacement \\QuizGame.Shared\\ vers \\QuizGameHost\\ et remplacez l’espace de noms par QuizGameHost.
-   Au lieu de remplacer le fichier app.xaml.cs, nous allons conserver sa version dans le nouveau projet en lui apportant une seule modification ciblée afin d’assurer la prise en charge du mode test local. Dans le fichier app.xaml.cs, remplacez cette ligne de code :

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

par:

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   Dans **Propriétés** &gt; **Générer** &gt; **Symboles de compilation conditionnelle**, ajoutez l’élément LOCALTESTMODEON.
-   Vous pourrez maintenant revenir au code que vous avez ajouté dans le fichier app.xaml.cs et résoudre le type TestView.
-   Dans le fichier package.appxmanifest, remplacez le nom de la fonctionnalité « internetClient » par « internetClientServer ».

**QuizGameClient**

-   Créez un projet d’application Windows 10 (**Ajouter** &gt; **Nouveau projet** &gt; **Windows universel** &gt; **Application vide (Windows universel)**) et appelez-le «quizgameclient».
-   Ajoutez une référence à l’élément P2PHelper (**Ajouter une référence** &gt; **Projets** &gt; **Solution** &gt; **P2PHelper**).
-   Dans l’**Explorateur de solutions**, créez un dossier pour chacun des dossiers partagés sur le disque. Ensuite, cliquez avec le bouton droit sur chaque dossier que vous venez de créer et sélectionnez **Ajouter** &gt; **Élément existant**, puis passez au dossier au-dessus. Ouvrez le dossier partagé approprié, sélectionnez tous les fichiers, puis cliquez sur **Ajouter en tant que lien**.
-   Copiez le fichier MainPage.xaml de l’emplacement \\QuizGame.WindowsPhone\\ vers \\QuizGameClient\\ et remplacez l’espace de noms par QuizGameClient.
-   Copiez le fichier App.xaml de l’emplacement \\QuizGame.Shared\\ vers \\QuizGameClient\\ et remplacez l’espace de noms par QuizGameClient.
-   Dans le fichier package.appxmanifest, remplacez le nom de la fonctionnalité «internetClient» par «internetClientServer».

Vous pourrez maintenant générer l’application et l’exécuter.

## <a name="adaptive-ui"></a>Interface utilisateur adaptative

L’application QuizGameHost Windows10 fonctionne correctement lors de l’application s’exécute dans une fenêtre large (opération uniquement possible sur un appareil doté d’un grand écran). Par contre, lorsque la fenêtre d’application est étroite (comme sur un appareil de petite taille, voire sur certains appareils plus grands), l’interface utilisateur est tellement écrasée qu’elle en devient illisible.

Nous pouvons utiliser la fonction adaptative de gestionnaire d’état visuel pour remédier au problème, comme nous l’avons expliqué dans la section [Étude de cas : Bookstore2](w8x-to-uwp-case-study-bookstore2.md). Tout d’abord, définissez les propriétés sur les éléments visuels afin que, par défaut, l’interface utilisateur soit affichée selon une disposition étroite. Toutes ces modifications sont effectuées dans le fichier \\View\\HostView.xaml.

-   Dans l’élément **Grid** principal, modifiez le paramètre **Height** du premier **RowDefinition** en remplaçant « 140 » par « Auto ».
-   Sur l’élément **Grid** qui contient le **TextBlock** nommé `pageTitle`, définissez `x:Name="pageTitleGrid"` et `Height="60"`. Ces deux premières étapes sont organisées de telle sorte que nous puissions contrôler efficacement la hauteur de ce paramètre **RowDefinition** par le biais d’une méthode setter dans un état visuel.
-   Sur `pageTitle`, définissez `Margin="-30,0,0,0"`.
-   Sur l’élément **Grid** signalé par le commentaire `<!-- Content -->`, définissez `x:Name="contentGrid"` et `Margin="-18,12,0,0"`.
-   Sur l’élément **TextBlock** situé juste au-dessus du commentaire `<!-- Options -->`, définissez `Margin="0,0,0,24"`.
-   Dans le style **TextBlock** par défaut (première ressource du fichier), remplacez la valeur de la méthode setter **FontSize** par « 15 ».
-   Dans `OptionContentControlStyle`, remplacez la valeur de la méthode setter **FontSize** par « 20 ». Cette étape et l’étape précédente nous permettent d’obtenir une rampe d’un type correct, qui fonctionnera efficacement sur tous les appareils. Il s’agit des tailles beaucoup plus souples que nous utilisions pour l’application Windows8.1 «30».
-   Enfin, ajoutez le balisage du Gestionnaire d’état visuel approprié à l’élément **Grid** racine.

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## <a name="universal-styling"></a>Stylisation universelle


Vous remarquerez que, dans Windows 10, les boutons ne présentent le même remplissage cible tactile dans leur modèle. Deux petites modifications devraient résoudre le problème. Tout d’abord, ajoutez ce balisage dans le fichier app.xaml des projets QuizGameHost et QuizGameClient.

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

Ensuite, ajoutez cette méthode setter à l’élément `OptionButtonStyle` dans le fichier \\View\\ClientView.xaml.

```xml
<Setter Property="Margin" Value="6"/>
```

Grâce à ce dernier ajustement, l’application se comportera comme auparavant et aura le même aspect qu’avant le portage, à une exception près: elle pourra s’exécuter sur tous les types d’appareils.

## <a name="conclusion"></a>Conclusion

L’application que nous avons portée dans le cadre de cette étude de cas était relativement complexe, car elle impliquait plusieurs projets, une bibliothèque de classes, une interface utilisateur assez volumineuse et une grande quantité de code. Pourtant, son portage s’est révélé très simple. La simplicité de portage est directement attribuables à la similarité entre la plate-forme de développement Windows 10 et les plates-formes Windows8.1 et Windows Phone 8.1. Le mode de conception de l’application d’origine, qui séparait les modèles, les modèles d’affichage et les affichages, contribue également à simplifier cette opération.
