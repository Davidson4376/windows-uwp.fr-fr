---
author: mcleanbyron
ms.assetid: 7a38a352-6e54-4949-87b1-992395a959fd
description: "En savoir plus sur les recommandations en matière d’expérience utilisateur et d’interface utilisateur pour les publicités dans les applications."
title: "Recommandations pour l&quot;expérience et l&quot;interface utilisateur pour les annonces"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, annonce, publicité, directives, meilleures pratiques"
ms.openlocfilehash: 8dc9c00bdeb47b5f07af0b9b27ef843e2afd4456
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2017
---
# <a name="ui-and-user-experience-guidelines-for-ads"></a>Recommandations pour l'expérience et l'interface utilisateur pour les annonces

Cet article fournit des recommandations pour concevoir des expériences exceptionnelles avec les bannières et spots publicitaires affichés dans vos applications. Pour obtenir des conseils généraux sur la conception de l’apparence et du style des applications, consultez [Conception et interface utilisateur](https://developer.microsoft.com/windows/apps/design).

> [!IMPORTANT]
> Toute utilisation de publicité dans votre application doit se conformer aux Politiques du Windows Store, y compris et sans s’y limiter, à la [politique10.10](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) (Conduite en matière de publicité et contenu publicitaire). En particulier, l’implémentation de bannières ou spots publicitaires dans votre application doit respecter les règles stipulées dans la [politique10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) des Politiques du WindowsStore. Cet article fournit des exemples d’implémentations qui ne respectent pas cette politique. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect des Politiques du Windows Store qui ne sont pas répertoriés dans le présent article.

## <a name="guidelines-for-banner-ads"></a>Recommandations pour les bannières publicitaires

Les sections suivantes fournissent des recommandations sur la façon d’implémenter des bannières publicitaires dans votre application à l’aide d’objets [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx). Elles donnent également des exemples d’implémentations qui ne respectent pas les règles stipulées dans la [politique10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) des Politiques du WindowsStore.

### <a name="best-practices"></a>Bonnes pratiques

Nous vous recommandons de suivre ces bonnes pratiques quand vous implémentez des bannières publicitaires dans votre application:

* Réservez la majeure partie de l’interface utilisateur de votre application aux contrôles et au contenu fonctionnels.

* Intégrez des publicités à votre expérience. Donnez à vos concepteurs un exemple d’annonce pour planifier ce à quoi ressemblera la publicité. La disposition de publicités en tant que contenu et la disposition scindée sont deux exemples de publicités bien planifiées dans les applications.

  Pour obtenir un aperçu du fonctionnement et de l’apparence de différentes tailles d’annonces dans votre application pendant les phases de développement et de test, vous pouvez utiliser nos [unités publicitaires en mode test](test-mode-values.md). Après les tests, pensez à [mettre à jour votre application avec des ID d’unité publicitaire réels](set-up-ad-units-in-your-app.md) avant d’envoyer l’application pour certification.

* Choisissez des [tailles de bannières publicitaires](supported-ad-sizes-for-banner-ads.md) adaptées à la disposition sur l’appareil utilisé.

* Planifiez pour les périodes au cours desquelles aucune annonce ne sera disponible. Il peut arriver à certains moments qu’aucune annonce ne soit envoyée à votre application. Disposez vos pages de telle façon qu’elles s’affichent de manière optimale avec ou sans annonce. Pour plus d’informations, consultez [Gestion des erreurs](error-handling-with-advertising-libraries.md).

* Si, dans votre scénario, la gestion des alertes envoyées à l’utilisateur est plus efficace avec une superposition, appelez [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx) quand la superposition s’affiche, puis appelez [AdControl.Resume](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.resume.aspx) à la fin de l’alerte.

<span />
### <a name="practices-to-avoid"></a>Pratiques à éviter

Nous vous recommandons d’éviter ces pratiques quand vous implémentez des bannières publicitaires dans votre application:

* Ne bloquez pas les publicités dans les espaces ouverts. L’espace publicitaire ne doit pas être placé dans le premier espace libre que vous trouvez. Au lieu de cela, il doit être incorporé dans la conception globale de votre application.

* Ne surchargez pas votre application avec une publicité excessive. Trop de publicités dans votre application détournent l’attention de son apparence et de sa facilité d’utilisation. Vous souhaitez gagner de l’argent avec les publicités, mais pas au détriment de l’application elle-même.

* Ne distrayez pas l’utilisateur de ses tâches de base. L’axe principal doit toujours être l’application. L’espace publicitaire doit être incorporé de manière à rester secondaire.

<span />
### <a name="examples-of-policy-violations"></a>Exemples de non-respect de la politique

Cette section donne des exemples d’implémentations de bannières publicitaires qui ne respectent pas les règles stipulées dans la [politique10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) des Politiques du WindowsStore. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect de la politique10.10.1 qui ne sont pas répertoriés ici.

* Empêcher d’une façon ou d’une autre l’utilisateur d’afficher la bannière publicitaire, comme modifier l’opacité de l’objet [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou placer un autre contrôle sur **AdControl** (sans appeler au préalable [AdControl.Suspend](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.suspend.aspx)).

* Obliger les utilisateurs à cliquer sur une bannière publicitaire pour pouvoir effectuer une tâche dans votre application, ou concevoir votre application de sorte que les utilisateurs n’aient pas d’autre choix que cliquer sur des bannières publicitaires.

* Contourner le minuteur d’actualisation intégré minimum pour les bannières publicitaires par quelque moyen que ce soit, y compris, mais sans s’y limiter, en remplaçant des objets [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou en actualisant automatiquement une page sans aucune interaction utilisateur.

* Utiliser des unités publicitaires dynamiques (celles provenant du tableau de bord du Centre de développement Windows) pendant les phases de développement et de test, ou dans un émulateur.

* Écrire ou distribuer du code qui appelle des services publicitaires par d’autres moyens que les bibliothèques de publicités Microsoft exécutées dans le contexte de votre application.

* Interagir avec des interfaces non documentées ou des objets enfants créés par les bibliothèques de publicités Microsoft, comme **WebView** ou **MediaElement**.

<span id="interstitialbestpractices10">
## <a name="guidelines-for-interstitial-ads"></a>Recommandations pour les spots publicitaires

Quand ils sont utilisés de façon élégante, les spots publicitaires peuvent augmenter considérablement vos revenus issus de l’application, sans impact négatif sur la satisfaction des utilisateurs. En cas d’utilisation incorrecte, ils peuvent produire l’effet inverse exact.

Les sections suivantes fournissent des recommandations sur la façon d’implémenter des spots vidéo et des bannières publicitaires dans votre application à l’aide d’objets [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx). Elles donnent également des exemples d’implémentations qui ne respectent pas les règles stipulées dans la [politique10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) des Politiques du WindowsStore. Étant donné que vous connaissez votre application mieux que quiconque, sauf en matière de stratégie, nous vous laissons prendre la meilleure décision finale. Ce qu’il faut absolument retenir c’est que les évaluations de vos applications et le montant de vos recettes sont étroitement liés.

### <a name="best-practices"></a>Bonnes pratiques

Nous vous recommandons de suivre ces bonnes pratiques quand vous implémentez des spots publicitaires dans votre application:

* Intégrez des spots publicitaires dans le flux naturel de l’application, par exemple entre les niveaux de jeu.

* Associez des annonces avec des avantages concrets, tels que:

    * Conseils concernant l’achèvement de niveau.

    * Temps supplémentaire pour réessayer un niveau.

    * Fonctionnalités d’avatar personnalisé, par exemple avec un tatouage ou un chapeau.

* Si votre application requiert le visionnage complet d’un spot vidéo, mentionnez cette règle d’emblée afin que les utilisateurs ne soient pas surpris par un message d’erreur en appuyant sur le bouton Fermer.

* Récupérez d’abord la publicité (en appelant la méthode [InterstitialAd.RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx)), dans l’idéal, 30 à 60secondes avant que vous ayez besoin de l’afficher.

* Abonnez-vous aux quatre événements exposés dans la classe [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) (**Canceled**, **Completed**, **AdReady** et **ErrorOccurred**) et utilisez-les pour prendre les bonnes décisions pour votre application.

* Bénéficiez d’une expérience intégrée au lieu d’annonces proposées par le serveur. Vous trouverez cela utile dans certains scénarios:

    * En mode hors connexion, lorsque les serveurs publicitaires sont injoignables.

    * Lorsque l’événement **ErrorOccurred** est déclenché.

    * Si vous choisissez d’économiser la bande passante de l’utilisateur en fonction de [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx), il existe des API dans la classe **ConnectionProfile** qui peuvent vous aider.

* Utilisez le délai d’expiration par défaut (30secondes), sauf si vous avez une raison valable de faire autrement, auquel cas n’allez pas en dessous de 10secondes. Les spots sont beaucoup plus longues à télécharger que les bannières standard, en particulier dans les marchés ne disposant pas de connexions haut débit.

<span/>

* Gardez à l’esprit les forfaits de données des utilisateurs. Par exemple, n’affichez pas de spot vidéo, ou avertissez l’utilisateur avant de le faire sur un appareil mobile approchant de ou ayant dépassé sa limite de données. Il existe des API dans la classe [ConnectionProfile](https://msdn.microsoft.com/library/windows/apps/windows.networking.connectivity.connectionprofile.aspx) qui peuvent vous aider.

* Améliorez en permanence votre application après la soumission initiale. Examinez les [rapports de publicité](../publish/advertising-performance-report.md) et apportez des modifications de conception pour améliorer les taux de remplissage et d’achèvement de spot vidéo.

<span />
### <a name="practices-to-avoid"></a>Pratiques à éviter

Nous vous recommandons d’éviter ces pratiques quand vous implémentez des spots publicitaires dans votre application:

* N’intégrez pas de spots publicitaires de manière excessive dans votre application. N’envoyez pas de publicités à une fréquence supérieure à toutes les 5minutes environ, à moins que l’utilisateur ne bénéficie d’un avantage concret en option, au-delà du jeu.

* N’affichez pas de spots au lancement de l’application, car les utilisateurs pourraient penser qu’ils ont cliqué sur la mauvaise vignette.

* N’affichez pas de spots de sortie. Il s’agit d’un mauvais calcul, car les taux d’achèvement seront proches de zéro.

* N’affichez pas plusieurs spots publicitaires d’affilée. Les utilisateurs seront contrariés de voir la barre de progression de l’annonce revenir au point de départ. Beaucoup penseront qu’il s’agit d’un bogue de codage ou d’affichage de publicité.

* Ne récupérez pas un spot vidéo plus de 5minutes avant d’appeler [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx). Un bon inventaire va optimiser la conversion de publicités récupérées en impressions facturables.

* Ne pénalisez pas un utilisateur pour les défaillances d’affichage de publicités, par exemple quand aucune publicité n’est disponible. Par exemple, si vous affichez une option d’interface utilisateur de type «Regarder une publicité pour obtenir *xxx*», vous devez fournir *xxx* si l’utilisateur a rempli sa part du contrat. Deux options sont à envisager:

    * N’incluez pas l’option, sauf si l’événement [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx) a été déclenché.

    * Proposez une application incluant une expérience intégrée qui procure les mêmes avantages qu’une annonce réelle.

* N’utilisez pas de spots publicitaires permettant à un utilisateur de gagner un avantage compétitif dans un jeu multijoueur. Par exemple, n’affichez pas de spot publicitaire incitant l’utilisateur à se servir d’une meilleure arme dans un jeu de tir. Une chemise personnalisée sur l’avatar du joueur pourrait faire l’affaire, tant qu’elle ne sert pas de camouflage!

<span />
### <a name="examples-of-policy-violations"></a>Exemples de non-respect de la politique

Cette section donne des exemples d’implémentations de spots publicitaires qui ne respectent pas les règles stipulées dans la [politique10.10.1](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) des Politiques du WindowsStore. Ces exemples sont fournis à titre d’information uniquement, pour vous aider à comprendre comment respecter la politique. Ces exemples ne sont pas exhaustifs. Il existe beaucoup d’autres cas possibles de non-respect de la politique10.10.1 qui ne sont pas répertoriés ici.

* Placer un élément d’interface utilisateur sur le conteneur de spot publicitaire.

* Appeler [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) quand l’utilisateur interagit avec l’application.

* Utiliser des spots publicitaires pour obtenir tout ce qui peut être utilisé comme une devise ou échangé avec d’autres utilisateurs.

* Demander un nouveau spot publicitaire dans le contexte du gestionnaire d’événements pour l’événement [InterstitialAd.ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.erroroccurred.aspx). Cela peut produire une boucle infinie et provoquer des problèmes de fonctionnement du service publicitaire.

* Demander un spot publicitaire simplement pour avoir une publicité de secours à une cascade de publicités. Si vous demandez un spot publicitaire et que vous recevez l’événement [InterstitialAd.AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.adready.aspx), le spot publicitaire suivant affiché dans votre application doit être celui qui est prêt à être affiché par la méthode [InterstitialAd.Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx).

* Utiliser des unités publicitaires dynamiques (celles provenant du tableau de bord du Centre de développement Windows) pendant les phases de développement et de test, ou dans un émulateur.

* Écrire ou distribuer du code qui appelle des services publicitaires par d’autres moyens que les bibliothèques de publicités Microsoft exécutées dans le contexte de votre application.

* Interagir avec des interfaces non documentées ou des objets enfants créés par les bibliothèques de publicités Microsoft, comme **WebView** ou **MediaElement**.

 

 
