---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "En savoir plus sur les recommandations en matière d’expérience utilisateur et d’interface utilisateur pour les publicités dans les applications."
title: "Recommandations pour pubs in-app: expérience et interface utilisateur"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d464a2de442e6f1833f429c8460c27bf85e577d1


---

# Recommandations en matière d’expérience utilisateur et d’interface utilisateur pour les publicités dans les applications




## Ressources d’interface utilisateur générales pour applications Windows

Vous trouverez des informations sur la façon de concevoir l’apparence des applications dans [Conception et interface utilisateur](https://developer.microsoft.com/windows/design).

## Meilleures pratiques AdControl

* [Meilleures pratiques AdControl: À FAIRE](#adcontrolbestpracticesdo10)
* [Meilleures pratiques AdControl: À NE PAS FAIRE](#adcontrolbestpracticesdont10)

<span id="adcontrolbestpracticesdo10"/>
### Meilleures pratiques AdControl: À FAIRE

* Intégrez des publicités à votre expérience. Donnez à vos concepteurs un exemple d’annonce pour planifier ce à quoi ressemblera la publicité. La disposition de publicités en tant que contenu et la disposition scindée sont deux exemples de publicités bien planifiées dans les applications.

  Pour bénéficier d’un aperçu du fonctionnement et de l’apparence de différentes tailles d’annonces au sein de votre application, vous pouvez utiliser nos unités de publicité en mode test pour Windows Phone, Windows 8.1 et Windows 10. Une fois vos tests terminés, pensez à [mettre à jour votre application avec des ID d’unité publicitaire réels](set-up-ad-units-in-your-app.md), avant d’envoyer l’application pour certification.

* Planifiez pour les périodes au cours desquelles aucune annonce ne sera disponible. Il peut arriver à certains moments qu’aucune annonce ne soit envoyée à votre application. Disposez vos pages de telle façon qu’elles s’affichent de manière optimale avec ou sans annonce. Pour plus d’informations, consultez [Gestion des erreurs](error-handling-with-advertising-libraries.md).

<span id="adcontrolbestpracticesdont10"/>
### Meilleures pratiques AdControl: À NE PAS FAIRE

* Bloquer de la publicité dans les espaces ouverts. L’espace publicitaire ne doit pas être placé dans le premier espace libre que vous trouvez. Au lieu de cela, il doit être incorporé dans la conception globale de votre application.

* Publicité excessive et saturation de votre application. Trop de publicités dans votre application détournent l’attention de son apparence et de sa facilité d’utilisation. Vous souhaitez gagner de l’argent avec les publicités, mais pas au détriment de l’application elle-même.

* Distraire l’utilisateur de ses tâches de base. L’axe principal doit toujours être l’application. L’espace publicitaire doit être incorporé de manière à rester secondaire.

<span id="interstitialbestpractices10"/>
## Meilleures pratiques Spots

* [Meilleures pratiques Spots: À FAIRE](#interstitialbestpracticesdo10)
* [Meilleures pratiques Spots: À ÉVITER](#interstitialbestpracticesavoid10)
* [Meilleures pratiques Spots: À NE JAMAIS FAIRE (Stratégie appliquée)](#interstitialbestpracticesnever10)

Lorsqu’ils sont utilisés de façon élégante, les spots publicitaires vidéo peuvent augmenter considérablement vos revenus issus de l’application, sans impact négatif sur la satisfaction des utilisateurs. En cas d’utilisation incorrecte, ils peuvent produire l’effet inverse exact.

Ici, nous cherchons à vous aider à atteindre l’élégance. Étant donné que vous connaissez votre application mieux que quiconque, sauf en matière de stratégie, nous vous laissons prendre la meilleure décision finale. Ce qu’il faut absolument retenir c’est que les évaluations de vos applications et le montant de vos recettes sont étroitement liés.

<span id="interstitialbestpracticesdo10"/>
### Meilleures pratiques Spots: À FAIRE

* Intégrez des spots publicitaires dans le flux naturel de l’application, par exemple entre les niveaux de jeu.

* Associez des annonces avec des avantages concrets, tels que:

    * Conseils concernant l’achèvement de niveau.

    * Temps supplémentaire pour réessayer un niveau.

    * Fonctionnalités d’avatar personnalisé, par exemple avec un tatouage ou un chapeau.

* Si votre application requiert le visionnage complet d’une publicité vidéo, mentionnez cette règle d’emblée afin que les utilisateurs ne soient pas surpris par un message d’erreur en appuyant sur le bouton Fermer.

* Récupérez d’abord la publicité (en appelant la méthode [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)), dans l’idéal, 30 à 60secondes avant que vous ayez besoin de l’afficher.

* Abonnez-vous aux quatre événements exposés dans la classe [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) (**Canceled**, **Completed**, **AdReady** et **ErrorOccurred**) et utilisez-les pour prendre les bonnes décisions pour votre application.

* Bénéficiez d’une expérience intégrée au lieu d’annonces proposées par le serveur. Vous trouverez cela utile dans certains scénarios:

    * En mode hors connexion, lorsque les serveurs publicitaires sont injoignables.

    * Lorsque l’événement **ErrorOccurred** est déclenché.

    * Si vous choisissez d’économiser la bande passante de l’utilisateur en fonction de [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx), il existe des API dans la classe **ConnectionProfile** qui peuvent vous aider.

* Utilisez le délai d’expiration par défaut (30s), sauf si vous avez une raison valable de faire autrement, auquel cas n’allez pas en dessous de 10s.

    * Les annonces vidéo sont beaucoup plus longues à télécharger que les bannières, en particulier dans les marchés ne disposant pas de connexions haut débit.


* Gardez à l’esprit les forfaits de données des utilisateurs. Par exemple, n’affichez pas d’annonce vidéo, ou avertissez l’utilisateur avant de le faire sur un appareil mobile approchant de ou ayant dépassé sa limite de données. Il existe des API dans la classe [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) qui peuvent vous aider.

* Améliorez en permanence votre application après la soumission initiale. Examinez les rapports de publicité et apportez des modifications de conception pour améliorer les taux de remplissage et d’achèvement vidéo.

<span id="interstitialbestpracticesavoid10"/>
### Meilleures pratiques Spots: À ÉVITER

* Exagération. N’envoyez pas de publicités à une fréquence supérieure à toutes les 5minutes environ, à moins que l’utilisateur ne bénéficie d’un avantage concret en option, au-delà du jeu.

* Des spots vidéo au lancement de l’application, car les utilisateurs pourraient penser qu’ils ont cliqué sur la mauvaise vignette.

* Des spots vidéo de sortie. Il s’agit d’un mauvais calcul, car les taux d’achèvement seront proches de zéro.

    * Deux annonces vidéo d’affilée.

    * Les utilisateurs seront contrariés de voir la barre de progression de l’annonce revenir au point de départ.

    * Beaucoup penseront qu’il s’agit d’un bogue de codage ou d’affichage de publicité.

* Récupération d’une annonce vidéo plus de 5minutes avant d’appeler [Afficher](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

    * Un bon inventaire va optimiser la conversion de publicités récupérées en impressions facturables.


* Pénaliser un utilisateur pour les défaillances d’affichage de publicités, par exemple lorsqu’aucune publicité n’est disponible. Par exemple, si vous affichez une option d’interface utilisateur de type «Regarder une publicité pour obtenir *xxx*», vous devez fournir *xxx* si l’utilisateur a rempli sa part du contrat. Deux options à envisager:

    * N’incluez pas l’option, sauf si l’événement [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) a été déclenché.

    * Proposez une application incluant une expérience intégrée qui procure les mêmes avantages qu’une annonce réelle.

* Permettre à un utilisateur de gagner un avantage compétitif dans un jeu multijoueur.

    * Une meilleure arme dans un jeu de tir entrerait clairement dans cette catégorie.

    * Une chemise personnalisée sur l’avatar du joueur pourrait faire l’affaire, tant qu’elle ne sert pas de camouflage!

<span id="interstitialbestpracticesnever10"/>
### Meilleures pratiques Spots: À NE JAMAIS FAIRE (Stratégie appliquée)

* Placer les éléments de l’interface utilisateur sur le conteneur de publicité.

    * Les annonceurs ont payé pour l’intégralité de l’écran.


* Appeler **Afficher** pendant que l’utilisateur utilise l’application.

    * Dans la mesure où **InterstitialAd** créera une superposition sur l’intégralité de l’écran, l’utilisateur risque d’être déstabilisé.

    * Cela peut également donner lieu à des taux de clic exagérés.

* Utiliser des annonces pour obtenir tout ce qui peut être utilisé comme une devise ou échangé avec d’autres utilisateurs.

 

 



<!--HONumber=Aug16_HO3-->


