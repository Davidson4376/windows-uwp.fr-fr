---
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: Apprenez à utiliser la classe AdScheduler pour afficher des publicités dans du contenu vidéo.
title: Afficher des publicités dans du contenu vidéo
ms.date: 03/22/2018
ms.topic: article
keywords: Windows10, uwp, annonces, publicités, vidéo, planificateur, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 29e2c46636445adac496d0f2149e956c5703c20d
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8189533"
---
# <a name="show-ads-in-video-content"></a>Afficher des publicités dans du contenu vidéo

Cette procédure pas à pas montre comment utiliser la classe **AdScheduler** pour afficher des publicités dans du contenu vidéo dans une application de plateforme Windows universelle (UWP) écrite en JavaScript avec HTML.

> [!NOTE]
> Cette fonctionnalité n’est actuellement prise en charge que pour les applications UWP écrites en JavaScript avec HTML.

**AdScheduler** fonctionne avec les médias à diffusion progressive ou continue et utilise les formats de charge utile standard de l’IAB: VAST (Video Ad Serving Template)2.0/3.0 et VMAP. L’utilisation des normes rend **AdScheduler** indépendant du service de publicité avec lequel il interagit.

La publicité pour contenu vidéo varie selon que le programme dure moins de dix minutes (format court) ou plus de dix minutes (format long). Bien que ce dernier soit plus difficile à mettre en place au niveau du service, la façon d’écrire le code côté client ne présente en fait aucune différence. Si **AdScheduler** reçoit une charge utile VAST avec une seule publicité au lieu d’un manifeste, elle est traitée comme si le manifeste appelait une publicité précédant la vidéo (s’arrêtant à 00:00).

## <a name="prerequisites"></a>Conditions préalables

