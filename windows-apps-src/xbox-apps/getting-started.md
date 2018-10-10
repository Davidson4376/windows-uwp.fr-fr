---
author: Mtoepke
title: Prise en main du développement d’applications UWP sur XboxOne
description: Procédure de configuration de votre PC et de votre console XboxOne pour le développement UWP.
ms.author: scotmi
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da260b4f9f5f50d97d39af883217dfbae91a566e
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4472592"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Prise en main du développement d’applications UWP sur XboxOne

Pour configurer correctement votre PC et votre console Xbox One pour le développement sur la plateforme Windows universelle (UWP), suivez **scrupuleusement** la procédure ci-après. Après avoir configuré tous les paramètres, vous pourrez vous familiariser avec le mode développeur sur Xbox One et avec la création d’applications UWP en consultant la page [UWP pour Xbox One](index.md). 

## <a name="before-you-start"></a>Avant de commencer

Avant de commencer, vous devez effectuer les opérations suivantes:
-   Configurer un PC avec la dernière version de Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2017.

    > [!NOTE]
    > Visual Studio 2017 is required if you are using the Windows 10, build 15063 SDK. -->

- Vérifier que vous disposez d’au moins 5Go d’espace libre sur votre console XboxOne.

## <a name="setting-up-your-development-pc"></a>Configuration de votre PC de développement

1.  Installez Visual Studio 2015 Update 3 ou Visual Studio 2017.

    Si vous installez Visual Studio 2015 Update 3, assurez-vous que vous choisissez d’installation **personnalisée** et que vous activez la case à cocher **Outils de développement d’applications Windows universelles** ; il n’est pas partie de l’installation par défaut. Si vous êtes développeur en C++, assurez-vous de choisir **Installation personnalisée** et de sélectionner **C++**.

    Si vous installez VisualStudio2017, veillez à choisir la charge de travail **Développement de plateforme Windows universelle**. Si vous êtes un développeur en C++, dans le volet de **synthèse** sur la droite, en cours de **développement de plateforme Windows universelle**, assurez-vous que vous sélectionnez la case à cocher **Outils de plateforme Windows universelle C++** . Il n’est pas partie de l’installation par défaut.

    Pour plus d’informations, consultez [configurer votre plateforme UWP sur l’environnement de développement Xbox](development-environment-setup.md).

2.  Installez le dernier [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Activer le Mode développeur sur votre ordinateur de développement (**paramètres / mise à jour et sécurité / pour les développeurs ou utilisent les fonctionnalités de développement / mode développeur**).

Maintenant que votre PC de développement est prêt, vous pouvez regarder cette vidéo ou poursuivre votre lecture pour savoir comment configurer votre console XboxOne pour le développement, et créer une application UWP que vous déploierez sur celle-ci.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configuration de votre console XboxOne

1.  Activez le mode développeur sur votre console XboxOne. Télécharger l’application, puis entrez-le dans la page [consoles Xbox One de la gérer](https://partner.microsoft.com/xboxactivate) dans votre compte du centre de développement et obtenir le code d’activation. Pour plus d’informations, consultez [Activation du mode Développeur XboxOne](devkit-activation.md). 

2.  Ouvrez l’application **Dev Mode Activation** et sélectionnez **basculer et redémarrer**. Félicitations, vous disposez maintenant d’une console XboxOne en mode développeur!
  
  > [!NOTE]
  > Vos applications et jeux commerciaux ne s’exécuteront pas en mode développeur, mais les applications ou jeux que vous créerez le feront sans problème. Pour exécuter vos applications et jeux favoris, rebasculez en mode commercial.
    
  > [!NOTE]
  > Avant de pouvoir déployer une application vers votre XboxOne en mode développeur, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte XboxLive existant ou créer un compte pour votre console en mode développeur. 

## <a name="creating-your-first-project-in-visual-studio"></a>Création de votre premier projet dans Visual Studio

Pour plus d’informations, consultez [configurer votre plateforme UWP sur l’environnement de développement Xbox](development-environment-setup.md).

1.  **Pour c#**: créer un nouveau projet Windows universel et dans l' **Explorateur de solutions**, cliquez sur le projet et sélectionnez **Propriétés**. Sélectionnez l’onglet **débogage** , remplacez **l’appareil cible** à **l’Ordinateur distant**, tapez l’adresse IP ou le nom d’hôte de votre console Xbox One dans le champ de **l’ordinateur distant** et sélectionnez **universel (protocole non chiffré)** dans le ** Mode d’authentification** liste déroulante.   

    Pour trouver l’adresseIP de votre console XboxOne, démarrez l’outil Accueil du développeur sur votre console (grande vignette figurant sur le côté droit de l’écran d’accueil) et examinez le coin supérieur gauche de l’écran. Pour plus d’informations sur l’outil Accueil du développeur, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).  

2.  **Pour C++ et HTML/Javascript projets**: vous suivez un chemin semblable aux projets c#, mais dans les propriétés du projet accédez à l’onglet **débogage** , sélectionnez **l’Ordinateur distant** dans le débogueur pour ouvrir la liste déroulante, tapez l’adresse IP ou le nom d’hôte de la console dans le champ du **Nom de l’ordinateur** et sélectionnez **universel (protocole non chiffré)** dans le champ de **Type d’authentification** .

3. Sélectionnez **x64** dans la liste déroulante à gauche du bouton vert de lecture dans la barre de menus supérieure.
   
4.  Lorsque vous appuyez sur F5, votre application est générée et commence à se déployer sur votre console Xbox One.
  
5.  La première fois que vous effectuez cette opération, VisualStudio vous demande d’entrer un code confidentiel pour votre console XboxOne. Vous pouvez obtenir un code confidentiel, démarrez l’accueil du développeur sur votre console Xbox One et en sélectionnant le bouton de **code confidentiel afficher Visual Studio** .
  
6.  Une fois cette opération effectuée, votre application commence à se déployer. La première exécution de cette procédure peut se révéler un peu lente (nous devons copier la totalité des outils sur votre console Xbox), mais ne devrait pas prendre plus de quelques minutes; dans le cas contraire, un problème est probablement survenu. Vérifiez que vous avez suivi toutes les étapes ci-dessus (notamment que vous avez défini le champ **Mode d’authentification** sur **Universel**) et que vous utilisez une connexion réseau câblée à votre console Xbox One.  

7. Il ne vous reste plus qu’à vous détendre et à profiter de votre première application exécutée sur la console.  

## <a name="thats-it"></a>C’est tout!

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Voir aussi  
- [Forum Aux Questions](frequently-asked-questions.md)  
- [Problèmes connus avec UWP dans le programme pour les développeurs Xbox](known-issues.md)
- [UWP sur XboxOne](index.md) 
