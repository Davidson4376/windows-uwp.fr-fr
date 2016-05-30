---
author: Mtoepke
title: Forum Aux Questions
description: Forum aux questions concernant UWP sur Xbox.
area: Xbox
---

# Forum Aux Questions

Vous n’obtenez pas les résultats escomptés ? 
Parcourez cette page répertoriant les questions fréquemment posées. 
Consultez également la rubrique [Problèmes connus](known-issues.md) et le forum [Développement d’applications Windows universelles](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### Pourquoi mes jeux et applications ne fonctionnent-ils pas ?

Si vos jeux et applications ne fonctionnent pas ou que vous n’avez pas accès au Windows Store ou aux services Microsoft Live, vous travaillez probablement en mode développeur. 
Le mode développeur est activé si, après avoir sélectionné Accueil, vous voyez apparaître une grande vignette Accueil du développeur sur le côté droit de votre écran, au lieu du contenu Gold/Live habituel. 
Si vous souhaitez jouer à des jeux, vous pouvez ouvrir l’outil Accueil du développeur et revenir au mode commercial à l’aide du bouton **Quitter le mode développeur**.

### Pourquoi ne puis-je pas me connecter à ma console Xbox One à l’aide de Visual Studio ?

Commencez par vérifier que vous travaillez en mode développeur, et non en mode commercial. 
Vous ne pouvez pas vous connecter à la console Xbox One en mode commercial. 
Pour connaître le mode actif, il vous suffit d’appuyer sur le bouton **Accueil** et de rechercher la vignette Accueil du développeur sur le côté droit de votre écran. 
Si cette vignette n’apparaît pas, mais que vous voyez le contenu Gold/Live, vous travaillez en mode commercial. 
Pour basculer en mode développeur, vous devez exécuter l’application Dev Mode Activation.

Pour plus d’informations, voir [Résolution des problèmes de déploiement](frequently-asked-questions.md#fixing-deployment-failures) plus loin sur cette page.

### Comment basculer entre les modes commercial et développeur ?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes.

### Comment savoir si je suis en mode commercial ou en mode développeur ?

Suivez les instructions de la rubrique [Activation du Mode développeur Xbox One](devkit-activation.md) pour obtenir une description de ces deux modes. 

Pour connaître le mode actif, il vous suffit d’appuyer sur le bouton **Accueil** et d’examiner le côté droit de votre écran. 
Si vous vous trouvez en mode développeur, la vignette Accueil du développeur apparaît sur la droite. 
Si vous êtes en mode commercial, vous voyez le contenu Gold/Live habituel.

### Mes jeux et applications continueront-ils de fonctionner si j’active le mode développeur ?

Oui. Lorsque vous vous trouvez en mode développeur, vous pouvez basculer vers le mode commercial qui vous permet de jouer à vos jeux. 
Pour plus d’informations, voir la page [Activation du Mode développeur Xbox One](devkit-activation.md). 

> **ATTENTION** &nbsp;&nbsp;La Mise à jour système de la version préliminaire pour développeurs du programme Xbox inclut des versions logicielles préliminaires anticipées et expérimentales. 
Cela signifie que certains jeux et applications courants ne fonctionneront pas comme prévu, et que vous risquez d’être confronté à des blocages occasionnels et à une perte de données.

### Vais-je perdre mes jeux et applications ou les modifications que j’ai enregistrées ?

Si vous décidez de quitter la version préliminaire pour développeurs, vous pourrez avoir besoin de procéder à une réinitialisation aux paramètres d’usine, ce qui effacera la totalité du contenu de votre console. 
Si cela se produit, vous devrez réinstaller tous les jeux et applications. 
Si vous avez joué en ligne, tous vos jeux ont été enregistrés sur votre profil cloud de compte Live, de sorte que vous n’avez pas pu les perdre.

### Comment quitter la version préliminaire pour développeurs ?

Pour plus d’informations sur la procédure permettant de quitter la version préliminaire pour développeurs, voir la rubrique [Désactivation du Mode développeur Xbox One](devkit-deactivation.md).

### J’ai vendu ma console Xbox One en la laissant en mode développeur. Comment désactiver le mode développeur ?

Si vous n’avez plus accès à votre console Xbox One, vous pouvez la désactiver dans le Centre de développement Windows. 
Pour plus d’informations, voir la section **Désactiver votre console à l’aide du Centre de développement Windows** dans la rubrique [Désactivation du Mode développeur Xbox One](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center).

### J’ai quitté la version préliminaire pour développeurs à l’aide du Centre de développement Windows, mais je suis toujours en mode développeur. Que dois-je faire ?

Démarrez l’outil Accueil du développeur et sélectionnez le bouton **Quitter le mode développeur**. 
Cette action a pour effet de redémarrer votre console en mode commercial. 

### Puis-je publier mon application ?

La publication d’applications sera disponible par le biais du Centre de développement plus tard dans l’année. 
Les applications UWP créées et testées sur une console Xbox One commerciale feront l’objet des mêmes processus d’ingestion, de révision et de publication que ceux actuellement effectués par Windows, et seront soumises à des vérifications supplémentaires pour répondre aux normes Xbox One en vigueur.

### Puis-je publier mon jeu ?

Vous pouvez utiliser UWP et votre console Xbox One en mode développeur pour créer et tester vos jeux sur Xbox One. 
Pour publier des jeux UWP, vous devez vous inscrire auprès d’[ID@XBOX](http://www.xbox.com/en-us/Developers/id). 
[ID@XBOX](http://www.xbox.com/en-us/Developers/id) offre aux développeurs un accès complet aux API Xbox Live pour leurs jeux, notamment Scores du joueur et Succès, ainsi que la possibilité de tirer parti du mode multijoueur sur plusieurs appareils, des enregistrements dans le cloud et de toutes les fonctionnalités de Xbox Live sur Xbox One. 
[ID@XBOX](http://www.xbox.com/en-us/Developers/id) permet également d’accéder à des kits de développement Xbox One pour les jeux qui sollicitent les capacités maximales du matériel Xbox One.

### Les moteurs de jeu standard fonctionneront-ils ?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version préliminaire.

### Quelles sont les fonctionnalités et ressources système disponibles pour les jeux UWP sur Xbox One ? 

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### Si je crée un jeu UWP DirectX 12 , s’exécutera-t-il sur ma console Xbox One en mode développeur ?

Pour plus d’informations, voir [Ressources système pour les applications et jeux UWP sur Xbox One](system-resource-allocation.md).

### La surface d’API UWP sera-t-elle disponible en totalité sur Xbox ?

Consultez la page [Problèmes connus](known-issues.md) concernant cette version préliminaire.

### Résolution des problèmes de déploiement

Si vous ne parvenez pas à déployer votre application à partir de Visual Studio, les étapes suivantes peuvent vous aider à résoudre le problème. 
Si vous êtes bloqué, demandez de l’aide sur le forum.

Si Visual Studio ne peut pas se connecter à votre console Xbox One :

1. Assurez-vous que vous êtes en mode développeur (comme décrit précédemment sur cette page).
2. Vérifiez que vous avez correctement configuré votre PC de développement. Avez-vous suivi *toutes* les instructions de la rubrique [Prise en main du développement d’applications UWP sur Xbox One](getting-started.md) ? 

3. Si vous ne l’avez pas encore fait, lisez les rubriques [Configuration de l’environnement de développement](development-environment-setup.md) et [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

4. Assurez-vous que vous utilisez une connexion réseau câblée à Xbox One. Nous avons constaté des problèmes de performances et de connectivité avec certains points de terminaison sans fil.

5. Vérifiez que vous pouvez « effectuer un test ping » sur l’adresse IP de votre console à partir de votre PC de développement.

6. Assurez-vous que vous utilisez l’option Universel (protocole non chiffré) dans la liste déroulante Authentification de l’onglet **Déboguer**. Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md).

7. Vérifiez que vous ne rencontrez pas un problème de couplage de code confidentiel ; consultez la section « Échecs de couplage de code confidentiel Visual Studio/Xbox » dans la rubrique [Problèmes connus](frequently-asked-questions.md).

Si Visual Studio peut se connecter, mais que le déploiement échoue (par exemple, vous obtenez le message d’erreur : « DEP0700 : Échec de l’inscription de l’application.(0x80073cf9) ») :

1. Assurez-vous que votre application n’est pas installée en la désinstallant à partir de l’application Collections dans l’interpréteur de commandes Xbox One. 

> **Remarque** &nbsp;&nbsp;La désinstallation de votre application à partir de Windows Device Portal (WDP) ne résoudra pas le problème.

2. Si vos problèmes persistent, désinstallez votre application ou jeu dans l’application Collections, quittez le mode développeur, redémarrez en mode commercial, puis rebasculez en mode développeur. 
Cette procédure effacera le stockage de développement.

3. Si vos problèmes persistent, suivez les étapes ci-dessus, puis utilisez **Réinitialiser et conserver mes jeux et applications** pour supprimer tous les états stockés sur votre console Xbox One. 
Accédez à Paramètres &gt; Système &gt; Informations et mise à jour de la console &gt; Réinitialiser la console, puis sélectionnez le bouton **Réinitialiser et conserver mes jeux et applications**.

> **Attention** &nbsp;&nbsp;Cette opération supprimera tous les paramètres enregistrés sur votre console Xbox One, notamment les paramètres sans fil, les comptes d’utilisateurs et tous les états d’avancement de jeu non enregistrés dans le stockage cloud.

> **Attention** &nbsp;&nbsp;Ne sélectionnez PAS le bouton **Réinitialiser et supprimer tous les éléments**.
Cette opération supprimera tous vos jeux, applications, paramètres et contenus, désactivera le mode développeur et supprimera votre console du groupe Developer Preview.

### Si je crée une application en HTML/JavaScript, comment puis-je activer la navigation par boîtier de commande ?

TVHelpers est un ensemble d’exemples et de bibliothèques JavaScript et XAML/C# conçu pour vous aider à créer des applications réussies pour Xbox One et la télévision en JavaScript et C#. 
TVJS est une bibliothèque qui vous permet de créer des applications UWP de grande qualité pour Xbox One. TVJS inclut la prise en charge de la navigation automatique par manette, la lecture de contenus multimédias enrichis, la recherche et plus encore. 
Vous pouvez utiliser TVJS avec votre application web hébergée tout aussi facilement qu’avec une application UWP web empaquetée avec accès complet aux API Windows Runtime.

Pour plus d’informations, voir le projet [TVHelpers](https://github.com/Microsoft/TVHelpers) et le projet [Wiki](https://github.com/Microsoft/TVHelpers/wiki).

## Voir aussi
- [Problèmes connus avec UWP sur la version préliminaire pour développeurs de Xbox One](known-issues.md)
- [UWP sur Xbox One](index.md)


<!--HONumber=May16_HO2-->


