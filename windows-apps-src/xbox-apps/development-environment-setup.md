---
author: Mtoepke
title: "Configurer votre plateforme UWP sur l’environnement de développement Xbox"
description: "Procédure de configuration et de test de votre plateforme Windows universelle sur l’environnement de développement Xbox."
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 93319caaa16afe84a897dbc4bd6370a5cef3cdd1
ms.lasthandoff: 02/08/2017

---

# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Configurer votre plateforme UWP sur l’environnement de développement Xbox

La plateforme Windows universelle (UWP) sur l’environnement de développement Xbox se compose d’un ordinateur dédié au développement connecté à une console Xbox One par le biais d’un réseau local.
L’ordinateur de développement requiert Windows 10, Visual Studio 2015 Update 2, la version d’évaluation 14295 du kit de développement logiciel (SDK) Windows 10 et divers outils de prise en charge.


Cet article couvre les étapes relatives à la configuration et au test de votre environnement de développement.

## <a name="visual-studio-setup"></a>Installation de Visual Studio

1. Installez Visual Studio 2015 Update 2 ou version ultérieure. Pour en savoir plus et pour l’installation, voir [Téléchargements et outils pour Windows 10](https://dev.windows.com/downloads).

1. Lorsque vous installez Visual Studio 2015 Update 2, assurez-vous que la case **Outils de développement d’applications Windows universelles** est cochée.

  ![Installer Visual Studio 2015 Update 2 ou version ultérieure.](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Installation du Kit de développement logiciel (SDK) Windows 10

Installez la dernière version d’évaluation du Kit de développement logiciel (SDK) Windows 10. Pour obtenir des informations sur l’installation, voir [Télécharger les mises à jour Insider Preview pour les développeurs](http://go.microsoft.com/fwlink/p/?LinkId=780552).

> [!IMPORTANT]
> Vous devez installer la dernière version du Kit de développement logiciel (SDK), toutefois, vous n’avez _pas_ à installer la dernière version de Windows Insider Preview du système d’exploitation.

## <a name="enabling-developer-mode"></a>Activation du mode développeur

Avant de pouvoir déployer des applications à partir de votre ordinateur de développement, vous devez activer le mode développeur via le menu Windows : Paramètres / Mise à jour et sécurité / Pour les développeurs / Mode développeur.

## <a name="setting-up-your-xbox-one"></a>Configuration de votre Xbox One

Avant de pouvoir déployer une application sur votre Xbox One, un utilisateur doit être connecté à la console. Vous pouvez utiliser votre compte Xbox Live existant ou créer un compte pour votre console en mode développeur. 

## <a name="create-your-first-application"></a>Créer votre première application

1. Assurez-vous que l’ordinateur de développement se trouve sur le même réseau local que la console Xbox One cible. En règle générale, cela signifie qu’ils doivent utiliser le même routeur et être sur le même sous-réseau. Une connexion réseau câblée est recommandée.

1. Assurez-vous que votre console Xbox One se trouve en Mode développeur.  Pour plus d’informations, voir [Activation du mode développeur sur Xbox One](devkit-activation.md).

1. Déterminez le langage de programmation que vous voulez utiliser pour votre application UWP.

1. Sur votre ordinateur de développement, sélectionnez **Nouveau projet**, puis **Windows / Universel / Application vide**.

### <a name="starting-a-c-project"></a>Démarrage d’un projet c#

  ![Boîte de dialogue Nouveau projet](images/vs_universal_blank.jpg)

1. Sélectionnez les options par défaut dans la boîte de dialogue **Nouveau projet Windows universel**. Si la boîte de dialogue **Mode développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

1. Configurez votre environnement de développement pour le débogage à distance :

  1. Cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.
  1. Dans l’onglet **Déboguer**, redéfinissez **Plateforme** sur **Active (x64)**. (La plateforme x86 n’est plus prise en charge sur Xbox.)   
  1. Redéfinissez **Périphérique cible** sur **Ordinateur distant**.
  1. Dans **Ordinateur distant**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).
  1. Dans la liste déroulante **Mode d’authentification**, sélectionnez **Universel (protocole non chiffré)**.

    ![Pages de propriétés BlankApp C#](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>Démarrage d’un projet C++

  ![Projet C++](images/vs_universal_cpp_blank.jpg)

1. Sélectionnez les options par défaut dans la boîte de dialogue **Nouveau projet Windows universel**. Si la boîte de dialogue **Mode développeur** s’affiche, cliquez sur **OK**. Une nouvelle application vide est créée.

1. Configurez votre environnement de développement pour le débogage à distance :

   1. Cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**.
   1. Sur l’onglet **Débogage** et définissez **Débogueur à lancer** sur **Ordinateur distant**.
   1. Dans **Nom de l’ordinateur**, entrez l’adresse IP du système ou le nom d’hôte de la console Xbox One. Pour plus d’informations sur l’obtention de l’adresse IP ou du nom d’hôte, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).
   1. Dans la liste déroulante **Type d’authentification**, sélectionnez **Universel (protocole non chiffré)**.

    ![Pages de propriétés BlankApp C++](images/vs_remote_cpp.jpg)

### <a name="pin-pair-your-device-with-visual-studio"></a>Jumeler le code confidentiel de votre appareil avec Visual Studio

1. Enregistrez vos paramètres et assurez-vous que votre console Xbox One est en Mode développeur.

1. Appuyez sur F5.

1. S’il s’agit de votre premier déploiement, vous obtiendrez une boîte de dialogue issue de Visual Studio vous demandant d’épingler par paire votre appareil.

  1. Pour obtenir un code confidentiel, ouvrez **Dev Home** à partir de l’écran d’accueil sur votre console Xbox One.
  1. Sélectionnez **Jumeler avec Visual Studio**.

    ![Boîte de dialogue Jumeler avec Visual Studio](images/devhome_visualstudio.png)

  1. Entrez votre code confidentiel dans la boîte de dialogue **Jumeler avec Visual Studio**. Le code confidentiel suivant est juste un exemple. Le vôtre sera différent.

    ![Boîte de dialogue du code confidentiel Jumeler avec Visual Studio](images/devhome_pin.png)

  1. Des erreurs de déploiement, le cas échéant, s’afficheront dans la fenêtre **Sortie**.

Félicitations, vous avez correctement créé et déployé votre première application UWP sur Xbox !



## <a name="see-also"></a>Voir aussi
- [Activation du Mode développeur sur Xbox One](devkit-activation.md)  
- [Téléchargements et outils pour Windows 10](https://dev.windows.com/downloads)  
- [Télécharger les mises à jour Insider Preview pour les développeurs](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Présentation des outils Xbox One](introduction-to-xbox-tools.md) 
- [UWP sur Xbox One](index.md)

----

