---
author: mcleanbyron
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: "Apprenez à utiliser la classe AdScheduler pour ajouter des annonces à du contenu vidéo."
title: "Ajouter des annonces à du contenu vidéo en HTML 5 et JavaScript"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: 5573d37068a6b6c10ff9e380bb8b34bb83516c35

---

# <a name="add-advertisements-to-video-content-in-html-5-and-javascript"></a>Ajouter des annonces à du contenu vidéo en HTML 5 et JavaScript


Cette procédure pas à pas montre comment utiliser la classe [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) pour ajouter des publicités à du contenu vidéo dans une application de plateforme Windows universelle (UWP) écrite en JavaScript avec HTML.

>**Remarque**&nbsp;&nbsp;Cette fonctionnalité n’est actuellement prise en charge que pour les applications UWP écrites en JavaScript avec HTML.

[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) fonctionne avec les médias à diffusion progressive ou continue et utilise les formats de charge utile standard de l’IAB : VAST (Video Ad Serving Template) 2.0/3.0 et VMAP. L’utilisation des normes rend [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) indépendant du service de publicité avec lequel il interagit.

La publicité pour contenu vidéo varie selon que le programme dure moins de dix minutes (format court) ou plus de dix minutes (format long). Bien que ce dernier soit plus difficile à mettre en place au niveau du service, la façon d’écrire le code côté client ne présente en fait aucune différence. Si [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) reçoit une charge utile VAST avec une seule publicité au lieu d’un manifeste, elle est traitée comme si le manifeste appelait une publicité précédant la vidéo (s’arrêtant à 00:00).

## <a name="prerequisites"></a>Conditions préalables

* Installez [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) avec Visual Studio 2015.

* Votre projet doit utiliser le contrôle [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) pour afficher le contenu vidéo dans lequel les publicités doivent apparaître. Ce contrôle est disponible dans la collection [TVHelpers](https://github.com/Microsoft/TVHelpers) des bibliothèques mises à la disposition sur GitHub par Microsoft.

  L’exemple suivant montre comment déclarer un [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) dans le balisage HTML. En règle générale, ce code appartient à la section `<body>` du fichier index.html (ou un autre fichier html, selon votre projet).

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  L’exemple suivant montre comment établir un [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) dans du code JavaScript.

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Comment utiliser la classe AdScheduler dans votre code

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence à la bibliothèque du **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** à votre projet.

  a. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence…**

  b. Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version 10.0).

  c. Dans **Gestionnaire de références**, cliquez sur OK.

4.  Ajoutez le fichier AdScheduler.js à votre projet :

  a.  Dans Visual Studio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.

  b.  Dans la zone de recherche, tapez **Microsoft.StoreServices.VideoAdScheduler**, puis installez le package Microsoft.StoreServices.VideoAdScheduler. Le fichier AdScheduler.js est ajouté au sous-répertoire .. /js de votre projet.

5.  Ouvrez le fichier index.html (ou un autre fichier html en fonction de votre projet). Dans la section `<head>`, après les références JavaScript des fichiers default.css et main.js du projet, ajoutez la référence à ad.js et adscheduler.js.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  <script src="/js/adscheduler.js"></script>
  ```

  <span/>
  > **Remarque**&nbsp;&nbsp;Vous devez placer cette ligne dans la section `<head>` après l’inclusion de main.js, faute de quoi vous rencontrerez une erreur pendant la génération de votre projet.

6.  Dans le fichier main.js de votre projet, ajoutez le code qui crée un objet [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx). Transmettez le **MediaPlayer** qui héberge votre contenu vidéo. Le code doit être placé de telle sorte qu’il s’exécute après [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/hh440975.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Utilisez les méthodes [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) ou [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) pour demander une planification publicitaire auprès du serveur, l’insérer dans la chronologie **MediaPlayer**, puis lire le média vidéo.

  * Si vous êtes partenaire Microsoft et que vous avez reçu l’autorisation de demander une planification publicitaire auprès du serveur d’annonces Microsoft, utilisez [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) et spécifiez l’ID d’application et l’ID d’unité publicitaire qui vous ont été fournis par votre représentant Microsoft. Cette méthode prend la forme d’un **promesse**, qui est une construction asynchrone où des pointeurs à deux fonctions sont transmis pour gérer respectivement les cas de réussite et d’échec. Pour plus d’informations, consultez [Modèles asynchrones dans UWP en utilisant JavaScript](https://msdn.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript).

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

  * Pour demander une planification publicitaire auprès d’un serveur d’annonces non Microsoft, utilisez [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) et transmettez l’URL du serveur. Cette méthode prend aussi la forme d’une **promesse**.

      > [!div class="tabbedCodeSnippets"]
      [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    <span/>
    >**Remarque**&nbsp;&nbsp;Vous devez appeler **play** même si la fonction échoue, car [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) demande à **MediaPlayer** d’ignorer la ou les annonces et de passer directement au contenu. Vous pouvez avoir une exigence différente, telle que l’insertion d’une publicité intégrée dans le cas où une annonce ne peut pas être récupérée à distance.

8.  Pendant la lecture, vous pouvez gérer d’autres événements qui permettent à votre application de suivre la progression et/ou les erreurs susceptibles de se produire après le processus de sélection d’annonce initial. Le code suivant contient certains de ces événements, dont [onPodStart](https://msdn.microsoft.com/library/windows/apps/mt732206.aspx), [onPodEnd](https://msdn.microsoft.com/library/windows/apps/mt732205.aspx), [onPodCountdown](https://msdn.microsoft.com/library/windows/apps/mt732204.aspx), [onAdProgress](https://msdn.microsoft.com/library/windows/apps/mt732201.aspx), [onAllComplete](https://msdn.microsoft.com/library/windows/apps/mt732202.aspx) et [onErrorOccurred](https://msdn.microsoft.com/library/windows/apps/mt732203.aspx).

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]



<!--HONumber=Dec16_HO2-->