* Installer le [SDK MicrosoftAdvertising](http://aka.ms/ads-sdk-uwp) avec VisualStudio2015 ou version ultérieure.

* Votre projet doit utiliser le contrôle [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) pour afficher le contenu vidéo dans lequel les publicités doivent apparaître. Ce contrôle est disponible dans la collection [TVHelpers](https://github.com/Microsoft/TVHelpers) des bibliothèques mises à la disposition sur GitHub par Microsoft.

  L’exemple suivant montre comment déclarer un [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) dans le balisage HTML. En règle générale, ce code appartient à la section `<body>` du fichier index.html (ou un autre fichier html, selon votre projet).

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  L’exemple suivant montre comment établir un [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) dans du code JavaScript.

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>Comment utiliser la classe AdScheduler dans votre code

1. Dans Visual Studio, ouvrez votre projet ou créez-en un.

2. Si votre projet cible **Toute UC**, mettez-le à jour pour utiliser une sortie de génération propre à l’architecture (par exemple, **x86**). Si votre projet cible **Toute UC**, vous ne pourrez pas ajouter une référence à la bibliothèque de publicités Microsoft dans les étapes suivantes. Pour plus d’informations, consultez [Erreurs de référence provoquées par le ciblage de Toute UC dans votre projet](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Ajoutez une référence à la bibliothèque du **SDK Microsoft Advertising pour JavaScript** à votre projet.

    1. Dans la fenêtre **Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence.**
    2. Dans **Gestionnaire de références**, développez **Windows universel**, cliquez sur **Extensions**, puis cochez la case en regard de **Kit de développement logiciel (SDK) Microsoft Advertising pour JavaScript** (version10.0).
    3. Dans **Gestionnaire de références**, cliquez sur OK.

4.  Ajoutez le fichier AdScheduler.js à votre projet:

    1. Dans VisualStudio, cliquez sur **Projet** et sur **Gérer les packages NuGet**.
    2. Dans la zone de recherche, tapez **Microsoft.StoreServices.VideoAdScheduler**, puis installez le package Microsoft.StoreServices.VideoAdScheduler. Le fichier AdScheduler.js est ajouté au sous-répertoire .. /js de votre projet.

5.  Ouvrez le fichier index.html (ou un autre fichier html en fonction de votre projet). Dans la section `<head>`, après les références JavaScript des fichiers default.css et main.js du projet, ajoutez la référence à ad.js et adscheduler.js.

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > Vous devez placer cette ligne dans la section `<head>` après l’inclusion de main.js, faute de quoi vous rencontrerez une erreur pendant la génération de votre projet.

6.  Dans le fichier main.js de votre projet, ajoutez le code qui crée un objet **AdScheduler**. Transmettez le **MediaPlayer** qui héberge votre contenu vidéo. Le code doit être placé de telle sorte qu’il s’exécute après [WinJS.UI.processAll](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh440975).

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  Utilisez les méthodes **requestSchedule** ou **requestScheduleByUrl** de l'objet **AdScheduler** pour demander une planification publicitaire auprès du serveur, l’insérer dans la chronologie **MediaPlayer**, puis lire le média vidéo.

    * Si vous êtes partenaire Microsoft et que vous avez reçu l’autorisation de demander une planification publicitaire auprès du serveur d’annonces Microsoft, utilisez **requestSchedule** et spécifiez l’ID d’application et l’ID d’unité publicitaire qui vous ont été fournis par votre représentant Microsoft.

        Cette méthode prend la forme d’une [Promesse](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript), soit une structure asynchrone dans laquelle des pointeurs à deux fonctions sont transmis: un pointeur pour la fonction **onComplete** à appeler lorsque la promesse est correctement finalisée et un pointeur pour la fonction **onError** à appeler si une erreur s’est produite. Dans la fonction **onComplete**, démarrez la lecture de votre contenu vidéo. La publicité démarrera la lecture à l’heure planifiée. Dans votre fonction **onError**, traitez l'erreur, puis démarrez la lecture de votre vidéo. Votre contenu vidéo sera lu sans publicité. L’argument de la fonction **onError** est un objet qui contient les membres suivants.

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * Pour demander une planification publicitaire auprès d’un serveur d’annonces non Microsoft, utilisez **requestScheduleByUrl** et transmettez l’URI du serveur. Cette méthode prend également la forme d’une **promesse** qui accepte des pointeurs pour les fonctions **onComplete** et **onError**. La charge utile de la publicité qui est retournée à partir du serveur doit respecter les formats de charge utile vidéo VAST (Video Ad Serving Template) ou VMAP (Video Multiple Ad Playlist).

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > Vous devez attendre le retour des fonctions **requestSchedule** ou **requestScheduleByUrl** avant de commencer la lecture du contenu vidéo principal dans le **MediaPlayer**. Si vous commencez à lire le contenu multimédia avant les retours **requestSchedule** (dans le cas d’une publication précédant la vidéo), la vidéo précédant la vidéo interrompra le contenu vidéo principal. Vous devez appeler **play** même si la fonction échoue, car **AdScheduler** demande à **MediaPlayer** d’ignorer la ou les annonces et de passer directement au contenu. Vous pouvez avoir une exigence différente, telle que l’insertion d’une publicité intégrée dans le cas où une annonce ne peut pas être récupérée à distance.

8.  Pendant la lecture, vous pouvez gérer d’autres événements qui permettent à votre application de suivre la progression et/ou les erreurs susceptibles de se produire après le processus de sélection d’annonce initial. Le code suivant contient certains de ces événements, dont **onPodStart**, **onPodEnd**, **onPodCountdown**, **onAdProgress**, **onAllComplete** et **onErrorOccurred**.

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>Membres AdScheduler

Cette section fournit des détails sur les membres de l'objet **AdScheduler**. Pour plus d’informations sur ces membres, reportez-vous aux commentaires et aux définitions se trouvant dans le fichier AdScheduler.js de votre projet.

### <a name="requestschedule"></a>requestSchedule

Cette méthode demande une planification publicitaire à partir du serveur ActiveDirectory de Microsoft et l’insère dans la chronologie du **MediaPlayer** qui a été transmise au constructeur **AdScheduler**.

Le troisième paramètre optionnel (*adTags*) est une collection JSON de paires nom/valeur qui peut être utilisée pour les apps dotées de fonctions de ciblage avancées. Par exemple, une app qui lit une variété de vidéos sur l'univers automobile peut compléter l’ID d’unité publicitaire avec la marque et le modèle des voitures présentées. Ce paramètre peut uniquement être utilisé par les partenaires ayant reçu une approbation de la part de Microsoft pour utiliser des balises publicitaires.

Il convient de noter les éléments suivants lorsque vous faites référence à *adTags*:

* Ce paramètre est une option très rarement utilisée. L’éditeur doit travailler en étroite collaboration avec Microsoft avant d’utiliser adTags.
* Les noms et les valeurs doivent être prédéterminés sur le service de publicité. Les balises publicitaires ne constituent pas des termes de recherche ou des mots clés ouverts.
* Le nombre maximal de balises est limité à 10.
* Les noms de balises sont limités à 16caractères.
* Les valeurs des balises peuvent contenir un maximum de 128caractères.

### <a name="requestschedulebyuri"></a>requestScheduleByUri

Cette méthode demande une planification publicitaire à partir du serveur publicitaire tiers (non Microsoft) spécifié dans l'URI et l’insère dans la chronologie du **MediaPlayer** qui a été transmis par le constructeur **AdScheduler**. La charge utile de la publicité retournée par le serveur publicitaire doit respecter les formats de charge utile vidéo VAST (Video Ad Serving Template) ou VMAP (Video Multiple Ad Playlist).

### <a name="mediatimeout"></a>mediaTimeout

Cette propriété obtient ou définit le nombre de millisecondes durant lesquelles le contenu multimédia doit être lisible. La valeur 0 indique au système qu'il n'existe aucun délai d’expiration. La valeur par défaut est 30000 millisecondes (30secondes).

### <a name="playskippedmedia"></a>playSkippedMedia

Cette propriété obtient ou définit une valeur **booléenne** qui indique si le support sera lu si l’utilisateur avance directement à un point situé au-delà de l'heure de début planifiée.

Le client publicitaire et le lecteur multimédia appliqueront des règles visant à définir ce qu'il advient des publicités durant l'avance rapide ou le retour rapide du contenu vidéo principal. Dans la plupart des cas, les développeurs d’apps ne permettent pas que les publicités soient entièrement ignorées, mais souhaitent toutefois garantir une expérience raisonnable pour l'utilisateur. Les deux options suivantes répondent aux besoins de la plupart des développeurs:

1. Autoriser les utilisateurs finaux à ignorer les pods publicitaires à volonté.
2. Autoriser les utilisateurs à ignorer les pods publicitaires, mais lire le pod publicitaire le plus récent lors de la reprise de la lecture.

La propriété **playSkippedMedia** remplit les conditions suivantes:

* Les publicités ne peuvent pas être ignorées ou avancées rapidement une fois qu'elles ont démarré.
* Toutes les publicités d'un pod publicitaire seront lues une fois que le pod a démarré.
* Une fois qu'elle a été lue, une publicité ne sera pas lue à nouveau pendant le contenu principal (film, épisode, etc.); les marqueurs publicitaires indiqueront que ce contenu a été lu ou supprimé.
* Les publicités préalables au contenu ne peuvent pas être ignorées.

Lors de la reprise de la lecture d'un contenu contenant des publicités, définissez **playSkippedMedia** sur **false** pour ignorer les publicités préalables au contenu et éviter la lecture de la coupure publicitaire la plus récente. Ensuite, une fois que le contenu aura démarré, définissez **playSkippedMedia** sur **true** pour vous assurer que les utilisateurs ne peuvent pas avancer rapidement les publicités suivantes.

> [!NOTE]
> Un pod est un groupe de publicités lues dans une séquence, tel un groupe de publicités lues durant une coupure publicitaire. Pour plus d’informations, consultez la spécification IAB Digital Video Ad Serving Template (VAST).

### <a name="requesttimeout"></a>requestTimeout

Cette propriété obtient ou définit le nombre de millisecondes à patienter avant l'expiration d'une réponse à une demande de publicité. La valeur 0 indique au système qu'il n'existe aucun délai d’expiration. La valeur par défaut est 30000 millisecondes (30secondes).

### <a name="schedule"></a>calendrier

Cette propriété obtient les données de planification récupérées auprès du serveur publicitaire. Cet objet comprend la hiérarchie complète des données correspondant à la structure de la charge utile VAST (Video Ad Serving Template) ou VMAP (Video Multiple Ad Playlist).

### <a name="onadprogress"></a>onAdProgress  

Cet événement est déclenché lorsque la lecture de publicités atteint des points de contrôle quartiles. Le deuxième paramètre du gestionnaire d’événements (*eventInfo*) est un objet JSON comprenant les membres suivants:

* **progress**: l’état de la lecture de la publicité (l'une des valeurs d’énumération **MediaProgress** définies dans AdScheduler.js).
* **clip**: le clip vidéo en cours de lecture. Cet objet n’est pas destiné à être utilisé dans votre code.
* **adPackage**: un objet qui représente la partie de la charge utile publicitaire correspondant à la publicité en cours de lecture. Cet objet n’est pas destiné à être utilisé dans votre code.

### <a name="onallcomplete"></a>onAllComplete  

Cet événement est déclenché lorsque le contenu principal prend fin et que toute publicité suivant le contenu est également terminée.

### <a name="onerroroccurred"></a>onErrorOccurred  

Cet événement est déclenché lorsque **AdScheduler** rencontre une erreur. Pour plus d'informations sur les valeurs de code d'erreur, consultez [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.errorcode).

### <a name="onpodcountdown"></a>onPodCountdown

Cet événement est déclenché lorsqu'une publicité est en cours de lecture et indique combien de temps il reste dans le pod actuel. Le deuxième paramètre du gestionnaire d’événements (*eventData*) est un objet JSON comprenant les membres suivants:

* **remainingAdTime**: le nombre de secondes restantes pour la publicité en cours de lecture.
* **remainingPodTime**: le nombre de secondes restantes pour le pod en cours de lecture.

> [!NOTE]
> Un pod est un groupe de publicités lues dans une séquence, tel un groupe de publicités lues durant une coupure publicitaire. Pour plus d’informations, consultez la spécification IAB Digital Video Ad Serving Template (VAST).

### <a name="onpodend"></a>onPodEnd  

Cet événement est déclenché lorsqu’un pod se termine. Le deuxième paramètre du gestionnaire d’événements (*eventData*) est un objet JSON comprenant les membres suivants:

* **startTime**: l'heure de début du pod, en secondes.
* **pod**: un objet représentant le pod. Cet objet n’est pas destiné à être utilisé dans votre code.

### <a name="onpodstart"></a>onPodStart

Cet événement est déclenché lorsqu’un pod démarre. Le deuxième paramètre du gestionnaire d’événements (*eventData*) est un objet JSON comprenant les membres suivants:

* **startTime**: l'heure de début du pod, en secondes.
* **pod**: un objet représentant le pod. Cet objet n’est pas destiné à être utilisé dans votre code.
