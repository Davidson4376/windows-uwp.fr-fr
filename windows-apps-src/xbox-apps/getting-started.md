---
title: Prise en main du développement d’applications UWP sur Xbox One
description: Procédure de configuration de votre PC et de votre console Xbox One pour le développement UWP.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c94d27e87853b570268e3a39fe941c817b3eda6a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590974"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Prise en main du développement d’applications UWP sur Xbox One

Pour configurer correctement votre PC et votre console Xbox One pour le développement sur la plateforme Windows universelle (UWP), suivez **scrupuleusement** la procédure ci-après. Après avoir configuré tous les paramètres, vous pourrez vous familiariser avec le mode développeur sur Xbox One et avec la création d’applications UWP en consultant la page [UWP pour Xbox One](index.md). 

## <a name="before-you-start"></a>Avant de commencer

Avant de commencer, vous devez effectuer les opérations suivantes :
-   Configurer un ordinateur sur lequel la dernière version de Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2017.

    > [!NOTE]
    > Visual Studio 2017 is required if you are using the Windows 10, build 15063 SDK. -->

- Vérifier que vous disposez d’au moins 5 Go d’espace libre sur votre console Xbox One.

## <a name="setting-up-your-development-pc"></a>Configuration de votre PC de développement

1.  Installer Visual Studio 2015 Update 3 ou Visual Studio 2017.

    Si vous installez Visual Studio 2015 Update 3, assurez-vous que vous choisissez **personnalisé** installer, puis sélectionnez le **outils développement d’applications universelles Windows** case à cocher – il n’est pas installer de la partie de la valeur par défaut. Si vous êtes développeur en C++, assurez-vous de choisir **Installation personnalisée** et de sélectionner **C++**.

    Si vous installez Visual Studio 2017, veillez à choisir la charge de travail **Développement de plateforme Windows universelle**. Si vous êtes un développeur C++, dans le **Résumé** volet de droite, sous **développement de plateforme Windows universelle**, assurez-vous que vous sélectionnez le **outils de plateforme Windows universelle C++** case à cocher. Il n’est pas partie de l’installation par défaut.

    Pour plus d’informations, consultez [configurer votre UWP sur un environnement de développement Xbox](development-environment-setup.md).

2.  Installez la dernière version [SDK Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Activer le Mode développeur pour votre PC de développement (**paramètres / mise à jour et sécurité / pour les développeurs / utiliser des fonctionnalités de développement / mode développeur**).

Maintenant que votre PC de développement est prêt, vous pouvez regarder cette vidéo ou poursuivre votre lecture pour savoir comment configurer votre console Xbox One pour le développement, et créer une application UWP que vous déploierez sur celle-ci.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configuration de votre console Xbox One

1.  Activez le mode développeur sur votre console Xbox One. Télécharger l’application, obtenir le code d’activation et entrez-la dans la **consoles gérer Xbox One** page dans votre compte de développeur d’applications partenaires. Pour plus d’informations, consultez [Activation du mode Développeur Xbox One](devkit-activation.md). 

2.  Ouvrir le **Activation du Mode développement** application et sélectionnez **commutateur et redémarrage**. Félicitations, vous disposez maintenant d’une console Xbox One en mode développeur !
  
  > [!NOTE]
  > Vos applications et jeux commerciaux ne s’exécuteront pas en mode développeur, mais les applications ou jeux que vous créerez le feront sans problème. Pour exécuter vos applications et jeux favoris, rebasculez en mode commercial.
    
  > [!NOTE]
  > Avant de pouvoir déployer une application vers votre Xbox One en mode développeur, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte Xbox Live existant ou créer un compte pour votre console en mode développeur. 

## <a name="creating-your-first-project-in-visual-studio"></a>Créer votre premier projet dans Visual Studio

Pour plus d’informations, consultez [configurer votre UWP sur un environnement de développement Xbox](development-environment-setup.md).

1.  **Pour C#** : Créer un nouveau projet Windows universel, puis, dans le **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **propriétés**. Sélectionnez le **déboguer** , modifiez **appareil cible** à **Machine distante**, tapez l’adresse IP ou le nom d’hôte de votre console Xbox One dans le **ordinateur distant**  champ, puis sélectionnez **universel (protocole non chiffré)** dans le **Mode d’authentification** liste déroulante.   

    Pour trouver l’adresse IP de votre console Xbox One, démarrez l’outil Accueil du développeur sur votre console (grande vignette figurant sur le côté droit de l’écran d’accueil) et examinez le coin supérieur gauche de l’écran. Pour plus d’informations sur l’outil Accueil du développeur, voir [Présentation des outils Xbox One](introduction-to-xbox-tools.md).  

2.  **Pour les projets C++ et HTML/Javascript**: Vous suivez un chemin d’accès semblable à C# projets, mais dans les propriétés du projet atteindre le **débogage** onglet, sélectionnez **Machine distante** dans le débogueur pour ouvrir la liste déroulante, tapez l’adresse IP ou le nom d’hôte de la console dans le **nom_machine** champ, puis sélectionnez **universel (protocole non chiffré)** dans le **Type d’authentification** champ.

3. Sélectionnez **x64** dans la liste déroulante à gauche du bouton de lecture vert dans la barre de menus supérieure.
   
4.  Lorsque vous appuyez sur F5, votre application est générée et commence à se déployer sur votre console Xbox One.
  
5.  La première fois que vous effectuez cette opération, Visual Studio vous demande d’entrer un code confidentiel pour votre console Xbox One. Vous pouvez obtenir un code confidentiel en début d’accueil de développement sur votre Xbox One et en sélectionnant le **code confidentiel afficher Visual Studio** bouton.
  
6.  Une fois cette opération effectuée, votre application commence à se déployer. La première exécution de cette procédure peut se révéler un peu lente (nous devons copier la totalité des outils sur votre console Xbox), mais ne devrait pas prendre plus de quelques minutes ; dans le cas contraire, un problème est probablement survenu. Vérifiez que vous avez suivi toutes les étapes ci-dessus (notamment que vous avez défini le champ **Mode d’authentification** sur **Universel**) et que vous utilisez une connexion réseau câblée à votre console Xbox One.  

7. Il ne vous reste plus qu’à vous détendre et à profiter de votre première application exécutée sur la console.  

## <a name="thats-it"></a>C’est tout !

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Voir également  
- [Forum aux questions](frequently-asked-questions.md)  
- [Problèmes connus avec UWP sur Xbox Developer Program](known-issues.md)
- [UWP sur Xbox One](index.md) 
