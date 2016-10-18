---
author: Mtoepke
title: "Prise en main du développement d’applications UWP sur XboxOne"
description: "Procédure de configuration de votre PC et de votre console XboxOne pour le développement UWP."
translationtype: Human Translation
ms.sourcegitcommit: d4ef0da606c98c5eb024f349720fe9641e4a541e
ms.openlocfilehash: 33b8369be9cc0fd54ee044ec2aa6ea38b352c908

---

#Prise en main du développement d’applications UWP sur XboxOne

Pour configurer correctement votre PC et votre console Xbox One pour le développement sur la plateforme Windows universelle (UWP), suivez **scrupuleusement** la procédure ci-après. Après avoir configuré tous les paramètres, vous pourrez vous familiariser avec le mode développeur sur Xbox One et avec la création d’applications UWP en consultant la page [UWP pour Xbox One](index.md). 

## Avant de commencer
Avant de commencer, vous devez effectuer les opérations suivantes:
-   Configurer un PC avec Windows 10.
-   Installer Microsoft Visual Studio2015 Update3.
- Vérifier que vous disposez d’au moins 5Go d’espace libre sur votre console XboxOne.

## Configuration de votre PC de développement
1.  Installez Visual Studio2015 Update. Veillez à choisir l’installation **Personnalisée** et à cocher la case **Outils de développement d’applications Windows universelles**, car ces outils ne font pas partie intégrante de l’installation par défaut. Si vous êtes développeur en C++, assurez-vous de choisir **Installation personnalisée** et de sélectionner **C++**. Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md). 

2.  Installez la dernière version du Kit SDK Windows10. Vous pouvez l’obtenir à l’adresse [https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk).

3.  Activez le mode développeur sur votre ordinateur de développement (Paramètres / Mise à jour et sécurité / Pour les développeurs / Mode développeur).

## Configuration de votre console XboxOne
1.  Activez le mode développeur sur votre console XboxOne. Téléchargez l’application, obtenez le code d’activation, puis entrez-le sur la page xboxactivate de votre compte du Centre de développement. Pour plus d’informations, voir [Activation du mode développeur sur Xbox One](devkit-activation.md). 

2.  Accédez à l’application Dev Mode Activation, puis sélectionnez **Basculer et redémarrer**. Félicitations, vous disposez maintenant d’une console XboxOne en mode développeur!
  
  > [!NOTE]
  > Vos applications et jeux commerciaux ne s’exécuteront pas en mode développeur, mais les applications ou jeux que vous créerez le feront sans problème. Pour exécuter vos applications et jeux favoris, rebasculez en mode commercial.
    
  > [!NOTE]
  > Avant de pouvoir déployer une application vers votre XboxOne en mode développeur, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte XboxLive existant ou créer un compte pour votre console en mode développeur. 

## Création de votre premier projet dans VisualStudio2015

Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md).

1.  **Pour C#**: créez un projet Windows universel, accédez aux propriétés du projet, sélectionnez l’onglet **Déboguer**, remplacez **Appareil cible** par **Ordinateur distant**, tapez l’adresse IP ou le nom d’hôte de votre console Xbox One dans le champ **Ordinateur distant**, puis sélectionnez **Universel (protocole non chiffré)** dans la liste déroulante **Mode d’authentification**.   

    Pour trouver l’adresseIP de votre console XboxOne, démarrez l’outil Accueil du développeur sur votre console (grande vignette figurant sur le côté droit de l’écran d’accueil) et examinez le coin supérieur gauche de l’écran. Pour plus d’informations sur l’outil Accueil du développeur, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).  

2.  **Pour les projets C++ et HTML/Javascript**: suivez une procédure similaire, mais dans les propriétés du projet, accédez à l’onglet **Débogage**, sélectionnez **Ordinateur distant** dans le Débogueur pour afficher la liste déroulante, tapez l’adresse IP ou le nom d’hôte de la console dans le champ **Nom de l’ordinateur**, puis sélectionnez **Universel (protocole non chiffré)** dans le champ **Type d’authentification**.
   
3.  Lorsque vous appuyez sur F5, votre application est générée et commence à se déployer sur votre console Xbox One.
  
4.  La première fois que vous effectuez cette opération, VisualStudio vous demande d’entrer un code confidentiel pour votre console XboxOne. Pour obtenir un code confidentiel, démarrez l’outil Accueil du développeur sur votre console Xbox One, puis sélectionnez le bouton **Jumeler avec Visual Studio**.
  
5.  Une fois cette opération effectuée, votre application commence à se déployer. La première exécution de cette procédure peut se révéler un peu lente (nous devons copier la totalité des outils sur votre console Xbox), mais ne devrait pas prendre plus de quelques minutes; dans le cas contraire, un problème est probablement survenu. Vérifiez que vous avez suivi toutes les étapes ci-dessus (notamment que vous avez défini le champ **Mode d’authentification** sur **Universel**) et que vous utilisez une connexion réseau câblée à votre console Xbox One.  

6. Il ne vous reste plus qu’à vous détendre et à profiter de votre première application exécutée sur la console.  

## C’est tout!

![Hello World](images/getting-started-hello-world.png)

## Voir aussi  
- [Forum Aux Questions](frequently-asked-questions.md)  
- [Problèmes connus](known-issues.md)
- [UWP sur XboxOne](index.md) 



<!--HONumber=Aug16_HO4-->


