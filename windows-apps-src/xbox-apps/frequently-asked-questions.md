---
author: Mtoepke
title: Forum Aux Questions
description: Forum aux questions concernant UWP sur Xbox.
translationtype: Human Translation
ms.sourcegitcommit: 07e7ec6a5816d4b8d33322b2b1af05ffc3e1b820
ms.openlocfilehash: 38e5d48b2b0303b5f7d13fdaf6f71e1b3fa56978

---

# Forum Aux Questions

Vous n’obtenez pas les résultats escomptés? Parcourez cette page répertoriant les questions fréquemment posées. Consultez également la rubrique [Problèmes connus](known-issues.md) et le forum [Développement d’applications Windows universelles](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### Pourquoi mes jeux et applications ne fonctionnent-ils pas?

Si vos jeux et applications ne fonctionnent pas ou que vous n’avez pas accès au WindowsStore ou aux services MicrosoftLive, vous travaillez probablement en mode développeur. Le mode développeur est activé si, après avoir sélectionné Accueil, vous voyez apparaître une grande vignette Accueil du développeur sur le côté droit de votre écran, au lieu du contenu Gold/Live habituel. Si vous souhaitez jouer à des jeux, vous pouvez ouvrir l’outil Accueil du développeur et revenir au mode commercial à l’aide du bouton **Quitter le mode développeur**.

### Pourquoi ne puis-je pas me connecter à ma console XboxOne à l’aide de VisualStudio?

Commencez par vérifier que vous travaillez en mode développeur, et non en mode commercial. Vous ne pouvez pas vous connecter à la console XboxOne en mode commercial. Pour connaître le mode actif, il vous suffit d’appuyer sur le bouton **Accueil** et de rechercher la vignette Accueil du développeur sur le côté droit de votre écran. Si cette vignette n’apparaît pas, mais que vous voyez le contenu Gold/Live, vous travaillez en mode commercial. Pour basculer en mode développeur, vous devez exécuter l’application Dev Mode Activation.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application.

Pour plus d’informations, voir [Résolution des problèmes de déploiement](#fixing-deployment-failures) plus loin sur cette page.

### Comment basculer entre les modes commercial et développeur?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes.

### Comment savoir si je suis en mode commercial ou en mode développeur?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes. 

Pour connaître le mode actif, il vous suffit d’appuyer sur le bouton **Accueil** et d’examiner le côté droit de votre écran. Si vous vous trouvez en mode développeur, la vignette Accueil du développeur apparaît sur la droite. Si vous êtes en mode commercial, vous voyez le contenu Gold/Live habituel.

### Mes jeux et applications continueront-ils de fonctionner si j’active le mode développeur?

Oui. Lorsque vous vous trouvez en mode développeur, vous pouvez basculer vers le mode commercial qui vous permet de jouer à vos jeux. Pour plus d’informations, voir la page [Activation du Mode développeur Xbox One](devkit-activation.md). 

### Vais-je perdre mes jeux et applications ou les modifications que j’ai enregistrées?

Si vous décidez de quitter le programme pour les développeurs, vous ne perdrez pas les applications et les jeux que vous avez installés. De plus, si vous avez joué en ligne, tous vos jeux ont été enregistrés sur votre profil cloud de compte Live. Vous ne pouvez donc pas les perdre.

### Comment quitter le programme pour les développeurs?

Pour plus d’informations la manière de quitter le programme pour les développeurs, voir [Désactivation du mode développeur Xbox One](devkit-deactivation.md).

### J’ai vendu ma console XboxOne en la laissant en mode développeur. Comment désactiver le mode développeur?

Si vous n’avez plus accès à votre console XboxOne, vous pouvez la désactiver dans le Centre de développement Windows. Pour plus d’informations, voir la section **Désactiver votre console à l’aide du Centre de développement Windows** dans la rubrique [Désactivation du Mode développeur Xbox One](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center).

### J’ai quitté le programme pour les développeurs à l’aide du Centre de développement Windows, mais je suis toujours en mode développeur. Que dois-je faire?

Démarrez l’outil Accueil du développeur et sélectionnez le bouton **Quitter le mode développeur**. Cette action a pour effet de redémarrer votre console en mode commercial. 

### Puis-je publier mon application?

Vous pouvez [publier des applications](../publish/index.md) par le biais du Centre de développement si vous disposez d’un [compte de développeur](https://developer.microsoft.com/store/register). Les applications UWP créées et testées sur une console XboxOne commerciale feront l’objet des mêmes processus d’ingestion, de révision et de publication que ceux actuellement effectués par Windows, et seront soumises à des vérifications supplémentaires pour répondre aux normes XboxOne en vigueur.

### Puis-je publier mon jeu?

Vous pouvez utiliser UWP et votre console XboxOne en mode développeur pour créer et tester vos jeux sur XboxOne. Pour publier des jeux UWP, vous devez vous inscrire auprès d’[ID@XBOX](http://www.xbox.com/Developers/id). 
[ID@XBOX](http://www.xbox.com/Developers/id) offre aux développeurs un accès complet aux API Xbox Live pour leurs jeux, notamment Scores du joueur et Succès, ainsi que la possibilité de tirer parti du mode multijoueur sur plusieurs appareils, des enregistrements dans le cloud et de toutes les fonctionnalités de Xbox Live sur Xbox One. 
[ID@XBOX](http://www.xbox.com/Developers/id) permet également d’accéder à des kits de développement Xbox One pour les jeux qui sollicitent les capacités maximales du matériel Xbox One.

### Les moteurs de jeu standard fonctionneront-ils?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### Quelles sont les fonctionnalités et ressources système disponibles pour les jeux UWP sur XboxOne? 

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### Si je crée un jeu UWP DirectX12 , s’exécutera-t-il sur ma console XboxOne en mode développeur?

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### La surface d’API UWP sera-t-elle disponible en totalité sur Xbox?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version.

### Résolution des problèmes de déploiement

Si vous ne parvenez pas à déployer votre application à partir de VisualStudio, les étapes suivantes peuvent vous aider à résoudre le problème. Si vous êtes bloqué, demandez de l’aide sur le forum.

> [!NOTE]
> Un utilisateur doit être connecté afin de pouvoir déployer une application. Si vous recevez un message d’erreur 0x87e10008, assurez-vous qu’un utilisateur est connecté, puis essayez de nouveau.

Si VisualStudio ne peut pas se connecter à votre console XboxOne:

1. Assurez-vous que vous êtes en mode développeur (comme décrit précédemment sur cette page).
2. Vérifiez que vous avez correctement configuré votre PC de développement. Avez-vous suivi *toutes* les instructions de la rubrique [Prise en main du développement d’applications UWP sur Xbox One](getting-started.md) ? 

3. Si vous ne l’avez pas encore fait, lisez les rubriques [Configuration de l’environnement de développement](development-environment-setup.md) et [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

4. Vérifiez que vous pouvez «effectuer un test ping» sur l’adresseIP de votre console à partir de votre PC de développement.
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

### Si je crée une application en HTML/JavaScript, comment puis-je activer la navigation par boîtier de commande?

TVHelpers est un ensemble d’exemples et de bibliothèques JavaScript et XAML/C# conçu pour vous aider à créer des applications réussies pour XboxOne et la télévision en JavaScript et C#. TVJS est une bibliothèque qui vous permet de créer des applications UWP de grande qualité pour XboxOne. TVJS inclut la prise en charge de la navigation automatique par manette, la lecture de contenus multimédias enrichis, la recherche et plus encore. Vous pouvez utiliser TVJS avec votre application web hébergée tout aussi facilement qu’avec une application UWP web empaqueté avec accès complet aux API WindowsRuntime.

Pour plus d’informations, voir le projet [TVHelpers](https://github.com/Microsoft/TVHelpers) et le projet [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## Voir également
- [Problèmes connus avec UWP sur XboxOne](known-issues.md)
- [UWP sur XboxOne](index.md)



<!--HONumber=Aug16_HO5-->


