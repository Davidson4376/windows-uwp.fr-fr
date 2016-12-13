---
author: Mtoepke
title: Forum Aux Questions
description: Forum aux questions concernant UWP sur Xbox.
translationtype: Human Translation
ms.sourcegitcommit: 1ee75d73cc42455677bc2c0df08b41f33fc4f7b0
ms.openlocfilehash: a6d410e3a4873bb96fba46789b28185c392a7492

---

# <a name="frequently-asked-questions"></a>Forum Aux Questions

Vous n’obtenez pas les résultats escomptés&nbsp;? Parcourez cette page répertoriant les questions fréquemment posées. Consultez également la rubrique [Problèmes connus](known-issues.md) et le forum [Développement d’applications Windows universelles](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### <a name="why-are-my-games-and-apps-not-working"></a>Pourquoi mes jeux et applications ne fonctionnent-ils pas&nbsp;?

Si vos jeux et applications ne fonctionnent pas ou que vous n’avez pas accès au Windows&nbsp;Store ou aux services Microsoft&nbsp;Live, vous travaillez probablement en mode développeur. Le mode développeur est activé si, après avoir sélectionné Accueil, vous voyez apparaître une grande vignette Accueil du développeur sur le côté droit de votre écran, au lieu du contenu Gold/Live habituel. Si vous souhaitez jouer à des jeux, vous pouvez ouvrir l’outil Accueil du développeur et revenir au mode commercial à l’aide du bouton **Quitter le mode développeur**.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Pourquoi ne puis-je pas me connecter à ma console Xbox&nbsp;One à l’aide de Visual&nbsp;Studio&nbsp;?

Commencez par vérifier que vous travaillez en mode développeur, et non en mode commercial. Vous ne pouvez pas vous connecter à la console Xbox&nbsp;One en mode commercial. Pour connaître le mode actif, il vous suffit d’appuyer sur le bouton **Accueil** et de rechercher la vignette Accueil du développeur sur le côté droit de votre écran. Si cette vignette n’apparaît pas, mais que vous voyez le contenu Gold/Live, vous travaillez en mode commercial. Pour basculer en mode développeur, vous devez exécuter l’application Dev Mode Activation.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application.

Pour plus d’informations, voir [Résolution des problèmes de déploiement](#fixing-deployment-failures) plus loin sur cette page.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>Comment basculer entre les modes commercial et développeur&nbsp;?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>Comment savoir si je suis en mode commercial ou en mode développeur&nbsp;?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes. 

Pour connaître le mode actif, il vous suffit d’appuyer sur le bouton **Accueil** et d’examiner le côté droit de votre écran. Si vous vous trouvez en mode développeur, la vignette Accueil du développeur apparaît sur la droite. Si vous êtes en mode commercial, vous voyez le contenu Gold/Live habituel.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Mes jeux et applications continueront-ils de fonctionner si j’active le mode développeur&nbsp;?

Oui. Lorsque vous vous trouvez en mode développeur, vous pouvez basculer vers le mode commercial qui vous permet de jouer à vos jeux. Pour plus d’informations, voir la page [Activation du Mode développeur Xbox One](devkit-activation.md). 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Vais-je perdre mes jeux et applications ou les modifications que j’ai enregistrées&nbsp;?

Si vous décidez de quitter le programme pour les développeurs, vous ne perdrez pas les applications et les jeux que vous avez installés. De plus, si vous avez joué en ligne, tous vos jeux ont été enregistrés sur votre profil cloud de compte Live. Vous ne pouvez donc pas les perdre.

### <a name="how-do-i-leave-the-developer-program"></a>Comment quitter le programme pour les développeurs&nbsp;?

Pour plus d’informations la manière de quitter le programme pour les développeurs, voir [Désactivation du mode développeur Xbox One](devkit-deactivation.md).

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>J’ai vendu ma console Xbox&nbsp;One en la laissant en mode développeur. Comment désactiver le mode développeur&nbsp;?

Si vous n’avez plus accès à votre console Xbox&nbsp;One, vous pouvez la désactiver dans le Centre de développement Windows. Pour plus d’informations, voir la section **Désactiver votre console à l’aide du Centre de développement Windows** dans la rubrique [Désactivation du Mode développeur Xbox One](devkit-deactivation.md#deactivate-your-console-using-windows-dev-center). 

### <a name="i-left-the-developer-program-using-windows-dev-center-but-im-in-still-developer-mode-what-do-i-do"></a>J’ai quitté le programme pour les développeurs à l’aide du Centre de développement Windows, mais je suis toujours en mode développeur. Que dois-je faire&nbsp;?

Démarrez l’outil Accueil du développeur et sélectionnez le bouton **Quitter le mode développeur**. Cette action a pour effet de redémarrer votre console en mode commercial. 

### <a name="can-i-publish-my-app"></a>Puis-je publier mon application&nbsp;?

Vous pouvez [publier des applications](../publish/index.md) par le biais du Centre de développement si vous disposez d’un [compte de développeur](https://developer.microsoft.com/store/register). Les applications UWP créées et testées sur une console Xbox&nbsp;One commerciale feront l’objet des mêmes processus d’ingestion, de révision et de publication que ceux actuellement effectués par Windows, et seront soumises à des vérifications supplémentaires pour répondre aux normes Xbox&nbsp;One en vigueur.

### <a name="can-i-publish-my-game"></a>Puis-je publier mon jeu&nbsp;?

Vous pouvez utiliser UWP et votre console Xbox&nbsp;One en mode développeur pour créer et tester vos jeux sur Xbox&nbsp;One. Pour publier des jeux UWP, vous devez vous inscrire auprès d’[ID@XBOX](http://www.xbox.com/Developers/id). 
[ID@XBOX](http://www.xbox.com/Developers/id) offre aux développeurs un accès complet aux API Xbox Live pour leurs jeux, notamment Scores du joueur et Succès, ainsi que la possibilité de tirer parti du mode multijoueur sur plusieurs appareils, des enregistrements dans le cloud et de toutes les fonctionnalités de Xbox Live sur Xbox One. 
[ID@XBOX](http://www.xbox.com/Developers/id) permet également d’accéder à des kits de développement Xbox One pour les jeux qui sollicitent les capacités maximales du matériel Xbox One.

### <a name="will-the-standard-game-engines-work"></a>Les moteurs de jeu standard fonctionneront-ils&nbsp;?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Quelles sont les fonctionnalités et ressources système disponibles pour les jeux UWP sur Xbox&nbsp;One&nbsp;? 

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>Si je crée un jeu UWP DirectX&nbsp;12 , s’exécutera-t-il sur ma console Xbox&nbsp;One en mode développeur&nbsp;?

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>La surface d’API UWP sera-t-elle disponible en totalité sur Xbox&nbsp;?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### <a name="fixing-deployment-failures"></a>Résolution des problèmes de déploiement

Si vous ne parvenez pas à déployer votre application à partir de Visual&nbsp;Studio, les étapes suivantes peuvent vous aider à résoudre le problème. Si vous êtes bloqué, demandez de l’aide sur le forum.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application. Si vous recevez un message d’erreur 0x87e10008, assurez-vous qu’un utilisateur est connecté, puis essayez de nouveau.

Si Visual&nbsp;Studio ne peut pas se connecter à votre console Xbox&nbsp;One&nbsp;:

1. Assurez-vous que vous êtes en mode développeur (comme décrit précédemment sur cette page).
2. Vérifiez que vous avez correctement configuré votre PC de développement. Avez-vous suivi *toutes* les instructions de la rubrique [Prise en main du développement d’applications UWP sur Xbox One](getting-started.md) ? 

3. Si vous ne l’avez pas encore fait, lisez les rubriques [Configuration de l’environnement de développement](development-environment-setup.md) et [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

4. Vérifiez que vous pouvez «&nbsp;effectuer un test ping&nbsp;» sur l’adresse&nbsp;IP de votre console à partir de votre PC de développement.
  > [!NOTE]
  > Pour des performances de déploiement optimales, nous vous recommandons d’utiliser une connexion câblée à votre console.

5. Assurez-vous que vous utilisez l’option Universel (protocole non chiffré) dans la liste déroulante Authentification de l’onglet **Déboguer**. Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md).

<!--6. Make sure you are not hitting a PIN pairing issue; see "Visual Studio/Xbox PIN pairing failures" in the [Known Issues](known-issues.md) topic.-->

<!--
If Visual Studio can connect, but deployment is failing (for example you get this error message: "DEP0700 : Registration of the app failed.(0x80073cf9)"):

1. Make sure that your app is not installed by uninstalling it from the Collections app in the Xbox One shell. 

> **Note**&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

2. If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode, and then switch back to Developer Mode. 
This will clear Dev Storage.

3. If your issues persist, follow the steps above and then use **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content and deactivate Developer Mode.
-->

### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Si je crée une application en HTML/JavaScript, comment puis-je activer la navigation par boîtier de commande&nbsp;?

TVHelpers est un ensemble d’exemples et de bibliothèques JavaScript et XAML/C# conçu pour vous aider à créer des applications réussies pour Xbox&nbsp;One et la télévision en JavaScript et C#. TVJS est une bibliothèque qui vous permet de créer des applications UWP de grande qualité pour Xbox&nbsp;One. TVJS inclut la prise en charge de la navigation automatique par manette, la lecture de contenus multimédias enrichis, la recherche et plus encore. Vous pouvez utiliser TVJS avec votre application web hébergée tout aussi facilement qu’avec une application UWP web empaqueté avec accès complet aux API Windows&nbsp;Runtime.

Pour plus d’informations, voir le projet [TVHelpers](https://github.com/Microsoft/TVHelpers) et le projet [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## <a name="see-also"></a>Voir également
- [Problèmes connus avec UWP sur Xbox&nbsp;One](known-issues.md)
- [UWP sur Xbox&nbsp;One](index.md)



<!--HONumber=Dec16_HO1-->


