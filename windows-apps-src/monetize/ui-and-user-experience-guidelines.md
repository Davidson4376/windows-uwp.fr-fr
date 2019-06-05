---
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: En savoir plus sur les recommandations en matière d’expérience utilisateur et d’interface utilisateur pour les publicités dans les applications.
title: Recommandations pour l'expérience et l'interface utilisateur pour les annonces
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, annonce, publicité, directives, meilleures pratiques
ms.localizationpriority: medium
ms.openlocfilehash: 7bb187092158386c503b91973d8d63ce291b4760
ms.sourcegitcommit: c62b260eecd373e57b19a623bae77e29180e0f3d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66565513"
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Recommandations pour l'expérience et l'interface utilisateur pour les annonces

Cet article fournit des recommandations pour concevoir des expériences exceptionnelles avec des bannières publicitaires, des spots et des publicités natives dans vos applications. Pour obtenir des conseils généraux sur la conception de l’apparence et du style des applications, consultez [Conception et interface utilisateur](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Toute utilisation de publicité dans votre app doit se conformer aux Politiques du Microsoft Store, y compris et sans s’y limiter, à la [politique 10.10](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) (Conduite en matière de publicité et contenu publicitaire). En particulier, l’implémentation de bannières ou spots publicitaires dans votre app doit respecter les règles stipulées dans la [politique 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des Politiques du Microsoft Store. Cet article fournit des exemples d’implémentations qui ne respectent pas cette politique. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect des Politiques du Microsoft Store qui ne sont pas répertoriés dans le présent article.

## <a name="general-best-practices"></a>Meilleures pratiques générales

Avant d’examiner nos recommandations s'agissant des différents types de publicités décrits dans cet article, passez tout d’abord en revue ces recommandations générales pour améliorer vos revenus publicitaires.

* [Planifiez soigneusement les emplacements de votre publicité](https://blogs.windows.com/buildingapps/2017/04/10/monetizing-app-advertisement-placement/). Consultez nos recommandations connexes sur [l'optimisation de la visibilité de vos unités publicitaires](optimize-ad-unit-viewability.md).
* [Utilisez des bannières publicitaires spots comme repli pour les spots vidéo publicitaires](https://blogs.windows.com/buildingapps/2017/04/17/monetizing-app-use-interstitial-banner-fallback-interstitial-video).
* [Connaissez vos utilisateurs pour servir des publicités mieux ciblées](https://blogs.windows.com/buildingapps/2017/05/17/monetize-app-know-user-serve-better-targeted-ads/).
* [Utilisez les bibliothèques de publicités les plus récentes](https://blogs.windows.com/buildingapps/2017/05/22/earn-money-moving-latest-advertising-libraries/).
* [Définissez correctement les paramètres de la réglementation COPPA pour votre application](https://blogs.windows.com/buildingapps/2017/06/21/monetizing-app-set-coppa-settings-app).


## <a name="guidelines-for-banner-ads"></a>Recommandations pour les bannières publicitaires

Les sections suivantes fournissent des recommandations sur la façon d’implémenter des [bannières publicitaires](banner-ads.md) dans votre app à l’aide d’objets [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol). Elles donnent également des exemples d’implémentation qui ne respectent pas les règles stipulées dans la [politique 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des Politiques du Microsoft Store.

### <a name="best-practices"></a>Meilleures pratiques

Nous vous recommandons de suivre ces bonnes pratiques quand vous implémentez des bannières publicitaires dans votre application :

* [Utilisez des tailles de l'Interactive Advertising Bureau](https://blogs.windows.com/buildingapps/2017/04/03/monetizing-app-use-interactive-advertising-bureau-ad-sizes) adaptées à la disposition sur l’appareil utilisé.

* Réservez la majeure partie de l’interface utilisateur de votre application aux contrôles et au contenu fonctionnels.

* Intégrez des publicités à votre expérience. Donnez à vos concepteurs un exemple d’annonce pour planifier ce à quoi ressemblera la publicité. La disposition de publicités en tant que contenu et la disposition scindée sont deux exemples de publicités bien planifiées dans les applications.

  Pour obtenir un aperçu du fonctionnement et de l’apparence de différentes tailles d’annonces dans votre application pendant les phases de développement et de test, vous pouvez utiliser nos [unités publicitaires en mode test](set-up-ad-units-in-your-app.md#test-ad-units). Après les tests, pensez à [mettre à jour votre application avec des ID d’unités publicitaires dynamiques](set-up-ad-units-in-your-app.md#live-ad-units) avant d’envoyer l’application pour certification.

* Planifiez pour les périodes au cours desquelles aucune annonce ne sera disponible. Il peut arriver à certains moments qu’aucune annonce ne soit envoyée à votre application. Disposez vos pages de telle façon qu’elles s’affichent de manière optimale avec ou sans annonce. Pour plus d’informations, consultez [Gérer des erreurs dans les publicités](error-handling-with-advertising-libraries.md).

* Si, dans votre scénario, la gestion des alertes envoyées à l’utilisateur est plus efficace avec une superposition, appelez [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend) quand la superposition s’affiche, puis appelez [AdControl.Resume](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.resume) à la fin de l’alerte.

### <a name="practices-to-avoid"></a>Pratiques à éviter

Nous vous recommandons d’éviter ces pratiques quand vous implémentez des bannières publicitaires dans votre application :

* Ne bloquez pas les publicités dans les espaces ouverts. L’espace publicitaire ne doit pas être placé dans le premier espace libre que vous trouvez. Au lieu de cela, il doit être incorporé dans la conception globale de votre application.

* Ne surchargez pas votre application avec une publicité excessive. Trop de publicités dans votre application détournent l’attention de son apparence et de sa facilité d’utilisation. Vous souhaitez gagner de l’argent avec les publicités, mais pas au détriment de l’application elle-même.

* Ne distrayez pas l’utilisateur de ses tâches de base. L’axe principal doit toujours être l’application. L’espace publicitaire doit être incorporé de manière à rester secondaire.

### <a name="examples-of-policy-violations"></a>Exemples de non-respect de la politique

Cette section donne des exemples d’implémentation de bannières publicitaires qui ne respectent pas les règles stipulées dans la [politique 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des Politiques du Microsoft Store. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect de la politique 10.10.1 qui ne sont pas répertoriés ici.

* Empêcher d’une façon ou d’une autre l’utilisateur d’afficher la bannière publicitaire, comme modifier l’opacité de l’objet [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) ou placer un autre contrôle sur **AdControl** (sans appeler au préalable [AdControl.Suspend](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.suspend)).

* Obliger les utilisateurs à cliquer sur une bannière publicitaire pour pouvoir effectuer une tâche dans votre application, ou concevoir votre application de sorte que les utilisateurs n’aient pas d’autre choix que cliquer sur des bannières publicitaires.

* Contourner le minuteur d’actualisation intégré minimum pour les bannières publicitaires par quelque moyen que ce soit, y compris, mais sans s’y limiter, en remplaçant des objets [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) ou en actualisant automatiquement une page sans aucune interaction utilisateur.

* À l’aide d’annonce en ligne unités (autrement dit, ad que vous obtenez auprès des partenaires) pendant le développement et de test, ou dans un émulateur.

* Écrire ou distribuer du code qui appelle des services publicitaires par d’autres moyens que les bibliothèques de publicités Microsoft exécutées dans le contexte de votre application.

* Interagir avec des interfaces non documentées ou des objets enfants créés par les bibliothèques de publicités Microsoft, comme **WebView** ou **MediaElement**.

* Placer des annonces publicitaires dans un viewbox pour réduire la taille des annonces pour permettre des annonces sur une page à la normale.

<span id="interstitialbestpractices10" />

## <a name="guidelines-for-interstitial-ads"></a>Recommandations pour les spots publicitaires

Quand ils sont utilisés de façon élégante, les [spots publicitaires](interstitial-ads.md) peuvent augmenter considérablement vos revenus issus de l’application, sans impact négatif sur la satisfaction des utilisateurs. En cas d’utilisation incorrecte, ils peuvent produire l’effet inverse exact.

Les sections suivantes fournissent des recommandations sur la façon d’implémenter des annonces vidéo et des bannières spots publicitaires dans votre app à l’aide d’objets [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad). Elles donnent également des exemples d’implémentation qui ne respectent pas les règles stipulées dans la [politique 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des Politiques du Microsoft Store. Étant donné que vous connaissez votre application mieux que quiconque, sauf en matière de stratégie, nous vous laissons prendre la meilleure décision finale. Ce qu’il faut absolument retenir c’est que les évaluations de vos applications et le montant de vos recettes sont étroitement liés.

### <a name="best-practices"></a>Meilleures pratiques

Nous vous recommandons de suivre ces bonnes pratiques quand vous implémentez des spots publicitaires dans votre application :

* Intégrez des spots publicitaires dans le flux naturel de l’application, par exemple entre les niveaux de jeu.

* Associez des annonces avec des avantages concrets, tels que :

    * Conseils concernant l’achèvement de niveau.

    * Temps supplémentaire pour réessayer un niveau.

    * Fonctionnalités d’avatar personnalisé, par exemple avec un tatouage ou un chapeau.

* Si votre application requiert le visionnage complet d’un spot vidéo, mentionnez cette règle d’emblée afin que les utilisateurs ne soient pas surpris par un message d’erreur en appuyant sur le bouton Fermer.

* Récupérez d’abord la publicité (en appelant la méthode [InterstitialAd.RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)), dans l’idéal, 30 à 60 secondes avant que vous ayez besoin de l’afficher.

* Abonnez-vous aux quatre événements exposés dans la classe [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) (**Canceled**, **Completed**, **AdReady** et **ErrorOccurred**) et utilisez-les pour prendre les bonnes décisions pour votre application.

* Bénéficiez d’une expérience intégrée au lieu d’annonces proposées par le serveur. Vous trouverez cela utile dans certains scénarios :

    * En mode hors connexion, lorsque les serveurs publicitaires sont injoignables.

    * Lorsque l’événement **ErrorOccurred** est déclenché.

    * Si vous choisissez d’économiser la bande passante de l’utilisateur en fonction de [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile), il existe des API dans la classe **ConnectionProfile** qui peuvent vous aider.

* Utilisez le délai d’expiration par défaut (30 secondes), sauf si vous avez une raison valable de faire autrement, auquel cas n’allez pas en dessous de 10 secondes. Les spots sont beaucoup plus longues à télécharger que les bannières standard, en particulier dans les marchés ne disposant pas de connexions haut débit.

* Gardez à l’esprit les forfaits de données des utilisateurs. Par exemple, n’affichez pas de spot vidéo, ou avertissez l’utilisateur avant de le faire sur un appareil mobile approchant de ou ayant dépassé sa limite de données. Il existe des API dans la classe [ConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) qui peuvent vous aider.

* Améliorez en permanence votre application après la soumission initiale. Examinez les [rapports de publicité](../publish/advertising-performance-report.md) et apportez des modifications de conception pour améliorer les taux de remplissage et d’achèvement de spot vidéo.

### <a name="practices-to-avoid"></a>Pratiques à éviter

Nous vous recommandons d’éviter ces pratiques quand vous implémentez des spots publicitaires dans votre application :

* N’intégrez pas de spots publicitaires de manière excessive dans votre application. N’envoyez pas de publicités à une fréquence supérieure à toutes les 5 minutes environ, à moins que l’utilisateur ne bénéficie d’un avantage concret en option, au-delà du jeu.

* N’affichez pas de spots au lancement de l’application, car les utilisateurs pourraient penser qu’ils ont cliqué sur la mauvaise vignette.

* N’affichez pas de spots de sortie. Il s’agit d’un mauvais calcul, car les taux d’achèvement seront proches de zéro.

* N’affichez pas plusieurs spots publicitaires d’affilée. Les utilisateurs seront contrariés de voir la barre de progression de l’annonce revenir au point de départ. Beaucoup penseront qu’il s’agit d’un bogue de codage ou d’affichage de publicité.

* Ne récupérez pas un spot vidéo plus de 5 minutes avant d’appeler [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show). Un bon inventaire va optimiser la conversion de publicités récupérées en impressions facturables.

* Ne pénalisez pas un utilisateur pour les défaillances d’affichage de publicités, par exemple quand aucune publicité n’est disponible. Par exemple, si vous affichez une option d’interface utilisateur de type « Regarder une publicité pour obtenir *xxx* », vous devez fournir *xxx* si l’utilisateur a rempli sa part du contrat. Deux options à envisager :

    * N’incluez pas l’option, sauf si l’événement [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready) a été déclenché.

    * Proposez une application incluant une expérience intégrée qui procure les mêmes avantages qu’une annonce réelle.

* N’utilisez pas de spots publicitaires permettant à un utilisateur de gagner un avantage compétitif dans un jeu multijoueur. Par exemple, n’affichez pas de spot publicitaire incitant l’utilisateur à se servir d’une meilleure arme dans un jeu de tir. Une chemise personnalisée sur l’avatar du joueur pourrait faire l’affaire, tant qu’elle ne sert pas de camouflage !

### <a name="examples-of-policy-violations"></a>Exemples de non-respect de la politique

Cette section donne des exemples d’implémentations de spots publicitaires qui ne respectent pas les règles stipulées dans la [politique 10.10.1](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) des Politiques du Microsoft Store. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect de la politique 10.10.1 qui ne sont pas répertoriés ici.

* Placer un élément d’interface utilisateur sur le conteneur de spot publicitaire.

* Appeler [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) quand l’utilisateur interagit avec l’application.

* Utiliser des spots publicitaires pour obtenir tout ce qui peut être utilisé comme une devise ou échangé avec d’autres utilisateurs.

* Demander un nouveau spot publicitaire dans le contexte du gestionnaire d’événements pour l’événement [InterstitialAd.ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.erroroccurred). Cela peut produire une boucle infinie et provoquer des problèmes de fonctionnement du service publicitaire.

* Demander un spot publicitaire simplement pour avoir une publicité de secours à une cascade de publicités. Si vous demandez un spot publicitaire et que vous recevez l’événement [InterstitialAd.AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.adready), le spot publicitaire suivant affiché dans votre application doit être celui qui est prêt à être affiché par la méthode [InterstitialAd.Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show).

* À l’aide d’annonce en ligne unités (autrement dit, ad que vous obtenez auprès des partenaires) pendant le développement et de test, ou dans un émulateur.

* Écrire ou distribuer du code qui appelle des services publicitaires par d’autres moyens que les bibliothèques de publicités Microsoft exécutées dans le contexte de votre application.

* Interagir avec des interfaces non documentées ou des objets enfants créés par les bibliothèques de publicités Microsoft, comme **WebView** ou **MediaElement**.

## <a name="guidelines-for-native-ads"></a>Recommandations concernant les publicités natives

Les [publicités natives](native-ads.md) vous donnent beaucoup de contrôle sur la façon de présenter le contenu publicitaire à vos utilisateurs. Respectez ces exigences et ces directives pour vous assurer que le message de l’annonceur atteigne vos utilisateurs tout en évitant que les publicités natives ne perturbent l'expérience de vos utilisateurs.

### <a name="register-the-container-for-your-native-ad"></a>Inscrire le conteneur de votre publicité native

Dans votre code, vous devez appeler la méthode [RegisterAdContainer](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2.registeradcontainer) de l'objet [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) pour enregistrer l’élément d’interface utilisateur agissant comme conteneur de la publicité native, ainsi que tous les contrôles spécifiques éventuels que vous souhaitez inscrire comme cibles interactives pour la publicité. C'est indispensable pour suivre correctement les expositions publicitaires et les clics.

Il existe deux surcharges pour la méthode **RegisterAdContainer** que vous pouvez utiliser :

* Si vous voulez que tout le conteneur de l'ensemble des éléments individuels de publicité native soit interactif, appelez la méthode **RegisterAdContainer(FrameworkElement)** et transmettez-lui le contrôle du conteneur. Par exemple, si vous affichez tous les éléments de publicité native dans des contrôles distincts tous hébergés dans un **StackPanel** et que vous voulez que tout le **StackPanel** soit interactif, transmettez le **StackPanel** à cette méthode.

* Si vous souhaitez que seuls certains éléments de publicité native soit interactifs, appelez la méthode **RegisterAdContainer(FrameworkElement, IVector(FrameworkElement))** . Seuls les contrôles que vous transmettez au deuxième paramètre seront interactifs.

### <a name="required-native-ad-elements"></a>Éléments de publicité native obligatoires

Au minimum, vous devez toujours montrer à l’utilisateur les éléments de publicité native suivants fournis par les propriétés de l'objet [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) dans votre conception de publicité native. Si vous ne parvenez pas à inclure ces éléments, vous obtiendrez peut-être de mauvaises performances et de faibles revenus de votre unité publicitaire.

1. Affichez toujours le titre de la publicité native (disponible dans la propriété **Title**). Fournissez suffisamment d’espace pour afficher au moins 25 caractères. Si le titre est plus long, remplacez le texte supplémentaire par des points de suspension.
2. Affichez toujours au moins l'un des éléments suivants pour différencier la publicité native du reste de votre application et indiquer clairement que le contenu est fourni par un annonceur :
    * L’icône identifiable *publicité* (disponible dans la propriété **AdIcon**). Cette icône est fournie par Microsoft.
    * Le texte *parrainé par* (disponible dans la propriété **SponsoredBy**). Ce texte est fourni par l'annonceur.
    * À la place du texte *parrainé par*, vous pouvez choisir d’afficher un autre texte qui permet de différencier la publicité native du reste de votre application, par exemple « Contenu sponsorisé », « Contenu promotionnel », « Contenu recommandé », etc.

### <a name="user-experience"></a>Expérience de l'utilisateur

Votre publicité native doit être délimitée clairement du reste de votre application et être entourée d’espace pour éviter les clics accidentels. Utilisez des bordures, des arrière-plans différents ou une autre interface utilisateur pour séparer le contenu de la publicité du reste de votre application. N’oubliez pas que les clics accidentels ne sont pas utiles pour votre chiffre d’affaires publicitaire ni pour l'expérience de votre utilisateur final à long terme.

### <a name="description"></a>Description

Si vous choisissez d’afficher la description de la publicité (disponible dans la propriété **Description** de l'objet **NativeAdV2**), fournissez suffisamment d’espace pour afficher au moins 75 caractères. Nous vous recommandons d’utiliser une animation pour afficher l’intégralité du contenu de description de la publicité.

### <a name="call-to-action"></a>Appel à l’action

Le texte *appel à l’action* (disponible dans la propriété **CallToAction** de l'objet **NativeAdV2**) est un composant essentiel de la publicité. Si vous choisissez d’afficher ce texte, suivez ces instructions :

* Montrez toujours le texte *appel à l’action* à l’utilisateur sur un contrôle interactif tel qu’un bouton ou un lien hypertexte.
* Affichez toujours le texte *appel à l’action* dans son intégralité.
* Vérifiez que le texte *appel à l’action* est séparé du reste du texte promotionnel de l’annonceur.
