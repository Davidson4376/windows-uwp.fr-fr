---
title: Forum Aux Questions
description: Forum aux questions concernant UWP sur Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 38b5144d9b70e6e2825ef11a64be8873ec5b7d63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614554"
---
# <a name="frequently-asked-questions"></a>Forum Aux Questions

Vous n’obtenez pas les résultats escomptés ? Parcourez cette page répertoriant les questions fréquemment posées. Consultez également la rubrique [Problèmes connus](known-issues.md) et le forum [Développement d’applications Windows universelles](https://go.microsoft.com/fwlink/?linkid=839446). 

### <a name="why-arent-my-games-and-apps-working"></a>Pourquoi mes jeux et mes applications ne fonctionnent-ils pas ?

Si vos jeux et vos applications ne fonctionnent pas, ou si vous ne pouvez pas accéder au Store ou aux services Microsoft Live, vous travaillez probablement en mode développeur. Pour déterminer le mode sous lequel vous travaillez, appuyez sur la touche **Accueil** de votre manette. Si cette opération vous ramène à l’outil Accueil du développeur à la place de la page d’accueil de vente au détail, cela signifie que vous êtes en mode développeur. Si vous souhaitez jouer à des jeux, vous pouvez ouvrir l’outil Accueil du développeur et revenir au mode commercial à l’aide du bouton **Quitter le mode développeur**.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Pourquoi ne puis-je pas me connecter à ma console Xbox One à l’aide de Visual Studio ?

Commencez par vérifier que vous travaillez en mode développeur, et non en mode commercial. Vous ne pouvez pas vous connecter à la console Xbox One en mode commercial. Pour déterminer le mode sous lequel vous travaillez, appuyez sur la touche **Accueil** de votre manette. Si vous voyez le contenu Gold/Live à la place de l’outil Accueil du développeur, cela signifie que vous vous trouvez en Mode commercial. Vous devez exécuter l’application Dev Mode Activation pour passer en Mode développeur.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application.

Pour plus d’informations, voir [Résolution des problèmes de déploiement](#fixing-deployment-failures) plus loin sur cette page.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>Comment basculer entre les modes commercial et développeur ?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>Comment savoir si je suis en mode commercial ou en mode développeur ?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes. 

Pour déterminer le mode sous lequel vous travaillez, appuyez sur la touche **Accueil** de votre manette. 
- Si vous êtes redirigé vers l’Accueil du développeur, vous êtes en Mode développeur.
- Si vous voyez le contenu Gold/Live, vous êtes en Mode commercial.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Mes jeux et applications continueront-ils de fonctionner si j’active le mode développeur ?

Oui. Lorsque vous vous trouvez en mode développeur, vous pouvez basculer vers le mode commercial qui vous permet de jouer à vos jeux. Pour plus d’informations, voir la page [Activation du Mode développeur Xbox One](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Puis-je développer et publier des applications x86 pour Xbox ?
Xbox ne prend plus en charge le développement d’applications x86 ni les soumissions d’applications x86 dans le Store. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Vais-je perdre mes jeux et applications ou les modifications que j’ai enregistrées ?

Si vous décidez de quitter le programme pour les développeurs, vous ne perdrez pas les applications et les jeux que vous avez installés. De plus, tant que vous êtes connecté en ligne lorsque vous jouez à vos jeux, toutes vos sauvegardes sont enregistrées sur le profil cloud de votre compte Live. Vous ne pourrez donc pas les perdre.

### <a name="how-do-i-leave-the-developer-program"></a>Comment quitter le programme pour les développeurs ?

Pour plus d’informations la manière de quitter le programme pour les développeurs, voir [Désactivation du mode développeur Xbox One](devkit-deactivation.md).

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>J’ai vendu ma console Xbox One en la laissant en mode développeur. Comment désactiver le mode développeur ?

Si vous n’avez plus accès à votre Xbox One, vous pouvez le désactiver dans Windows Partner Center. Pour plus d’informations, consultez le **désactiver votre console à l’aide de partenaires** section dans le [une désactivation de Mode développeur Xbox](devkit-deactivation.md#deactivate-your-console-using-partner-center) rubrique. 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>J’ai laissé le programme de développeurs à l’aide de partenaires, mais je suis toujours le mode développeur. Que dois-je faire ?

Démarrez l’outil Accueil du développeur et sélectionnez le bouton **Quitter le mode développeur**. Cette action a pour effet de redémarrer votre console en mode commercial. 

### <a name="can-i-publish-my-app"></a>Puis-je publier mon application ?

Vous pouvez [publier des applications](../publish/index.md) via des partenaires si vous avez un [compte de développeur](https://developer.microsoft.com/store/register). Les applications UWP créées et testées sur une console Xbox One commerciale feront l’objet des mêmes processus d’ingestion, de révision et de publication que ceux actuellement effectués par Windows. Elles seront également soumises à des vérifications supplémentaires pour répondre aux normes Xbox One en vigueur.

### <a name="can-i-publish-my-game"></a>Puis-je publier mon jeu ?

Vous pouvez utiliser UWP et votre console Xbox One en mode développeur pour créer et tester vos jeux sur Xbox One. Pour publier des jeux UWP, vous devez vous enregistrer auprès de [ID@XBOX](https://www.xbox.com/Developers/id) ou intégrer le [Programme de créateurs de Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator). Pour plus d’informations, voir la page [Vue d’ensemble du programme pour les développeurs](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>Les moteurs de jeu standard fonctionneront-ils ?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Quelles sont les fonctionnalités et ressources système disponibles pour les jeux UWP sur Xbox One ? 

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>Si je crée un jeu UWP DirectX 12 , s’exécutera-t-il sur ma console Xbox One en mode développeur ?

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>La surface d’API UWP sera-t-elle disponible en totalité sur Xbox ?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### <a name="fixing-deployment-failures"></a>Résolution des problèmes de déploiement

Si vous ne parvenez pas à déployer votre application à partir de Visual Studio, ces étapes peuvent vous aider à résoudre le problème. Si vous êtes bloqué, demandez de l’aide sur le forum.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application. Si vous recevez un message d’erreur 0x87e10008, assurez-vous qu’un utilisateur est connecté, puis essayez de nouveau.

Si Visual Studio ne peut pas se connecter à votre console Xbox One :

1. Assurez-vous que vous êtes en mode développeur (comme décrit précédemment sur cette page).
2. Vérifiez que vous avez correctement configuré votre PC de développement. Avez-vous suivi *toutes* les instructions de la rubrique [Prise en main du développement d’applications UWP sur Xbox One](getting-started.md) ? 

3. Si vous ne l’avez pas encore fait, lisez les rubriques [Configuration de l’environnement de développement](development-environment-setup.md) et [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

4. Vérifiez que vous pouvez « effectuer un test ping » sur l’adresse IP de votre console à partir de votre PC de développement.
  > [!NOTE]
  > Pour des performances de déploiement optimales, nous vous recommandons d’utiliser une connexion câblée à votre console.

5. Assurez-vous que vous utilisez l’option Universel (protocole non chiffré) dans la liste déroulante Authentification de l’onglet **Déboguer**. Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Si je crée une application en HTML/JavaScript, comment puis-je activer la navigation par boîtier de commande ?

TVHelpers est un ensemble d’exemples et de bibliothèques JavaScript et XAML/C# pour vous aider à créer des applications réussies pour Xbox One et la télévision en JavaScript et C#. TVJS est une bibliothèque qui vous permet de créer des applications UWP de grande qualité pour Xbox One. TVJS inclut la prise en charge de la navigation automatique sur manette, la lecture de contenus multimédias riches, la recherche et plus encore. Vous pouvez utiliser TVJS avec votre application web hébergée tout aussi facilement qu’avec une application UWP web empaqueté avec accès complet aux API Windows Runtime.

Pour plus d’informations, voir le projet [TVHelpers](https://github.com/Microsoft/TVHelpers) et le projet [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## <a name="see-also"></a>Voir également
- [Problèmes connus avec UWP sur Xbox One](known-issues.md)
- [UWP sur Xbox One](index.md)
- [UWP sur Xbox One](index.md)
