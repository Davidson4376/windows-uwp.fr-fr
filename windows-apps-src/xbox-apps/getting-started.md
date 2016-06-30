---
author: Mtoepke
title: "Prise en main du développement d’applications UWP sur Xbox One"
description: "Procédure de configuration de votre PC et de votre console Xbox One pour le développement UWP."
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: b070b6bec350c3c893f934e212b1de743b349861

---

#Prise en main du développement d’applications UWP sur Xbox One

Pour configurer correctement votre PC et votre console Xbox One pour le développement UWP, suivez **scrupuleusement** la procédure ci-après. Après avoir configuré tous les paramètres, vous pourrez vous familiariser avec le mode développeur sur Xbox One et avec la création d’applications UWP en consultant la page [UWP pour Xbox One](index.md). 

## Avant de commencer
Avant de commencer, vous devez effectuer les opérations suivantes :
-   Créez un compte du [Centre de développement Windows](https://dev.windows.com).
-   Inscrivez-vous au [Programme Windows Insider](https://insider.windows.com/). Vous devrez effectuer cette opération pour obtenir la version d’évaluation du SDK Windows.
-   Configurez un PC Windows 10 (toute version conviendra, y compris la dernière version d’évaluation Windows 10 Insider). Dans le cadre de cette version préliminaire, nos outils de développement nécessitent l’exécution de Windows 10. 
-   Connectez votre console Xbox One à un réseau. Pour des performances optimales, utilisez une connexion câblée.
- Vérifiez que vous disposez d’au moins 5 Go d’espace libre sur votre console Xbox One.

## Configuration de votre PC de développement
1.  Installez Visual Studio 2015 Update 2. Veillez à choisir l’installation **Personnalisée** et à cocher la case **Outils de développement d’applications Windows universelles**, car ces outils ne font pas partie intégrante de l’installation par défaut. Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md) (si vous êtes un développeur C++, prenez soin de choisir l’installation personnalisée et de sélectionner également C++).

2.  Installez la dernière version d’évaluation du Kit de développement logiciel (SDK) Windows 10. Vous pouvez l’obtenir à partir du [Programme Windows Insider](http://go.microsoft.com/fwlink/p/?LinkId=780552).
  
  > **Important** &nbsp;&nbsp;L’installation de cette version d’évaluation du SDK sur votre PC n’autorisera pas la soumission d’applications créées sur ce PC au Windows Store ; par conséquent, n’installez pas cette version sur votre PC de développement de production. 

## Configuration de votre console Xbox One
1.  Activez le mode développeur sur votre console Xbox One. Téléchargez l’application, obtenez le code d’activation, puis entrez-le sur la page xboxactivate de votre compte du Centre de développement. Pour plus d’informations, voir [Activation du Mode développeur sur Xbox One](devkit-activation.md). 

2.  Attendez que votre console Xbox One effectue une mise à jour système de façon à exécuter la version préliminaire pour développeurs. Ce processus peut nécessiter jusqu’à 4 heures. Si vous ne souhaitez pas attendre, effectuez un redémarrage matériel de votre console en appuyant longuement sur le bouton d’alimentation pendant 10 secondes, puis en rallumant la console. Cette opération déclenchera la mise à jour.  

3.  Accédez à l’application Dev Mode Activation, puis sélectionnez **Basculer et redémarrer**. Félicitations, vous disposez maintenant d’une console Xbox One en mode développeur !
  
  > **Remarque** &nbsp;&nbsp;Vos applications et jeux commerciaux ne s’exécuteront pas en mode développeur, mais les applications ou jeux que vous créerez le feront sans problème. Pour exécuter vos applications et jeux favoris, rebasculez en mode commercial.
  
  > **Remarque** &nbsp;&nbsp;Avant de pouvoir déployer une application vers votre Xbox One en Mode développeur, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte Xbox Live existant ou créer un compte pour votre console en Mode développeur. 

## Création de votre premier projet dans Visual Studio 2015

Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md).

1.  Pour C# : créez un projet Windows universel, accédez aux propriétés du projet, sélectionnez l’onglet **Déboguer**, remplacez **Appareil cible** par **Ordinateur distant**, tapez l’adresse IP ou le nom d’hôte de votre console Xbox One dans le champ **Ordinateur distant**, puis sélectionnez **Universel (protocole non chiffré)** dans la liste déroulante **Mode d’authentification**.   

    Pour trouver l’adresse IP de votre console Xbox One, démarrez l’outil Accueil du développeur sur votre console (grande vignette figurant sur le côté droit de l’écran d’accueil) et examinez le coin supérieur gauche de l’écran. Pour plus d’informations sur l’outil Accueil du développeur, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).  

2.  Pour les projets C++ et HTML/Javascript : suivez une procédure similaire, mais dans les propriétés du projet, accédez à l’onglet **Débogage**, sélectionnez **Ordinateur distant** dans le Débogueur pour afficher la liste déroulante, tapez l’adresse IP ou le nom d’hôte de la console dans le champ **Nom de l’ordinateur**, puis sélectionnez **Universel (protocole non chiffré)** dans le champ **Type d’authentification**.
   
3.  Lorsque vous appuyez sur F5, votre application est générée et commence à se déployer sur votre console Xbox One.
  
4.  La première fois que vous effectuez cette opération, Visual Studio vous demande d’entrer un code confidentiel pour votre console Xbox One. Pour obtenir un code confidentiel, démarrez l’outil Accueil du développeur sur votre console Xbox One, puis appuyez sur le bouton **Jumeler avec Visual Studio**.
  
5.  Une fois cette opération effectuée, votre application commence à se déployer. La première exécution de cette procédure peut se révéler un peu lente (nous devons copier la totalité des outils sur votre console Xbox), mais ne devrait pas prendre plus de quelques minutes ; dans le cas contraire, un problème est probablement survenu. Vérifiez que vous avez suivi toutes les étapes ci-dessus (notamment que vous avez défini le champ **Mode d’authentification** sur **Universel**) et que vous utilisez une connexion réseau câblée à votre console Xbox One.  

6. Il ne vous reste plus qu’à vous détendre et à profiter de votre première application exécutée sur la console.  
   ![Hello World](images/getting-started-hello-world.png)
   

## Voir aussi  
- [Forum Aux Questions](frequently-asked-questions.md)  
- [Problèmes connus](known-issues.md)
- [UWP sur Xbox One](index.md)



<!--HONumber=Jun16_HO4-->


