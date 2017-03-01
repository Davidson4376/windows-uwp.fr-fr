---
author: Mtoepke
title: "Prise en main du développement d’applications UWP sur Xbox One"
description: "Procédure de configuration de votre PC et de votre console Xbox One pour le développement UWP."
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 6e033ffa-502e-4daa-b5b2-6f853f68b66c
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 59090536600e3e45345832c6ca61baa3fe1f5b15
ms.lasthandoff: 02/08/2017

---

#<a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Prise en main du développement d’applications UWP sur Xbox One

Pour configurer correctement votre PC et votre console Xbox One pour le développement sur la plateforme Windows universelle (UWP), suivez **scrupuleusement** la procédure ci-après. Après avoir configuré tous les paramètres, vous pourrez vous familiariser avec le mode développeur sur Xbox One et avec la création d’applications UWP en consultant la page [UWP pour Xbox One](index.md). 

## <a name="before-you-start"></a>Avant de commencer
Avant de commencer, vous devez effectuer les opérations suivantes :
-    Configurer un PC avec Windows 10.
-    Installer Microsoft Visual Studio 2015 Update 3.
- Vérifier que vous disposez d’au moins 5 Go d’espace libre sur votre console Xbox One.

## <a name="setting-up-your-development-pc"></a>Configuration de votre PC de développement
1.    Installez Visual Studio 2015 Update. Veillez à choisir l’installation **Personnalisée** et à cocher la case **Outils de développement d’applications Windows universelles**, car ces outils ne font pas partie intégrante de l’installation par défaut. Si vous êtes développeur en C++, assurez-vous de choisir **Installation personnalisée** et de sélectionner **C++**. Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md). 

2.    Installez la dernière version du SDK Windows 10. Vous pouvez obtenir ce kit à l’adresse [https://developer.microsoft.com/windows/downloads/windows-10-sdk](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Activez le mode développeur sur votre ordinateur de développement (Paramètres / Mise à jour et sécurité / Pour les développeurs / Mode développeur).

## <a name="setting-up-your-xbox-one-console"></a>Configuration de votre console Xbox One
1.    Activez le mode développeur sur votre console Xbox One. Téléchargez l’application, obtenez le code d’activation, puis entrez-le sur la page xboxactivate de votre compte du Centre de développement. Pour plus d’informations, voir [Activation du mode développeur sur Xbox One](devkit-activation.md). 

2.    Accédez à l’application Dev Mode Activation, puis sélectionnez **Basculer et redémarrer**. Félicitations, vous disposez maintenant d’une console Xbox One en mode développeur !
  
  > [!NOTE]
  > Vos applications et jeux commerciaux ne s’exécuteront pas en mode développeur, mais les applications ou jeux que vous créerez le feront sans problème. Pour exécuter vos applications et jeux favoris, rebasculez en mode commercial.
    
  > [!NOTE]
  > Avant de pouvoir déployer une application vers votre Xbox One en mode développeur, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte Xbox Live existant ou créer un compte pour votre console en mode développeur. 

## <a name="creating-your-first-project-in-visual-studio-2015"></a>Création de votre premier projet dans Visual Studio 2015

Pour plus d’informations, voir [Configuration de l’environnement de développement](development-environment-setup.md).

1.    **Pour C#** : créez un projet Windows universel, accédez aux propriétés du projet, sélectionnez l’onglet **Déboguer**, remplacez **Appareil cible** par **Ordinateur distant**, tapez l’adresse IP ou le nom d’hôte de votre console Xbox One dans le champ **Ordinateur distant**, puis sélectionnez **Universel (protocole non chiffré)** dans la liste déroulante **Mode d’authentification**.   

    Pour trouver l’adresse IP de votre console Xbox One, démarrez l’outil Accueil du développeur sur votre console (grande vignette figurant sur le côté droit de l’écran d’accueil) et examinez le coin supérieur gauche de l’écran. Pour plus d’informations sur l’outil Accueil du développeur, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).  

2.    **Pour les projets C++ et HTML/Javascript** : suivez une procédure similaire, mais dans les propriétés du projet, accédez à l’onglet **Débogage**, sélectionnez **Ordinateur distant** dans le Débogueur pour afficher la liste déroulante, tapez l’adresse IP ou le nom d’hôte de la console dans le champ **Nom de l’ordinateur**, puis sélectionnez **Universel (protocole non chiffré)** dans le champ **Type d’authentification**.
   
3.    Lorsque vous appuyez sur F5, votre application est générée et commence à se déployer sur votre console Xbox One.
  
4.    La première fois que vous effectuez cette opération, Visual Studio vous demande d’entrer un code confidentiel pour votre console Xbox One. Pour obtenir un code confidentiel, démarrez l’outil Accueil du développeur sur votre console Xbox One, puis sélectionnez le bouton **Jumeler avec Visual Studio**.
  
5.    Une fois cette opération effectuée, votre application commence à se déployer. La première exécution de cette procédure peut se révéler un peu lente (nous devons copier la totalité des outils sur votre console Xbox), mais ne devrait pas prendre plus de quelques minutes ; dans le cas contraire, un problème est probablement survenu. Vérifiez que vous avez suivi toutes les étapes ci-dessus (notamment que vous avez défini le champ **Mode d’authentification** sur **Universel**) et que vous utilisez une connexion réseau câblée à votre console Xbox One.  

6. Il ne vous reste plus qu’à vous détendre et à profiter de votre première application exécutée sur la console.  

## <a name="thats-it"></a>C’est tout !

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Voir aussi  
- [Forum Aux Questions](frequently-asked-questions.md)  
- [Problèmes connus](known-issues.md)
- [UWP sur Xbox One](index.md) 

